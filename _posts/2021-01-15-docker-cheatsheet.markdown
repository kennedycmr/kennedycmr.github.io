---
layout: post
title:  "Docker Cheetsheet"
date:   2021-01-15 06:00:00 -0500
categories: windows docker terraform gcp
tags: windows docker terraform gcp
---
I have started the new year with the intention of using my employer provided laptop for several months. This comes with Windows 10 as the SOE, and so I will be re-aquainting myself with using Windows 10 in a productive manner. As much of my work right now, revolves around creating applications and setting up pipelines and CI/CD configurations for resources provisioned on GCP, I find it easiest to use docker on my workstation as a means to quickly QA pipelines. The below will be common docker configurations I use. 

# Prequisties
* Docker installed (you will need admin access)
* Debian WSL2 container installed from the Windows store.  
  * To get around the company blocking of the Windows store, modify the following registry key: 
{% highlight powershell %}
HKLM\SOFTWARE\Policies\Microsoft\WindowsStore\RequirePrivateStoreOnly = 0
{% endhighlight %}
* Microsoft Windows Terminal (also from the Windows store) 

# Standard Docker Image with Terraform
* Ensure Docker is started on the workstation. 

* Create a folder that will be mounted on the Docker image. This will be used as a way to get files in/out of the container so that we don't lose important information when the container stops. (e.g. C:\Temp)

* Create a variable pointing to that folder
{% highlight powershell %} 
$folder = 'C:\Temp'
{% endhighlight %}

* Create a file in that folder called "setup.sh". The contents of the file should be: 
{% highlight bash %}
#!/bin/bash 

apt-get install -y unzip gettext-base python3 python3-dev python3-venv wget
export TERRAFORM_VERSION=0.13.5
cd ~
rm -rf terrafor*
wget -q https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip
unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip
export PATH=$PATH:$(pwd)

mkdir -p /workdir/terraform
cd /workdir/terraform
terraform --version
{% endhighlight %}

* Start the container using the standard Google container that has the gcloud binaries already installed
{% highlight bash %}
docker run --volume="$folder":"/workdir" -ti google/cloud-sdk:latest bash
{% endhighlight %}

* The container should be started and you should be at a prompt inside the container. Run the setup script which will setup the container. 
{% highlight bash %}
source /workdir/setup.sh
{% endhighlight %}
