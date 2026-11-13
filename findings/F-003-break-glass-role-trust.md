# F-003 — Overly broad trust on emergency administrator role

| Attribute | Assessment |
| --- | --- |
| Severity | **Critical — provisional** |
| Classification | Scenario-supported / independently unverified |
| Environment | Isolated AWS training laboratory; anonymised |
| Source exercises | Labs 4, 5 |
| Real client affected | Not established |
| Remediation status | Proposed; not independently verified |

## Observation

An emergency administrator role trusts the entire account via an account-root principal entry, without additional conditions suitable for a privileged break-glass workflow. The supplied lab identity also has permission to call STS AssumeRole.

## Preconditions and exploitation boundaries

Caller has appropriate identity-based AssumeRole authorisation and the role trust permits assumption; SCP, boundaries and conditions do not prevent it.

## Security and business impact

Low-trust in-account principal may obtain emergency administrator privileges outside an incident.

**Risk qualification:** The severity assumes the described lab conditions. A client-specific rating must consider attack complexity, initial access, privilege boundaries, sensitivity of data, compensating controls and business exposure.

## Evidence required

Collect the exact trust policy and caller permission. Record a redacted STS session ARN or failed/successful AssumeRole test conducted within written scope.

**Evidence status:** No command output, policy JSON, event ID, screenshot or completed retest was supplied for this finding. The lab description is a scenario source, **not an observed proof**. Evidence placeholders and sanitisation requirements are maintained in the [evidence register](../appendices/evidence-register.md).

## Recommended remediation

Trust only approved emergency operators or controlled federated roles; require strong authentication and an approved activation process; alert on role assumption; tightly govern session duration and permission boundaries.

## Closure / retest criteria

Unapproved caller is denied AssumeRole; approved emergency workflow remains available and logged.

## Notes and limitations

Provisional: scenario-defined; no runtime output supplied.

## Related material

[Risk register](../report/assessment-report.md) · [Attack paths](../report/attack-paths.md) · [Remediation roadmap](../report/remediation-roadmap.md) · [Lab traceability](../appendices/lab-traceability.md)
