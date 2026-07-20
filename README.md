# RedForce AI
## Enterprise AI Security Validation Platform

RedForce AI is a proprietary, autonomous security testing platform designed to validate the security posture of AI-native systems across the full stack they run on — from the model and its interfaces to the applications, APIs, data pipelines, and infrastructure that support them. It is built for enterprise environments where security validation must be continuous, auditable, and privacy-preserving.

This repository contains public documentation covering the platform's vision, design principles, high-level capabilities, and use cases. The source code is private.

---

## Vision

Traditional security testing was designed for a different era. Static analysis, network scanning, and conventional penetration testing address a well-understood attack surface — one defined by deterministic inputs, outputs, and logic.

AI-native systems do not behave deterministically. A large language model does not execute instructions; it interprets them. A retrieval-augmented generation pipeline does not query a database; it synthesises context from distributed knowledge sources. An autonomous agent does not follow a fixed program; it reasons, plans, and acts across tool boundaries in ways that vary with every invocation.

This architectural shift creates a class of vulnerabilities that conventional security tooling was not built to detect: vulnerabilities that emerge from model behaviour, context manipulation, tool misuse, and the interaction between AI components and enterprise data systems.

Critically, AI security is not only about the model. A production AI system has attack surface at every layer: the model and its interfaces, the web application or API through which it is accessed, the infrastructure it runs on, the data pipeline that feeds it, and the network through which it communicates. A chatbot embedded in a web application inherits the full web attack surface of that application. An AI-driven agent running on a server inherits the hardening posture of that server. A cloud-hosted AI system inherits the configuration risks of the cloud environment. Securing the model in isolation while leaving these layers unvalidated is not AI security — it is partial security.

RedForce AI is designed to address this reality. Its purpose is to provide enterprise security teams with a systematic, repeatable, and governance-aligned approach to validating AI system security across the entire stack, at every stage of the development and deployment lifecycle.

---

## The Problem

Enterprise organisations adopting AI face a security validation challenge that is qualitatively different from anything that preceded it.

**Full-stack attack surface expansion.** Every AI integration introduces new attack surface at multiple layers simultaneously — the model interface, the surrounding application, the API layer, the data pipeline, and the underlying infrastructure. These surfaces interact and compound. A vulnerability in the web application exposing an AI chatbot can be exploited to manipulate the model's behaviour. A misconfigured cloud environment hosting an AI system can expose the data it processes. Treating each layer in isolation produces gaps that adversaries exploit.

**Prompt injection at scale.** Prompt injection — the manipulation of model behaviour through crafted inputs — has been demonstrated across production systems, including those deployed by major technology providers. As AI systems are granted more autonomy and access to enterprise tooling, the consequences of successful injection escalate accordingly.

**RAG pipeline integrity.** Retrieval-augmented systems depend on the integrity of their knowledge sources. Adversarial manipulation of retrieval corpora — through document poisoning, embedding manipulation, chunk-boundary exploitation, or cross-tenant retrieval leakage — can cause systems to return incorrect, harmful, or attacker-controlled outputs without visible indication of compromise.

**Agent abuse and insecure tool invocation.** Autonomous agents operate by invoking external tools — APIs, databases, code interpreters, communication systems. Security boundaries that would be enforced by conventional access controls are often absent or insufficiently defined in agentic architectures. Tool-invocation hijacking, semantic boundary violations across tool chains, and agent-loop attacks represent an emerging and largely uncharted enterprise risk surface.

**Model Context Protocol (MCP) security.** As AI systems connect to enterprise tooling through protocol layers such as MCP, the trust boundary of the AI system expands dramatically. MCP server compromise, tool description poisoning, and cross-server privilege escalation are active threat vectors with limited current tooling coverage.

**AI governance and auditability.** Regulatory and compliance frameworks — including the EU AI Act, NIST AI RMF, and ISO/IEC 42001 — require that AI systems demonstrate not only functional performance but security assurance. Organisations need the ability to produce audit-ready evidence of security validation, mapped to recognised standards, and communicable to governance bodies, regulators, and executive leadership.

**Continuous validation.** AI systems are updated, fine-tuned, retrained, and extended continuously. A security assessment performed at deployment does not remain valid as the system evolves. Enterprise AI security requires a continuous validation model, not a point-in-time audit.

---

## Design Principles

RedForce AI is designed around principles that reflect the requirements of enterprise deployment and the realities of AI security practice.

**Zero Data Egress.** Security validation of enterprise AI systems must not require the transmission of proprietary data, model outputs, or internal context to external services. RedForce AI operates entirely within the customer's environment. No telemetry, no cloud dependency, no data leaves the boundary.

**Privacy by Design.** Data minimisation and access control are built into the platform architecture, not added as configuration. The platform does not require access to production data to perform security validation.

**Security by Default.** The platform's own security posture is treated with the same rigour it applies to the systems it evaluates. Secure defaults, minimal permissions, and audit logging are standard, not optional.

**Human-in-the-Loop Governance.** The humans in the loop are the client organisation's own security leadership — the CISO, Security Lead, or SOC Head — not external reviewers. These named, accountable individuals authorise the scope of what RedForce AI is permitted to assess, acknowledge findings, and approve remediation actions. This governance model ensures that risk decisions remain within the client organisation's chain of accountability, making the security programme defensible under SOC 2, ISO 27001, and PCI-DSS audit requirements. The platform replaces the labour of manual pentesting and auditing; it does not replace the governance chain that makes security decisions accountable.

**Explainable Findings.** Every finding produced by the platform includes the evidence that produced it, the reasoning that classified it, and the remediation context that makes it actionable. Security teams should not need to reverse-engineer how a finding was generated.

**Enterprise First.** RedForce AI is designed for the operational realities of enterprise environments — existing security toolchains, compliance requirements, change management processes, and the need for findings that can be communicated across technical and non-technical stakeholders.

**Modular Architecture.** Enterprise security requirements vary significantly across industries, regulatory environments, and technology stacks. RedForce AI is designed to be extended — new assessment capabilities, new standards mappings, and new integration points can be added without disrupting existing validation workflows.

---

## High-Level Capabilities

RedForce AI provides structured security validation across the full stack of an AI system. This reflects a foundational principle: AI security is full-stack security. The platform covers both the AI-specific attack surface and the application, API, and infrastructure layers that AI systems depend on — because a vulnerability at any layer can be exploited to compromise the AI system's integrity or the data it processes.

**Web Application Security Testing.** Systematic validation of web applications against established vulnerability classifications, with coverage aligned to OWASP Top 10:2025. Applications that embed or expose AI capabilities inherit the full web attack surface; this layer is a prerequisite for AI security, not a separate concern.

**API Security Testing.** Assessment of REST and GraphQL API implementations against OWASP API Security Top 10:2023, including authentication, authorisation, rate limiting, and data exposure risks. AI systems exposed through APIs require API-layer security as a foundational control.

**Infrastructure and Configuration Assessment.** Validation of server hardening, cloud configuration, and network controls for environments hosting AI systems. A model running on a misconfigured server or in an insecure cloud environment is not a secured AI system.

**LLM Security Validation.** Structured assessment of large language model integrations against OWASP LLM Top 10 v1.1, covering prompt injection, insecure output handling, training data exposure, model denial of service, and supply chain vulnerabilities.

**RAG Pipeline Security.** Evaluation of retrieval-augmented generation systems across the full pipeline — including embedding integrity, retrieval manipulation resistance, source-document injection, chunk-boundary attack surface, cross-tenant retrieval leakage, and indirect prompt injection via retrieved content. See [RAG_AGENT_MCP_SECURITY.md](RAG_AGENT_MCP_SECURITY.md) for detailed coverage.

**Autonomous Agent Security.** Assessment of agentic architectures against emerging threat models — including tool-invocation hijacking, semantic tool-call boundary violations, agent-loop attacks, cross-tool privilege escalation through reasoning chains, and agentic memory integrity. See [RAG_AGENT_MCP_SECURITY.md](RAG_AGENT_MCP_SECURITY.md) for detailed coverage.

**Model Context Protocol (MCP) Security.** Validation of MCP server implementations and client integrations — covering MCP server compromise, tool description poisoning, cross-server privilege escalation, and MCP client-side injection. See [RAG_AGENT_MCP_SECURITY.md](RAG_AGENT_MCP_SECURITY.md) for detailed coverage.

**AI Threat Modeling.** Structured threat modeling for AI systems using STRIDE, PASTA, and LINDDUN adapted for AI-specific trust boundaries — prompt boundary, embedding boundary, agentic action boundary, and retrieval boundary. See [AI_THREAT_MODELING.md](AI_THREAT_MODELING.md) for detailed coverage.

**Standards Coverage.** Platform findings are mapped to applicable current standards: OWASP Top 10:2025, API Security Top 10:2023, LLM Top 10 v1.1, Mobile Top 10:2024, MITRE ATLAS, and NIST AI RMF — providing a standards-aligned evidence base for compliance and governance.

**Risk Reporting and Executive Dashboards.** Findings are presented with severity classification, business impact context, and remediation guidance. Aggregated risk posture views, trend analysis, and standards coverage metrics give security leadership the visibility needed to communicate AI security posture to boards, regulators, and governance bodies.

---

## Intended Users

**Security Teams** requiring systematic, repeatable tooling for assessing AI system security across the full stack — from model interfaces to the application, API, and infrastructure layers they run on.

**DevSecOps Engineers** integrating security validation into CI/CD pipelines and requiring automated assessment that covers AI-specific risks alongside conventional vulnerability classes.

**AI Governance and Risk Teams** responsible for demonstrating that AI deployments meet security and compliance requirements, and for producing audit-ready evidence of ongoing security validation aligned to EU AI Act, NIST AI RMF, and ISO/IEC 42001.

**CISOs and Security Leadership** requiring accurate, interpretable risk reporting communicable to executive leadership, board-level governance, and external regulators — with a clear human accountability chain embedded in the validation workflow.

**Enterprise Architects and AI Platform Teams** building internal AI capabilities who require security validation integrated into the AI development and deployment lifecycle from the outset.

**AI Security Researchers** investigating RAG pipeline integrity, agentic threat models, MCP security, and AI-specific threat modeling methodology.

---

## Research Foundation

RedForce AI is informed by ongoing practitioner research in enterprise AI security and the organisational determinants that govern whether AI security programmes achieve meaningful outcomes at scale.

The platform's design reflects doctoral research into the organisational, governance, and human factors that determine successful enterprise adoption of AI-driven cybersecurity systems. The Human-in-the-Loop governance architecture — in which named client-organisation security leadership authorises scope, acknowledges findings, and approves remediation actions — is a practical instantiation of that research framework, grounded in the principle that technical controls alone do not constitute a security programme without an accountable human governance chain.

Security validation that cannot be integrated into enterprise processes and governance structures does not reduce risk — it produces reports.

---

## Current Status

RedForce AI is under active development. This documentation repository reflects the current state of the platform's vision, design principles, and capability set.

Capability documentation, integration guides, and deployment specifications are published as the platform matures. Organisations with specific evaluation or early-access interests are invited to make contact directly.

---

## Documentation Index

| Document | Description |
|---|---|
| [VISION.md](VISION.md) | Platform vision and founding rationale |
| [PROBLEM_STATEMENT.md](PROBLEM_STATEMENT.md) | Why existing security tools are insufficient for AI-native systems |
| [DESIGN_PRINCIPLES.md](DESIGN_PRINCIPLES.md) | Engineering and architectural principles |
| [USE_CASES.md](USE_CASES.md) | Enterprise scenarios and how the platform addresses them |
| [RAG_AGENT_MCP_SECURITY.md](RAG_AGENT_MCP_SECURITY.md) | RAG, Agent, and MCP security — threat landscape and assessment requirements |
| [AI_THREAT_MODELING.md](AI_THREAT_MODELING.md) | AI-adapted threat modeling methodology |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution guidelines for this documentation repository |

---

## Collaboration

The AI security field is developing rapidly, and some of its most consequential questions remain unsettled — technically, organisationally, and from a governance perspective.

We are interested in connecting with practitioners and researchers working on:

- AI and LLM security assessment methodologies
- RAG pipeline integrity and retrieval-layer threat modeling
- Agentic AI threat modeling and attack surface analysis
- MCP security and tool-protocol trust boundaries
- AI governance frameworks and enterprise AI risk management
- Secure AI SDLC design and DevSecOps integration for AI systems
- Organisational determinants of effective AI security programme adoption

If your work intersects with any of these areas, substantive exchange is welcome.

---

## Contact

**Harpreet Sohi** — Founder and Principal Architect, RedForce AI

- Website: [hsohi.in](https://hsohi.in)
- LinkedIn: [linkedin.com/in/hsohi13](https://linkedin.com/in/hsohi13)
- GitHub: [github.com/Preetsohi](https://github.com/Preetsohi)
- Email: hsohi@hsohi.in

---

*This repository contains public documentation only. RedForce AI source code is proprietary and is not available in this repository. Documentation is updated on a regular cycle as the platform develops.*
