<p align="center">
  <h1 align="center">FinOps Cost Intelligence Agent</h1>
  <p align="center">
    AI-powered AWS Cost Analysis & Optimization using Amazon Bedrock Multi-Agent Collaboration
  </p>
</p>

---

# Overview

The FinOps Cost Intelligence Agent is an AI-powered AWS FinOps assistant that provides:

- AWS cost analysis
- spend visibility
- linked account breakdowns
- regional cost analysis
- usage insights
- Trusted Advisor optimization recommendations
- savings opportunity prioritization

The solution leverages:

- Amazon Bedrock Agents
- Amazon Nova foundation models
- AWS Cost Explorer
- AWS Trusted Advisor
- AWS Lambda
- Amazon Cognito
- AWS Amplify

This project is forked and enhanced from the AWS sample implementation:

https://github.com/aws-solutions-library-samples/guidance-for-cost-analysis-and-optimization-with-amazon-bedrock-agents

---

# Technology Stack

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

This project contains several enhancements over the original AWS sample implementation.

## Cost Analysis Improvements

### Enhanced Cost Analysis Agent

Improved Bedrock agent orchestration and reasoning:

- improved action group selection
- grouping validation logic
- stricter response interpretation
- reduced hallucination risk
- better handling of regional/account/service breakdowns
- improved response formatting

### Improved Cost Grouping Support

Enhanced AWS cost analysis support for:

- AWS Service
- Linked Account
- Usage Type
- Availability Zone
- Billing Entity

### Improved OpenAPI Schema

Updated schema behaviour:

- renamed generic grouped response field to `top_10_items`
- added explicit `group_by` validation
- added schema guardrails and warnings
- improved action descriptions

---

## Cost Optimization Improvements

### Enhanced Cost Optimization Agent

Improved optimization workflows:

- prioritises recommendations by estimated savings
- separates quick wins from validation-required changes
- improves recommendation ranking
- improves resource-level recommendation handling
- improves recommendation interpretation

### Improved Trusted Advisor Integration

Enhanced:

- recommendation summaries
- recommendation descriptions
- resource lookup workflows
- savings interpretation
- recommendation prioritisation

---

# Architecture

## Reference Architecture

<img width="1369" height="862" alt="Reference Architecture" src="https://github.com/user-attachments/assets/548fd570-dbea-4ce6-a100-2a2c8ab870df" />

---

# Solution Architecture

This solution uses Amazon Bedrock multi-agent collaboration to provide AWS FinOps insights through specialised agents.

## Agent Architecture

| Agent | Role | Responsibilities | AWS Services Used |
|------|------|------------------|------------------|
| **Supervisor Agent** | Request Orchestrator | Receives user questions, determines intent, routes requests to the correct specialist agent, and combines responses for multi-step workflows. | Amazon Bedrock Agents |
| **Cost Insights Agent** | Spend Analysis Specialist | Analyses AWS historical spend, cost trends, service-level costs, linked account spend, usage type breakdowns, Availability Zone/regional cost views, and billing insights. | AWS Cost Explorer, Lambda, Bedrock |
| **Cost Optimization Agent** | Savings Specialist | Retrieves AWS Trusted Advisor cost optimization findings, prioritises savings opportunities, shows affected resources, separates quick wins from validation-required recommendations. | AWS Trusted Advisor, Lambda, Bedrock |

---

## Request Flow

```text
User
   ↓
AWS Amplify Web Application
   ↓
Amazon Cognito Authentication
   ↓
Supervisor Agent
   ├── Cost Insights Agent
   └── Cost Optimization Agent
   ↓
Lambda Action Groups
   ├── AWS Cost Explorer
   └── AWS Trusted Advisor
```

---

# Components

| Component | Purpose |
|---------|---------|
| AWS Amplify | Hosts the chatbot web application |
| Amazon Cognito | User authentication and identity management |
| Amazon Bedrock | Multi-agent orchestration |
| Amazon Nova | Foundation model used by agents |
| AWS Lambda | Executes backend action groups |
| AWS Cost Explorer | Historical cost and usage analysis |
| AWS Trusted Advisor | Cost optimization recommendations |
| CloudFormation | Infrastructure deployment |

---

# Pre-Requisites

Before deployment, ensure the following services and permissions are available.

## AWS Services

- Amazon Bedrock enabled in your AWS Region
- Access to Amazon Nova foundation models
- AWS Cost Explorer enabled
- AWS Trusted Advisor enabled
- Amazon Cognito access enabled
- AWS Amplify access enabled

---

## AWS Support Plan Requirement

Trusted Advisor cost optimization recommendations require one of the following support plans:

- AWS Business Support
- AWS Enterprise On-Ramp
- AWS Enterprise Support

Basic Support is not sufficient for full optimization recommendations.

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

Once deployment completes, Amplify will provide a hosted application URL.

Example:

```text
https://main.xxxxxx.amplifyapp.com
```

---

## Step 2 — Deploy Backend Infrastructure

Deploy the CloudFormation template:

```text
deployment/cfn-finops-bedrock-multiagent-nova.yaml
```

Example deployment:

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

After deployment completes, capture the following outputs:

| Output | Purpose |
|------|---------|
| Agent ID | Bedrock Agent Identifier |
| Agent Alias ID | Bedrock Agent Alias |
| Cognito User Pool ID | Authentication configuration |
| Cognito Identity Pool ID | Federated identity configuration |
| AWS Region | Frontend configuration |

---

## Step 4 — Configure Frontend

Update the Amplify frontend configuration using the CloudFormation outputs from Step 3.

Redeploy the frontend if required.

---

## Step 5 — Login

Login using:

- email address
- temporary password

---

# Sample Questions

## Cost Analysis Questions

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

## Cost Optimization Questions

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
- Cognito security hardening
- API throttling and abuse controls
- logging and monitoring
- Bedrock access restrictions
- frontend configuration protection
- production authentication review
- network access controls

---

# Troubleshooting

## CloudFormation OpenAPI Errors

If deployment fails with:

```text
Failed to create OpenAPI 3 model
```

Validate:

- JSON commas
- OpenAPI formatting
- YAML indentation
- schema property nesting

---

## Trusted Advisor Returns No Results

Verify:

- support plan eligibility
- Trusted Advisor access
- AWS Organization permissions
- regional availability

---

## Bedrock Access Errors

Verify:

- Bedrock enabled in target Region
- access granted to Amazon Nova models
- IAM permissions for Bedrock agents

---

# Future Enhancements

Potential roadmap items:

- Cognito Hosted UI with SAML/OIDC federation
- Cost anomaly detection agent
- Savings Plans recommendation agent
- Reserved Instance recommendation agent
- tagging compliance analysis
- AWS budget monitoring
- Slack / Microsoft Teams integration
- API Gateway backend integration
- reporting dashboards
- historical trend visualisations

---

# References

| Reference | Description |
|---------|-------------|
| AWS Sample Solution | Original AWS reference implementation |
| Amazon Bedrock Agents Documentation | Multi-agent orchestration |
| Amazon Nova Documentation | Foundation model documentation |
| AWS Cost Explorer API | Historical spend analysis |
| AWS Trusted Advisor API | Cost optimization recommendations |
| Amazon Cognito Documentation | Authentication and identity management |
| AWS Amplify Documentation | Frontend hosting |
| AWS Lambda Documentation | Backend execution |

---

# Reference Links

| Service | Link |
|--------|------|
| AWS Sample Repository | https://github.com/aws-solutions-library-samples/guidance-for-cost-analysis-and-optimization-with-amazon-bedrock-agents |
| Amazon Bedrock Agents | https://docs.aws.amazon.com/bedrock/latest/userguide/agents.html |
| Bedrock Multi-Agent Collaboration | https://docs.aws.amazon.com/bedrock/latest/userguide/agents-multi-agent-collaboration.html |
| Amazon Nova Models | https://docs.aws.amazon.com/bedrock/latest/userguide/model-parameters-nova.html |
| AWS Cost Explorer API | https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_GetCostAndUsage.html |
| AWS Trusted Advisor API | https://docs.aws.amazon.com/trustedadvisor/latest/APIReference/Welcome.html |
| Amazon Cognito | https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html |
| AWS Amplify | https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html |
| AWS Lambda | https://docs.aws.amazon.com/lambda/latest/dg/welcome.html |

---

# License / Attribution

This project is based on AWS sample guidance and has been customised and enhanced for improved AWS FinOps use cases.

Original AWS sample:

https://github.com/aws-solutions-library-samples/guidance-for-cost-analysis-and-optimization-with-amazon-bedrock-agents
