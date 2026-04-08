# Automate deployment of resources by using Azure Resource Manager (ARM) templates or Bicep files
#### Interpret an Azure Resource Manager template or a Bicep file

#### Modify an existing Azure Resource Manager template

#### Modify an existing Bicep file

#### Deploy resources by using an Azure Resource Manager template or a Bicep file
> `New-AzResourceGroupDeployment`

Virtual machines are deployed to resource groups, so you must run the New-AzResourceGroupDeployment cmdlet.  
There are a variety of places a template file can be pulled from:  
> `-TemplateUri` if web based  
> `-Templatefile` if stored locally  
> `-TemplateSpecId` to specify a template that was save to Azure as a template spec.   

#### Export a deployment as an Azure Resource Manager template or convert an Azure Resource Manager template to a Bicep file

# Create and configure virtual machines
#### Create a virtual machine

#### Configure encryption at host for Azure virtual machines

#### Move a virtual machine to another resource group, subscription, or region

#### Manage virtual machine sizes

#### Manage virtual machine disks
!! Data Disks can be detached whilst the VM is on, OS Disks cannot !!  


#### Deploy virtual machines to availability zones and availability sets  

Planned/scheduled maintenance → update domains (one taken down at a time)  
Unplanned hardware failure → fault domains (rack/power failure)  

#### Deploy and configure an Azure Virtual Machine Scale Sets

# Provision and manage containers in the Azure portal
#### Create and manage an Azure Container Registry

#### Provision a container by using Azure Container Instances

#### Provision a container by using Azure Container Apps

#### Manage sizing and scaling for containers, including Azure Container Instances and Azure Container Apps
An Azure container instance (Docker container) can mount Azure File Storage shares as directories and use them as persistent storage.  

# Create and configure Azure App Service
#### Provision an App Service plan  
| Tier | Custom Domain | SSL | Scale Out (instances) | Storage | Auto-scale | Use Case |
|------|--------------|-----|----------------------|---------|------------|----------|
| **Free** | ❌ | ❌ | 1 | 1 GB | ❌ | Dev/test only |
| **Shared** | ✅ | ❌ | 1 | 1 GB | ❌ | Dev/test only |
| **Basic** | ✅ | ✅ | Up to 3 | 10 GB | ❌ | Low traffic prod |
| **Standard** | ✅ | ✅ | Up to 10 | 50 GB | ✅ | Production |
| **Premium** | ✅ | ✅ | Up to 30 | 250 GB | ✅ | High scale prod |
| **Isolated** | ✅ | ✅ | Up to 100 | 1 TB | ✅ | VNet isolated |  

#### Configure scaling for an App Service plan

#### Create an App Service

#### Configure certificates and Transport Layer Security (TLS) for an App Service

#### Map an existing custom DNS name to an App Service

#### Configure backup for an App Service

#### Configure networking settings for an App Service

#### Configure deployment slots for an App Service
