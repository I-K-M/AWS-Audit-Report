# F-006 — Application SSRF exposes EC2 role credentials via IMDSv1

| Attribute | Assessment |
| --- | --- |
| Severity | **Critical — provisional** |
| Classification | Scenario-supported / independently unverified |
| Environment | Isolated AWS training laboratory; anonymised |
| Source exercises | Labs 11, 13 |
| Real client affected | Not established |
| Remediation status | Proposed; not independently verified |

## Observation

An EC2-hosted application accepts an untrusted URL and fetches it server-side. With IMDSv1 still accepted, a GET-only SSRF can reach instance metadata and retrieve temporary IAM role credentials. The attached role can access a lab secret.

## Preconditions and exploitation boundaries

User-controlled server-side request to an internal/link-local destination; metadata endpoint reachable; IMDSv1 accepted (HttpTokens optional); application response exposes content; EC2 role has target permission.

## Security and business impact

Compromise of EC2 role credentials and access to secrets or other AWS resources within the role's permissions.

**Risk qualification:** The severity assumes the described lab conditions. A client-specific rating must consider attack complexity, initial access, privilege boundaries, sensitivity of data, compensating controls and business exposure.

## Evidence required

Capture a safe SSRF proof to a controlled destination, original MetadataOptions, sanitised evidence of metadata access, IAM role policy and a lab-only synthetic resource-access result.

**Evidence status:** No command output, policy JSON, event ID, screenshot or completed retest was supplied for this finding. The lab description is a scenario source, **not an observed proof**. Evidence placeholders and sanitisation requirements are maintained in the [evidence register](../appendices/evidence-register.md).

## Recommended remediation

Fix server-side URL fetching with robust destination, redirect and network restrictions; enforce IMDSv2 after compatibility tests; minimise instance-role permissions; monitor unusual credential usage.

## Closure / retest criteria

SSRF cannot reach IMDS or other restricted destinations; HttpTokens required; workload remains healthy; original path fails.

## Notes and limitations

Provisional: lab-defined, no direct test transcript supplied.

## Related material

[Risk register](../report/assessment-report.md) · [Attack paths](../report/attack-paths.md) · [Remediation roadmap](../report/remediation-roadmap.md) · [Lab traceability](../appendices/lab-traceability.md)
