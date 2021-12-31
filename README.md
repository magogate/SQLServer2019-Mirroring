# SQLServer2019-Mirroring
How to enable mirroring in SQL Server

# Intruduction
We are assuming that we already have SQL 2019 Cluster (Active / Passive) environment setup - between Node1 & Node2. However, what if both nodes goes down? or something goes wrong with SAN device?
In such cases, considering Disaster Recovery (DR), it's ideal to have another environment setup (Preferably in Cloud).

## 1. Installation of Windows Server 2019 Server as Mirror Server (On-Premise)
We are going to have 4th Virtual Machine - first 3 are - [gogate-dc-1]:Domain Controller/SAN, [gogate-node-1]:Primary Node, [gogate-node-2]:Secondary Node - as Mirror and we will add that in same Network with other 3 VMs. 
1. Install Windows Server 2019 by creating a Virtual Machine
2. Change machine name 
3. Allocate IP Address 
   #### Computer Name 
        1. This PC --> Properties --> Advanced System Settings --> Computer Name (gogate-mirror-1)
   #### IP Addresses
        1. Control Panel --> Network & Internet --> Network and Sharing Center --> Ethernet0 --> Properties --> Internet Protocol Verstion 4 --> (TCP/IPv4)
           - Static IP Address : 192.168.80.50 (https://www.paessler.com/it-explained/ip-address) & (https://www.rapidtables.com/convert/number/binary-to-decimal.html)
           - Subnet Mask : 255.255.255.0 (https://www.paessler.com/it-explained/ip-address)
           - Default Gateway : 192.168.80.2 [Same for all VMs] (https://en.wikipedia.org/wiki/Default_gateway)
           - Preferred DNS Server : 192.168.80.10 [Same for all VMs] (https://www.cloudflare.com/learning/dns/what-is-a-dns-server/)
           - Alternet DNS Server : Keep blank
   #### Disable Firewall
        1. Control Panel --> System and Security --> Windows Defender Firewall --> Turn Off Windows Defender Firewall
        
## 2. Adding (gogate-mirror-1) to Domain
1. Validate if you can ping to Domain from both the nodes - ping gogates.local
2. Assign domain name for both nodes 
   - This PC --> Properties --> Advanced System Settings --> Member Of Domain - gogates.local
   - Specify credentials for Domain Admin - gogates\Administrator & P@ssword#123
   - Restart server
   - Follow same steps for both nodes
   - While logging you should be able to login as Domain Administrator user instead of local Administrator
   - Validate nodes in Domain Controller using "Active Directory Users and Computers" 
