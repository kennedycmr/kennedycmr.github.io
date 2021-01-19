---
layout: post
title:  "Terraform - Create a Folder, Project and Service Account in GCP"
date:   2021-01-18 18:56:00 -0500
categories: terraform gcp devops
tags: terraform gcp devops
---
Below is a standard code block to get started creating a basic GCP project structure. In this example we create the following as starting blocks: 
* New project
* New folder to house the project
* New service account in the project

Of course, nothing substitutes the actual Terraform documentation, but below is a common starting block I use.  

* Start with declaring some static variables in the pipeline that are passed into Terraform:
{% highlight bash %}
export TF_VAR_GCP_ORG_ID="xxxxxxxxxxxx"
export TF_VAR_BILLING_ACCOUNT_ID="xxxxx-xxxxx-xxxxx"
{% endhighlight %}

* The run Terraform using the below code example. This could be all in a single 'main.tf' file or seperated out in dedicated variables, and other files named according to what they are doing. 
{% highlight python %}
variable "GCP_ORG_ID" {
  description = "organization id. Obtain from IAM -> Manage Resources"
  type        = string
}

variable "BILLING_ACCOUNT_ID" {
  description = "Billing account ID to be used by default"
  type        = string
}


resource "google_folder" "commonservices" {
  display_name = "Common Services"
  parent       = "organizations/${var.GCP_ORG_ID}"
}

resource "google_project" "my_gcp_project" {
  name       = "my_gcp_project"
  project_id = "my_gcp_project222"
  folder_id  = google_folder.commonservices.name
  billing_account = var.BILLING_ACCOUNT_ID
}

resource "google_service_account" "my_gcp_project" {
  account_id = "run-service-account"
  display_name = "run-service-account"
  description = "Used as a run account for cloud function myFunction"
  project = google_project.my_gcp_project.id
}
{% endhighlight %}
 
