---
title: "EC2 (Private Subnet) — Windows + SQL Server"
weight: 5
chapter: false
pre : " <b> 5.5. </b> "
---

Overview

This lab covers launching and administering a Windows EC2 instance running SQL Server Express in a private subnet. It focuses on secure, least-privilege administration using AWS Systems Manager (SSM), networking considerations (private subnets, route tables, NAT, VPC endpoints), and backup (EBS snapshots). The lab demonstrates how application Lambda functions or internal services can access the database via private networking while avoiding public exposure.

Prerequisites

- AWS account with permission to create VPCs, EC2, IAM roles, Systems Manager, and EBS snapshots.
- Basic familiarity with AWS Console or AWS CLI.
- An existing VPC for the lab or permission to create one.

Learning objectives

- Launch a Windows EC2 instance into a private subnet with an IAM role for SSM.
- Install and configure SQL Server Express on the instance.
- Administer the instance securely via AWS Systems Manager (Session Manager / Run Command) without opening RDP to the internet.
- Configure security groups and routing so that backend Lambdas or internal hosts can reach the database while it remains private.
- Take EBS snapshots for backups and restore verification.

Lab steps (high level)

1. Prepare network
   - Verify or create a VPC with public and private subnets, proper route tables, and NAT or egress strategy for private instances.
   - (Optional) Add VPC endpoints for S3/SSM to reduce egress if needed.
2. Create an IAM role for EC2
   - Create an instance profile role granting `AmazonSSMManagedInstanceCore` and minimal additional permissions (EBS snapshot if needed).
3. Launch EC2 (Windows) in the private subnet
   - Choose a Windows AMI and attach the instance profile.
   - Use a security group that allows only necessary inbound traffic from internal CIDR ranges (e.g., database port 1433 from private app subnets).
4. Install SQL Server Express
   - Use SSM `Run Command` to download and install SQL Server Express silently, or connect via SSM Session Manager to run setup interactively.
   - Configure SQL Server to listen on the private IP and enable SQL authentication or Windows auth as the lab requires.
5. Validate connectivity from internal clients
   - From a Lambda or from a bastion host in the same VPC, test a TCP connection to the SQL Server port and run a test query.
6. Administration via SSM
   - Use Session Manager for interactive admin or Port Forwarding (Session Manager plugin) to avoid RDP exposure.
   - Use SSM Run Command for automation (patching, install, config).
7. Backup & snapshots
   - Create an EBS snapshot of the volume after initial setup; demonstrate restore to a new volume/instance.
8. Cleanup
   - Terminate the EC2 instance, delete snapshots and IAM role, and remove any temporary resources.

Testing & troubleshooting

- If SSM does not connect, verify the instance has the SSM agent installed and the instance role includes `AmazonSSMManagedInstanceCore`.
- Check Security Group rules: ensure SQL port is open only to internal subnets and not 0.0.0.0/0.
- If instance needs OS updates but no outbound internet, ensure NAT or SSM + VPC endpoints are configured for patching.
- Inspect CloudWatch (SSM / EC2) logs for install/run errors.

Notes & best practices

- Prefer SSM Session Manager over opening RDP to the internet; consider SSM Port Forwarding for RDP when GUI is required.
- Use least-privilege IAM roles for EC2 and Lambda access to the database.
- Use VPC endpoints for S3/SSM to reduce public egress and improve security.
- Automate backups with scheduled EBS snapshots and consider storing DB backups in S3.

References

- AWS Systems Manager Session Manager
- Amazon EC2 Windows AMIs
- SQL Server Express installation documentation
