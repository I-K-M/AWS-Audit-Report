# Detection, Investigation and Incident Response

## Objectives

Establish whether AWS security telemetry can reconstruct a suspected privilege-escalation or sensitive-data access sequence, whether alerts are generated when expected, and whether analysts can identify the principal, source, target and subsequent activity.

## Relevant telemetry

| Hypothesis | Candidate event / data | Qualification |
| --- | --- | --- |
| IAM policy self-escalation | `CreatePolicyVersion`, `SetDefaultPolicyVersion` | Event and resulting policy version must be inspected; event alone does not prove administrative access |
| Unexpected privilege grants | `AttachUserPolicy`, `PutUserPolicy`, `UpdateAssumeRolePolicy` | Compare requester and target with approved changes |
| Privileged workload launch | `CreateFunction`, `UpdateFunctionConfiguration` | Correlate execution role, creation actor and actual invoked principal |
| Cross-role activity | `AssumeRole`, `sourceIdentity` where set | Validate cross-account and temporal links; avoid treating an arbitrary source identity string as attested |
| Credential persistence | `CreateAccessKey` | Investigate whether access key was authorised and used |
| S3 object extraction | S3 object-level data events such as `GetObject` | Requires appropriate data-event collection in the organisation's own CloudTrail stores |
| Secret access | `GetSecretValue` where available in relevant telemetry | Sensitive response values should never be recorded in the public case study |
| KMS key use | `Decrypt` events where logged | Context and service-initiated activity matter |
| EBS snapshot access | Snapshot attributes, sharing changes, copying/volume creation events | Correlate permission grants with downstream activity |

CloudTrail **Event history** and retained management events do not by themselves guarantee object-level S3 evidence. GuardDuty may use sources that do not rely upon a customer-configured S3 data-event trail. AWS telemetry coverage must be checked explicitly.

## Incident exercises represented

**Lab 6:** Investigator has read-only CloudTrail, GuardDuty and IAM access to trace an IAM escalation and the actions following it. This scenario calls for actor attribution, source-IP review, timeline construction and assessment of any follow-on persistence.

**Lab 10:** Investigator analyses **exported JSON files** representing GuardDuty findings and CloudTrail management/S3 data events. The supplied lab description specifically states that **no live GuardDuty detector or CloudTrail Lake event data store is present**. A missing detector here is therefore an expected exercise condition, not a security finding.

## Detection-validation procedure

1. Record the approved test identifier, exact time window and originating identity.
2. Verify which event sources and resource selectors were enabled before the test.
3. Locate events and their identifiers; correlate role sessions, API errors and source IPs.
4. Check whether an alert or GuardDuty finding was generated; record delays and severity where observable.
5. Review alert routing, analyst triage and documented response.
6. Distinguish **no telemetry**, **telemetry without configured alert**, **alert without response**, and **detection outside expected coverage**.

**No detection metrics or mean time to detect/respond are claimed in this repository**, because the corresponding operational logs and alert-handling records were not provided.

## Response guidance

For a credible IAM escalation, preserve logs and relevant policy versions; contain compromised principals and sessions using AWS-supported techniques; inspect downstream role assumptions and resource access; remove unauthorised policy versions or trust changes; revoke or rotate affected credentials; then retest with the original permissions matrix.

For a suspected S3 or EBS disclosure, preserve evidence, stop unintended sharing, identify objects or volumes accessible to the compromised principal, and handle any real-data exposure under the applicable incident-response and privacy processes.

For SSRF/IMDS, fix the request-forwarding vulnerability, require IMDSv2 where supported, review the instance-role permission set, investigate unusual use of instance credentials and remove unneeded privilege.

## Reportable conclusions

Detection performance is a separate finding **only if supported by a declared detection objective and observed test outcome**. A scan that triggers no GuardDuty finding is not, on its own, evidence of deficient cloud monitoring.
