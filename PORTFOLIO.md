# AI Defense Lab — Portfolio

**Student:** Ikanke Okon Asuquo

**GitHub:** https://github.com/Ikankeabasi

**Hugging Face Space:** https://ai-security-defense-lab-amblessed.streamlit.app/

**Completed:** 31/07/2026

---

Fill in each section as you complete a level. Link directly to your commit diff so a recruiter can see exactly what you changed.

---

## Level 1 — MedVitals AI · Cloud Infrastructure Security

**Problem:** MedVitals AI exposed AWS credentials in its source code, and its IAM policy granted Administrator-level access through wildcard permissions. If an attacker obtained the credentials, they could access cloud resources, create privileged users, escalate permissions, and compromise sensitive patient data.

**Method:** Investigated AWS CloudTrail logs using the 5Ws (Who, What, When, Where, and How) to identify Indicators of Compromise (IoCs). Removed hardcoded credentials by replacing them with environment variables, protected secrets using a .gitignore file, and replaced the wildcard IAM policy with a least-privilege policy that grants only the permissions required by the application.

**Evidence:** https://github.com/AibinuolaDamilola/ai-security-defense-lab/commit/120e27b527582b45a53ee62a4b4e0a01f48e0033

**Outcome:** The application no longer exposes sensitive credentials in source code, secrets are excluded from version control, and IAM permissions are restricted to only the required resources and actions. These changes reduce the attack surface and limit the impact of future credential compromise.

**Skills:** CloudTrail Log Forensics · IAM Least Privilege · Secrets Management · Incident Timeline Reporting

**Others:**

- Technical write-up (Medium):
https://medium.com/@ikanke2021/ai-security-defense-lab-level-1-my-first-hands-on-cloud-security-investigation-21a05cfe567b

- LinkedIn post: https://www.linkedin.com/posts/ikanke-asuquo-7825623b4_cybersecurity-aisecurity-cloudsecurity-share-7489077707708895232-zf4o/

- Incident Timeline Report:
(https://github.com/Ikankeabasi/ai-security-defense-lab/blob/main/Incident-Timeline-Report-Level-1.md)

- GitHub Commit:
https://github.com/AibinuolaDamilola/ai-security-defense-lab/commit/ac71117800dadb318df892b39b66ed0b4a9d93d6

---

## Level 2 — DataForge ML · AI Model Security

**Problem:** DataForge ML’s model loader downloaded AI model weights from the unverified Hugging Face account logix-community in legacy pickle format without running integrity verification before loading the model.

**Method:** Audited model_loader.py and identified unsafe pickle.load() usage, lack of model integrity verification, an unverified model source, missing checksum, missing model card, unknown licence, and use of the legacy .pkl format. Ran Picklescan against the model fixture and mapped the incident to MITRE ATLAS AML.T0010 — ML Supply Chain Compromise.

**Evidence:**  https://github.com/Ikankeabasi/ai-security-defense-lab/commit/b85b96cb428457f5d5f2c45deade0e97076084de

**Outcome:** The inference pipeline was hardened by replacing unsafe pickle loading with safetensors, adding a mandatory Picklescan pre-load security gate, and changing the model source to a verified repository placeholder.

**Skills:** AI model supply chain security · Picklescan · safetensors · MITRE ATLAS AML.T0010 · Static malware analysis · CI/CD security gates.

**Others:**
- Technical write-up link: https://github.com/Ikankeabasi/hernetiq-fellowship-portfolio./blob/main/week-6/week-6-model-security-assessment..pdf
- LinkedIn post link: https://www.linkedin.com/posts/ikanke-asuquo-7825623b4_aisecurity-cybersecurity-mlsecurity-share-7495198753507840000-Vjl3/

---

## Level 3 — CartBot AI · Application & API Security

**Problem:** CartBot AI’s customer-facing API trusted a client-supplied `customer_id` header with no cryptographic verification.

**Method:** Audited `api_config.py` and identified `TRUST_CUSTOMER_ID_HEADER = True`, `REQUIRE_JWT_VALIDATION = False`, and `RATE_LIMIT_ENABLED = False`. Queried the CartBot AI assistant to trigger the indirect prompt injection, demonstrated BOLA by accessing another customer’s orders, ran the Bulk Harvest simulation, and used Semgrep to identify the vulnerable patterns.

**Evidence:** https://github.com/Ikankeabasi/ai-security-defense-lab/commit/120e27b527582b45a53ee62a4b4e0a01f48e0033

**Outcome:** The API can no longer be BOLA’d via header spoofing — JWT validation verifies the requester and requested customer ID, rate limiting is enabled, and the system prompt restricts customer-data retrieval while treating product content as untrusted data.

**Skills:** API security · OWASP API Top 10 (BOLA) · MITRE ATLAS AML.T0051 (Indirect Prompt Injection) · MITRE ATLAS AML.T0054 (LLM Data Exfiltration) · JWT authentication · Rate limiting / Denial of Wallet mitigation · Semgrep static analysis · Defence-in-depth architecture

**Others:**
- https://github.com/Ikankeabasi/hernetiq-fellowship-portfolio./blob/main/week-8/week-8-ai-application-security-report.md
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
