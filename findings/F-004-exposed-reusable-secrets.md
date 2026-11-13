# F-004 — Reusable credentials exposed through readable configuration

| Attribute | Assessment |
| --- | --- |
| Severity | **High — provisional** |
| Classification | Scenario-supported / independently unverified |
| Environment | Isolated AWS training laboratory; anonymised |
| Source exercises | Labs 1, 8 |
| Real client affected | Not established |
| Remediation status | Proposed; not independently verified |

## Observation

A training secret is placed in a readable IAM/resource metadata field, Lambda environment configuration or a plaintext SSM String parameter, and may act as a credential for a second access context. The exact exposure surface varies by lab instance.

## Preconditions and exploitation boundaries

A principal can read the relevant configuration or resource metadata and the stored value remains valid and reusable. A Lambda environment variable is protected at rest by Lambda encryption, but values may be visible to authorised configuration readers.

## Security and business impact

Leakage of application credentials and potential secondary-account or service compromise.

**Risk qualification:** The severity assumes the described lab conditions. A client-specific rating must consider attack complexity, initial access, privilege boundaries, sensitivity of data, compensating controls and business exposure.

## Evidence required

Collect sanitised configuration showing location and exposure category, IAM Get/List access policy, and a non-sensitive proof of whether a second identity was reachable. Never publish the secret itself.

**Evidence status:** No command output, policy JSON, event ID, screenshot or completed retest was supplied for this finding. The lab description is a scenario source, **not an observed proof**. Evidence placeholders and sanitisation requirements are maintained in the [evidence register](../appendices/evidence-register.md).

## Recommended remediation

Use Secrets Manager or protected SSM SecureString according to the use case; restrict GetParameter/GetFunctionConfiguration/metadata access; avoid long-lived reusable credentials; rotate any exposed secret.

## Closure / retest criteria

The initial identity cannot read sensitive values; affected old credentials fail; applications function with the replacement access method.

## Notes and limitations

Provisional: either/or lab variant; not all storage locations are confirmed in one instance.

## Related material

[Risk register](../report/assessment-report.md) · [Attack paths](../report/attack-paths.md) · [Remediation roadmap](../report/remediation-roadmap.md) · [Lab traceability](../appendices/lab-traceability.md)
