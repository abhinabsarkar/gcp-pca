# GCP CLI basics

* [Install cloud SDK](https://cloud.google.com/sdk/docs/install)

After installation is complete, it starts a terminal window and runs the gcloud init command. It will create a default profile for you. 

```txt
Your current project has been set to: [qwiklabs-gcp-00-b217b3dac28d].

Your project default Compute Engine zone has been set to [us-central1-a].
You can change it by running [gcloud config set compute/zone NAME].

Your project default Compute Engine region has been set to [us-central1].
You can change it by running [gcloud config set compute/region NAME].

Created a default .boto configuration file at [C:\Users\<username>\.boto]. See this file and
[https://cloud.google.com/storage/docs/gsutil/commands/config] for more
information about configuring Google Cloud Storage.
Your Google Cloud SDK is configured and ready to use!

* Commands that require authentication will use student-03-78a2f7158e3f@qwiklabs.net by default
* Commands will reference project `qwiklabs-gcp-00-b217b3dac28d` by default
* Compute Engine commands will use region `us-central1` by default
* Compute Engine commands will use zone `us-central1-a` by default
```

## Basic CLI authentication commands
```bash
# Login
gcloud auth login
# Logout all
gcloud auth revoke --all
# Logout specific account
gcloud auth revoke <your_account>
# List the config settings
gcloud config list
# Set the project in the gcloud config
gcloud config set project <PROJECT_ID>
```

## Create VMs in GCP
```bash
# Command to create a Windows VM
# port 80 and 443 rules have the tags "http-server" and "https-server"
gcloud compute instances create "my-vm-1" \
--machine-type "n1-standard-1" \
--image-project "windows-cloud" \
--image "windows-server-2012-r2-dc-v20191008" \
--subnet "default" \
--tags http-server,https-server --verbosity=debug
# RDP & install IIS using PowerShell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
# View the default IIS web server page
curl http://<external-ip>/

# Command to create a Linux VM
# port 80 and 443 rules have the tags "http-server" and "https-server"
gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image-family "debian-10" \
--subnet "default" \
--tags http-server,https-server
# SSH into linux VM using gcloud cli
gcloud compute ssh <vm-name>
# install the Nginx web server
sudo apt-get install nginx-light -y
# View the default NGINX web server page
curl http://<external-ip>/
# Command to list VMs
gcloud compute instances list --zones=us-central1-a --verbosity=debug
# Command to delete VMs
gcloud compute instances delete my-vm-2 --zone=us-central1-a --verbosity=debug
```
## Create a Cloud Storage bucket
```bash
# Create a Cloud Storage bucket (make bucket)
export LOCATION=US
export DEVSHELL_PROJECT_ID=qwiklabs-gcp-01-022203c969ec
gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID
# copy a banner image from a publicly accessible Cloud Storage location to local
gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png
# Copy the banner image to your newly created Cloud Storage bucket
gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
# Modify the ACL of the object so that it is readable by everyone
gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
```
## Create a Deployment Manager deployment
```bash
# Download an Deployment Manager template
gsutil cp gs://cloud-training/gcpfcoreinfra/mydeploy.yaml mydeploy.yaml
# Replace the 'ZONE' placeholder string with your GCP zone
sed -i -e "s/ZONE/$MY_ZONE/" mydeploy.yaml
# Replace the 'PROJECT_ID' placeholder string with your GCP project Id
sed -i -e "s/PROJECT_ID/$PROJECT_ID/" mydeploy.yaml
# Create a deployment
gcloud deployment-manager deployments create my-first-depl --config mydeploy.yaml
# The deployment details can be viewed in the dashboard or using the CLI
gcloud deployment-manager deployments describe my-first-depl
# Update a Deployment Manager deployment
nano mydeploy.yaml
# Update the value of the startup-script key
value: "apt-get update; apt-get install nginx-light -y"
# Update the deployment
gcloud deployment-manager deployments update my-first-depl --config mydeploy.yaml
# Delete a deployment already created
gcloud deployment-manager deployments delete  my-first-depl
```

## Install logging & monitoring agent on VMs
```bash
# Install monitoring agent on VM
curl -sSO https://dl.google.com/cloudagents/install-monitoring-agent.sh
sudo bash install-monitoring-agent.sh
# install logging agent on VM
curl -sSO https://dl.google.com/cloudagents/install-logging-agent.sh
sudo bash install-logging-agent.sh
```

## Create VPC & subnet inside the VPC
```bash
# create the privatenet network VPC
gcloud compute networks create privatenet --subnet-mode=custom
# create the privatesubnet-us subnet inside the VPC
gcloud compute networks subnets create privatesubnet-us --network=privatenet --region=us-central1 --range=172.16.0.0/24
# create the privatesubnet-eu subnet inside the VPC
gcloud compute networks subnets create privatesubnet-eu --network=privatenet --region=europe-west1 --range=172.20.0.0/20
# list the available VPC networks
gcloud compute networks list
```

## Create firewall rules
```bash
# create the privatenet-allow-icmp-ssh-rdp firewall rule
gcloud compute firewall-rules create privatenet-allow-icmp-ssh-rdp --direction=INGRESS \
    --priority=1000 --network=privatenet --action=ALLOW --rules=icmp,tcp:22,tcp:3389 \
    --source-ranges=0.0.0.0/0
```

## Create VM inside a subnet
```bash
# Create a VM inside the subnet privatesubnet-us
gcloud compute instances create privatenet-us-vm --zone=us-central1-c --machine-type=f1-micro \
    --subnet=privatesubnet-us --image-family=debian-10 --image-project=debian-cloud \
    --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=privatenet-us-vm
# List the VMs, sorted by zone
gcloud compute instances list --sort-by=ZONE
```

## Implement Private Google Access and Cloud NAT
```bash
# Create a Storage Bucket
gsutil mb -l US gs://abs_lab_bucket
# Copy an image file into the bucket
gsutil cp gs://cloud-training/gcpnet/private/access.svg gs://abs_lab_bucket
# Copy the image from the bucket into local VM. This should work
gsutil cp gs://abs_lab_bucket/*.svg .

# Create a VPC network
gcloud compute networks create privatenet --subnet-mode=custom
# Create subnet within the VPC
gcloud compute networks subnets create privatenet-us --network=privatenet --region=us-central1 --range=10.130.0.0/20	
# create the firewall rule
# IAP connections come from a specific set of IP addresses (35.235.240.0/20)
gcloud compute firewall-rules create privatenet-allow-ssh --direction=INGRESS \
    --priority=1000 --network=privatenet --action=ALLOW --rules=tcp:22 \
    --source-ranges=35.235.240.0/20
# Create a VM instance with no public IP address on the subnet
gcloud compute instances create vm-internal --zone=us-central1-c \
    --machine-type=n1-standard-1 --subnet=privatenet-us \
    --image-family=debian-10 --image-project=debian-cloud \
    --boot-disk-size=10GB --boot-disk-type=pd-standard \
    --boot-disk-device-name=vm-internal --no-address
# SSH to vm-internal to test the IAP tunnel
gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap
# Test external connectivity from the VM. It won't work as the VM has no external IP address
ping -c 2 www.google.com
# Copy the image to vm-internal. It won't work as the VM has no external IP address 
gsutil cp gs://abs_lab_bucket/*.svg .
# Ctrl+C to stop the request. Exit the VM

# Enable Private Google Access to reach external IP addresses of Google APIs and service
gcloud compute networks subnets update privatenet-us --enable-private-ip-google-access

# SSH to vm-internal through the IAP tunnel
gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap
# Test external connectivity from the VM. It won't work as the VM has no external IP address
ping -c 2 www.google.com
# Copy the image to vm-internal. This works as the Private Google Access is enabled
gsutil cp gs://abs_lab_bucket/*.svg .

# Configure a Cloud NAT gateway
# Create a Cloud Router associated with the VPC
gcloud compute routers create nat-router \
    --network=privatenet \
    --region=us-central1
# Create a Cloud NAT gateway associated with the Cloud Router
gcloud compute routers nats create nat-config \
    --router=nat-router \
    --auto-allocate-nat-external-ips \
    --nat-all-subnet-ip-ranges \
    --enable-logging
# Test external connectivity from the VM. It will work now.
ping -c 2 www.google.com
```
To route requests through a static IP address, you need to configure the  VPC egress to route all outbound traffic through a VPC network that has a Cloud NAT gateway configured with the static IP address. 
 * [Static outbound IP address](https://cloud.google.com/run/docs/configuring/static-outbound-ip#task_overview)