# Udacity Cloud DevOps using Microsoft Azure Nanodegree Program Capstone Project: Ensuring Quality Releases

## Table of Contents
* [Introduction](#Introduction)
* [Getting Started](#Getting-Started)
* [Install our dependencies](#Install-our-dependencies)
* [Configure storage account and state backend for Terraform](#Configure-storage-account-and-state-backend-for-Terraform)
* [Create a Service Principal for Terraform](#Create-a-Service-Principal-for-Terraform)
* [References](#References)

## Introduction
This is the capstone project for the Udacity Cloud DevOps using Microsoft Azure Nanodegree Program, in this project we create disposable test environments and run a variety of automated tests with the click of a button. Additionally, we monitor and provide insight into an application's behavior, and determine root causes by querying the application’s custom log files.

For this project we use the following tools:

- Azure DevOps: For creating a CI/CD pipeline to run Terraform scripts and execute tests with Selenium, Postman and Jmeter
- Terraform: For creating Azure infrastructure as code (IaS)
- Postman: For creating a regression test suite and publish the results to Azure Pipelines.
- Selenium: For creating a UI test suite for a website.
- JMeter: For creating a Stress Test Suite and an Endurance Test Suite.
- Azure Monitor: For configuring alerts to trigger given a condition from an App Service.

## Getting Started
For this project we will follow the next steps:
1. Install our dependencies
2. Configure storage account and state backend for Terraform
3. Create a Service Principal for Terraform
- Use Terraform.
- Configure Azure Monitor.
- Configure Azure Log Analytics.

## Install our dependencies
For the successful run of this project we need to do the following prework:
1. Install Visual Studio Code: https://code.visualstudio.com/
2. Create an Outlook Account: https://outlook.live.com/
3. Create a free Azure Account: https://azure.microsoft.com/
4. Create an Azure Devops account: https://azure.microsoft.com/services/devops/
5. Install Azure CLI: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
6. Install Terraform: https://learn.hashicorp.com/tutorials/terraform/install-cli#install-terraform
7. Install the Java Development Kit: https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
8. Install JMeter: https://jmeter.apache.org/download_jmeter.cgi
9. Install Postman: https://www.postman.com/downloads/
10. Install Python: https://www.python.org/downloads/
11. Install Selenium for Python: https://pypi.org/project/selenium/
12. Install Chromedriver: https://sites.google.com/a/chromium.org/chromedriver/downloads

## Configure storage account and state backend for Terraform
Terraform state is used to reconcile deployed resources with Terraform configurations. State allows Terraform to know what Azure resources to add, update, or delete. By default, Terraform state is stored locally when you run the terraform ```apply``` command. This configuration isn't ideal for the following reasons:

Local state doesn't work well in a team or collaborative environment.
Terraform state can include sensitive information.
Storing state locally increases the chance of inadvertent deletion.
Terraform supports the persisting of state in remote storage. One such supported back end is Azure Storage. This document shows how to configure and use Azure Storage for this purpose.

For the project we will use Azure Storage for persisting the Terraform state in remote storage. For this we will run the ```terraformconfig.sh``` file, which has the necessary configuration files for creating a blob storage to store the state.

First, we have to ensure we are logged in into Azure CLI by running the following command:
```Bash
az login
```

Once that's done, we can run the ```terraformconfig.sh``` file with the following command:
```Bash
sh terraformconfig.sh
```

We will get 3 outputs:
- storage_account_name
- container_name 
- access_key

We will use this fields, plus a ```key``` field we will define as ```test.terraform.tfstate```

The access_key can be sourced from an Environment Variable, the environment variables that can be used with the Azure provider must have the “ARM” prefix, 
To do this open the Terminal and write the following command:
```Bash
export ARM_ACCESS_KEY="your_access_key" (Unix Systems)

setx ARM_ACCESS_KEY="your_access_key" (Windows Systems)
```

In the ```main.tf``` in the ```environments\test``` directory input the following fields:
```Bash
terraform {
  backend "azurerm" {
    storage_account_name = "your_storage_account_name"
    container_name       = "your_container_name"
    key                  = "test.terraform.tfstate"
    access_key           = "ARM_ACCESS_KEY"
  }
}
```




## Create a Service Principal for Terraform


## References
- [Udacity Project Starter Files](https://video.udacity-data.com/topher/2020/June/5ed815bf_project-starter-resources/project-starter-resources.zip)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Outlook](https://outlook.live.com/)
- [Azure](https://azure.microsoft.com/)
- [Azure DevOps](https://azure.microsoft.com/services/devops/)
- [Azure Command Line Interface](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli#install-terraform)
- [Terraform Azure Documentation](https://learn.hashicorp.com/collections/terraform/azure-get-started)
- [Java Development Kit](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
- [Jmeter](https://jmeter.apache.org/download_jmeter.cgi)
- [Postman](https://www.postman.com/downloads/)
- [Python](https://www.python.org/downloads/)
- [Selenium for Python](https://pypi.org/project/selenium/)
- [Chromedriver](https://sites.google.com/a/chromium.org/chromedriver/downloads)
- [Tutorial: Store Terraform state in Azure Storage](https://docs.microsoft.com/en-us/azure/developer/terraform/store-state-in-azure-storage)