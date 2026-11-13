# F-005 — Overly broad S3 access and inadequate bucket safeguards

| Attribute | Assessment |
| --- | --- |
| Severity | **High — provisional** |
| Classification | Scenario-supported / independently unverified |
| Environment | Isolated AWS training laboratory; anonymised |
| Source exercises | Labs 7, 9 |
| Real client affected | Not established |
| Remediation status | Proposed; not independently verified |

## Observation

The lab scenarios include object exposure through an overly broad bucket policy or ACL, possible access to retained object versions, disabled public-access safeguards, allowance of insecure transport, and omitted versioning. These issues arise in separate exercises.

## Preconditions and exploitation boundaries

A policy/ACL/IAM permission combination actually grants access to a sensitive object or version; network, encryption, object ownership, SCP/RCP and Block Public Access conditions must be evaluated. A public statement alone does not establish successful public access.

## Security and business impact

Unintended reading or recovery of confidential S3 content; weakened transport and recoverability protections.

**Risk qualification:** The severity assumes the described lab conditions. A client-specific rating must consider attack complexity, initial access, privilege boundaries, sensitivity of data, compensating controls and business exposure.

## Evidence required

Capture redacted bucket policy/ACL, effective Block Public Access status, object version ACL/policy context, authorised access test and the applicable encryption/versioning configuration.

**Evidence status:** No command output, policy JSON, event ID, screenshot or completed retest was supplied for this finding. The lab description is a scenario source, **not an observed proof**. Evidence placeholders and sanitisation requirements are maintained in the [evidence register](../appendices/evidence-register.md).

## Recommended remediation

Apply least-privilege resource and identity policies; enable Block Public Access unless publication is explicitly required; prohibit insecure transport; govern object version access; use versioning and key management aligned with recovery/compliance requirements.

## Closure / retest criteria

Unauthorised principal cannot retrieve current or historical sensitive objects; legitimate access works; required encryption, TLS and recovery settings are compliant.

## Notes and limitations

Provisional. S3 already encrypts new objects at rest by default with SSE-S3: absent SSE-KMS is not the same as unencrypted storage.

## Related material

[Risk register](../report/assessment-report.md) · [Attack paths](../report/attack-paths.md) · [Remediation roadmap](../report/remediation-roadmap.md) · [Lab traceability](../appendices/lab-traceability.md)
