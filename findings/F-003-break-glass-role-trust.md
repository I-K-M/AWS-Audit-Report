# F-003 — Overly broad trust on emergency administrator role

| Attribute | Assessment |
| --- | --- |
| Severity | **Critical** (lab scenario) |
| Source | Labs 4, 5 |

## Observation

An emergency administrator role trusts the entire account via an account-root principal entry, without additional conditions suitable for a privileged break-glass workflow. The supplied lab identity also has permission to call STS AssumeRole.

## Conditions

Caller has appropriate identity-based AssumeRole authorisation and the role trust permits assumption; SCP, boundaries and conditions do not prevent it.

## Impact

Low-trust in-account principal may obtain emergency administrator privileges outside an incident.


## Verification

Collect the exact trust policy and caller permission. Record a redacted STS session ARN or failed/successful AssumeRole test conducted within written scope.


## Remediation

Trust only approved emergency operators or controlled federated roles; require strong authentication and an approved activation process; alert on role assumption; tightly govern session duration and permission boundaries.

## Retest

Unapproved caller is denied AssumeRole; approved emergency workflow remains available and logged.

## Context

The exercise describes the exposure; supporting API output was not included in the source material.
