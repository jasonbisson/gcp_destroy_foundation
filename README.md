# Destory Secure Foundation example

This repository contains an updated bash script and cloud build deployment file designed to destroy a secure foundation. While this is not typical use case the ability to run a terraform destroy is gap for the foundation. This tactical repo follows the same pattern of building the foundation, but requires a manual command for each environment and level of the deployment.

## Usage

1. Clone the repo of the stage that you want to destroy.
   ```
   gcloud source repos clone <ie gcp-projects> --project=YOUR_CLOUD_BUILD_PROJECT_ID
   ```
1. Navigate into the repo.
   ```
   cd <the cloned repo>
   ```
1. Copy Cloud Build configuration files for Terraform. You may need to modify the command to reflect
   your current directory.
   ```
   cp ../gcp_destroy_foundation/cloudbuild-tf-* .
   ```
1. Copy the Terraform wrapper script to the root of your new repository (modify accordingly based on your current directory).
   ```
   cp ../gcp_destroy_foundation/tf-wrapper.sh .
   ```
1. Ensure wrapper script can be executed.
   ```
   chmod 755 ./tf-wrapper.sh
   ```
1. Change to git branch that will be destroyed
   ```
   git checkout <existing_branch>
   ```
1. Run Cloud command to kick off the terraform destroy 
   ```
    gcloud builds submit . --config=cloudbuild-tf-destroy.yaml --project <CFT Cloud Build project id> --substitutions=BRANCH_NAME="$(git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/')",_ARTIFACT_BUCKET_NAME='CFT Artifact bucket',_STATE_BUCKET_NAME='CFT Terraform state bucket',_TF_SA_EMAIL='your_terraform_service_account@<seed project id>.iam.gserviceaccount.com'
   ```