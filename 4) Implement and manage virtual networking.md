# Configure and manage virtual networks in Azure
#### Create and configure virtual networks and subnets

#### Create and configure virtual network peering

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

#### Configure an internal or public load balancer

#### Troubleshoot load balancing
