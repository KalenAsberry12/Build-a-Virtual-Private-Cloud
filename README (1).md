<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a Virtual Private Cloud

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-vpc)

**Author:** Kalen Asberry  
**Email:** kalenasberry52@gmail.com

---

## Build a Virtual Private Cloud (VPC)

![Image](http://learn.nextwork.org/secure_red_beautiful_frog/uploads/aws-networks-vpc_2facf927)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) is a secure, isolated network within AWS where you can design and control how your resources communicate. It is useful because it gives you flexibility to build scalable, secure, and highly available architectures while controlling access, routing, and connectivity to the internet or private networks.

### How I used Amazon VPC in this project

In today’s project, I used Amazon VPC to create a custom network from scratch. I:

Defined an IPv4 CIDR block for the VPC.

Built a public subnet for internet-facing resources.

Enabled auto-assign public IPv4 addresses.

Created and attached an Internet Gateway so instances in the subnet could communicate with the internet.

This hands-on project helped me practice foundational cloud networking skills used in real-world deployments.

### One thing I didn't expect in this project was...

One thing I didn’t expect in this project was how easy it is to misconfigure CIDR blocks and subnet ranges. I realized that subnet CIDR blocks must be completely contained within the VPC CIDR, and small errors can prevent resources from launching correctly. This taught me the importance of attention to detail in network design.

### This project took me...

This project took me about 90 minutes from start to finish, including testing configurations in both the AWS Console and CloudShell CLI.

---

## Virtual Private Clouds (VPCs)

VPCs (Virtual Private Clouds) are private, isolated networks inside AWS that you design and control. They let you decide how your resources (like servers, databases, and apps) are connected, secured, and accessed.

Without VPCs, every AWS resource would sit in one giant, open cloud with no privacy—like a country without cities or a Google Drive with every user’s files in the same folder. With VPCs, you get:

🔒 Privacy & Security – control who can access your resources.

🗂️ Organization – group resources into subnets for better management.

🌐 Control – decide which resources can connect to the internet and which stay private.

In short: VPCs are the foundation that make cloud networking secure, organized, and scalable.

There was already a default VPC in my account ever since my AWS account was created. This is because AWS automatically provides a default VPC in each region so you can launch resources right away without needing to build networking from scratch.

The default VPC makes it easy for beginners (or anyone starting quickly) to run services like EC2 instances or databases with internet connectivity on Day 1. Without it, you’d first have to learn how to create and configure your own VPC before using many core services.

Over time, as requirements get more advanced (like stricter security, private subnets, or custom routing), you can design and use your own custom VPCs instead of relying on the default one.

To set up my VPC, I had to define an IPv4 CIDR block, which is a way of assigning a range of IP addresses that all resources inside the VPC can use.

🌍 IP Address → Think of it as a unique street address for each resource in your network. Without an IP address, resources wouldn’t know how to find and talk to each other.

🔢 IPv4 → Internet Protocol version 4, the most common system for IPs, written as four numbers separated by dots (e.g., 192.168.0.1). There are over 4 billion possible IPv4 addresses.

📦 CIDR Block → Instead of assigning IPs one by one, CIDR (Classless Inter-Domain Routing) lets you reserve a whole block of addresses for your VPC.

For example:

10.0.0.0/16 → gives you 65,536 possible addresses (10.0.0.0 → 10.0.255.255).

10.0.0.0/24 → gives you just 256 possible addresses (10.0.0.0 → 10.0.0.255).

The number after the slash (/) tells you how many bits are fixed. The smaller the number, the bigger the range of available IP addresses.


![Image](http://learn.nextwork.org/secure_red_beautiful_frog/uploads/aws-networks-vpc_2facf927)

---

## Subnets

Subnets are smaller sections of a VPC’s IP address range that let you organize and control where resources live in your network. If your VPC is like a city, subnets are the neighborhoods inside that city.

Public Subnets → host resources that need internet access (like web servers).

Private Subnets → host resources that should stay isolated (like databases).

Every subnet in a VPC must have its own unique CIDR block (no overlapping ranges).

There are already subnets in my account because the default VPC automatically creates one subnet per Availability Zone (AZ) in a region.

💡 Availability Zones (AZs) are clusters of AWS data centers in a region. Each subnet must live in exactly one AZ. Spreading subnets across AZs makes your setup more reliable, since if one AZ fails, resources in another can keep running.

Once I created my subnet, I enabled auto-assign public IPv4 addresses. This setting makes sure that any EC2 instance launched in this subnet automatically gets a public IP address, so it can send and receive traffic from the internet.

By default, resources only get private IP addresses, which allow internal communication inside the VPC but not internet access. Enabling auto-assign saves time and effort because I don’t need to manually assign a public IP to each instance.

In short: this setting ensures my internet-facing resources are always reachable from outside the VPC.

The difference between public and private subnets is whether or not they can connect to the internet.

🌐 Public Subnet → Resources inside can communicate with the internet (e.g., a web server).

🔐 Private Subnet → Resources stay internal and cannot directly reach the internet (e.g., a database).

Right now, even though my subnet is labeled “Public,” it’s not truly a public subnet yet. For a subnet to be considered public, it must:

Be connected to an Internet Gateway (IGW).

Have a route in its route table that directs traffic to the IGW.

Until I attach an internet gateway and update the routing, this subnet is functionally private.

![Image](http://learn.nextwork.org/secure_red_beautiful_frog/uploads/aws-networks-vpc_157c4219)

---

## Internet gateways

Internet gateways are the connection point between your VPC and the internet.

Think of your VPC as a private city: resources inside can talk to each other, but without an internet gateway, there’s no road leading in or out. By attaching an internet gateway, you create that bridge so:

🚀 Instances in your public subnet can access the internet (e.g., download software updates).

🌐 External users can reach your resources (e.g., connect to a web server).

Internet gateways are essential when you want to make applications available to users outside your private VPC. Without one, your VPC remains fully isolated from the internet.

Attaching an internet gateway to a VPC means that resources in your VPC can send and receive traffic from the internet. For example, EC2 instances with public IP addresses can now connect to external websites and also be accessed by users outside your VPC.

If I missed this step, my VPC would remain completely isolated—instances could only communicate with each other internally, but they wouldn’t be able to reach the internet or serve external users.

In short: the internet gateway is what turns a private network into one that can host internet-facing applications.

![Image](http://learn.nextwork.org/secure_red_beautiful_frog/uploads/aws-networks-vpc_4ae90410)

---

## Using the AWS CLI

VPC resources could also be created with CloudShell, which is a browser-based shell built right into the AWS Management Console. It gives you a preconfigured environment where you can run code and commands without needing to install anything locally. The best part is that AWS CloudShell already comes with AWS CLI pre-installed.

CLI (Command Line Interface) is a tool that lets you create, delete, and manage AWS resources using text commands instead of clicking through the console. With the CLI, you can automate tasks, run scripts, and manage cloud infrastructure more efficiently.

To set up a VPC or a subnet with the CLI/Console, you must pass a valid IPv4 CIDR block (e.g., 10.0.0.0/16, 10.0.0.0/24). If you’re seeing errors, check:

CIDR syntax & range

Format must be x.x.x.x/nn (four numbers 0–255 separated by dots).

Use private ranges only: 10.0.0.0/8, 172.16.0.0/12, or 192.168.0.0/16.

Subnet must be inside the VPC

The subnet CIDR must be fully contained within the VPC CIDR and not overlap any other subnet.

The subnet’s prefix must be larger (more specific) than the VPC’s.
Examples:

VPC 10.0.0.0/16 → subnet like 10.0.1.0/24 or 10.0.0.0/20 (✅)

Subnet 10.0.0.0/12 in a /16 VPC (❌ too big).

Supported sizes

AWS VPC/subnet masks must be between /16 and /28. Trying /29 or /32 for a subnet will fail.

Availability Zone & Region

When creating a subnet, pick an AZ that exists in the current Region. AZ typos or mismatches cause errors.




Compared to using the AWS Console, an advantage of using commands in CloudShell is that it’s faster and more repeatable. I can run a single command or script to set up resources like a VPC, subnet, and internet gateway, which saves time and reduces the chance of clicking the wrong option. This also makes automation possible, which is a big advantage for real-world cloud engineering.

An advantage of using the Console is that it’s visual and beginner-friendly. I can see diagrams, menus, and settings clearly, which makes it easier to understand what’s happening, especially when learning or troubleshooting.

Overall, I preferred using CloudShell for its efficiency and automation potential, but I see the value of the Console when I need a clear visual guide or when explaining networking concepts to others.

![Image](http://learn.nextwork.org/secure_red_beautiful_frog/uploads/aws-networks-vpc_9b2465411)

---

---
