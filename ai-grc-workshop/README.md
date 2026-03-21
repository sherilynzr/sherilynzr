# AI Cyber GRC — Risk Assessment Workshop

**Source:** WiCyS 2025 Conference  
**Workshop:** *AI Cyber GRC: Striking the Tricky Balance Between Managing Risks of AI vs. Risks to AI*  
**Presenter:** Shafia Zubair — Director of GRC · UC Chicago Professor  
**Frameworks:** Google Secure AI Framework · MITRE ATLAS · OWASP Top 10 LLM · CSA MAESTRO · NIST CSF 2.0

---

## About this workshop

This hands-on exercise presented three fictional AI deployment scenarios and asked participants to analyze each across three dimensions simultaneously:

- **Risk Assessment** — identifying the top AI-specific risks using the Google Secure AI Framework
- **Security Controls** — recommending controls using MITRE ATLAS, OWASP Top 10 LLM, and CSA MAESTRO
- **Securing Operations** — identifying ongoing governance and operational safeguards using Security Fundamentals

Each scenario was written with deliberate red herrings: details designed to sound like evidence of due diligence that, on closer inspection, don't address the actual risk. Part of the exercise was learning to separate those signals from the real gaps.

---

## The Core Framework

The workshop introduced a foundational distinction that structured every scenario:

| | Definition | Examples |
|---|---|---|
| **Risk TO AI** | Threats that attack or degrade the AI system itself | Data poisoning, adversarial inputs, model tampering, supply chain compromise, firmware hijack |
| **Risk OF AI** | Harms the AI system causes when functioning as intended | Hallucinated content, biased outputs, privacy leakage, non-compliant decisions |

A second model — the three-gear operating process — provides a structural checklist for any AI risk assessment:

```
Infrastructure  ⟷  Model  ⟷  Data
```

Before identifying specific risks, run through all three gears: what infrastructure risks exist, what model risks exist, and what data risks exist.

---

## Scenarios

| Scenario | Company | Deployment Type | Key Risk Themes |
|---|---|---|---|
| [Scenario 1](./scenario-01-maplewood-health.md) | Maplewood Health System | Internal LLM · Healthcare | PHI exposure · shared API key · HIPAA · hallucination risk |
| [Scenario 2](./scenario-02-northbridge-msp.md) | NorthBridge IT Solutions | Customer-facing LLM · IT MSP | Data poisoning · supply chain · GDPR · vendor risk |
| [Scenario 3](./scenario-03-terraedge-agriculture.md) | TerraEdge Equipment Co. | Physical AI · Autonomous robots | Adversarial vision attacks · firmware hijack · EU AI Act |

---

## Patterns Across All Three Scenarios

**"Someone reviewed it" is not risk management.**  
Legal reviewed the NorthBridge contract. The CISO approved Maplewood's pilot. The product team resolved TerraEdge's legal flag. In each case a review happened — but none constituted a formal risk assessment. Review ≠ assessment.

**Supply chain risk appears in every scenario.**  
Maplewood fine-tuned on a third-party foundation model. NorthBridge licensed from Vektora AI. TerraEdge used a third-party annotation vendor. In all three cases, the organization's risk posture depends on a vendor whose security posture is unverified. This is GV.SC (NIST CSF Supply Chain Risk Management) in practice.

**Functional adequacy is not security adequacy.**  
Staff love CareScribe. Helios gets good support ratings. TerraEdge's QA passes. User satisfaction and functional quality are not evidence that security controls are effective.

**Regulatory context changes the operations answer.**  
HIPAA, GDPR, and the EU AI Act each impose materially different obligations. You cannot apply a generic security baseline across all three and call it done.

---

*WiCyS 2025 · Sherilyn Rong · CRISC candidate*
