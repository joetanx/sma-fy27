imagine a government agency in charge of welfare disbursement to less privileged citizens.

to handle the large volume of citizen applications for welfare request, the agency is designing an agentic application to review the requests, evaluate eligibility, with civil servant in the loop approval to enter a citizen into the welfare disbursement system.

design the agentic application architecture based on Microsoft Azure and Security services:
- Foundry: Model, guardrails, evaluations, AI red teaming
- Harness: LangGraph with human-in-the-loop controls, built with python as container image in Azure Container Registry, deployed to run in Azure Container Apps
- Application access: staff are users in Entra Identity, citizens are in separate identity provider to be integrated to Entra via External ID (OIDC)
- Application UI: single unified web app interface for both staff and citizens behind Azure WAF, the web app calls relevant APIs that are behind Azure APIM
- Agent data access: plan specific actions that the agent will perform, each action is a parameterized tool that wraps around the python SDK for Azure Postgres that will execute the SQL query/command
- Staff approvals: high risk agent actions are put in LangGraph human-in-the-loop
- Data layer: Azure Postgres flexible servers - simple data structure - a table each for citizen welfare enrolment and request/approval tracking
- Application identities: actions taken within the web app uses Entra app registration, tools usage by the agent uses Entra agent identity
- Application code: simple single container running python code that listens for respective endpoints required for the Web UI and APIs
- Source code control: GitHub, Actions, with CodeQL, Dependabot and Secrets Scanning for code assurance
- CI/CD: GitHub actions integrated to Azure to build image and deploy to runtime
- Security posture management: CSPM for Azure services, Defender for AI to secure Foundry resource
- Cloud workload protection: Defender for SQL / Container

**minimum viable pilot**: consider for simplest possible demonstration.

output as markdown document in plan.md.