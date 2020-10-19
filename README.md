# How to create a VPN between Azure and AWS using only managed solutions

What if you can stablish a connection between Azure and AWS using their managed solutions instead to have to use virtual machines? Did you know that since the beginning of 2019 you could do this?

Yes, you can! This is because since [February/2019](https://aws.amazon.com/about-aws/whats-new/2019/02/aws-site-to-site-vpn-now-supports-ikev2/)  AWS started to support IKEv2 allowing them to work both as initiator and responder mode, like Azure does. 

That said, if before you had to use an appliance or virtual machine acting as VPN Server on the other side when using the AWS Virtual Private Gateway, now you don't need anymore. You can simply connect the AWS Virtual Private Gateway with the Azure VPN Gateway directly without worry to manage IaaS resources like virtual machines.

So in this article I'll show to you how to setup this. Below the draw of our lab:


![draw](images/draw.png)

## Configuring Azure 

1. Create a resource group on Azure to deploy the  resources on that:

![newrg](images/newrg.png)

![createrg](images/createrg.png)

Choose the subscription, the name and the region to be deployed:

![creatingrg](images/creatingrg.png)

2. Create a Virtual Network and a subnet
