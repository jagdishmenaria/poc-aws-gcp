# poc-aws-gcp
<strong>Multi-Cloud project (AWS &amp; GCP Cloud with Terraform)</strong>

<b>Description</b>: 	In this project we are using AWS and GCP to Create AWS IAM User & Access Keys, using the access keys in GCP we will Run Terraform Code in Google Cloud & Terminate the resources.

<b>Steps:</b> 
1. Create AWS IAM User:<br>
    a) Create an IAM user from AWS console

     b) Generate access keys to be processed using Terraform in GCP

2. Run Terraform Code in Google Cloud Console <br>

   −Upload AWS Access Keys to GCP console, prepare Terraform configurational files, including AWS access keys, and GCP project ID config, provide necessary permissions for execution, and create a service account. <br>
   −Set up the Terraform run in GCP through AWS IAM user access keys, and create AWS S3 bucket, create GKE cluster and SQL instance.<br>

	a) Run the following commands to prepare AWS & GCP Environments:<br>

         ./aws_set_credentials.sh accessKeys.csv
         gcloud config set project <project_id>
         ./gcp_set_project.sh
  
	b) Run the following commands to enable the Container Registry API, Kubernetes Engine API and the Cloud SQL API:<br>
   
			gcloud services enable containerregistry.googleapis.com 
			gcloud services enable container.googleapis.com 
			gcloud services enable sqladmin.googleapis.com

	c) Before executing the Terraform commands, open the Google Editor and update the file tcb_aws_storage.tf replacing the bucket name with an unique name (AWS requires unique bucket names)

	d) Go back to the terminal and Run the following commands to finish provision infrastructure steps:

			cd/terraform
			terraform init
			terraform plan
			terraform apply

	e) Till these steps terraform is successfully initiated and now you can check manually that the Kubernetes cluster and sql instance are created through terraform.

3. Navigate to cloud SQL in GCP Console, create new user account and create a table.<br>
	
 	a) First, set up the Private connection in SQL instance, Enable Private IP and set up network as default

	b) Set up a Public IP section, add a new network for Public testing purpose (0.0.0.0/0)

 	c) Go to the User section and click on create new user account (built-in authentication and set password)
	
 	d) Connect to MySQL DB running on Cloud SQL. Run this command in cloud shell, add project IP, and input the password when asked.

			mysql --host=<public_ip_cloudsql> --port=3306 -u app -p

  	e) Once connected to the database instance, create the table:
   
			use <>DB_NAME;
			source~ /<PATH_TO_SQL_FILE>/create_table.sql;
			show full tables;
			select * from <TABLE_NAME>;
			exit;

4. Create a docker image and push it to google container registry.
   
	a) Run the following commands and replace the <project_ID> with your project ID:

			Cd ~/<PATH_TO_APP_DIRECTORY>/app
			docker build -t gcr.io/<project_id>/hello-app:1.0 .
			gcloud docker -- push gcr.io/<project_id>/hello-app:1.0

	b) Go to kubernetes folder and change the below in hello-world-testing-system.yaml file:

			Copy/paste the AWS generated Access Key ID and Secret Access Key
			Replace the project_ID
			Provide DB Hostname, Instance name, user, password and port details
			
5. Connect to the GKE Cluster, Deploy application and Test it.<br>		
	
 	a) Connect GKE Cluster through GCP console, and click on 'Run in cloud shell' option

	b) Cd~/<PATH_TO_KUBERNETES_DIRECTORY>/kubernetes

  	c) Apply the declarative yaml file to deploy the pods and expose service in kubernetes:
   
			kubectl apply -f hello-world-testing-system.yaml

  	d) Application is deployed successfully, and can be verified by the public IP in Services and ingress section

6. Terminate the resources<br>
		-Make sure to delete or stop all services or tools running. It saves your cost

-----------------------------------------------------------------------------------------------
<b>TROUBLESHOOTING</b>
1. There might be issue you will face during the file location.
	Solution:- If faced such issue try to add the direct file path of that particular file

2. While running commands there would be some delays during GKE Cluster and SQL Instance creations.
	Solution - Be patient it may happen that each one of the command can take 5-10 minutes to deploy
	
3. In the last step, it may happen that the Public IP doesn’t work.
	Solution - Need to wait for atleast 2-5 mins then try again. Also check on GKE cluster workloads if all sections are green marked or not.	
-----------------------------------------------------------------------------------------------	
<b>Naming Conventions:</b><br>
		AWS_BUCKET = hello-world-testing-system-pdf-en-157 <br>
		AWS_IAM_user = hello-world-testing-system-en-app1 <br>
		mysql --host=<public_ip> --port=3306 -u app -p <br>
		DB_NAME=helloworldtesting <br>
		Kubernetes cluster = hello-world-kubernetes-cluster-en <br>
		DB Instance = hello-world-testing-system-database-instance-en <br>
		DB user=app <br>
		DB pwd=xxxxxxxxxx <br>
		Image= hello-app <br>
 
 -----------------------------------------------------------------------------------------------
