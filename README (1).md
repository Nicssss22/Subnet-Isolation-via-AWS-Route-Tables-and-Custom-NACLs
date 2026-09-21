<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Creating a Private Subnet

**Project Link:** [View Project](https://nextwork.ai/projects/4e96e29d-98b8-551e-ae93-da39c36e30fd)

**Author:** agustinnico2302@gmail.com  
**Email:** agustinnico2302@gmail.com

---

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/4e96e29d-98b8-551e-ae93-da39c36e30fd_afe1fdbd)

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC provides a private, isolated network environment in the AWS Cloud where you can deploy and manage resources. Its flexibility lets you customize your cloud network by setting security rules, configuring routing policies, and organizing or scaling resources through subnets, along with many other features.

### How I used Amazon VPC in this project

In today’s project, I used Amazon VPC to create a private environment by creating a private subnet where I can launch resources that should only be accessible within the private network. Additionally, setting up a private route table ensures that my private subnet only interacts with internal resources in the VPC because there is no Internet Gateway attached and the route target remains local. Lastly, setting up a private network ACL explicitly denies all traffic in my private subnet, serving as another layer of security even when an Internet Gateway is not present in the routing table.

### One thing I didn't expect in this project was...

One thing I didn’t expect in this project was that creating a custom network ACL would deny all traffic by default. This is a useful and secure feature because it allows me to permit only the traffic that is necessary. AWS enforces a zero-trust baseline by starting with a highly secure and completely isolated virtual firewall.

### This project took me...

This project took me about one to two hours because I had already built the VPC foundation in previous projects. I was also more familiar with the terminology and concepts that took time to understand in my earlier work.

## Private vs Public Subnets

The difference between public and private subnets is that a public subnet has a route to an Internet Gateway, allowing it to interact with the public internet. On the other hand, a private subnet does not have this route. It mainly interacts with resources within the VPC, but it can indirectly send or receive data from the internet through services such as a NAT Gateway.

Having private subnets are useful because it protects sensitive resources such as database from from being directly accessible from the internet. Instead, the public subnet serves as the proxy so these resources can be indirectly accessible.

My private and public subnets cannot have the same CIDR block because it defeats the purpose of grouping related resources instead of mixing them together. Additionally, each subnet should have separate responsibilities. For instance, a public subnet should be public-facing and interact with the internet, while a private subnet should work in the backend, where it stores sensitive data that should not be directly accessible from the internet.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/4e96e29d-98b8-551e-ae93-da39c36e30fd_afe1fdbd)

## A dedicated route table

By default, my private subnet is associated with default route table that I defined during the last project wherein this route table will be the default route table across the VPC.

I had to set up a new route table because my private subnet is currently associated with the route table that has internet gateway attached to it, which it defeats the purpose that it shouldn't be accessible online or via internet. Instead, I setup a new route table dedicated for the private subnet so it can only interact with the internet resources in the VPC.

My private subnet's dedicated route table only has one inbound and one outbound rule that allows the destination of 10.0.0.0/16 and local as a target so that traffic can interact with the the internal resources of my subnet.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/4e96e29d-98b8-551e-ae93-da39c36e30fd_b4b904b5)

## A new network ACL

By default, my private subnet is associated with default network ACL of the VPC, it is the NACL created right after the VPC is created.

I set up a dedicated network ACL for my private subnet because the default Network ACL is allowing all the traffic into the subnet. Removing the internet gateway from the route rable isn't enough since attackers can exploit misconfigured private subnet's NACL to gain access to its resources. Creating a new NACL would implicitly deny all the access by default and allowing specific traffic if neccessary.

My new network ACL has two simple rules: it denies all inbound and outbound traffic in the subnet.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/4e96e29d-98b8-551e-ae93-da39c36e30fd_1ed2cb07)

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/4e96e29d-98b8-551e-ae93-da39c36e30fd)*
