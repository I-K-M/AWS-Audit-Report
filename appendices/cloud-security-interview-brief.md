# Cloud Security Interview Brief — AWS

**Purpose:** Concise professional revision sheet covering the concepts represented by the portfolio. Responses are written for a Cloud Security / DevSecOps interview.

## Ten technical questions

| Question | Concise answer |
| --- | --- |
| **1. How is AWS IAM access evaluated?** | Identity-based and resource-based policies interact with trust and session policies, permission boundaries and organisational guardrails where applicable. An explicit Deny overrides an Allow; absence of a required Allow normally denies the action. |
| **2. What distinguishes IMDSv2 from IMDSv1?** | IMDSv2 requires a token obtained via an HTTP PUT request, reducing exposure to some SSRF patterns. It is defence in depth, not a replacement for fixing SSRF or restricting instance-role privileges. |
| **3. Why does `iam:PassRole` matter?** | It permits an identity to assign an IAM role to a compatible AWS service. Combined with service creation or modification, this may let a weaker principal run a workload with greater privileges. The role must also trust that service. |
| **4. Give an AWS IAM privilege escalation example.** | A CI identity that can create and activate a new version of its own attached managed policy may widen its permissions. The correct fix is separating policy administration from operational identities and testing the effective permissions. |
| **5. How would you investigate unexpected AWS administrative activity?** | Establish the timeline from CloudTrail management events, follow principal and STS session context, correlate source IP and affected resources, examine downstream changes, and check GuardDuty and SIEM findings without assuming an alert always exists. |
| **6. What is an SCP?** | A Service Control Policy is an AWS Organizations permissions guardrail for principals in member accounts. It limits maximum permissions but does not grant access. The management account is outside normal SCP restrictions. |
| **7. Audit of posture versus penetration test?** | A posture assessment evaluates resource configuration and control alignment; a penetration test demonstrates exploitability and impact within explicit rules of engagement. Both require evidence and practical remediations. |
| **8. How would you secure sensitive S3 data?** | Least-privilege identity and bucket policies, appropriate Block Public Access settings, enforced TLS, encryption/key controls, versioning/recovery, visibility of object-level activity where required, and periodic access review. |
| **9. What makes an AWS security test authorised?** | Written authority from an entitled party, clearly scoped accounts, regions, services, methods and timing, plus stop conditions, emergency contacts, confidentiality safeguards and compliance with applicable AWS testing rules. |
| **10. How should findings be prioritised?** | Validate factual exposure, assess likelihood and business consequence, consider attack paths and blast radius, then order action by urgency and effort. Group by root cause and define retest/acceptance criteria; an easy fix does not outrank a confirmed severe issue by default. |

## Tool reference

| Tool | Principal role | Typical access | When |
| --- | --- | --- | --- |
| AWS CLI | Identity confirmation and targeted verification | Targeted read; mutations only when approved | Throughout |
| CloudFox | Offensive-oriented discovery and trust mapping | Read-only | Early hypothesis generation |
| Pacu | Permission and exploit-path testing | Potentially mutating | Bounded proof-of-impact phase |
| Prowler | Benchmark and configuration posture checks | Read-oriented audit | Broad baseline review |
| ScoutSuite | Multi-service posture visualisation | Read-only audit | Configuration exploration |
| CloudTrail | API evidence and timeline reconstruction | Log-read | Investigation and retest |
| GuardDuty | Managed threat-detection findings | Findings-read | Detection review and incident triage |

## Certification direction

**Target role:** AWS Cloud Security Engineer / DevSecOps Engineer, with IAM governance, detection and secure infrastructure delivery responsibilities.

**Primary medium-term target:** AWS Certified Security – Specialty, following hands-on practice in IAM, networking, logging, encryption and incident response. If core AWS architecture experience is insufficient, AWS Solutions Architect – Associate is a sensible stepping stone.

**Portfolio complement:** Maintain an evidence-backed AWS assessment, a defensive IAM/STS lab and an infrastructure-as-code security project. Certifications demonstrate knowledge; specific technical artefacts and reproducible remediation work demonstrate application.

This is a **proposed career plan**, not a claim that any certification has been earned.
