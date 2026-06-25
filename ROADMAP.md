# Roadmap

This roadmap describes the high-level capability development direction for RedForce AI. It is published to give stakeholders, prospective customers, and the research community visibility into where the platform is heading.

Specific release dates are deliberately not published. Capability availability depends on engineering completion, validation against benchmark targets, and readiness for the customer use cases the capability supports. We prefer to under-promise and ship working capability rather than commit to dates and ship incomplete work.

---

## Current State

The platform foundation is operational. The following capabilities are working today with zero false positives in critical and high severity tiers against industry benchmark targets:

- Dynamic application security testing against web applications
- API security testing against REST APIs
- Source code analysis with SAST patterns, CVE detection, secrets scanning, and IaC misconfiguration analysis
- Authenticated scanning with credential pre-validation
- OWASP Top 10:2025 coverage for web applications
- Compliance-ready findings schema with confidence scoring and evidence preservation
- Containerised, air-gapped deployment with verifiable network isolation
- Local language model integration for analysis enhancement

The platform has been validated against industry-standard deliberately vulnerable applications including DVWA and OWASP Juice Shop, confirming real exploit identification with proof-of-concept evidence and zero false positives in critical and high severity tiers.

---

## Near-Term Capability Development

The current development focus is on capabilities that complete the Version 1.0 platform launch.

**AI and LLM Security Validation.** Comprehensive OWASP LLM Top 10 v1.1 coverage and MITRE ATLAS technique testing for production LLM integrations. Covers prompt injection, insecure output handling, training data poisoning, model denial of service, supply chain risks, and agent-specific attack surfaces.

**Threat Intelligence Enrichment.** Offline-first integration with public CVE databases, EPSS exploitability scoring, and CISA Known Exploited Vulnerabilities. Findings include current threat context without compromising the platform's air-gap architecture.

**Live Scan Visibility.** Real-time scan progress streaming to the user interface with phase-level events from each engine. Users see what the platform is doing throughout scan execution rather than waiting for completion.

**Documentation-Based Threat Modelling.** Optional ingestion of design documents, proposals, test cases, and architecture documentation to generate AI-assisted threat models. The capability is a multiplier when documentation exists and degrades gracefully when documentation is sparse.

**Attack Chain Reasoning.** The reasoning layer that synthesises findings across engines into attack narratives. This is the differentiating capability that distinguishes RedForce from conventional single-input scanners.

**Modular Task Execution.** Support for running individual capabilities or combinations rather than full converged assessments. Enables CI/CD integration patterns where specific capabilities run on specific triggers.

**Compliance Framework Coverage.** Control mapping for PCI-DSS, HIPAA, and GDPR in initial release. Mapping for EU AI Act, NIST AI RMF, and ISO/IEC 42001 follows in subsequent releases as those frameworks stabilise.

**Organisational Capability Assessment.** Assessment of organisational readiness, governance maturity, and capability development indicators. Translates technical findings into strategic risk and capability gap views appropriate for executive and governance audiences.

**Executive and Audit Reporting.** Reports calibrated to CISO, governance, and audit audiences. Tamper-evident scan records suitable for compliance attestation.

**Self-Service Onboarding.** Customer signup, deployment, and first-scan workflow that does not require direct support intervention. Enables the volume tier of the platform.

---

## Subsequent Capability Development

Following the Version 1.0 launch, the development direction extends in several parallel tracks.

**Security-Fine-Tuned Local Model.** A purpose-trained language model optimised for security reasoning, attack chain narrative generation, and threat modelling. Distributed to customers and runs locally. Delivers measurably better reasoning quality than general-purpose open-weights models for security-specific tasks while preserving the zero-egress operational model.

**Production Hosting Security.** Cloud configuration assessment for production hosting environments at the depth mid-scale customers need. Not intended to compete with full cloud security posture management products, but to cover the configuration security gaps that affect customer-deployed applications.

**Mobile Application Security.** OWASP Mobile Top 10:2024 coverage for Android and iOS applications. Static and dynamic analysis appropriate for mobile-specific attack surfaces.

**Retest and Verification Workflow.** Targeted re-execution of specific findings after remediation, with verification reporting that confirms whether fixes held. Integrates the platform into the remediation cycle, not just the discovery cycle.

**Continuous Assessment.** Scheduled scan execution with differential reporting that highlights new findings, resolved findings, and changes in organisational capability scores over time. Supports the continuous-validation operational model that AI security requires.

**Custom Compliance Templates.** Customer-defined compliance frameworks for organisations with proprietary control sets or industry-specific requirements not covered by standard frameworks.

**Multi-Tenancy and Organisational Isolation.** Full row-level security for organisations that need to run RedForce across multiple business units or subsidiaries with strict isolation requirements.

**Extended Standards Coverage.** Additional framework mappings as they become relevant. Standards evolve continuously; the platform's registry-driven mapping architecture supports new standards without core engine changes.

---

## Longer-Term Direction

Several capabilities are part of the long-term direction but are not committed to specific releases. They publish here for transparency about platform direction.

**Adaptive Learning Loop.** A proprietary learning mechanism that improves payload effectiveness, false positive detection, and confidence calibration based on observed scan outcomes. The loop requires scan volume to learn from, so it develops after launch as the platform accumulates operational data. The mechanism preserves customer data privacy through architectural separation between learning and individual customer scan data.

**Attack Path Knowledge Graph.** Graph-based representation of finding relationships, attack techniques, and organisational asset dependencies. Supports advanced attack chain reasoning and blast radius analysis.

**Detection Rule Generation.** Generation of YARA, Sigma, and Snort detection rules from confirmed findings. Provides defensive output that security operations teams can deploy immediately, not just findings that engineering teams must address.

**SIEM Integration.** Forwarding of confirmed findings as security events to enterprise SIEM platforms. Integrates RedForce findings into existing security operations workflows.

**Federated Threat Intelligence.** Privacy-preserving sharing of attack patterns and finding contexts across customer organisations, supporting collective defence without exposing individual customer data.

---

## What This Roadmap Does Not Promise

The roadmap is a development direction, not a commitment to specific delivery dates. The platform is built by a small, focused team that prioritises shipping working capability over hitting calendar commitments.

Capabilities listed may ship in different orders than presented based on customer need, technical dependency, or empirical learning from earlier capabilities. Capabilities may be modified, deferred, or replaced if development reveals better approaches to the underlying problems.

The roadmap publishes to give honest visibility into direction, not to create commitments that constrain future engineering decisions. Customer evaluation discussions should engage with the founder directly for current capability availability and realistic delivery expectations.

---

## How Roadmap Decisions Are Made

Three inputs drive roadmap prioritisation:

**Customer evidence.** Capabilities that solve real customer problems get prioritised over capabilities that are technically interesting but lack demand signal.

**Research alignment.** Capabilities that advance the doctoral research foundation on organisational determinants of AI cybersecurity adoption receive weight even when their immediate commercial value is unclear, because they strengthen the platform's long-term differentiation.

**Strategic positioning.** Capabilities that strengthen the platform's defensible position against well-funded competitors are prioritised, particularly those that competitors cannot easily replicate.

The roadmap is reviewed quarterly. Customer feedback, market evolution, and engineering reality all inform updates. The published version reflects current direction as of the most recent review.

---

## Engaging With the Roadmap

Organisations with evaluation interests, integration requirements, or capability requests are invited to engage directly through the channels listed in the repository README.

Researchers and practitioners with interest in the converged analysis model, organisational adaptation indicators, or the platform's underlying research foundation are similarly welcome to engage. The platform's development is informed by ongoing exchange with the broader AI security community.

The fastest way to influence roadmap priorities is to articulate a specific use case that current capabilities do not address and a specific outcome the capability would enable.
