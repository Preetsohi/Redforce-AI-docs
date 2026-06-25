# Architecture

RedForce AI is architected around three principles that drive every design decision: zero data egress from the customer environment, converged analysis across multiple input streams, and modular execution so customers can use individual capabilities or the full converged assessment as appropriate.

This document describes the architecture at a level appropriate for technical evaluation. Deployment specifications, configuration details, and operational guides will publish as the platform matures.

---

## High-Level Architecture

The platform separates three concerns:

**Input acquisition** gathers the three data streams the platform reasons over: documentation provided by the customer, source code from the customer repository, and dynamic data from probing the running application or API.

**Analysis engines** process each input stream through purpose-built engines. Each engine produces findings in a uniform schema that feeds the reasoning layer.

**Reasoning and reporting** synthesises findings across engines, plans attack chains, generates compliance mappings, and produces reports appropriate for technical, governance, and executive audiences.

All processing occurs within the customer's deployment environment. The platform makes no outbound calls to external services during scanning. The only optional external interaction is offline-first threat intelligence synchronisation, which runs separately from scanning and can be disabled entirely for fully air-gapped operation.

---

## Analysis Engines

The platform uses purpose-built engines for distinct analysis domains. Each engine runs independently and produces findings in a uniform schema.

**Dynamic Application Security Testing Engine.** Probes running web applications and APIs for vulnerabilities. Performs authenticated crawling, OWASP Top 10:2025 testing, and exploit confirmation. Built around industry-standard tools enhanced with custom exploit modules and a confidence-scored validation layer that eliminates false positives in critical and high severity tiers.

**Static Analysis Engine.** Analyses source code for vulnerability patterns, identifies known CVEs in dependencies, detects secrets in committed code, and finds infrastructure-as-code misconfigurations. Built on permissively licensed open-source tools (Semgrep and Trivy) wrapped in the platform's analysis and validation layer.

**URL and Endpoint Analysis Engine.** Performs targeted analysis of specific URLs and API endpoints, including ZAP-based assessment, vulnerability template matching, and JavaScript secrets detection.

**AI and LLM Security Engine.** Validates AI integrations against OWASP LLM Top 10 v1.1 and MITRE ATLAS techniques. Covers prompt injection, insecure output handling, model denial of service, training data poisoning, supply chain risks, and agent-specific attack surfaces.

Each engine maintains its own scope, toolset, and reporting output. Engines do not share runtime state. They feed into a unified findings layer where the reasoning engine integrates results across all sources.

---

## The Reasoning Layer

The reasoning layer is the differentiating component of the platform. It is not another engine — it is a synthesis layer that operates above the engines.

Conventional security platforms produce findings as independent artifacts. A SAST tool reports SQL injection in a code path. A DAST tool reports SQL injection on an endpoint. A compliance tool maps the finding to PCI-DSS 6.5.1. These are three separate outputs that a human integrates.

RedForce AI's reasoning layer integrates them as a single output. It recognises that the SAST and DAST findings refer to the same underlying vulnerability. It identifies what business logic the documentation indicates the vulnerable endpoint protects. It determines whether the vulnerability is exploitable in production based on authentication context. It then plans an attack chain: how an adversary would discover the vulnerability, exploit it, and pivot to higher-value access.

The output is an attack narrative rather than a finding list. The narrative shows how findings connect, what realistic exploitation paths exist, and where remediation should focus to break the most attack chains with the least engineering effort.

This reasoning is performed entirely on-premises using a locally hosted language model, with no data transmitted to external services.

---

## Air-Gap Architecture

The platform is designed to operate in environments where no outbound network traffic is permitted during scanning. This is a critical requirement for regulated industries, government deployments, and any environment where customer data sensitivity precludes cloud LLM dependencies.

The scan execution path operates under network isolation enforced by host firewall rules, not application-layer convention. Scan engines can reach the customer's target application and the locally hosted language model. They cannot reach the internet during scan execution. This is verifiable from outside the application by inspecting the firewall configuration.

Threat intelligence data — CVE descriptions, EPSS exploitability scores, CISA Known Exploited Vulnerabilities — is held in a local database. The database is updated through a sandboxed synchronisation component that runs separately from scanning, on an explicit schedule, with its own network egress configuration. The sync component is the only platform component permitted internet access, and it does not run during scans.

This architecture provides three guarantees: scan-time air-gap is enforced by the operating system and is verifiable by a security auditor; threat intelligence remains current through controlled, auditable update windows; and the customer can disable the sync component entirely for fully air-gapped operation, accepting only the freshness trade-off.

---

## Local Language Model

The platform uses a locally hosted language model for reasoning and analysis tasks. The model runs on the customer's infrastructure with no cloud LLM dependency.

The current implementation uses an industry-standard open-weights model bound to local interfaces only — not exposed on customer LAN networks and not reachable from the internet. The model serves the reasoning layer, the threat-modelling-from-documentation capability, and the analysis enhancement features.

Subsequent platform releases will offer a security-fine-tuned model trained specifically on penetration testing, attack chain reasoning, and security narrative writing. The fine-tuned model delivers measurably better reasoning quality for security-specific tasks while preserving the local-only operational model.

For customers who explicitly require maximum reasoning quality and accept the operational implications, a configuration option supports routing specific reasoning steps to a customer-controlled external language model. This option is disabled by default. The customer brings their own API key and bears their own token costs. The platform remains zero-egress in its default configuration.

---

## Modular Execution

Customers do not always need a full converged assessment. The platform supports modular execution where each capability runs independently:

- Threat modelling alone, from documentation input
- Static code analysis alone, against a repository
- Dynamic application testing alone, against a running target
- API security testing alone, against an API specification
- AI security testing alone, against an LLM endpoint
- Compliance gap analysis alone, against an existing findings dataset
- Full converged assessment combining all of the above

Modular execution matters for two reasons. First, customer maturity varies — an organisation new to RedForce may start with compliance gap analysis to satisfy an auditor, then expand to full assessment once the platform proves value. Second, integration patterns vary — a CI/CD pipeline may invoke only the SAST engine on every commit while running full assessments weekly.

The platform exposes both a web interface for interactive use and a command-line interface for automation. The command-line interface supports the same modular execution model and produces structured output suitable for pipeline integration.

---

## Reporting Architecture

The platform produces reports calibrated to different audiences from a single underlying scan.

**Technical reports** provide the detailed findings, proof of concept evidence, remediation guidance, and tool output that engineering teams need to fix issues.

**Compliance reports** map findings to specific controls in PCI-DSS, HIPAA, GDPR, EU AI Act, NIST AI RMF, ISO/IEC 42001, and OWASP frameworks. Each control includes the underlying findings that drove the control assessment and the remediation steps required to close gaps.

**Executive reports** translate technical findings into organisational risk language appropriate for CISO and board communication. These reports focus on capability maturity, governance posture, and strategic risk rather than individual technical issues.

**Audit reports** provide tamper-evident evidence of when scans ran, what they covered, who authorised them, and what findings they produced. This supports compliance attestation and regulatory review.

The reports share a common underlying findings dataset but present it in formats appropriate to their audiences. Engineering teams do not see governance maturity scores in their reports. Executives do not see proof-of-concept payloads.

---

## Deployment Models

The platform supports two deployment models in v1.0:

**Self-hosted deployment** runs the platform entirely on customer infrastructure. This is the required model for customers with data sovereignty constraints, regulated industries, and fully air-gapped environments. All scanning, all reasoning, and all data storage occur within the customer boundary.

**Managed deployment** runs the platform on RedForce-hosted infrastructure with customer-isolated tenancy. This model suits customers without infrastructure or operations capacity to host the platform themselves. Even in the managed model, scanning of customer targets uses customer-controlled scanning agents to preserve the air-gap properties where customer requirements demand it.

The platform supports migration between models. A customer that starts on managed deployment and later requires self-hosting for compliance reasons can transition without losing historical scan data or assessment continuity.

---

## Standards Coverage

Platform findings map to applicable standards including:

- OWASP Top 10:2025 (Web)
- OWASP API Security Top 10:2023
- OWASP LLM Top 10 v1.1
- OWASP Mobile Top 10:2024
- MITRE ATT&CK Enterprise techniques
- MITRE ATLAS techniques for AI systems
- NIST AI Risk Management Framework
- ISO/IEC 42001 AI Management System controls

Standards mappings are maintained in a versioned registry, not hardcoded in engine logic. When standards bodies publish new versions, the registry updates without requiring engine code changes. This is a deliberate architectural choice that recognises standards evolve continuously and platform support must keep pace without forcing version coupling.

---

## What Is Not in This Document

This architecture overview deliberately omits implementation specifics that publish as the platform matures: API specifications, configuration formats, deployment topologies for specific cloud environments, and integration patterns with specific enterprise tools.

The document also omits internal architecture decisions that are subject to refinement during ongoing development. Documentation will update as the platform stabilises.

For specific evaluation or early-access discussions, contact the founder directly through the channels listed in the repository README.
