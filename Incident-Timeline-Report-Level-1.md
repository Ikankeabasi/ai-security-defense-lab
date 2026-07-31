# Incident Timeline Report — MedVitals AI Level 1

**Author:** Ikanke Okon Asuquo

**Role:** AI Security Fellow (HerNetIQ Cohort 1)

**Project:** AI Defense Lab – Level 1

**Date:** 31 July 2026

---

# Executive Summary

An investigation was conducted after suspicious activity was detected within the MedVitals AI AWS environment.

CloudTrail logs revealed that an attacker successfully assumed an administrative role using exposed AWS credentials that had been hardcoded into the application's source code and published in a public GitHub repository.

The attacker obtained temporary administrative privileges, enumerated cloud storage resources, staged sensitive patient records for exfiltration, and successfully downloaded the data.

---

# Incident Timeline

| Time (UTC) | Event | Evidence | Security Assessment |
|------------|-------|----------|---------------------|
| Before Incident | AWS credentials were hardcoded inside `config.py` and pushed to a public GitHub repository. | Source code review | Critical credential exposure. |
| Before Incident | IAM policy granted `Action: *` on `Resource: *`. | deploy-role-policy.json | Excessive permissions violated the Principle of Least Privilege. |
| 03:02:09 | Attacker executed `AssumeRole`. | CloudTrail Event 14 | External IP, Python requests client, suspicious administrative access. |
| 03:02:31 | Attacker executed `ListBuckets`. | CloudTrail Event 15 | Cloud storage enumeration confirmed reconnaissance. |
| 03:03:02 | Attacker executed `PutObject`. | CloudTrail Event 16 | Patient records staged for exfiltration. Successful upload confirmed by returned ETag. |
| 03:04:18 | Attacker executed `GetObject`. | CloudTrail Event 17 | Exfiltration completed. Sensitive patient data downloaded. |

---

# Indicators of Compromise (IoCs)

- Unknown external IP address (`198.51.100.45`)
- User Agent: `python-requests`
- Administrative `AssumeRole`
- Access outside normal operating hours
- Successful S3 bucket enumeration
- Data staging using `PutObject`
- Data exfiltration using `GetObject`

---

# Root Cause

The investigation identified two primary security failures:

1. Sensitive AWS credentials were hardcoded directly into the application source code.

2. The deployment IAM policy granted unrestricted permissions (`Action: *`, `Resource: *`), allowing complete account compromise after credential theft.

---

# Business Impact

The compromise exposed sensitive healthcare information stored by MedVitals AI.

Potential impacts include:

- Exposure of patient records
- Loss of customer trust
- Regulatory compliance violations
- Financial and reputational damage

---

# Remediation Actions

The following corrective actions were implemented:

- Removed hardcoded credentials from the application.
- Replaced secrets with environment variables.
- Created a `.env` file.
- Protected secrets using `.gitignore`.
- Replaced wildcard IAM permissions with a least-privilege policy.
- Limited S3 access to only required buckets.
- Restricted allowed AWS actions to operational requirements.

---

# Lessons Learned

This investigation demonstrated the importance of:

- CloudTrail log analysis
- Threat hunting using the 5Ws
- Identifying Indicators of Compromise (IoCs)
- Secrets management
- IAM least privilege
- Incident timeline reporting
- Cloud security hardening

---

# Investigation Status

**Status:** Closed

The identified vulnerabilities were remediated, the attack path was documented, and security controls were strengthened to reduce the likelihood and impact of future credential compromise.
