# CostAnalysisAgent
Provides Cost Analysis insights and savings recommendations

Deployment Steps:
1- Deploy the ZIP File using AWS Amplify (this will deploy the web agent).
2- Once the deployment is complete, Amplify will provide a URL for Chatbot (check 'Domain'). Use this URL to access Cost Analysis Agent.
3- Execute the Cloudformation Template with Role having permissions on Bedrock, Cognito, Lambda & IAM.
4- Provide your email id to receive the password.
5- The Output from Cloud formation template will be used to configure the Cost Analysis chat bot in Step 2.

Sample Questions:


Reference Architecture:

<img width="1369" height="862" alt="image" src="https://github.com/user-attachments/assets/548fd570-dbea-4ce6-a100-2a2c8ab870df" />


Note: forked from https://github.com/aws-solutions-library-samples/guidance-for-cost-analysis-and-optimization-with-amazon-bedrock-agents
