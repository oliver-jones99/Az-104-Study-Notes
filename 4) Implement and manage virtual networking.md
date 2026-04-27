# Configure and manage virtual networks in Azure

#### Create and configure virtual networks and subnets

#### Create and configure virtual network peering

Peering is configured from inside the VNet resource → Settings → Peerings → + Add.
The portal form creates both directions simultaneously — you name the A→B link and the B→A link in one operation. Azure creates two separate peering objects.  
Peering is not transitive. If VNet-A peers with VNet-B, and VNet-B peers with VNet-C, VNet-A cannot communicate with VNet-C automatically.

#### Configure public IP addresses

Public IPs can be applied to NICs, Load Balancers, VPC gateways etc

#### Configure user-defined routes

#### Troubleshoot network connectivity

# Configure secure access to virtual networks

#### Create and configure network security groups (NSGs) and application security groups

ASGs let you group NICs by logical roles, for example asg-webservers.  
You can reference these groups inside of NSGs instead of hard coding IP addresses.  
This eases administration as if you need to add VMs or NICS to an NSG, you can just manage the application group.

**Exam Gotchas:**

1. ASGs Must be in the same region as the VMs theyre assigned to.
2. A Nic can be a member of multiple ASGs.
3. ASGs are referenced in NSG in / out rules as a source or destination.
4. You can combine ASG and Subnet-based rules in the same NSG.
5. An ASG can only contain NICs from a single VNet, determined by whichever VNet the first assigned NIC belongs to.

#### Evaluate effective security rules in NSGs

#### Implement Azure Bastion

#### Configure Network Gateways and VPN Gateways

P2S VPN can be setup to allow individual computers (points) to access your private Azure networks. To achieve this you need to set up a Gateway Subnet in your VNet to house your gateway options (Express Route, VPN gateway which can then house P2S and S2S)  
Its recommended to give your Gateway Subnet a /27 range so it can accomodate most of the options, /28 is the minimum but leaves no headroom.

P2S can then be configured on your VPN gateway, it requires its own address space which must not conflict with your client side network or private network.

Point-to-Site (P2S) VPN clients must be downloaded and reinstalled again after virtual network peering is successfully configured to ensure that the new routes are downloaded to the client.

You can also configure a S2S Gateway on the same VPN Gateway to connect your on prem network to your private VNet, though this requires a Local Network Gateway on the client side.

#### Configure service endpoints for Azure platform as a service (PaaS)

#### Configure private endpoints for Azure PaaS

# Configure name resolution and load balancing

#### Configure Azure DNS

Delegation = NS record in the parent zone, named after the subdomain, pointing at the child's nameservers

Azure DNS Zone core concept: Azure hosting your DNS records for you — instead of managing a DNS server yourself.

Public DNS Zones: Used for internet facing domains, you create a public DNS zone and point your domain registrar's nameservers at Azure's.

Private DNS Zone: Used for internal name resolution within Azure. Not accessible from the internet at all — only VNets you explicitly link to the zone can use it.

Azure Private DNS Resolver: It's a managed service that acts as a DNS proxy for hybrid scenarios — specifically when your on-premises network needs to resolve Azure private DNS zones, or when Azure VMs need to resolve on-premises DNS names. It has inbound and outbound endpoints that bridge the two worlds.

|                                | Public DNS Zone                                          | Private DNS Zone                          | DNS Private Resolver                                 |
| ------------------------------ | -------------------------------------------------------- | ----------------------------------------- | ---------------------------------------------------- |
| **Purpose**                    | Host DNS records for internet-facing domains             | Internal name resolution within Azure     | Bridge between on-premises DNS and Azure private DNS |
| **Who can query it**           | Anyone on the internet                                   | Only linked VNets                         | On-premises networks and Azure VNets (hybrid)        |
| **Requires VNet link**         | No                                                       | Yes — mandatory                           | Yes — inbound/outbound endpoints attached to subnets |
| **Auto VM registration**       | No                                                       | Yes — optional tick box when linking      | No                                                   |
| **DNSSEC**                     | Yes                                                      | No                                        | No                                                   |
| **Managed nameservers**        | Yes — Azure provides ns1-xx through ns4-xx               | N/A — not internet accessible             | N/A                                                  |
| **Registrar change needed**    | Yes — point your domain registrar at Azure's nameservers | No                                        | No                                                   |
| **Typical use case**           | `contoso.com` public website                             | `vm1.corp.internal` private VM resolution | On-prem servers resolving Azure private zones        |
| **Internet accessible**        | Yes                                                      | No                                        | No — internal proxy only                             |
| **Replaces custom DNS server** | Yes — for public domains                                 | Yes — for internal Azure resolution       | Partially — for hybrid forwarding scenarios          |

#### Configure an internal or public load balancer

Azure Load Balancer Distribution Modes

Azure Load Balancer uses a hash-based algorithm to distribute traffic across backend VMs.
Three distribution modes are available:

| Mode                                | Tuple   | Components                                                         |
| ----------------------------------- | ------- | ------------------------------------------------------------------ |
| Default                             | 5-tuple | Source IP, Source Port, Destination IP, Destination Port, Protocol |
| Session Persistence (IP Affinity)   | 2-tuple | Source IP, Destination IP                                          |
| Session Persistence (IP + Protocol) | 3-tuple | Source IP, Destination IP, Protocol                                |

#### Troubleshoot load balancing

Key Points

- **5-tuple (default)** — best for even traffic distribution; source port adds entropy so
  connections are spread across all backend VMs
- **2-tuple / 3-tuple** — pins a client to the same backend VM across sessions; useful when
  server-side session state can't be shared, but risks uneven distribution
- A health probe monitors backend VM availability but does **not** affect distribution mode
- Upgrading SKU does not change distribution behaviour

Common Exam Trap

> "Intermittent timeouts + uneven distribution" → fix with **5-tuple hash**, not session persistence.
> Session persistence (IP affinity) _worsens_ uneven distribution by locking clients to specific VMs

#### Common Ports

| Port | Protocol | Service           |
| ---- | -------- | ----------------- |
| 20   | TCP      | FTP (Data)        |
| 21   | TCP      | FTP (Control)     |
| 22   | TCP      | SSH               |
| 23   | TCP      | Telnet            |
| 25   | TCP      | SMTP              |
| 53   | TCP/UDP  | DNS               |
| 67   | UDP      | DHCP (Server)     |
| 68   | UDP      | DHCP (Client)     |
| 80   | TCP      | HTTP              |
| 110  | TCP      | POP3              |
| 143  | TCP      | IMAP              |
| 161  | UDP      | SNMP              |
| 162  | UDP      | SNMP Trap         |
| 389  | TCP/UDP  | LDAP              |
| 443  | TCP      | HTTPS             |
| 445  | TCP      | SMB               |
| 465  | TCP      | SMTPS             |
| 587  | TCP      | SMTP (Submission) |
| 636  | TCP      | LDAPS             |
| 993  | TCP      | IMAPS             |
| 995  | TCP      | POP3S             |
| 1433 | TCP      | SQL Server        |
| 1723 | TCP      | PPTP VPN          |
| 3306 | TCP      | MySQL             |
| 3389 | TCP      | RDP               |
| 5985 | TCP      | WinRM (HTTP)      |
| 5986 | TCP      | WinRM (HTTPS)     |
| 8080 | TCP      | HTTP Alternate    |
| 8443 | TCP      | HTTPS Alternate   |
