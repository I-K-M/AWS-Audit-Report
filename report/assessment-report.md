# AWS Security Assessment — Consolidated Report

**Anonymised laboratory-based assessment**  
**Assessment date:** 13 November 2026  
**Audience:** Engineering leadership, Cloud Security, DevSecOps and security operations  
**Classification:** Public / Sanitised  
**Evidence status:** Scenario-supported; technical artefacts pending  
**Assessment boundary:** Fourteen independent, disposable AWS security exercises

## 1. Executive summary

### Overall assessment

The laboratory scenarios illustrate several high-impact AWS security failure modes: IAM principals able to alter their own permissions; service roles granting excessive privilege to Lambda; emergency administrator roles with broad trust; reusable credentials stored in readable configuration; insufficiently constrained access to S3 objects and EBS snapshots; and an EC2-hosted SSRF primitive capable of reaching instance credentials when IMDSv1 remains enabled.

The most material concern is **privilege accumulation through combinations of otherwise routine permissions**. In the composite IAM scenario, a CI service identity can change an attached managed policy, obtain the ability to pass a privileged execution role, create a Lambda function and follow an allowed trust relationship to an administrative role, ultimately reaching a protected secret. In a separate EC2 application scenario, a URL-fetching feature permits server-side requests to instance metadata; IMDSv1 access exposes temporary role credentials, allowing access to a sensitive secret.

These are **described exercise scenarios, not observations of an actual customer's AWS estate**. No infrastructure was accessed or tested to prepare this report. Accordingly, severity levels reflect the consequence *if* each configuration and attack path is confirmed.

### Priority actions

1. Remove IAM self-modification primitives from application and deployment identities; tightly constrain `iam:PassRole` and its associated service-creation permissions.
2. Limit privileged execution-role access and restrict emergency-role trust to explicitly identified, authorised operators with strong authentication and monitoring.
3. Eliminate reusable secrets from broadly readable configuration; rotate exposed credentials and enforce access boundaries for sensitive data.
4. Fix the application SSRF and require IMDSv2 on EC2 instances, with compatibility testing.
5. Restrict S3 object and snapshot access; validate logging, detection and remediation effectiveness.

### Risk statement

**Indicative exposure: High to Critical in individual, deliberately vulnerable lab scenarios.** It would be misleading to assign a single measured organisational risk score or compliance percentage without the complete resource inventory, test outputs, compensating controls and a defined business context.

## 2. Scope, basis and limitations

The report is a **portfolio case study** built from fourteen independent training-laboratory descriptions. It represents a simulated assessment of a fictional organisation under a hypothetical statement of work, and not an executed client commission. Each exercise may have used different permissions, AWS accounts, resource names and starting conditions; they must **not** be represented as one interconnected estate.

Primary source: user-supplied lab briefs 1–14, covering IAM reconnaissance and escalation, IAM/S3/EC2 hardening, CloudTrail and GuardDuty investigations, S3 object access, secrets exposure, SSRF/IMDS, EBS snapshot sharing and a multi-step IAM capstone.

**Not supplied:** unredacted or redacted command results, AWS account inventory, CloudTrail event IDs, screenshot evidence, GuardDuty findings, permission-policy JSON from an actual execution or evidence of passed remediation checks. The [evidence register](../appendices/evidence-register.md) lists missing corroboration.

### Assessment method

See [Scope and Rules of Engagement](scope-and-roe.md) and [Methodology](methodology.md). The proposed approach is: scope and authorise; identify starting principals; inventory IAM and resources; inspect trust and sensitive data-access controls; construct attack hypotheses; validate only in authorised labs; correlate the resulting activity with audit records; prescribe targeted remediation and perform a retest.

## 3. Consolidated risk register

| ID | Finding | Indicative risk | Root cause | Lab references | Verification |
| --- | --- | --- | --- | --- | --- |
| [F-001](../findings/F-001-self-modifying-iam-policy.md) | Self-editable IAM managed policy enables privilege escalation | Critical | Excessive IAM policy-management rights | 2, 5, 14 | Pending |
| [F-002](../findings/F-002-passrole-lambda-privilege-escalation.md) | `PassRole` plus Lambda permits execution with a highly privileged role | Critical | Unrestricted privilege delegation | 3, 14 | Pending |
| [F-003](../findings/F-003-break-glass-role-trust.md) | Overly broad administrator role trust | Critical | Poor separation of emergency privileges | 4, 5 | Pending |
| [F-004](../findings/F-004-exposed-reusable-secrets.md) | Reusable credentials exposed in readable configuration | High | Secrets management failure | 1, 8 | Pending |
| [F-005](../findings/F-005-s3-data-exposure.md) | Insufficiently restricted S3 object access and bucket protections | High | Data-access governance failure | 7, 9 | Pending |
| [F-006](../findings/F-006-ssrf-imds-credential-exposure.md) | SSRF enables retrieval of EC2 role credentials through IMDSv1 | Critical | Insecure application egress and metadata controls | 11, 13 | Pending |
| [F-007](../findings/F-007-ebs-snapshot-disclosure.md) | Snapshot sharing exposes retained filesystem data | High | Snapshot access governance failure | 12 | Pending |
| [F-008](../findings/F-008-iam-credential-governance.md) | Overbroad read access, old access keys and weak privilege safeguards | High | Identity lifecycle and least-privilege gaps | 1, 5 | Pending |

These are **eight thematic findings**, not eight independent confirmed vulnerabilities from a single account.

## 4. Attack-path analysis

The [attack-path analysis](attack-paths.md) details two logically separate cases:

**Path A — Managed-policy escalation to protected data (lab 14).** A low-privilege CI identity has managed-policy version control over an attached policy. It changes permissions, enables privileged Lambda execution through a delegable role, assumes a further administrative role and gains access to a protected secret. The key risk is not any single API call in isolation but the unintended combined privilege path.

**Path B — SSRF to EC2 instance-role compromise (lab 11).** An application performs arbitrary server-side URL requests; metadata credentials are reachable via IMDSv1; the role's authorisation permits retrieval of sensitive secrets. IMDSv2 is a meaningful defence-in-depth control against the described GET-only attack but does not substitute for fixing SSRF, network boundaries and IAM permissions.

Neither path is asserted to have been re-executed or verified during preparation of this portfolio.

## 5. Root-cause themes

### A. Identity and delegation governance

IAM policy self-management, privileged compute execution and broad administrator trusts indicate a failure to separate application operations from security administration. Relevant controls include least-privilege IAM, permission boundaries where appropriate, privileged-role isolation, conditions on `iam:PassRole`, and explicit trust principals.

### B. Secrets and data access

Sensitive values in accessible configuration and overly permissive storage controls make ordinary read access disproportionately valuable. Controls should include Secrets Manager or carefully governed encrypted SSM parameters, credential rotation, narrow resource policies and monitored access to sensitive objects.

### C. Compute and application security

The SSRF/IMDS chain requires both an application-layer request primitive and a metadata exposure path. Correct application allowlisting and URL validation, egress protections, IMDSv2 and instance-role minimisation are complementary.

### D. Security operations and recovery

The incident-response exercises involve reconstructing IAM escalation and S3 data-access timelines. Their existence demonstrates learning objectives, **not proof that a SOC detected, alerted on or contained the incidents**. An executable detection test plan is necessary.

## 6. Detection and investigation

Relevant AWS management events include `CreatePolicyVersion`, `SetDefaultPolicyVersion`, `UpdateAssumeRolePolicy`, `CreateFunction`, `AttachRolePolicy`, `AssumeRole` and `CreateAccessKey`. Data access to S3 objects and some other resources needs suitable CloudTrail data-event coverage if the organisation expects to investigate those operations from its own trails.

Detection hypotheses should correlate identity, source IP, request sequence and target resources. GuardDuty findings may support investigations but are **not guaranteed for every action**, nor can missing GuardDuty results alone prove detection failure. The export-only incident lab deliberately contains no live detector.

See [Detection & incident response](detection-and-response.md).

## 7. Remediation approach

Remediation is driven first by plausible impact and exploitability, then by effort and dependencies. Near-term actions reduce the ability of low-trust identities to create privileged sessions; structural work standardises IAM role patterns, secrets handling and preventive policy checks. Change control and application compatibility matter: for example, IMDSv2 enforcement must be tested against software that consumes instance metadata, and blanket S3 public-access restrictions must account for documented legitimate publication requirements.

The [remediation roadmap](remediation-roadmap.md) supplies owners, indicative sequencing, acceptance criteria and retest steps.

## 8. Assessment conclusion

The supplied exercises constitute a substantive foundation for an AWS cloud-security portfolio. Their common lesson is that **authorization paths**, not merely individual misconfigurations, determine the practical blast radius of a compromised identity.

The portfolio becomes a **verified hands-on assessment** only when lab execution traces, sanitised output, findings validation and after-remediation evidence are attached. Until then it should be described as a *scenario-based security assessment and remediation study*, never as a commissioned penetration test or live production audit.
