# How to create a VPN between Azure and AWS using only managed solutions

What if you can stablish a connection between Azure and AWS using only managed solutions instead to have to use virtual machines? Did you know that since the beginning of 2019 you could do this?

Yes, you can! This is because since [February/2019](https://aws.amazon.com/about-aws/whats-new/2019/02/aws-site-to-site-vpn-now-supports-ikev2/)  AWS started to support IKEv2 allowing them to work both as initiator and responder mode, like Azure does. 

That said, if before you had to use an appliance or virtual machine acting as VPN Server on the other side when using the AWS Virtual Private Gateway, now you don't need anymore. You can simply connect the AWS Virtual Private Gateway with the Azure VPN Gateway directly without worry to manage IaaS resources like virtual machines.

So in this article I'll show to you how to setup this. Below the draw of our lab:

![draw](images/draw.png)

## Configuring Azure 

### 1. Create a resource group on Azure to deploy the  resources on that:

![newrg](images/newrg.png)

![create](images/create.png)

Choose the subscription, the name and the region to be deployed:

![creating](images/creating.png)

### 2. Create a Virtual Network and a subnet

![createvnet](images/createvnet.png)

![createvnetbutton](images/createvnetbutton.png)

Define the subscription, resource group, name and region to be deployed:

![vnetdefinitions](images/vnetdefinitions.png)

Set the address space for the virtual network and for the subnet. 
Here I'm defining the virtual network address space to **172.10.0.0/16**, changing the "default" subnet name to **"subnet-01"** and defining the subnet address range to **172.10.1.0/24**:

![vnetaddr](images/vnetaddr.png)

![vnetvalidation](images/vnetvalidation.png)

	
### 3. Create the VPN Gateway

The Azure VPN Gateway is a resource composed of 2 or more VM's that are deployed to a specific subnet called Gateway Subnet where the recommendation is to use a /27. He contain routing tables and run specific gateway services. Note that you can't access those VM's.

 To create, go to your Resource Group, then click to **+ Add**
 
 ![addvpngw](images/addvpngw.PNG)
 
 ![newvpngw](images/newvpngw.png)
 
 ![createvpngw](images/createvpngw.png)
 
 Then fill the fields like below:
 
 ![vpngwsummary](images/vpngwsummary.png)
 
 After click to Review + create, in a few minutes the Virtual Network Gateway will be ready:
 
 ![vpnready](images/vpnready.png)
 
 ## Configuring AWS
 
 ### 4. Create the Virtual Private Cloud (VPC)
 
 ![createvpc](images/createvpc.png)
 
 ### 5. Create a subnet inside the VPC (Virtual Network)
 
 ![createsubnetvpc](images/createsubnetvpc.png)
 
 ### 6. Ceate a customer gateway pointing to the public ip address of Azure VPN Gateway
 
 The Customer Gateway is an AWS resource with information to AWS about the customer gateway device, which in this case is the Azure VPN Gateway.
 
 ![createcustomergw](images/createcustomergw.png)
 
 ### 7. Create the virtual private gateway then attach to the VPC
 
 ![createvpg](images/createvpg.png)
 
 ![attachvpgtovpc](images/attachvpgtovpc.png)
 
 ![attachvpgtovpc2](images/attachvpgtovpc2.png)
 
 ### 8. Create a site-to-site VPN Connection
 
 ![createvpnconnection](images/createvpnconnection.png)
 
 Set the routing as static pointing to the azure subnet-01 prefix (172.10.1.0/24)
 
 ![setstaticroute](images/setstaticroute.png)
 
 After fill the options, click to create.
 
 ### 9. Download the configuration file
 
 Please note that you need to change the Vendor, Platform and Software to Generic once Azure isn't a valid option:
 
 ![downloadconfig](images/downloadconfig.png)
 
 In this configuration file you will note that there are the Shared Keys and the Public Ip Address for each of one of the two IPSec tunnels created by AWS:
 
 ![ipsec1](images/ipsec1.png)
  
 ![ipsec1config](images/ipsec1config.png)
  
 ![ipsec2](images/ipsec2.png)
   
 ![ipsec2config](images/ipsec2config.png)
 
 After the creation, you should have something like this:
 
 ![awsvpnconfig](images/awsvpnconfig.png)
 
 
 
 
 
 
 
 
 
 
 
 
 
