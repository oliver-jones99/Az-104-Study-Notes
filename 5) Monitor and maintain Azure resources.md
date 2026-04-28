# Monitor resources in Azure

Azure Monitor has 2 main monitoring features: Metrics and Logs.

Metrics are numerical values collected at predetermined intervals to describe some aspect of a system. Azure Monitor Metrics automatically monitors a predefined set of metrics for every Azure VM, and retains the data for 93 days with some exceptions.

Logs are system events containing time stamps and different types of data. Azure automatically records activity logs for all Azure resources. Azure Monitor doesn't collect logs by default, but you can configure Azure Monitor Logs to collect from any Azure resource. Azure Monitor Logs stores log data in a Log Analytics workspace for querying and analysis.

You can create Data collection rules which are then applied to resources: Data collection rules (DCRs) are sets of instructions supporting data collection in Azure Monitor. They provide a consistent and centralized way to define and customize different data collection scenarios.

#### Interpret metrics in Azure Monitor

#### Configure log settings in Azure Monitor

Create a Log Analytics Workspace, here is where the logs will be stored.  
Then we create a Data Collection Rule (DCR), This is where we choose what is collecting, we can filter by all sorts of levels, including warning level.  
During DCR Creation it will automatically push the monitoring agent to VMs.

#### Query and analyze logs in Azure Monitor

Filtering & Shaping

| Operator   | What it does                               | Example                                      |
| ---------- | ------------------------------------------ | -------------------------------------------- |
| `where`    | Filter rows by condition (≈ SQL WHERE)     | `Heartbeat \| where Computer == "VM1"`       |
| `project`  | Select or rename columns                   | `SecurityEvent \| project Account, Activity` |
| `extend`   | Add a new calculated column, keep existing | `Perf \| extend MBFree = CounterValue/1024`  |
| `top`      | Return N rows sorted by a column           | `SecurityEvent \| top 10 by TimeGenerated`   |
| `sort by`  | Order rows asc or desc                     | `Heartbeat \| sort by TimeGenerated desc`    |
| `distinct` | Return unique values of a column           | `SecurityEvent \| distinct Account`          |

Aggregation

| Operator        | What it does                                    | Example                                           |
| --------------- | ----------------------------------------------- | ------------------------------------------------- |
| `summarize`     | Group & aggregate (≈ SQL GROUP BY)              | `SecurityEvent \| summarize count() by Account`   |
| `count()`       | Count rows — used inside summarize              | `Heartbeat \| summarize count() by Computer`      |
| `dcount()`      | Count distinct values                           | `SecurityEvent \| summarize dcount(Account)`      |
| `avg() / sum()` | Average or total of a numeric column            | `Perf \| summarize avg(CounterValue) by Computer` |
| `bin()`         | Round timestamps into buckets (for time charts) | `\| summarize count() by bin(TimeGenerated, 1h)`  |

Time & Joins

| Operator | What it does                               | Example                               |
| -------- | ------------------------------------------ | ------------------------------------- |
| `ago()`  | Relative time filter — most common on exam | `where TimeGenerated > ago(24h)`      |
| `join`   | Merge two tables on a key column           | `TableA \| join (TableB) on Computer` |
| `union`  | Combine rows from multiple tables          | `union SecurityEvent, Syslog`         |

#### Set up alert rules, action groups, and alert processing rules in Azure Monitor

An alert rule in Azure Monitor is used to detect a specific condition, for example when a virtual machine is connected to VNet1. The alert rule monitors the relevant activity or resource signal and triggers when the defined condition occurs. An action group defines what happens when the alert fires, such as sending an email notification to an administrator. Therefore, the alert rule detects the event, and the action group performs the notification action.

The maximum number of notifications which can be sent through an action group is 100 per hour, however voice and SMS are capped at 1 every 5 minutes, meaning the maximum for them is 12 per hour. what actually drives these actions to be triggered is the alert rule.

#### Configure and interpret monitoring of virtual machines, storage accounts, and networks by using Azure Monitor Insights

#### Use Azure Network Watcher and Connection monitor

Network Watcher has several tools:

1. IP flow verify: checks if a packet is allowed or denied from a virtual machine based on 5-tuple information. The security group decision and the name of the rule that denied the packet will be returned. (5-tuple = source and desitination Ip and port, and protocol)

2. NSG Diagnostics: The Network Security Group Diagnostics tool provides detailed information to understand and debug the security configuration of your network. For a given source-destination pair, network security group diagnostics returns all network security groups that will be traversed, the rules that will be applied in each network security group, and the final allow/deny status for the flow.

3. Next Hop: provides the next hop from the target virtual machine to the destination IP address.  
   !Note! You can also get Next Hop information in the Effective Routes blade on the NIC for all the address prefixes, also lets you know which user denifed route causes it.

4. Effective Security Rules: Combined view of all NSGs applying to a NIC

5. VPN Troubleshoot: diagnoses the health of the virtual network gateway or connection. This request is a long running transaction, and the results are returned once the diagnosis is complete. You can select multiple gateways or connections to troubleshoot simultaneously.

6. Packet Capture: uses the Network Watcher Agent VM extension, which needs to be installed on the VM (Azure can do this automatically when you initiate a capture from the portal/CLI) - Works on both Windows and Linux VMs. This then saves a .cap file wherever specifed (Usually on the machine or to a storage account.) which can be opened in something like wireshark.

7. Connection Troubleshoot: provides the capability to check a direct TCP or ICMP connection from a virtual machine (VM), application gateway v2, or Bastion host to a VM, fully qualified domain name (FQDN), URI, or IP address.

8. Connection Monitor: enables you to monitor connectivity in your Azure and hybrid network. Use workspace configuration to store monitoring data generated by Connection Monitor tests in Log Analytics workspace.

#### When to use what tool

| Tool                     | Question it answers                                            |
| ------------------------ | -------------------------------------------------------------- |
| IP Flow Verify           | Is this specific packet allowed or blocked by an NSG?          |
| Next Hop                 | Where will traffic from this VM actually go?                   |
| Effective Security Rules | What NSG rules are actually applying to this NIC right now?    |
| Connection Monitor       | Is connectivity between these two endpoints working over time? |
| NSG Diagnostics          | which exact rule is responsible for this traffic decision?     |
| Packet Capture           | What does the actual packet-level traffic look like?           |
| VPN Diagnostics          | Why is my VPN Gateway/connection not working?                  |

# Implement backup and recovery

#### Create a Recovery Services vault

The Recovery Services Vault is where you can store backups of VMs, SQL databases, Azure File Shares, and Site Recovery. Older

#### Create an Azure Backup vault

The Backup Vault supports less work loads but is growing in scope. It stores the backups of Azure Disks, Blobs, Databases for PostreSQL, and AKS Backups.

The exam rule of thumb: if the question involves VM backup, MARS agents, or Site Recovery — it's an RSV. If it involves managed disks or blobs as the backup target — it's a Backup Vault.

#### Create and configure a backup policy

Backups are saved for 30 days by default

#### Perform backup and restore operations by using Azure Backup

| Restore Options               | What it does                                                                                                                                                                                                                                                                         | Laymans Use case                                                                                                                                                     |
| :---------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Create a new VM               | Recreates the VM from a restore point, must be in same region as source VM.                                                                                                                                                                                                          | Builds you a brand new VM from the restore point automatically.                                                                                                      |
| Restore Disk                  | Restores a VM disk, which can then be used to create a new VM. The disks are copied to the resource group you specify                                                                                                                                                                | Takes the disk from the restore point and drops it as a VHD into a storage account. give you the raw disk and you decide what to do with it.                         |
| Replace Existing (Disk on VM) | You can restore a disk and use it to replace a disk on the existing VM. Azure Backup takes a snapshot of the existing VM before replacing the disk and stores it in the staging location you specify. **The current VM must exist. You can't use this option if the VM is deleted.** | Swaps out the OS/data disk on the same VM with one from a restore point. Your VM is still there but something went wrong with the disk contents so you roll it back. |
| Cross-region Restore          | Restores into a paired region. Only works if GRS was enabled on the vault.                                                                                                                                                                                                           |                                                                                                                                                                      |
| Recover Files                 | Recovers individual files from a recovery point by mounting the snapshot on the target machine using the iSCSI initiator in the machine.                                                                                                                                             |                                                                                                                                                                      |

Exam Gotchas:

1. Cross-region restore requires GRS vault, not LRS.
2. File recovery requires downloading a script and running it on the target VM
3. Replace existing only works if the original VM still exists

#### Configure Azure Site Recovery for Azure resources

Azure Site Recovery supports churn (data change rate) up to 100 MB/s per virtual machine. You'll be able to protect your Azure virtual machines having high churning workloads (like databases) using the High Churn option in Azure Site Recovery.  
**Important**: This feature is available in all regions where Azure Site Recovery is supported and Premium Blob storage accounts are available.

#### Perform a failover to a secondary region by using Site Recovery

#### Configure and interpret reports and alerts for backups
