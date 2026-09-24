# AI Defense Lab — Portfolio

**Student:** Ikanke Okon Asuquo

**GitHub:** https://github.com/Ikankeabasi

**Hugging Face Space:** [My HF Space URL] 

**Completed:** 24/09/2026

---

## Level 1 — MedVitals AI · Cloud Infrastructure Security

### Scenario / Investigation
MedVitals AI is a simulated healthcare AI application. I was tasked with investigating a cloud-security incident involving exposed AWS credentials and overly permissive IAM access, then determining the security impact and implementing appropriate remediation.

### Problem / Vulnerability
AWS credentials were exposed in the application's source code, while the IAM policy granted Administrator-level access through wildcard permissions. I identified the issue through source-code and CloudTrail investigation using the 5Ws (Who, What, When, Where, and How) to identify Indicators of Compromise (IoCs).

An attacker who obtained the credentials could potentially access cloud resources, create privileged users, escalate permissions, and compromise sensitive patient data.

### Evidence
- CloudTrail investigation and IoC analysis
- Incident Timeline Report: https://github.com/Ikankeabasi/ai-security-defense-lab/blob/main/Incident-Timeline-Report-Level-1.md
- Supporting implementation/commit evidence: https://github.com/AibinuolaDamilola/ai-security-defense-lab/commit/ac71117800dadb318df892b39b66ed0b4a9d93d6

### Remediation
I removed hardcoded credentials and replaced them with environment variables, protected secrets from version control using .gitignore, and replaced wildcard Administrator-style IAM permissions with a least-privilege policy that grants only the permissions required by the application.

These changes address both the secret-exposure risk and the excessive-permission risk by reducing what an attacker could obtain and do if credentials were compromised.

### Commit
https://github.com/AibinuolaDamilola/ai-security-defense-lab/commit/ac71117800dadb318df892b39b66ed0b4a9d93d6

### Outcome
The application no longer exposes sensitive credentials in source code, secrets are excluded from version control, and IAM permissions are restricted to the required resources and actions.

This reduces the cloud attack surface and limits the potential impact of future credential compromise.

### Skills Demonstrated
CloudTrail Log Forensics · IAM Least Privilege · Secrets Management · Incident Timeline Reporting · Cloud Security Investigation · IoC Analysis

### Supporting Artifacts
- Technical write-up (Medium): https://medium.com/@ikanke2021/ai-security-defense-lab-level-1-my-first-hands-on-cloud-security-investigation-21a05cfe567b
- LinkedIn post: https://www.linkedin.com/posts/ikanke-asuquo-7825623b4_cybersecurity-aisecurity-cloudsecurity-share-7489077707708895232-zf4o/
- Incident Timeline Report: https://github.com/Ikankeabasi/ai-security-defense-lab/blob/main/Incident-Timeline-Report-Level-1.md

### LinkedIn Case Study
https://www.linkedin.com/posts/ikanke-asuquo-7825623b4_cybersecurity-aisecurity-cloudsecurity-share-7489077707708895232-zf4o/

---

## Level 2 — DataForge ML · AI Model Security

### Scenario / Investigation
DataForge ML is a simulated AI/ML environment. I was tasked with assessing the security of an AI model supply chain and determining whether a model being downloaded and loaded into an inference pipeline could introduce security risk.

### Problem / Vulnerability
DataForge ML’s model loader downloaded AI model weights from the unverified Hugging Face account logix-community in legacy pickle format without integrity verification before loading the model.

I identified unsafe pickle.load() usage, lack of model integrity verification, an unverified model source, missing checksum, missing model card, unknown licence, and use of the legacy .pkl format.

An attacker could potentially compromise the ML supply chain by distributing a malicious serialized model that could introduce unsafe behavior into the environment.

### Evidence
- Picklescan assessment of the model fixture
- Static inspection of model_loader.py
- MITRE ATLAS mapping: AML.T0010 — ML Supply Chain Compromise
- Relevant commit evidence: https://github.com/Ikankeabasi/ai-security-defense-lab/commit/b85b96cb428457f5d5f2c45deade0e97076084de

### Remediation
I hardened the inference pipeline by replacing unsafe pickle loading with safetensors, adding a mandatory Picklescan pre-load security gate, and changing the model source to a verified repository placeholder.

The remediation addresses the vulnerability by reducing unsafe deserialization risk and introducing a security check before a model is trusted and loaded.

### Commit
https://github.com/Ikankeabasi/ai-security-defense-lab/commit/b85b96cb428457f5d5f2c45deade0e97076084de

### Outcome
The inference pipeline now has stronger supply-chain controls: unsafe legacy loading was replaced, model scanning was introduced before loading, and the source-validation approach was strengthened.

This matters because an AI application's security depends not only on how a model behaves, but also on whether the model artifact itself can be trusted.

### Skills Demonstrated
AI Model Supply Chain Security · Picklescan · safetensors · MITRE ATLAS AML.T0010 · Static Malware Analysis · Model Risk Assessment · CI/CD Security Gates

### Supporting Artifacts
- Technical write-up: https://github.com/Ikankeabasi/hernetiq-fellowship-portfolio./blob/main/week-6/week-6-model-security-assessment..pdf

### LinkedIn Case Study
https://www.linkedin.com/posts/ikanke-asuquo-7825623b4_aisecurity-cybersecurity-mlsecurity-share-7495198753507840000-Vjl3/

---

## Level 3 — CartBot AI · Application & API Security

### Scenario / Investigation
CartBot AI is a simulated AI-powered e-commerce application with customer-facing APIs, customer/order data, product content, and an AI assistant.

I approached the lab as an application and AI security assessment: understanding the application, investigating API authorization, examining attacker-controlled content entering the AI workflow, assessing abuse at scale, and validating remediation.

### Problem / Vulnerability
The assessment identified multiple security weaknesses:

- Broken Object Level Authorization (BOLA)
- Indirect Prompt Injection
- Insufficient Rate Limiting / potential Denial of Wallet

I identified vulnerable configuration and application behavior including:

- TRUST_CUSTOMER_ID_HEADER = True
- REQUIRE_JWT_VALIDATION = False
- RATE_LIMIT_ENABLED = False

The application could trust a client-supplied customer identifier without sufficient cryptographic verification or object-level authorization. Attacker-controlled product content could influence the AI workflow, and unrestricted requests could support automated abuse and increased LLM resource consumption.

Potential impact included unauthorized customer-data access, AI behavior manipulation, bulk harvesting, service degradation, and increased operational costs.

### Evidence
- BOLA demonstration involving access to another customer's orders
- Indirect prompt injection investigation through the CartBot AI assistant
- Bulk Harvest simulation
- Semgrep static analysis
- Security verification results: **4 FAIL → 4 PASS**
- AI Application Security Assessment Report: https://github.com/Ikankeabasi/hernetiq-fellowship-portfolio./blob/main/week-8/week-8-ai-application-security-report.md

### Remediation
I implemented and enabled:

- Server-side JWT validation
- Object-level authorization and ownership checks
- Rate limiting
- Stronger handling of externally controlled product content as untrusted data
- Controls to reduce indirect prompt injection risk
- Security verification and automated testing after remediation

These controls address the vulnerabilities at the relevant application and AI trust boundaries rather than relying on the model alone.

### Commit
https://github.com/Ikankeabasi/ai-security-defense-lab/commit/120e27b527582b45a53ee62a4b4e0a01f48e0033

### Outcome
After remediation, the security verification results improved from **4 FAIL to 4 PASS**.

The application now performs stronger requester validation and object-level authorization, restricts excessive request volume, and has improved handling of untrusted product content in the AI workflow.

This matters to both the AI application and the business because it reduces risks to customer confidentiality, AI integrity, service availability, and LLM-related financial resources.

### Skills Demonstrated
API Security · OWASP API Top 10 · Broken Object Level Authorization (BOLA) · JWT Authentication · Object-Level Authorization · Indirect Prompt Injection Analysis · MITRE ATLAS AML.T0051 · Rate Limiting · Denial of Wallet Mitigation · Semgrep Static Analysis · Threat Modeling · Trust-Boundary Analysis · Defence-in-Depth Architecture · Security Remediation & Verification

### Supporting Artifacts
- AI Application Security Report: https://github.com/Ikankeabasi/hernetiq-fellowship-portfolio./blob/main/week-8/week-8-ai-application-security-report.md
- Week 8 portfolio evidence: https://github.com/Ikankeabasi/hernetiq-fellowship-portfolio./tree/main/week-8
- Threat model: https://github.com/Ikankeabasi/hernetiq-fellowship-portfolio./blob/main/week-8/api-security-threat-model.md

### LinkedIn Case Study
https://lnkd.in/p/eDBKUKtz

---

## Level 4 — PayGuard · Data Security in AI

**Problem:**

PayGuard's RAG system had weaknesses around tenant isolation, vector-store access, embedding exposure, fine-tuning data integrity, and query control. The RAG layer trusted the client-supplied `tenant_id`, the vector store used a shared index without enforced tenant filtering, the fine-tuning pipeline accepted training data without integrity validation, and retrieval had no effective rate limit.

I also assessed the security impact of embedding inversion and the fine-tuning backdoor demonstrated in the lab. These weaknesses created a path to cross-tenant data exposure, poisoned model behaviour, and increased inference/resource costs.

**Method:**

I inspected the PayGuard RAG configuration and embedding model source, triggered cross-tenant retrieval by spoofing the `tenant_id` field, ran the scale demonstration, and simulated embedding inversion on a leaked record.

I then inspected the Airflow fine-tuning pipeline, triggered the poisoned model behaviour, ran Semgrep against the PayGuard fixtures, and documented the findings using the STRIDE threat model.

For remediation, I moved tenant enforcement to the database/vector-store layer, strengthened fine-tuning data validation, protected the retrieval boundary, and verified the security changes against the Level 4 requirements.

**Evidence:**

- Level 4 remediation commit:[ https://github.com/Ikankeabasi/ai-security-defense-lab/commit/4c2dfc5a606a6d81c7f43d8f0c21fe01f2705f60](https://github.com/Ikankeabasi/ai-security-defense-lab/commit/f141340cb548ab9d1d21ce2b536094d84fa5366a)
- Level 4 STRIDE findings: https://github.com/Ikankeabasi/hernetiq-fellowship-portfolio./blob/main/week-11/week-11-level-4-data-security-findings.md
- Level 4 lab: https://github.com/Ikankeabasi/ai-security-defense-lab/blob/main/levels/level4_payguard.py

**Outcome:**

Cross-tenant retrieval is no longer possible at the application layer or the database layer — even a future code change that forgets to check `tenant_id` cannot bypass the database-level tenant boundary.

The remediation also strengthens fine-tuning data validation and the retrieval security controls, reducing the risk of tenant-data exposure, poisoned model behaviour, and uncontrolled query abuse.

**Skills:**

RAG Security · Vector Database Access Control · OWASP LLM Top 10 (LLM09, LLM05) · STRIDE Threat Modeling · Airflow Pipeline Security · Database-Level Authorization Design · Semgrep Static Analysis.

**Others:**
- [Technical write-up link]
- [LinkedIn post link]
- [Any additional evidence/screenshot link]

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
