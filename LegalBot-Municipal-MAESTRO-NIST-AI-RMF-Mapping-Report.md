# LegalBot Municipal Part A — MAESTRO + NIST AI RMF Mapping Report

**Mission:** S25 — LegalBot Municipal Part A: Lab Evidence + MAESTRO/NIST AI RMF Report  
**Domain:** Domain 5 — Autonomous Agent Governance  
**Scope:** Mapping the LegalBot Municipal Part A investigation to MAESTRO and the NIST AI Risk Management Framework (AI RMF). This is a framework-mapping report, not the Part B remediation report.

## 1. Purpose and scope

This report uses two complementary lenses to document the risks shown in the LegalBot Municipal lab:

- **MAESTRO** identifies where a risk sits within the technical agentic system.
- **NIST AI RMF** identifies how an organization should govern, contextualize, measure and manage that risk.

The scenario concerns a municipal AI assistant that processes citizen comments on case records. The supplied vulnerable configuration shows standing authority to finalize dismissals, no mandatory human-approval gate, no citizen-input sanitization, and disabled agent-action logging. A citizen comment contains an instruction that attempts to redirect the agent and claims supervisor authorization.

The analysis below describes the supplied lab scenario and configuration. It does not claim that a real municipal case was changed or that a particular Semgrep scan was personally executed.

## 2. Investigation findings

### Finding 1 — Standing permission exceeds the agent's task needs

The supplied configuration sets `CAN_FINALIZE_DISMISSAL = True`. LegalBot's ordinary purpose is to assist with municipal questions and casework, but it also holds authority to finalize a sensitive case disposition without a separate human decision.

- **Risk:** A compromised or misdirected agent can perform an action beyond what its normal task requires.
- **Potential impact:** An incorrect dismissal, an unreliable municipal decision, and reduced public trust.
- **MAESTRO mapping:** **Layer 3 — Agent Frameworks** (the agent's workflow and tool-use path); **Layer 6 — Security & Compliance** (permission and approval requirements).
- **NIST AI RMF mapping:** **Govern** (define accountability and action-approval policy); **Map** (identify the agent's intended role, authority and affected people); **Manage** (reduce unnecessary authority and establish approval boundaries).

### Finding 2 — Goal hijack through untrusted citizen content

The supplied citizen comment claims that a case has already been reviewed and includes a “SYSTEM OVERRIDE” instruction to dismiss it immediately. The content is untrusted input, not proof of authorization, but the scenario shows how it can be interpreted as a directive.

- **Risk:** An attacker can try to redirect the agent away from its intended purpose.
- **Potential impact:** The agent may pursue an unauthorized objective and attempt a consequential action.
- **MAESTRO mapping:** **Layer 1 — Foundation Models** (the model's interpretation of the instruction); **Layer 2 — Data Operations** (citizen content entering the system as input); **Layer 3 — Agent Frameworks** (the workflow that passes the content into agent reasoning and action).
- **NIST AI RMF mapping:** **Map** (identify untrusted input, intended use and potential impact); **Measure** (test whether adversarial comments redirect behavior); **Manage** (treat citizen content as data rather than authority and add controls that prevent it from directly authorizing actions).

### Finding 3 — No mandatory human approval before a sensitive action

The supplied configuration sets `REQUIRE_HUMAN_APPROVAL = False`. In the vulnerable scenario, a successful goal hijack can therefore progress into case finalization without an approval checkpoint.

- **Risk:** The system allows the agent's interpretation to become a sensitive administrative action without an independent authorization decision.
- **Potential impact:** An unauthorized or incorrect case disposition may be completed before staff can intervene.
- **MAESTRO mapping:** **Layer 3 — Agent Frameworks** (action orchestration and execution); **Layer 6 — Security & Compliance** (authorization and human oversight).
- **NIST AI RMF mapping:** **Govern** (define which actions require human authorization); **Measure** (verify that unauthorized actions are blocked); **Manage** (require a human decision before the sensitive action can execute).

### Finding 4 — Insufficient action observability

The supplied configuration sets `LOG_AGENT_ACTIONS = False`. The lab scenario also describes no audit log being created when the vulnerable action occurs.

- **Risk:** Investigators may lack a reliable record of the action requested, the action attempted or completed, and the context that led to it.
- **Potential impact:** Delayed detection, weaker incident investigation, and difficulty demonstrating accountability.
- **MAESTRO mapping:** **Layer 5 — Evaluation & Observability** (logging and monitoring); **Layer 6 — Security & Compliance** (auditability and accountability).
- **NIST AI RMF mapping:** **Govern** (assign responsibility for oversight and records); **Measure** (assess whether monitoring and evidence are sufficient); **Manage** (retain appropriate logs and use them to investigate and respond).

### Finding 5 — Static-analysis checks are needed to identify risky configuration

The lab instructs the fellow to run Semgrep against the supplied fixture and says three findings are expected in `agent_config.py`. The configuration shown in the lab contains the risky settings described above. The actual scan output should be retained separately as Evidence 04; this report does not substitute for that output.

- **Risk:** Risky configuration may remain unnoticed if checks are not part of the review process.
- **Potential impact:** Insecure permissions, missing approval requirements or missing input protections may reach deployment.
- **MAESTRO mapping:** **Layer 5 — Evaluation & Observability** (security evaluation and testing); **Layer 6 — Security & Compliance** (security checks and policy enforcement).
- **NIST AI RMF mapping:** **Measure** (use repeatable tests and static analysis to evaluate controls); **Manage** (track findings and ensure they are addressed according to risk).

## 3. Combined mapping matrix

| Lab finding | MAESTRO technical layer(s) | Relevant NIST AI RMF function(s) | Why the mapping fits |
|---|---|---|---|
| Standing dismissal permission | Layer 3 — Agent Frameworks; Layer 6 — Security & Compliance | Govern, Map, Manage | The agent's action capability exceeds its ordinary task needs; organizational policy and risk reduction are required. |
| Untrusted instruction in citizen comment | Layer 1 — Foundation Models; Layer 2 — Data Operations; Layer 3 — Agent Frameworks | Map, Measure, Manage | The model receives adversarial content through an input path; behavior must be assessed and constrained. |
| No human-approval gate | Layer 3 — Agent Frameworks; Layer 6 — Security & Compliance | Govern, Measure, Manage | The action workflow lacks an independent authorization boundary for a sensitive operation. |
| Disabled action logging | Layer 5 — Evaluation & Observability; Layer 6 — Security & Compliance | Govern, Measure, Manage | Oversight requires assigned responsibility, useful evidence and a response process. |
| Semgrep static-analysis task | Layer 5 — Evaluation & Observability; Layer 6 — Security & Compliance | Measure, Manage | Repeatable checks help identify risky configuration and support tracking of findings. |

**Layers not independently established by this investigation:** The available scenario does not establish a separate deployment/infrastructure compromise (**MAESTRO Layer 4**) or a separate external agent-ecosystem issue (**Layer 7**). They should not be marked as confirmed findings without additional evidence. Layer 4 may still be reviewed if further deployment or credential evidence becomes available.

## 4. How the frameworks complement each other

MAESTRO and NIST AI RMF should not be treated as competing checklists. They answer different questions about the same risk.

- **MAESTRO asks where the technical risk exists.** In this scenario, the most direct concerns are the model's handling of untrusted content, the agent's action workflow, its permission boundary, and the absence of useful action logs.
- **NIST AI RMF asks how the organization should manage the risk.** Govern establishes ownership and approval policy; Map describes the system, context and affected parties; Measure evaluates behavior and control effectiveness; Manage prioritizes and responds to the findings.

Using both frameworks prevents the assessment from stopping at “the agent can be hijacked.” It connects the technical path to the organizational responsibilities needed to prevent an unsafe instruction from becoming an unauthorized action.

## 5. Overall conclusion

The central issue in the LegalBot Municipal scenario is the combination of **untrusted input, excessive standing authority, no mandatory human approval, and insufficient action logging**. A goal hijack is more consequential when the agent already has the power to perform a sensitive action. MAESTRO locates the risk across model interpretation, data handling, agent workflow, observability and security controls. NIST AI RMF then provides the governance and risk-management structure for understanding the context, testing the controls, assigning accountability and prioritizing the response.

This report is the **Part A mapping deliverable**. It is separate from the code-remediation artifact or Part B fix report. The screenshots, Semgrep output and any implementation/verification results should remain linked as their own evidence items.

## 6. Framework references

- **MAESTRO:** Seven-layer agentic threat-modeling framework as covered in the HerNetIQ Domain 5 learning material.
- **NIST AI Risk Management Framework (AI RMF):** Govern, Map, Measure and Manage functions as covered in the HerNetIQ Domain 5 learning material.
- **Lab scenario:** LegalBot Municipal, Level 5 — `levels/level5_legalbot.py` and its supplied configuration and evidence instructions.
