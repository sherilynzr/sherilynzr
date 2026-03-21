# Scenario 1 — Internal AI Solution: Healthcare

**Company:** Maplewood Health System  
**Industry:** Healthcare · Regional hospital network  
**AI Deployment Type:** Internal LLM · Staff-facing only  
**Frameworks:** Google Secure AI Framework · MITRE ATLAS · OWASP Top 10 LLM · CSA MAESTRO · NIST CSF 2.0

---

## The Situation

Maplewood Health System is a regional hospital network operating 14 facilities across the Midwest with approximately 9,200 clinical and administrative staff. Six months ago, the IT department deployed an internal AI documentation assistant called **CareScribe** to help nurses and attending physicians draft patient notes, summarize discharge instructions, and flag potential gaps in care plans.

CareScribe is accessible only through a secured intranet portal — no patient can interact with it directly. Staff log in with their existing hospital credentials, describe a patient situation in plain language, and CareScribe generates a structured draft they can review and submit to the EHR.

To pull relevant context, CareScribe connects to the live **Epic EHR system via a shared service account API key**. That same key is currently used by three other internal applications: a scheduling tool, a lab results aggregator, and an internal reporting dashboard. The model itself was built on a third-party foundation model licensed from a vendor, then fine-tuned by Maplewood's IT team using **two years of de-identified EHR exports**.

The CISO approved CareScribe for deployment after a 30-day pilot in one ICU unit. Staff satisfaction scores were high — average note completion time dropped from 18 minutes to 4. The legal team reviewed the deployment and confirmed it was HIPAA-compliant **"in principle"** because patient data never leaves Maplewood's network perimeter. No formal Security Rule risk analysis was conducted prior to go-live.

The compliance team is currently focused on a Joint Commission re-accreditation and has not scheduled a review of CareScribe.

---

## What You Need to Figure Out

Working from the scenario above, identify the following for Maplewood Health System:

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

> **Note:** Some details in this scenario are designed to sound reassuring. Not everything that sounds like a control actually is one. Read carefully before you answer.

---

## Analysis

### Risk Assessment

| # | Risk | Reasoning |
|---|---|---|
| 1 | PHI/PII exposure via model output | The shared API key gives CareScribe — and three other applications — broad EHR access. A context contamination error could surface one patient's data during another patient's encounter. The blast radius of a single key compromise is enormous. |
| 2 | Training data re-identification | The fine-tuning dataset was described as "de-identified," but de-identification in healthcare is harder than most teams assume. Rare diagnoses, unusual treatment combinations, and timestamps are quasi-identifiers. Models can also memorize and reproduce training data in ways that defeat de-identification after the fact. |
| 3 | Hallucinated clinical content | A fabricated drug dosage, incorrect contraindication, or invented care plan element in a note that gets submitted to the EHR is not a data issue — it is a patient safety event. This risk is unique to healthcare AI deployments and does not have a direct analog in most other sectors. |

### Security Controls

| # | Control | Control Type | Framework Reference |
|---|---|---|---|
| 1 | Dedicated, scoped API credential for CareScribe only — read-only, current-encounter-only access | Preventive | CSA MAESTRO — least-privilege access; OWASP Top 10 LLM — LLM09 |
| 2 | Output guardrails and PII filtering — intercept model responses before they reach the staff interface and flag or strip PHI that should not be surfaced | Preventive / Detective | OWASP Top 10 LLM — LLM06: Sensitive Information Disclosure |
| 3 | Human-in-the-loop requirement — no AI-generated clinical content enters the EHR without a licensed clinician reviewing and attesting to it | Compensating | MITRE ATLAS — Human-AI teaming; Google SAIF — human oversight |

### Securing Operations

| # | Operational Safeguard | Compliance Anchor |
|---|---|---|
| 1 | Conduct the formal HIPAA Security Rule §164.308 risk analysis that was skipped before go-live | HIPAA Security Rule |
| 2 | Staff AI literacy training — "HIPAA compliant in principle" does not mean staff know what is safe to input | NIST CSF PR.AT — Awareness and Training |
| 3 | Audit logging and model versioning — tamper-evident log of every query and response; version-pinned model for incident reconstruction | NIST CSF DE.CM — Continuous Monitoring |

---

## The Red Herring

> *"The data never leaves our network."*

This is a perimeter-security argument being used to dismiss an AI risk argument. The threat in this scenario does not require data to leave Maplewood's network. The risk lives entirely inside the perimeter: an over-permissioned shared API key, potential context contamination between patient encounters, and a model that can surface PHI in unexpected ways — all without a single byte crossing the firewall. Perimeter controls and AI-specific controls are not the same thing.

---

## NIST CSF 2.0 Mapping

| CSF Function | Gap in this scenario |
|---|---|
| GOVERN · GV.RM | No formal risk assessment was conducted before go-live |
| IDENTIFY · ID.AM | Shared API key is untracked with unclear scope and ownership |
| PROTECT · PR.AA | No least-privilege enforcement — one key used by four applications |
| DETECT · DE.CM | No logging or monitoring of model queries or responses |

---

*Part of the [AI Cyber GRC Workshop Analysis](./README.md) · WiCyS 2025 · Sherilyn Rong*
