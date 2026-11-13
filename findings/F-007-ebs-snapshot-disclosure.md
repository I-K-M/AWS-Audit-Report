# F-007 — Shared EBS snapshot permits unintended filesystem access

| Attribute | Assessment |
| --- | --- |
| Severity | **High — provisional** |
| Classification | Scenario-supported / independently unverified |
| Environment | Isolated AWS training laboratory; anonymised |
| Source exercises | Labs 12 |
| Real client affected | Not established |
| Remediation status | Proposed; not independently verified |

## Observation

An EBS snapshot containing filesystem secrets is shared with a principal that can discover it and restore or copy it to a volume. The snapshot's permissions and the principal's EBS privileges enable a copy of retained data.

## Preconditions and exploitation boundaries

Snapshot restoration permission for unintended principal; permission to create/copy volume and access resulting filesystem; encryption/KMS key sharing conditions allow the operation.

## Security and business impact

Disclosure of retained secrets, configuration files and other historical filesystem content.

**Risk qualification:** The severity assumes the described lab conditions. A client-specific rating must consider attack complexity, initial access, privilege boundaries, sensitivity of data, compensating controls and business exposure.

## Evidence required

Record snapshot ownership and sharing attributes, encryption/key-policy state, redacted volume restoration steps and a benign synthetic-marker read from an authorised exercise.

**Evidence status:** No command output, policy JSON, event ID, screenshot or completed retest was supplied for this finding. The lab description is a scenario source, **not an observed proof**. Evidence placeholders and sanitisation requirements are maintained in the [evidence register](../appendices/evidence-register.md).

## Recommended remediation

Remove unnecessary snapshot shares, enforce review of CreateVolumePermission changes, use restrictive KMS key policies, avoid secrets in images and backups, monitor unusual snapshot sharing/copy activity.

## Closure / retest criteria

Unapproved principal cannot restore/copy protected snapshot; authorised restore and recovery still work.

## Notes and limitations

Provisional: scenario-defined; no filesystem output supplied.

## Related material

[Risk register](../report/assessment-report.md) · [Attack paths](../report/attack-paths.md) · [Remediation roadmap](../report/remediation-roadmap.md) · [Lab traceability](../appendices/lab-traceability.md)
