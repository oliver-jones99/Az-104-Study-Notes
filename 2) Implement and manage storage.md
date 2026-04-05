# Implement and manage storage (15–20%)

## Configure access to storage
#### -Configure Azure Storage firewalls and virtual networks

#### -Create and use shared access signature (SAS) tokens
Shared access signature tokens can grant highly customisable and restricted rights to Storage Accounts.
Can control what parts of the storage account, permissions, versioning, start and end time, and even allowed IP ranges.
Cannot see token once the page has been shut.
!! These cannot be revoked for the storage account once created. to have a revokable access method read below !!

#### -Configure stored access policies
These can be at the resource level (blob, container etc), and then SAS tokens can be generated and applied to the policy.
Can use account key (authenticates via cryptographic key), or user delegation (authenticates through RBAC and Entra)
User Delegation Tokens:
User cannot assign more permissions than what they have.
Can be revoked in Entra- also get audit logs in entra.
These policies can then be deleted, which will in turn revoke any access. These are the go to for any situation where access may need to be revoked.
Cannot see token once the page has been shut.

#### -Manage access keys
Access keys are used to authenticate application requests.
There are 2 available to allow 0 downtime (Can point all apps to the secondary key whilst the primary key is being rotated.)
"Rotating" a key creates a new one, and immediately invalidates the old one

#### -Configure identity-based access for Azure Files
In IAM -> add role assignments -> pick role (what people can do) -> pick members (who can do it, ideally through a group).
Here you can also edit conditional access, what actions can be done after what checks.
For higher level administration of RBAC you would use Azure Policy applied to a management group which would then be inherited down.

Exam Gotchas:
"Identity-based access" = file shares = Entra Kerberos.
This is not RBAC, this allows domain joined devices to access the file shares like a network drive.

## Configure and manage storage accounts
#### -Create and configure storage accounts

#### -Configure Azure Storage redundancy

#### -Configure object replication

#### -Configure storage account encryption

#### -Manage data by using Azure Storage Explorer and AzCopy

## Configure Azure Files and Azure Blob Storage
#### -Create and configure a file share in Azure Files

#### -Create and configure a container in Azure Blob Storage

#### -Configure storage tiers
#### -Configure soft delete for blobs and containers

#### -Configure snapshots and soft delete for Azure Files

#### -Configure blob lifecycle management

#### -Configure blob versioning
