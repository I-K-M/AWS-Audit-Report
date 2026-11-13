# AWS Security Assessment Report

**Scope:** Fourteen isolated AWS security exercises  
**Coverage:** IAM, STS, Lambda, EC2, S3, EBS, SSM, Secrets Manager, CloudTrail and GuardDuty  
**Assessment date:** 13 November 2026

## Executive summary

The scenarios expose two credible routes from restricted access to sensitive AWS resources. In the first, an application identity can alter its own managed policy. Combined with the ability to pass a privileged role to Lambda, this permits an escalation towards administrative access. In the second, a web application accepts an arbitrary URL for server-side retrieval. IMDSv1 remains available on the host, allowing the application to expose instance-role credentials.

Both paths depend on several conditions being present at once. Fixing the most obvious symptom would reduce exposure, but permissions and trust relationships also need attention.

The remaining exercises cover a broadly trusted emergency administrator role, credentials stored in readable configuration, S3 object access, shared EBS snapshots and shortcomings in IAM access-key and authentication controls. The set contains separate examples of exploitation, configuration hardening and log-based investigation.

### Principal risks

| Area | Exposure | Consequence under the exercise conditions |
| --- | --- | --- |
| IAM policy administration | CI identity manages versions of its attached policy | Self-escalation to broader AWS permissions |
| Privilege delegation | Deployer passes a privileged role to Lambda | Execution with permissions unavailable to the deployer |
| Role trust | Emergency administrator role trusts an overbroad principal | Unintended assumption of administrative privileges |
| Application and EC2 | SSRF reaches IMDSv1 | Theft and misuse of temporary instance credentials |
| Storage and secrets | Accessible S3 objects, snapshots and configuration values | Disclosure of credentials or sensitive retained data |

### Recommended actions

1. Separate application/deployment permissions from IAM policy administration. Remove self-service policy-version changes and restrict which execution roles can be passed to AWS services.
2. Rework emergency-role trust to approved identities and require an appropriate privileged-access process.
3. Address exposed credentials and data access. Restrict S3 and snapshot sharing; review the permissions that allow configuration readers to retrieve reusable secrets.
4. Remediate the URL-fetching SSRF, enforce IMDSv2 where compatible and reduce EC2 instance-role permissions.
5. Check that CloudTrail management events, relevant data events and security alerts support investigation of these paths.

## 1. Findings

| ID | Finding | Severity | Source exercises | Control owner |
| --- | --- | --- | --- | --- |
| [F-01](../findings/F-001-self-modifying-iam-policy.md) | IAM policy self-modification | Critical | 2, 5, 14 | IAM / Platform |
| [F-02](../findings/F-002-passrole-lambda-privilege-escalation.md) | Privileged Lambda via PassRole | Critical | 3, 14 | IAM / Serverless |
| [F-03](../findings/F-003-break-glass-role-trust.md) | Overbroad emergency-role trust | Critical | 4, 5 | IAM / Operations |
| [F-04](../findings/F-004-exposed-reusable-secrets.md) | Reusable secrets in readable configuration | High | 1, 8 | Application / IAM |
| [F-05](../findings/F-005-s3-data-exposure.md) | S3 access-control and recovery gaps | High | 7, 9 | Data Platform |
| [F-06](../findings/F-006-ssrf-imds-credential-exposure.md) | SSRF to EC2 role credentials | Critical | 11, 13 | Application / Compute |
| [F-07](../findings/F-007-ebs-snapshot-disclosure.md) | Excessive EBS snapshot sharing | High | 12 | Compute / Storage |
| [F-08](../findings/F-008-iam-credential-governance.md) | Broad IAM access and weak credential controls | High | 1, 5 | IAM / Security |

Severity is based on the access and impact specified in each lab. The scenarios are independent; the table is not an inventory of defects found in one company.

## 2. IAM privilege escalation

### F-01 — Self-modifiable managed policy

An operational identity can create a new version of a managed policy attached to itself and make the new version active. In the relevant exercises, this allows the identity to acquire privileges it did not hold initially.

The exposure is rooted in policy administration rights being granted to the policy's own consumer. Restricting ordinary read access alone would not address it.

**Correction:** remove `iam:CreatePolicyVersion` and `iam:SetDefaultPolicyVersion` from operational identities on their own effective policies. Route changes through a separate administrator with review and deployment controls.

### F-02 — PassRole and privileged Lambda execution

The deployer can create a Lambda function and pass it a role holding greater permissions, including administrative rights in one exercise. Lambda assumes the execution role when it runs. The deployer therefore gains an indirect route to privileged API actions.

**Correction:** limit `iam:PassRole` to approved runtime roles, review the role's actual permissions and restrict the ability to create or reconfigure workloads using privileged roles.

### F-03 — Emergency-role trust

A break-glass administrator role accepts trust from the whole account. The exercise's initial identity also has `sts:AssumeRole` permission, allowing it to assume that role.

An account-root ARN in a trust policy refers to the account as a principal. It does not mean that only its root user can assume the role.

**Correction:** identify the actual emergency principals, narrow trust and require appropriate authentication, approval and monitoring.

The IAM relationships are illustrated in [Attack paths](attack-paths.md).

## 3. Data exposure

### Secrets in configuration

The training scenarios place a reusable value in a surface readable by a less-privileged identity: a Lambda environment variable, an SSM parameter or another metadata field. The value may unlock a second AWS identity.

The immediate concern is not the use of Lambda or SSM itself. It is the combination of a reusable credential and access to its stored value.

**Correction:** restrict configuration reads, migrate credentials to appropriately controlled storage and replace reusable access keys with short-lived credentials where feasible. Rotate any credentials that have been disclosed.

### S3 access

The S3 exercises involve a bucket policy or ACL that permits unintended object access, including potentially retained versions. Another exercise starts with disabled Block Public Access, broadly scoped policy permissions, no explicit TLS requirement and versioning disabled.

S3 encrypts new objects at rest using SSE-S3 by default. A requirement for SSE-KMS must therefore be treated as a specific key-management control rather than proof that a bucket otherwise stores cleartext.

**Correction:** validate effective object permissions, restrict the sensitive dataset, configure Block Public Access as appropriate, enforce TLS and set a recovery/versioning policy.

### EBS snapshot access

A snapshot holding filesystem data is shared with a principal that can restore it to a volume and read retained files. Snapshot sharing and related KMS permissions need to be evaluated together.

**Correction:** limit snapshot restore permissions, review sharing changes and account for residual secrets in backups.

## 4. EC2 and application security

The web application accepts a destination URL and retrieves it server-side. Under the exercise conditions, this gives access to the EC2 metadata service because IMDSv1 remains enabled. The returned temporary credentials permit access to the protected lab secret.

Enabling IMDSv2 disrupts the described GET-only metadata request but does not remove the underlying SSRF. The application should prevent requests to internal, link-local and otherwise prohibited destinations, including through redirects.

**Correction:** fix server-side URL retrieval, test its network restrictions, require IMDSv2 after compatibility testing and reduce the role's access to secrets.

## 5. Detection and forensics

The incident-response exercises use CloudTrail management events and exported data events to reconstruct IAM escalation and S3 data access. Useful signals include:

| Event | Investigative question |
| --- | --- |
| `CreatePolicyVersion` / `SetDefaultPolicyVersion` | Who altered an effective IAM policy? |
| `CreateFunction` / `Invoke` | Was privileged runtime execution established? |
| `AssumeRole` | Which principal moved into another role? |
| `GetSecretValue` | Which identity requested a secret? |
| S3 `GetObject` data event | Which object was read and when? |

These signals need appropriate coverage and correlation. GuardDuty may produce relevant findings, but one API call does not imply one guaranteed alert. The export-based investigation exercise did not use a live GuardDuty detector.

See [Detection and investigation](detection-and-response.md).

## 6. Remediation priorities

**Immediate risk reduction:** remove IAM self-escalation and unapproved role delegation, protect the emergency administrator role, and revoke credentials confirmed exposed.

**Next engineering cycle:** fix the SSRF and IMDS path, bring S3 and EBS sharing into line with intended access and verify workload compatibility.

**Ongoing controls:** standardise IAM role design and CI federation; review managed-policy changes and privileged role assumptions; test alerting and closure of findings.

The [remediation plan](remediation-roadmap.md) describes the control owners and verification tests.

## 7. Basis of assessment

This report consolidates the supplied briefs for fourteen disposable AWS training labs. It does not represent a live client commission. No original execution output was supplied, and the report does not assert that any scenario was independently retested.

The [source exercise index](../appendices/lab-traceability.md) maps each finding to its source and identifies the exercises concerned with hardening or forensics.
