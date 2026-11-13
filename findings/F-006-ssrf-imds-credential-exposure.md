# F-006 — Application SSRF exposes EC2 role credentials via IMDSv1

| Attribute | Assessment |
| --- | --- |
| Severity | **Critical** (lab scenario) |
| Source | Labs 11, 13 |

## Observation

An EC2-hosted application accepts an untrusted URL and fetches it server-side. With IMDSv1 still accepted, a GET-only SSRF can reach instance metadata and retrieve temporary IAM role credentials. The attached role can access a lab secret.

## Conditions

User-controlled server-side request to an internal/link-local destination; metadata endpoint reachable; IMDSv1 accepted (HttpTokens optional); application response exposes content; EC2 role has target permission.

## Impact

Compromise of EC2 role credentials and access to secrets or other AWS resources within the role's permissions.


## Verification

Capture a safe SSRF proof to a controlled destination, original MetadataOptions, sanitised evidence of metadata access, IAM role policy and a lab-only synthetic resource-access result.


## Remediation

Fix server-side URL fetching with robust destination, redirect and network restrictions; enforce IMDSv2 after compatibility tests; minimise instance-role permissions; monitor unusual credential usage.

## Retest

SSRF cannot reach IMDS or other restricted destinations; HttpTokens required; workload remains healthy; original path fails.

## Context

The stated SSRF path depends on the application's request behaviour and the instance role's permissions.
