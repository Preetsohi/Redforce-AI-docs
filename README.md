# RedForce AI

**Enterprise AI Cybersecurity Validation Platform**

RedForce AI is a proprietary security testing platform that validates the security posture of modern applications, AI-native systems, and the organizational capabilities required to operate them safely. It is built for enterprise environments where security validation must be continuous, auditable, privacy-preserving, and aligned with how human pentesters actually work.

This repository contains public documentation covering the platform's vision, design principles, architecture, capabilities, and roadmap. The source code is proprietary.

---

## What Makes RedForce Different

Most security tools take a single input and produce findings. Static analysis tools take code. Dynamic scanners take a URL. Software composition tools take a manifest. Each tool produces its own dashboard. A human is left to integrate the findings.

Skilled human pentesters do not work this way. A pentester reads the design documentation to understand business logic, reviews the code to identify weaknesses, probes the running application to confirm exploitability, and then chains findings across these contexts into an attack narrative that no individual tool could produce.

**RedForce automates this human approach.** Three input streams converge into one AI reasoning layer that plans attack chains the way an experienced pentester would:

- **Documentation analysis** — ingests SDDs, proposals, test cases, architecture documents to understand business logic and threat surface
- **Source code analysis** — SAST patterns, CVE detection, secrets scanning, IaC misconfiguration analysis
- **Dynamic application analysis** — authenticated crawling, OWASP testing, API security, exploit confirmation

The reasoning layer plans attack chains across all three. It identifies which CVEs are actually exploitable given the specific code. It connects findings into narratives the way a human pentester writes a report. No hit-and-trial. Plan, chain, link vulnerabilities, exploit, and report with proof of concept.

Beyond technical findings, RedForce assesses organizational capability — the governance maturity, capability development, and adaptation indicators that determine whether security findings translate into sustained security improvement.

---

## Vision

Traditional security testing was designed for a different era. Static analysis, network scanning, and conventional penetration testing address a well-understood attack surface — one defined by inputs, outputs, and deterministic logic.

AI-native systems do not behave deterministically. A large language model does not execute instructions, it interprets them. A retrieval-augmented generation pipeline does not query a database, it synthesises context from distributed knowledge sources. An autonomous agent does not follow a fixed program, it reasons, plans, and acts across tool boundaries in ways that vary with every invocation.

This architectural shift creates a class of vulnerabilities that conventional security tooling was not built to detect: vulnerabilities that emerge from model behaviour, context manipulation, tool misuse, and the interaction between AI components and enterprise data systems.

RedForce AI is designed to address this gap. Its purpose is to give enterprise security teams a systematic, repeatable, and governance-aligned approach to validating the security of both conventional and AI-native systems at every stage of the development and deployment lifecycle.

---

## The Problem

Enterprise organisations adopting AI face a security validation challenge that is qualitatively different from anything that preceded it.

**Expanded and dynamic attack surface.** Every AI integration point — an LLM API, a RAG pipeline, an agent framework, a model context protocol server — introduces a new category of attack surface that evolves as models update, retrieval corpora change, and agent capabilities expand.

**Prompt injection at scale.** Prompt injection has been demonstrated across production systems, including those deployed by major technology providers. As AI systems gain autonomy and access to enterprise tooling, the consequences of successful injection escalate accordingly.

**Agent abuse and insecure tool invocation.** Autonomous agents invoke external tools, APIs, databases, and code interpreters. Security boundaries that conventional access controls would enforce are often absent or insufficiently defined in agentic architectures.

**RAG poisoning and knowledge base integrity.** Retrieval-augmented systems depend on the integrity of their knowledge sources. Adversarial manipulation of retrieval corpora can cause systems to return attacker-controlled outputs without any visible indication of compromise.

**Continuous validation needs.** AI systems are not static. They are updated, fine-tuned, retrained, and extended continuously. A security assessment performed at deployment does not remain valid as the system evolves.

**Organisational adaptation gaps.** Existing tools produce findings. They rarely translate findings into organisational capability improvements — the governance, maturity, and continuous learning required to sustain AI security as an operational discipline.

---

## Design Principles

RedForce AI is designed around principles that reflect both the requirements of enterprise deployment and the realities of AI security practice.

**Zero Data Egress.** Security validation of enterprise systems must not require transmission of proprietary data, model outputs, or internal context to external services. RedForce AI operates entirely within the customer environment. No telemetry, no cloud dependency, no data leaves the boundary.

**Privacy by Design.** Data minimisation and access control are built into the platform architecture, not added as configuration. The platform does not require access to production data to perform security validation.

**Converged Analysis.** Documentation, code, and runtime application data are analysed together, not in isolation. The reasoning layer produces attack narratives that no single-input tool could generate.

**Human-in-the-Loop.** Automated validation surfaces findings and evidence. It does not replace security judgement. The platform supports human review, triage, and decision-making at every stage.

**Explainable Findings.** Every finding includes the evidence that produced it, the reasoning that classified it, and the remediation context that makes it actionable. Security teams should not need to reverse-engineer how a finding was generated.

**Security by Default.** The platform's own security posture is treated with the same rigour it applies to the systems it evaluates. Secure defaults, minimal permissions, and audit logging are standard, not optional.

**Modular Architecture.** Enterprise security requirements vary significantly across industries and regulatory environments. RedForce AI is designed to be extended. New assessment capabilities, new standards mappings, and new integration points can be added without disrupting existing validation workflows.

---

## Capabilities

RedForce AI provides structured validation across the primary domains where modern enterprises face security risk.

**Application Security Testing.** Systematic validation of web applications against OWASP Top 10:2025, with confirmed exploit modules for common vulnerability classes. Findings include reproducible proof of concept evidence and mapping to recognised vulnerability taxonomies.

**API Security Testing.** Assessment of REST and GraphQL APIs against OWASP API Security Top 10:2023, including authentication, authorisation, rate limiting, and data exposure risks.

**AI and LLM Security Validation.** Structured assessment of large language model integrations against OWASP LLM Top 10 v1.1, including prompt injection, insecure output handling, training data poisoning, model denial of service, and supply chain vulnerabilities.

**Retrieval-Augmented Generation Assessment.** Evaluation of RAG pipeline security, including retrieval integrity, context manipulation resistance, and knowledge base boundary enforcement.

**Agent and Agentic System Security.** Assessment of autonomous agent architectures against emerging threat models, including tool invocation security, inter-agent trust boundaries, and privilege escalation through agent reasoning chains.

**Model Context Protocol (MCP) Security.** Validation of MCP server implementations and client integrations against security requirements specific to the protocol architecture.

**Source Code and Dependency Analysis.** SAST analysis, CVE detection against current vulnerability databases, secrets detection, and infrastructure-as-code misconfiguration analysis.

**Threat Modelling from Documentation.** Optional ingestion of design documents, proposals, and test cases to generate AI-assisted threat models that engineers can refine and validate.

**Attack Chain Reasoning.** The reasoning layer connects findings across documentation, code, and dynamic analysis into attack narratives showing realistic exploitation paths, not isolated vulnerabilities.

**Standards Coverage.** Findings map to applicable standards including OWASP Top 10:2025, OWASP API Security Top 10:2023, OWASP LLM Top 10 v1.1, OWASP Mobile Top 10:2024, and MITRE ATLAS.

**Compliance Reporting.** Control mapping for PCI-DSS, HIPAA, GDPR, EU AI Act, NIST AI RMF, and ISO/IEC 42001 with gap analysis and remediation guidance.

**Organisational Risk Reporting.** Findings translate into executive-level risk views that support governance, board reporting, and strategic decision-making.

---

## Intended Users

RedForce AI is designed for the following roles within enterprise organisations:

**Security Teams and Penetration Testers** who require systematic, repeatable tooling for assessing both AI-native and conventional system security.

**DevSecOps Engineers** integrating security validation into CI/CD pipelines and requiring automated assessment gates that cover AI-specific risks alongside conventional vulnerability classes.

**AI Governance and Risk Teams** responsible for demonstrating that AI deployments meet security and compliance requirements, and for producing audit-ready evidence of ongoing validation.

**CISOs and Security Leadership** requiring accurate, interpretable risk reporting that can be communicated to executive leadership, board-level governance, and external regulators.

**Enterprise Architects and AI Platform Teams** building internal AI capabilities and integrating security validation into the development and deployment lifecycle.

---

## Research Foundation

RedForce AI is grounded in ongoing doctoral research on the organisational determinants of successful AI-driven cybersecurity adoption in enterprise environments. The research informs platform design beyond technical engineering, addressing the organisational realities that determine whether security tooling produces sustained capability improvement or merely produces reports.

See [RESEARCH.md](RESEARCH.md) for detail on the research foundation, current literature gaps, and how the platform addresses them.

---

## Current Status

RedForce AI is under active development toward Version 1.0 launch.

The platform foundation is operational with confirmed zero-false-positive results in critical and high severity tiers against industry-standard benchmark targets. The current development focus is the converged reasoning layer, organisational capability assessment, and compliance framework integration.

This documentation reflects the platform vision, design principles, and roadmap. Specific capability documentation, integration guides, and deployment specifications publish as the platform matures.

See [ROADMAP.md](ROADMAP.md) for the high-level capability roadmap.

Organisations with specific evaluation or early-access interests are invited to make contact directly.

---

## Collaboration

The AI security field is developing rapidly, and some of the most consequential questions it faces are not yet settled — technically, organisationally, or from a governance perspective.

We are interested in connecting with practitioners and researchers working on:

- AI and LLM security assessment methodologies
- Agentic AI threat modelling and attack surface analysis
- AI governance frameworks and enterprise risk management
- Secure AI SDLC design and DevSecOps integration for AI systems
- Organisational determinants of effective AI security programme adoption
- Converged analysis approaches that integrate static, dynamic, and contextual security testing

If your work intersects with any of these areas, we are open to substantive exchange.

---

## Contact

**Harpreet Sohi** — Founder and Principal Architect, RedForce AI

- Website: [hsohi.in](https://hsohi.in)
- LinkedIn: [linkedin.com/in/hsohi13](https://linkedin.com/in/hsohi13)
- GitHub: [github.com/Preetsohi](https://github.com/Preetsohi)
- Email: hsohi@hsohi.in

---

*This repository contains public documentation only. RedForce AI source code is proprietary and is not available in this repository. Documentation is updated as the platform develops.*
