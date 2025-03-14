## Azure Network Engineer (AZ-700)

**I'm going to also publicly document my notes for the Azure Network Engineer certification (AZ-700) exam. I'm currently applying for cloud engineer roles so I think this will augment my skills in the meantime. I definitely want to take this exam but I'll probably do that after I actually get the job.**

## 03.10.2025
**Today's Topic**
* Introduction to Azure Virtual Networks

________________________
Design name resolution for your virtual network

Long work day. Another long day.


## 03.09.2025
**Today's Topic**
* Introduction to Azure Virtual Networks

________________________
Design name resolution for your virtual network


## 03.08.2025
**Today's Topic**
* Introduction to Azure Virtual Networks

________________________
Design name resolution for your virtual network


DNS is split into two areas: Public and Private DNS. Lets start with Public. 

Public DNS services resolve names and IP addresses for resources and services accessible over the Internet. 

Azure DNS uses anycast networking. Each DNS query is directed to the closet avaiable DNS server. 

In Azure DNS, you can create address records manually within relevant zones by using the Host records (A/AAAA) and Alias records (CNAME). 

Azure DNS provides a reliable, secure DNS service to manage and resolve domain names in a virtual network without needing to add a custom DNS solution. 

(I definitely need to learn DNS because I'm not entirely sure what's happening right now.)

Quick refresher on DNS. Obviously DNS maps domain names (say, a website name) to an IP address. A records map a domain to an IPv4 address. AAAA records map a domain to an IPv6 address. CNAME records map a domain name to another domain name. MX is for email, TXT is for arbitrary text, and NS records define the authoritative name servers for a domain. 

Azure DNS is like the equivalent of GoDaddy for Cloudflare. It's a cloud-based DNS hosting service. You CAN NOT register domain names with Azure DNS (such as buy domain names). You'll have to get them (buy them) from a registrar like Namecheap or GoDaddy. Azure DNS also doesn't provide recursive DNS resolution (like Google's `8.8.8.8`). Instead, it hosts authoritative DNS records. 

In Azure, you create a DNS zone. Azure will provide you with four name servers that you update at your domain registrar where you bought your domain from. Then, you create an A record to map your domain to an IPv4 address. You then create your CNAME record to map your domain to your Azure public endpoint URL. 

Authoritative name server - think of it as the official keeper of records for a domain (website name). If someone queries (searches) for www.evcloudresumechallenge.com, Azure DNS responds with the correct IP address. Azure provides 4 authoritative name servers and Azure DNS handles all DNS records for it. An authoritative name server is responsible for answering all DNS queries about a specific domain. 

A recursive DNS resolver is like a middleman that finds the answer to a DNS query by asking different servers. Say if you type www.evcloudresumechallenge.com into a web browser. Your computer asks a recursive resolver (like Google's `8.8.8.8`). The recursive resolver will say I don't know Let me find out. Then is aska a Root DNS server "Who handles the .com domains?". The root server responds with a Top-Level Domain (TLD) server saying "Go ask the .com name servers.". The TLD server points to the Authoritative Name Server (which would be DNS in this case). The Authoritative Name Server responds "The IP for www.evcloudresumechallenge.com is X.X.X.x". Then your browser knows where to go. 

Your ISP provider can also give you a recursive DNS resolver. Google is one. Cloudflare has one (`1.1.1.1`). 

A DNS zone is like a folder that holds all the DNS records for a domain. It contains the A record, CNAME record, and other records. Think of a zone like a container for your domain. That's it. 

Other common recursive resolvers: Comcast `75.75.75.75` and AT&T `68.94.156.1`. There are 13 sets of root DNS servers, operated by different organizations worldwide. The root server doesn't know any IP addresses but it knows where to find domains like `.com` and `.org`. The root servers are managed by ICANN (Internet Corporation for Assigned Names and Numbers). Organizations liek Verisign, NASA, and the U.S. Department of Defense operate them. 

The Top-Level Domain (TLD) servers handle specific domains like `.com`, `.net`, and `.org`. They are run by Verisign and Public Interest Registry (PIR). Country-code TLDs like `.uk` and `.fr` are run but national organizations. They also don't have the IP address but they know the authoritative name servers. 

Remember, you're the owner of the authoritative name server. Azure provides 4 name servers for redundancy. 

Azure DNS = the entire DNS management system.

Azure DNS Zone = the folder holding the records for one specific domain. 


The name of the Azure DNS zone must be unique wihin the resource group. 

Microsoft has a whole section that goes over delegating DNS domains. Now, I just asked Chat GPT a bunch of questions on how all of this works so I know how it works but I may forget. Well, maybe I should type it out. 

Okay. You deploy your Azure DNS service (remember, this is the overall services for all DNS). You create an Azure DNS zone for your specific domain name. Azure automatically deploys 4 authoritative name servers for your zone. Remember, Azure DNS is not the domain registrar. You have to go to your domain registrar (like Cloudflare) and change your NS records to point to the 4 Azure authoritative servers that were just created. Once the name Servers are assigned, Azure DNS automatically creates authoritative NS recods in your zone. 

You can also set up child zones (for example, contoso.com is the parent and partners.contoso.com would be a child). You can create an Azure DNS zone for the child but the NS records must be created in the parent zone rather than the domain registrar. So I'm assuming how you go to Cloudflare to list the new NS records, you would just go to the Azure DNS parent zone and list the NS records for the child zone here. I 

An Azure DNS record set is a collection of records in a zone that have the same name and are the same type. A record set can't contain two idential records. 

Lets talk Private DNS servers

When resources in Azure need to resolve domain names to internal IP addresses, they can use: Azure DNS Private Zones, Azure-provided name resolution, and name resolution that uses your own DNS server. 

DNS servers within a VNet can forward DNS queries to the recursive resolvers in Azure. Forwarding queries allows VMs to see both your on-prem resources and Azure-provided host names.

Access to the recursive resolvers in Azure is provided by the virtual IP 168.63.129.16. 

DNS forwarding also enables DNS resolution between virtual networks and allows your on-prem machines to resolve to Azure-provided host names. In order to resolve a VM's host name, the DNS server VM must reside int he same VNet and be configured to forward host name queries to Azure. Because the DNS suffix is different in each VNet, you can use conditional forwarding rules to send DNS queries to the correct Vnet for resolution.

^^^ I'll need to ask Chat GPT exactly what this means. I'm running out low on my question limit for the day. 

Azure provided DNS is its own free default internal DNS. This is the DNS you see when you have like `myLinuxVM.compute.azure.com`. The main issues are you can't control the DNS zone names or the life cycle of the DNS records since Azure handles all of it. 

Private DNS zones in Azure are avaiable to internal resources only and are global in scope (applies to all regions, subscriptions, RGs, etc). This resource is called Private DNS zones in the portal. 

At the VNet level, default DNS configuration is part of the DHCP assignments made by Azure, specifying the special address `168.63.129.16` to use Azure DNS services. You can override the default configuration by configuring an alternate DNS server at the VM NIC. 

Two days to link VNets to private zones: 1.) Each VNet can link to one private DNS zone for registration with up to 100 Vnets linked to the same private DNS zone. 2.) You can link a VNet to each private zone with each VNet being able to link to up to 1,000 private DNS zones. 

Make sure you understand this conditional forwarding stuff. Use the lab to finish off this section. 






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
