#create a Virtual Machine instance on Google Cloud with the value of the Startup Script 
gcloud compute instances create "bloghost" \
--metadata startup-script="
  apt-get update
  apt-get install apache2 php php-mysql -y
  service apache2 restart" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"

#Allowing HTTP traffic for the firewall
gcloud compute firewall-rules create default-allow-http \
--direction=INGRESS --priority=1000 --network=default --action=ALLOW \
--rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

#Creating  a Cloud Storage bucket using gsutil 
#enter your chosen location into an environment variable called LOCATION.
export LOCATION=US

#Creating  a Cloud Storage bucket named after the project ID 
gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID

#Retrieve an imaage from cloud storage location
gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png

#copying the image into the storage bucket created
gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png

#making the object accessible by everyone
gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png

#Creating Cloud SQL instance
gcloud sql instances create blog-db --tier=db-n1-standard-2 --region=europe-west2 --database-version=

#set root user password
gcloud sql users set-password root --host=% --instance blog-db --password izik

#Adding user account 
gcloud sql users create blogdbuser \
   --host=[HOST] --instance=blog-db --password=izik

#Listing users 
gcloud sql users list --instance=blog-db

#Adding Connections
gcloud sql instances patch [INSTANCE_NAME] --authorized-networks=[IP_ADDR1],[IP_ADDR2]...