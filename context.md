imagine a government agency in charge of welfare disbursement to less privileged citizens.

to handle the large volume of citizen applications for welfare request, the agency is designing an agentic application.
- a substantial amount of civil servant's time is spent explaining the eligibility criteria and application process, and clarify/request for the required information for processing - the agent's main role is to offload this from staff processing time
- when an application has all the required information, the agent evaluates eligibility according to the rubric, and add the request with its scoring and recommendations into staff approval queue
- the policy rubric will just be markdown files that agent can read and review (will be embedded in container app for pilot)

i just need a simplest minimum viable product to demonstrate the logic:
- Foundry: Model, guardrails, evaluations, AI red teaming
- Harness: LangGraph with human-in-the-loop controls, built with python as container image in Azure Container Registry, deployed to run in Azure Container Apps
- Application access: staff are users in Entra Identity, citizens are in separate identity provider to be integrated to Entra via External ID (OIDC)
- Application UI: single unified web app interface for both staff and citizens behind Azure WAF, the web app calls relevant APIs that are behind Azure APIM
- Data layer: Azure Postgres flexible servers - simple data structure - 1 table for citizen welfare enrollment and another table for request/approval tracking
- Citizens record: DbCitizen table - use UUID (in-place of social security number/NRIC) for each citizen, with column to record enrollment status (active, not-applied, rejected)
- Request tracking: agent goes into human-in-the-loop state and submits the requests into the DbState table, UUID foreign key to match with DbCitizen table, keeps every request submited with status (approved, rejected, pending) so that agent can look up request history
- Agent data access: plan specific actions that the agent will perform, each action is a parameterized tool that wraps around the python SDK for Azure Postgres that will execute the SQL query/command
- Agent assessment: rank request score (0-10) in score (INT) column, recommendations in markdown under recommendations (TEXT) column, the web UI will parse the markdown string into human readable for the staff
- Application code: simple single container running python code that listens for respective endpoints required for the Web UI and APIs
- Source code control: GitHub, Actions, with CodeQL, Dependabot and Secrets Scanning for code assurance
- CI/CD: GitHub actions integrated to Azure to build image and deploy to runtime
- Security posture management: CSPM for Azure services, Defender for AI to secure Foundry resource
- Cloud workload protection: Defender for SQL / Container

write the plan document in markdown format `plan.md`.