# AI Threat Modeling

## Structured Threat Analysis for AI-Native Systems

---

## Introduction

Threat modeling is the most consequential defensive activity an organisation can perform before deploying a new system. It identifies what can go wrong, who might cause it, and where controls are most needed — before adversaries answer those questions empirically. For conventional software systems, established frameworks exist: STRIDE identifies spoofing, tampering, repudiation, information disclosure, denial of service, and elevation of privilege. PASTA structures threat analysis around business objectives and attack simulation. LINDDUN addresses privacy threat categories systematically.

These frameworks were designed for systems with deterministic behaviour, well-defined trust boundaries, and static data flows. AI-native systems have none of these properties.

A language model's trust boundary shifts with every prompt. A RAG pipeline's data flow changes with every retrieval. An agent's action surface expands as its tool set grows. Applying conventional threat modeling frameworks to these systems without adaptation produces incomplete threat models — not because the frameworks are wrong, but because they were not designed for the architectural realities they are now being asked to address.

This document describes why AI systems require dedicated threat modeling, where conventional frameworks fall short, how those frameworks can be adapted for AI-specific architectures, and what the outputs of structured AI threat modeling enable for enterprise security governance.

Threat modeling is currently the least-practiced defensive activity in enterprise AI deployments — in part because it has historically required expensive specialist consultancy. Automating structured AI threat modeling is one of the most consequential capabilities an AI security platform can provide.

---

## 1. Why AI Systems Require Dedicated Threat Modeling

### Trust Boundaries Are Dynamic and Implicit

In a conventional three-tier web application, trust boundaries are explicit and stable: the browser trusts the application server; the application server trusts the database within defined query parameters; external inputs are untrusted. These boundaries can be drawn on a data flow diagram and remain accurate across the system's operational lifetime.

In an AI-native system, trust boundaries are dynamic and often implicit:

- The **prompt boundary** separates trusted system instructions from untrusted user input — but this boundary is not enforced by a security control; it is maintained by the model's interpretation of its context, which can be manipulated.
- The **retrieval boundary** separates the content the RAG system was designed to retrieve from content that could be adversarially introduced — but this boundary depends on the integrity of the retrieval corpus, which changes continuously.
- The **agentic action boundary** separates actions the agent is authorised to take from actions it could be manipulated into taking — but this boundary is defined by the interaction of tool permissions, reasoning behaviour, and context, not by a single enforceable control.
- The **embedding boundary** separates the semantic space the embedding model was designed to represent from adversarially crafted inputs designed to exploit that representation — a boundary with no direct conventional equivalent.

None of these boundaries appear on a conventional data flow diagram. A threat model that does not represent them cannot reason about the attacks that exploit them.

### Data Flows Include Non-Deterministic Processing Stages

Conventional data flow diagrams represent data moving between processes, data stores, and external entities through defined channels. The data transformation at each stage is deterministic and reviewable.

AI systems introduce processing stages — model inference, embedding generation, retrieval ranking, agent reasoning — that are non-deterministic and not directly reviewable. The same input can produce different outputs. The same retrieved document can have different effects on model behaviour depending on context. A threat model that cannot represent non-deterministic processing stages cannot reason about the attack classes that exploit non-determinism.

### Attack Classes Have No Conventional Equivalent

Several of the most consequential AI attack classes do not map to conventional threat categories without significant adaptation:

- Prompt injection is simultaneously spoofing (impersonating an authorised instruction source), tampering (modifying the effective instruction set), and potentially elevation of privilege (gaining capabilities not granted to the user role) — but it operates through a mechanism — natural language interpretation — that STRIDE was not designed to model.
- Hallucination as an information disclosure vector — where a model generates plausible but false information that an organisation then acts on — has no direct conventional equivalent.
- Agentic goal drift — where an agent's behaviour progressively diverges from its intended objective through accumulated context manipulation — is an elevation of privilege attack that operates across time rather than at a single decision point.
- Membership inference — where an adversary determines whether a specific data record was present in a model's training data through targeted queries — is a privacy threat that LINDDUN's conventional categories do not fully capture.

---

## 2. Limitations of Applying Conventional Frameworks Without Adaptation

### STRIDE Applied to AI Systems

STRIDE is a well-established threat enumeration framework. Applied naively to an AI system, it produces a threat model that captures some risks and misses others systematically.

**What STRIDE captures adequately:** Network-layer threats to the infrastructure hosting the AI system; authentication and authorisation threats to the API layer; data integrity threats to training data pipelines at the storage layer.

**What STRIDE misses without adaptation:** Prompt injection as a combined spoofing-tampering-elevation vector operating through the model's interpretation layer; the prompt boundary as a trust boundary requiring explicit representation; retrieval manipulation as a tampering threat operating through the data pipeline rather than the storage layer; agentic goal drift as an elevation of privilege threat that accumulates across reasoning steps rather than occurring at a single decision point; tool-invocation hijacking as an elevation of privilege threat that operates through the agent's reasoning rather than through permission bypass.

### LINDDUN Applied to AI Systems

LINDDUN is a privacy threat framework covering linkability, identifiability, non-repudiation, detectability, disclosure of information, unawareness, and non-compliance. Applied to AI systems without adaptation, it misses several AI-specific privacy threat classes.

**What LINDDUN misses without adaptation:** Training data reconstruction — where an adversary uses targeted model queries to reconstruct training data records — is an identifiability and disclosure threat not covered by LINDDUN's conventional attack patterns. Embedding-space membership inference — where retrieval query patterns reveal whether a document is present in the retrieval corpus — is a detectability threat specific to RAG architectures. Prompt history inference — where model behaviour reveals information about prior conversations or system prompt content — is an identifiability threat that has no conventional data flow equivalent.

### PASTA Applied to AI Systems

PASTA's process-for-attack-simulation-and-threat-analysis approach is well-suited to AI systems in principle — its business-objective grounding and attack simulation orientation are appropriate for the non-deterministic behaviour of AI systems. The limitation is at the attack pattern layer: PASTA relies on attack pattern libraries that were not developed with AI-specific attack classes in mind. Applied without an AI-extended attack pattern library, PASTA produces a simulation that misses the most consequential AI-specific attack paths.

---

## 3. An AI-Adapted Threat Modeling Approach

### Extending the Data Flow Diagram for AI Systems

AI-adapted data flow diagrams must represent components and flows that do not appear in conventional DFDs:

- **Embedding stores** as data stores with specific integrity requirements and specific attack surface (embedding manipulation, membership inference)
- **Retrieval pipelines** as processing stages with non-deterministic output and specific trust requirements (source integrity, retrieval boundary enforcement)
- **Prompt boundaries** as trust boundaries between the system instruction layer and the user input layer — explicitly drawn and explicitly assessed
- **Agentic action boundaries** as trust boundaries between the agent's reasoning layer and the tool invocation layer — with explicit representation of what actions are authorised and what conditions could cause unauthorised action
- **Inter-agent communication channels** in multi-agent architectures — as data flows with specific trust and integrity requirements distinct from conventional API calls
- **MCP server connections** as external entity relationships with specific trust boundary and integrity requirements

### Extending STRIDE for AI Systems

Each STRIDE category requires extension to cover AI-specific threat instantiations:

| STRIDE Category | Conventional Threat | AI-Specific Extension |
|---|---|---|
| Spoofing | Identity impersonation at authentication layer | Prompt injection impersonating authorised instruction source; tool description poisoning misrepresenting tool identity to AI client |
| Tampering | Data modification in transit or at rest | Retrieval corpus poisoning; embedding manipulation; adversarial document injection; MCP tool description modification |
| Repudiation | Denial of actions taken | Agent action logging gaps enabling denial of agentic actions; absence of audit trail for tool invocations |
| Information Disclosure | Unauthorised data access | Training data extraction through targeted queries; retrieval boundary violations exposing out-of-scope content; cross-tenant retrieval leakage; system prompt extraction |
| Denial of Service | Resource exhaustion | Model denial of service through adversarial input crafting; retrieval system overload through query manipulation; agent-loop denial through reasoning exhaustion attacks |
| Elevation of Privilege | Permission bypass | Agentic goal drift producing unauthorised actions; cross-tool privilege escalation through reasoning chains; agent-to-agent privilege escalation in multi-agent architectures |

### Extending LINDDUN for AI Privacy Threat Analysis

AI-specific extensions to LINDDUN include:

- **Training data reconstruction** as an identifiability and disclosure threat — requiring assessment of whether model query patterns can be used to reconstruct training data records
- **Embedding-space membership inference** as a detectability threat — requiring assessment of whether retrieval query patterns reveal corpus membership
- **Prompt history inference** as an identifiability threat — requiring assessment of whether model behaviour reveals prior conversation content or system prompt structure
- **Cross-session context leakage** in persistent-memory agent deployments — as a linkability threat requiring explicit assessment of memory isolation across sessions and users

### Incorporating MITRE ATLAS

MITRE ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems) provides a structured taxonomy of adversarial techniques targeting AI systems, analogous to MITRE ATT&CK for conventional systems. AI threat modeling should incorporate ATLAS technique mapping as a standard component — identifying which ATLAS techniques are relevant to each component of the system under assessment and ensuring that the threat model accounts for documented adversarial behaviour.

Key ATLAS technique categories for enterprise AI threat modeling include: ML model access (black-box, white-box, transferability-based); data poisoning (training data, retrieval corpus, embedding store); adversarial examples (evasion attacks, perturbation-based attacks); model inversion and extraction; and LLM-specific techniques including prompt injection, jailbreaking, and indirect injection.

---

## 4. Threat Modeling as a Pre-Assessment Activity

Threat modeling and security testing are complementary, not interchangeable. Threat modeling identifies the attack surface and the priority attack paths. Security testing validates whether those paths are exploitable. Performing security testing without a preceding threat model produces coverage that is broad but not necessarily directed at the most consequential risks.

For AI systems specifically, threat modeling before assessment produces three concrete benefits:

**Scope definition.** A structured threat model identifies which components require assessment, which trust boundaries require validation, and which attack classes are most relevant to the specific architecture under review. This directs assessment effort toward the highest-risk surface rather than distributing it uniformly across all possible test cases.

**Assessment prioritisation.** Not all identified threats have equal business impact. A threat model grounded in business objectives — consistent with the PASTA approach — produces a risk-ranked threat list that allows assessment resources to be allocated to the threats that matter most to the deploying organisation.

**Governance documentation.** A completed AI threat model is a governance artefact. It demonstrates that the organisation has systematically identified the risks its AI system presents, assessed their likelihood and impact, and directed security controls toward the highest-priority risks. This is the form of evidence that NIST AI RMF Govern and Map functions, EU AI Act conformity assessment requirements, and ISO/IEC 42001 risk management clauses call for — and that conventional security assessment reports do not produce.

---

## 5. Standards Alignment

AI threat modeling as described in this document aligns with the following framework requirements:

**NIST AI RMF**
- Govern 1.0: Organisational practices address AI risk — threat modeling is the mechanism by which AI risk is systematically identified
- Map 2.0: Scientific findings and context are used to identify AI risks — threat modeling incorporates current AI attack taxonomy (MITRE ATLAS, OWASP LLM Top 10 v1.1)
- Map 5.0: Likelihood and impact of identified risks are assessed — threat model risk ranking provides this assessment
- Measure 2.5: The AI system to be deployed is demonstrated to be valid and reliable for its intended use — threat model outputs inform the scope of validation testing

**EU AI Act**
- Article 9: Risk management system — AI threat modeling is a structured implementation of Article 9's requirement for systematic risk identification and analysis
- Article 15: Accuracy, robustness, and cybersecurity — threat model outputs define the robustness and cybersecurity requirements against which the system must be validated

**ISO/IEC 42001**
- Clause 6.1: Actions to address risks and opportunities — AI threat modeling provides the structured risk identification input this clause requires
- Clause 8.4: AI system operation — threat model outputs inform operational controls and monitoring requirements

**OWASP LLM Top 10 v1.1**
AI threat modeling should explicitly assess exposure to each of the ten risk categories, using the threat model's component and data flow representation to identify which categories are relevant to the specific system architecture under review.

---

## 6. Practical Implications for Enterprise AI Deployments

Three conclusions follow for organisations deploying AI systems.

**Threat modeling should precede deployment, not follow it.** The cost of identifying an architectural risk through threat modeling is remediation at design time. The cost of identifying the same risk through a post-deployment security assessment is remediation in production. For AI systems, where architectural decisions about model access, retrieval scope, agent tool permissions, and MCP server trust relationships have long-term security implications, pre-deployment threat modeling is a cost-reduction measure as much as a risk-reduction measure.

**Threat modeling outputs should be maintained as living documents.** An AI system's threat model is not valid indefinitely. As the retrieval corpus changes, as agent tool sets expand, as MCP server connections are added, the threat model requires updating. Organisations that treat the threat model as a one-time pre-deployment artefact will find it progressively less representative of the actual risk surface of the deployed system.

**Automated threat modeling enables consistent governance at scale.** Manual threat modeling by specialist consultants is expensive, inconsistent, and not repeatable at the cadence that AI system evolution requires. Automating structured AI threat modeling — producing standards-aligned threat models with STRIDE, LINDDUN, and MITRE ATLAS mappings as a routine part of the AI development and deployment lifecycle — is the only model that makes systematic AI risk governance achievable at enterprise scale.

---

## Standards and Framework Reference

| Framework | Relevant Coverage |
|---|---|
| OWASP LLM Top 10 v1.1 | Full taxonomy — all ten risk categories relevant to AI threat modeling scope |
| MITRE ATLAS | Full technique taxonomy — adversarial ML techniques mapped to threat model components |
| NIST AI RMF | Govern 1.0, Map 2.0, Map 5.0, Measure 2.5 |
| EU AI Act | Article 9 Risk Management System, Article 15 Accuracy and Robustness |
| ISO/IEC 42001 | Clause 6.1 Risk Assessment, Clause 8.4 AI System Operation |
| STRIDE | Extended for AI-specific threat categories as described in Section 3 |
| LINDDUN | Extended for AI privacy threat categories as described in Section 3 |
| PASTA | Process for Attack Simulation and Threat Analysis — applicable with AI-extended attack pattern library |

---

*This document is maintained as part of the RedForce AI public documentation repository. It reflects current methodology for AI-adapted threat modeling and will be updated as frameworks, attack taxonomies, and enterprise AI architectures evolve.*
