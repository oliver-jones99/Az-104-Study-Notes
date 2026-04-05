# Manage Azure identities and governance (20–25%)
## Resources:
-[John Savil Identity](https://www.youtube.com/watch?v=megA6BPpYqo&list=PLlVtbbG169nGlGPWs9xaLKT1KfwqREHbs&index=6)


## Manage Microsoft Entra users and groups

Entra ID is uses as the clouds IDP, this is frequently linked to traditional AD DS.
These services handle a lot of the Authentication and Authorization in your environment

-Create users and groups

This can be done in a variety of ways, often on platforms moving to the cloud traditional AD AS services are synced to entra through Connect Sync or Cloud Sync
Users and groups can be created in the entra portals or alternatively through bulk operations from CSV.

-Manage user and group properties

Groups should help enforce principle of least priviledge, the right permissions for the right accounts for the right people.
Soft delete lasts 30 days.
Security groups for access control and 365 groups for collaboration.
Can create dynamic groups where users are added through rules.

-Manage licenses in Microsoft Entra ID
Licenses can be assigned directly to users or through groups.

-Manage external users
B2B, authenticates through the external users tenants, for cross company collaboration.
External ID, aimed at customers and usually kept in a seperate tenant from your own business tenant.

-Configure self-service password reset (SSPR)
Need Entra P1 or P2 license for this.
Need 1 or 2 auth methods.

## Manage access to Azure resources
-Manage built-in Azure roles

-Assign roles at different scopes

Roles can be applied to groups, users, service principals, and managed identities.

-Interpret access assignments

## Manage Azure subscriptions and governance
-Implement and manage Azure Policy

-Configure resource locks
2 Types, ReadOnly and CanNotDelete, can be applied at subscription, resource group, or resource level, and are inhereted down.

-Apply and manage tags on resources

-Manage resource groups

-Manage subscriptions

-Manage costs by using alerts, budgets, and Azure Advisor recommendations

-Configure management groups
