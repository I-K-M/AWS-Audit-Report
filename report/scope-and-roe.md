# Scope and Rules of Engagement

## Engagement scope

The assessment covers a series of disposable AWS accounts supplied for isolated training exercises. Each exercise defines its own temporary credentials, allocated region, resource set and permitted actions. Activities are confined to the allocated environment and its operating window.

| Item | Scope |
| --- | --- |
| AWS accounts | The account allocated to the individual exercise |
| Regions | The region assigned on deployment |
| Services | IAM, STS, EC2, Lambda, S3, EBS, SSM, Secrets Manager, CloudTrail and GuardDuty, where included |
| Starting access | Temporary IAM user or role credentials supplied for the exercise |
| Data | Synthetic challenge records and resources |
| Test type | Reconnaissance, constrained exploitation, remediation or log analysis as specified by the exercise |

## Operating rules

Read-only exercises permit inspection and enumeration only. Exploitation exercises permit the specified IAM, STS, Lambda or EC2 operations within the disposable account. Hardening exercises permit changes to the designated resources. Forensic exercises prohibit changes to the scene.

Excluded activities include denial-of-service, operations against unrelated AWS accounts, external third-party assets, uncontrolled data extraction, interference with production infrastructure and alteration of security logs.

## Real-engagement requirements

A customer engagement would additionally need a signed authorisation identifying the account owner, account IDs, regions, excluded resources, permitted attack techniques, test windows, stop conditions and named technical/incident contacts. The agreement should cover evidence handling, incident escalation, restoration, credential revocation and applicable AWS testing restrictions.

No live engagement authorisation is implied by this document.

## Stop conditions

Testing must stop if the observed account differs from the allocated account, a third-party asset is reached, sensitive non-synthetic data is encountered, service stability is affected or the permitted window expires. Any unexpected exposure is escalated through the designated exercise or client channel.

## Evidence handling

The report uses sanitised account and resource references. Technical evidence for a production engagement would be stored under an agreed retention and access policy; secrets and customer data are not part of this public assessment.
