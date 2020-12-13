---
layout: post
title:  "Start an Interactive Podman Container for GCloud SDK Testing"
date:   2020-12-13 05:39:00 -0500
categories: iac 
tags: containers gcp gcloud
---
Right now I am doing a lot of work with developing business services that run on Google Cloud Platform. My specialization is the pipelines required to automate deployment of the IaC, using Bitbucket or Cloud Build pipelines and leveraging Terraform. Rather than waiting an average 4 minutes for a pipeline run to determine if my code logic is correct, it is far quicker to run a local container to manually verify scripts and processes before committing those changes to the repository. Using a container over using my normal user profile on my laptop also means I can be sure I am using a virgin environment and not relying on specifics of my OS installation that won't be available natively in a container. 

Below are the commands I use to start those containers. I will add to them over time as my needs mature. 

# Regular Daily Use 
The below command explained: 
run = run this image. 
- --volume="$PWD":"/workdir" : this means to mount the current directory I am in, as the "/workdir" directory in the container. In this way, we have a way to move files in/out of the container.   
- -ti  = run interactive. In otherwords, run the container, and allow you to type commands and do stuff in it.   
- google/cloud-sdk"latest   = run the container from google that has the latest cloud-sdk installed. This way all I need is a service account keys file and I can authenticate to Google Cloud Platform and do things.   
- bash   = this is the command to run when the container is launched. In this case I want the bash shell to launch.  


{% highlight bash %}
podman run --volume="$PWD":"/workdir"  -ti google/cloud-sdk:latest bash
{% endhighlight %}
