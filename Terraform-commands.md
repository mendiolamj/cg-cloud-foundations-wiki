Terraform CLI tricks

terraform -install-autocomplete #Setup tab auto-completion, requires logging back in

When you need a quick reference guide, you can always pull up Terraform’s built-in command-line documentation. There are a couple of commands for doing that:

terraform

terraform -h

terraform --help

Format and Validate Terraform code

terraform fmt #format code per HCL canonical standard

terraform validate _ #validate code for syntax_

terraform validate -backend=false #validate code skip backend validation

Initialize your Terraform working directory

terraform init #initialize directory, pull down providers

terraform init -get-plugins=false #initialize directory, do not download plugins

terraform init -verify-plugins=false #initialize directory, do not verify plugins for Hashicorp signature