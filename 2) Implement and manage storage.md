# Implement and manage storage (15–20%)

## Configure access to storage  
#### -Configure Azure Storage firewalls and virtual networks  
Azure File Shares can be accessed through the Server Message Block (SMB) protocol. This uses TCP and therefore travels through port 445.  

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
These policies can then be deleted, which will in turn revoke any access. These are the go to for any situation where access   may need to be revoked.
Cannot see token once the page has been shut.  

Immutability Policies, there are 2 types:  
1) Time Based Retention Policy: Blobs cannot be modified or deleted for a specific period  
2) Legal Hold: Applied via a tag to relevant resources, stays in place until someone removes that tag.  
The Policy needs to be locked to be compliant.  



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
Data Lake Storage is activated by enabling Hierarchical Namespace (HNS) on the storage account and is blob based.  
It is only supported on Standard GPv2 and Premium Block Blob accounts.  
It is for big data processing, data pipelines, cost-effective analytics storage.  

Exam Gothcas:  
!! This is a one way change, once upgraded to HNS you cannot reverse !!  
!! To enable POSIX-compliant access control lists (ACLs), the hierarchical namespace must be used !!

#### -Configure Azure Storage redundancy
Locally Redudant Storage (LRS):  
-3 copies within a single data center. (Protects agaist rack level failure.)  

Zone Redundant Storage (ZRS):   
-3 copies across 3 availability zones. (Protects against Zone or "Data Center" level failure.)  

Geo Redundant Storage (GRS):  
-6 copies across 2 regions (LRS in each region, Protects against region level failure.)  

Geo Zone Redundant Storage (GZRS):  
-6 copies total, ZRS in primary region and LRS in secondary region.  

!! GRS and GZRS only support reading in the secondary region after a failover !!  
For reading in the secondary region under normal circumstances, you need Read Access versions (RA-GRS, RA-GZRS)   


#### -Configure object replication
Asynchronus replication can be configures between storage accounts.   
The accounts can be in different regions, subscriptions, and tenants.  
requirements: Blob Versioning enabled, Storage v2 or Premium Blob, Source account needs Change Feed enabled, Both need Blob Versioning enabled.  
Can't be done on datalake accounts.  
Only replicates Blobs.  

#### -Configure storage account encryption

#### -Manage data by using Azure Storage Explorer and AzCopy

## Configure Azure Files and Azure Blob Storage
#### -Create and configure a file share in Azure Files

#### -Create and configure a container in Azure Blob Storage

#### -Configure storage tiers

| Tier | Access Frequency | Storage Cost | Access/Retrieval Cost | Retrieval Speed | Min Storage Duration |
|------|-----------------|-------------|----------------------|-----------------|----------------------|
| **Hot** | Frequent | Highest | Lowest | Instant | None |
| **Cool** | Infrequent | Medium | Medium | Instant | 30 days |
| **Cold** | Rarely | Lower | Higher | Instant | 90 days |
| **Archive** | Almost never | Cheapest | Most expensive | Hours (rehydration) | 180 days |

Exam Gotchas:
Archive blobs must be rehydrated for access, can take up to 15 hours.  
Archive not available on ZRS and GZRS.  
Tiers are set per blob, the accounts tier is just what each blob defaults to.  

#### -Configure soft delete for blobs and containers

#### -Configure snapshots and soft delete for Azure Files

#### -Configure blob lifecycle management

For any access based lifecycle policies, Access tracking needs to be enabled in the data protection blade.  
After this is done you can create policies based on days since last accessed.  

#### -Configure blob versioning
