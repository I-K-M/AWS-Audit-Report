# F-008 — Insufficient IAM least privilege, credential hygiene and MFA safeguards

| Attribute | Assessment |
| --- | --- |
| Severity | **High — provisional** |
| Classification | Scenario-supported / independently unverified |
| Environment | Isolated AWS training laboratory; anonymised |
| Source exercises | Labs 1, 5 |
| Real client affected | Not established |
| Remediation status | Proposed; not independently verified |

## Observation

A nominally low-privilege account can enumerate broad IAM metadata; a separate hardening lab includes an application policy with Action/Resource wildcards, an old IAM access key, and missing MFA controls for sensitive operations. These are independent governance weaknesses rather than one confirmed identity.

## Preconditions and exploitation boundaries

Overbroad policies remain attached to operational identities; long-lived keys lack business justification or monitored rotation; interactive sensitive operations are not strongly authenticated when required.

## Security and business impact

Greater reconnaissance visibility, long-lived credential exposure window and larger potential blast radius following identity compromise.

**Risk qualification:** The severity assumes the described lab conditions. A client-specific rating must consider attack complexity, initial access, privilege boundaries, sensitivity of data, compensating controls and business exposure.

## Evidence required

Collect effective IAM permissions, policy versions, access-key last-used metadata where available and the actual authentication/federation model. Determine whether sensitive operations use MFA, an IdP or workloads with non-interactive roles.

**Evidence status:** No command output, policy JSON, event ID, screenshot or completed retest was supplied for this finding. The lab description is a scenario source, **not an observed proof**. Evidence placeholders and sanitisation requirements are maintained in the [evidence register](../appendices/evidence-register.md).

## Recommended remediation

Reduce broad IAM reads and wildcard policies, migrate CI to federated short-lived credentials where feasible, deactivate unused keys, protect privileged human activity with strong authentication and add regular access reviews.

## Closure / retest criteria

Least-privilege tests pass for expected tasks; obsolete credentials are inactive; privileged workflows meet authentication requirements.

## Notes and limitations

Provisional: the 90-day key age is a review threshold, not proof of compromise or a universal AWS requirement.

## Related material

[Risk register](../report/assessment-report.md) · [Attack paths](../report/attack-paths.md) · [Remediation roadmap](../report/remediation-roadmap.md) · [Lab traceability](../appendices/lab-traceability.md)
