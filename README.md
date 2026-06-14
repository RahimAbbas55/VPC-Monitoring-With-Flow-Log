<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Monitoring with Flow Logs

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-monitoring)

**Author:** irahimsindhu@gmail.com  
**Email:** irahimsindhu@gmail.com

---

## VPC Monitoring with Flow Logs

![Image](http://learn.nextwork.org/affectionate_olive_majestic_marjoram/uploads/aws-networks-monitoring_3e1e79a1)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC is a virtual private network within AWS that allows you to launch and manage resources in an isolated environment. It is useful because it gives you control over networking, security, routing, and connectivity for your cloud resources.

### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to set up the networking environment for my EC2 instances and configured CloudWatch Logs to collect and monitor system and application logs for troubleshooting and analysis.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was the simplicity and the step -by-step approach in achieving the task.

### This project took me...

This project took me 35 minutes to complete.

---

## In the first part of my project...

### Step 1 - Set up VPCs

In this step, I will create 2 separate VPCs from scratch.

### Step 2 - Launch EC2 instances

In this step, I will launch 1 EC2 instance in both of my VPCs.

### Step 3 - Set up Logs

In this step, I will be doing the following:
- Set up a way to track all inbound and outbound network traffic.
- Set up a space that stores all of these records.

### Step 4 - Set IAM permissions for Logs

In this step, I will achieve the following:
- Give VPC Flow Logs the permission to write logs and send them to CloudWatch.
- Finish setting up your subnet's flow log.

---

## Multi-VPC Architecture

I started my project by launching 2 separate VPCs each with 1 availability zone, 1 public subnet.

The CIDR blocks for VPCs 1 and 2 are 10.1.0.0/16 and 10.2.0.0/16 respectively. They have to be unique because we want to minimize the risk of IP address conflicts once the VPCs are set up and ready to be used.

### I also launched EC2 instances in each subnet

My EC2 instances' security groups allow all ICMP requests. This is because ICMP traffic is required for network diagnostics and troubleshooting, such as ping and path MTU discovery, enabling administrators to verify connectivity and identify network issues.

![Image](http://learn.nextwork.org/affectionate_olive_majestic_marjoram/uploads/aws-networks-monitoring_e7fa8775)

---

## Logs

Logs are like a diary for your computer systems. They record everything that happens, from users logging in to errors popping up. It's the go-to place to understand what's going on with your systems, troubleshoot problems, and keep an eye on who’s doing what.

Log groups are big folder in AWS where you keep related logs together. Usually, logs from the same source or application will go into the same log group, BUT logs are also region-specific. This means log data gets created and saved in the region it was created, although you can use CloudWatch dashboards to bring together logs from different regions.

### I also set up a flow log for VPC 1

![Image](http://learn.nextwork.org/affectionate_olive_majestic_marjoram/uploads/aws-networks-monitoring_e8398869)

---

## IAM Policy and Roles

I created an IAM policy because the application required permissions to create log streams and write log events to a specific CloudWatch Logs log group.

I also created an IAM role and attached the IAM policy to it so that the EC2 instance could write logs to the CloudWatch Logs log group while adhering to AWS security best practices.

A custom trust policy is a policy that is used to very narrowly define who can use a role.

![Image](http://learn.nextwork.org/affectionate_olive_majestic_marjoram/uploads/aws-networks-monitoring_4334d777)

---

## In the second part of my project...

### Step 5 - Ping testing and troubleshooting

In this step, I will get my instance 1 to send test messages to instance 2.

### Step 6 - Set up a peering connection

In this step, I will set up a connection link between your VPCs.

### Step 7 - Analyze flow logs

In this step, I will demonstrate the following:
- Review the flow logs recorded aboout VPC 1's public subnet.
- Analyse the flow logs to get some tasty insights.

---

## Connectivity troubleshooting

My first ping test between my EC2 instances had no replies, which means that there is no rule that allows my instance 2 to allow the inbound traffic to reach it.

![Image](http://learn.nextwork.org/affectionate_olive_majestic_marjoram/uploads/aws-networks-monitoring_99d4ba42)

I could receive ping replies if I ran the ping test using the other instance's public IP address, which means that instance 2 is correctly configured to respond to ping requests and instance 1 can communicate with instance 2 if the traffic goes across the public channel of the internet.

---

## Connectivity troubleshooting

Looking at VPC 1's route table, I identified that the ping test with Instance 2's private address failed because there was no route directing traffic to the CIDR block of VPC 2 through the VPC peering connection. As a result, traffic destined for Instance 2 could not be routed correctly.

### To solve this, I set up a peering connection between my VPCs

I also updated both VPCs' route tables to include routes through the VPC peering connection, enabling private communication between the two VPCs without traversing the public internet.

![Image](http://learn.nextwork.org/affectionate_olive_majestic_marjoram/uploads/aws-networks-monitoring_7316a13d)

---

## Connectivity troubleshooting

I received ping replies from Instance 2's private IP address! This means that a peering connection between the private resources of the VPC instances has been setup successfully and both VPCs can communicate with each other over without relying on the public internet.

![Image](http://learn.nextwork.org/affectionate_olive_majestic_marjoram/uploads/aws-networks-monitoring_4ec7821f)

---

## Analyzing flow logs

Flow Logs provide information about network traffic entering and leaving resources, including source and destination IPs, ports, protocols, and whether the traffic was accepted or rejected.

For example, the flow log I've captured tells us that the packet sent was rejected.

![Image](http://learn.nextwork.org/affectionate_olive_majestic_marjoram/uploads/aws-networks-monitoring_d116818e)

---

## Logs Insights

Logs Insights is a CloudWatch feature that lets you query, analyze, and visualize log data to troubleshoot issues and gain operational insights.

I ran the query "Top 10 byte transfers by source and destination IP addresses". This query analyzes all about discovering the top 10 biggest data transfers between IP addresses in your network! You'll find out which resources are moving the most data, which uncovers the busiest traffic routes in your network.

![Image](http://learn.nextwork.org/affectionate_olive_majestic_marjoram/uploads/aws-networks-monitoring_3e1e79a1)

---

---
