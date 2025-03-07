## Azure Network Engineer (AZ-700)

**I'm going to also publicly document my notes for the Azure Network Engineer certification (AZ-700) exam. I'm currently applying for cloud engineer roles so I think this will augment my skills in the meantime. I definitely want to take this exam but I'll probably do that after I actually get the job.**


## 03.07.2025
**Today's Topic**
* Introduction to Azure Virtual Networks

________________________
Configure public IP services

A dynamic PIP is allocated when you create or start a VM. It's released when you stop or delete the VM. In each Azure region, PIP addresses are assigned from a unique pool of addresses. By default the allocation is dynamic. 

PIPs have two SKU: Standard or Basic. 

Basic
- Static or dynamic
- Idle Timeout: inbound traffic has a idle timeout of 4 - 30 minutes. Outbound traffic has 4 minutes
- Security: open by default. NSGs are recommended by optinal. 
- No availability zones
- No routing preferences
- No global tier

Standard
- Static
- Idle timeout: same as Basic
- Secure by default. Allow traffic with NSG is required. 
- Yes to AZ, routing preference and global tier. 


You can create a public IP address prefix and it will be specific to your region. 

You can also use custom IP address prefixes (I think this is the same as you buying an IP from the internet and using it here). 




________________________
VNets

All resources in a VNet can communicate outbound to the internet, by default. Use PIPs and LBs to allow external communication inbound to Azure resources. You can use these same resources for outbound communication but you might want to use a NAT Gateway or something instead.

Azure resources can have service endpoints to connect to other Azure resource types (go over this in detail because I'm only used to private endpoints). 

To communicate to on-prem resources, use a VPN gateway, S2S VPNs, and Azure ExpressRoute.

Use NSGs and network virtual appliances to filter traffic. 

Azure routes traffic between subnets, peered VNets, on-prem networks, and the Internet by default. You can make route tables and use BGP to override default the default routes Azure creates. 

You can only add RFC 1918 address spaces. You can not add: `224.X.X.X, 127.X.X.X, 169.254.X.X, 168.63.X.X`. Any of that goofy stuff

Azure reserves the first four and the last IP for a total of five IP addresses within each subnet. First IP is the network address. Second is reserved by Azure for the default gateway. Third and fourth are reserved by Azure to map the Azure DNS IPs to the VNet space. 

Considerations when implementing virtual networks: 
- Ensure nonoverlapping address spaces.
- Is any security isolation required?
- Do you need to mitigate any IP addressing limitations?
- Are there any connections between Azure VNets and on-prem networks?
- Is there any isolation required for administrative purposes?
- Are you using any Azure services that create their own VNets?

Smallest Azures subnet is /29 and the largest is a /2. 

Certain Azure services require their own subnet. 

![Image](AZ700-Intro-1.PNG)

A VNet must has a unique name in its resource group. Its naming convention has a resource group scope. So you can have the same VNet name in the same subscription in different groups but not in the same group. Obviously dont do that though. 
