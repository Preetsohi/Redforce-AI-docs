# Problem Statement

## Why Existing Security Tools Are Insufficient for AI-Native Systems

---

## 1. The Transformation of the Application Landscape

For most of the history of enterprise software, an application was a defined artefact: a codebase with known inputs, deterministic logic, and predictable outputs. Security disciplines evolved accordingly. Threat modelling assumed bounded behaviour. Testing verified known conditions. Compliance frameworks mapped controls to static assets.

That foundational assumption no longer holds.

The modern enterprise application is increasingly AI-native — not merely software that uses AI as a feature, but systems in which large language models, retrieval-augmented pipelines, autonomous agents, and external model integrations are core to how business logic is executed. Decisions that were once encoded as rules are now inferred. Outputs that were once deterministic are now probabilistic. Behaviour that was once reviewable in source code is now emergent from model weights, prompt configurations, and runtime context that no static asset can fully represent.

This transformation is not gradual. The adoption curve for AI-integrated systems across financial services, healthcare, legal, and critical infrastructure sectors has compressed what would ordinarily be a decade of architectural evolution into a period of two to three years. Security programmes have not kept pace. The gap between the sophistication of AI deployment and the maturity of AI security practice is widening — and the consequences are beginning to manifest in ways that traditional risk frameworks are not equipped to measure.

---

## 2. New Attack Surfaces Introduced by AI-Native Architectures

The shift to AI-native systems introduces attack surfaces that have no direct precedent in conventional application security. Four categories are particularly consequential for enterprise risk.

**Large Language Models (LLMs).** LLMs process natural language as both instruction and data, and they do so without a clear, enforceable boundary between the two. This creates a class of vulnerability — prompt injection, jailbreaking, system prompt extraction, and goal hijacking — that has no analogue in traditional injection attacks. Unlike SQL injection, which exploits a failure of parameterisation, prompt injection exploits the fundamental architecture of how language models process context. There is no patch that eliminates it; there is only mitigation, detection, and monitoring.

**Retrieval-Augmented Generation (RAG) Systems.** RAG architectures introduce a dynamic data layer between the model and the enterprise knowledge base. This layer is a new attack surface in multiple dimensions: adversarially crafted documents embedded in the retrieval corpus can manipulate model outputs through indirect prompt injection; source-document injection, chunk-boundary exploitation, and embedding integrity attacks can cause systems to return attacker-influenced content without visible indication of compromise; and in multi-tenant deployments, inadequate retrieval isolation can result in cross-tenant data leakage at the retrieval layer. Each of these vectors operates beneath the visibility of conventional application scanning.

**Model Context Protocol (MCP) Servers.** As AI systems are increasingly connected to enterprise tools — code repositories, communication platforms, databases, file systems — through protocol layers such as MCP, the trust boundary of the AI system expands dramatically. MCP server compromise, tool description poisoning, and cross-server privilege escalation represent an emerging and largely uncharted threat surface. An agent granted access to internal systems through a context protocol carries implicit permissions that may exceed what any human operator explicitly authorised, and the trust relationships between MCP clients and servers are not governed by conventional access control frameworks.

**AI Agents.** Autonomous agents that plan, act, and iterate across multiple steps introduce compounding risk. Tool-invocation hijacking, semantic tool-call boundary violations, agent-loop attacks, and cross-tool privilege escalation through reasoning chains are attack classes specific to agentic architectures that have no direct conventional equivalent. The behaviour of a multi-step agent is not derivable from inspecting any single component — it emerges from the interaction of model reasoning, environmental context, and available actions — and it can diverge from intended behaviour in ways that are difficult to anticipate, reproduce, or detect after the fact.

---

## 3. The Limits of Conventional Security Testing

The security industry has three primary mechanisms for validating application security: static analysis (SAST), dynamic analysis (DAST), and human-led penetration testing. Each has significant limitations when applied to AI-native systems. Critically, the correct response to these limitations is not to abandon conventional testing — it is to extend it. AI security requires coverage of both the AI-specific attack surface and the foundational application, API, and infrastructure layers that AI systems depend on. A RAG system with a hardened retrieval pipeline but an exposed admin API is not a secure AI system. Conventional tooling remains necessary for these foundational layers; it is insufficient for the AI-specific layers above them.

**Static analysis** examines source code for known vulnerability patterns. AI-native systems derive much of their behaviour from model weights, prompt templates, and runtime configuration — none of which are amenable to static inspection. A SAST tool that scores a codebase as clean provides no assurance about the security of the AI system that codebase orchestrates. Static analysis remains valuable for the application and infrastructure code surrounding an AI system; it cannot assess the AI system itself.

**Dynamic analysis** probes running applications for exploitable conditions by sending known-bad inputs and observing responses. This approach was designed for systems with deterministic, rule-based logic. LLMs are non-deterministic: the same input can produce different outputs across invocations, and safety-relevant behaviour can be present in one context and absent in another. Standard DAST payloads are not designed to surface prompt injection, indirect context manipulation, RAG pipeline exploitation, or agent goal deviation. Coverage statistics generated by conventional DAST scanners against AI endpoints are not meaningful measures of AI security assurance — though DAST remains essential for the web application and API layers that AI systems are accessed through.

**Penetration testing** provides the deepest form of human-led validation but is constrained by time, scope, and the expertise available at the moment of engagement. The threat landscape for AI systems is evolving faster than the pool of practitioners qualified to test it. Point-in-time penetration tests conducted against a system whose AI behaviour, model version, prompt configuration, or retrieval corpus may change within weeks of the engagement completion date provide diminishing assurance value. Additionally, the labour cost of manual penetration testing does not scale to the volume and velocity of AI system deployments that enterprise organisations now face.

Taken together, these tools assess a fundamentally different class of system than the AI-native applications they are increasingly applied to. The coverage they provide for foundational layers is real and necessary. For the AI-specific attack surface, it is structurally insufficient.

---

## 4. The Case for Continuous AI Security Validation

Enterprise security programmes have historically accepted periodic assessment as a reasonable proxy for ongoing assurance. For stable systems with infrequent change cycles, this was a defensible position. For AI-native systems, it is not.

AI systems change continuously and often silently. Model updates alter behavioural boundaries without triggering a software release. Prompt configurations are modified by product teams outside security review cycles. Retrieval corpora are updated with content that may introduce indirect injection vectors. Agent tool permissions expand as integrations grow. MCP server connections are added as enterprise tooling expands. None of these changes are guaranteed to trigger a security assessment under conventional governance models.

The implication is that point-in-time assessment creates a false assurance effect: organisations believe their AI systems have been validated when, in practice, the validated state no longer reflects the deployed state. The delta between the two is unmeasured and unmanaged risk.

Continuous AI security validation — persistent, automated testing of AI system behaviour against a consistent and evolving threat baseline — is the only model that matches the actual change velocity of AI-native systems. It does not replace human expertise or human governance accountability; it provides the continuous signal that makes human expertise actionable and gives security leadership the current, evidence-based picture of posture that governance decisions require.

---

## 5. Governance, Accountability, and the Human Chain of Custody

Technical testing addresses exploitability. It does not, by itself, address accountability, auditability, or compliance — and in enterprise environments subject to emerging AI regulation, these are not optional considerations.

The EU AI Act, NIST AI RMF, ISO/IEC 42001, and a growing body of sector-specific AI governance requirements are establishing expectations that go beyond vulnerability counts. They require organisations to demonstrate that AI systems are tested against known risk taxonomies, that findings are traceable to authoritative standards, that security posture is monitored over time, and that evidence of due diligence is available for audit.

Equally important — and less commonly addressed by AI security tooling — is the requirement for a human accountability chain. SOC 2, ISO 27001, and PCI-DSS require that risk decisions are made and signed off by named, accountable individuals within the organisation. An AI security programme in which findings are generated automatically but never formally acknowledged by a named security governance role does not satisfy this requirement. The humans in the loop — the CISO, Security Lead, or SOC Head — must be demonstrably involved: authorising assessment scope, acknowledging findings, and approving remediation actions. This is not a UX consideration; it is the mechanism by which a security programme is defensible under audit and reportable to a board risk committee.

Conventional security tooling was not designed to produce this kind of structured, human-accountable evidence for AI systems. Enterprise organisations therefore require AI security platforms that combine rigorous technical testing across the full stack with structured governance outputs: findings mapped to current standards, human acknowledgement and approval workflows that create a documented accountability record, security posture tracked over time, and evidence packages that support regulatory and board-level reporting. Technical capability and governance accountability are not separable concerns. They are two aspects of the same requirement.

---

## Conclusion

The application security discipline is confronting a structural inflection point. The systems it is asked to protect have changed in kind, not merely in complexity. The attack surfaces are new, the threat actors are adapting faster than the defenders, and the tools that defined two decades of practice are insufficient for the AI-specific layers of the systems they are now asked to secure.

Addressing this gap requires a precise understanding of what is actually needed: AI-native systems require security validation that covers the full stack — foundational application, API, and infrastructure layers assessed with extended conventional tooling, and AI-specific layers assessed with methodology purpose-built for their characteristics. That validation must be continuous, because AI systems change continuously. It must be standards-aligned, because governance and regulatory obligations require authoritative evidence. And it must maintain a documented human accountability chain, because the security decisions that matter most — what to assess, what risk to accept, what to remediate — must remain with named, responsible individuals within the deploying organisation.

The cost of deferring that understanding is not theoretical. It is measured in the exploitable exposure that accumulates in every AI system assessed by tools not designed to find it, and in the governance liability that accumulates in every organisation that cannot demonstrate who was accountable for its security decisions.

---

*This document is maintained as part of the RedForce AI public documentation repository. It reflects the state of the AI security landscape as understood at the time of publication and will be updated as the field evolves.*
