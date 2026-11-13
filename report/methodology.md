# Assessment Methodology and Tooling

## Approach

The conceptual workflow is: **authorise → identify → enumerate → analyse trust and permissions → validate a bounded hypothesis → inspect detection coverage → report → remediate → retest**.

Independent lab briefs are analysed separately. Their permissions and findings must never be assumed to coexist in a single AWS account.

## Tool selection and IAM requirements

| Tool | Purpose | Preferred permissions | Boundaries |
| --- | --- | --- | --- |
| AWS CLI / STS | Confirm identity, inspect individual resources, capture reproducible outputs | Targeted `sts:GetCallerIdentity`, required `Get/List/Describe` | No mutation unless expressly authorised |
| Prowler | Structured posture and benchmark checks | Dedicated security-audit role with tool-specific read permissions | Findings require validation; scope data-readable APIs carefully |
| ScoutSuite | Cross-service inventory and visual configuration review | Read-only audit profile, least privilege | Results may be incomplete if APIs are denied |
| CloudFox | Offensive-oriented IAM, trust and asset enumeration | Targeted read-only permissions | Generated follow-up commands must be reviewed; not proof of exploitability |
| Pacu | IAM permission analysis and approved demonstration of exploit paths | Separate lab-only identity with explicit permissions for the scenario | Potentially mutating modules are high-risk; not run by default |
| CloudTrail / GuardDuty | Investigation and detection review | Audit-role access to logs/findings or supplied sanitised exports | GuardDuty detections are not guaranteed; data-event logging needs coverage |

**Least privilege:** Start with a purpose-built audit role; `SecurityAudit` may be a starting point but can require scoped supplemental permissions. `ReadOnlyAccess` is comparatively broad and can expose sensitive data. Do not request `AdministratorAccess` for a configuration review. Any mutating permissions should be bound to the exact resources and duration approved for a lab test.

## Evidence standard

Each finding should include: resource identifier (sanitised), original configuration, applicable policy and conditions, actual API responses, user/account context, timestamps, whether a test was permitted, and the corrected-state result. Record **directly observed**, **scenario-described**, **inferred** and **not assessed** claims separately.

Proposed verification artefacts: policy documents showing active versions, Lambda role ARN and trust policy, S3 policy and public-access settings, EC2 metadata options, snapshot sharing attributes, CloudTrail event IDs and sanitised JSON result excerpts. No such raw evidence has been supplied for this initial report.

## Severity method

Use a contextual analysis of likelihood and consequence: starting access needed, exploit preconditions, role privileges, data sensitivity, compensating controls, reachable surface and blast radius. The ratings in this repository are conditional, not calculated CVSS scores or actual business impact estimates.

## Testing discipline

Only exercise the minimum test needed to establish an authorised effect. Prefer an intentionally benign marker or a permission-check demonstration to broad data extraction. Capture before/after state and account for eventual consistency of AWS policy changes. Where fixes may interrupt workloads, stage and test them with the application owner.

## Coverage limitations

CloudFox, Prowler, ScoutSuite and Pacu do not deliver a guarantee of completeness. They can miss conditional IAM evaluation effects, relationships across accounts, application-specific paths, limited-visibility resources and tools' version-dependent behaviour. Recheck findings manually against effective permissions, organisational restrictions and resource policies.
