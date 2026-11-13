# Risk-Based Remediation Roadmap

**Purpose:** Translate the independent laboratory findings into prioritised security-engineering actions. The timings below are illustrative planning bands, **not client-approved SLAs**.

## Priority matrix

| Sequence | Control outcome | Findings | Indicative effort | Accountable function | Closure evidence |
| --- | --- | --- | --- | --- | --- |
| P0 — Immediate containment where confirmed | Remove IAM self-escalation and broad privileged-role delegation | F-001, F-002, F-003 | Low–Medium | IAM / Platform Security | Negative privilege test and approved-role tests succeed |
| P0 — Confirmed credential exposure | Revoke/rotate compromised reusable credentials; assess access logs | F-004, F-008 | Low–Medium | Identity / Incident Response | Old access no longer authenticates, workload restored on new identity |
| P0 — Confirmed data exposure | Restrict unintended S3 and EBS access; preserve incident evidence | F-005, F-007 | Medium | Data Platform / Security | Unauthorised principal cannot access; expected use remains available |
| P1 — Application and instance defence | Remediate SSRF, enforce IMDSv2 and limit EC2 instance role | F-006 | Medium | Application / Compute | SSRF retest fails; metadata and application regression checks pass |
| P1 — Storage baseline | Align bucket policies, Block Public Access, TLS, versioning and key governance to data needs | F-005 | Medium | Storage / Platform | Configuration checks and recovery tests pass |
| P2 — Foundational IAM governance | Standardise deployment roles, break-glass access, access-key lifecycle and permission review | F-001–F-004, F-008 | High | Cloud Security / Platform | Policy-as-code checks, scheduled reviews and access tests |
| P2 — Detection engineering | Ensure selected CloudTrail management/data-event visibility and approved high-signal alerts | Cross-cutting | Medium–High | Security Operations | Controlled detection exercise and response evidence |

## High-value immediate actions

**IAM self-management:** Remove `iam:CreatePolicyVersion` and `iam:SetDefaultPolicyVersion` from application roles/users that use the target policy. Test from the same initial identity to confirm the escalation path is blocked.

**Emergency role trust:** Replace a whole-account principal where not genuinely required with specific trusted principals, governed conditions and appropriate privileged-access controls. Verify legitimate emergency use still works.

**Credentials:** Disable confirmed compromised keys promptly and migrate long-lived CI secrets to short-lived federation where practical. Age alone is a review signal, not conclusive evidence of credential compromise.

## Structural remediation

- Define role patterns that distinguish deploy permissions, identity administration and execution runtime privileges.
- Combine an appropriate `iam:PassRole` resource restriction with valid service-specific conditions and explicit control of service creation/invocation.
- Use policy-as-code testing and review IAM trust relationships for unintended cross-account, service and emergency access.
- Manage secrets through dedicated protected services; monitor and prevent hard-coded sensitive values in configuration and CI.
- Apply defence-in-depth to URL-fetch features: allowlists, URL parsing, DNS/redirect revalidation and network egress controls.
- Standardise S3 and snapshot policy reviews. S3 encrypts new objects at rest by default using SSE-S3; a requirement for SSE-KMS should be presented as a **key-governance/compliance decision**, not as proof that objects otherwise remain unencrypted.
- Preserve source evidence before making security-relevant changes and retest negative and positive access cases.

## Acceptance criteria

A finding may be closed only when (a) the original exploit prerequisite is demonstrably removed; (b) a retest using the previous initial access does not reproduce the issue; (c) the intended service functionality remains operational; and (d) the change and its evidence are traceable to an owner and a review date.

## Caveats

The repository does not claim any of these changes were made to a live system. Labs 5, 9 and 13 **describe hardening tasks**, but no completion screenshots, compliance-check outputs or before/after JSON were supplied. Their final remediation state therefore remains **unverified in this report**.
