
Terraform is used to manage infrastructure as code in a repeatable, automated, and controlled manner. In the context of Google Cloud Platform (GCP), Terraform helps manage various resources, like virtual machines, storage buckets, networks, and databases, without manually clicking through the GCP console.

Why Use Terraform for Google Cloud Infrastructure?
Infrastructure as Code (IaC):

With Terraform, you define your entire infrastructure (e.g., VMs, networks, storage) in simple configuration files (.tf files).
This allows you to version control your infrastructure, which means you can track changes and collaborate with others (e.g., in Git).
You can apply and replicate the same configurations across different environments (e.g., development, staging, production) without errors or manual steps.


With Terraform, you define your entire infrastructure (e.g., VMs, networks, storage) in simple configuration files (.tf files).

Before excute below in git bash, make sure  config is in .tf file 
terraform init:
   Initializes & configures the backend, installs plugins/providers, & checks out an existing configuration from a version control
terraform plan:
   Matches/previews local changes against a remote state, and proposes an Execution Plan.
terraform apply:
   Asks for approval to the proposed plan, and applies changes to cloud
terraform destroy
   Removes your stack from the Cloud