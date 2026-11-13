# Assessment Methodology

The source exercises use four different modes: reconnaissance, exploitation, remediation and incident investigation. The same method applies across them, with permissions adjusted for the particular mode.

## 1. Initial access and inventory

Identify the AWS principal with `sts:GetCallerIdentity`, confirm the allocated region, then inspect the permissions held by the starting identity. Inventory the resources relevant to the test: IAM policies and trusts, Lambda execution roles, EC2 metadata options, S3 access controls and exposed snapshot permissions.

Avoid assuming a principal can perform an operation simply because one policy appears to allow it. Resource policies, service control policies, permission boundaries, session policies and conditions may change the effective result.

## 2. Tool selection

| Tool | Purpose | Access requirement |
| --- | --- | --- |
| AWS CLI | Identity checks and targeted inspection | Selected `List`, `Get` and `Describe` operations |
| CloudFox | IAM and resource mapping; attack-path hypotheses | Read-only AWS access |
| Prowler | Configuration checks against security baselines | Security audit / targeted read permissions |
| ScoutSuite | Cross-service configuration review | Read-only audit permissions |
| Pacu | IAM enumeration and approved privilege-escalation testing | Reconnaissance permissions; additional mutations only in designated labs |
| CloudTrail / GuardDuty | Event analysis and investigative context | Read access to configured logs, findings or exports |

The supplied exercises describe AWS CLI actions. The tools above form an appropriate assessment plan; their inclusion does not indicate that a scanner was run during compilation.

## 3. Privilege-path analysis

For each identity, inspect the permissions it holds and the other permissions it can **cause a service or role to exercise**. This is particularly relevant to:

- Managed-policy version control on an attached IAM policy.
- `iam:PassRole` combined with service creation and invocation.
- Role trusts that admit a broader set of principals than intended.
- Credentials exposed through metadata or configuration.
- EC2 instance roles reachable through application flaws and IMDS.

An attack path is assessed as a whole; the existence of an isolated permission is insufficient.

## 4. Data-access review

Test whether sensitive objects, historical versions and backup snapshots are reachable by identities without a business requirement. Inspect IAM policy, resource policy, ACL/public-access settings and KMS constraints where they affect the outcome.

For storage controls, distinguish confidentiality failures from hardening or availability gaps. The absence of SSE-KMS, for example, does not mean new S3 objects are unencrypted.

## 5. Detection and remediation

Use management events to build IAM and role-assumption timelines. When the investigation concerns S3 object reads, establish whether object-level data events were recorded. Compare the intended detection coverage with the logs actually available.

For each correction, record the original condition, the policy or configuration change, a negative test against the original unwanted behaviour and a positive test for expected application operation.

## 6. Reporting standard

Findings include a specific control failure, the conditions under which it matters, the resulting access or impact, and a checkable remediation. Several exercises share the same root cause; duplicate symptoms are consolidated rather than presented as separate security incidents.
