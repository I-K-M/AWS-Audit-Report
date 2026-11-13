# Evidence Register and Publication Standard

**Current evidence position:** The report was compiled from the fourteen training-lab briefs shared in the conversation. **No original lab command outputs or execution screenshots were supplied.** All verification fields remain unfilled by design.

## Register

| Evidence ID | Finding / purpose | Artefact required | Source | Status |
| --- | --- | --- | --- | --- |
| EV-001 | F-001 | Attached policy, effective version before/after, relevant CloudTrail event IDs | Lab 2 or 14 | Not provided |
| EV-002 | F-002 | `iam:PassRole` permission, Lambda execution role trust/policy, safe principal identity | Lab 3 or 14 | Not provided |
| EV-003 | F-003 | Emergency role trust, principal's AssumeRole permission, sanitised STS result | Lab 4 | Not provided |
| EV-004 | F-004 | Redacted secret-storage location and safe pivot identity proof | Lab 8 | Not provided |
| EV-005 | F-005 | Bucket policy/ACL/BPA, object version and sanitised access result | Labs 7 and 9 | Not provided |
| EV-006 | F-006 | Safe SSRF evidence, MetadataOptions, principal policy and after-fix retest | Labs 11 and 13 | Not provided |
| EV-007 | F-007 | Snapshot sharing attributes, encryption/KMS context, safe restore proof | Lab 12 | Not provided |
| EV-008 | F-008 | IAM permissions, key last-used metadata, MFA controls, after-hardening policy | Labs 1 and 5 | Not provided |
| EV-009 | Detection | CloudTrail IAM escalation timeline with event IDs | Lab 6 | Not provided |
| EV-010 | Forensics | Sanitised exported CloudTrail/GuardDuty JSON and investigation notes | Lab 10 | Not provided |
| EV-011 | Remediation | Before/after compliance checks from IAM, S3 and IMDS exercises | Labs 5, 9 and 13 | Not provided |

## Publication checklist

- [ ] Replace AWS account IDs with consistent placeholders, e.g. `111122223333` (clearly fictional).
- [ ] Redact access key IDs, secret access keys, STS session tokens, secret values and challenge flags.
- [ ] Remove personal names, emails, IP addresses, internal DNS names, instance IDs and sensitive resource names unless demonstrably synthetic.
- [ ] Preserve enough role/policy structure and relative timestamps to substantiate the finding.
- [ ] Label outputs **Lab-verified** only when captured from the actual authorised execution.
- [ ] Include execution time, tool/version, AWS region and identity class without revealing sensitive identifiers.
- [ ] Record both a negative test and intended-functionality test after remediation.
- [ ] Never upload real customer material or training-platform proprietary text verbatim.

## Safe evidence excerpt format

```text
Evidence ID: EV-001
Source exercise: Lab 02
Recorded time (UTC): [not supplied]
Principal: [sanitised role or user]
Observed action: [API event]
Observed result: [redacted exact response]
Corroborating CloudTrail event ID: [not supplied]
Validation status: UNVERIFIED
```

Do not populate the empty fields with invented command output or simulated event IDs. The difference between an illustrative case study and a verified assessment must remain explicit throughout the repository.
