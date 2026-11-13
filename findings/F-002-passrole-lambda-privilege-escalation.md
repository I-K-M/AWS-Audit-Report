# F-002 — Privileged AWS Lambda execution through role delegation

| Attribute | Assessment |
| --- | --- |
| Severity | **Critical — provisional** |
| Classification | Scenario-supported / independently unverified |
| Environment | Isolated AWS training laboratory; anonymised |
| Source exercises | Labs 3, 14 |
| Real client affected | Not established |
| Remediation status | Proposed; not independently verified |

## Observation

A deploy identity can pass a role with substantially greater privileges to AWS Lambda while also possessing the relevant Lambda creation and invocation permissions. The Lambda service assumes the execution role; the originating user need not assume it directly.

## Preconditions and exploitation boundaries

Deployer can call iam:PassRole for the role and create/invoke a function; execution role trusts lambda.amazonaws.com; attached role permissions permit protected actions. In lab 14, further STS role assumption is also described.

## Security and business impact

Delegated code execution with elevated AWS permissions; possible access to protected secrets or downstream privileged roles.

**Risk qualification:** The severity assumes the described lab conditions. A client-specific rating must consider attack complexity, initial access, privilege boundaries, sensitivity of data, compensating controls and business exposure.

## Evidence required

Inspect caller IAM permissions, passable role scope, Lambda execution-role trust and attached policies. Collect sanitised role identity from an expressly authorised benign invocation and associated CloudTrail events.

**Evidence status:** No command output, policy JSON, event ID, screenshot or completed retest was supplied for this finding. The lab description is a scenario source, **not an observed proof**. Evidence placeholders and sanitisation requirements are maintained in the [evidence register](../appendices/evidence-register.md).

## Recommended remediation

Restrict iam:PassRole to minimally privileged roles and the expected service; constrain creation/invocation of workloads; separate CI and administrator identities; review privileged execution roles; scrutinise downstream trust.

## Closure / retest criteria

Deployer cannot attach a prohibited privileged role to a Lambda while authorised low-privilege deployments still succeed.

## Notes and limitations

Provisional: scenario-defined; no runtime output supplied.

## Related material

[Risk register](../report/assessment-report.md) · [Attack paths](../report/attack-paths.md) · [Remediation roadmap](../report/remediation-roadmap.md) · [Lab traceability](../appendices/lab-traceability.md)
