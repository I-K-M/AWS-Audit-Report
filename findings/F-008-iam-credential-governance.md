# F-008 — Insufficient IAM least privilege, credential hygiene and MFA safeguards

| Attribute | Assessment |
| --- | --- |
| Severity | **High** (lab scenario) |
| Source | Labs 1, 5 |

## Observation

A nominally low-privilege account can enumerate broad IAM metadata; a separate hardening lab includes an application policy with Action/Resource wildcards, an old IAM access key, and missing MFA controls for sensitive operations. These are independent governance weaknesses rather than one confirmed identity.

## Conditions

Overbroad policies remain attached to operational identities; long-lived keys lack business justification or monitored rotation; interactive sensitive operations are not strongly authenticated when required.

## Impact

Greater reconnaissance visibility, long-lived credential exposure window and larger potential blast radius following identity compromise.


## Verification

Collect effective IAM permissions, policy versions, access-key last-used metadata where available and the actual authentication/federation model. Determine whether sensitive operations use MFA, an IdP or workloads with non-interactive roles.


## Remediation

Reduce broad IAM reads and wildcard policies, migrate CI to federated short-lived credentials where feasible, deactivate unused keys, protect privileged human activity with strong authentication and add regular access reviews.

## Retest

Least-privilege tests pass for expected tasks; obsolete credentials are inactive; privileged workflows meet authentication requirements.

## Context

A key older than 90 days warrants review; its age alone does not establish compromise.
