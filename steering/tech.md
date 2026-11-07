# Tech stack and conventions

- IaC: AWS CDK v2, TypeScript, single stack `SocSentinelStack` in `infra/`.
- Backend: Python 3.12 Lambda functions in `backend/` (ingest, agent, approve, contain, simulate).
- AI: Amazon Bedrock Converse API with tool use. Model id comes from env var MODEL_ID
  (use whatever Claude model the workshop account has enabled). Max 6 tool-use turns.
- Data: one DynamoDB table `Incidents` (pk: incidentId). Store tool calls as a list attribute.
- Frontend: plain HTML + vanilla JS (no build step) in `frontend/`, deployed to S3 via
  CDK BucketDeployment, served by CloudFront. Use Cognito Hosted UI for login.
- Static reference data in `seed/`: findings.json, mitre-mapping.json, ip-reputation.json.
  Bundle them into the agent Lambda.
- Keep it simple: no frameworks that need a build step, no containers, no VPC.
- Every command must work from the Kiro browser terminal: `npm ci`, `npx cdk synth`,
  `npx cdk deploy --require-approval never`.
