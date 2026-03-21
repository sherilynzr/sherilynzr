# Scenario 3 — Physical AI Solution: Agriculture Equipment Manufacturer

**Company:** TerraEdge Equipment Co.  
**Industry:** Agriculture · Autonomous equipment manufacturing  
**AI Deployment Type:** Physical AI · Autonomous field robots  
**Frameworks:** Google Secure AI Framework · MITRE ATLAS · OWASP Top 10 LLM · CSA MAESTRO · NIST CSF 2.0

---

## The Situation

TerraEdge Equipment Co. designs and manufactures autonomous field robots used by large-scale commercial farms across North America and the EU. Their fleet of over 2,400 deployed machines handles precision crop spraying, obstacle detection, crop health assessment, and autonomous GPS-waypoint navigation at speeds up to 12 mph. The AI system at the core of each robot is a **computer vision model built in-house** by TerraEdge's engineering team.

Each robot transmits telemetry data back to TerraEdge's **cloud-based fleet dashboard over a cellular connection**. Field technicians have the ability to **push firmware updates remotely** — the machine does not need to return to a service center for updates. The AI vision model is updated on a quarterly schedule.

TerraEdge's engineering team runs **functional QA testing** on each firmware release before it goes out to the fleet. However, the team does not perform adversarial security testing on the model itself. The most recent quarterly update introduced a new obstacle detection module. That module was trained on a **dataset licensed from a third-party annotation vendor** whose security practices have not been reviewed by TerraEdge.

Three months ago, TerraEdge's legal team flagged that several EU clients operate in jurisdictions where the **EU AI Act may classify autonomous navigation systems as high-risk AI**, which would trigger traceability, documentation, and human oversight requirements. The product team reviewed the flag and closed it, noting that the robots **"only operate in agricultural fields, not near people."**

TerraEdge's cyber insurance policy covers equipment damage but **explicitly excludes AI-related incidents**.

---

## What You Need to Figure Out

Working from the scenario above, identify the following for TerraEdge Equipment Co.:

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

> **Note:** Physical AI introduces a risk dimension that software-only deployments don't have — model failure can directly cause physical harm. Keep that harm pathway in mind as you work through each column.

---

## Analysis

### Risk Assessment

| # | Risk | Reasoning |
|---|---|---|
| 1 | Adversarial input attack on the vision model | Physical adversarial markers — stickers, printed patterns, or environmental modifications — placed on obstacles could cause the vision model to misclassify and fail to stop. At 12 mph, a misclassification is a collision event. MITRE ATLAS documents this as an evasion attack: the attacker doesn't breach the system, they manipulate what the model perceives. |
| 2 | Training data supply chain compromise | The third-party annotation vendor's dataset for the new obstacle detection module has not been security-assessed. Poisoned training labels could systematically teach the model to ignore specific object types — such as a particular color of clothing worn by farmworkers — without any visible sign during functional QA testing. |
| 3 | Remote firmware update hijack | The cellular update channel for a fleet of 2,400+ machines is a high-value target. If the channel lacks strong cryptographic integrity verification, an attacker does not need to compromise individual machines — they can push a malicious model version to the entire fleet simultaneously. |

### Security Controls

| # | Control | Control Type | Framework Reference |
|---|---|---|---|
| 1 | Adversarial robustness testing before each quarterly model update — include physical-world adversarial examples specific to agricultural environments | Preventive | MITRE ATLAS — AML.T0043: Craft Adversarial Data |
| 2 | Cryptographically signed firmware — robots reject any update package that is unsigned or fails integrity verification | Preventive | Google SAIF — model integrity; CSA MAESTRO — supply chain integrity |
| 3 | Anomaly detection on telemetry — flag unusual navigation patterns, sudden obstacle-detection failure rates, or abnormal sensor behavior as potential attack indicators | Detective | NIST CSF DE.CM — Continuous Monitoring |

### Securing Operations

| # | Operational Safeguard | Compliance Anchor |
|---|---|---|
| 1 | EU AI Act classification review — autonomous navigation systems operating near people (farmworkers, contractors, bystanders) likely qualify as high-risk AI under Annex III; traceability, documentation, and human oversight obligations must be assessed by qualified legal counsel | EU AI Act Annex III |
| 2 | Physical safety kill switch and human override capability — every machine must have a reliable emergency stop that does not depend on the AI model functioning correctly | Safety engineering baseline |
| 3 | Cyber insurance gap escalation — the current policy explicitly excludes AI incidents; this represents an uninsured residual risk that must be formally accepted at the executive or risk committee level with documented rationale | CRISC Domain 3 — Risk Response: Accept with documentation |

---

## The Red Herrings

> *"Functional QA passed."*

Functional QA tests whether the model performs its intended task correctly under normal conditions. It does not test whether the model is resilient to adversarial manipulation, poisoned training data, or unexpected real-world edge cases. A model can pass every functional test and still be vulnerable to the attacks described above. These are different testing disciplines.

> *"Only operates in agricultural fields, not near people."*

This is factually incorrect. Commercial farms employ farmworkers, equipment operators, contractors, irrigation technicians, and inspectors. Claiming a robot doesn't operate near people because it operates in a field does not hold up legally or factually — and it does not satisfy the EU AI Act's classification criteria.

> *"The product team reviewed the legal flag and closed it."*

Closing a legal flag does not resolve the underlying regulatory question. A product team is not the appropriate decision-maker for EU AI Act compliance classification — that determination requires qualified legal review. The flag being closed is an escalation failure, not a resolved risk. Under CRISC principles, this risk was effectively accepted without documentation or appropriate authority.

---

## NIST CSF 2.0 Mapping

| CSF Function | Gap in this scenario |
|---|---|
| GOVERN · GV.RM | EU AI Act legal flag closed by product team without appropriate escalation or documentation |
| GOVERN · GV.SC | Third-party annotation vendor not assessed — AI training data supply chain unmanaged |
| IDENTIFY · ID.RA | No adversarial threat modeling performed on the vision system |
| PROTECT · PR.PS | No firmware integrity verification; update channel is an unprotected attack surface |

---

*Part of the [AI Cyber GRC Workshop Analysis](./README.md) · WiCyS 2025 · Sherilyn Rong*
