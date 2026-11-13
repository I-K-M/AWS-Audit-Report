# F-002 — Privileged AWS Lambda execution through role delegation

| Attribute | Assessment |
| --- | --- |
| Severity | **Critical** (lab scenario) |
| Source | Labs 3, 14 |

## Observation

A deploy identity can pass a role with substantially greater privileges to AWS Lambda while also possessing the relevant Lambda creation and invocation permissions. The Lambda service assumes the execution role; the originating user need not assume it directly.

## Conditions

Deployer can call iam:PassRole for the role and create/invoke a function; execution role trusts lambda.amazonaws.com; attached role permissions permit protected actions. In lab 14, further STS role assumption is also described.

## Impact

Delegated code execution with elevated AWS permissions; possible access to protected secrets or downstream privileged roles.


## Verification

Inspect caller IAM permissions, passable role scope, Lambda execution-role trust and attached policies. Collect sanitised role identity from an expressly authorised benign invocation and associated CloudTrail events.


## Remediation

Restrict iam:PassRole to minimally privileged roles and the expected service; constrain creation/invocation of workloads; separate CI and administrator identities; review privileged execution roles; scrutinise downstream trust.

## Retest

Deployer cannot attach a prohibited privileged role to a Lambda while authorised low-privilege deployments still succeed.

## Context

The exercise describes the exposure; supporting API output was not included in the source material.
