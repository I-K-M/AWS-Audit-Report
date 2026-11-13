# Laboratory-to-Finding Traceability

This register identifies which **separate** training scenario supplies the basis of each report section. It does not claim that all scenarios were observed in one account.

| Lab | Scenario summary | Mapped findings | Role in portfolio |
| --- | --- | --- | --- |
| 01 | Low-privilege AWS reconnaissance; broad IAM reads and a readable synthetic marker | F-004, F-008 | Identity and discovery scope; possible sensitive metadata exposure |
| 02 | Self-managed IAM policy version enables privilege escalation | F-001 | Single-technique IAM escalation |
| 03 | Deployer uses PassRole and Lambda with privileged execution role | F-002 | Delegation and compute escalation |
| 04 | Broad trust of emergency administrator role | F-003 | Direct STS assumption risk |
| 05 | IAM hardening: wildcard policy, policy-version rights, broad trust, key age, MFA/boundary | F-001, F-003, F-008 | Remediation exercise, not automatically verified closure |
| 06 | CloudTrail/GuardDuty IAM escalation investigation | F-001 and detection context | Forensics method and timeline reconstruction |
| 07 | S3 object access despite apparently protective settings; possible historical version | F-005 | Sensitive object exposure scenario |
| 08 | Reusable credential in Lambda/SSM configuration and pivot | F-004 | Secrets-management exposure scenario |
| 09 | S3 hardening: public access, policy, TLS, SSE-KMS and versioning | F-005 | Remediation baseline and acceptance criteria |
| 10 | Exported JSON GuardDuty finding, CloudTrail and S3 data-event investigation | Detection context and F-005 | Exfiltration forensics; **no live detector** is expected |
| 11 | URL-fetch SSRF → IMDSv1 → instance-role credentials → protected secret | F-006 | Independent web-to-cloud attack path |
| 12 | Shared EBS snapshot restored to expose retained filesystem secret | F-007 | Backup/snapshot disclosure scenario |
| 13 | EC2 metadata hardening: require IMDSv2, test hop limit | F-006 | Defence-in-depth remediation exercise |
| 14 | CI foothold → managed policy version → Lambda/PassRole → AssumeRole → secret | F-001, F-002 | Main multi-stage IAM attack path |

## Traceability caveats

- Lab 1 uses a **variable secret placement**; tags, role descriptions and object storage should not all be asserted as simultaneously present.
- Lab 8 presents **alternative** Lambda and SSM secret locations; evidence must establish which was actually used.
- Lab 7 describes several possible S3 access-control/versioning defects, not a proven combination of all possible mechanisms.
- Lab 9 concerns a demonstration configuration with a requested SSE-KMS control. Modern S3 applies default SSE-S3 to newly uploaded objects, so a missing customer-managed encryption configuration does not establish unencrypted data at rest.
- Lab 10 is a log-export analysis exercise: absence of live GuardDuty or CloudTrail Lake is intentional.
- Labs 5, 9 and 13 specify required **target states**; their successful completion is not documented here.
- Lab 14's privileged execution and STS hop is a scenario chain; actual role trust and policy evidence is needed for verification.

## Evidence progression

**Scenario-described → Lab observation collected → Independently reviewed → Retested after remediation → Closed.**

Until the corresponding evidence is attached, status remains **Scenario-described / Unverified**.
