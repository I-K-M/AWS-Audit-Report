# Scope and Rules of Engagement — Demonstration Extract

**Status:** Illustrative template only. **Not signed, authorised or binding.**  
**Target:** Fictional organisation and independent, disposable AWS training accounts.

## Engagement purpose

Assess defined AWS IAM, storage, EC2 and logging security controls; validate prescribed attack paths in lab environments; document findings and candidate remediation. The purpose is educational portfolio work rather than a commissioned penetration test.

## Authorisation and responsibility

A real engagement must have an executed statement of work and written authorisation from a person entitled to authorise testing of every affected AWS account and relevant third-party asset. AWS's applicable penetration-testing policy must also be checked. The laboratory platform's explicit terms control any hands-on exercises.

## In-scope assets

| Dimension | Demonstration scope |
| --- | --- |
| AWS accounts | Only the disposable account assigned to each lab at deployment; no real account IDs supplied |
| Regions | Only the region allocated to that lab; no cross-region discovery without authorisation |
| Identities | Lab-provided temporary users/roles and explicitly reachable resources |
| Services | IAM, STS, Lambda, EC2/IMDS, S3, EBS, SSM, Secrets Manager, CloudTrail and GuardDuty where present |
| Activity | Read-only enumeration by default; narrowly defined mutating actions solely where that lab explicitly requires them |
| Time | The individual lab's TTL and permitted window; never presumed to be permanent |
| Data | Synthetic challenge artefacts only; minimise collection |

## Explicit exclusions

No activity against other customers' AWS accounts, cloud infrastructure outside the allocated lab, unmanaged public services, live organisations or third-party systems. No denial-of-service, persistence outside explicit training exercises, covert log alteration, uncontrolled credential reuse, production modifications or extraction of genuine personal/customer data.

## Operating controls

- Verify the AWS principal and region before work.
- Maintain a time-stamped testing log and preserve exact commands where non-sensitive.
- Stop immediately if activity unexpectedly crosses the scope or reveals actual personal data, live credentials or shared third-party infrastructure.
- Never run generated `loot` commands, invasive Pacu modules or mutation operations without reviewing their effect and confirming they are permitted.
- Use synthetic markers to prove access; do not collect unnecessary sensitive content.
- Make no changes to CloudTrail, GuardDuty or evidence repositories without separately and explicitly authorised scope.
- Keep test credentials in an approved local secret store, do not commit them, and revoke or discard them when the lab expires.

## Escalation and stop conditions

In a real engagement, the signed RoE must name the authorised client representative, the technical contact and 24-hour incident contact, including a communication route, pause criteria, approval process for out-of-scope discoveries and restoration owner. For these scenario briefs **no names, contacts or signed authority are available**; therefore this document cannot itself authorise testing.

## Evidence, privacy and retention

Sanitise AWS account identifiers, session credentials, personal details, secret values, hostnames and IP addresses. Retain only the minimum artefacts needed for validation and delete source materials according to the governing engagement agreement. Do not place raw production logs or real secrets in this public repository.

## Completion criteria

Assessment completion requires scope compliance, findings traceability, documented limitations, risk-based remediation and an agreed procedure for retesting. This repository currently documents a **demonstration framework**, not completed contractual sign-off.
