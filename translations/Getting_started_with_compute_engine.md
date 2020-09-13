#Display a list of zones assigned 
gcloud compute zones list | grep us-central1

#Set your default zone
gcloud config set compute/zone us-central1-a

#create a Virtual Machine instance on Google Cloud 
gcloud compute instances create "my-vm-1" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"

#Allowing HTTP traffic for the firewall
gcloud compute firewall-rules create default-allow-http \
--direction=INGRESS --priority=1000 --network=default --action=ALLOW \
--rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

#Allowing HTTP traffic for the firewall
gcloud compute firewall-rules create default-allow-http \
--direction=INGRESS --priority=1000 --network=default --action=ALLOW \
--rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

#creating the second Virtual Machine

#Set your default zone
gcloud config set compute/zone us-central1-b

#create a Virtual Machine instance on Google Cloud 
gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"

#Allowing HTTP traffic for the firewall
gcloud compute firewall-rules create default-allow-http \
--direction=INGRESS --priority=1000 --network=default --action=ALLOW \
--rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

#SSH into a virtual machine instance (my-vm-2)
gcloud compute ssh my-vm-2 --zone us-central1-b

#Ping my-vm-1 to confirm that my-vm-2 can reach it over the network
ping my-vm-1

#SSH into a virtual machine instance (my-vm-1)
ssh my-vm-1

#Installing the Nginx web server
sudo apt-get install nginx-light -y

#adding a custom message to the home page of the web server
sudo nano /var/www/html/index.nginx-debian.html

#confirm that the Nginx web server is serving the new page 
 curl http://localhost/
 
#exiting the command prompt on my-vm-1
exit

#confirming that my-vm-2 can reach the web server on my-vm-1
curl http://my-vm-1/

#To view the internal and external IP addresses for the two instances
gcloud compute instances list
 



 
 

