---
title: "Proposal: Online Learning Platform — Practice, Exams & Live Classes"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## 1. Project Summary

This project builds an online platform for studying, practice, exam creation and live classes. The platform targets students and teachers and provides: a question bank (create/edit), practice exercises, mock exams, live classes (via Chime SDK), and progress reporting and analytics.

The architecture follows a serverless / hybrid approach aligned with the attached diagram to optimize cost, scalability and operational overhead. Static assets are served via CloudFront + S3, APIs and business logic are handled by API Gateway / Lambda / AppSync, data persisted in DynamoDB, and Cognito handles authentication and authorization.

Event-driven components (EventBridge + SNS) handle notifications (exam schedules, results), and CloudWatch collects logs and metrics. Amazon Chime SDK is used for live class sessions and recording; AppSync or API Gateway provides realtime/GraphQL needs.

Security considerations include network segmentation, VPC endpoints for DynamoDB, least-privilege IAM roles, and secure secrets storage (Secrets Manager / Parameter Store).

## 2. Problem Statement

- Students and teachers currently rely on multiple disjoint tools for studying, testing and live teaching; there is a need for an integrated platform.
- Administrators/teachers need easier and more controlled ways to create, review and distribute exam content.
- Performance and availability can suffer during peak times (mock exams, live classes).
- Lack of real-time notifications and reporting for students and teachers.
- Requirements for protecting exam content and user data (answer confidentiality, account security).

## 3. Solution & Architecture

The proposed solution uses the components reflected in the `images` folder:

- Edge: Route 53 + CloudFront (S3 origin) for static assets and CDN caching.
- Authentication: Amazon Cognito for user auth, sessions and optional MFA.
- API Layer: Amazon API Gateway (REST) and/or AWS AppSync (GraphQL); AWS Lambda for business logic.
- Data Layer: Amazon DynamoDB for users, exams, questions, results (use VPC Endpoints for additional security).
- Real-time / Live: Amazon Chime SDK for live classes, recordings and interactions.
- Events & Notifications: Amazon EventBridge + SNS for scheduled workflows and notifications.
- Observability: Amazon CloudWatch (logs, metrics, dashboards) and EventBridge for log routing.

### Architecture Diagram

![Architecture Diagram](/images/architecture_2HTD_LearningHub.png)

### AWS Services Used

- Amazon Route 53
- Amazon CloudFront
- Amazon S3
- Amazon Cognito
- Amazon API Gateway
- AWS AppSync (optional)
- AWS Lambda
- Amazon DynamoDB (VPC Endpoints)
- Amazon Chime SDK
- Amazon EventBridge
- Amazon SNS
- Amazon CloudWatch
- AWS IAM, AWS Secrets Manager / SSM Parameter Store

### Component Design

- Edge Layer: CloudFront + S3 for static content and global caching.
- Auth & Identity: Cognito User Pools + Identity Pools for access control and federated login.
- API & Business Logic: API Gateway / AppSync -> Lambda. Lambdas deployed in private subnets if internal resources are required, or use VPC endpoints for DynamoDB.
- Data Model: DynamoDB with partition key / sort key schema for exams, questions, user results; GSI for common queries.
- Live Class: Use Chime SDK components for session setup, recording and streaming to S3.
- Events: EventBridge rules to trigger SNS notifications and Lambda workflows (grading, reminders).
- Monitoring & Logging: CloudWatch Logs + Dashboards and alarms for errors, latency and concurrency.

## 4. Technical Implementation

1. IaC & Provisioning
   - Use Terraform / CloudFormation / AWS CDK to provision VPC, CloudFront, S3, Cognito, API Gateway/AppSync, Lambda, DynamoDB, EventBridge, SNS, CloudWatch and Chime resources.
2. CI/CD
   - Configure pipelines (GitHub Actions / CodePipeline) to build frontend artifacts -> deploy to S3/CloudFront; build & deploy Lambda packages or container images; apply IaC changes.
3. API & Data
   - Design GraphQL schema (if using AppSync) or REST endpoints; implement Lambda handlers for create/edit exams, submission, grading and reporting.
   - DynamoDB tables: exams, questions, user_submissions, results; TTL for transient data; backups enabled.
4. Live class
   - Integrate Chime SDK for meeting sessions, assign host roles to teachers, attendees to students; store recordings in S3.
5. Security
   - Least-privilege IAM roles, Cognito user pool configuration, encryption at rest (DynamoDB) and in transit (HTTPS), store secrets in Secrets Manager.
6. Observability & Testing
   - CloudWatch dashboards, structured logging, optional X-Ray tracing. Perform load testing focused on peak exam windows and live-class concurrency.

## 5. Timeline & Milestones

- Week 1-2: Detailed architecture, data model, IaC scaffold
- Week 3-4: Implement authentication, static site (S3 + CloudFront), basic API endpoints
- Week 5-6: Implement exam creation, question bank, submission & grading workflows
- Week 7: Integrate Chime SDK and test live class flows
- Week 8: Observability, load testing, security review, documentation and handover

## 6. Budget Estimation

Note: estimates are indicative — use AWS Pricing Calculator for precise numbers.

- S3 + CloudFront: $20–$150 / month (depends on traffic and egress)
- Cognito: $0–$50 / month (depends on active users)
- API Gateway / AppSync + Lambda: $50–$300 / month
- DynamoDB: $50–$500 / month (depends on read/write capacity / on-demand)
- Chime SDK: $50–$500+ (depends on meeting minutes & recordings storage)
- CloudWatch + Logs: $10–$200 / month
- SNS / EventBridge: $5–$50

Estimated total: $200–$1,500 / month (wide range to reflect different configurations).

## 7. Risk Assessment

- Cost spikes when live-class usage or downloads increase — mitigation: autoscaling, on-demand DynamoDB, cost alarms and budgets.
- Security risk (leaked answers or accounts) — mitigation: RBAC, session controls, encrypt data, regular security reviews and pen tests.
- Performance bottlenecks during peak exams — mitigation: caching via CloudFront, DynamoDB on-demand or provisioned throughput, provisioned concurrency for critical Lambdas.
- Chime/third-party dependency risk — mitigation: fallback workflows (delayed recording access), test alternative streaming options.

## 8. Expected Outcomes

- Integrated user experience: study, practice, mock exams and live classes on one platform.
- Reduced teacher effort for creating & managing exams — higher productivity.
- Ability to handle concurrent mock exams with high availability and low latency.
- Real-time notifications & reporting to improve learning outcomes through analytics.
- Cost control and the ability to scale cost-effectively as user base grows.

---

If you want, I can further refine the proposal with: table designs for DynamoDB, sample IAM policies, or a detailed cost breakdown using the AWS Pricing Calculator.
