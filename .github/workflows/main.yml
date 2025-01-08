name: Trigger and Check parameterized jenkins job 

on:
  workflow_dispatch:
    inputs:
      deployment_server:
        description: "The IP address of the server to deploy to"
        required: true
        default: "54.234.143.11"
      port:
        description: "The port to deploy the application on (between 8000 and 9000)"
        required: true
        default: "8080"
      branch_name:
        description: "The Git branch to build from"
        required: true
        default: "main"
      jenkins_stages:
        description: "Comma-separated list of Jenkins stages to run"
        required: true
        default: "pipeline1"

jobs:
  trigger-and-check-jenkins:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Trigger Jenkins Job
        run: |
          set -e
          JOB_NAME="pipeline1"
          JENKINS_URL="http://54.234.143.11:8080/"
          USERNAME="nive"
          TOKEN="113d42a9030690bd0980aa032a5a65e792"
          DEPLOYMENT_SERVER="${{ github.event.inputs.deployment_server }}"
          PORT="${{ github.event.inputs.port }}"
          BRANCH_NAME="${{ github.event.inputs.branch_name }}"
          STAGES="${{ github.event.inputs.jenkins_stages }}"

       
          PARAMS="RUN_BASIC_ENV=false&RUN_FILE_OPS=false&RUN_NETWORK_TEST=false&RUN_BUILD_SIM=false"

         
          for STAGE in $(echo $STAGES | tr ',' '\n'); do
            PARAMS=$(echo $PARAMS | sed "s/${STAGE}=false/${STAGE}=true/")
          done

          echo "Triggering Jenkins Job with parameters: $PARAMS"

          # Trigger Jenkins job
          curl -X POST -u $USERNAME:$TOKEN \
            "$JENKINS_URL/job/$JOB_NAME/buildWithParameters?$PARAMS&DEPLOYMENT_SERVER=${DEPLOYMENT_SERVER}&PORT=${PORT}&BRANCH_NAME=${BRANCH_NAME}"
