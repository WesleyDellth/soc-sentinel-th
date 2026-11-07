# Compliance Matrix — SOC Sentinel TH

Reference: NCSA (สกมช.) cloud security standard — competition rule set.
Replace the Rule IDs below with the official IDs given on event day.

| Rule ID | Rule | How we comply | AWS resource | Evidence | Status |
|---|---|---|---|---|---|
| ENC-01 | Data at rest encrypted | Customer-managed KMS key with rotation | KMS key, DynamoDB, S3, Logs | `infra/lib/stack.ts` (kms.Key enableKeyRotation) | ⬜ |
| ENC-02 | Data in transit encrypted | HTTPS only, enforceSSL, TLS 1.2 | CloudFront, S3, API Gateway | CloudFront ViewerProtocolPolicy | ⬜ |
| IAM-01 | Strong authentication | Cognito with MFA required | Cognito User Pool | mfa: REQUIRED | ⬜ |
| IAM-02 | Least privilege | One role per Lambda, no wildcard | IAM roles | cdk-nag / IAM policy review | ⬜ |
| NET-01 | No public data stores | Block Public Access, CloudFront OAC | S3 | blockPublicAccess: BLOCK_ALL | ⬜ |
| NET-02 | Edge protection | WAF managed rules | WAFv2 WebACL | WebACL association | ⬜ |
| LOG-01 | Audit logging | CloudTrail + API access logs | CloudTrail, API Gateway | Trail, accessLogDestination | ⬜ |
| LOG-02 | Log retention | 365 days | CloudWatch Logs | logRetention | ⬜ |
| BCK-01 | Backup / recovery | DynamoDB PITR, S3 versioning | DynamoDB, S3 | pointInTimeRecovery | ⬜ |
| GOV-01 | Data residency | Thai/ASEAN region | All | stack env region | ⬜ |
| GOV-02 | Resource tagging | Project/Owner/DataClass tags | All | Tags.of(app) | ⬜ |
| AI-01 | Human oversight of AI | Approve before containment, DRY_RUN | contain Lambda | approval check in code | ⬜ |
| AI-02 | Prompt-injection defence | Untrusted-data delimiters + filter | agent Lambda | test finding f-012 result | ⬜ |
| AI-03 | AI decision audit trail | Every tool call stored | DynamoDB Incidents | incident.toolCalls | ⬜ |

Status: ✅ pass · ⚠️ partial · ⬜ not checked yet
