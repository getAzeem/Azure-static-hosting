# 🚀 Azure Static Website Deployment with Terraform & Azure DevOps

This project provides a complete solution for automatically deploying a static website to **Azure Blob Storage**. It leverages **Terraform** for Infrastructure as Code (IaC) and **Azure DevOps** for a seamless CI/CD pipeline.

-----

## 🌟 Key Features

  * **Infrastructure as Code**: All Azure resources are declaratively defined using Terraform.
  * **CI/CD Pipeline**: An automated build and deployment pipeline powered by Azure DevOps.
  * **Static Website Hosting**: Utilizes the cost-effective static website hosting feature of Azure Blob Storage.
  * **Remote State Management**: Securely stores the Terraform state file in a remote Azure Storage Account.
  * **Custom Domain & Error Pages**: Easily configurable for a custom domain and includes a custom 404 error page.

-----

## 📂 Project Structure

The repository is organized with the following files:

  * `main.tf`: Defines the core Azure resources.
  * `variables.tf`: Contains variable definitions for customization.
  * `outputs.tf`: Specifies the output values after deployment (e.g., website URL).
  * `providers.tf`: Configures the Terraform providers (e.g., AzureRM).
  * `backend.tf`: Configures the remote backend for Terraform state.
  * `azure-pipeline.yml`: The complete CI/CD pipeline definition for Azure DevOps.
  * `index.html`: The main landing page for the static site.
  * `404.html`: A custom error page.

-----

## 🛠️ Setup and Configuration

Follow these steps to configure and deploy the project.

### Prerequisites

  * An active **Azure Account** subscription.
  * An **Azure DevOps** organization and project.
  * Basic knowledge of Terraform and Azure services.
  * An Azure DevOps agent pool (you can use Microsoft-hosted agents like `ubuntu-latest` or a self-hosted one).

### 1\. Configure the Terraform Backend

**This is a mandatory step.** The Terraform backend stores the state of your infrastructure remotely and safely.

#### Manual Azure Setup for Backend

1.  **Create a Resource Group**: Go to the Azure Portal and create a new resource group.
2.  **Create a Storage Account**: Inside the new resource group, create a storage account.
3.  **Create a Blob Container**: Within the storage account, create a blob container (e.g., `tfstate`).

#### Update `backend.tf`

Open the `backend.tf` file and replace the placeholders with the details of the resources you just created:

```terraform
terraform {
  backend "azurerm" {
    resource_group_name  = "Your-Existing-Resource-Group-Name"
    storage_account_name = "Your-Existing-Storage-Account-Name"
    container_name       = "Your-Container-Name" # e.g., tfstate
    key                  = "terraform.tfstate"
  }
}
```

### 2\. Configure the Azure DevOps Pipeline

#### Create an Azure Service Connection

1.  In Azure DevOps, navigate to **Project Settings** \> **Service connections**.
2.  Click **New service connection** and select **Azure Resource Manager**.
3.  Choose **Service principal (automatic)** and follow the prompts to authorize it for your subscription.
4.  Note down the name of the service connection you created.

#### Update `azure-pipeline.yml`

Open the `azure-pipeline.yml` file and update the following values:

  * **`pool.name`**: Change this to the name of your Azure DevOps agent pool (e.g., `Azure Pipelines` for Microsoft-hosted agents).
  * **Service Connection Name**: Replace all instances of `'Your-Service-Connection-Name'` with the actual name of your service connection.

### 3\. Customize Your Website

  * Modify `index.html` with your website's content.
  * Update `404.html` for your custom error page.
  * Add any other static assets like CSS, JavaScript, or images to the root directory.

-----

## ▶️ Deployment Steps

1.  **Fork or Clone** this repository into your Azure DevOps project.
2.  **Complete** the backend and pipeline configuration steps detailed above.
3.  **Create a New Pipeline** in Azure DevOps, selecting "Existing Azure Pipelines YAML file" and pointing it to `azure-pipeline.yml`.
4.  **Run the pipeline** to automatically provision the infrastructure and deploy your website files.
5.  **Access your website** using the URL provided in the pipeline's output.

-----

## 🌐 Expected Outputs

After a successful pipeline run, the following information will be displayed in the Azure DevOps logs:

  * `resource_group_name`: The name of the newly created resource group.
  * `storage_account_name`: The name of the storage account hosting your site.
  * `primary_web_host`: **The live URL of your static website.**

-----

## 🤔 Troubleshooting Common Issues

  * **Backend Storage Not Found**: Double-check that the resource group, storage account, and container names in `backend.tf` are correct and that these resources exist in Azure.
  * **Authentication Errors**: Verify your service connection has the required permissions (e.g., Contributor) on the Azure subscription.
  * **Terraform Version Conflicts**: Adjust the `terraformVersion` variable in `azure-pipeline.yml` if your configuration requires a different version.

-----

## ⚠️ Important: Clean Up Resources

To avoid incurring Azure costs, remember to destroy the infrastructure when you are finished. You can do this by running the `terraform destroy` command locally (after configuring your local environment) or by adding a destroy stage to your pipeline.
