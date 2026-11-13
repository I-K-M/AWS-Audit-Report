# AWS Security Assessment
### Anonymised, scenario-based cloud security assessment | Professional portfolio case study

**Assessment type:** AWS configuration review, privilege-escalation path analysis, simulated incident investigation and remediation planning  
**Environment:** Independent, disposable AWS training laboratories  
**Document status:** Scenario-based assessment; source evidence not yet supplied for independent verification  
**Prepared:** 13 November 2026 | **Language:** British English | **Classification:** Public / Sanitised

> **Evidence integrity notice.** This repository is **not** a report of a commissioned engagement or a live organisation's security posture. It consolidates fourteen separate training-laboratory briefs into an anonymised professional assessment. Described weaknesses are supported by the supplied lab scenarios, but actual command outputs, screenshots and CloudTrail records were **not provided** during compilation. Consequently, risk ratings are provisional, attack paths are labelled as laboratory scenarios, and no exploitation or remediation is represented as independently verified. No real credentials, account identifiers, personal data or secret values are published.

## Executive overview

The source scenarios demonstrate how an apparently constrained AWS identity can become a material risk when IAM permissions, trust relationships, service roles and data-access controls interact. The strongest self-contained scenario links modification of an IAM managed policy to privileged Lambda execution, subsequent role assumption and access to a protected secret. A separate application scenario connects server-side request forgery (SSRF), optional IMDSv1 and overprivileged instance credentials.

The assessment organises the exercises into **eight thematic findings**, four of which have *indicative* critical severity **if their described attack preconditions are met**. These are teaching scenarios, not eight independently confirmed defects in one production estate.

| ID | Thematic finding | Indicative severity | Source labs |
| --- | --- | --- | --- |
| [F-001](findings/F-001-self-modifying-iam-policy.md) | IAM managed-policy self-modification permits privilege escalation | Critical | 2, 5, 14 |
| [F-002](findings/F-002-passrole-lambda-privilege-escalation.md) | Privileged execution through `iam:PassRole` and Lambda | Critical | 3, 14 |
| [F-003](findings/F-003-break-glass-role-trust.md) | Overly broad emergency-role trust enables unintended assumption | Critical | 4, 5 |
| [F-004](findings/F-004-exposed-reusable-secrets.md) | Reusable credentials exposed through configuration | High | 1, 8 |
| [F-005](findings/F-005-s3-data-exposure.md) | Excessive S3 object access and insufficient bucket guardrails | High | 7, 9 |
| [F-006](findings/F-006-ssrf-imds-credential-exposure.md) | SSRF enables EC2 instance-credential retrieval via IMDSv1 | Critical | 11, 13 |
| [F-007](findings/F-007-ebs-snapshot-disclosure.md) | Inappropriately shared EBS snapshot exposes retained data | High | 12 |
| [F-008](findings/F-008-iam-credential-governance.md) | Inadequate IAM credential and privilege governance | High | 1, 5 |

Severity denotes **potential consequence under the specified laboratory conditions**, not validated exploitability or business risk in a live estate.

## Read the assessment

| Document | Purpose |
| --- | --- |
| [Assessment report](report/assessment-report.md) | Executive summary, assumptions, risk register, root-cause analysis and conclusions |
| [Scope & rules of engagement](report/scope-and-roe.md) | Authorisation model, in-scope activities, exclusions, stop conditions and evidence handling |
| [Assessment methodology](report/methodology.md) | Tool selection, least-privilege access, evidence standard and validation approach |
| [Attack-path analysis](report/attack-paths.md) | Two independent scenario kill chains and their preventative controls |
| [Detection & incident response](report/detection-and-response.md) | CloudTrail/GuardDuty expectations, investigation and test-coverage caveats |
| [Remediation roadmap](report/remediation-roadmap.md) | Risk-based sequencing, ownership, acceptance criteria and retesting |
| [Findings](findings/) | Eight individual, actionable findings with verification and remediation |
| [Lab-to-finding traceability](appendices/lab-traceability.md) | Mapping from each of the fourteen source scenarios to the assessment |
| [Evidence register](appendices/evidence-register.md) | Evidence placeholders, provenance requirements and sanitisation controls |
| [Interview revision](appendices/cloud-security-interview-brief.md) | Concise AWS security interview answers and certification direction |

## How to use the material

1. **Portfolio review:** Start with the [assessment report](report/assessment-report.md), then inspect an IAM finding and the associated attack-path analysis.
2. **Technical validation:** Recreate a deliberately vulnerable configuration **only within an authorised, isolated environment**. Capture baseline permissions, test results, relevant CloudTrail event IDs and after-remediation outputs.
3. **Evidence promotion:** Change an item from *Scenario-supported / Unverified* to *Lab-verified* only when a redacted, independently reviewable artefact has been attached to the [evidence register](appendices/evidence-register.md).
4. **Client adaptation:** Replace the demonstration RoE with a signed, client-specific agreement and reassess all severity ratings against the actual business impact and compensating controls.

## Professional scope and limitations

- Exercises originated in separate disposable labs; no single real-world environment is asserted to contain every finding.
- No live customer data, account identifiers, public IP addresses, exploitable hostnames or working credentials are included.
- Tool mentions (AWS CLI, CloudFox, Pacu, Prowler and ScoutSuite) describe a proposed approach, **not evidence that every tool was run**.
- Findings are grouped by **root cause and impact**, not by the number of lab flags.
- AWS platform behaviour and tool capabilities may change; re-check vendor guidance at the time of any real engagement.

## Responsible use

Content is intended for **authorised security testing, defensive engineering and education**. Do not apply invasive tests to environments without written approval. The repository intentionally excludes working exploitation payloads, credential values and real sensitive outputs.

---

**Assessment proposition:** identify the exposure, demonstrate the path within scope, explain the business consequence, prescribe an implementable control, and define the retest needed to close the risk.
