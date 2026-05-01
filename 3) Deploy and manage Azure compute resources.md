# Automate deployment of resources by using Azure Resource Manager (ARM) templates or Bicep files

#### Interpret an Azure Resource Manager template or a Bicep file

There are 2 types of dependancies in Azure Bicep Files, implicit and explicit.

Explicit relies on the `DependsOn:` property, this means the ressource only gets created if that dependency is fulfilled.

Implicit dependency is created when the resource declaration reference another resource in the same deployment. e.g Copy the properties from this other resource.

Deployment Order:  
ARM constructs a directed acyclic graph (DAG) from these dependencies and then:

1. Deploys all resources with no dependencies simultaneously (in parallel)
2. As each resource completes, it unblocks any resources that were waiting on it
3. Those newly unblocked resources deploy in parallel with each other
4. This continues until the graph is fully resolved

#### Modify an existing Azure Resource Manager template

There is a powershell command for exporting templates:
`Export-AzResourceGroup -ResourceGroupName <YourResourceGroupName>`

#### Modify an existing Bicep file

#### Deploy resources by using an Azure Resource Manager template or a Bicep file

> `New-AzResourceGroupDeployment`

Virtual machines are deployed to resource groups, so you must run the New-AzResourceGroupDeployment cmdlet.  
There are a variety of places a template file can be pulled from:

> `-TemplateUri` if web based  
> `-Templatefile` if stored locally  
> `-TemplateSpecId` to specify a template that was save to Azure as a template spec.

You can add a copy element into an ARM template, this acts as a loop in various situations.

#### Export a deployment as an Azure Resource Manager template or convert an Azure Resource Manager template to a Bicep file

> `Save-AzDeploymentTemplate`
> Exports an existing Azure deployment's ARM template and saves it to a local JSON file.

# Create and configure virtual machines

#### Create a virtual machine

Spot Instances: Azure Spot instances allow you to provision virtual machines at a reduced cost, but these virtual machines can be stopped by Azure when Azure needs the capacity for other pay-as-you-go workloads, or when the price of the spot instance exceeds the maximum price that you have set.  
You can assign multiple NICs to the same VM, this would allow it to connect to multiple subnets, but they must be on the same VNet.

If assigning further NICS to access other subnets these will usually need to be configured in the VMs. If using windows / Powershell these are the commands:

> `route add (subnet address IP) 10.0.2.0 MASK 255.255.255.0 (Gateway IP) 10.0.2.1 -p`

!! The subnet Gateway IP in Azure is usually the first available IP in the address range, the -p flag ensures the route persists through reboots!!

VMs which are soft deleted are retained fo 30 days

#### Configure encryption at host for Azure virtual machines

#### Move a virtual machine to another resource group, subscription, or region

| Scenario                | Tool                                             | VM needs to stop? | Notes                                                                                                                             |
| ----------------------- | ------------------------------------------------ | ----------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| Between Resource Groups | `Move-AzResource`                                | Recommended       | Must move dependent resources too (NIC, disks). RBAC assignments are preserved.                                                   |
| Between Subscriptions   | `Move-AzResource` + `-DestinationSubscriptionId` | Recommended       | Both subs must be in same Entra tenant. Resource providers must be registered in destination. RBAC assignments do NOT carry over. |
| Between Regions         | Azure Resource Mover / manual snapshot / ASR     | Yes               | No native move — VM must be recreated. Azure Resource Mover is the recommended tool.                                              |

You can also redeploy VMs, using both the portal and the 'Set-AzVM -ResourceGroupName "myRG" -Name "myVM" -Redeploy' powershell command.  
This Shuts down the VM, Moves it to a new physical host within the same region, Restarts it there.

#### Manage virtual machine sizes

#### Manage virtual machine disks

!! Data Disks can be detached whilst the VM is on, OS Disks cannot !!

#### Deploy virtual machines to availability zones and availability sets

Planned/scheduled maintenance → update domains (one taken down at a time)  
Unplanned hardware failure → fault domains think of zone level failure, most regions only have 3 availability zones, which would be the max most times.

#### Deploy and configure an Azure Virtual Machine Scale Sets

# Provision and manage containers in the Azure portal

When you deploy a container group into a VNet, Azure container instanced requires a delegated subnet for them to run in, no other resources can run on this subnet.

#### Create and manage an Azure Container Registry

#### Provision a container by using Azure Container Instances

#### Provision a container by using Azure Container Apps

#### Manage sizing and scaling for containers, including Azure Container Instances and Azure Container Apps

An Azure container instance (Docker container) can mount Azure File Storage shares as directories and use them as persistent storage.

# Create and configure Azure App Service

#### Provision an App Service plan

| Tier         | Custom Domain | SSL | Scale Out (instances) | Storage | Auto-scale | Use Case         |
| ------------ | ------------- | --- | --------------------- | ------- | ---------- | ---------------- |
| **Free**     | ❌            | ❌  | 1                     | 1 GB    | ❌         | Dev/test only    |
| **Shared**   | ✅            | ❌  | 1                     | 1 GB    | ❌         | Dev/test only    |
| **Basic**    | ✅            | ✅  | Up to 3               | 10 GB   | ❌         | Low traffic prod |
| **Standard** | ✅            | ✅  | Up to 10              | 50 GB   | ✅         | Production       |
| **Premium**  | ✅            | ✅  | Up to 30              | 250 GB  | ✅         | High scale prod  |
| **Isolated** | ✅            | ✅  | Up to 100             | 1 TB    | ✅         | VNet isolated    |

#### Configure scaling for an App Service plan

#### Create an App Service

#### Configure certificates and Transport Layer Security (TLS) for an App Service

#### Map an existing custom DNS name to an App Service

#### Configure backup for an App Service

#### Configure networking settings for an App Service

#### Configure deployment slots for an App Service
