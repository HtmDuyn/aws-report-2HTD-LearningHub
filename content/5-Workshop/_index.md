---
title: "Workshop"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Workshop — LearningHub (2HTD)


#### Overview

This workshop accompanies the 2HTD-LearningHub proposal and guides participants through hands-on deployment and testing of core components: frontend on Vercel, serverless backend (AWS Lambda + API Gateway), relational data on SQL Server hosted in EC2 (private subnet), and media stored in Amazon S3. The labs emphasize secure hybrid access, network design (VPC/subnets), EC2 administration via SSM, presigned S3 uploads, Lambda APIs, and Cognito authentication.

The workshop is organized as short labs that can run in a small test environment (single VPC) or an expanded environment (cloud VPC + on-prem simulation) to demonstrate different network flows and NAT/egress considerations.

### Learning objectives

- Understand the LearningHub hybrid architecture (Vercel + Lambda + EC2 + S3).
- Provision VPCs and subnets, and apply VPC endpoints / NAT design to control egress costs.
- Deploy EC2 Windows + SQL Server Express and manage it via AWS Systems Manager (SSM).
- Configure S3 bucket upload flows using presigned URLs from the frontend.
- Deploy Lambda (Node.js) and expose it via API Gateway (HTTP API).
- Configure Amazon Cognito for authentication and test protected APIs.

### Lab index

1. [Workshop overview](5.1-Workshop-overview/)
2. [Prerequisite & environment setup](5.2-Prerequiste/)
3. [S3 access from VPC (with VPC endpoints)](5.3-S3-vpc/)
4. [S3 access from On-prem simulation](5.4-S3-onprem/)
5. [VPC Endpoint Policies (optional)](5.5-Policy/)
6. [Cleanup](5.6-Cleanup/)

---

