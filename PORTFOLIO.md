# AI Defense Lab — Portfolio

**Student:** Ikankeabasi Okon Asuquo

**GitHub:** https://github.com/Ikankeabasi

**Hugging Face Space:** Not deployed yet

**Completed:** 31/07/2026

---

Fill in each section as you complete a level. Link directly to your commit diff so a recruiter can see exactly what you changed.

---

## Level 1 — MedVitals AI · Cloud Infrastructure Security

**Problem:** MedVitals AI exposed AWS credentials in its source code, and its IAM policy granted Administrator-level access through wildcard permissions. If an attacker obtained the credentials, they could access cloud resources, create privileged users, escalate permissions, and compromise sensitive patient data.

**Method:** Investigated AWS CloudTrail logs using the 5Ws (Who, What, When, Where, and How) to identify Indicators of Compromise (IoCs). Removed hardcoded credentials by replacing them with environment variables, protected secrets using a .gitignore file, and replaced the wildcard IAM policy with a least-privilege policy that grants only the permissions required by the application.

**Evidence:** https://github.com/AibinuolaDamilola/ai-security-defense-lab/commit/ac71117800dadb318df892b39b66ed0b4a9d93d6

**Outcome:** The application no longer exposes sensitive credentials in source code, secrets are excluded from version control, and IAM permissions are restricted to only the required resources and actions. These changes reduce the attack surface and limit the impact of future credential compromise.

**Skills:** CloudTrail Log Forensics · IAM Least Privilege · Secrets Management · Incident Timeline Reporting

**Others:**
- Technical write-up: https://github.com/Ikankeabasi/ai-security-defense-lab/blob/main/Incident-Timeline-Report-Level-1.md 
- LinkedIn post: [Add your LinkedIn post link]
-  Additional documentation: https://github.com/AibinuolaDamilola/ai-security-defense-lab/commit/ac71117800dadb318df892b39b66ed0b4a9d93d6

---

## Level 2 — DataForge ML · AI Model Security

**Problem:**

**Method:**

**Evidence:** [Link to commit]

**Outcome:**

**Skills:** Model Supply Chain Verification · Pickle Exploit Detection · Safetensors · Automated Model Scanning

**Others:**
- [Technical write-up link]
- [LinkedIn post link]

---

## Level 3 — CartBot AI · Application & API Security

**Problem:**

**Method:**

**Evidence:** [Link to commit]

**Outcome:**

**Skills:** AI API Hardening · Rate Limiting · Output Filtering · OWASP LLM Top 10 · Direct Prompt Injection Defence

**Others:**
- [Technical write-up link]
- [LinkedIn post link]

---

## Level 4 — PayGuard · Data Security in AI

**Problem:**

**Method:**

**Evidence:** [Link to commit]

**Outcome:**

**Skills:** STRIDE Threat Modeling · RAG Pipeline Security · Multi-Tenant Data Isolation · Indirect Prompt Injection Defence

**Others:**
- [Technical write-up link]
- [LinkedIn post link]

---

## Level 5 — LegalBot Municipal · Agentic AI Security

**Problem:**

**Method:**

**Evidence:** [Link to commit]

**Outcome:**

**Skills:** Excessive Agency Mitigation · Llama Guard Integration · Pydantic Schema Enforcement · Autonomous Agent Containment

**Others:**
- [Technical write-up link]
- [LinkedIn post link]
