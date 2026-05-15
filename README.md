<p align="center">
  <h1 align="center">FinOps Cost Intelligence Agent</h1>
  <p align="center">
    AI-powered AWS Cost Analysis & Optimization using Amazon Bedrock Multi-Agent Collaboration
  </p>
</p>

---

# Overview

FinOps Cost Intelligence Agent is an AI-powered AWS FinOps assistant that helps organisations analyse AWS spend, understand cost drivers, and identify optimization opportunities using natural language.

Built on Amazon Bedrock multi-agent collaboration, the solution combines AWS cost analytics, Trusted Advisor recommendations, and a web-based chatbot experience to provide actionable FinOps insights.

This project is **forked and enhanced from the AWS sample implementation**:

https://github.com/aws-solutions-library-samples/guidance-for-cost-analysis-and-optimization-with-amazon-bedrock-agents

---

# Solution Technology Stack

| Layer | Technology |
|------|------------|
| Frontend | AWS Amplify |
| Authentication | Amazon Cognito |
| AI Orchestration | Amazon Bedrock Agents |
| Foundation Model | Amazon Nova |
| Backend Execution | AWS Lambda |
| Cost Analytics | AWS Cost Explorer |
| Optimization Engine | AWS Trusted Advisor |
| Infrastructure as Code | AWS CloudFormation |

---

# Key Enhancements Over AWS Sample

This implementation includes several improvements over the original AWS sample.

## Cost Analysis Enhancements

- Improved Bedrock agent instructions for better orchestration and response quality
- Stronger action group validation to reduce incorrect interpretations
- Enhanced grouping logic for cost analysis queries
- Improved regional cost analysis using Availability Zone grouping
- Better response formatting and consistency
- OpenAPI schema improvements for clearer Bedrock reasoning
- Added explicit response interpretation guardrails

### Supported Cost Analysis Views

- AWS Service
- Linked Account
- Usage Type
- Availability Zone
- Billing Entity

---

## Cost Optimization Enhancements

- Improved cost optimization agent reasoning
- Better Trusted Advisor recommendation prioritisation
- Savings-based recommendation ranking
- Quick win identification
- Risk and effort classification guidance
- Improved resource-level recommendation lookup
- Better recommendation descriptions and Bedrock function guidance

---

# Architecture

## Reference Architecture

<img width="1369" height="862" alt="Reference Architecture" src="https://github.com/user-attachments/assets/548fd570-dbea-4ce6-a100-2a2c8ab870df" />

---

# Solution Architecture

The solution uses Amazon Bedrock multi-agent collaboration with specialist FinOps agents.

## Agent Architecture

| Agent | Role | Responsibilities | AWS Services Used |
|------|------|------------------|------------------|
| **Supervisor Agent** | Request Orchestrator | Receives user requests, determines intent, routes questions to the appropriate specialist agent, and coordinates multi-step responses. | Amazon Bedrock Agents |
| **Cost Insights Agent** | Spend Analysis Specialist | Analyses AWS historical spend, service-level cost drivers, linked account spend, usage type analysis, billing insights, and regional cost breakdowns. | AWS Cost Explorer, AWS Lambda, Amazon Bedrock |
| **Cost Optimization Agent** | Savings Specialist | Retrieves Trusted Advisor cost optimization recommendations, ranks savings opportunities, identifies quick wins, and shows affected resources. | AWS Trusted Advisor, AWS Lambda, Amazon Bedrock |

---

---

# Pre-Requisites

Before deployment, ensure the following services and permissions are available.

## AWS Services

- Amazon Bedrock enabled in your target AWS Region
- Access to Amazon Nova foundation models
- AWS Cost Explorer enabled
- AWS Trusted Advisor enabled
- Amazon Cognito access
- AWS Amplify access

---

## AWS Support Plan Requirement

Trusted Advisor cost optimization recommendations require one of the following:

- Support Plan => AWS Business Support

**Basic Support is not sufficient for full cost optimization recommendations.**

---

## Required IAM Permissions

The deployment role should have permissions for:

- Amazon Bedrock
- AWS Lambda
- IAM
- Amazon Cognito
- CloudFormation
- AWS Trusted Advisor
- AWS Cost Explorer
- AWS Amplify

---

# Deployment Steps

## Step 1 — Deploy Frontend

Deploy the provided frontend ZIP package using AWS Amplify.

### Steps

1. Open AWS Amplify Console
2. Select **Deploy without Git provider**
3. Upload the frontend ZIP package
4. Complete deployment

Amplify will generate a hosted application URL.

Example:

```text
https://main.xxxxxx.amplifyapp.com
```

---

## Step 2 — Deploy Backend Infrastructure

Deploy the CloudFormation template using console or using aws cli.

```text
deployment/cfn-finops-bedrock-multiagent-nova.yaml
```

Example:

```bash
aws cloudformation deploy \
  --template-file deployment/cfn-finops-bedrock-multiagent-nova.yaml \
  --stack-name finops-agent \
  --capabilities CAPABILITY_NAMED_IAM
```

During deployment:

- provide your email address
- Cognito user will be created automatically
- temporary password will be emailed

---

## Step 3 — Retrieve CloudFormation Outputs

After deployment, capture the following outputs:

| Output | Purpose |
|------|---------|
| Agent ID | Bedrock Agent identifier |
| Agent Alias ID | Bedrock Agent alias |
| Cognito User Pool ID | Authentication configuration |
| Cognito Identity Pool ID | Federated identity configuration |
| AWS Region | Frontend configuration |

---

## Step 4 — Configure Frontend

Update the frontend configuration using the CloudFormation outputs from Step 3.

Redeploy the frontend if required.

---

## Step 5 — Login

Login using:

- email address
- temporary password

---

# Sample Questions

## Cost Analysis

```text
What were my AWS costs last month?
```

```text
Show my AWS cost by linked account for April 2026.
```

```text
Which AWS services cost the most this month?
```

```text
Show AWS cost by region for the last 30 days.
```

```text
Show my AWS usage type breakdown for this month.
```

---

## Cost Optimization

```text
What are my current AWS cost saving opportunities?
```

```text
Show the top 5 cost optimization recommendations.
```

```text
Which recommendations have the highest monthly savings?
```

```text
Show affected resources for the highest saving recommendation.
```

```text
Show quick wins for AWS cost optimization.
```

---

# Security Notes

This project is based on an AWS sample solution and should be reviewed before production deployment.

Recommended hardening activities:

- IAM least privilege review
- Cognito security hardening e.g. enable MFA.
- logging and monitoring

---

# Future Enhancements

Potential roadmap items:

- Cognito Hosted UI with SAML federation for SSO integration
- Savings Plans recommendation agent
- tagging compliance analysis
- AWS budget monitoring
- Slack / Microsoft Teams chatbot integration
- Enhance reporting e.g download data using csv or pdf's.
- historical trend visualisations


---

# License / Attribution

This project is based on AWS sample guidance and has been customised and enhanced for improved AWS FinOps use cases.
Original AWS sample:
https://github.com/aws-solutions-library-samples/guidance-for-cost-analysis-and-optimization-with-amazon-bedrock-agents
