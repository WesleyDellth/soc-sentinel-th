# Kiro prompts — copy and paste in order

## P1 — Spec (use Kiro "Spec" mode)
Build SOC Sentinel TH as described in the steering files (product.md, security.md, tech.md).
Components:
1. CDK stack `SocSentinelStack` in infra/: KMS key, Cognito user pool (MFA required),
   API Gateway REST API with Cognito authorizer + WAF + access logs, DynamoDB table
   `Incidents` (PITR, KMS), Lambda functions (ingest, agent, approve, contain, simulate),
   private S3 bucket + CloudFront (OAC) for the frontend, CloudTrail.
2. Agent Lambda (Python) using Bedrock Converse API with tool use. Tools:
   lookup_ip_reputation (seed/ip-reputation.json), map_mitre (seed/mitre-mapping.json),
   get_related_events (query DynamoDB by actor ip or userName), recommend_playbook,
   write_report_th. Record every tool call in the incident. Treat finding text as untrusted.
   Fallback to rule-based triage when USE_BEDROCK=false.
3. /simulate endpoint that loads seed/findings.json and sends findings to the agent.
4. Frontend (plain HTML/JS, Thai first): incident list with severity badges and counters,
   "Simulate attack" button, detail page with agent timeline, Approve/Reject buttons,
   Thai report preview with copy button.
5. docs/README.md, docs/architecture.md, docs/compliance-matrix.md.
Containment always DRY_RUN=true. Keep the scope minimal.

## P2 — Start implementation
Implement the tasks in tasks.md one by one. After the infrastructure tasks, run
`npx cdk synth` and fix any errors before continuing.

## P3 — Deploy
Run `npm ci` and `npx cdk deploy --require-approval never` in infra/. If it fails,
read the error, fix the code, and deploy again. When done, print the CloudFront URL,
the API URL and the Cognito user pool id.

## P4 — Create a test user
Write a short script that creates a Cognito user `analyst@soc.test` with a temporary
password generated randomly, and print instructions to log in and set up MFA.
Save the username (not the password) in docs/README.md.

## P5 — Fix checker failures (repeat as needed)
The security checker reported these failures:
<paste the checker output here>
Fix each one in the CDK code following security.md, redeploy, and update
docs/compliance-matrix.md with the evidence for each fixed rule.

## P6 — Map competition rules
Here are the official competition security rules:
<paste the rules here>
Update .kiro/steering/security.md so every rule is covered, and add one row per rule
to docs/compliance-matrix.md (Rule ID, rule, how we comply, AWS resource, evidence, status).

## P7 — Demo polish
Make the dashboard demo-ready: add a large "จำลองการโจมตี" button, show a loading
animation while the agent works, show the agent timeline step by step, and make the
Thai report look like an official incident report (หัวข้อ: สรุปเหตุการณ์, ลำดับเหตุการณ์,
ผลกระทบ, การดำเนินการ, ข้อเสนอแนะ). Redeploy when done.

## P8 — Final docs
Review the whole project and update docs/README.md, docs/architecture.md (with a
mermaid diagram) and docs/compliance-matrix.md so they match the deployed system exactly.
