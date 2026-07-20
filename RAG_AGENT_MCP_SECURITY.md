# RAG, Agent, and MCP Security

## Threat Landscape and Assessment Requirements for Advanced AI Architectures

---

## Introduction

The majority of current AI security tooling focuses on the model layer: prompt injection, jailbreaking, and output safety. These are real and important risks. They are not, however, the complete risk picture for enterprise AI deployments.

Production AI systems are not isolated models. They are architectures — composed of retrieval pipelines, autonomous reasoning engines, tool invocation frameworks, and protocol layers that connect AI capabilities to enterprise data and systems. Each of these components introduces attack surface that is distinct from the model itself, requires distinct assessment methodology, and is currently underserved by available security tooling.

This document covers three of the most consequential advanced attack surfaces in enterprise AI deployments: Retrieval-Augmented Generation (RAG) pipelines, autonomous agent architectures, and Model Context Protocol (MCP) implementations. For each, it describes the threat landscape, explains why conventional and model-focused security assessment is insufficient, and defines what structured security assessment of that surface requires.

These three surfaces represent the primary differentiation of RedForce AI's assessment capability relative to tools that assess only at the model layer.

---

## Part 1: RAG Pipeline Security

### What RAG Introduces

Retrieval-Augmented Generation systems extend a language model's knowledge by retrieving relevant content from an external corpus at inference time and incorporating that content into the model's context. This architecture is now standard for enterprise knowledge assistants, internal chatbots, document Q&A systems, and regulatory compliance tools.

The retrieval pipeline introduces a data layer between the external world and the model's reasoning process. That data layer is an attack surface.

### Threat Landscape

**Source-Document Injection.** An adversary with the ability to introduce content into the retrieval corpus — through a poisoned document, a manipulated knowledge base entry, or an upstream data feed — can place instructions inside retrieved content that the model will process as context. Unlike direct prompt injection, source-document injection does not require access to the model interface; it requires only access to the data sources the retrieval system trusts.

**Indirect Prompt Injection via Retrieved Content.** Retrieved documents containing adversarial instructions can redirect model behaviour without the user's knowledge. A retrieved support article, policy document, or external web page may contain embedded instructions that override or supplement the system prompt when incorporated into the model's context window. This attack is particularly difficult to detect because the injection vector is the retrieval system itself, not the user's input.

**Embedding Integrity Attacks.** RAG systems index content through embedding models that represent documents as vectors in a high-dimensional space. Adversarial manipulation of documents designed to exploit the geometry of the embedding space — causing malicious content to be retrieved in response to legitimate queries — constitutes an embedding integrity attack. The retrieved content appears semantically relevant; its malicious character is not apparent from the query or the retrieval score.

**Chunk-Boundary Exploitation.** RAG systems split documents into chunks for indexing. The chunking strategy determines what context surrounds any given piece of content at retrieval time. Adversarial content engineered to exploit chunk boundaries — placing injections at positions where they will be retrieved without the surrounding context that would reveal their nature — represents a class of attack specific to the chunking architecture of the target system.

**Cross-Tenant Retrieval Leakage.** In multi-tenant RAG deployments, where a single retrieval infrastructure serves multiple organisational units or client organisations, inadequate isolation of retrieval scope can result in content from one tenant's corpus being returned in response to queries from another. This is a data boundary violation that conventional access control models are not designed to prevent at the retrieval layer.

**Retrieval Manipulation.** Query manipulation techniques designed to alter what the retrieval system returns — exploiting ranking algorithms, metadata filters, or retrieval thresholds — can cause the model to operate on attacker-selected context rather than the context the system was designed to provide.

### Why Conventional Assessment Is Insufficient

Standard DAST tools send known-bad payloads to application endpoints and observe responses. RAG attack vectors do not operate through the application endpoint — they operate through the retrieval corpus, the embedding model, the chunking pipeline, and the context assembly process. None of these are observable by endpoint-focused scanning.

Penetration testing can identify RAG vulnerabilities, but only if the tester has sufficient expertise in retrieval architecture and dedicates time specifically to this surface. In practice, RAG pipeline security is rarely the focus of a time-bounded engagement.

### What Structured RAG Assessment Requires

Assessment of a RAG pipeline requires: evaluation of retrieval boundary enforcement under adversarial query conditions; injection of adversarially crafted documents into the corpus and observation of model behaviour; testing of embedding integrity against manipulation of document content and metadata; validation of chunk-boundary handling against known exploitation patterns; and in multi-tenant deployments, explicit testing of cross-tenant retrieval isolation.

Standards alignment: OWASP LLM Top 10 v1.1 — LLM02 (Insecure Output Handling), LLM06 (Sensitive Information Disclosure), LLM10 (Model Theft); MITRE ATLAS — AML.T0054 (LLM Prompt Injection), AML.T0051 (LLM Data Poisoning).

---

## Part 2: Autonomous Agent Security

### What Agents Introduce

Autonomous agents extend language model capability by enabling models to take actions: invoking tools, calling APIs, writing and executing code, interacting with external services, and operating across multi-step reasoning chains without continuous human direction. Enterprise agent deployments now include coding assistants with repository access, workflow automation agents with calendar and communication system integration, and research agents with broad web and database access.

The combination of reasoning capability and action capability creates a risk profile that has no direct precedent in conventional application security.

### Threat Landscape

**Tool-Invocation Hijacking.** An agent that can be manipulated into invoking a tool it was not instructed to invoke — or invoking a permitted tool with parameters it was not instructed to use — represents a significant enterprise risk. Tool-invocation hijacking can occur through prompt injection into the agent's context, through manipulation of the agent's reasoning about which tool is appropriate, or through adversarial content in tool outputs that redirects subsequent tool invocations.

**Semantic Tool-Call Boundary Violations.** Agents operate within an intended scope of tool use — a set of tools and parameter ranges that the deploying organisation considers authorised. Violations of this boundary may not be detectable by conventional access controls if the agent has legitimate permission to invoke the tool but is being directed to use it outside its intended scope. Assessing whether an agent's tool invocations remain within semantically authorised boundaries under adversarial conditions requires evaluation methodology that goes beyond permission-level access control testing.

**Agent-Loop Attacks.** Agents that operate in iterative reasoning loops — planning, acting, observing, replanning — can be manipulated through injections into their observation inputs. An adversarial tool result, retrieved document, or API response that the agent incorporates into its next reasoning step can redirect the agent's subsequent actions across an extended sequence. The attack payload may be small; its effect, amplified through the agent's reasoning loop, may be large.

**Cross-Tool Privilege Escalation through Reasoning Chains.** An agent with access to multiple tools — each individually operating within authorised scope — may be manipulated into combining tool invocations in sequences that produce outcomes beyond what any single tool invocation would permit. This form of privilege escalation operates at the level of the agent's reasoning rather than at the level of individual tool permissions, and is not detectable by tool-level access control review.

**Agentic Memory Integrity.** Agents with persistent memory — whether through explicit memory tools, context summarisation, or external storage — are vulnerable to manipulation of that memory. Adversarial content written to an agent's memory store can influence subsequent sessions, persist across context resets, and establish false beliefs or false history that redirect agent behaviour over time.

**Agent-to-Agent Trust Boundary Violations.** In multi-agent architectures, where a coordinating agent delegates tasks to specialised sub-agents, the trust relationships between agents are often implicit and unvalidated. A compromised or manipulated sub-agent can inject adversarial content into the outputs it returns to the coordinating agent, potentially redirecting the broader task execution. Second-order injection — where a low-privilege agent is used to manipulate a high-privilege agent — is a specific and consequential form of this attack.

### Why Conventional Assessment Is Insufficient

Agent behaviour is emergent. It cannot be derived from inspecting any single component — not the model, not the tool definitions, not the system prompt. It emerges from the interaction of all of these with the specific sequence of inputs the agent encounters. Conventional static and dynamic analysis tools are not designed to assess emergent multi-step behaviour under adversarial conditions.

Penetration testing of agent systems requires expertise in agentic architectures, the ability to construct and execute multi-step adversarial interaction sequences, and sufficient time to explore the non-deterministic space of agent behaviour. These conditions are rarely met in standard engagement scopes.

### What Structured Agent Assessment Requires

Assessment of autonomous agent security requires: construction and execution of adversarial interaction sequences targeting each tool in the agent's toolkit; testing of agent behaviour under manipulation of observation inputs across multi-step reasoning loops; evaluation of cross-tool invocation sequences for privilege escalation potential; testing of memory integrity under adversarial write conditions; and in multi-agent deployments, explicit testing of inter-agent trust boundaries.

Standards alignment: OWASP LLM Top 10 v1.1 — LLM01 (Prompt Injection), LLM08 (Excessive Agency), LLM09 (Overreliance); MITRE ATLAS — AML.T0054 (LLM Prompt Injection), AML.T0043 (Craft Adversarial Data).

---

## Part 3: Model Context Protocol (MCP) Security

### What MCP Introduces

The Model Context Protocol has emerged as a standard mechanism for connecting AI systems to external tools and data sources. MCP defines how AI clients communicate with MCP servers that expose tools, resources, and prompts. Enterprise deployments now use MCP to connect AI systems to code repositories, calendar and communication systems, databases, file systems, and internal APIs.

MCP dramatically expands the action surface of an AI system. It also introduces a set of trust relationships — between the AI client, the MCP server, and the tools the server exposes — that are not governed by conventional access control frameworks and are not assessed by conventional security tooling.

### Threat Landscape

**MCP Server Compromise.** An MCP server that has been compromised — through vulnerability exploitation, credential theft, or supply chain attack — can return adversarial tool results, modified resource content, or manipulated prompt definitions to the AI client. The AI client has no native mechanism to detect that the MCP server's outputs have been tampered with; it processes them as trusted context.

**Tool Description Poisoning.** MCP servers expose tools through natural-language descriptions that the AI client uses to determine which tool to invoke and how to invoke it. Adversarial modification of tool descriptions — replacing legitimate descriptions with instructions that redirect tool invocation behaviour — can cause the AI client to invoke tools outside their intended scope or to pass parameters that produce unintended outcomes. Tool description poisoning requires access to the MCP server configuration, not to the AI client or its system prompt.

**Cross-Server Privilege Escalation.** Enterprise AI deployments may connect to multiple MCP servers simultaneously, each with different permission scopes. An adversary who controls or has compromised a low-privilege MCP server can attempt to inject content into that server's outputs that redirects the AI client's interactions with higher-privilege MCP servers. This form of cross-server escalation exploits the AI client's unified context rather than individual server permissions.

**MCP Client-Side Injection.** The MCP client processes tool results, resource content, and server-provided prompts as part of its context. Adversarial content in any of these inputs — crafted to appear as instructions rather than data — constitutes client-side injection through the MCP layer. Unlike direct prompt injection, which targets the user input boundary, MCP client-side injection targets the tool-result and resource-content boundaries.

**Trust Boundary Violations between MCP Client and Server.** The MCP architecture assumes a trust relationship between client and server that may not be warranted in all deployment configurations. Misconfigured authentication, absent integrity verification of server responses, and implicit trust in server-provided prompt definitions each represent trust boundary violations that expand the exploitable attack surface.

### Why Conventional Assessment Is Insufficient

MCP is a recent protocol. Security assessment tooling for MCP-connected systems does not yet exist in mature form in the broader market. Conventional web application scanners assess HTTP endpoints; they are not designed to assess the trust relationships and data flows specific to the MCP architecture. Penetration testers without specific MCP expertise are unlikely to assess this surface systematically.

### What Structured MCP Assessment Requires

Assessment of MCP security requires: testing of MCP server authentication and integrity controls; evaluation of tool description content for injection vectors; testing of cross-server context isolation under adversarial conditions; assessment of client-side handling of adversarial tool results and resource content; and validation of trust boundary enforcement between MCP client and each connected server.

Standards alignment: OWASP LLM Top 10 v1.1 — LLM01 (Prompt Injection), LLM02 (Insecure Output Handling), LLM08 (Excessive Agency); MITRE ATLAS — AML.T0054 (LLM Prompt Injection), AML.T0056 (LLM Jailbreak).

---

## Implications for Enterprise Deployments

Three operational conclusions follow from the threat landscape described above.

**Point-in-time assessment is structurally insufficient.** RAG corpora are updated continuously. Agent tool configurations evolve as integrations expand. MCP server implementations change as the protocol matures and enterprise deployments grow. A security assessment conducted against a specific state of these systems does not remain valid as that state changes. Continuous validation — automated, scheduled, and triggered by configuration change — is the only model that matches the actual change velocity of these surfaces.

**Model-layer testing does not substitute for pipeline and protocol testing.** An organisation that has assessed its LLM integration against OWASP LLM Top 10 v1.1 has not thereby assessed the security of its RAG pipeline, its agent architecture, or its MCP implementation. These are distinct surfaces requiring distinct methodology. Conflating model-layer assurance with full-stack AI security assurance produces a false confidence effect.

**Governance must account for expanded action surfaces.** When an AI system can invoke tools, write to external systems, and communicate with other agents, the consequences of a security failure are no longer limited to information disclosure. They extend to unauthorised action. Governance frameworks that treat AI security as a data protection problem are not calibrated to the action risk introduced by agentic and MCP-connected architectures. Security governance for these systems must address scope authorisation, action logging, and human accountability for remediation decisions.

---

## Standards and Framework Reference

| Framework | Relevant Coverage |
|---|---|
| OWASP LLM Top 10 v1.1 | LLM01 Prompt Injection, LLM02 Insecure Output Handling, LLM06 Sensitive Information Disclosure, LLM08 Excessive Agency, LLM09 Overreliance, LLM10 Model Theft |
| MITRE ATLAS | AML.T0054 LLM Prompt Injection, AML.T0051 LLM Data Poisoning, AML.T0043 Craft Adversarial Data, AML.T0056 LLM Jailbreak |
| NIST AI RMF | Govern 1.0, Map 2.0, Measure 2.5, Manage 2.2 |
| ISO/IEC 42001 | Clause 6.1 Risk Assessment, Clause 8.4 AI System Operation |
| EU AI Act | Article 9 Risk Management, Article 15 Accuracy and Robustness |

---

*This document is maintained as part of the RedForce AI public documentation repository. It reflects current understanding of the RAG, agent, and MCP threat landscape and will be updated as the field evolves.*
