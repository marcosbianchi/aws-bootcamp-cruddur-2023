# Week 0 — Billing and Architecture

## Required Homework/Tasks

### Install and Verify AWS CLI 

After adding config in gitpod.yml, showed identity and listed EventBridge rules from a terminal 

![aws-cli-proof](assets/mb-proof-aws-cli.png)

### Create a Budget

Created a Zero Spend Budget ($1) because I want to be alerted as soon as any service crosses the free tier limits.

Added a Policy to allow my admin user to access Budgets (see [Allow Billing Access to my Admin User](week0.md#allow-billing-access-to-my-admin-user))

![Alt text](assets/mb-proof-budget.png)

### Recreate Logical Architectural Design

- Introduced a change in the frontend part, assuming that server side processing is not needed and it can be served from an S3 bucket. Access to this bucket is restricted to only the CDN.
Using a CDN we can improve the end user experience by making access to our application more "locally".
- Also added a VPC Endpoint to communicate the backend service and the DynamoDB database without requering to use a NAT gateway and thus reduce costs.

![Alt text](assets/mb-proof-arch-diagram-lucidchart.png)

[Link to diagram (comments are welcome!)](https://lucid.app/lucidchart/a261f663-6e35-45a3-b1b6-5e1bdcfaed0b/edit?viewport_loc=-271%2C30%2C2591%2C1305%2C0_0&invitationId=inv_8751caeb-e57a-4862-b56e-c24dce22d3a0)

### Conceptual Diagram

![Alt text](assets/conepctual-diagram-mb%20Medium.jpeg)


## Homework Challenges

### Added AWS WAF service 

- As an extra layer of security to protect our application, I integrated CloudFront CDN with an AWS WAF Web ACL that inspects and manages web requests. It can block them based on a specified criteria. For instance, it can block combinations of HTTP methods that are not supported by CloudFront.

### Create EventBridge rule 

Created an event rule in the default event bus that catches all AWS Health events and added an SNS Topic as a target to send emails each time the rule triggers.

![Alt text](assets/mb-proof-eventbridge-health-rule.png)

![Alt text](assets/mb-proof-eventbridge-rule-cli.png)

### Allow Billing Access to my Admin User

Steps:
1. Created a BillingFullAccess policy for my admin user marcos.bianchi
2. Created user group "billing-admins" and attached previous policy
3. Added my user marcos.bianchi to the previous group

Followed this guide:
[IAM tutorial: Delegate access to the billing console](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_billing.html?icmpid=docs_iam_console#tutorial-billing-step2)

![Alt text](assets/mb-proof-billing-policy.png)
