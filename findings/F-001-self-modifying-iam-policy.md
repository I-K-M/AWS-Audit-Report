# F-001 — IAM managed-policy self-modification enables privilege escalation

| Attribute | Assessment |
| --- | --- |
| Severity | **Critical — provisional** |
| Classification | Scenario-supported / independently unverified |
| Environment | Isolated AWS training laboratory; anonymised |
| Source exercises | Labs 2, 5, 14 |
| Real client affected | Not established |
| Remediation status | Proposed; not independently verified |

## Observation

An application or CI principal is permitted to create and activate versions of a customer-managed IAM policy that is attached to the same principal. A new permissive default version can materially broaden that principal's effective permissions.

## Preconditions and exploitation boundaries

Pre-existing access to a principal with policy-version management rights; editable attached managed policy; no effective SCP, permission boundary, session policy or resource-level constraint that prevents the resultant access.

## Security and business impact

Unauthorised privilege expansion, potentially reaching full administration of the affected AWS account.

**Risk qualification:** The severity assumes the described lab conditions. A client-specific rating must consider attack complexity, initial access, privilege boundaries, sensitivity of data, compensating controls and business exposure.

## Evidence required

Obtain the attached policy ARN, active and previous versions and the policy controlling the version-management APIs. Verify whether a bounded test moved a previously denied action to allowed. Retain event IDs for CreatePolicyVersion and SetDefaultPolicyVersion.

**Evidence status:** No command output, policy JSON, event ID, screenshot or completed retest was supplied for this finding. The lab description is a scenario source, **not an observed proof**. Evidence placeholders and sanitisation requirements are maintained in the [evidence register](../appendices/evidence-register.md).

## Recommended remediation

Remove policy-version mutation rights from application and CI principals; centralise policy changes in a tightly governed administration role; add independent approval and policy-as-code controls; consider suitable permission boundaries/SCPs as defence in depth.

## Closure / retest criteria

Original principal cannot create or activate elevated policy versions; a permitted deployment workflow continues to work.

## Notes and limitations

Provisional: scenario-defined; no runtime output supplied.

## Related material

[Risk register](../report/assessment-report.md) · [Attack paths](../report/attack-paths.md) · [Remediation roadmap](../report/remediation-roadmap.md) · [Lab traceability](../appendices/lab-traceability.md)
