# AWS Security Assessment

**IAM privilege escalation · Cloud attack paths · Data exposure · Detection and hardening**

Security assessment of fourteen isolated AWS training environments, covering identity compromise, privilege delegation, compute metadata, storage exposure and incident investigation. The cases are anonymised and grouped by control failure rather than presented as fourteen unrelated challenges.

## Assessment summary

Two attack paths deserve particular attention:

**CI identity to administrative access.** A service principal can modify a managed policy attached to itself. The resulting permissions allow it to deploy Lambda with a more privileged execution role, assume another role and access a protected secret.

**SSRF to EC2 role compromise.** An application fetches attacker-controlled URLs. With IMDSv1 accessible, the request can expose instance-role credentials that are authorised to read sensitive data.

Additional scenarios cover broad emergency-role trust, secrets in readable configuration, excessive S3 access, exposed EBS snapshots and ineffective IAM credential controls.

## Findings

| ID | Security issue | Risk* |
| --- | --- | --- |
| [F-01](findings/F-001-self-modifying-iam-policy.md) | Self-modifiable IAM policy | Critical |
| [F-02](findings/F-002-passrole-lambda-privilege-escalation.md) | Privileged Lambda execution via `iam:PassRole` | Critical |
| [F-03](findings/F-003-break-glass-role-trust.md) | Broad trust on emergency administrator role | Critical |
| [F-04](findings/F-004-exposed-reusable-secrets.md) | Exposed reusable credentials | High |
| [F-05](findings/F-005-s3-data-exposure.md) | Excessive S3 object access | High |
| [F-06](findings/F-006-ssrf-imds-credential-exposure.md) | SSRF and instance-metadata credential theft | Critical |
| [F-07](findings/F-007-ebs-snapshot-disclosure.md) | Shared EBS snapshot exposes stored data | High |
| [F-08](findings/F-008-iam-credential-governance.md) | Weak IAM privilege and credential governance | High |

*Risk ratings reflect the deliberately vulnerable exercise conditions, not an assessed production estate.*

## Report

- [Executive report and risk register](report/assessment-report.md)
- [Attack paths](report/attack-paths.md)
- [Technical methodology](report/methodology.md)
- [CloudTrail, GuardDuty and incident investigation](report/detection-and-response.md)
- [Remediation plan](report/remediation-roadmap.md)
- [Scope and rules of engagement](report/scope-and-roe.md)
- [Source exercise index](appendices/lab-traceability.md)

## Coverage

**Identity:** IAM users, roles, managed policies, trust policies, STS, MFA, access keys and privilege escalation.

**Compute:** Lambda execution roles, EC2, SSRF, IMDS and EBS snapshots.

**Data:** S3 bucket policies, ACLs, object versions, SSM Parameter Store and Secrets Manager.

**Security operations:** CloudTrail, GuardDuty, investigation timelines and hardening validation.

The assessment is based on the configuration and attack scenarios described in the training exercises. Original command output was not supplied for publication, so the report does not claim independent execution or production findings.
