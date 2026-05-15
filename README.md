# FinOps Cost Intelligence Agent with Amazon Bedrock

An AI-powered AWS FinOps assistant that provides **cost analysis, spend visibility, and cost optimization recommendations** using **Amazon Bedrock Agents, Amazon Nova, AWS Cost Explorer, AWS Trusted Advisor, Cognito, Lambda, and AWS Amplify**.

This project is **forked and enhanced from the AWS sample solution**:

AWS Sample Reference:  
https://github.com/aws-solutions-library-samples/guidance-for-cost-analysis-and-optimization-with-amazon-bedrock-agents

---

# Overview

This solution deploys a multi-agent FinOps chatbot that helps AWS users:

- Analyse historical AWS spend
- Break down costs by service, account, usage type, and region
- Discover AWS cost optimization opportunities
- Review Trusted Advisor cost recommendations
- Inspect affected resources for savings recommendations
- Interact through a web-based chatbot hosted on AWS Amplify

The architecture uses **Amazon Bedrock multi-agent collaboration**, where a supervisor agent routes requests to specialist agents.

---

# Key Enhancements Over AWS Sample

This fork includes several improvements over the original AWS sample implementation.

## Cost Analysis Improvements

### Improved Cost Analysis Agent Logic
Enhanced Bedrock agent instructions to:

- enforce proper action group selection
- prevent incorrect grouping interpretation
- stop service-level data being shown as regional/account data
- improve response consistency
- validate requested grouping vs returned grouping

### Improved Cost Grouping Support
Enhanced cost analysis support for:

- AWS Service
- Linked Account
- Usage Type
- Availability Zone (regional-style analysis)
- Billing Entity

### Better Response Schema
Updated OpenAPI schema:

- renamed generic grouped response field to `top_10_items`
- added explicit `group_by` response validation
- added schema warnings to prevent Bedrock hallucinating grouped results

---

## Cost Optimization Improvements

### Improved Cost Optimization Agent
Enhanced Trusted Advisor optimization behaviour:

- prioritises recommendations by estimated monthly savings
- separates quick wins from recommendations requiring validation
- classifies risk and implementation effort
- improves resource-level recommendation handling
- prevents mixing actual spend with projected savings

### Better Trusted Advisor Tool Definitions
Improved action group descriptions for:

- recommendation summary retrieval
- resource-level recommendation detail lookup

This significantly improves agent decision-making.

---

# Architecture

## Reference Architecture

<img width="1369" height="862" alt="Reference Architecture" src="https://github.com/user-attachments/assets/548fd570-dbea-4ce6-a100-2a2c8ab870df" />

---

# Components

| Component | Purpose |
|---------|---------|
| AWS Amplify | Hosts the chatbot web application |
| Amazon Bedrock | Multi-agent orchestration |
| Amazon Nova | Foundation model powering agents |
| Cognito | Authentication and user management |
| AWS Lambda | Backend action group execution |
| AWS Cost Explorer | Historical cost analysis |
| AWS Trusted Advisor | Cost optimization recommendations |
| CloudFormation | Infrastructure deployment |

---

# Solution Architecture

## Agents

### Supervisor Agent
Routes user questions to the appropriate specialist agent.

### Cost Insights Agent
Handles:

- historical spend analysis
- service cost breakdown
- linked account analysis
- usage type analysis
- regional spend analysis
- billing trends

### Cost Optimization Agent
Handles:

- Trusted Advisor cost optimization recommendations
- highest savings opportunities
- affected resources
- quick wins
- recommendation prioritisation

---

# Pre-Requisites

Before deployment, ensure:

## AWS Services

- Amazon Bedrock enabled in your AWS Region
- Access to Amazon Nova foundation models
- AWS Cost Explorer enabled
- AWS Trusted Advisor enabled

## Support Plan Requirement

Trusted Advisor cost optimization recommendations require:

- AWS Business Support
- AWS Enterprise On-Ramp
- AWS Enterprise Support

**Basic Support is NOT sufficient for full cost optimization recommendations.**

## IAM Permissions

The deployment role requires permissions for:

- Amazon Bedrock
- AWS Lambda
- IAM
- Cognito
- CloudFormation
- AWS Trusted Advisor
- Cost Explorer
- Amplify (if deploying frontend manually)

---

# Deployment Steps

## Step 1 — Deploy Web Frontend

Deploy the provided frontend ZIP package to AWS Amplify.

1. Open AWS Amplify Console
2. Select **Deploy without Git provider**
3. Upload the frontend ZIP file
4. Complete deployment

After deployment, Amplify will generate a hosted application URL.

Example:

```text
https://main.xxxxxx.amplifyapp.com
```

---

## Step 2 — Deploy Backend Infrastructure

Deploy:

```text
deployment/cfn-finops-bedrock-multiagent-nova.yaml
```

using CloudFormation.

Required parameter:

- Email address for Cognito user creation

Example:

```bash
aws cloudformation deploy \
  --template-file deployment/cfn-finops-bedrock-multiagent-nova.yaml \
  --stack-name finops-agent \
  --capabilities CAPABILITY_NAMED_IAM
```

---

## Step 3 — Retrieve CloudFormation Outputs

After deployment, capture:

- Bedrock Agent ID
- Bedrock Agent Alias ID
- Cognito User Pool ID
- Cognito Identity Pool ID
- AWS Region

---

## Step 4 — Configure Amplify Frontend

Update the frontend configuration using the CloudFormation outputs from Step 3.

Then redeploy the frontend if required.

---

## Step 5 — Login

A temporary password will be sent to the supplied email address.

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

---

# Security Notes

This project is based on an AWS sample and intended as a reference implementation.

Before production use, review:

- IAM least privilege
- Bedrock agent permissions
- Cognito authentication hardening
- network access controls
- logging and monitoring
- API throttling / abuse controls
- frontend secret/config management

---

# Troubleshooting

## CloudFormation OpenAPI Errors

If deployment fails with:

```text
Failed to create OpenAPI 3 model
```

Check:

- JSON commas
- OpenAPI schema formatting
- YAML indentation
- schema property nesting

---

## Trusted Advisor Empty Results

If optimization recommendations are empty:

- confirm support plan eligibility
- confirm Trusted Advisor access
- verify organization-level access if using AWS Organizations

---

# Future Enhancements

Potential roadmap:

- SSO via Cognito Hosted UI + SAML/OIDC
- Cost anomaly detection agent
- Savings Plans recommendation agent
- Reserved Instance advisor
- tagging compliance agent
- budget monitoring agent
- Slack / Teams chatbot integration
- API Gateway backend instead of direct Bedrock invocation

---

# License / Attribution

This project is based on AWS sample guidance and has been customised for enhanced FinOps use cases.

Original AWS sample:

https://github.com/aws-solutions-library-samples/guidance-for-cost-analysis-and-optimization-with-amazon-bedrock-agents
