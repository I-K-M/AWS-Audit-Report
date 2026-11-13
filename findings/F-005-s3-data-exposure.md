# F-005 — Overly broad S3 access and inadequate bucket safeguards

| Attribute | Assessment |
| --- | --- |
| Severity | **High** (lab scenario) |
| Source | Labs 7, 9 |

## Observation

The lab scenarios include object exposure through an overly broad bucket policy or ACL, possible access to retained object versions, disabled public-access safeguards, allowance of insecure transport, and omitted versioning. These issues arise in separate exercises.

## Conditions

A policy/ACL/IAM permission combination actually grants access to a sensitive object or version; network, encryption, object ownership, SCP/RCP and Block Public Access conditions must be evaluated. A public statement alone does not establish successful public access.

## Impact

Unintended reading or recovery of confidential S3 content; weakened transport and recoverability protections.


## Verification

Capture redacted bucket policy/ACL, effective Block Public Access status, object version ACL/policy context, authorised access test and the applicable encryption/versioning configuration.


## Remediation

Apply least-privilege resource and identity policies; enable Block Public Access unless publication is explicitly required; prohibit insecure transport; govern object version access; use versioning and key management aligned with recovery/compliance requirements.

## Retest

Unauthorised principal cannot retrieve current or historical sensitive objects; legitimate access works; required encryption, TLS and recovery settings are compliant.

## Context

S3 encrypts new objects by default using SSE-S3. A missing SSE-KMS policy is a key-management issue, not evidence of unencrypted storage.
