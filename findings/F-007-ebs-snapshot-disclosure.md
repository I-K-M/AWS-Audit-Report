# F-007 — Shared EBS snapshot permits unintended filesystem access

| Attribute | Assessment |
| --- | --- |
| Severity | **High** (lab scenario) |
| Source | Labs 12 |

## Observation

An EBS snapshot containing filesystem secrets is shared with a principal that can discover it and restore or copy it to a volume. The snapshot's permissions and the principal's EBS privileges enable a copy of retained data.

## Conditions

Snapshot restoration permission for unintended principal; permission to create/copy volume and access resulting filesystem; encryption/KMS key sharing conditions allow the operation.

## Impact

Disclosure of retained secrets, configuration files and other historical filesystem content.


## Verification

Record snapshot ownership and sharing attributes, encryption/key-policy state, redacted volume restoration steps and a benign synthetic-marker read from an authorised exercise.


## Remediation

Remove unnecessary snapshot shares, enforce review of CreateVolumePermission changes, use restrictive KMS key policies, avoid secrets in images and backups, monitor unusual snapshot sharing/copy activity.

## Retest

Unapproved principal cannot restore/copy protected snapshot; authorised restore and recovery still work.

## Context

Provisional: scenario-defined; no filesystem output supplied.
