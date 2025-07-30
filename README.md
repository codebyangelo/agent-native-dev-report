# agent-native-dev-report-2025



## Report on "Ship Production Software in Minutes, Not Months" by Eno Reyes (Factory)

This report synthesizes insights from Eno Reyes's presentation "Ship Production Software in Minutes,
Not Months," outlining the shift towards agent-native development. It then provides a technical
summary of the proposed architecture and methodology, followed by a detailed threat analysis
identifying potential blind spots, and concludes with reflections on compliance with key standards
such as NIST, ISO, and IEC 27001.

## 1. Technical Summary: Agent-Native Development and Architecture

Eno Reyes articulates a fundamental transition in software development from human-driven to
agent-driven paradigms. He criticizes the current approach of "sprinkling AI on old software," which
lacks clarity and true transformative power. Instead, he advocates for "agent-native development,"
where the majority of software lifecycle tasks are delegated to AI agents.

### Core Principles and Vision:

* Delegation: AI agents, referred to as "droids," take over groundwork, research, code generation,
testing, and even incident response.
* Context as King: The efficacy of AI tools hinges entirely on the quality and completeness of the
context they receive. Failures often stem from a lack of context. The solution lies in providing
comprehensive, centralized context from all engineering tools and data sources, including historical
data, team discussions, and runbooks.
* AI as Co-worker/Platform: AI should be viewed as more than just a tool; it's a collaborative entity,
akin to a "co-worker and a platform," capable of continuous learning and improvement.
* English as Programming Language: Reflecting Andrej Karpathy's quote, "The hottest new
programming language is English," the vision is for natural language to become the primary interface
for instructing agents.
* Human Augmentation, Not Replacement: Agents are presented as "climbing gear" that amplify
engineers' capabilities, allowing them to move from the "inner loop" (direct coding) to the "outer loop"
(orchestration, high-level pattern building, strategic thinking).
* Evolution of Developer Skillset: Future developers will excel not primarily through technical
  knowledge or optimization skills, but through clear thinking and effective communication with both humans and AI.

### Architectural Understanding (Implied by Droid Operations):

The Factory platform, leveraging "droids," appears to operate on a distributed agentic architecture with
several key components and capabilities:

* Centralized Context Repository: A crucial component that aggregates data from disparate sources
(e.g., codebases, git branches, machine configurations, system logs, past incidents, runbooks from
Notion/Confluence, team discussions from Slack, user memories, organizational memories). This
repository is the bedrock for providing rich context to agents.
* Specialized AI Agents ("Droids"): These agents are designed for specific tasks and exhibit
"separation of concerns." Examples include:

* Planning Agent: Gathers trends, customer feedback, organizational data, identifies patterns and
technical constraints, and helps iterate on PRDs.
* Project Management Agent: Takes PRDs, uses tools like Linear/Jira to create tickets, epics, and
roadmaps.
* Code Generation/Development Agent: Writes code, runs pre-commit hooks, lints, and generates
CI-passing Pull Requests.
* Incident Response Agent: Ingests Sentry incidents, pulls context from diverse sources (logs, past
incidents, runbooks, Slack), performs RCA, generates mitigation plans, and even updates runbooks and
response workflows.

* Tool Integration Layer: Agents have the "ability to access" and "leverage tools" (e.g., git, code editors,
Sentry, Linear, Jira, Notion, Confluence, Slack). This implies robust APIs and integration mechanisms to
interact with existing enterprise tooling.
* Parallel Processing Infrastructure: The platform is designed to support "thousands of agents
working in parallel," indicating a highly scalable and distributed backend.
* Interactive / Collaborative Interface: An "intuitive interface for managing and delegating these
tasks" suggests a user-facing platform where humans can interact with droids, clarify plans, provide
feedback, and oversee their operations.
* Learning and Memory Mechanism: The concept of "user and organization level memory" and agents
learning from "response patterns and common issues" suggests sophisticated memory management
and continuous learning capabilities that adapt over time.
* Automated Knowledge Management: Documentations and processes, once seen as problems, now
"train the AI agent and give it context," becoming a "conversation with future developers and AI
systems." RCAs lead to automated runbook generation and workflow updates, reducing manual
curation.

Reyes highlights that their process currently does not require "MCPs or API keys to pass around,"
suggesting an alternative, potentially more integrated or secure, internal mechanism for context
transfer. The focus is on orchestrating systems rather than direct execution.

## 2. Threat Modeling and Blindspot Assessment

The transition to agent-native development, while promising efficiency, introduces new attack surfaces
and necessitates a re-evaluation of traditional security models.

### Potential Threats and Attack Vectors:

Context Poisoning/Manipulation: Since "AI tools are only as good as the context they receive,"
malicious actors could:

* Inject biased or false information into the centralized context repository (e.g., fake logs, misleading
team discussions, manipulated historical incidents, corrupted runbooks). This could lead to agents
generating flawed code, incorrect RCAs, or unsafe mitigation plans.
* Alter training data: If documentation and historical processes directly train agents, tampering with
these could embed vulnerabilities or undesirable behaviors into the agents themselves.

### Agent Impersonation/Hijacking:

* Spoofing Agents: Malicious agents could impersonate legitimate droids to gain unauthorized access
to tools and data.
* Hijacking Agents: Gaining control of a legitimate droid could allow an attacker to execute arbitrary
code, modify critical systems (e.g., codebases, PRs, CI/CD pipelines), or exfiltrate sensitive data at
machine speed.
* Privilege Escalation: An agent with broad access across multiple tools could be a single point of
failure if compromised, leading to significant privilege escalation.
Automated Vulnerability Injection:
* Backdoors in AI-Generated Code: A compromised or misconfigured code-generating droid could
intentionally or unintentionally introduce vulnerabilities (e.g., insecure API key handling, buffer
 overflows, injection flaws) into the codebase, which might then pass automated CI/CD checks.
* Logic Bombs/Time-delayed Malice: Code generated by a compromised agent could contain
dormant malicious logic that activates under specific conditions.
* Supply Chain Attacks via Agentic Pipelines: If agents are integrated into the entire SDLC, a
compromise at any stage (e.g., an agent generating a PR, an agent deploying) could propagate
vulnerabilities across the software supply chain.
* Data Exfiltration: Droids, by design, access vast amounts of sensitive organizational data (logs, code,
customer feedback, internal discussions). A compromised droid could act as an automated exfiltration
tool.

### Denial of Service (DoS)/Resource Exhaustion:

* Agent Loop/Erroneous Tasking: A rogue or misconfigured agent could enter an infinite loop,
continuously generating useless artifacts, consuming resources, or flooding systems (e.g., creating
thousands of Jira tickets, generating excessive PRs).
* Parallel Execution Abuse: An attacker leveraging the "thousands of agents working in parallel"
infrastructure could orchestrate a large-scale attack against internal or external systems.

### Lack of Auditability/Explainability: If droid decisions are opaque, it becomes difficult to:

* Trace the root cause of an error or security incident.
* Prove compliance.
* Detect malicious activity that mimics legitimate agent behavior.

Over-reliance and Automation Bias: Over-trusting agents without sufficient human oversight or
validation can lead to humans ignoring critical warnings or failing to detect subtle errors introduced by
the AI. This is a significant blind spot if the human role shifts entirely to "orchestration" without robust
verification points.

### Intellectual Property (IP) Risks:

* Data Leakage in Context: Proprietary algorithms, trade secrets, or sensitive business logic present
in documentation or internal discussions could be inadvertently exposed if the agents process and
share this context in insecure ways.
* IP Ownership of AI-Generated Code: While not a security threat, the legal ownership of code
entirely generated or heavily modified by AI agents requires clear policies.

### Specific Blind Spots:

* "No MCPs or API Keys Passed Around": While potentially simplifying some security aspects (e.g., API
key sprawl), this statement raises a critical blind spot: How is authentication, authorization, and secure
communication handled internally? If it's through a less visible or proprietary mechanism, it could be
harder to inspect, audit, and secure than standard, well-vetted protocols.
* Implicit Trust: The shift towards agents "questioning" human input implies an advanced capability,
but also requires a deep understanding of when an agent's skepticism is valid vs. when it's a
hallucination or misinterpretation. Over-trust in the agent's "clarity-seeking" could lead to missed
human errors.
* Shadow AI Agents: As adoption grows, the potential for unmanaged or unmonitored "shadow
agents" operating outside the central platform's governance becomes a risk, similar to shadow IT.
* Human Oversight Training Gap: If developers shift to "orchestration," their training must evolve to
cover monitoring, validating AI outputs, understanding AI limitations, and responding to agent failures.
A gap here is a significant blind spot.
* "Memory" Security: The "user and organization level memory" sounds powerful but represents a
massive centralized repository of potentially sensitive historical data. How this memory is secured,
segmented, and purged (if necessary) is a critical blind spot if not explicitly addressed.
* Validation of Automated Runbook/Workflow Updates: Automated updates to incident response
runbooks or operational workflows must have robust validation and approval mechanisms to prevent a
flawed RCA from leading to widespread, incorrect operational changes.

## 3. Compliance Reflections (NIST, ISO, IEC 27001)

The agent-native development paradigm has significant implications for compliance with established
information security and AI governance frameworks.

### NIST AI Risk Management Framework (AI RMF):

The NIST AI RMF is designed to help organizations manage risks across the AI lifecycle. Factory's
approach aligns with several aspects but also surfaces areas requiring close attention:

* Govern: The framework emphasizes mapping, measuring, and managing AI risks. The shift to
agent-driven development requires a robust governance model for AI agents, including defining roles,
responsibilities, and accountability for agent actions (e.g., who is responsible if an agent injects a bug?).
The internal "memory" and automated knowledge sharing would need to be governed to ensure
fairness, transparency, and data quality.
* Map: Identifying AI capabilities, contexts, and risks. The detailed breakdown of droid functions (code
generation, RCA, PRD creation) facilitates mapping, but the vast data access and autonomous nature of
droids escalate the potential impact of identified risks.
* Measure: Assessing identified risks. Organizations would need metrics to evaluate agent
performance, error rates, bias (if applicable), and security posture. This includes auditing agent
decision-making processes.
* Manage: Prioritizing and mitigating risks. The promise of "reducing repeat incidents" aligns with risk
mitigation. However, the automated propagation of changes (e.g., updated runbooks) requires
controlled and validated management processes to ensure safety and security. The "security purposes
and ownership consideration" mentioned at the end of the talk is a direct call for RMF-aligned risk
management.
* Trustworthiness: NIST AI RMF also emphasizes trustworthiness characteristics such as safety,
security, resilience, transparency, interpretability, accountability, and privacy. The "black box" nature of
some LLM decisions could challenge interpretability and accountability. The extensive data access by
agents raises significant privacy concerns.

### ISO/IEC 27001 (Information Security Management System - ISMS):

ISO 27001 requires organizations to establish, implement, maintain, and continually improve an ISMS.
Agent-native development impacts nearly every control in Annex A:

* A.5 Information Security Policies: New policies explicitly governing the use, deployment, and
oversight of AI agents are required, including acceptable use, data handling by agents, and incident
response procedures for agent-related incidents.
* A.6 Organization of Information Security: Clear roles and responsibilities for managing AI agents,
including human oversight, audit trails for agent actions, and a defined process for agent-related
security incidents, are crucial.
* A.8 Asset Management: AI agents themselves, the data they process (especially the "context" and
"memory"), and the outputs they generate (code, PRDs, RCAs) are all critical information assets that
require classification, ownership, and protection.
* A.9 Access Control: Granular access controls for agents to systems, tools, and data are paramount.
The "no MCPs or API keys" statement will require rigorous validation of their alternative access
mechanisms. The principle of least privilege must be strictly applied to agents.
* A.12 Operations Security: Agents will be heavily involved in operational processes. Secure
development of AI systems, change management for agent configurations, logging and monitoring of
agent activities, and backup/recovery for agent-related data are critical. The automated nature of
agent-driven incident response requires extreme diligence in this area.
* A.14 System Acquisition, Development and Maintenance: Secure by design principles must extend
to the development of agents and the agent platform. AI-generated code must undergo security testing
and validation commensurate with traditional code.
* A.15 Supplier Relationships: If Factory is a third-party platform, ISO 27001 requires robust supplier
security assessments and contracts to ensure their platform meets security expectations.
* A.16 Information Security Incident Management: The claim of significantly reduced response times
is positive, but the incident management process must fully integrate agent-driven capabilities while
ensuring human oversight, forensic capabilities, and robust reporting.
* A.18 Compliance: This includes legal, statutory, regulatory, and contractual requirements, as well as
intellectual property rights. The automated data access by agents (e.g., team discussions, customer
feedback) will raise significant privacy (e.g., GDPR, CCPA) and data protection concerns. The "ownership
consideration" mentioned highlights the need for clear IP policies for AI-generated code.

### IEC Standards for AI in Software Development (e.g., ISO/IEC JTC 1/SC 42, ISO/IEC 42001, ISO/IEC 5338):

* ISO/IEC JTC 1/SC 42: This joint committee develops foundational and horizontal standards for AI. The
concepts presented by Eno Reyes, particularly the "entire ecosystem" approach of agentic
development, align with the scope of SC 42's work. Adherence to these emerging standards will be
crucial for interoperability and responsible AI deployment.
* ISO/IEC 42001 (AI Management System - AIMS): This standard establishes requirements for an
AIMS. Factory's platform, and organizations adopting it, would benefit greatly from implementing an
AIMS to manage AI-related risks systematically, ensuring transparency, accountability, and ethical
considerations throughout the AI lifecycle. This includes risk, lifecycle, and data quality management
for AI systems.
* ISO/IEC 5338 (AI Lifecycle Management): This standard provides a framework for controlling,
managing, executing, and improving AI systems throughout their lifecycles. Factory's claims of
continuous improvement, automated runbook generation, and learning from incidents directly map to
the objectives of ISO/IEC 5338. Compliance would require structured processes for how agents learn,
how their models are updated, and how new processes they derive are validated and integrated.

In conclusion, Eno Reyes's vision of agent-native development promises a significant leap in software
production efficiency and reliability. However, realizing this future securely and compliantly demands a
proactive and comprehensive approach to managing AI risks, addressing new attack vectors, and
rigorously applying existing and emerging security and governance frameworks to the autonomous,
context-driven AI agents. The human element shifts from direct execution to sophisticated
orchestration, requiring a parallel evolution in human skills and oversight mechanisms to ensure
trustworthiness.

**Note**: This assessment identifies risks based on the speaker’s described architecture and goals. Specific
security controls (e.g., authentication mechanisms, audit trails) were not detailed in the presentation,
so these gaps are flagged as critical blind spots for adopters to resolve.
