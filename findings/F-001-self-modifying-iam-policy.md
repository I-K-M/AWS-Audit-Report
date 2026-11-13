# F-001 — IAM managed-policy self-modification enables privilege escalation

| Attribute | Assessment |
| --- | --- |
| Severity | **Critical** (lab scenario) |
| Source | Labs 2, 5, 14 |

## Observation

An application or CI principal is permitted to create and activate versions of a customer-managed IAM policy that is attached to the same principal. A new permissive default version can materially broaden that principal's effective permissions.

## Conditions

Pre-existing access to a principal with policy-version management rights; editable attached managed policy; no effective SCP, permission boundary, session policy or resource-level constraint that prevents the resultant access.

## Impact

Unauthorised privilege expansion, potentially reaching full administration of the affected AWS account.


## Verification

Obtain the attached policy ARN, active and previous versions and the policy controlling the version-management APIs. Verify whether a bounded test moved a previously denied action to allowed. Retain event IDs for CreatePolicyVersion and SetDefaultPolicyVersion.


## Remediation

Remove policy-version mutation rights from application and CI principals; centralise policy changes in a tightly governed administration role; add independent approval and policy-as-code controls; consider suitable permission boundaries/SCPs as defence in depth.

## Retest

Original principal cannot create or activate elevated policy versions; a permitted deployment workflow continues to work.

## Context

The exercise describes the exposure; supporting API output was not included in the source material.
