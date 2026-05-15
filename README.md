# CostAnalysisAgent
Provides Cost Analysis insights and savings recommendations

Deployment Steps:
1- Deploy the ZIP File using AWS Amplify (this will deploy the web agent).
2- Once the deployment is complete, Amplify will provide a URL for Chatbot (check 'Domain'). Use this URL to access Cost Analysis Agent.
3- Execute the Cloudformation Template with Role having permissions on Bedrock, Cognito, Lambda & IAM.
4- Provide your email id to receive the password.
5- The Output from Cloud formation template will be used to configure the Cost Analysis chat bot in Step 2.

Sample Questions:
What were my AWS costs last month?
Show my AWS cost by linked account for April 2026.
Which AWS services cost the most this month?
Show AWS cost by region for the last 30 days.

What are my current AWS cost saving opportunities?
Show the top 5 cost optimization recommendations.
Which recommendations have the highest monthly savings?
Show affected resources for the highest saving recommendation.


Reference Architecture:

<img width="1369" height="862" alt="image" src="https://github.com/user-attachments/assets/548fd570-dbea-4ce6-a100-2a2c8ab870df" />

Pre-Requisites:
- The userlying services uses AWS Trusted Advisor recommendation. Ensure Support plan > Basic
- Trusted Advisor is enabled for AWS Org.

  
Note: forked from https://github.com/aws-solutions-library-samples/guidance-for-cost-analysis-and-optimization-with-amazon-bedrock-agents
