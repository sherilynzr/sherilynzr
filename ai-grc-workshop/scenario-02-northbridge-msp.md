# Scenario 2 — Customer-Facing AI Solution: IT Managed Service Provider

**Company:** NorthBridge IT Solutions  
**Industry:** IT Managed Services · EU and US markets  
**AI Deployment Type:** Customer-facing LLM · Enterprise client portal  
**Frameworks:** Google Secure AI Framework · MITRE ATLAS · OWASP Top 10 LLM · CSA MAESTRO · NIST CSF 2.0

---

## The Situation

NorthBridge IT Solutions is a managed service provider supporting approximately 430 enterprise clients across the EU and United States. Eight months ago, NorthBridge embedded an AI-powered support assistant called **Helios** into its customer portal. Enterprise clients use Helios to submit and triage IT support tickets, search the NorthBridge knowledge base, and get escalation routing when their issue requires a human technician.

Helios is built on a foundation model licensed from a third-party AI vendor called **Vektora AI**. NorthBridge's engineering team fine-tuned it on their internal knowledge base and **three years of anonymized client ticket data** spanning all 430 enterprise clients. The vendor contract with Vektora AI covers uptime SLAs and basic support, but contains **no clauses covering model update notifications, security testing requirements, or data handling obligations** beyond what was in the original licensing agreement. NorthBridge's legal team reviewed the contract before signing and raised no concerns.

When enterprise users access the portal for the first time, they click a **consent acknowledgment button** before their initial Helios session. NorthBridge's CTO stated in a recent company blog post that Helios **"does not store personal data."** The security team has not conducted a vendor security assessment of Vektora AI. The compliance lead is currently managing a SOC 2 Type II audit and has explicitly deprioritized a review of the Helios deployment until after the audit closes in six weeks.

---

## What You Need to Figure Out

Working from the scenario above, identify the following for NorthBridge IT Solutions:

**Risk Assessment** *(use Google Secure AI Framework as your lens)*
1. What is the most significant AI-specific risk in this deployment?
2. What is the second most significant risk?
3. What is the third?

**Security Controls** *(reference MITRE ATLAS, OWASP Top 10 LLM, CSA MAESTRO)*
1. What control should be implemented first?
2. What is the second priority control?
3. What is the third?

**Securing Operations** *(Security Fundamentals)*
1. What ongoing operational safeguard is most critical?
2. What is the second?
3. What is the third?

> **Note:** Several details in this scenario are designed to create a false sense of due diligence. Identify which ones before you begin your analysis.

---

## Assumptions Made During the Exercise

These were the working assumptions my group identified while analyzing this scenario:

- A consent acknowledgment button does not establish GDPR lawful basis on its own
- "Code ownership is ambiguous" — no party has formally accepted accountability for model security
- "Vendor has robust access controls" was assumed without verification — that assumption needs to become a formal vendor assessment

---

## Analysis

### Risk Assessment

| # | Risk | Reasoning |
|---|---|---|
| 1 | Data poisoning | Client ticket history was used for fine-tuning across all 430 enterprise clients. A malicious actor with access to one client's ticket data could inject content designed to skew model behavior across the entire customer base — a cross-tenant contamination risk that doesn't exist in single-tenant deployments. |
| 2 | Model source tampering — supply chain risk | The Vektora AI contract contains no model update notification clauses. NorthBridge has zero visibility into when the model changes, what changes between versions, or whether Vektora AI has tested updates for security regressions. This is an unmanaged dependency in the AI supply chain. |
| 3 | Sensitive data disclosure | Enterprise IT support tickets almost certainly contain PII, internal network details, system credentials, and configuration information. The CTO's claim that Helios "does not store personal data" is unverified — and storage is irrelevant to in-session leakage, where sensitive content from one client's ticket could surface in another's session. |

### Security Controls

| # | Control | Control Type | Framework Reference |
|---|---|---|---|
| 1 | Input sanitization and prompt injection defenses — validate inputs and strip sensitive content before it enters the model context | Preventive | OWASP Top 10 LLM — LLM01: Prompt Injection |
| 2 | Client data isolation — enforce strict tenant boundaries so one client's ticket data cannot inform or appear in another client's session | Preventive | CSA MAESTRO — multi-tenant isolation |
| 3 | Model hardening and adversarial testing before any Vektora AI update goes live in the NorthBridge environment | Preventive / Detective | MITRE ATLAS — AML.T0010: ML Supply Chain Compromise |

### Securing Operations

| # | Operational Safeguard | Compliance Anchor |
|---|---|---|
| 1 | Conduct a DPIA (Data Protection Impact Assessment) — mandatory under GDPR Art. 35 for large-scale processing of personal data; the consent button does not substitute for this | GDPR Article 35 |
| 2 | Data minimization and pseudonymization — limit PII in the model context to what is strictly necessary; pseudonymize where possible | GDPR Article 5(1)(c) |
| 3 | Integrity and accountability / audit logging — log every Helios interaction in a tamper-evident store for incident reconstruction and regulatory response capability | NIST CSF DE.CM · GDPR Article 5(2) |

---

## The Red Herrings

> *"SOC 2 audit is in progress."*

SOC 2 audits NorthBridge's own internal controls. It says nothing about Vektora AI's security posture, the model's behavior, or the adequacy of the vendor contract for AI risk purposes.

> *"Legal reviewed the contract."*

Legal reviewing a contract confirms the contract was reviewed — not that it is adequate for AI-specific risk obligations. A contract without model update notifications, security testing requirements, or data handling clauses is a material gap regardless of whether legal signed off on it.

> *"The compliance lead will get to it after the audit closes."*

Deprioritizing a known risk gap is itself a risk management decision. It should be formally documented as a risk acceptance with a timeline and escalation path — not treated as a scheduling note.

---

## NIST CSF 2.0 Mapping

| CSF Function | Gap in this scenario |
|---|---|
| GOVERN · GV.SC | No vendor risk assessment of Vektora AI — AI supply chain risk entirely unmanaged |
| GOVERN · GV.RM | Known compliance gap deprioritized without formal risk acceptance or escalation documentation |
| PROTECT · PR.DS | No data minimization controls on what enters the model context from client tickets |
| RESPOND · RS.CO | No logging infrastructure exists to support a regulatory response under GDPR |

---

*Part of the [AI Cyber GRC Workshop Analysis](./README.md) · WiCyS 2025 · Sherilyn Rong*
