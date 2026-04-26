# Manage Azure identities and governance (20–25%)

## Resources:

-[John Savil Identity](https://www.youtube.com/watch?v=megA6BPpYqo&list=PLlVtbbG169nGlGPWs9xaLKT1KfwqREHbs&index=6)

## Manage Microsoft Entra users and groups

Entra ID is uses as the clouds IDP, this is frequently linked to traditional AD DS.
These services handle a lot of the Authentication and Authorization in your environment

#### -Create users and groups

This can be done in a variety of ways, often on platforms moving to the cloud traditional AD AS services are synced to entra through Connect Sync or Cloud Sync
Users and groups can be created in the entra portals or alternatively through bulk operations from CSV.

#### -Manage user and group properties

Groups should help enforce principle of least priviledge, the right permissions for the right accounts for the right people.
Soft delete lasts 30 days.
Security groups for access control and 365 groups for collaboration.
Can create dynamic groups where users are added through rules.

#### -Manage licenses in Microsoft Entra ID

Licenses can be assigned directly to users or through groups.

Exam Gotcha:
Users need a Usage Location assigned before a license can be applied, this is becasue ot all Microsoft 365 services are available in all locations

#### -Manage external users

B2B, authenticates through the external users tenants, for cross company collaboration.
External ID, aimed at customers and usually kept in a seperate tenant from your own business tenant.

Can use External Collabortation settings to specify which roles in your organization can invite external users for B2B collaboration.

#### -Configure self-service password reset (SSPR)

Need Entra P1 or P2 license for this.
Need 1 or 2 auth methods.

## Manage access to Azure resources

#### -Manage built-in Azure roles

#### -Assign roles at different scopes

Roles can be applied to groups, users, service principals, and managed identities.

#### -Interpret access assignments

## Manage Azure subscriptions and governance

#### -Implement and manage Azure Policy

To administer Azure Policy through a CLI, `Microsoft.PolicyInsights` must be registered in your Subscription.  
Microsoft.PolicyInsights must be registered to query compliance state and trigger remediation — without it, Policy is write-only.

#### -Configure resource locks

2 Types, ReadOnly and CanNotDelete, can be applied at subscription, resource group, or resource level, and are inhereted down.

Delete locks can be applied to subscriptions, resource groups, and individual resources.

Exam Gotcha: a Delete Lock prevents accidental deletion of resources within the resource group while still allowing the resource group itself to be deleted once it is empty.

#### -Apply and manage tags on resources

#### -Manage resource groups

#### -Manage subscriptions

#### -Manage costs by using alerts, budgets, and Azure Advisor recommendations

#### -Configure management groups
