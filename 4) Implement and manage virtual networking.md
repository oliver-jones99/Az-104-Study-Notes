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

Azure DNS Zone core concept:  Azure hosting your DNS records for you — instead of managing a DNS server yourself.  

Public DNS Zones: Used for internet facing domains, you create a public DNS zone and point your domain registrar's nameservers at Azure's.  

Private DNS Zone: Used for internal name resolution within Azure. Not accessible from the internet at all — only VNets you explicitly link to the zone can use it. 

Azure Private DNS Resolver: It's a managed service that acts as a DNS proxy for hybrid scenarios — specifically when your on-premises network needs to resolve Azure private DNS zones, or when Azure VMs need to resolve on-premises DNS names. It has inbound and outbound endpoints that bridge the two worlds.  

| | Public DNS Zone | Private DNS Zone | DNS Private Resolver |
|---|---|---|---|
| **Purpose** | Host DNS records for internet-facing domains | Internal name resolution within Azure | Bridge between on-premises DNS and Azure private DNS |
| **Who can query it** | Anyone on the internet | Only linked VNets | On-premises networks and Azure VNets (hybrid) |
| **Requires VNet link** | No | Yes — mandatory | Yes — inbound/outbound endpoints attached to subnets |
| **Auto VM registration** | No | Yes — optional tick box when linking | No |
| **DNSSEC** | Yes | No | No |
| **Managed nameservers** | Yes — Azure provides ns1-xx through ns4-xx | N/A — not internet accessible | N/A |
| **Registrar change needed** | Yes — point your domain registrar at Azure's nameservers | No | No |
| **Typical use case** | `contoso.com` public website | `vm1.corp.internal` private VM resolution | On-prem servers resolving Azure private zones |
| **Internet accessible** | Yes | No | No — internal proxy only |
| **Replaces custom DNS server** | Yes — for public domains | Yes — for internal Azure resolution | Partially — for hybrid forwarding scenarios |

#### Configure an internal or public load balancer

#### Troubleshoot load balancing
