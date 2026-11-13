# F-004 — Reusable credentials exposed through readable configuration

| Attribute | Assessment |
| --- | --- |
| Severity | **High** (lab scenario) |
| Source | Labs 1, 8 |

## Observation

A training secret is placed in a readable IAM/resource metadata field, Lambda environment configuration or a plaintext SSM String parameter, and may act as a credential for a second access context. The exact exposure surface varies by lab instance.

## Conditions

A principal can read the relevant configuration or resource metadata and the stored value remains valid and reusable. A Lambda environment variable is protected at rest by Lambda encryption, but values may be visible to authorised configuration readers.

## Impact

Leakage of application credentials and potential secondary-account or service compromise.


## Verification

Collect sanitised configuration showing location and exposure category, IAM Get/List access policy, and a non-sensitive proof of whether a second identity was reachable. Never publish the secret itself.


## Remediation

Use Secrets Manager or protected SSM SecureString according to the use case; restrict GetParameter/GetFunctionConfiguration/metadata access; avoid long-lived reusable credentials; rotate any exposed secret.

## Retest

The initial identity cannot read sensitive values; affected old credentials fail; applications function with the replacement access method.

## Context

Lab 8 uses alternative secret locations, depending on the deployed exercise.
