# poc-aws-gcp
<strong>Multi-Cloud project (AWS &amp; GCP Cloud with Terraform)</strong>

<b>Description</b>: 	In this project we are using AWS and GCP to Create AWS IAM User & Access Keys, using the access keys in GCP we will Run Terraform Code in Google Cloud & Terminate the resources.

<b>Steps:</b> 
1. Create AWS IAM User:<br>
    a)Create an IAM user from AWS console <br>
    b)Generate access keys to be processed using Terraform in GCP.<br>

2. Run Terraform Code in Google Cloud Console <br>
   −Upload AWS Access Keys to GCP console, prepare Terraform configurational files, including AWS access keys, and GCP project ID config, provide necessary permissions for execution, and create a service account. <br>
   −Set up the Terraform run in GCP through AWS IAM user access keys, and create AWS S3 bucket, create GKE cluster and SQL instance.<br>

   a)Run the following commands to prepare AWS & GCP Environments:<br>

         ./aws_set_credentials.sh accessKeys.csv
         gcloud config set project <project_id>
         ./gcp_set_project.sh
  
    b)Run the following commands to enable the Container Registry API, Kubernetes Engine API and the Cloud SQL API:<br>
   
			gcloud services enable containerregistry.googleapis.com 
			gcloud services enable container.googleapis.com 
			gcloud services enable sqladmin.googleapis.com
