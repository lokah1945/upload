
# OpenClaw Computer Autonomous Project

## Executive Summary

The OpenClaw Computer Autonomous Project aims to create a comprehensive, autonomous computing platform that seamlessly integrates large language models (LLMs), multi‑agent orchestration, and real‑world tooling to automate complex workflows. Building on insights from the Perplexity Computer’s end‑to‑end architecture and the more expansive OpenClaw v2 design, this project unifies planning, execution, reasoning, and evaluation into a robust, multi‑plane system. At its core, the platform converts user goals into structured task graphs, routes those tasks to specialized sub‑agents, executes them in secure sandbox environments, and evaluates results against predefined success criteria. A multi‑model router selects the most appropriate LLM provider for each task, while a rich memory hierarchy persists user preferences, artifacts, and job state. Reliability and safety are enforced through rigorous error taxonomy, retry logic, and human‑in‑the‑loop approval mechanisms. Unlike generic chatbots, OpenClaw is engineered specifically for software engineering and technical knowledge work, with connectors to code repositories, project management tools, CI/CD pipelines, and browser automation. The system is designed for extensibility with domain‑specific skills and a formal API contract, providing a foundation for future expansion and integration.

## Vision & Mission

**Vision:** To build an autonomous computer that augments and accelerates technical work by orchestrating multiple intelligent agents, dynamic tools, and rigorous evaluation frameworks in a single, coherent platform. The system should empower users to define high‑level goals and delegate execution to a reliable, transparent, and extensible autonomous engine that handles planning, coding, research, testing, and deployment.

**Mission:** OpenClaw will deliver a production‑grade, multi‑plane architecture that translates user intents into executable plans, selects and coordinates the best AI models, manages isolated execution environments, and consistently produces high‑quality results. By prioritizing software engineering workflows and embedding evaluation, approval, and telemetry mechanisms at every stage, OpenClaw seeks to become the premier autonomous computing platform for deep work.

## Background & Context

The Perplexity Computer (v1) described a modular system composed of an orchestration layer, agent runtime, model router, sandbox execution environment, and memory & artifact store. It outlined how user requests are transformed into jobs and task graphs, scheduled through a planner agent, executed by sub‑agents in sandboxes, and integrated with external tools like email, search, browsers, and connectors. Error handling, long‑running workflows, memory isolation, and cost accounting were also covered, with pseudocode for orchestrator components.

OpenClaw v2 expands on this foundation by proposing a four‑plane architecture—Gateway/Control, Execution, Reasoning, and Evaluation—each with specialized responsibilities. The design introduces nuanced roles for sub‑agents (Planner, Coder‑fast, Coder‑strong, Verifier, Reviewer, Retriever, UI‑Agent), a sophisticated router using task complexity and risk tiers, patch and test harnesses for safe code execution, a memory hierarchy with session/repo/team/operational scopes, and a comprehensive error taxonomy with retry matrices. V2 also defines explicit API contracts, telemetry models, KPIs, connectors (GitHub, Jira, Slack, CI, etc.), a browser engine, evaluation frameworks, and a 90‑day roadmap.

## Problem Statement

Modern knowledge workers—especially software engineers—face complex tasks that span research, coding, testing, documentation, and deployment. Existing LLM‑powered assistants excel at producing text but struggle with multi‑step workflows, long‑running operations, stateful context, tool integration, and rigorous evaluation. The challenge is to build a system that can decompose high‑level goals into actionable tasks, select the right models and tools, execute them reliably, maintain context across sessions, handle errors gracefully, and provide visibility and control to users. Additionally, there is a need to differentiate between general digital assistants and a dedicated engineering automation system that focuses on code quality, CI/CD integration, and industry standards. The problem is further complicated by the variability of tasks, the need for safe execution sandboxes, and the requirement to orchestrate multiple agents without losing transparency or accountability.

## Solution Overview

The OpenClaw solution integrates the Perplexity Computer’s orchestration, agent runtime, model router, sandbox, and memory layers with the v2 multi‑plane architecture and domain‑specific enhancements. The system accepts a user goal via the gateway API, authenticates and quotas the request, creates a job entity, and invokes the planner agent to generate a task graph. A router evaluates the task’s complexity, risk, and content to choose appropriate reasoning roles (e.g., Planner, Coder, Verifier) and select the best LLM models. Tasks are executed in isolated sandbox environments that provide a real file system, browser, and connectors to external services like GitHub or Jira. A memory hierarchy persists context at session, repository, team, and operational levels. Tools are abstracted through a unified interface that normalizes connector actions and manages credentials. During execution, the system monitors error conditions against a formal taxonomy, triggers retries or recovery actions, and escalates to human reviewers or approval policies when necessary. Telemetry and KPIs are collected throughout, feeding an evaluation plane that benchmarks agent performance, monitors cost and reliability, and informs future improvements.

## Core Features

- **Multi‑Plane Architecture:** Separates concerns into Gateway/Control (authentication, routing, planning), Execution (sandbox, tool adapters, connectors), Reasoning (task decomposition, model selection, sub‑agent orchestration), and Evaluation (metrics, tests, human feedback) planes.
- **Planner and Task Graph:** Transforms user goals into structured task graphs with explicit dependencies, enabling parallel and staged execution.
- **Model Router:** Chooses the optimal LLM provider (OpenAI, Anthropic, Google, xAI, etc.) based on task type, complexity, budget, and risk considerations.
- **Dynamic Sub‑Agents:** Specialized roles (Planner, Coder‑fast, Coder‑strong, Verifier, Reviewer, Retriever, UI‑Agent) perform planning, coding, research, verification, review, retrieval, and UI actions. Routing logic uses complexity and risk tiers to assign the right role.
- **Sandbox Execution Environment:** Provides a real file system, secure code execution (Python/Node), browser automation, and connectors to external applications. Each task runs in its own sandbox to ensure isolation and reproducibility.
- **Memory Hierarchy:** Supports session‑level context (conversation), repository memory (codebase state and history), team memory (shared across collaborations), and operational memory (global platform knowledge). Memories store artifacts, preferences, and experience for later reuse.
- **Tool Abstraction and Connectors:** Normalizes interactions with GitHub, Jira, Slack, CI/CD, search engines, and browsers, exposing consistent APIs for sub‑agents. Skills allow domain‑specific toolkits to be plugged in.
- **Error Taxonomy and Retry Engine:** Categorizes failures across model, tool, environment, browser, connector, and task logic classes. Defines retry policies, recovery playbooks, severity levels, and escalation to human review. A telemetry system collects error metrics and enforces reliability SLOs.
- **Patch and Test Harnesses:** Safely apply code modifications (patches) and run unit/integration tests within the sandbox. Test orchestration uses CLI and dataset exploration tools, verifying that changes meet quality criteria.
- **Approval and Review Workflows:** Supports human‑in‑the‑loop approvals at configurable checkpoints. Reviewers can inspect diffs, reasoning logs, and test results before allowing the agent to merge changes or deploy artifacts.
- **Evaluation Framework:** Benchmarks sub‑agent performance using KPIs (accuracy, latency, cost, reliability), synthetic tasks, golden datasets, and real‑user feedback. Results inform routing heuristics, memory policies, and skill improvements.
- **Extensible API Contract:** Defines services for routing, tasks, planning, context preparation, memory management, artifact retrieval, approval handling, sub‑agent management, tool execution, connector actions, and telemetry. A uniform response envelope standardizes status and error reporting.
- **Roadmap and Strategy:** Provides a phased implementation plan (30/60/90 days) covering milestone definitions, required components, test harness development, evaluation criteria, and long‑term expansion into domain‑specific skills and production integrations.

## Technical Architecture

### System Components

The system is organized into several major components:

1. **Gateway & Control Plane:** Handles authentication, rate limiting, API versioning, and routing. It distinguishes between chat, deep research, and autonomous computer modes. For the computer mode, it creates a `Job` entity with metadata (user ID, job ID, created date, time limits, preferences). It invokes the Planner to build a task graph and stores the job in a database. It also includes scheduling logic to determine which job runs when and at what priority.
2. **Planner and Router:** The Planner (a high‑capacity LLM) decomposes the goal into tasks with descriptions, types, dependencies, and resource requirements. The Router uses the task characteristics (complexity tier, risk tier, domain, current budget and memory state) to choose LLM models (via the Model Harness) and assign roles (Planner, Coder‑fast, Coder‑strong, Verifier, Reviewer, Retriever, UI‑Agent). A dynamic agent router ensures that tasks are routed to the best agent for the job, and it can re‑route tasks if the initial attempt fails or if conditions change.
3. **Execution Plane:** Each task runs in a secure sandbox with a real file system, browser session, and optional environment (Node, Python). Sub‑agents execute code, run CLI commands, browse the web, and interact with connectors (GitHub, Jira, Slack, email). A patch harness applies diffs and a test harness runs unit/integration tests. The sandbox router abstracts the underlying infrastructure, supporting multiple providers (e.g., Cloudflare Workers, in‑house clusters) and ensuring resource isolation. Task state and artifacts are persisted to a memory store.
4. **Reasoning Plane:** Contains the core LLM agents with specialized roles. The Planner decomposes tasks, the Coder roles generate code, the Verifier runs tests and static checks, the Reviewer inspects diffs and test results, and the Retriever gathers external information. Roles can be combined or repeated depending on task complexity and risk. The reasoning plane interacts with memory, tool adapters, and the router to produce plan steps, code patches, test harness invocations, and review reports. It also handles fallback logic when errors occur.
5. **Evaluation Plane:** Collects telemetry on task outcomes, execution metrics, error occurrences, and user feedback. It benchmarks different agent configurations against synthetic and real tasks, providing insights into reliability, cost, accuracy, and latency. The evaluation plane supports A/B testing, golden datasets, and continuous regression tests to inform improvements to the router, planning heuristics, and model selection.

### Data Flow / Process Flow

1. **Intake:** A user submits a goal via the API. The gateway authenticates, rate‑limits, and creates a job record.
2. **Routing & Planning:** The Planner agent receives the job and produces a task graph. The router classifies each task by type, complexity, and risk, and chooses models and roles accordingly. The job transitions into the planning state.
3. **Context Preparation:** The system loads relevant memory (session, repo, team, operational) and artifacts. The planning service may fetch remote code repositories and infer repository structure. Checkpoints are created in a `Checkpoint` table, capturing the job state at each transition.
4. **Execution Loop:** For each task in dependency order:
   - The router selects a sub‑agent and model. The agent queries connectors if needed (e.g., GitHub for diff context, Jira for issue metadata, search engines via the Browser service). The sandbox environment is prepared with necessary files and dependencies.
   - The agent generates a plan step or code patch. The patch harness applies diffs and the test harness runs relevant tests. If tests fail, the agent may retry, re‑plan, or request additional context. Errors are classified via the error taxonomy and the retry engine triggers recovery actions (e.g., revert patch, use Coder‑strong instead of Coder‑fast).
   - If the task involves browsing or research, the Browser plane provides page content and a safe execution environment. If connectors are used, the Connector service normalizes actions (e.g., comment on a PR, create a Jira ticket).
   - Upon success, artifacts (e.g., code diffs, reports) are persisted. The task state is updated and a checkpoint is recorded. If an approval step is defined, the system pauses and awaits human review before proceeding.
   - The system monitors budget consumption (API tokens, compute, time) and may trigger early termination or degraded modes if limits are exceeded.
5. **Verification & Review:** The Verifier agent runs unit tests, static analyzers, linters, or integration tests. The Reviewer agent inspects diff context, test results, and reasoning logs to ensure quality. If issues are found, the task may be retried or escalated for human intervention.
6. **Completion / Failure:** Once all tasks succeed or the system reaches an end state, the job is marked complete and artifacts are delivered (e.g., code merged, slides emailed). On unrecoverable failure, the job state is set to failed with a summary of error classes and context. Metrics and logs are sent to the evaluation plane.

### AI Capabilities

OpenClaw’s AI agents use large language models with specialized system prompts tailored to their roles. The Planner uses high‑capacity models (e.g., GPT‑4 or Claude Opus) to create structured plans. Coder agents balance speed and accuracy by selecting between a fast but less accurate model and a strong but slower model. The Verifier and Reviewer use models optimized for code comprehension, reasoning, and adherence to acceptance criteria. The Router’s heuristics (task class, complexity tier, risk tier, budget) determine when to switch models, retry with a stronger model, or involve human approval. Agents leverage external search engines, APIs, and memory to gather information. They maintain reasoning logs and produce structured outputs (JSON, diff patches) rather than free‑form text, enabling deterministic orchestration.

### UX / Interaction Model

The user interacts with OpenClaw primarily through an API or UI that supports three modes: chat, research, and autonomous computer. In the computer mode, the user defines a high‑level goal and optionally attaches context (e.g., repository URL, issue ID). The UI displays progress on the task graph, budget consumption, test results, and pending approvals. Users can approve or reject patches, provide feedback on reasoning, and adjust preferences (e.g., model choice, risk tolerance). The UI emphasizes transparency, showing reasoning logs, diffs, and telemetry. Shortcuts allow users to intervene, ask follow‑up questions, or cancel jobs. The UI also exposes saved artifacts and memory, enabling retrieval of past results.

### Comparison with Perplexity Computer

Perplexity Computer v1 focused on establishing a modular architecture for general workflows: it defined an orchestration layer, agent runtime, multi‑model router, sandbox execution environment, memory store, task planner, and scheduler. It included pseudocode and schema designs for tasks, jobs, memory, and artifacts, along with error handling, multi‑tenant isolation, and cost accounting. However, it remained conceptual and targeted generic digital work rather than a specific domain.

OpenClaw v2 builds on these foundations but shifts the focus to engineering and technical knowledge work. It formalizes a four‑plane architecture, distinguishes roles for sub‑agents, and introduces a sophisticated router informed by complexity and risk tiers. V2 also defines a patch/test harness, skill contracts, evaluation frameworks, telemetry models, KPIs, and connectors to developer tools. It adds a memory hierarchy that separates session, repository, team, and operational context. The error taxonomy is expanded with recovery strategies and a retry matrix. Approval policies and review workflows are emphasized, and UI guidelines are defined for transparency. Compared to Perplexity v1, OpenClaw v2 provides a far more detailed blueprint, targeted domain, and production‑ready design.

### Unique Advantages

- **Domain Specialization:** By focusing on software engineering and technical projects, OpenClaw can optimize its tools, connectors, evaluation metrics, and sub‑agent roles for high‑impact developer tasks.
- **Fine‑Grained Routing:** Complexity and risk tiers enable the router to adaptively select the right models and roles, balancing speed, cost, and quality. Step‑level rerouting and recovery policies ensure resiliency.
- **Integrated Patch/Test Harness:** Securely applies code changes and runs tests within the sandbox, reducing the risk of breaking builds and enabling iterative development loops.
- **Human‑In‑The‑Loop:** Approval modes (auto‑approve, auto‑reject, manual) allow organizations to calibrate trust and safety. Reviewers have access to reasoning logs, diffs, and tests, improving accountability.
- **Multi‑Layer Memory:** Separating session, repo, team, and operational memories allows precise context retrieval and reduces prompt bloat. Memories persist across jobs and teams, enabling knowledge reuse.
- **Comprehensive Error Handling:** A detailed error taxonomy with recovery playbooks and severity levels ensures robust execution. The retry engine and escalation logic minimize downtime.
- **Evaluation & KPIs:** Built‑in benchmarking, telemetry, and evaluation frameworks provide quantitative feedback for continuous improvement. KPIs (accuracy, latency, cost, reliability) guide optimization and trade‑off decisions.
- **Extensible Skills & Connectors:** Developers can add domain‑specific skills (e.g., data science, infrastructure) and connectors to new tools. The skill contract formalizes input/output and memory usage, enabling modularity.
- **Production‑Grade API:** A well‑defined API contract across services supports integration with external systems, microservices, and UI clients.

## Implementation Strategy

OpenClaw’s implementation follows a phased roadmap:

1. **Foundational Infrastructure (Day 0‑30):** Set up the gateway, identity management, database schemas (Job, Task, Memory, Artifact, Error), sandbox router, and basic agent runtime. Implement the Planner with simple heuristics and integrate a multi‑model router. Provide core connectors (GitHub, browser) and a minimal UI for job submission and monitoring.
2. **Enhanced Routing & Agents (Day 30‑60):** Introduce complexity and risk tiers, dynamic sub‑agent roles (Coder‑fast/strong, Verifier, Reviewer), and step‑level re‑routing. Build the patch harness and test harness. Implement the memory hierarchy and context preparation service. Add error taxonomy and retry engine. Expand connectors (Jira, Slack, CI/CD). Start collecting telemetry and basic KPIs.
3. **Evaluation & Expansion (Day 60‑90):** Develop the evaluation plane with benchmarking infrastructure, golden datasets, synthetic tests, and A/B testing. Refine routing heuristics based on evaluation results. Extend approval policies and UI features (diff viewer, reasoning logs, budgets). Add new skills (data analysis, infrastructure) and connectors (cloud providers, databases). Harden security, observability, and scalability for production readiness.

### Development Roadmap

The project’s 90‑day roadmap includes milestones for each plane, including releasing a functional planner and router by Day 30, integrating patch/test harnesses by Day 60, and deploying the evaluation framework by Day 90. Parallel threads address UI development, connector integrations, telemetry instrumentation, and security controls. After the initial release, focus shifts to domain‑specific skills, advanced reasoning models, and enterprise features (RBAC, compliance, cost optimization).

### Technical Deep Dive

The subsequent sections preserve the full technical details from the Perplexity Computer v1 documents and the OpenClaw v2 parts. They provide in‑depth descriptions of the mental model, task planning, scheduler architecture, agent runtime, model router design, sandbox environment, dynamic sub‑agents, error handling, long‑running workflows, memory and artifacts, SQL schemas, API service contracts, cost accounting, scaling, security, 4‑plane architecture, KPIs, connectors, browser plane, evaluation frameworks, roadmaps, API schema definitions, router heuristics, retry matrices, patch/test harness logic, and implementation guidelines. These details are essential for engineers implementing or auditing the system and ensure that no technical nuance is lost in the high‑level summary.

## Risks & Constraints

- **Model Limitations:** LLMs may hallucinate or produce incorrect code. The router mitigates this by selecting stronger models or triggering reviews, but residual risk remains.
- **Resource Constraints:** Autonomous tasks can consume significant compute, API tokens, and time. Budget management, sandbox quotas, and early termination policies are needed.
- **Error Propagation:** Complex dependency chains can cascade failures. The retry engine and recovery policies must be carefully tuned to avoid infinite loops or partial state corruption.
- **Security & Privacy:** Sandbox isolation, connector access controls, and memory boundaries must prevent data leaks and unauthorized actions. Secrets management and RBAC are required.
- **Human Dependence:** Manual approvals improve safety but can slow execution. Balancing automation with human oversight requires careful policy tuning.
- **Scalability:** As tasks and users scale, orchestration, routing, memory retrieval, and evaluation pipelines must handle high throughput and concurrency without degrading performance.
- **Domain Drift:** While OpenClaw targets engineering tasks, expansion into other domains may require new skills, connectors, and evaluation criteria. Clear modularity and skill contracts are necessary to avoid architectural erosion.

## Future Expansion

Future directions for OpenClaw include adding domain‑specific skills for data science, infrastructure management, and design; expanding connectors to cloud providers (AWS, Azure, GCP), databases, and analytics tools; introducing autonomous macro‑agents that coordinate multiple micro‑agents; enhancing natural language UI for task definition; integrating with enterprise identity and access management; and exploring federated or privacy‑preserving architectures. Continuous evaluation against new benchmarks and real‑world usage will guide model upgrades and routing strategies. Collaboration features (shared memory, co‑editing) and offline support could further broaden adoption.

## Appendices

### Appendix A – Perplexity Computer v1 (Parts 1‑4)



#### PerplexityComputer.part1.md

# Perplexity Computer – End‑to‑End Technical Overview (Reconstructed)

> Catatan: Dokumen ini adalah rekonstruksi arsitektur berdasarkan gejala eksternal, pola industri, dan penjelasan publik yang sangat high‑level. Ini **bukan** dokumen desain resmi internal Perplexity.

---

## Part 0 – Mental Model Singkat

Secara konsep, Perplexity Computer bisa dipahami sebagai:

- **Orchestration Layer**  
  Mengelola job, task graph (DAG), scheduling, dan komunikasi antar agen.

- **Agent Runtime**  
  Menjalankan “agen” berbasis LLM yang bisa:
  - merencanakan langkah,
  - memanggil tools (search, browser, email, dll.),
  - menulis/membaca file,
  - berinteraksi dengan sandbox eksekusi.

- **Model Harness / Multi‑Model Router**  
  Lapisan yang mengabstraksi provider LLM (OpenAI, Anthropic, Google, xAI, dst.) dan memilih model yang tepat sesuai jenis tugas.

- **Sandbox Execution Environment**  
  Lingkungan eksekusi terisolasi per job/task:
  - filesystem nyata,
  - browser nyata,
  - runtime kode (Python/Node, dsb.),
  - konektor ke aplikasi eksternal (Gmail, GitHub, dsb.).

- **Memory & Artefact Store**  
  Menyimpan:
  - preferensi/pengalaman user,
  - state job yang long‑running,
  - semua artefak (file, laporan, kode, log).

---

## Part 1 – From User Request to “Job”

### 1.1. User mengirim goal

User tidak hanya mengirim “pertanyaan”, tapi seringkali:

- *“Bangun aplikasi X dan deploy”*  
- *“Riset topik Y dan bikin slide Z lalu kirim ke email saya”*

Request ini diperlakukan sebagai **Goal** untuk sebuah **Job**, bukan sekadar chat message.

### 1.2. Gateway / API layer

Komponen pertama yang menerima request:

- Autentikasi, quota, rate limit.
- Membedakan mode:
  - Chat biasa,
  - Deep Research,
  - Computer (workflow).

Jika mode Computer:

- Buat entity `Job` dengan:
  - `job_id`,
  - `user_id`,
  - `created_at`,
  - konfigurasi (batas waktu, preferensi model, dsb.).

- Simpan di database (misalnya Postgres).

---

## Part 2 – Perencanaan: dari Goal → Task Graph

### 2.1. Core “Planner / Controller” Agent

Job baru dilewatkan ke **Planner Agent**, yang berjalan di atas model frontier (misal model setara Opus / GPT kelas reasoning tinggi).

Prompt sistemnya kira‑kira:

- Menjelaskan:
  - kamu adalah “controller”,
  - kamu tidak menyelesaikan sendiri semua kerja,
  - tugasmu adalah memecah goal menjadi **task graph** yang bisa dikerjakan sub‑agen.

Planner memproduksi output struktural (bukan teks bebas), misal:

```json
{
  "tasks": [
    {
      "id": "research_competitors",
      "description": "Find top 5 competitors for product X, and summarize their features and pricing.",
      "type": "research",
      "depends_on": []
    },
    {
      "id": "draft_slides",
      "description": "Create a slide outline based on research.",
      "type": "doc_generation",
      "depends_on": ["research_competitors"]
    },
    {
      "id": "finalize_slides",
      "description": "Refine slide content and export as PPTX.",
      "type": "doc_polish",
      "depends_on": ["draft_slides"]
    },
    {
      "id": "send_email",
      "description": "Email the slide deck to the user.",
      "type": "email_action",
      "depends_on": ["finalize_slides"]
    }
  ]
}
```

### 2.2. Penyimpanan Task Graph

Orchestrator menyimpan task graph sebagai tabel:

- `jobs` – 1 row per job.
- `tasks` – 1 row per task:
  - `task_id`, `job_id`,
  - `type`, `status` (pending/running/done/failed),
  - `depends_on` (list / tabel relasi),
  - `assigned_worker_id`,
  - pointer ke artefak (file / summary) yang dihasilkan task.

Task graph ini dianggap sebagai **DAG** (Directed Acyclic Graph) yang akan dieksekusi oleh scheduler.

---

## Part 3 – Scheduler & Sub‑Agent Execution

### 3.1. Scheduler

Scheduler periodik (atau event‑driven) melakukan:

1. Query task dengan:
   - `status = 'pending'`,
   - semua `depends_on` statusnya `done`.

2. Untuk tiap task eligible:
   - pilih worker sandbox yang idle atau sesuai kapasitas,
   - tandai task `status = 'running'`,
   - kirim payload ke worker melalui queue / RPC (misalnya gRPC, NATS, Kafka, dsb.).

Payload berisi:

- `task_id`, `job_id`,
- `task_type`, `description`,
- referensi ke artefak yang perlu dibaca,
- konfigurasi model/tool.

### 3.2. Sub‑Agent Runtime

Di worker, ada **Agent Runtime** yang:

1. Membangun konteks:
   - memuat info job (goal awal),
   - membaca artefak dari task lain yang sudah selesai,
   - memuat memory user yang relevan.

2. Memilih **peran** dan **model**:
   - misalnya: `type = research` → pakai “Research Agent” + model X,
   - `type = doc_generation` → pakai “Writer Agent” + model Y.

3. Menjalankan loop “LLM + tools” untuk menyelesaikan task.

Pseudo‑loop:

```python
state = initial_state_from_task(task)

while not state.done and step < MAX_STEPS:
    # 1) Compose prompt (system + user + tools + history)
    prompt = build_prompt(state)

    # 2) Call LLM (via model router)
    llm_response = call_model(prompt, tools_schema=AVAILABLE_TOOLS)

    # 3) Jika LLM memanggil tool
    if llm_response.tool_call:
        tool_result = execute_tool(llm_response.tool_call)
        state = update_state_with_tool_result(state, tool_result)
    else:
        # 4) LLM memberikan final output untuk task ini
        state.output = llm_response.content
        state.done = True

    step += 1

save_task_output(task_id, state.output)
mark_task_done(task_id)
```

---

## Part 4 – Multi‑Model Router / Model Harness

### 4.1. Konfigurasi Model per Role

Di bawah agent runtime ada **Model Router**. Ia punya mapping:

- `planning`, `controller` → model reasoning kuat (costly, slow).
- `research` → model dengan kemampuan search + summarization bagus.
- `code` → model yang unggul dalam coding.
- `image` → model generasi gambar.
- `video` → model generasi video.
- `fast_utility` → model ringan & cepat.
- `long_context` → model dengan context window besar.

Runtime memanggil router seperti:

```ts
const model = router.pick({
  role: "research",
  contextTokens: estimated_context_tokens,
  latencyBudgetMs: 20000,
  costSensitivity: "medium"
});
```

Model router mengabstraksi vendor:

- OpenAI (GPT),
- Anthropic (Claude),
- Google (Gemini, Veo),
- xAI (Grok),
- model lain.

### 4.2. Mode Multi‑Model / “Council‑like”

Untuk beberapa jenis tugas (khususnya “critical reasoning” atau yang butuh verifikasi), orchestrator bisa:

1. Kirim prompt yang sama ke beberapa model secara paralel.
2. Kumpulkan jawaban.
3. Panggil model lain (atau salah satu) untuk:
   - membandingkan jawaban,
   - menyusun summary dan menandai area yang tidak sepakat.

Pola ini mirip “model ensemble / council”, tapi di Computer diterapkan **bukan hanya untuk satu jawaban ke user**, kadang untuk membantu agen membuat keputusan internal yang lebih robust.

---

## Part 5 – Sandbox Eksekusi (Filesystem, Browser, Kode)

### 5.1. Isolated Compute per Job/Task

Setiap tugas yang butuh:

- menjalankan kode (Python, Node),
- mengoperasikan browser beneran,
- menyimpan file,

dijalankan di **sandbox terisolasi**.

Secara implementasi umum:

- Backend “Sandbox Router” menyediakan API:
  - `CreateSandbox(template)`
  - `RunCommand(sandbox_id, command, files...)`
  - `ReadFile(sandbox_id, path)`
  - `DestroySandbox(sandbox_id)`

- `template` menentukan:
  - base image (python, node, mixed),
  - environment (tools, browser, CLI).

### 5.2. Integrasi dengan Agent Runtime

Di agent runtime, tools yang terlihat LLM kira‑kira seperti:

- `run_python_code`,
- `write_file`,
- `read_file`,
- `run_browser_action`,
- `send_http_request`,
- dll.

Ketika LLM output tool call (misalnya `run_python_code`):

1. Runtime terjemahkan ke panggilan ke Sandbox Router.
2. Sandbox Router memastikan ada container/VM aktif untuk job ini.
3. Kode dijalankan, output dan file baru disimpan di sandbox.
4. Hasil (stdout, stderr, path file, dsb.) dikembalikan ke agent runtime sebagai input turn berikutnya.

### 5.3. Browser Automation

Di dalam sandbox:

- ada headless browser (Chromium) dengan binding ke bahasa (Python/Node),
- LLM cukup memberikan high‑level instruction:
  - “open URL X”,
  - “search for Y”,
  - “screenshot section Z”.

Runtime menerjemahkan instruksi itu ke skrip (Playwright/Puppeteer) yang kemudian dijalankan di sandbox.

State (cookies, session) disimpan di sandbox volume per‑job.

---

## Part 6 – Tools & Integrasi Aplikasi (Email, GitHub, dsb.)

### 6.1. Tool Registry

Ada registry yang mendeskripsikan semua tools yang diizinkan:

- `gmail_send`
- `github_create_pr`
- `notion_update_page`
- `slack_post_message`
- dll.

Masing‑masing tool punya:

- JSON schema input,
- endpoint internal yang mengeksekusi,
- policy tentang apakah butuh konfirmasi user.

### 6.2. Tool Calling dari LLM

Di prompt agent, didefinisikan schema tools ini. LLM:

- memutuskan kapan memakai tool,
- membentuk JSON sesuai schema.

Runtime:

1. Validasi JSON.
2. Panggil service tool yang sesuai (Gmail connector, GitHub connector, dsb.).
3. Tangkap respon (sukses/gagal, data tambahan).
4. Memberi respon ini kembali ke LLM.

Tool‑tool ini digunakan sepanjang workflow Computer untuk:

- mengirim email hasil,
- commit/push ke repo,
- update task management system.

---

## Part 7 – Dynamic Sub‑Agents & Error Handling

### 7.1. Sub‑Agents dari Masalah

Jika sebuah agent menemui masalah yang ia anggap perlu dipecah (kurang info, error kompleks, dll.):

1. Agent membuat “problem report” dan memanggil Planner/Controller lagi dengan konteks:
   - state saat ini,
   - error yang terjadi,
   - hipotesis langkah lanjutan.

2. Planner mengembalikan sub‑task baru yang disisipkan sebagai node tambahan di task graph:
   - misal `debug_build_error`,
   - `collect_missing_requirements`.

3. Scheduler kemudian mengeksekusi sub‑tasks baru itu.

Dengan demikian, DAG bisa **bertumbuh dinamis** selama job berjalan.

### 7.2. Retry & Rollback

Untuk task yang gagal:

- Orchestrator bisa:
  - retry otomatis beberapa kali (dengan variasi prompt),
  - meminta konfirmasi user,
  - atau menandai job sebagai membutuhkan intervensi manual.

Rollback di level Computer biasanya bukan rollback database transaksional, tetapi:

- menghentikan job,
- atau menambah task “remedial” baru (misalnya revert PR, kirim email koreksi, dsb.).

---

## Part 8 – Memory & Artefacts

### 8.1. User Memory

Di luar Computer, sistem memiliki **memory per user** yang menyimpan:

- preferensi komunikasi (bahasa, gaya penulisan),
- projek yang sering disebut,
- constraints (budget, tech stack favorit, dsb.).

Saat job Computer dibuat, sebagian memory ini diinject ke Planner dan sub‑agents yang relevan.

### 8.2. Job Memory & Artefact Store

Untuk setiap job:

- semua output disimpan sebagai artefak:
  - file (dokumen, slide, kode),
  - ringkasan intermediate (JSON, markdown),
  - log eksekusi.

Artefak disimpan di storage (S3/minio/gcs sejenis), dengan metadata di DB.

Agent runtime tidak selalu membawa seluruh artefak ke context LLM; biasanya:

- ada **retrieval layer** (search/embedding) yang memilih artefak mana yang relevan untuk prompt tertentu,
- atau summary multi‑level untuk menjaga token tetap masuk akal.

---

## Part 9 – Long‑Running Workflows & Concurrency

### 9.1. Job Multi‑Jam / Multi‑Hari

Karena Computer bisa berjalan lama:

- task tidak semuanya dijalankan dalam satu cluster session,
- worker sandbox bisa dimatikan dan dinyalakan ulang,
- state job tetap ada di DB dan storage.

Mekanisme:

- setiap task menulis checkpoint (misalnya progress index, file partial),
- orchestrator menyimpan info itu,
- jika worker mati, scheduler bisa:
  - assign task ke worker lain,
  - melanjutkan dari checkpoint terakhir (kalau dirancang demikian).

### 9.2. Banyak Job Sekaligus

Arsitektur multi‑tenant:

- banyak `job_id` bisa berjalan paralel,
- scheduler dan worker dibuat horizontal scalable.

Orchestrator mengatur:

- limit per user / per org,
- prioritas (misalnya job pendek vs job panjang),
- fairness (tidak ada satu user yang menghabiskan seluruh cluster).

---

## Part 10 – Safety, Permissions, & Human‑in‑the‑Loop

### 10.1. Permissions

Karena Computer bisa:

- kirim email,
- menyentuh repo,
- mengakses data sensitif,

maka:

- setiap integrasi tools memerlukan linking (OAuth/API key),
- ada policy per tool:
  - misalnya `gmail_send` butuh explicit opt‑in user,
  - beberapa aksi “destruktif” mungkin selalu butuh konfirmasi.

### 10.2. Human‑in‑the‑Loop

Untuk langkah‑langkah tertentu:

- sebelum bertindak (misalnya push ke main, kirim email ke seluruh tim),  
- komputer bisa:
  - meminta review user (preview output),
  - menunggu approval.

Ini melindungi dari kesalahan fatal di workflow otomatis.

---

## Part 11 – Bagaimana Menggunakan Desain Ini untuk Implementasi Sendiri

Jika kamu ingin membangun sistem mirip Perplexity Computer:

1. **Mulai dari Orchestrator + Agent Runtime simpel**  
   - Job + Task DAG,
   - satu jenis agent dulu (research + writer),
   - satu provider model.

2. **Tambahkan Model Router**  
   - bungkus semua panggilan LLM lewat satu service,
   - tambah aturan routing berdasarkan role tugas.

3. **Tambahkan Sandbox Eksekusi**  
   - mulai dari container (Docker/K8s) yang menyediakan Python + browser,
   - expose API sederhana untuk `run_code` dan `run_browser_action`.

4. **Tambah Tools**  
   - definisikan JSON schema tools,
   - buat microservice untuk Gmail, GitHub, dsb.

5. **Iterasi ke Sub‑Agents Dinamis**  
   - izinkan agent meminta “bantu agen lain” ketika menemui masalah,
   - mutasi DAG di runtime.

6. **Layer Memory & Artefact**  
   - simpan semua file & ringkasan,
   - buat retrieval sederhana untuk memasukkan context ke LLM.

Desain di atas tidak sama 1:1 dengan server Perplexity, tapi cukup dekat untuk kamu jadikan blueprint implementasi multi‑agent/multi‑model orchestration yang modern.

---


#### PerplexityComputer.part2.md

# Perplexity Computer – Engineering Appendix

> Appendix ini melengkapi overview sebelumnya dengan skema tabel, kontrak service, dan contoh prompt sistem yang bisa kamu jadikan blueprint implementasi.

---

## Part A – Skema Tabel Inti

### A.1. Tabel `users`

Minimal:

```sql
CREATE TABLE users (
  id            UUID PRIMARY KEY,
  email         TEXT UNIQUE NOT NULL,
  name          TEXT,
  created_at    TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at    TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

### A.2. Tabel `user_memory`

Untuk menyimpan preferensi & fakta penting tentang user.

```sql
CREATE TABLE user_memory (
  id            UUID PRIMARY KEY,
  user_id       UUID NOT NULL REFERENCES users(id),
  key           TEXT NOT NULL,
  value         JSONB NOT NULL,
  embedding     VECTOR,           -- opsional, kalau pakai vector DB bisa diganti
  created_at    TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at    TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE INDEX idx_user_memory_user ON user_memory(user_id);
```

### A.3. Tabel `jobs`

Satu row per “Computer job”.

```sql
CREATE TYPE job_status AS ENUM (
  'pending',
  'running',
  'completed',
  'failed',
  'paused',
  'cancelled'
);

CREATE TABLE jobs (
  id              UUID PRIMARY KEY,
  user_id         UUID NOT NULL REFERENCES users(id),
  title           TEXT,
  goal            TEXT NOT NULL,         -- raw user intent
  mode            TEXT NOT NULL,         -- e.g. "computer"
  status          job_status NOT NULL DEFAULT 'pending',
  config          JSONB,                 -- batas resource, preferensi model, dsb.
  created_at      TIMESTAMPTZ NOT NULL DEFAULT now(),
  started_at      TIMESTAMPTZ,
  completed_at    TIMESTAMPTZ,
  last_error      TEXT
);

CREATE INDEX idx_jobs_user ON jobs(user_id);
CREATE INDEX idx_jobs_status ON jobs(status);
```

### A.4. Tabel `tasks`

Mewakili node DAG.

```sql
CREATE TYPE task_status AS ENUM (
  'pending',
  'running',
  'completed',
  'failed',
  'cancelled',
  'skipped'
);

CREATE TABLE tasks (
  id              UUID PRIMARY KEY,
  job_id          UUID NOT NULL REFERENCES jobs(id),
  type            TEXT NOT NULL,      -- "research", "doc_generation", "code", etc.
  description     TEXT NOT NULL,
  status          task_status NOT NULL DEFAULT 'pending',
  assigned_worker TEXT,               -- worker/sandbox id
  priority        INT NOT NULL DEFAULT 0,
  planned_by      TEXT,               -- planner agent version / model
  metadata        JSONB,              -- info tambahan (misal tool hints)
  created_at      TIMESTAMPTZ NOT NULL DEFAULT now(),
  started_at      TIMESTAMPTZ,
  completed_at    TIMESTAMPTZ,
  last_error      TEXT
);

CREATE INDEX idx_tasks_job ON tasks(job_id);
CREATE INDEX idx_tasks_status ON tasks(status);
```

### A.5. Tabel `task_dependencies`

Relasi DAG.

```sql
CREATE TABLE task_dependencies (
  task_id         UUID NOT NULL REFERENCES tasks(id),
  depends_on_id   UUID NOT NULL REFERENCES tasks(id),
  PRIMARY KEY (task_id, depends_on_id)
);

CREATE INDEX idx_task_dep_depends_on ON task_dependencies(depends_on_id);
```

### A.6. Tabel `artefacts`

Menyimpan semua artefak (file, ringkasan, dsb.).

```sql
CREATE TYPE artefact_type AS ENUM (
  'file',
  'markdown',
  'json',
  'log',
  'summary'
);

CREATE TABLE artefacts (
  id              UUID PRIMARY KEY,
  job_id          UUID NOT NULL REFERENCES jobs(id),
  task_id         UUID REFERENCES tasks(id),
  type            artefact_type NOT NULL,
  name            TEXT NOT NULL,          -- e.g. "research_summary", "slides.pptx"
  storage_path    TEXT NOT NULL,          -- path di object storage
  mime_type       TEXT,
  size_bytes      BIGINT,
  metadata        JSONB,
  created_at      TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE INDEX idx_artefacts_job ON artefacts(job_id);
CREATE INDEX idx_artefacts_task ON artefacts(task_id);
```

---

## Part B – Kontrak Service Orchestrator & Worker

### B.1. API Orchestrator (HTTP)

#### 1. Create Job

`POST /v1/jobs`

```json
{
  "goal": "Research 5 competitors for our SaaS and create slide deck, then email it to my team.",
  "title": "Competitor research & deck",
  "config": {
    "max_runtime_minutes": 120,
    "model_prefs": {
      "planning": "core_reasoning",
      "research": "research",
      "doc_generation": "long_context"
    }
  }
}
```

Respon:

```json
{
  "job_id": "uuid",
  "status": "pending"
}
```

#### 2. Get Job Status

`GET /v1/jobs/{job_id}`

```json
{
  "job_id": "uuid",
  "status": "running",
  "progress": {
    "total_tasks": 7,
    "completed": 3,
    "failed": 0
  },
  "current_phase": "draft_slides",
  "last_error": null
}
```

#### 3. List Artefacts

`GET /v1/jobs/{job_id}/artefacts`

```json
[
  {
    "id": "uuid",
    "type": "summary",
    "name": "competitor_overview",
    "mime_type": "application/json"
  },
  {
    "id": "uuid",
    "type": "file",
    "name": "slides_v1.pptx",
    "mime_type": "application/vnd.openxmlformats-officedocument.presentationml.presentation"
  }
]
```

### B.2. Kontrak Orchestrator → Worker (Queue / RPC)

Misal pakai JSON via queue:

```json
{
  "task_id": "uuid",
  "job_id": "uuid",
  "task_type": "research",
  "description": "Find top 5 competitors for product X",
  "user_id": "uuid",
  "model_hint": "research",
  "input_artefacts": [
    {
      "id": "uuid",
      "name": "context_summary",
      "type": "summary"
    }
  ],
  "config": {
    "max_steps": 12
  }
}
```

Worker menjalankan agent runtime, lalu melaporkan:

```json
{
  "task_id": "uuid",
  "status": "completed",
  "output_artefacts": [
    {
      "id": "uuid",
      "name": "competitor_list",
      "type": "json",
      "storage_path": "jobs/{job_id}/competitors.json"
    },
    {
      "id": "uuid",
      "name": "competitor_summary",
      "type": "summary",
      "storage_path": "jobs/{job_id}/competitor_summary.md"
    }
  ]
}
```

---

## Part C – Kontrak Model Router

### C.1. Call Model

`POST /v1/models/generate`

```json
{
  "role": "research",
  "model_hint": "research",
  "messages": [
    { "role": "system", "content": "..." },
    { "role": "user", "content": "..." }
  ],
  "tools": [
    {
      "name": "web_search",
      "description": "Search the web",
      "parameters": { "type": "object", "properties": { "query": { "type": "string" } }, "required": ["query"] }
    }
  ]
}
```

Respon (mode tool calling):

```json
{
  "model": "provider/model-name",
  "usage": {
    "prompt_tokens": 1234,
    "completion_tokens": 456
  },
  "choices": [
    {
      "type": "tool_call",
      "tool_name": "web_search",
      "arguments": { "query": "top 5 competitors for ..." }
    }
  ]
}
```

Atau:

```json
{
  "choices": [
    {
      "type": "message",
      "role": "assistant",
      "content": "Here is the summary: ..."
    }
  ]
}
```

### C.2. Multi‑Model / Council Call (opsional)

`POST /v1/models/council`

```json
{
  "models": ["opus-4.6", "gpt-5.2", "gemini-3.0"],
  "messages": [
    { "role": "system", "content": "..." },
    { "role": "user", "content": "..." }
  ],
  "mode": "compare"          // atau "vote", "aggregate"
}
```

Respon:

```json
{
  "answers": {
    "opus-4.6": { "content": "..." },
    "gpt-5.2": { "content": "..." },
    "gemini-3.0": { "content": "..." }
  },
  "aggregate": {
    "summary": "...",
    "disagreements": [
      { "topic": "X", "details": "model A says..., model B says..." }
    ]
  }
}
```

---

## Part D – Kontrak Sandbox Execution

### D.1. Create Sandbox

`POST /v1/sandbox`

```json
{
  "template": "python-browser", 
  "job_id": "uuid"
}
```

Respon:

```json
{
  "sandbox_id": "sandbox-123"
}
```

### D.2. Run Command

`POST /v1/sandbox/{sandbox_id}/run`

```json
{
  "command": ["python", "main.py"],
  "files": {
    "main.py": "print('hello')\n"
  },
  "timeout_seconds": 120
}
```

Respon:

```json
{
  "exit_code": 0,
  "stdout": "hello\n",
  "stderr": "",
  "generated_files": [
    "output/report.md"
  ]
}
```

### D.3. Browser Action

`POST /v1/sandbox/{sandbox_id}/browser`

```json
{
  "actions": [
    { "type": "goto", "url": "https://example.com" },
    { "type": "wait_for_selector", "selector": "h1" },
    { "type": "screenshot", "path": "screens/home.png" }
  ]
}
```

Respon:

```json
{
  "result": "ok",
  "files": [
    "screens/home.png"
  ]
}
```

---

## Part E – Prompt Sistem Contoh

### E.1. Planner / Controller Agent

```text
You are the Planner and Controller for a multi-agent system called Computer.

You do NOT execute user work directly. Your job is to:
- understand the user's high-level goal,
- break it down into a graph of tasks (a DAG),
- assign each task a type and a clear description,
- define dependencies between tasks,
- optionally propose tool hints that will help.

Constraints:
- Output MUST be valid JSON with the following shape:
  {
    "tasks": [
      {
        "id": "string-task-id",
        "type": "one of: research, doc_generation, doc_polish, code, test, deploy, email_action, data_analysis, browser_action, misc",
        "description": "human readable one-sentence description",
        "depends_on": ["optional", "list", "of", "task-ids"],
        "tool_hints": ["optional-list-of-tool-names-or-empty"]
      }
    ]
  }
- IDs must be unique within this job.
- Use dependencies to express correct execution order.
- Keep tasks reasonably coarse-grained: each task should represent a meaningful unit of work that takes from a few seconds to several minutes to complete.

You will be given:
- the user's goal,
- any relevant user memory,
- any initial artefacts.

Your entire response must be the JSON object only.
```

### E.2. Research Agent

```text
You are the Research Agent in a multi-agent system.

Your job:
- given a research task description and relevant context,
- decide which web pages to read,
- extract the most important facts,
- produce a structured summary that other agents can consume.

You MUST:
- rely on the web_search and browser tools when needed,
- write outputs in a machine-friendly format (JSON or markdown with clear sections),
- avoid hallucinating: if something is unknown from available sources, say that it's unknown.

You will be provided:
- the high-level goal of the job,
- the specific research task description,
- any prior artefacts from other tasks.

When you finish:
- write a concise but complete summary that covers all required aspects,
- avoid making decisions outside of your task scope (those are for the Planner/Controller).
```

### E.3. Document / Slide Generator Agent

```text
You are the Document Generator Agent.

Input:
- a clear task description (e.g., "Create a slide deck"),
- one or more research summaries and structured data artefacts.

Your job:
- transform the input information into a well-structured document (markdown, doc outline, slide outline),
- be explicit about structure: sections, slides, bullet points.

Constraints:
- Do not invent facts; only use what is in the provided summaries and artefacts.
- Prefer neutral, professional tone unless instructed otherwise.
- If you see gaps or missing data, highlight them clearly instead of fabricating.

Output format:
- For slide decks: produce markdown with top-level headings per slide and bullet points for content.
- Optionally include notes to the future "export-to-PPTX" task (e.g., layout suggestions).
```

### E.4. Code / Execution Agent

```text
You are the Code Execution Agent in a sandbox environment.

You can:
- write files,
- run code using the provided tools (python, node, shell),
- read and modify existing project files.

Your responsibilities:
- implement the requested functionality as described in the task description,
- run tests or sanity checks where appropriate,
- write logs and reports as artefacts for other agents.

Rules:
- Use the filesystem and code execution tools instead of describing code in prose only.
- Prefer small, incremental steps: write code, run, inspect error, fix.
- Never access external network endpoints unless explicitly allowed by the task config.
- If a change is potentially destructive (e.g., data migration), prepare a plan and artefact describing the risk and steps before executing.

Output:
- A summary of what you changed or created,
- Pointers (paths) to key files,
- Test results (pass/fail) if applicable.
```

---

## Part F – Contoh Alur End‑to‑End (Ringkas)

Untuk goal:

> “Bangun prototype web app sederhana, deploy ke staging, dan kirim link ke email saya.”

1. Gateway buat `job` + kirim ke Planner.  
2. Planner buat DAG kira‑kira:
   - `collect_requirements` (research),
   - `design_architecture` (doc_generation),
   - `implement_backend` (code),
   - `implement_frontend` (code, depends on backend),
   - `deploy_staging` (deploy, depends on implement tasks),
   - `send_email_link` (email_action, depends on deploy).  
3. Scheduler jalankan task satu per satu / paralel sesuai dependency.  
4. Code Agent pakai sandbox + tools eksekusi.  
5. Deploy Agent pakai sandbox + integrasi deploy (CLI/CI).  
6. Email Agent pakai tool Gmail, kirim link staging ke user.  
7. Orchestrator update `job.status = completed`, artefak final tersimpan.

---




#### PerplexityComputer.part3.md

## Part G – Error Handling, Observability, dan Logging

### G.1. Error handling di level task

Ketika sebuah task gagal:

- Worker mengirim status ke orchestrator dengan:
  - `status = "failed"`,
  - `last_error` (stack trace ringkas / pesan LLM),
  - optional `retry_suggestion` (misalnya dari agent sendiri: “coba pakai versi Node 20”).
- Orchestrator punya kebijakan:
  - `max_retries` per task,
  - `retry_backoff` (exponential / fixed delay),
  - apakah boleh mengubah sedikit prompt (misalnya tambah instruksi debugging).

State mesin sederhana per task:

- `pending` → `running` → `completed`
- `pending` → `running` → `failed` → (retry) → `running` → `completed`  
- `pending` → `running` → `failed` (exceeded retries) → `failed` final

Jika `failed` final dan task adalah critical (tidak bisa di‑skip), job bisa berpindah ke:

- `status = "failed"`  
- atau `status = "paused"` sambil menunggu intervensi user.

### G.2. Error handling di level job

Job bisa:

- `failed` jika ada task non‑optional yang gagal permanen,
- `paused` jika:
  - butuh approval user,
  - butuh kredensial tambahan,
  - ada konflik state eksternal (misal repo berubah drastis),

UI bisa menyediakan:

- aksi “resume”,
- aksi “skip task ini saja” (kalau diizinkan),
- aksi “cancel job”.

### G.3. Logging & tracing

Untuk debugging sistem sekompleks ini, perlu:

- **structured logs** per task:
  - ID job, ID task, worker, model yang dipakai, tools yang dipanggil, durasi, ukuran prompt/response.
- **tracing** antar service:
  - correlation ID per job & task,
  - bisa pakai OpenTelemetry untuk menghubungkan:
    - panggilan orchestrator → worker → model router → sandbox → tools.

Ini penting untuk:

- memahami bottleneck (misal model tertentu terlalu lambat),
- menganalisis cost (token & runtime),
- forensic ketika Computer melakukan aksi tak terduga.

---

## Part H – Cost & Token Accounting

### H.1. Token usage tracking

Setiap panggilan LLM melalui model router mengembalikan:

- `prompt_tokens`,
- `completion_tokens`,
- `total_tokens`,
- `model_id`.

Router mengirim data ini ke:

- tabel `model_usage` (per job, per task, per model),
- atau sistem billing internal.

Contoh skema minimal:

```sql
CREATE TABLE model_usage (
  id              UUID PRIMARY KEY,
  job_id          UUID NOT NULL REFERENCES jobs(id),
  task_id         UUID REFERENCES tasks(id),
  model_id        TEXT NOT NULL,
  prompt_tokens   INT NOT NULL,
  completion_tokens INT NOT NULL,
  cost_usd        NUMERIC(18,6),
  created_at      TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

### H.2. Batasan cost/job

`jobs.config` bisa memuat:

- `max_token_total`,
- `max_cost_usd`,
- `max_steps`.

Orchestrator:

- menghitung kumulatif usage per job,
- menghentikan / paus job jika melewati batas,
- memberi artefak laporan “job berhenti karena melebihi budget”.

---

## Part I – Multi‑Tenant & Isolasi Data

### I.1. Isolasi antar user / org

Meskipun cluster sandbox, DB, dan model router shared:

- semua tabel (`jobs`, `tasks`, `artefacts`) selalu terkait ke `user_id` atau `org_id`,
- API layer memastikan:
  - user hanya bisa akses job dan artefak miliknya,
  - permission lebih granular (org / workspace) jika ada.

Sandbox:

- sebaiknya tidak pernah share filesystem antar job yang berbeda,
- environment variables / secrets diisolasi per org / per integration.

### I.2. Secret management

Integrasi eksternal (Gmail, GitHub, dsb.) menggunakan:

- sistem secret store (Vault / KMS / cloud secret manager),
- token diambil on‑demand oleh tools service, bukan disimpan di LLM context.

Prompt agent **tidak boleh** berisi secret. Yang dikirim ke model hanyalah:

- “kamu bisa memakai tool X untuk mengirim email sebagai user”.

---

## Part J – Mode Interaktif vs Mode Full‑Auto

### J.1. Mode full‑auto

Di mode ini:

- Planner langsung membuat DAG lengkap,
- Computer langsung menjalankan semua task sesuai rencana,
- hanya berhenti jika:
  - perlu kredensial baru,
  - ada aksi high‑risk yang diwajibkan untuk konfirmasi.

Kelebihan:

- hands‑off, bisa jalan berjam‑jam tanpa intervensi.

Kekurangan:

- butuh guardrail dan policy yang ketat.

### J.2. Mode semi‑auto / interaktif

Di mode ini:

- setelah Planner membuat DAG, UI menampilkan rencana ke user:
  - daftar task + urutan,
  - estimasi cost/time.
- User bisa:
  - edit / hapus task,
  - ubah prioritas,
  - lock beberapa constraint (misal “jangan sentuh repo main”).

Per langkah critical:

- sebelum `deploy` atau `send_email`,
- Computer mengirim notifikasi / minta approval.

---

## Part K – Scaling: Horizontal & Backpressure

### K.1. Horizontal scaling worker & sandbox

Worker agent runtime:

- stateless (kecuali koneksi sandbox), mudah di‑scale out,
- dipadukan dengan queue agar distribusi task merata.

Sandbox:

- bisa di‑scale berdasarkan:
  - jumlah job aktif,
  - rata‑rata durasi task,
  - resource usage (CPU, RAM).

### K.2. Backpressure & throttling

Untuk mencegah:

- overload ke provider LLM,
- saturasi integrasi (rate limit GitHub, Gmail, dsb.),

orchestrator + model router melakukan:

- rate limit global dan per user,
- antrean call LLM,
- fallback:
  - turunkan concurrency,
  - ganti model ke yang lebih murah/cepat jika tidak kritikal.

---

## Part L – Security & Compliance (High‑Level)

### L.1. Data privacy

Karena Computer punya akses:

- data sensitif user,
- akun kerja (email, repo, doc),

maka:

- data job & artefak:
  - dienkripsi at‑rest (storage) dan in‑transit (TLS),
- logs:
  - di‑scrub (masking) untuk fields tertentu,
  - diatur retention (hapus setelah X hari).

### L.2. Policy enforcement

Ada policy engine (bisa rule‑based) yang memeriksa:

- jenis tool,
- target (misal domain email, branch repo),
- level risiko.

Contoh kebijakan:

- tidak boleh kirim email ke domain eksternal tanpa konfirmasi,
- tidak boleh push ke `main` tanpa approval user,
- tidak boleh mengirim konten tertentu (safety policy LLM).

---

## Part M – Roadmap Implementasi Praktis (Untukmu)

Untuk menyulap semua ini jadi sesuatu yang bisa kamu coding:

1. **MVP 1 – Orchestrator + Single Agent**
   - implement tabel `jobs` + `tasks` + scheduler sederhana,
   - satu jenis agent (Research Agent),
   - satu model provider.

2. **MVP 2 – Tambah Sandbox & Code Agent**
   - buat service sandbox minimal (Docker/K8s) untuk run kode,
   - implement Code Execution Agent dengan loop LLM + tools.

3. **MVP 3 – Multi‑Model Router**
   - abstraksi panggilan LLM ke satu endpoint internal,
   - tambahkan 2–3 model berbeda,
   - rule routing berdasarkan role (`research`, `code`, `planning`).

4. **MVP 4 – Basic Tools (Email + HTTP)**
   - buat tool `http_request` + `send_email` (SMTP atau API),
   - definisikan JSON schema tool, integrate ke agent runtime.

5. **MVP 5 – Sub‑Agents Dinamis**
   - izinkan agent menambah task baru (mutasi DAG) untuk error/problem,
   - buat Planner yang bisa dipanggil ulang untuk “handle problem”.

6. **MVP 6 – UI Progress + Artefact Browser**
   - endpoint status job/task,
   - list & download artefak,
   - tampilan DAG simplistik (misal graph kecil).

Dengan langkah‑langkah ini, kamu akan punya “Computer‑like” sistem yang walaupun tidak sama persis, tapi secara arsitektur dan behaviour sangat dekat dengan konsep yang digambarkan publik.





#### PerplexityComputer.part4.md

# Perplexity Computer Clone – Project Skeleton & Pseudo Code

Catatan: Ini blueprint implementasi, bukan kode production. Sesuaikan dengan stack pilihanmu.

---

## 1. Struktur Project (Monorepo)

Direkomendasikan monorepo dengan beberapa service:

```text
perplexity-computer-clone/
  docs/
    architecture.md
  services/
    api-gateway/
    orchestrator/
    agent-worker/
    model-router/
    sandbox-router/
    tools-hub/
  packages/
    shared-types/
    shared-db/
    shared-logging/
    shared-config/
  infra/
    k8s/
    docker/
  scripts/
    dev/
    migrate-db/
```

### 1.1. `services/api-gateway`

- HTTP frontdoor untuk user.
- Auth, rate limit, route ke orchestrator, file download.

Folder:

```text
services/api-gateway/
  src/
    index.ts
    routes/
      jobs.ts
      artefacts.ts
      auth.ts
    middleware/
    config/
```

### 1.2. `services/orchestrator`

- Kelola `jobs`, `tasks`, DAG, scheduler.

```text
services/orchestrator/
  src/
    index.ts
    scheduler/
      scheduler-loop.ts
    controllers/
      jobs-controller.ts
      internal-tasks-controller.ts
    dag/
      planner-client.ts
      dag-store.ts
      dag-ops.ts
    queue/
      producer.ts   # kirim task ke agent-worker
    repositories/
      jobs-repo.ts
      tasks-repo.ts
      artefacts-repo.ts
    config/
```

### 1.3. `services/agent-worker`

- Worker yang menarik task dari queue dan menjalankan Agent Runtime.

```text
services/agent-worker/
  src/
    index.ts
    worker-loop.ts
    agent-runtime/
      runtime.ts
      state.ts
      tools-dispatcher.ts
      prompts/
        planner-system.txt
        research-system.txt
        docgen-system.txt
        code-system.txt
    clients/
      model-router-client.ts
      sandbox-router-client.ts
      tools-hub-client.ts
      orchestrator-client.ts
    config/
```

### 1.4. `services/model-router`

- Abstraksi semua model LLM, multi‑model routing.

```text
services/model-router/
  src/
    index.ts
    router/
      router.ts
      policies.ts
      health-check.ts
    providers/
      openai-provider.ts
      anthropic-provider.ts
      google-provider.ts
      xai-provider.ts
    types/
    config/
```

### 1.5. `services/sandbox-router`

- Ngatur sandbox eksekusi (Docker/K8s/gVisor).

```text
services/sandbox-router/
  src/
    index.ts
    api/
      sandbox-controller.ts
    drivers/
      k8s-driver.ts
      docker-driver.ts
    models/
      sandbox-state.ts
    config/
```

### 1.6. `services/tools-hub`

- Kumpulan tools HTTP yang bisa dipanggil LLM (Gmail, GitHub, Slack, HTTP generic, dsb.).

```text
services/tools-hub/
  src/
    index.ts
    api/
      gmail-controller.ts
      github-controller.ts
      http-request-controller.ts
    clients/
      gmail-client.ts
      github-client.ts
    config/
```

### 1.7. `packages/shared-*`

- `shared-types/` – definisi TypeScript types (Job, Task, ToolCall, dsb.).
- `shared-db/` – koneksi DB, migrasi.
- `shared-logging/` – wrapper logger.
- `shared-config/` – load ENV, dsb.

---

## 2. Pseudo Code – Orchestrator

### 2.1. Job creation (API Gateway → Orchestrator)

`services/api-gateway/src/routes/jobs.ts`

```ts
// create job
router.post("/v1/jobs", async (req, res) => {
  const userId = req.auth.userId;
  const { goal, title, config } = req.body;

  const job = await orchestratorClient.createJob({
    userId,
    goal,
    title,
    config,
  });

  res.json({ job_id: job.id, status: job.status });
});
```

`services/orchestrator/src/controllers/jobs-controller.ts`

```ts
export async function createJob(input: {
  userId: string;
  goal: string;
  title?: string;
  config?: any;
}): Promise<Job> {
  const job = await jobsRepo.insert({
    userId: input.userId,
    goal: input.goal,
    title: input.title ?? deriveTitleFromGoal(input.goal),
    status: "pending",
    config: input.config ?? {},
  });

  // trigger planning asynchronously (planner agent)
  schedulePlanningTask(job.id);

  return job;
}
```

### 2.2. Planning (Planner Agent sebagai task khusus)

`services/orchestrator/src/scheduler/scheduler-loop.ts`

```ts
async function schedulerLoop() {
  while (true) {
    const pendingTasks = await tasksRepo.findRunnableTasks();
    for (const task of pendingTasks) {
      await assignTaskToWorker(task);
    }
    await sleep(1000);
  }
}
```

`services/orchestrator/src/dag/planner-client.ts`

```ts
export async function runPlanner(job: Job): Promise<void> {
  // Planner bisa berjalan sebagai task pertama,
  // atau sebagai panggilan khusus ke agent-worker dengan role "planner".
  const plannerTask = await tasksRepo.insert({
    jobId: job.id,
    type: "planning",
    description: "Initial planning for job",
    status: "pending",
  });

  // schedulerLoop akan menjemput plannerTask dan worker akan menjalankan Planner Agent.
}
```

Worker ketika menerima `type = planning` akan pakai prompt sistem Planner, memanggil model router, dan menghasilkan DAG.

`services/agent-worker/src/worker-loop.ts`

```ts
async function workerLoop() {
  while (true) {
    const taskMessage = await queueClient.consumeTask();  // blocking
    if (!taskMessage) continue;
    await handleTask(taskMessage);
  }
}
```

`services/agent-worker/src/worker-loop.ts`

```ts
async function handleTask(msg: TaskMessage) {
  const { task_id, job_id, task_type } = msg;

  try {
    if (task_type === "planning") {
      await runPlannerAgent(msg);
    } else {
      await runGenericAgent(msg);
    }
    await orchestratorClient.markTaskCompleted(task_id, /* outputs */);
  } catch (err) {
    await orchestratorClient.markTaskFailed(task_id, formatError(err));
  }
}
```

`services/agent-worker/src/agent-runtime/runtime.ts` (Planner)

```ts
export async function runPlannerAgent(msg: TaskMessage) {
  const job = await orchestratorClient.getJob(msg.job_id);
  const userMemory = await memoryClient.getUserMemory(job.user_id);

  const systemPrompt = loadPlannerSystemPrompt();
  const userPrompt = JSON.stringify({
    goal: job.goal,
    config: job.config,
    user_memory: userMemory,
  });

  const llmResp = await modelRouterClient.generate({
    role: "planning",
    model_hint: "core_reasoning",
    messages: [
      { role: "system", content: systemPrompt },
      { role: "user", content: userPrompt },
    ],
    tools: [], // planner biasanya tidak butuh tool
  });

  const planJson = parseJson(llmResp.choices.content);
  // planJson.tasks: array { id, type, description, depends_on, tool_hints }

  await dagOps.applyPlan(msg.job_id, planJson.tasks);
}
```

`dagOps.applyPlan` akan menulis ke tabel `tasks` + `task_dependencies`.

---

## 3. Pseudo Code – Agent Runtime Generic

`services/agent-worker/src/agent-runtime/runtime.ts`

```ts
export async function runGenericAgent(msg: TaskMessage) {
  const state: AgentState = await buildInitialState(msg);

  for (let step = 0; step < state.config.maxSteps; step++) {
    const prompt = buildPrompt(state);

    const llmResp = await modelRouterClient.generate({
      role: inferRoleFromTaskType(state.task.type),
      model_hint: modelHintFromTaskType(state.task.type),
      messages: state.history,
      tools: getToolsForTaskType(state.task.type),
    });

    const choice = llmResp.choices;

    if (choice.type === "tool_call") {
      const toolResult = await toolsDispatcher.execute({
        toolName: choice.tool_name,
        args: choice.arguments,
        state,
      });

      state.history.push({
        role: "assistant",
        content: JSON.stringify(choice),
      });
      state.history.push({
        role: "tool",
        name: choice.tool_name,
        content: JSON.stringify(toolResult),
      });

      state = updateStateFromToolResult(state, toolResult);
      continue;
    }

    // assistant final answer
    state.history.push({
      role: "assistant",
      content: choice.content,
    });

    state.done = true;
    state.finalOutput = choice.content;
    break;
  }

  await persistOutputs(state);
}
```

`buildPrompt`, `getToolsForTaskType`, `updateStateFromToolResult`, `persistOutputs` bisa dipecah per agent role (research, doc, code).

---

## 4. Pseudo Code – Model Router

`services/model-router/src/router/router.ts`

```ts
export async function generate(req: GenerateRequest): Promise<LLMResponse> {
  const modelConfig = pickModel(req);

  const provider = getProvider(modelConfig.provider);

  const providerReq = mapToProviderFormat(req, modelConfig);

  const resp = await provider.generate(providerReq);

  await usageRepo.recordUsage({
    jobId: req.meta?.jobId,
    taskId: req.meta?.taskId,
    modelId: modelConfig.id,
    promptTokens: resp.usage.prompt_tokens,
    completionTokens: resp.usage.completion_tokens,
    costUsd: estimateCost(modelConfig, resp.usage),
  });

  return mapFromProviderFormat(resp);
}
```

`services/model-router/src/router/policies.ts`

```ts
export function pickModel(req: GenerateRequest): ModelConfig {
  const role = req.role;
  const ctx = estimateContextTokens(req.messages);

  // contoh rule simple
  if (role === "planning" || role === "controller") {
    return models.core_reasoning;
  }

  if (role === "research") {
    return models.research;
  }

  if (role === "doc_generation") {
    if (ctx > 64000) return models.long_context;
    return models.research; // misalnya
  }

  if (role === "code") {
    return models.code;
  }

  // default
  return models.core_reasoning;
}
```

---

## 5. Pseudo Code – Sandbox Router

`services/sandbox-router/src/api/sandbox-controller.ts`

```ts
// Create sandbox
router.post("/v1/sandbox", async (req, res) => {
  const { template, job_id } = req.body;

  const sandboxId = await sandboxService.createSandbox({
    template,
    jobId: job_id,
  });

  res.json({ sandbox_id: sandboxId });
});

// Run command
router.post("/v1/sandbox/:sandboxId/run", async (req, res) => {
  const { sandboxId } = req.params;
  const { command, files, timeout_seconds } = req.body;

  const result = await sandboxService.runCommand({
    sandboxId,
    command,
    files,
    timeoutSeconds: timeout_seconds,
  });

  res.json(result);
});
```

`services/sandbox-router/src/drivers/k8s-driver.ts`

```ts
export async function createSandboxPod(input: {
  template: string;
  jobId: string;
}): Promise<string> {
  // create pod with labels: jobId, template
  // mount ephemeral volume
  // return sandboxId = podName
}

export async function execInSandbox(input: {
  sandboxId: string;
  command: string[];
  files?: Record<string, string>;
  timeoutSeconds?: number;
}): Promise<RunResult> {
  // 1) copy files into pod volume
  // 2) kubectl exec / API exec
  // 3) capture stdout/stderr/exit
  // 4) list generated files (if needed)
}
```

---

## 6. Pseudo Code – Tools Dispatcher di Agent Worker

`services/agent-worker/src/agent-runtime/tools-dispatcher.ts`

```ts
export async function execute(input: {
  toolName: string;
  args: any;
  state: AgentState;
}): Promise<any> {
  switch (input.toolName) {
    case "web_search":
      return toolsHubClient.webSearch(input.args);

    case "http_request":
      return toolsHubClient.httpRequest(input.args);

    case "gmail_send":
      return toolsHubClient.gmailSend({
        ...input.args,
        userId: input.state.job.userId,
      });

    case "run_python_code":
      return runPythonTool(input);

    default:
      throw new Error(`Unknown tool: ${input.toolName}`);
  }
}

async function runPythonTool(input: {
  args: { code: string };
  state: AgentState;
}) {
  const sb = await ensureSandboxForJob(input.state.job.id, "python-runtime");
  const result = await sandboxRouterClient.run({
    sandboxId: sb.id,
    command: ["python", "main.py"],
    files: { "main.py": input.args.code },
    timeout_seconds: 120,
  });

  // simpan sebagai artefak log
  await artefactsRepo.insert({
    jobId: input.state.job.id,
    taskId: input.state.task.id,
    type: "log",
    name: "python_run_log",
    storagePath: await saveToObjectStorage(result.stdout),
  });

  return result;
}
```

---

## 7. Pseudo Code – Integrasi dengan Artefact Store

`services/agent-worker/src/agent-runtime/state.ts`

```ts
export async function persistOutputs(state: AgentState) {
  const { job, task } = state;

  // contoh: final output ditulis sebagai summary artefact
  const path = await objectStorage.saveText(
    `jobs/${job.id}/tasks/${task.id}/output.md`,
    state.finalOutput ?? ""
  );

  await artefactsRepo.insert({
    jobId: job.id,
    taskId: task.id,
    type: "summary",
    name: `${task.type}_output`,
    storagePath: path,
    mimeType: "text/markdown",
  });
}
```

---

## 8. Contoh Flow Implementasi Nyata

Untuk memulai coding:

1. Implement `shared-types` (TypeScript interface untuk Job, Task, TaskMessage, GenerateRequest, dsb.).
2. Implement `services/orchestrator`:
   - koneksi DB + migrasi,
   - createJob, getJob, markTaskCompleted/Failed,
   - schedulerLoop yang kirim task ke queue.
3. Implement `services/agent-worker`:
   - subscribe queue,
   - `handleTask`, `runPlannerAgent`, `runGenericAgent`.
4. Implement `services/model-router`:
   - satu provider dulu (OpenAI/GPT),
   - `generate` + simple `pickModel`.
5. Implement `services/sandbox-router` minimal:
   - untuk awal bisa local Docker exec, lalu upgrade ke K8s.
6. Implement `services/tools-hub`:
   - mulai dengan `http_request` dan `web_search` (bisa pakai headless browser simple atau API search eksternal).

Setelah MVP jalan, kamu bisa iteratif tambah:

- multi‑model,
- sub‑agents dinamis,
- UI grafis DAG, dll.



### Appendix B – Perplexity Computer v2 (Parts 1‑10)


#### PerplexityComputerV2.part1.md

# Sasaran arsitektur

Bangun OpenClaw sebagai **sistem 4-layer**:

## 1. Gateway / control plane

Gunakan OpenClaw Gateway sebagai proses always-on yang menjadi pusat semua hal berikut:

* routing model
* session orchestration
* policy enforcement
* hooks lifecycle
* auth
* HTTP APIs
* WebSocket control / RPC
* tool registry
* approval boundary
* task checkpointing
* cron / heartbeat coordination

Gateway bukan sekadar “entry point”, tetapi **otak operasional**. Semua keputusan lintas task harus lewat sini: model mana yang dipakai, tool mana yang diizinkan, apakah sebuah action perlu approval, dan bagaimana recovery dilakukan saat task gagal atau interrupted.

Secara produk, Gateway juga harus menjadi titik integrasi untuk:

* dashboard observability
* budget control
* audit log
* workspace routing
* artifact indexing
* skill dispatch
* connector access policy

Artinya, Gateway harus dipikirkan seperti gabungan dari:

* API gateway
* orchestration layer
* policy engine
* telemetry switchboard

Kalau layer ini lemah, semua kecanggihan model dan tools akan terasa tidak konsisten.

## 2. Execution plane

Execution plane adalah runtime yang benar-benar melakukan pekerjaan. Bukan reasoning, tapi **action**.

Di sini OpenClaw seharusnya memanfaatkan penuh:

* `exec`
* `apply_patch`
* browser relay
* sub-agents
* hooks
* heartbeat
* cron
* background tasks
* artifact persistence

Execution plane harus diperlakukan seperti **distributed worker system**.
Bukan “LLM yang kadang bisa menjalankan command”, tetapi worker runtime dengan state, retries, isolation, specialization, dan recovery.

Pisahkan execution ke beberapa kelas node:

* **planner node**: reasoning berat, minim tool execution
* **sandbox-fast**: lint, unit test, repo indexing, quick grep
* **sandbox-build**: compile, install, build verification
* **sandbox-integration**: docker, service startup, integration tests
* **browser node**: CI dashboard, admin console, docs behind login
* **connector worker**: GitHub, Jira, Slack, Sentry, CI actions
* **memory worker**: summarization, checkpoint compaction, repo memory updates

Dengan pemisahan ini, OpenClaw berhenti menjadi “satu loop agentik” dan mulai menjadi **multi-runtime engineering system**.

## 3. Reasoning plane

Reasoning plane tidak boleh memakai satu model untuk semua pekerjaan. Itu sumber biaya bocor, latensi tinggi, dan kualitas inkonsisten.

Minimal harus ada pemisahan peran:

* **Planner**
  menyusun rencana, memecah masalah, menentukan budget, mendeteksi dependency, memilih urutan tools, menentukan kapan spawn sub-agent

* **Coder**
  menulis patch, merevisi implementasi, membuat test, melakukan refactor, menyusun perubahan file

* **Verifier**
  membaca log, mengklasifikasi error, mengekstrak failure signal, menyusun failure bundle yang ringkas dan actionable

* **Reviewer**
  menilai patch, mendeteksi regression risk, style issues, security concerns, dependency hazards

* **Retriever**
  memetakan repo, mencari file kandidat, memperluas symbol scope, mencari docs dan ownership map

* **UI / Browser agent**
  menjalankan workflow web yang butuh stabilitas aksi, verifikasi visual, dan state awareness

Struktur ini penting karena masalah engineering jarang gagal di “IQ mentah”. Biasanya gagal di:

* pemilihan konteks yang salah
* patch yang terlalu luas
* loop test yang boros
* log parsing yang buruk
* retry yang membabi buta
* action browser yang tidak state-aware

## 4. Evaluation plane

Ini bagian yang paling sering diabaikan. Tanpa evaluation plane, semua peningkatan terasa subjektif.

Semua perubahan harus diikat ke:

* benchmark offline
* benchmark workflow
* replay production tasks
* task outcome telemetry
* regression gate

Tanpa ini, tim akan selalu berkata “rasanya lebih bagus,” padahal success rate mungkin stagnan atau cost justru naik.

---

# 1) Ubah target produk: menangkan lane coding, jangan generalist dulu

Perplexity Computer kuat karena breadth: multi-step workflows, background execution, subagents, skills, web + files + connected apps, serta kemampuan bertindak lewat connectors dan MCP-like integrations.

Tetapi breadth bukan jalur tercepat untuk menang. Jalur tercepat adalah **kedalaman**.

OpenClaw harus diposisikan sebagai:

> “Agent engineering yang bisa membaca repo, memetakan masalah, membuat patch, menjalankan test yang relevan, pulih dari kegagalan, dan menghasilkan artifact engineering yang bisa langsung dipakai.”

Fokus lane yang harus dikuasai dulu:

* Issue → patch → test → PR draft
* Bug triage dari error/log/stack trace
* Test repair
* Dependency upgrade dengan risk analysis
* Refactor scoped dan terverifikasi
* CI failure diagnosis
* Incident hotfix preparation
* Review-ready code change

Ini lebih defensible daripada general digital worker, karena membutuhkan kombinasi:

* repo reasoning
* patch safety
* verification loop
* connector depth
* auditability
* approval control

Semua itu area yang bisa benar-benar dibedakan.

## KPI produk yang wajib dipasang

Jangan ukur “wow factor”. Ukur hal-hal ini:

* **Issue-to-PR success rate**
* **pass@1 untuk repo tasks**
* **median time-to-first-working-patch**
* **median wall-clock to green CI**
* **% patch lolos review tanpa edit manusia**
* **cost per resolved task**
* **resume-success after interruption/failure**
* **tool-call failure rate**
* **browser task success rate**
* **context-loss rate pada task > 30 menit**
* **human intervention count per task**
* **rollback count**
* **regression rate setelah merge**
* **mean retries per resolved task**
* **approval frequency per action class**

Kalau metrik ini naik, produknya benar-benar membaik. Kalau tidak, berarti hanya UI atau prompting yang berubah.

---

# 2) Routing model per peran, bukan satu model besar

OpenClaw harus punya **internal router** di atas Gateway.

## Routing minimum

### Planner

Dipakai untuk:

* dekomposisi task
* dependency analysis
* execution plan
* failure recovery planning
* deciding when to ask vs auto-act

Prioritas:

* reasoning depth
* long context
* budget awareness
* conservative planning

### Coder-fast

Dipakai untuk:

* patch kecil
* 1–3 file edits
* unit test fixes sederhana
* known pattern transformations

Prioritas:

* latency rendah
* code edit reliability
* biaya murah

### Coder-strong

Dipakai untuk:

* multi-file patch
* ambiguous architecture changes
* refactor
* API migrations
* deeper test-aware changes

Prioritas:

* reasoning lebih kuat
* better long-range coherence

### Verifier

Dipakai untuk:

* triage logs
* summarize failures
* classify failure bundles
* identify most likely root cause
* suggest next test scope

Prioritas:

* cepat
* murah
* structured output discipline

### Reviewer

Dipakai untuk:

* security scan
* regression check
* style consistency
* hidden edge cases
* review note generation

Prioritas:

* conservative judgment
* high precision over creativity

### Retriever

Dipakai untuk:

* file shortlist
* repo map
* symbol expansion
* dependency search
* docs lookup
* ownership hints

Prioritas:

* murah
* cepat
* high recall

### UI-agent

Dipakai untuk:

* CI dashboard
* console exploration
* web admin actions
* docs behind auth
* browser-native flows

Prioritas:

* stable tool use
* state awareness
* retry discipline

## Contoh hard routing rules

* `task.class = planning` → Planner
* `task.class = patch && files_changed <= 3` → Coder-fast
* `task.class = patch && files_changed > 3` → Planner + Coder-strong
* `task.class = repo_exploration` → Retriever
* `task.class = test_fail_triage` → Verifier
* `task.class = review/security/regression` → Reviewer
* `task.class = browser_console` → UI-agent
* `task.class = dependency_upgrade` → Planner + Retriever + Coder-strong + Reviewer

## Dynamic routing rules

Tambahkan rule berbasis sinyal runtime:

* jika repo > N files dan owner map belum ada → panggil Retriever dulu
* jika build failed dan log > X lines → Verifier dulu, bukan Coder
* jika patch kedua gagal di area sama → escalate ke Planner
* jika touched files mengandung critical paths → wajib Reviewer
* jika lockfile berubah → approval gate naik
* jika model confidence turun setelah 2 retries → switch strategy, bukan ulang prompt

Tujuan router bukan sekadar memilih model termurah. Tujuannya adalah **mengurangi salah langkah**.

---

# 3) Bangun coding harness yang ketat

`exec` dan `apply_patch` adalah dasar yang bagus, tetapi tidak cukup. Yang membedakan agent engineering yang demo-friendly dengan yang production-capable adalah **harness**.

## A. Repo indexer

Bangun service repo intelligence terpisah. Jangan paksa model mengerti repo dari file tree mentah.

Indexer minimal harus menghasilkan:

* tree map
* symbol index
* import graph
* call graph ringan
* test ownership map
* file embeddings
* commit history summary
* blame hot spots
* architectural clusters
* risky module markers
* config / entrypoint detection
* build/test command candidates

Expose sebagai tools:

* `repo.search_symbols`
* `repo.find_references`
* `repo.rank_files_for_task`
* `repo.related_tests`
* `repo.semantic_search`
* `repo.dependency_impact`
* `repo.find_entrypoints`
* `repo.get_module_summary`
* `repo.find_recent_changes`
* `repo.find_high_risk_files`

Tujuannya supaya Planner dan Coder tidak perlu membaca 80 file hanya untuk mulai kerja.

## B. Patch engine ganda

Jangan hanya mengandalkan satu mode edit. Harus ada **edit strategy stack**.

### Mode 1: small_edit

Untuk perubahan kecil:

* rename lokal
* fix logic satu fungsi
* add test kecil
* edit config minor

### Mode 2: structured_patch

Untuk perubahan yang lebih eksplisit:

* hunks terstruktur
* multi-file deterministic edits
* patch reasoning trace
* safer rollback

### Mode 3: branch_patch

Untuk perubahan besar:

* buat temp branch
* edit via shell / scripts / codemod
* jalankan formatter
* stage hunks selektif
* hasilkan diff bundle
* simpan evidence

## Fallback chain

1. inline edit
2. structured patch
3. branch_patch via shell/git
4. codemod/scripted transform
5. split patch into smaller scoped edits

Jangan biarkan agent gagal total hanya karena satu mekanisme edit tidak cocok.

## C. Test loop engine

Banyak agent gagal karena urutan verifikasinya buruk. Mereka menjalankan semua test terlalu cepat, terlalu luas, atau terlalu lambat.

Gunakan orkestrator test seperti ini:

1. jalankan syntax / parse / type / lint check yang paling murah
2. jalankan minimal relevant tests
3. jalankan package-level tests
4. jalankan broader impact subset
5. jika lolos, siapkan full CI handoff
6. parse semua output jadi failure bundle
7. kirim hanya bundle ringkas ke Verifier

Klasifikasi kegagalan minimal:

* syntax
* import/type
* missing dependency
* env/config
* flaky
* assertion mismatch
* integration failure
* timeout
* resource exhaustion
* performance regression

Setelah klasifikasi, putuskan next action:

* patch fix
* retry
* quarantine flaky
* ask approval
* escalate ke human
* collect more context

## D. Sandboxed execution tiers

Jangan jalankan semua hal di satu lingkungan.

Pisahkan:

* **sandbox-fast** → lint, grep, unit tests
* **sandbox-build** → compile, packaging, lockfile effects
* **sandbox-integration** → services, containers, e2e dependencies
* **sandbox-browser** → UI flows, dashboard inspection

Kelebihannya:

* retry lebih cerdas
* cache lebih baik
* cost lebih jelas
* security boundary lebih kuat
* debugging lebih mudah

---

# 4) Gunakan sub-agents sebagai pipeline paralel, bukan gimmick

Sub-agents hanya berguna jika punya **topologi yang jelas**. Kalau tidak, mereka hanya menambah token dan noise.

## Topologi default untuk task coding besar

Spawn hingga 4 agent:

### Scout agent

Tugas:

* peta repo
* shortlist file
* cari owner/risk
* cari test terkait
* identifikasi dependency impact

Output:

* file shortlist
* repo map ringkas
* dependency risk
* suggested plan seeds

### Implementer agent

Tugas:

* menghasilkan patch awal
* membuat test yang perlu
* menyiapkan diff summary

Output:

* patch candidate
* changed files summary
* implementation notes
* unresolved doubts

### Test agent

Tugas:

* menjalankan test loop
* mem-parse build failures
* menyiapkan failure bundle

Output:

* pass/fail status
* relevant logs
* failure classification
* rerun recommendations

### Review agent

Tugas:

* regression review
* security review
* style and consistency scan

Output:

* review notes
* severity-tagged concerns
* accept / revise recommendation

### Doc agent

Opsional untuk task yang siap dipublish:

* changelog
* migration notes
* PR body
* release note snippet

## Kebijakan sub-agent

Setiap sub-agent harus punya:

* objective
* file scope
* tool scope
* budget
* timeout
* stop condition
* output schema

Parent agent **tidak boleh** menelan transcript mentah semua sub-agent.
Parent hanya menerima:

* summary
* artifacts
* confidence
* blockers
* recommended next step

Tanpa itu, context window akan dipenuhi transcript internal yang tidak berguna.

## Kapan spawn sub-agents

Gunakan rule seperti:

* repo task > 3 file → scout + implementer
* task menyentuh browser/admin → tambah UI-agent
* task menyentuh dependency / security → tambah reviewer
* build/test duration lama → dedicated test agent
* task > 20 menit → checkpoint + split work by sub-agent

Tujuan paralelisme bukan sekadar cepat, tetapi **mengurangi serial blind spots**.

---

# 5) Tambahkan memory hierarchy yang benar

Memory mentah berupa transcript panjang itu tidak berguna. Yang dibutuhkan agent adalah **memory yang terstruktur, ringkas, dan task-relevant**.

## A. Session memory

Untuk satu task:

* objective
* assumptions
* constraints
* chosen plan
* file shortlist
* attempted fixes
* failed attempts
* current blockers
* pending approvals
* next best action

## B. Repo memory

Persisten per repo:

* build commands
* test commands
* lint/fmt commands
* architecture summary
* module ownership
* common failure patterns
* dangerous modules
* critical files
* CI peculiarities
* known flaky tests
* dependency conventions

## C. User / team memory

Persisten per team/workspace:

* coding style preference
* review preference
* risk tolerance
* deployment constraints
* security expectations
* PR format preference
* acceptable automation boundaries

## D. Operational memory

Persisten untuk runtime:

* flaky environments
* slow commands
* provider incidents
* repeated tool failures
* model-specific failure signatures
* browser fragility patterns
* approval bottlenecks

## Prinsip memory

* simpan **abstraction**, bukan transcript
* update dengan hooks
* compact berkala
* beri TTL untuk data operasional
* tandai confidence dan freshness
* pisahkan memory manusia, repo, dan runtime

Kalau memory tidak dipisah seperti ini, agent akan:

* mengulang kesalahan
* lupa build commands
* salah mengira failure lama sebagai failure baru
* menambah token cost dari task ke task

---

# 6) Bangun skills domain-specific yang benar-benar executable

“Skill” tidak boleh berhenti di prompt template. Skill harus menjadi **workflow contract**.

## Skills minimum

* Issue-to-PR
* Bug triage
* Refactor
* Test repair
* Dependency upgrade
* Migration
* Security fix
* Release prep
* Code review
* Incident hotfix
* CI red build recovery
* Backport / cherry-pick assistant

## Isi wajib setiap skill

* input schema
* preflight checks
* required context
* tool whitelist
* execution graph
* approval rules
* rollback behavior
* required artifacts
* success criteria
* escalation rules
* confidence thresholds
* fallback paths
* checkpoint map

## Contoh: Issue-to-PR skill

1. read issue
2. map repo
3. shortlist files
4. propose execution plan
5. implement patch
6. run targeted tests
7. run broader checks
8. review/regression scan
9. generate PR body
10. request approval jika confidence rendah / sensitive files berubah

## Contoh: Test repair skill

1. ingest failure log
2. classify failure
3. locate impacted tests/files
4. determine apakah source bug atau test brittleness
5. patch test atau code
6. rerun focused test
7. rerun package subset
8. produce repair notes

## Skill maturity levels

Tambahkan level:

* **L1**: assisted execution
* **L2**: auto-run with approval gates
* **L3**: fully autonomous in narrow lane

Dengan ini, tim tahu skill mana yang siap dipakai luas dan mana yang masih experimental.

---

# 7) Bangun connector layer yang lebih dalam untuk workflow engineering

Kalau mau unggul di developer workflow, konektor harus bisa **bertindak**, bukan hanya membaca.

## Tier 1 connectors

* GitHub / GitLab
* Jira / Linear
* Slack / Discord
* CI/CD provider
* Sentry / Datadog / Grafana
* package registries
* artifact storage
* cloud logs

## Tier 2 connectors

* Notion / Confluence
* Google Drive / Docs
* Postgres / warehouse read-only
* feature flag systems
* secrets managers
* incident tooling
* release tooling

## Aksi wajib per connector

### GitHub / GitLab

* create/update PR
* comment on PR
* request reviewers
* read checks
* fetch artifacts
* rebase/cherry-pick hints
* create draft PR
* label issue/PR

### Jira / Linear

* create ticket
* update status
* comment summary
* attach incident evidence
* link PR/commit

### CI/CD

* rerun failed jobs
* fetch logs
* fetch junit/xml artifacts
* detect flaky tests
* correlate failure across runs

### Slack / Discord

* post status summary
* ask for approval
* send failure digest
* notify checkpoint reached

### Sentry / Datadog

* read incident traces
* correlate deploy window
* extract top error signatures
* identify likely module/regression

Kalau konektor hanya search/read, agent tidak akan terasa jauh berbeda dari assistant biasa.

---

# 8) Bangun browser plane untuk devtools dan admin tasks

Browser tool harus diperlakukan sebagai **first-class execution environment**, bukan fallback.

OpenClaw perlu memakainya untuk:

* CI dashboards
* internal consoles
* admin panels
* package registries
* docs behind login
* release dashboards
* observability panels

## Upgrade yang perlu ditambahkan

### DOM-to-goal planner

Agent harus bisa menerjemahkan goal seperti “rerun failed job dan ambil artifact” menjadi:

* cari pipeline latest
* filter failed jobs
* buka detail job
* rerun
* download artifact
* summarize results

### Action retry taxonomy

Tidak semua browser failure sama. Minimal klasifikasi:

* stale ref
* modal obstruction
* page not loaded
* auth redirect
* hidden element
* optimistic click failure
* async render race
* navigation mismatch

### Visual confirmation model

Setelah aksi penting, agent harus memverifikasi:

* apakah tombol berubah state
* apakah toast sukses muncul
* apakah URL/state berubah
* apakah artifact tersedia
* apakah console errors meningkat

### Page state fingerprinting

Simpan fingerprint state page supaya agent tahu:

* ini halaman yang sama atau tidak
* apakah DOM besar berubah
* apakah perlu re-snapshot
* apakah refs lama invalid

### Browser artifact bundle

Setiap browser workflow harus bisa menghasilkan:

* screenshot
* DOM snapshot
* network summary
* console errors
* page title/url history
* trace id
* action timeline

## Rule browser yang wajib

Selalu:

1. detect current state
2. act
3. verify
4. retry / rollback / escalate

Jangan pernah klik secara buta.

---

# 9) Reliability engineering: pembeda demo vs produk

Ini layer yang paling menentukan apakah sistem bisa dipakai berulang.

## A. Result schema seragam untuk semua tools

Semua tool harus return:

* `status`
* `duration_ms`
* `artifact_refs`
* `structured_output`
* `retryable`
* `error_class`
* `confidence`
* `side_effects`
* `next_recommended_action`

Dengan skema seragam, Planner bisa bernalar lintas tool tanpa parsing ad hoc.

## B. Error taxonomy

Minimal:

* model_refusal
* rate_limit
* context_overflow
* tool_timeout
* permission_denied
* flaky_env
* stale_state
* invalid_patch
* invalid_plan
* ambiguous_goal
* external_service_down
* auth_expired
* artifact_missing
* dependency_resolution_failed
* non_deterministic_test
* policy_blocked

## C. Retry policy

* **model errors** → retry dengan fallback model/provider
* **tool timeout** → retry same tool sekali, lalu alternate path
* **invalid patch** → ganti editing mode
* **stale browser state** → re-snapshot + rebind refs
* **flaky tests** → quarantine evidence + rerun strategy
* **auth expired** → reauth flow / explicit ask
* **context overflow** → summarize and trim working set

## D. Checkpointing

Checkpoint di:

* task accepted
* plan approved
* repo mapped
* patch generated
* tests started
* tests passed
* review completed
* PR artifacts ready
* publish action completed

Setiap checkpoint simpan:

* current state
* artifacts
* next step
* resume instruction
* open approvals

Tujuan checkpointing adalah resumability, auditability, dan partial reuse.

---

# 10) Approval policy: kurangi babysitting, jangan hilangkan kontrol

Autonomy tanpa policy akan menakutkan. Policy tanpa autonomy akan menyebalkan.

Buat **policy matrix** yang jelas.

| Action                           | Default policy                 |
| -------------------------------- | ------------------------------ |
| read files / search / repo index | auto                           |
| local edits in workspace         | auto                           |
| run lint/unit tests              | auto                           |
| run broader test subsets         | auto / threshold-based         |
| install packages                 | ask                            |
| modify lockfiles                 | ask                            |
| network egress                   | ask / allowlist                |
| secrets access                   | explicit ask                   |
| create branch                    | auto                           |
| git push / create PR             | ask                            |
| production/admin write action    | explicit ask                   |
| issue tracker update             | ask / policy-based             |
| rerun CI                         | auto / ask based on org policy |

## Confidence gates

Tambahkan confidence score per langkah:

* `< 0.7` → ask
* security-sensitive → ask regardless
* critical files / migrations / lockfiles → ask
* repetitive known-safe operation → auto

## Repo-critical file policy

Tag file/path tertentu sebagai high-risk:

* auth
* billing
* infra configs
* migrations
* secrets handling
* CI configs
* deployment scripts

Jika tersentuh, reviewer wajib dan approval boundary naik.

---

# 11) UI/UX: kalau experience-nya buruk, engine bagus pun tetap kalah

Pengguna tidak membeli “chain-of-thought.” Mereka membeli **rasa kontrol dan rasa progres**.

## UI minimum yang harus ada

* task board dengan step-by-step status
* live artifacts panel
* cost meter per task
* current model/router decision
* failure reason yang spesifik
* resume button
* rerun from checkpoint
* diff viewer
* test results viewer
* browser trace viewer
* approvals inbox
* “why this action” panel
* confidence indicator per step
* blocker banner
* sub-agent summary view

## Jangan tampilkan

* transcript mentah panjang
* raw chain internal
* seluruh JSON tool outputs
* log tanpa kompresi

## Tampilkan

* plan
* progress
* artifacts
* approvals
* blockers
* outcome
* cost
* next best action

Kesan produk yang harus muncul adalah:

> “Sistem ini tahu sedang berada di mana, sedang melakukan apa, dan saya masih memegang kendali.”

---

# 12) Telemetry dan eval harness: non-negotiable

Semua sistem agent akan terlihat cerdas di demo. Yang membedakan pemenang adalah **berapa sering ia benar**.

## Benchmark suite yang perlu disiapkan

### Coding benchmark internal

Minimal 100 task per kategori:

* small bugfix
* multi-file feature
* refactor
* test repair
* dependency upgrade
* CI failure diagnosis
* config fix
* migration task

### Web/admin benchmark

* login + navigate + change setting
* inspect failed CI + rerun
* download artifact + summarize
* create ticket from failure
* fetch trace/log + correlate

### Long-horizon benchmark

* task > 30 menit
* task dengan 3+ sub-agents
* task resume after interruption
* recurring triage via cron/heartbeat

## Metrik benchmark

* success rate
* first-pass success
* wall-clock time
* model time
* tool time
* cost
* retries
* human interventions
* rollback count
* regression rate
* browser action count
* context reload count
* approval count
* artifact completeness
* PR acceptance rate

## Production telemetry

Log:

* model used
* router decision
* tokens
* latency
* tools used
* retries
* failures by class
* confidence per step
* approvals
* task outcome
* resume count
* checkpoint usage
* action side effects

Tanpa dashboard ini, Anda tidak bisa improve secara sistematis.

---

# 13) Roadmap implementasi 90 hari

## Fase 1 — minggu 1–2

**Tujuan: baseline, observability, kontrol**

Bangun:

* tool instrumentation
* error taxonomy
* checkpoint store
* role-based model routing
* eval harness v1
* repo memory schema

Deliverable:

* metrics dashboard
* baseline success/cost
* router config v1
* task replay harness

## Fase 2 — minggu 3–4

**Tujuan: coding depth**

Bangun:

* repo indexer
* targeted test runner
* patch engine ganda
* issue-to-PR skill
* bug triage skill
* test repair skill

Deliverable:

* 3 executable skills
* file ranking tools
* failure bundle generator

## Fase 3 — minggu 5–6

**Tujuan: parallelism dan recovery**

Bangun:

* sub-agent orchestration default
* parent-child summary protocol
* checkpoint resume
* confidence-based approval policy
* background task supervisor

Deliverable:

* multi-agent orchestration v1
* resumability
* reduced babysitting

## Fase 4 — minggu 7–8

**Tujuan: engineering connectors**

Bangun:

* GitHub/GitLab write actions
* Jira/Linear create/update
* CI artifact ingestion
* Slack summary posting
* Sentry/Datadog read path

Deliverable:

* engineering connector suite v1

## Fase 5 — minggu 9–10

**Tujuan: browser/admin plane**

Bangun:

* browser retry engine
* visual confirmation
* page state fingerprinting
* trace artifact bundle
* CI/admin console workflows

Deliverable:

* browser success dashboard
* admin/web automation pack

## Fase 6 — minggu 11–12

**Tujuan: hardening**

Bangun:

* benchmark regression gate di CI
* provider failover
* prompt/skill versioning
* audit log export
* security review
* worker autoscaling bila perlu

Deliverable:

* release candidate
* weekly eval report
* SLOs

---

# 14) Konfigurasi operasional yang direkomendasikan

## Gateway

* dedicated gateway host
* loopback bind + reverse proxy
* token auth wajib
* hot config reload
* workspace per repo/task
* worker pool terpisah per role

## Workspace policy

* satu task = satu branch + satu workspace snapshot
* artifact store per task:

  * logs
  * diffs
  * screenshots
  * summaries
  * junit/xml/json
* cleanup via cron
* retention policy berbasis task state

## Hooks

Gunakan untuk:

* session memory save
* audit logging
* budget alarms
* failure notifications
* auto-summary post-task
* repo memory updates

## Heartbeat

Gunakan hanya untuk:

* stuck-task detection
* stale PR follow-up
* recurring triage
* scheduled status summaries
* connector health checks

Heartbeat jangan dipakai untuk kerja berat yang seharusnya event-driven.

---

# 15) Hal yang jangan dilakukan

* jangan memakai satu model untuk planner + coder + reviewer sekaligus
* jangan menjalankan full test suite di awal
* jangan kirim seluruh repo ke model
* jangan spawn sub-agents tanpa output schema
* jangan simpan memory sebagai transcript mentah
* jangan mengandalkan satu edit mechanism saja
* jangan membangun “skills” hanya sebagai prompt panjang
* jangan ukur kualitas dari demo task yang dipilih
* jangan membiarkan retry berjalan tanpa error taxonomy
* jangan campur policy, routing, dan execution di satu layer

---

# 16) Definisi “menang” yang realistis

Kemenangan yang realistis bukan “semua orang bilang ini lebih keren”. Kemenangan adalah ketika, untuk lane engineering-heavy, OpenClaw menunjukkan:

* issue-to-PR success rate lebih tinggi
* cost per resolved task lebih rendah
* lebih cepat menuju green CI
* intervensi manusia lebih sedikit
* resumability lebih baik
* repo reasoning lebih dalam
* approval boundaries lebih aman
* engineering connectors lebih berguna
* model routing lebih efisien
* progress dan artifacts lebih transparan

Kalau semua ini naik secara konsisten, maka OpenClaw memang lebih kuat daripada Perplexity Computer di lane yang paling penting untuk developer automation.

---

# Penajaman posisi strategis

Kalau saya ringkas menjadi satu kalimat:

> **OpenClaw tidak perlu menjadi agent paling umum. OpenClaw harus menjadi agent engineering paling dapat diandalkan.**

Dan untuk sampai ke sana, kuncinya bukan satu model yang lebih pintar, tetapi kombinasi dari:

* routing yang tepat,
* harness yang ketat,
* memory yang terstruktur,
* connectors yang bisa bertindak,
* browser plane yang stabil,
* approval policy yang masuk akal,
* dan eval/telemetry yang disiplin.



#### PerplexityComputerV2.part2.md

# Technical Design Doc

## OpenClaw Engineering Agent Platform

### Objective

Membangun OpenClaw menjadi **agent platform untuk software engineering automation** yang unggul pada reliability, coding depth, operational control, dan cost efficiency.

Target utama bukan “general worker”, tetapi **engineering-heavy workflows**:

* issue-to-PR
* bug triage
* test repair
* dependency upgrade
* refactor
* CI failure recovery
* incident hotfix assist
* code review assist

---

# 1. Product goal

## 1.1 Goal utama

OpenClaw harus mampu menyelesaikan task engineering secara end-to-end dengan karakteristik berikut:

* mampu membaca dan memahami repo secara terstruktur
* memilih konteks yang relevan, bukan membanjiri model dengan seluruh codebase
* menghasilkan patch yang valid dan terverifikasi
* menjalankan test secara bertahap dan efisien
* pulih dari kegagalan tool, model, browser, atau environment
* menghasilkan artifact engineering yang siap dipakai manusia
* menjaga approval boundaries yang aman
* menyediakan progres dan evidence yang transparan

## 1.2 Non-goal fase awal

Pada fase awal, OpenClaw **tidak** mengejar:

* general productivity assistant lintas semua domain
* deep office suite automation
* autonomous browsing untuk consumer workflows
* long-form research generalist
* no-approval fully autonomous production actions

---

# 2. System overview

Arsitektur dibagi menjadi 4 plane utama:

## 2.1 Control plane

Mengatur:

* task creation
* routing
* policy enforcement
* approval
* checkpointing
* hooks
* cron/heartbeat
* telemetry
* artifact indexing

## 2.2 Execution plane

Menjalankan:

* shell commands
* patch application
* repo indexing
* tests/builds
* browser actions
* connector actions
* sub-agent tasks

## 2.3 Reasoning plane

Menjalankan reasoning berbasis peran:

* planner
* coder
* verifier
* reviewer
* retriever
* ui-agent

## 2.4 Evaluation plane

Mengukur:

* benchmark performance
* production outcome
* retry/failure patterns
* cost/latency
* regression across versions

---

# 3. Core design principles

## 3.1 Role-specialized reasoning

Satu model tidak digunakan untuk semua hal.

## 3.2 Tool-first execution

LLM tidak dianggap sebagai executor langsung, tetapi sebagai pengarah runtime tools.

## 3.3 Minimal-context reasoning

Model hanya diberi konteks yang relevan dan dikurasi.

## 3.4 Evidence-based progress

Setiap langkah penting harus menghasilkan artifact atau structured evidence.

## 3.5 Checkpoint and resumability

Task harus bisa di-resume dari state yang jelas.

## 3.6 Safety via policy, not paralysis

Default policy harus meminimalkan babysitting tanpa menghilangkan kontrol.

## 3.7 Benchmark-gated improvement

Setiap perubahan sistem harus diuji terhadap benchmark dan telemetry.

---

# 4. Task lifecycle state machine

Berikut state machine utama untuk setiap task.

## 4.1 States

### CREATED

Task dibuat, metadata awal disimpan.

### SCOPING

Task diklasifikasi:

* task type
* repo scope
* sensitivity
* expected duration
* likely tool classes

### PLANNED

Planner menghasilkan plan awal:

* subtasks
* file scope
* test scope
* approval needs
* model/tool budget

### CONTEXT_READY

Retriever / repo indexer selesai menyiapkan context kerja:

* shortlisted files
* repo map
* related tests
* risks

### EXECUTING

Implementer / browser / connector actions berjalan.

### VERIFYING

Tests, lint, build, browser validation, review scan berjalan.

### BLOCKED

Task butuh:

* approval
* missing credentials
* missing environment
* unresolved ambiguity
* external outage recovery

### RETRYING

Retry terencana sedang dilakukan dengan alternate path.

### READY_FOR_REVIEW

Patch/artifact siap ditampilkan ke user atau reviewer.

### COMPLETED

Outcome sukses dicapai.

### FAILED

Task berhenti karena fatal error, policy stop, atau budget exhausted.

### CANCELLED

Task dibatalkan user atau system policy.

## 4.2 State transitions

Contoh alur normal:

`CREATED -> SCOPING -> PLANNED -> CONTEXT_READY -> EXECUTING -> VERIFYING -> READY_FOR_REVIEW -> COMPLETED`

Contoh alur dengan gangguan:

`EXECUTING -> BLOCKED -> EXECUTING`

`VERIFYING -> RETRYING -> EXECUTING`

`VERIFYING -> FAILED`

## 4.3 Checkpoint policy

Checkpoint disimpan saat:

* selesai scoping
* selesai planning
* context ready
* patch generated
* tests started
* tests passed
* review generated
* PR artifacts ready

Checkpoint minimal berisi:

* task_state
* plan_summary
* file_scope
* current_artifacts
* last_successful_step
* next_recommended_action
* pending_approvals
* retry_count
* confidence_snapshot

---

# 5. Router design

## 5.1 Router objective

Memilih:

* reasoning role
* model tier
* tool path
* whether to spawn sub-agents
* approval mode
* budget allocation

## 5.2 Input signals

Router menerima sinyal dari:

* task class
* repo size
* touched file estimate
* critical path detection
* test/build complexity
* browser need
* connector need
* ambiguity score
* confidence score
* prior failure history
* remaining budget
* user policy profile

## 5.3 Output schema router

```json
{
  "task_class": "issue_to_pr",
  "reasoning_roles": ["planner", "retriever", "coder_strong", "reviewer"],
  "tool_plan": ["repo.rank_files_for_task", "exec", "structured_patch", "test.run_targeted"],
  "subagent_plan": [
    {"role": "scout", "enabled": true},
    {"role": "implementer", "enabled": true},
    {"role": "tester", "enabled": true},
    {"role": "reviewer", "enabled": false}
  ],
  "approval_mode": "standard",
  "budget": {
    "max_model_calls": 18,
    "max_tool_calls": 35,
    "max_wall_clock_minutes": 25
  },
  "confidence_gate": 0.72
}
```

## 5.4 Router rules v1

### Rule group A: task classification

* planning-heavy → planner
* code-edit small scope → coder-fast
* code-edit multi-file → planner + coder-strong
* failure triage → verifier
* review/regression/security → reviewer
* repo search/navigation → retriever
* web console task → ui-agent

### Rule group B: complexity

* file scope <= 3, tests small → no sub-agents
* file scope > 3 → spawn scout
* expected duration > 15 min → checkpoint mandatory
* browser + code in same task → split browser into dedicated ui-agent

### Rule group C: risk

* auth/billing/migration/infra touched → reviewer mandatory
* lockfile changed → ask before finalizing
* production write action → explicit ask

### Rule group D: recovery

* two consecutive patch failures → escalate to planner
* repeated stale browser refs → force resnapshot mode
* repeated build env issues → switch to env diagnosis path
* context overflow risk → summarize/compact before next call

---

# 6. Memory architecture

Memory dibagi menjadi 4 lapis.

## 6.1 Session memory

Scope: per task

### Schema v1

```json
{
  "task_id": "task_123",
  "objective": "Fix failing login tests after auth middleware change",
  "constraints": [
    "Do not modify production credentials flow",
    "Need passing targeted tests before broader run"
  ],
  "assumptions": [
    "Failure introduced in recent middleware refactor"
  ],
  "file_shortlist": [
    "auth/middleware.ts",
    "tests/login.spec.ts",
    "auth/session.ts"
  ],
  "attempted_actions": [
    {
      "step": "patch",
      "summary": "Adjusted cookie propagation in middleware",
      "result": "failed"
    }
  ],
  "open_questions": [
    "Is session mocking stale or auth logic actually broken?"
  ],
  "current_plan": [
    "inspect related tests",
    "patch auth/session.ts",
    "rerun targeted login tests"
  ],
  "pending_approvals": [],
  "confidence": 0.68
}
```

## 6.2 Repo memory

Scope: per repository

### Schema v1

```json
{
  "repo_id": "repo_abc",
  "build_commands": ["pnpm build"],
  "test_commands": {
    "unit": "pnpm test:unit",
    "integration": "pnpm test:integration",
    "e2e": "pnpm test:e2e"
  },
  "lint_commands": ["pnpm lint"],
  "format_commands": ["pnpm format"],
  "critical_paths": [
    "auth/",
    "billing/",
    "infra/"
  ],
  "common_failure_patterns": [
    {
      "pattern": "Playwright timeouts on CI",
      "recommendation": "rerun once before escalating"
    }
  ],
  "known_flaky_tests": [
    "tests/e2e/report-export.spec.ts"
  ],
  "architecture_summary": {
    "frontend": "apps/web",
    "backend": "services/api",
    "shared": "packages/core"
  }
}
```

## 6.3 User/team memory

Scope: workspace/team/user policy

```json
{
  "team_id": "team_1",
  "review_preferences": [
    "Prefer smaller diffs",
    "Add tests for bugfixes"
  ],
  "risk_policies": {
    "lockfile_changes_require_approval": true,
    "migration_files_require_approval": true
  },
  "pr_preferences": {
    "include_test_summary": true,
    "include_risk_section": true
  }
}
```

## 6.4 Operational memory

Scope: runtime/system health

```json
{
  "provider_incidents": [
    {
      "provider": "model_x",
      "issue": "elevated rate limits",
      "severity": "medium"
    }
  ],
  "tool_failure_signatures": [
    {
      "tool": "browser.click",
      "pattern": "stale_ref_after_modal",
      "recommended_recovery": "resnapshot_and_refind"
    }
  ],
  "env_signals": [
    {
      "workspace": "repo_abc",
      "issue": "docker build cache corrupted",
      "last_seen": "2026-04-05T10:31:00Z"
    }
  ]
}
```

## 6.5 Memory update policy

* save on state transitions
* compact on task completion
* weekly summarize repo memory
* TTL for operational incidents
* confidence/freshness mandatory
* transcript raw tidak disimpan sebagai primary memory

---

# 7. Skill contract

Skill bukan prompt panjang, tetapi executable workflow contract.

## 7.1 Skill interface v1

```json
{
  "skill_name": "issue_to_pr",
  "version": "1.0",
  "input_schema": {
    "issue_text": "string",
    "repo_id": "string",
    "branch_mode": "string",
    "constraints": "array"
  },
  "preflight_checks": [
    "repo_available",
    "workspace_initialized",
    "test_commands_detected"
  ],
  "tool_whitelist": [
    "repo.rank_files_for_task",
    "repo.related_tests",
    "exec",
    "patch.inline",
    "patch.structured",
    "test.run_targeted",
    "review.scan"
  ],
  "execution_graph": [
    "read_issue",
    "map_repo",
    "plan_patch",
    "implement",
    "run_targeted_tests",
    "run_broader_checks",
    "review_scan",
    "generate_pr_artifacts"
  ],
  "required_artifacts": [
    "diff",
    "test_summary",
    "risk_summary",
    "pr_body"
  ],
  "success_criteria": [
    "patch_generated",
    "targeted_tests_passed",
    "no_high_severity_review_blocker"
  ],
  "rollback_behavior": "revert_workspace_changes_since_last_checkpoint",
  "escalation_rules": [
    "ask_if_confidence_below_threshold",
    "ask_if_critical_paths_touched"
  ]
}
```

## 7.2 Skill minimum set v1

* issue_to_pr
* bug_triage
* test_repair
* dependency_upgrade
* refactor_scoped
* code_review_assist
* hotfix_assist

## 7.3 Skill maturity levels

* **experimental**
* **beta**
* **trusted**
* **autonomous-narrow**

---

# 8. Sub-agent protocol

## 8.1 Why

Sub-agents dipakai untuk paralelisme dan spesialisasi, bukan untuk menambah narasi internal.

## 8.2 Allowed sub-agent roles

* scout
* implementer
* tester
* reviewer
* doc_writer
* ui_executor

## 8.3 Sub-agent launch schema

```json
{
  "subagent_id": "sa_01",
  "role": "scout",
  "objective": "Identify likely files and tests related to failing login flow",
  "file_scope": ["auth/", "tests/"],
  "tool_scope": ["repo.search_symbols", "repo.related_tests"],
  "budget": {
    "max_model_calls": 4,
    "max_tool_calls": 8,
    "timeout_minutes": 5
  },
  "stop_condition": "file_shortlist_and_test_map_ready"
}
```

## 8.4 Sub-agent return schema

```json
{
  "subagent_id": "sa_01",
  "status": "completed",
  "summary": "Likely issue centers on auth/session.ts and stale cookie expectations in login.spec.ts",
  "artifacts": [
    "artifact://file_shortlist.json",
    "artifact://related_tests.json"
  ],
  "confidence": 0.81,
  "blocking_issues": [],
  "recommended_next_action": "patch_session_cookie_handling"
}
```

## 8.5 Parent aggregation rule

Parent hanya membaca:

* summary
* artifacts
* confidence
* blockers
* next action

Bukan transcript penuh.

---

# 9. Tool abstraction layer

Semua tools harus punya result schema seragam.

## 9.1 Unified tool result schema

```json
{
  "tool_name": "test.run_targeted",
  "status": "failed",
  "duration_ms": 14230,
  "retryable": true,
  "error_class": "assertion_failure",
  "structured_output": {
    "tests_run": 4,
    "tests_failed": 1,
    "failed_tests": [
      "tests/login.spec.ts::login cookie persists"
    ]
  },
  "artifact_refs": [
    "artifact://logs/targeted_tests.log",
    "artifact://reports/junit.xml"
  ],
  "confidence": 0.92,
  "side_effects": [],
  "next_recommended_action": "invoke_verifier_on_failure_bundle"
}
```

## 9.2 Tool categories

* repo intelligence tools
* patch tools
* shell/exec tools
* test/build tools
* browser tools
* connector tools
* memory tools
* review/security tools

---

# 10. Error taxonomy

## 10.1 Model errors

* model_refusal
* model_hallucinated_tool_use
* context_overflow
* low_confidence_output
* invalid_structured_response

## 10.2 Tool errors

* tool_timeout
* permission_denied
* command_failed
* invalid_patch
* artifact_missing
* rate_limit
* malformed_request

## 10.3 Environment errors

* flaky_env
* missing_dependency
* disk_full
* workspace_corrupted
* cache_corrupted
* service_unavailable

## 10.4 Browser errors

* stale_state
* stale_ref
* auth_redirect
* modal_blocked
* hidden_element
* action_not_applied
* page_not_loaded

## 10.5 Task logic errors

* ambiguous_goal
* invalid_plan
* scope_too_broad
* unsafe_action_blocked
* approval_required
* confidence_below_threshold

## 10.6 Recovery mapping

Setiap error class wajib punya:

* retry policy
* alternate path
* escalation rule
* logging severity

---

# 11. Retry engine

## 11.1 Retry principles

Retry bukan “coba lagi prompt yang sama”. Retry harus terarah.

## 11.2 Retry rules

* model refusal → fallback model/provider
* invalid structured output → same model once, stricter schema
* invalid patch → switch patch mode
* stale browser state → resnapshot and refind refs
* flaky tests → rerun with flaky evidence
* command timeout → rerun once with longer timeout or narrower scope
* repeated ambiguity → ask user or freeze risky branch of execution

## 11.3 Retry budget

Per step:

* default max retry: 2
* browser: 3 untuk stale-state family
* model schema failures: 1 same model + 1 fallback
* patch generation: 2 before planner escalation

---

# 12. Approval engine

## 12.1 Approval classes

### Auto

* read/search
* repo indexing
* local workspace edits
* lint
* unit tests
* safe summaries

### Ask

* install package
* modify lockfile
* rerun CI
* create issue comment
* create draft PR

### Explicit ask

* secrets access
* production write action
* admin panel write
* git push
* final PR publish
* migration execution
* infra/deploy changes

## 12.2 Confidence gate

Per step, jika:

* confidence < threshold
* high-risk paths touched
* sensitive connector action
  maka task masuk `BLOCKED_FOR_APPROVAL`.

## 12.3 Approval payload

Approval request harus ringkas:

```json
{
  "action": "create_draft_pr",
  "reason": "Patch is ready and targeted tests passed",
  "risk_summary": [
    "Touches auth/session.ts",
    "No lockfile changes",
    "No migration files changed"
  ],
  "artifacts": [
    "artifact://diff.patch",
    "artifact://test_summary.md"
  ],
  "confidence": 0.79
}
```

---

# 13. Repo intelligence layer

## 13.1 Objective

Mengurangi kebutuhan membaca banyak file mentah.

## 13.2 Services

* tree index
* symbol index
* dependency map
* test ownership map
* semantic file embeddings
* commit recency map
* blame heatmap
* critical path annotations

## 13.3 API examples

* `repo.rank_files_for_task(query, top_k)`
* `repo.find_references(symbol, scope)`
* `repo.related_tests(file_or_symbol)`
* `repo.get_module_summary(path)`
* `repo.dependency_impact(files[])`
* `repo.find_recent_changes(paths[])`

## 13.4 Ranking signals

* lexical similarity
* semantic similarity
* import proximity
* recent commit proximity
* test linkage
* ownership signal
* criticality weighting

---

# 14. Test orchestration engine

## 14.1 Goal

Menjalankan verification dengan urutan biaya-rendah ke biaya-tinggi.

## 14.2 Pipeline

1. syntax/type/lint precheck
2. targeted tests
3. package/module subset
4. broader impact subset
5. optional full CI handoff

## 14.3 Failure bundle schema

```json
{
  "run_id": "run_001",
  "class": "assertion_failure",
  "failing_targets": [
    "tests/login.spec.ts::login cookie persists"
  ],
  "suspected_files": [
    "auth/session.ts"
  ],
  "log_excerpt_refs": [
    "artifact://logs/targeted_tests.log#L120-L155"
  ],
  "recommended_route": "verifier_then_coder"
}
```

## 14.4 Test policy

* jangan jalankan full suite di awal
* targeted tests harus otomatis diprioritaskan
* flaky tests ditandai dan diperlakukan khusus
* logs harus dipotong menjadi failure bundles, bukan dikirim penuh ke model

---

# 15. Browser execution design

## 15.1 Browser workflow phases

* detect
* plan
* act
* verify
* recover

## 15.2 Browser state bundle

Setelah tiap milestone:

* screenshot
* DOM snapshot
* URL/title
* action timeline
* console summary
* network anomalies
* detected state fingerprint

## 15.3 Browser recovery strategies

* stale refs → re-snapshot
* auth redirect → pause + request auth confirmation
* modal obstruction → dismiss/replan
* invisible action target → scroll/refind
* async rendering → wait with condition

---

# 16. Connector action model

## 16.1 Connector classes

* code host
* issue tracker
* CI provider
* chat platform
* observability
* docs/knowledge
* artifact store

## 16.2 Connector action schema

```json
{
  "connector": "github",
  "action": "create_draft_pr",
  "inputs": {
    "title": "Fix login cookie persistence after middleware refactor",
    "body_ref": "artifact://pr_body.md",
    "branch": "task/login-cookie-fix"
  },
  "approval_class": "ask"
}
```

## 16.3 Connector guarantees

* structured response
* side effect tracking
* idempotency key where possible
* retry classification
* artifact link-back

---

# 17. Telemetry model

## 17.1 Task-level telemetry

* task_id
* task_type
* repo_id
* start/end time
* final outcome
* human interventions
* total cost
* total retries
* total tool calls
* checkpoint count

## 17.2 Step-level telemetry

* step_name
* model role
* model/provider
* tokens
* latency
* tool used
* status
* confidence
* error class if any

## 17.3 Artifact telemetry

* artifact type
* generation step
* size
* retention class

## 17.4 Evaluation linkage

Setiap task completion harus bisa dipetakan ke:

* benchmark bucket
* skill version
* router version
* prompt version
* toolchain version

---

# 18. KPI framework

## 18.1 Primary KPIs

* issue-to-PR success rate
* pass@1 on repo tasks
* median time-to-first-working-patch
* median wall-clock to green CI
* human edits before accept
* cost per resolved task

## 18.2 Secondary KPIs

* retry count per task
* resume success rate
* tool failure rate
* browser task success rate
* critical-path approval rate
* context-loss rate
* regression rate after merge

## 18.3 Safety KPIs

* unsafe action blocked rate
* approval bypass incident count
* lockfile change without approval count
* sensitive connector misuse count

---

# 19. Initial implementation plan by component

## 19.1 Must build first

* task state machine
* router v1
* checkpoint store
* repo memory schema
* unified tool result schema
* test orchestration v1
* issue_to_pr skill
* bug_triage skill
* approval engine v1
* telemetry dashboard

## 19.2 Must build second

* repo indexer
* sub-agent protocol
* browser recovery engine
* connector write actions
* review scan engine
* failure bundle compactor

## 19.3 Must harden third

* benchmark gating in CI
* provider failover
* prompt/skill versioning
* operational memory TTL
* audit export
* recovery simulation tests

---

# 20. Release criteria for v1

OpenClaw layak disebut siap untuk engineering lane apabila:

* issue-to-PR skill sukses di benchmark internal pada threshold minimum
* retry engine bekerja untuk error classes utama
* checkpoints dapat di-resume
* approval engine menjaga high-risk actions
* targeted test orchestration stabil
* telemetry lengkap dan dapat dianalisis
* sub-agent protocol tidak membanjiri context
* connector actions memiliki audit trail
* browser tasks dasar mencapai baseline success rate yang memadai

---

# 21. Ringkasan keputusan desain

Keputusan kunci sistem ini adalah:

1. **OpenClaw difokuskan ke engineering automation terlebih dahulu**
2. **LLM dipisah berdasarkan peran, bukan satu model untuk semua**
3. **Repo intelligence menjadi lapisan wajib, bukan opsional**
4. **Patching harus punya beberapa mode dan fallback**
5. **Verification dilakukan bertahap dan efisien**
6. **Sub-agents dipakai sebagai pipeline paralel dengan output schema**
7. **Memory harus terstruktur, bukan transcript mentah**
8. **Connector harus bisa bertindak, bukan cuma membaca**
9. **Browser harus state-aware dan recoverable**
10. **Semua hal diikat ke telemetry dan benchmark**

---



#### PerplexityComputerV2.part3.md

# A. Router Rules Lengkap

## A.1 Tujuan router

Router bertanggung jawab untuk menentukan, untuk setiap task:

1. **task class**
2. **complexity tier**
3. **risk tier**
4. **reasoning roles**
5. **tool plan**
6. **sub-agent plan**
7. **approval mode**
8. **budget**
9. **recovery strategy default**
10. **checkpoint cadence**

Router bukan sekadar “pilih model mana”, tetapi policy engine kecil yang menentukan **bagaimana task akan dikerjakan**.

---

## A.2 Input ke router

Router menerima `TaskIntake` yang minimal berisi:

* goal / task prompt
* repo metadata
* workspace metadata
* user/team policy
* available tools/connectors
* prior memory
* current environment signals
* optional issue/ticket/browser context
* budget constraints

---

## A.3 Task classes v1

Gunakan task classes berikut:

* `planning`
* `repo_exploration`
* `issue_to_pr`
* `bug_triage`
* `test_repair`
* `dependency_upgrade`
* `refactor_scoped`
* `code_review`
* `ci_failure_debug`
* `browser_console_task`
* `incident_hotfix_assist`
* `release_prep`
* `mixed_task`
* `unknown`

---

## A.4 Complexity tiers

* `tiny` → 1 file, no tests/browse likely
* `small` → 2–3 files, targeted tests only
* `medium` → 3–8 files, package-level verification
* `large` → 8+ files or multi-surface changes
* `long_horizon` → likely > 20 min, multiple agents, resumable

---

## A.5 Risk tiers

* `low`
* `moderate`
* `high`
* `critical`

Risk ditentukan dari kombinasi:

* touched paths
* lockfile/package changes
* infra/deploy/migration/auth/billing/security areas
* browser/admin write actions
* production-like connectors
* secrets exposure
* policy profile

---

## A.6 Reasoning roles v1

* `planner`
* `retriever`
* `coder_fast`
* `coder_strong`
* `verifier`
* `reviewer`
* `ui_agent`

---

## A.7 Router pseudocode utama

```python id="xkfdzy"
def route_task(task_intake, repo_memory, team_memory, operational_memory):
    task_class = classify_task(task_intake)
    complexity = estimate_complexity(task_intake, repo_memory)
    risk = estimate_risk(task_intake, repo_memory, team_memory)
    context_pressure = estimate_context_pressure(task_intake, repo_memory)
    needs_browser = detect_browser_need(task_intake)
    needs_connectors = detect_connector_need(task_intake)
    needs_code_changes = detect_code_change_need(task_intake)
    needs_verification = detect_verification_need(task_intake)

    roles = select_roles(
        task_class=task_class,
        complexity=complexity,
        risk=risk,
        needs_browser=needs_browser,
        needs_code_changes=needs_code_changes,
        needs_verification=needs_verification,
    )

    tool_plan = select_tool_plan(
        task_class=task_class,
        complexity=complexity,
        risk=risk,
        needs_browser=needs_browser,
        needs_connectors=needs_connectors,
        repo_memory=repo_memory,
    )

    subagent_plan = decide_subagents(
        task_class=task_class,
        complexity=complexity,
        risk=risk,
        needs_browser=needs_browser,
        context_pressure=context_pressure,
    )

    approval_mode = select_approval_mode(
        risk=risk,
        team_memory=team_memory,
        task_intake=task_intake,
    )

    budget = allocate_budget(
        task_class=task_class,
        complexity=complexity,
        risk=risk,
        needs_browser=needs_browser,
        needs_connectors=needs_connectors,
    )

    checkpoint_policy = decide_checkpoint_policy(
        complexity=complexity,
        risk=risk,
        needs_browser=needs_browser,
        needs_connectors=needs_connectors,
    )

    recovery_policy = select_recovery_policy(
        task_class=task_class,
        risk=risk,
        operational_memory=operational_memory,
    )

    return {
        "task_class": task_class,
        "complexity": complexity,
        "risk_tier": risk,
        "roles": roles,
        "tool_plan": tool_plan,
        "subagent_plan": subagent_plan,
        "approval_mode": approval_mode,
        "budget": budget,
        "checkpoint_policy": checkpoint_policy,
        "recovery_policy": recovery_policy,
    }
```

---

## A.8 Task classification pseudocode

```python id="68duau"
def classify_task(task_intake):
    text = normalize(task_intake.goal)

    if mentions(text, ["plan", "approach", "design", "strategy"]) and not task_intake.requires_execution:
        return "planning"

    if mentions(text, ["find files", "where is", "map repo", "understand codebase"]):
        return "repo_exploration"

    if task_intake.issue_context and mentions(text, ["fix", "implement", "patch", "resolve"]):
        return "issue_to_pr"

    if mentions(text, ["failing test", "fix test", "repair test"]):
        return "test_repair"

    if mentions(text, ["bug", "error", "stack trace", "exception", "why failing"]):
        return "bug_triage"

    if mentions(text, ["upgrade dependency", "bump package", "migrate version"]):
        return "dependency_upgrade"

    if mentions(text, ["refactor", "cleanup", "restructure"]) and not mentions(text, ["upgrade dependency"]):
        return "refactor_scoped"

    if mentions(text, ["review this patch", "review PR", "security review", "regression review"]):
        return "code_review"

    if mentions(text, ["CI failed", "pipeline failed", "green CI", "build failing"]):
        return "ci_failure_debug"

    if task_intake.browser_context or mentions(text, ["dashboard", "admin panel", "console", "web UI"]):
        return "browser_console_task"

    if mentions(text, ["hotfix", "incident", "urgent prod", "sev"]):
        return "incident_hotfix_assist"

    if mentions(text, ["release", "cut release", "release notes", "prepare release"]):
        return "release_prep"

    if task_intake.requires_code and task_intake.requires_browser:
        return "mixed_task"

    return "unknown"
```

---

## A.9 Complexity estimation pseudocode

```python id="ycynhe"
def estimate_complexity(task_intake, repo_memory):
    touched_estimate = estimate_touched_files(task_intake, repo_memory)
    has_browser = detect_browser_need(task_intake)
    has_connectors = detect_connector_need(task_intake)
    expected_test_scope = estimate_test_scope(task_intake, repo_memory)
    expected_duration = estimate_duration_minutes(task_intake, repo_memory)

    if touched_estimate <= 1 and expected_test_scope == "none":
        return "tiny"

    if touched_estimate <= 3 and expected_test_scope in ["none", "targeted"] and not has_browser:
        return "small"

    if touched_estimate <= 8 and expected_test_scope in ["targeted", "package"]:
        return "medium"

    if expected_duration > 20 or has_browser and touched_estimate > 3 or has_connectors and has_browser:
        return "long_horizon"

    return "large"
```

---

## A.10 Risk estimation pseudocode

```python id="nq6w8m"
def estimate_risk(task_intake, repo_memory, team_memory):
    risk_score = 0

    critical_paths = repo_memory.get("critical_paths", [])
    requested_paths = infer_candidate_paths(task_intake)

    for path in requested_paths:
        if path_matches_any(path, critical_paths):
            risk_score += 3

    if task_intake.may_modify_lockfiles:
        risk_score += 2

    if task_intake.requires_secrets:
        risk_score += 4

    if task_intake.requires_prod_write:
        risk_score += 5

    if task_intake.browser_admin_write:
        risk_score += 3

    if task_intake.requires_migration:
        risk_score += 4

    if task_intake.requires_deploy_change:
        risk_score += 4

    if task_intake.issue_priority in ["sev1", "sev2"]:
        risk_score += 2

    if team_memory.get("risk_policies", {}).get("strict_mode", False):
        risk_score += 1

    if risk_score <= 1:
        return "low"
    if risk_score <= 4:
        return "moderate"
    if risk_score <= 8:
        return "high"
    return "critical"
```

---

## A.11 Role selection pseudocode

```python id="rl8ft2"
def select_roles(task_class, complexity, risk, needs_browser, needs_code_changes, needs_verification):
    roles = set()

    if task_class in ["planning", "unknown"]:
        roles.add("planner")

    if task_class in ["repo_exploration"]:
        roles.update(["retriever"])

    if task_class in ["issue_to_pr", "refactor_scoped", "dependency_upgrade"]:
        roles.update(["planner", "retriever"])
        roles.add("coder_fast" if complexity in ["tiny", "small"] else "coder_strong")

    if task_class in ["bug_triage", "ci_failure_debug", "test_repair"]:
        roles.update(["retriever", "verifier"])
        if needs_code_changes:
            roles.add("coder_fast" if complexity in ["tiny", "small"] else "coder_strong")

    if task_class in ["code_review"]:
        roles.update(["reviewer", "retriever"])

    if task_class in ["browser_console_task"]:
        roles.update(["ui_agent"])
        if needs_code_changes:
            roles.add("planner")

    if task_class in ["incident_hotfix_assist", "mixed_task"]:
        roles.update(["planner", "retriever", "verifier"])
        roles.add("coder_strong")
        if needs_browser:
            roles.add("ui_agent")

    if risk in ["high", "critical"]:
        roles.add("reviewer")

    if needs_verification and "verifier" not in roles:
        roles.add("verifier")

    return sorted(roles)
```

---

## A.12 Tool plan selection pseudocode

```python id="ggkq5f"
def select_tool_plan(task_class, complexity, risk, needs_browser, needs_connectors, repo_memory):
    tools = []

    # common repo intelligence
    if task_class in ["issue_to_pr", "bug_triage", "test_repair", "refactor_scoped", "dependency_upgrade", "ci_failure_debug"]:
        tools += [
            "repo.rank_files_for_task",
            "repo.related_tests",
            "repo.search_symbols"
        ]

    # patch strategy
    if task_class in ["issue_to_pr", "test_repair", "dependency_upgrade", "refactor_scoped", "incident_hotfix_assist"]:
        if complexity in ["tiny", "small"]:
            tools += ["patch.inline", "test.run_targeted"]
        else:
            tools += ["patch.structured", "exec", "test.run_targeted", "test.run_package_subset"]

    # triage flows
    if task_class in ["bug_triage", "ci_failure_debug"]:
        tools += ["exec", "logs.parse_failure_bundle", "review.scan_regression"]

    # review-only flows
    if task_class == "code_review":
        tools += ["review.scan_regression", "review.scan_security", "repo.dependency_impact"]

    # browser flows
    if needs_browser:
        tools += [
            "browser.snapshot",
            "browser.find_ref",
            "browser.act",
            "browser.verify",
        ]

    # connectors
    if needs_connectors:
        tools += ["connector.read"]
        if risk in ["low", "moderate"]:
            tools += ["connector.propose_write"]

    # risk-based additions
    if risk in ["high", "critical"]:
        tools += ["review.scan_security", "review.scan_risk"]

    # dedupe preserve order
    return dedupe(tools)
```

---

## A.13 Sub-agent decision pseudocode

```python id="1qqg48"
def decide_subagents(task_class, complexity, risk, needs_browser, context_pressure):
    subagents = []

    if complexity in ["tiny", "small"] and risk == "low":
        return subagents

    if task_class in ["issue_to_pr", "refactor_scoped", "dependency_upgrade", "mixed_task", "incident_hotfix_assist"]:
        subagents.append(make_subagent("scout"))
        subagents.append(make_subagent("implementer"))
        subagents.append(make_subagent("tester"))

    if task_class in ["bug_triage", "ci_failure_debug", "test_repair"]:
        subagents.append(make_subagent("scout"))
        subagents.append(make_subagent("tester"))

    if needs_browser:
        subagents.append(make_subagent("ui_executor"))

    if risk in ["high", "critical"]:
        subagents.append(make_subagent("reviewer"))

    if context_pressure == "high" and not has_role(subagents, "scout"):
        subagents.append(make_subagent("scout"))

    return limit_subagents(subagents, max_count=4)
```

---

## A.14 Approval mode selection pseudocode

```python id="nx4bch"
def select_approval_mode(risk, team_memory, task_intake):
    strict_mode = team_memory.get("risk_policies", {}).get("strict_mode", False)

    if task_intake.requires_prod_write or task_intake.requires_secrets:
        return "explicit"

    if risk == "critical":
        return "explicit"

    if risk == "high":
        return "guarded"

    if strict_mode:
        return "guarded"

    return "standard"
```

---

## A.15 Budget allocation pseudocode

```python id="2t33kx"
def allocate_budget(task_class, complexity, risk, needs_browser, needs_connectors):
    base = {
        "max_model_calls": 8,
        "max_tool_calls": 15,
        "max_wall_clock_minutes": 10
    }

    if complexity == "small":
        base = {
            "max_model_calls": 12,
            "max_tool_calls": 20,
            "max_wall_clock_minutes": 15
        }
    elif complexity == "medium":
        base = {
            "max_model_calls": 18,
            "max_tool_calls": 35,
            "max_wall_clock_minutes": 25
        }
    elif complexity in ["large", "long_horizon"]:
        base = {
            "max_model_calls": 28,
            "max_tool_calls": 60,
            "max_wall_clock_minutes": 45
        }

    if needs_browser:
        base["max_tool_calls"] += 10
        base["max_wall_clock_minutes"] += 10

    if needs_connectors:
        base["max_tool_calls"] += 8

    if risk in ["high", "critical"]:
        base["max_model_calls"] += 4

    if task_class == "code_review":
        base["max_tool_calls"] = max(base["max_tool_calls"], 18)

    return base
```

---

## A.16 Checkpoint policy pseudocode

```python id="kw7x4x"
def decide_checkpoint_policy(complexity, risk, needs_browser, needs_connectors):
    policy = {
        "enabled": True,
        "save_on_states": ["PLANNED", "CONTEXT_READY", "READY_FOR_REVIEW"],
        "save_on_tool_side_effects": True,
        "save_on_approval_boundary": True
    }

    if complexity in ["large", "long_horizon"]:
        policy["save_on_states"] += ["EXECUTING", "VERIFYING", "RETRYING"]

    if risk in ["high", "critical"]:
        policy["save_before_sensitive_actions"] = True

    if needs_browser or needs_connectors:
        policy["save_before_external_write"] = True

    return policy
```

---

## A.17 Recovery policy pseudocode

```python id="k2szfr"
def select_recovery_policy(task_class, risk, operational_memory):
    policy = {
        "model_schema_retry_same_model": 1,
        "model_fallback_retry": 1,
        "tool_timeout_retry": 1,
        "patch_mode_fallback_enabled": True,
        "browser_resnapshot_on_stale_ref": True,
        "planner_escalation_after_patch_failures": 2,
        "checkpoint_resume_enabled": True
    }

    if task_class in ["browser_console_task", "mixed_task"]:
        policy["browser_max_retries"] = 3

    if task_class in ["ci_failure_debug", "bug_triage", "test_repair"]:
        policy["failure_bundle_compaction"] = True

    if risk == "critical":
        policy["ask_before_aggressive_recovery"] = True

    if operational_memory_has_provider_incident(operational_memory):
        policy["prefer_provider_failover"] = True

    return policy
```

---

## A.18 Step-level routing pseudocode

Router tidak hanya berjalan sekali di intake. Ia harus berjalan ulang per step.

```python id="1j7hyj"
def reroute_after_step(task_state, latest_step_result):
    if latest_step_result["status"] == "failed":
        error_class = latest_step_result["error_class"]

        if error_class in ["invalid_patch"]:
            return "switch_patch_mode"

        if error_class in ["tool_timeout", "service_unavailable"]:
            return "retry_or_alternate_tool"

        if error_class in ["stale_ref", "stale_state"]:
            return "browser_recovery"

        if error_class in ["assertion_failure", "command_failed"]:
            return "invoke_verifier"

        if error_class in ["approval_required", "unsafe_action_blocked"]:
            return "await_approval"

        if error_class in ["context_overflow"]:
            return "summarize_and_trim"

    if latest_step_result["status"] == "completed" and latest_step_result.get("next_recommended_action"):
        return latest_step_result["next_recommended_action"]

    return "continue"
```

---

## A.19 Heuristic rules yang penting

### Rules untuk coding tasks

* jangan patch sebelum `repo.rank_files_for_task` selesai, kecuali file target sudah eksplisit
* jangan full test sebelum targeted tests
* jangan broad review scan sebelum ada patch candidate
* jika dua patch gagal di lokasi sama, panggil planner ulang

### Rules untuk browser tasks

* snapshot dulu, act belakangan
* verifikasi setelah setiap write action
* stale ref wajib resnapshot, bukan klik ulang buta
* browser write action tanpa verification = invalid flow

### Rules untuk connector tasks

* write actions butuh idempotency key jika tersedia
* external write wajib menyimpan artifact summary
* jika connector write gagal, jangan ulang buta tanpa cek apakah side effect sebenarnya sudah terjadi

---

# B. JSON Schema Inti

Di bawah ini saya definisikan object-object inti yang saya sarankan ada di runtime OpenClaw.

---

## B.1 Task schema

```json id="y5gdth"
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Task",
  "type": "object",
  "required": [
    "task_id",
    "created_at",
    "goal",
    "status",
    "task_class",
    "complexity",
    "risk_tier",
    "workspace",
    "routing",
    "budget"
  ],
  "properties": {
    "task_id": {"type": "string"},
    "created_at": {"type": "string", "format": "date-time"},
    "updated_at": {"type": "string", "format": "date-time"},
    "goal": {"type": "string"},
    "normalized_goal": {"type": "string"},
    "status": {
      "type": "string",
      "enum": [
        "CREATED",
        "SCOPING",
        "PLANNED",
        "CONTEXT_READY",
        "EXECUTING",
        "VERIFYING",
        "BLOCKED",
        "RETRYING",
        "READY_FOR_REVIEW",
        "COMPLETED",
        "FAILED",
        "CANCELLED"
      ]
    },
    "task_class": {
      "type": "string",
      "enum": [
        "planning",
        "repo_exploration",
        "issue_to_pr",
        "bug_triage",
        "test_repair",
        "dependency_upgrade",
        "refactor_scoped",
        "code_review",
        "ci_failure_debug",
        "browser_console_task",
        "incident_hotfix_assist",
        "release_prep",
        "mixed_task",
        "unknown"
      ]
    },
    "complexity": {
      "type": "string",
      "enum": ["tiny", "small", "medium", "large", "long_horizon"]
    },
    "risk_tier": {
      "type": "string",
      "enum": ["low", "moderate", "high", "critical"]
    },
    "workspace": {
      "type": "object",
      "required": ["repo_id", "workspace_id"],
      "properties": {
        "repo_id": {"type": "string"},
        "workspace_id": {"type": "string"},
        "branch_name": {"type": "string"},
        "snapshot_id": {"type": "string"}
      }
    },
    "routing": {
      "type": "object",
      "required": ["roles", "tool_plan", "approval_mode"],
      "properties": {
        "roles": {
          "type": "array",
          "items": {
            "type": "string",
            "enum": [
              "planner",
              "retriever",
              "coder_fast",
              "coder_strong",
              "verifier",
              "reviewer",
              "ui_agent"
            ]
          }
        },
        "tool_plan": {
          "type": "array",
          "items": {"type": "string"}
        },
        "subagent_plan": {
          "type": "array",
          "items": {"$ref": "#/$defs/SubagentSpec"}
        },
        "approval_mode": {
          "type": "string",
          "enum": ["standard", "guarded", "explicit"]
        }
      }
    },
    "budget": {
      "$ref": "#/$defs/Budget"
    },
    "constraints": {
      "type": "array",
      "items": {"type": "string"}
    },
    "issue_context": {
      "type": "object",
      "properties": {
        "issue_id": {"type": "string"},
        "title": {"type": "string"},
        "priority": {"type": "string"},
        "tracker": {"type": "string"}
      }
    },
    "browser_context": {
      "type": "object",
      "properties": {
        "target_app": {"type": "string"},
        "target_url": {"type": "string"}
      }
    },
    "current_checkpoint_id": {"type": "string"},
    "latest_confidence": {"type": "number", "minimum": 0, "maximum": 1},
    "artifacts": {
      "type": "array",
      "items": {"type": "string"}
    },
    "pending_approvals": {
      "type": "array",
      "items": {"type": "string"}
    }
  },
  "$defs": {
    "Budget": {
      "type": "object",
      "required": ["max_model_calls", "max_tool_calls", "max_wall_clock_minutes"],
      "properties": {
        "max_model_calls": {"type": "integer", "minimum": 1},
        "max_tool_calls": {"type": "integer", "minimum": 1},
        "max_wall_clock_minutes": {"type": "integer", "minimum": 1}
      }
    },
    "SubagentSpec": {
      "type": "object",
      "required": ["role", "enabled"],
      "properties": {
        "role": {
          "type": "string",
          "enum": ["scout", "implementer", "tester", "reviewer", "doc_writer", "ui_executor"]
        },
        "enabled": {"type": "boolean"},
        "objective": {"type": "string"},
        "file_scope": {
          "type": "array",
          "items": {"type": "string"}
        }
      }
    }
  }
}
```

---

## B.2 TaskPlan schema

```json id="xzfnd8"
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "TaskPlan",
  "type": "object",
  "required": ["plan_id", "task_id", "steps", "success_criteria"],
  "properties": {
    "plan_id": {"type": "string"},
    "task_id": {"type": "string"},
    "summary": {"type": "string"},
    "steps": {
      "type": "array",
      "items": {"$ref": "#/$defs/PlanStep"}
    },
    "success_criteria": {
      "type": "array",
      "items": {"type": "string"}
    },
    "open_questions": {
      "type": "array",
      "items": {"type": "string"}
    },
    "assumptions": {
      "type": "array",
      "items": {"type": "string"}
    },
    "confidence": {"type": "number", "minimum": 0, "maximum": 1}
  },
  "$defs": {
    "PlanStep": {
      "type": "object",
      "required": ["step_id", "name", "kind", "status"],
      "properties": {
        "step_id": {"type": "string"},
        "name": {"type": "string"},
        "kind": {
          "type": "string",
          "enum": [
            "read_context",
            "repo_search",
            "patch",
            "test",
            "review",
            "browser",
            "connector",
            "approval",
            "summarize"
          ]
        },
        "status": {
          "type": "string",
          "enum": ["pending", "running", "completed", "failed", "blocked", "skipped"]
        },
        "depends_on": {
          "type": "array",
          "items": {"type": "string"}
        },
        "tool_candidates": {
          "type": "array",
          "items": {"type": "string"}
        },
        "expected_artifacts": {
          "type": "array",
          "items": {"type": "string"}
        },
        "confidence_gate": {"type": "number", "minimum": 0, "maximum": 1}
      }
    }
  }
}
```

---

## B.3 Checkpoint schema

```json id="2ahfez"
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Checkpoint",
  "type": "object",
  "required": [
    "checkpoint_id",
    "task_id",
    "created_at",
    "task_status",
    "last_successful_step",
    "resume_instruction"
  ],
  "properties": {
    "checkpoint_id": {"type": "string"},
    "task_id": {"type": "string"},
    "created_at": {"type": "string", "format": "date-time"},
    "task_status": {"type": "string"},
    "last_successful_step": {"type": "string"},
    "plan_summary": {"type": "string"},
    "file_scope": {
      "type": "array",
      "items": {"type": "string"}
    },
    "artifacts": {
      "type": "array",
      "items": {"type": "string"}
    },
    "pending_approvals": {
      "type": "array",
      "items": {"type": "string"}
    },
    "retry_count": {"type": "integer", "minimum": 0},
    "confidence_snapshot": {"type": "number", "minimum": 0, "maximum": 1},
    "resume_instruction": {"type": "string"},
    "next_recommended_action": {"type": "string"},
    "workspace_snapshot_id": {"type": "string"}
  }
}
```

---

## B.4 SessionMemory schema

```json id="5fij85"
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "SessionMemory",
  "type": "object",
  "required": ["task_id", "objective", "current_plan"],
  "properties": {
    "task_id": {"type": "string"},
    "objective": {"type": "string"},
    "constraints": {
      "type": "array",
      "items": {"type": "string"}
    },
    "assumptions": {
      "type": "array",
      "items": {"type": "string"}
    },
    "file_shortlist": {
      "type": "array",
      "items": {"type": "string"}
    },
    "attempted_actions": {
      "type": "array",
      "items": {"$ref": "#/$defs/AttemptedAction"}
    },
    "open_questions": {
      "type": "array",
      "items": {"type": "string"}
    },
    "current_plan": {
      "type": "array",
      "items": {"type": "string"}
    },
    "pending_approvals": {
      "type": "array",
      "items": {"type": "string"}
    },
    "confidence": {"type": "number", "minimum": 0, "maximum": 1}
  },
  "$defs": {
    "AttemptedAction": {
      "type": "object",
      "required": ["step", "summary", "result"],
      "properties": {
        "step": {"type": "string"},
        "summary": {"type": "string"},
        "result": {"type": "string", "enum": ["succeeded", "failed", "partial"]},
        "artifact_refs": {
          "type": "array",
          "items": {"type": "string"}
        }
      }
    }
  }
}
```

---

## B.5 RepoMemory schema

```json id="1jqf1m"
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "RepoMemory",
  "type": "object",
  "required": ["repo_id"],
  "properties": {
    "repo_id": {"type": "string"},
    "build_commands": {
      "type": "array",
      "items": {"type": "string"}
    },
    "test_commands": {
      "type": "object",
      "additionalProperties": {"type": "string"}
    },
    "lint_commands": {
      "type": "array",
      "items": {"type": "string"}
    },
    "format_commands": {
      "type": "array",
      "items": {"type": "string"}
    },
    "critical_paths": {
      "type": "array",
      "items": {"type": "string"}
    },
    "architecture_summary": {
      "type": "object",
      "additionalProperties": {"type": "string"}
    },
    "common_failure_patterns": {
      "type": "array",
      "items": {"$ref": "#/$defs/FailurePattern"}
    },
    "known_flaky_tests": {
      "type": "array",
      "items": {"type": "string"}
    },
    "dangerous_modules": {
      "type": "array",
      "items": {"type": "string"}
    },
    "freshness_ts": {"type": "string", "format": "date-time"}
  },
  "$defs": {
    "FailurePattern": {
      "type": "object",
      "required": ["pattern", "recommendation"],
      "properties": {
        "pattern": {"type": "string"},
        "recommendation": {"type": "string"}
      }
    }
  }
}
```

---

## B.6 TeamMemory schema

```json id="mrjlwm"
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "TeamMemory",
  "type": "object",
  "required": ["team_id"],
  "properties": {
    "team_id": {"type": "string"},
    "review_preferences": {
      "type": "array",
      "items": {"type": "string"}
    },
    "risk_policies": {
      "type": "object",
      "properties": {
        "strict_mode": {"type": "boolean"},
        "lockfile_changes_require_approval": {"type": "boolean"},
        "migration_files_require_approval": {"type": "boolean"},
        "critical_paths_require_review": {"type": "boolean"}
      }
    },
    "pr_preferences": {
      "type": "object",
      "properties": {
        "include_test_summary": {"type": "boolean"},
        "include_risk_section": {"type": "boolean"},
        "prefer_small_diffs": {"type": "boolean"}
      }
    }
  }
}
```

---

## B.7 OperationalMemory schema

```json id="e8i56q"
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "OperationalMemory",
  "type": "object",
  "properties": {
    "provider_incidents": {
      "type": "array",
      "items": {"$ref": "#/$defs/ProviderIncident"}
    },
    "tool_failure_signatures": {
      "type": "array",
      "items": {"$ref": "#/$defs/ToolFailureSignature"}
    },
    "env_signals": {
      "type": "array",
      "items": {"$ref": "#/$defs/EnvSignal"}
    }
  },
  "$defs": {
    "ProviderIncident": {
      "type": "object",
      "required": ["provider", "issue", "severity"],
      "properties": {
        "provider": {"type": "string"},
        "issue": {"type": "string"},
        "severity": {"type": "string", "enum": ["low", "medium", "high", "critical"]}
      }
    },
    "ToolFailureSignature": {
      "type": "object",
      "required": ["tool", "pattern", "recommended_recovery"],
      "properties": {
        "tool": {"type": "string"},
        "pattern": {"type": "string"},
        "recommended_recovery": {"type": "string"}
      }
    },
    "EnvSignal": {
      "type": "object",
      "required": ["workspace", "issue", "last_seen"],
      "properties": {
        "workspace": {"type": "string"},
        "issue": {"type": "string"},
        "last_seen": {"type": "string", "format": "date-time"}
      }
    }
  }
}
```

---

## B.8 Artifact schema

```json id="58kt8r"
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Artifact",
  "type": "object",
  "required": [
    "artifact_id",
    "task_id",
    "type",
    "created_at",
    "uri"
  ],
  "properties": {
    "artifact_id": {"type": "string"},
    "task_id": {"type": "string"},
    "type": {
      "type": "string",
      "enum": [
        "diff",
        "patch",
        "log",
        "junit",
        "json_report",
        "markdown_summary",
        "screenshot",
        "dom_snapshot",
        "network_summary",
        "console_summary",
        "pr_body",
        "review_report",
        "risk_report",
        "failure_bundle"
      ]
    },
    "created_at": {"type": "string", "format": "date-time"},
    "producer_step": {"type": "string"},
    "uri": {"type": "string"},
    "mime_type": {"type": "string"},
    "size_bytes": {"type": "integer", "minimum": 0},
    "retention_class": {
      "type": "string",
      "enum": ["short", "standard", "audit"]
    },
    "metadata": {
      "type": "object",
      "additionalProperties": true
    }
  }
}
```

---

## B.9 ToolResult schema

```json id="nnyprz"
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ToolResult",
  "type": "object",
  "required": [
    "tool_name",
    "status",
    "duration_ms",
    "retryable",
    "structured_output",
    "artifact_refs"
  ],
  "properties": {
    "tool_name": {"type": "string"},
    "status": {"type": "string", "enum": ["completed", "failed", "partial"]},
    "duration_ms": {"type": "integer", "minimum": 0},
    "retryable": {"type": "boolean"},
    "error_class": {"type": "string"},
    "structured_output": {
      "type": "object",
      "additionalProperties": true
    },
    "artifact_refs": {
      "type": "array",
      "items": {"type": "string"}
    },
    "confidence": {"type": "number", "minimum": 0, "maximum": 1},
    "side_effects": {
      "type": "array",
      "items": {"type": "string"}
    },
    "next_recommended_action": {"type": "string"}
  }
}
```

---

## B.10 ApprovalRequest schema

```json id="wvp9pk"
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ApprovalRequest",
  "type": "object",
  "required": [
    "approval_id",
    "task_id",
    "action",
    "reason",
    "confidence",
    "status"
  ],
  "properties": {
    "approval_id": {"type": "string"},
    "task_id": {"type": "string"},
    "action": {"type": "string"},
    "reason": {"type": "string"},
    "risk_summary": {
      "type": "array",
      "items": {"type": "string"}
    },
    "artifact_refs": {
      "type": "array",
      "items": {"type": "string"}
    },
    "confidence": {"type": "number", "minimum": 0, "maximum": 1},
    "status": {
      "type": "string",
      "enum": ["pending", "approved", "rejected", "expired"]
    },
    "requested_at": {"type": "string", "format": "date-time"},
    "resolved_at": {"type": "string", "format": "date-time"}
  }
}
```

---

## B.11 SubagentRun schema

```json id="o2dsts"
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "SubagentRun",
  "type": "object",
  "required": [
    "subagent_id",
    "task_id",
    "role",
    "objective",
    "status"
  ],
  "properties": {
    "subagent_id": {"type": "string"},
    "task_id": {"type": "string"},
    "role": {
      "type": "string",
      "enum": ["scout", "implementer", "tester", "reviewer", "doc_writer", "ui_executor"]
    },
    "objective": {"type": "string"},
    "file_scope": {
      "type": "array",
      "items": {"type": "string"}
    },
    "tool_scope": {
      "type": "array",
      "items": {"type": "string"}
    },
    "status": {
      "type": "string",
      "enum": ["created", "running", "completed", "failed", "cancelled"]
    },
    "summary": {"type": "string"},
    "artifacts": {
      "type": "array",
      "items": {"type": "string"}
    },
    "confidence": {"type": "number", "minimum": 0, "maximum": 1},
    "blocking_issues": {
      "type": "array",
      "items": {"type": "string"}
    },
    "recommended_next_action": {"type": "string"}
  }
}
```

---

## B.12 FailureBundle schema

```json id="oazr89"
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "FailureBundle",
  "type": "object",
  "required": [
    "bundle_id",
    "task_id",
    "class",
    "failing_targets"
  ],
  "properties": {
    "bundle_id": {"type": "string"},
    "task_id": {"type": "string"},
    "class": {
      "type": "string",
      "enum": [
        "syntax",
        "import_type",
        "assertion_failure",
        "flaky",
        "env_config",
        "timeout",
        "performance",
        "browser_state",
        "connector_error",
        "unknown"
      ]
    },
    "failing_targets": {
      "type": "array",
      "items": {"type": "string"}
    },
    "suspected_files": {
      "type": "array",
      "items": {"type": "string"}
    },
    "log_excerpt_refs": {
      "type": "array",
      "items": {"type": "string"}
    },
    "recommended_route": {"type": "string"}
  }
}
```

---

# Rekomendasi implementasi praktis

Kalau saya sederhanakan, implementasi runtime bisa dimulai dari object berikut dulu:

1. `Task`
2. `TaskPlan`
3. `Checkpoint`
4. `ToolResult`
5. `Artifact`
6. `ApprovalRequest`
7. `SessionMemory`
8. `RepoMemory`

Delapan object ini sudah cukup untuk membangun:

* orchestration loop
* state machine
* resumability
* approvals
* telemetry dasar
* agent-to-tool contract

---

# Urutan implementasi yang saya sarankan

## Sprint 1

Bangun:

* `Task`
* `TaskPlan`
* `Checkpoint`
* `ToolResult`
* router intake
* state transitions dasar

## Sprint 2

Bangun:

* `Artifact`
* `ApprovalRequest`
* `SessionMemory`
* retry engine
* checkpoint resume

## Sprint 3

Bangun:

* `RepoMemory`
* `SubagentRun`
* `FailureBundle`
* planner/verifier loop
* targeted test orchestration

---

# Keputusan desain penting dari A + B

Dari dua bagian ini, ada beberapa keputusan struktural yang sekarang sudah “terkunci”:

* routing dilakukan **dua kali**: saat intake dan setelah step penting
* task harus selalu punya `complexity` dan `risk_tier`
* checkpoint bukan fitur tambahan, tapi bagian inti lifecycle
* tool outputs harus seragam
* memory harus dipisah antara session, repo, team, dan operational
* approval adalah first-class object, bukan sekadar boolean
* sub-agent outputs harus ringkas dan terstruktur
* failure harus dipaketkan menjadi `FailureBundle`, bukan raw logs mentah

---



#### PerplexityComputerV2.part4.md

# D. Error Taxonomy + Retry Matrix Operasional

## 1. Tujuan error system

Error system harus menjawab 5 pertanyaan untuk setiap kegagalan:

1. **apa yang gagal?**
2. **ini kegagalan model, tool, environment, browser, connector, atau logic task?**
3. **apakah aman untuk retry?**
4. **kalau retry, retry dengan strategi apa?**
5. **kapan harus escalate ke planner, reviewer, atau manusia?**

---

# 2. Prinsip dasar desain

## 2.1 Error bukan string bebas

Jangan biarkan semua tool/model hanya mengembalikan pesan teks seperti:

* “something went wrong”
* “request failed”
* “timeout”
* “unable to complete”

Setiap failure harus dipetakan ke `error_class` yang baku.

## 2.2 Retry harus class-aware

`timeout` tidak diperlakukan sama dengan `invalid_patch`.
`stale_ref` tidak diperlakukan sama dengan `permission_denied`.
`rate_limit` tidak diperlakukan sama dengan `approval_required`.

## 2.3 Retry budget harus terbatas

Tidak boleh ada infinite recovery loop. Setiap error family punya retry cap.

## 2.4 Recovery path harus bisa berubah

Kadang solusi bukan retry pada path yang sama, tetapi:

* ganti model,
* ganti tool,
* ganti mode patch,
* re-snapshot browser,
* ringkas context,
* atau minta approval.

## 2.5 Escalation harus eksplisit

Setelah ambang tertentu, sistem harus berhenti auto-retry dan:

* kembali ke planner,
* minta approval,
* atau fail dengan artifact yang jelas.

---

# 3. Struktur error object

Saya sarankan setiap kegagalan dinormalisasi ke struktur seperti ini:

```json id="9ihdzn"
{
  "error_id": "err_123",
  "timestamp": "2026-04-05T09:12:00Z",
  "scope": "tool",
  "error_class": "tool_timeout",
  "error_family": "transient_tool_failure",
  "severity": "medium",
  "retryable": true,
  "retry_strategy": "same_tool_once_then_alternate",
  "source": {
    "component": "test.run_targeted",
    "step_id": "step_test_01",
    "task_id": "task_123"
  },
  "message": "Targeted test run exceeded timeout after 120 seconds",
  "structured_context": {
    "timeout_seconds": 120,
    "command": "pnpm test login.spec.ts"
  },
  "artifact_refs": [
    "artifact://logs/targeted_test_timeout.log"
  ],
  "recommended_next_action": "retry_with_narrower_scope",
  "escalate_if_repeated": true
}
```

---

# 4. Error families

Agar lebih mudah dikelola, kelompokkan dulu ke family besar.

## 4.1 Model failures

Masalah dari reasoning/model layer.

Contoh:

* invalid structured output
* refusal
* context overflow
* hallucinated tool call
* incoherent plan

## 4.2 Tool execution failures

Masalah dari tools lokal/runtime.

Contoh:

* shell timeout
* command failed
* patch apply failure
* artifact missing

## 4.3 Environment failures

Masalah dari sandbox/workspace/build runtime.

Contoh:

* missing dependency
* corrupted workspace
* disk full
* service unavailable

## 4.4 Browser failures

Masalah dari UI/browser execution.

Contoh:

* stale ref
* modal blocked
* auth redirect
* page not loaded

## 4.5 Connector failures

Masalah dari API eksternal / app connector.

Contoh:

* permission denied
* rate limit
* partial write uncertainty
* external service down

## 4.6 Task logic / policy failures

Masalah pada strategi atau safety boundary.

Contoh:

* ambiguous goal
* approval required
* unsafe action blocked
* scope too broad
* confidence below threshold

---

# 5. Error taxonomy v1

Di bawah ini daftar kelas error yang saya sarankan sebagai baseline.

---

## 5.1 Model error classes

### `model_refusal`

Model menolak melakukan langkah yang sebenarnya valid, atau menolak dalam format yang memutus workflow.

Recovery:

* retry sekali dengan prompt framing lebih ketat
* jika tetap gagal, fallback ke model/provider lain

### `invalid_structured_response`

Model tidak mengikuti schema JSON / contract.

Recovery:

* retry sekali pada model yang sama dengan schema repair prompt
* jika gagal lagi, fallback model

### `context_overflow`

Input/context terlalu besar atau model kehilangan koherensi akibat context pressure.

Recovery:

* summarize working set
* trim logs
* kirim hanya failure bundle / shortlisted files
* reroute ke planner/retriever

### `hallucinated_tool_use`

Model meminta tool yang tidak tersedia atau argumen yang tak valid secara struktural.

Recovery:

* jangan execute
* repair via router/tool planner
* turunkan confidence

### `low_confidence_output`

Respons model terlalu lemah / ambigu untuk dipakai lanjut.

Recovery:

* escalate ke planner atau reviewer
* jangan auto-act bila langkah sensitif

### `invalid_plan`

Plan yang dihasilkan bertentangan dengan policy, terlalu luas, atau tidak executable.

Recovery:

* replan via planner
* sempitkan scope

---

## 5.2 Tool error classes

### `tool_timeout`

Tool tidak selesai dalam waktu yang diizinkan.

Recovery:

* retry sekali
* jika masih gagal, sempitkan scope atau pindah ke alternate path

### `command_failed`

Command shell gagal secara eksplisit.

Recovery:

* parse stderr/stdout
* klasifikasikan lebih lanjut
* kirim ke verifier jika perlu

### `invalid_patch`

Patch tidak bisa diaplikasikan atau menghasilkan file rusak.

Recovery:

* ganti patch mode
* inline → structured → branch_patch
* setelah 2 gagal, escalate planner

### `artifact_missing`

Tool mengklaim output ada, tapi artifact tidak ditemukan / invalid.

Recovery:

* verify once
* regenerate artifact
* jika tetap gagal, mark tool unreliable

### `malformed_request`

Permintaan ke tool adapter rusak atau argumen tidak valid.

Recovery:

* repair request
* jangan retry blind

### `permission_denied`

Tool lokal / file / workspace tidak punya izin yang dibutuhkan.

Recovery:

* stop auto-retry
* minta approval atau perbaiki workspace policy

---

## 5.3 Environment error classes

### `missing_dependency`

Package/binary/service yang dibutuhkan tidak tersedia.

Recovery:

* jika policy mengizinkan, minta approval untuk install
* kalau tidak, fail dengan dependency report

### `workspace_corrupted`

Workspace / snapshot / git state tidak konsisten.

Recovery:

* restore dari snapshot/checkpoint
* recreate clean workspace

### `cache_corrupted`

Cache build/test rusak.

Recovery:

* clear relevant cache
* rerun sekali

### `disk_full`

Storage workspace habis.

Recovery:

* cleanup artifacts non-essential
* fail jika tidak cukup

### `service_unavailable`

Service lokal/integration target tidak hidup.

Recovery:

* retry startup
* check health
* jika tidak recover, mark env-blocked

### `flaky_env`

Lingkungan menghasilkan hasil non-deterministik.

Recovery:

* rerun dengan bukti
* tandai unstable
* jangan escalate patch terlalu cepat

---

## 5.4 Browser error classes

### `stale_ref`

Ref DOM sudah kadaluarsa.

Recovery:

* resnapshot
* refind target
* jangan klik ulang blind

### `stale_state`

State halaman berubah setelah aksi / navigasi sehingga rencana lama invalid.

Recovery:

* fingerprint current state
* replan langkah berikutnya

### `auth_redirect`

Browser diarahkan ke login / sesi habis.

Recovery:

* stop auto progression
* minta autentikasi / konfirmasi user

### `modal_blocked`

Aksi terhalang modal/toast/overlay.

Recovery:

* dismiss if safe
* refind element
* verify page state lagi

### `hidden_element`

Elemen target tidak interactable.

Recovery:

* scroll/refind/wait
* jika tetap gagal, alternate selector strategy

### `action_not_applied`

Klik/input seolah berhasil tapi state tidak berubah.

Recovery:

* verify state delta
* retry once
* lalu ganti action strategy

### `page_not_loaded`

Halaman belum siap / resource utama gagal dimuat.

Recovery:

* wait condition
* reload once
* inspect network/console summary

---

## 5.5 Connector error classes

### `connector_rate_limit`

API eksternal membatasi request.

Recovery:

* backoff
* retry sekali/terjadwal
* jika task mendesak, alternate provider path

### `connector_permission_denied`

Akses ke resource eksternal ditolak.

Recovery:

* jangan retry blind
* minta approval / auth / rights clarification

### `connector_partial_write_unknown`

Write request mungkin berhasil, tapi respons gagal atau ambigu.

Recovery:

* wajib check post-condition/idempotency
* jangan ulang blind
* fetch current external state dulu

### `external_service_down`

Provider eksternal down.

Recovery:

* failover kalau ada
* jika tidak, block dan resume nanti

### `connector_bad_request`

Payload write/read salah format.

Recovery:

* repair payload
* retry sekali

---

## 5.6 Task logic / policy error classes

### `ambiguous_goal`

Tujuan terlalu ambigu untuk eksekusi aman.

Recovery:

* re-scope via planner
* jika tetap ambigu dan high-risk, ask user

### `approval_required`

Action tidak boleh lanjut tanpa persetujuan.

Recovery:

* buat `ApprovalRequest`
* pindah ke `BLOCKED`

### `unsafe_action_blocked`

Action diblok policy engine.

Recovery:

* jangan retry
* minta explicit approval atau ubah rencana

### `scope_too_broad`

Task berkembang terlalu lebar dari objective awal.

Recovery:

* split task
* prioritaskan sub-goal utama

### `confidence_below_threshold`

Confidence step terlalu rendah untuk lanjut otomatis.

Recovery:

* escalate
* ask jika action sensitif
* reviewer/planner assist

### `budget_exhausted`

Budget token/tool/wall-clock habis.

Recovery:

* checkpoint
* summarize progress
* minta lanjut eksplisit atau resume later path

---

# 6. Retry matrix operasional

Di bawah ini versi operasionalnya.

## 6.1 Model retry matrix

| Error class                   |         Retry? | Max retry | Recovery path                                        | Escalation                |
| ----------------------------- | -------------: | --------: | ---------------------------------------------------- | ------------------------- |
| `model_refusal`               |             Ya |         2 | retry dengan prompt lebih ketat, lalu fallback model | planner jika tetap gagal  |
| `invalid_structured_response` |             Ya |         2 | same model once, lalu fallback model                 | fail schema jika berulang |
| `context_overflow`            |             Ya |         1 | summarize + trim context                             | planner/retriever         |
| `hallucinated_tool_use`       | Tidak langsung |         0 | repair via router/tool planner                       | turunkan confidence       |
| `low_confidence_output`       |     Tergantung |         1 | planner/reviewer assist                              | approval bila sensitif    |
| `invalid_plan`                |             Ya |         1 | replan dengan planner                                | ask jika tetap ambigu     |

---

## 6.2 Tool retry matrix

| Error class         |      Retry? | Max retry | Recovery path                        | Escalation         |
| ------------------- | ----------: | --------: | ------------------------------------ | ------------------ |
| `tool_timeout`      |          Ya |         1 | retry same tool, lalu alternate path | verifier/planner   |
| `command_failed`    | Tidak blind |         0 | parse failure, classify              | verifier           |
| `invalid_patch`     |          Ya |         2 | switch patch mode                    | planner            |
| `artifact_missing`  |          Ya |         1 | verify/regenerate artifact           | mark tool degraded |
| `malformed_request` |          Ya |         1 | repair request                       | adapter fix        |
| `permission_denied` |       Tidak |         0 | approval/policy fix                  | blocked            |

---

## 6.3 Environment retry matrix

| Error class           |      Retry? | Max retry | Recovery path                           | Escalation         |
| --------------------- | ----------: | --------: | --------------------------------------- | ------------------ |
| `missing_dependency`  | Tidak blind |         0 | ask install / report missing dep        | blocked            |
| `workspace_corrupted` |          Ya |         1 | restore checkpoint / recreate workspace | fail if persistent |
| `cache_corrupted`     |          Ya |         1 | clear cache and rerun                   | env warning        |
| `disk_full`           | Ya terbatas |         1 | cleanup then retry                      | infra alert        |
| `service_unavailable` |          Ya |         1 | health check + restart                  | blocked            |
| `flaky_env`           |          Ya |         1 | rerun with evidence                     | mark unstable      |

---

## 6.4 Browser retry matrix

| Error class          | Retry? | Max retry | Recovery path                | Escalation          |
| -------------------- | -----: | --------: | ---------------------------- | ------------------- |
| `stale_ref`          |     Ya |         3 | resnapshot + refind          | ui-agent replan     |
| `stale_state`        |     Ya |         2 | refingerprint + replan       | planner/ui-agent    |
| `auth_redirect`      |  Tidak |         0 | request auth/confirmation    | blocked             |
| `modal_blocked`      |     Ya |         2 | dismiss + retry              | ui-agent            |
| `hidden_element`     |     Ya |         2 | scroll/refind/wait           | alternate selector  |
| `action_not_applied` |     Ya |         1 | verify then alternate action | ui-agent            |
| `page_not_loaded`    |     Ya |         1 | wait/reload/inspect console  | blocked if persists |

---

## 6.5 Connector retry matrix

| Error class                       |      Retry? | Max retry | Recovery path               | Escalation        |
| --------------------------------- | ----------: | --------: | --------------------------- | ----------------- |
| `connector_rate_limit`            |          Ya |         1 | backoff + retry             | delayed/resume    |
| `connector_permission_denied`     |       Tidak |         0 | auth/approval clarification | blocked           |
| `connector_partial_write_unknown` | Tidak blind |         0 | verify post-condition first | human-safe review |
| `external_service_down`           | Ya terbatas |         1 | failover or wait            | blocked           |
| `connector_bad_request`           |          Ya |         1 | repair payload              | adapter fix       |

---

## 6.6 Policy/task retry matrix

| Error class                  |      Retry? | Max retry | Recovery path            | Escalation            |
| ---------------------------- | ----------: | --------: | ------------------------ | --------------------- |
| `ambiguous_goal`             | Ya terbatas |         1 | planner rescope          | ask user if high-risk |
| `approval_required`          |       Tidak |         0 | create approval request  | blocked               |
| `unsafe_action_blocked`      |       Tidak |         0 | plan alternate safe path | explicit approval     |
| `scope_too_broad`            |          Ya |         1 | split task               | planner               |
| `confidence_below_threshold` |  Tergantung |         1 | reviewer/planner assist  | approval              |
| `budget_exhausted`           | Tidak blind |         0 | checkpoint + summarize   | stop/resume           |

---

# 7. Recovery playbooks

Retry matrix bagus, tapi runtime juga perlu playbook yang bisa dipanggil.

---

## 7.1 Playbook: invalid patch

### Trigger

* `invalid_patch`
* apply failed
* syntax rusak setelah edit
* hunk mismatch berat

### Recovery flow

1. simpan failed patch artifact
2. tandai editing mode saat ini
3. kalau mode `inline` → pindah ke `structured_patch`
4. kalau mode `structured_patch` → pindah ke `branch_patch`
5. rerun minimal syntax check
6. jika gagal 2 kali → planner replan dengan file scope baru

### Jangan lakukan

* apply patch yang sama berkali-kali
* langsung broad test tanpa syntax validation

---

## 7.2 Playbook: CI/test failure after patch

### Trigger

* `command_failed`
* test run failed
* build failed

### Recovery flow

1. parse logs jadi `FailureBundle`
2. klasifikasikan:

   * syntax
   * import/type
   * assertion
   * env
   * flaky
3. kirim bundle ke verifier
4. verifier pilih:

   * patch source
   * patch test
   * rerun flaky
   * env fix
5. rerun hanya targeted scope

### Jangan lakukan

* kirim seluruh log mentah ke model
* jalankan full suite langsung

---

## 7.3 Playbook: stale browser flow

### Trigger

* `stale_ref`
* `stale_state`
* `action_not_applied`

### Recovery flow

1. capture current screenshot + DOM snapshot
2. buat state fingerprint baru
3. bandingkan expected vs actual state
4. refind target
5. replan langkah browser dari state sekarang
6. verify setelah action berikutnya

### Jangan lakukan

* klik ulang ref lama
* mengasumsikan halaman masih sama

---

## 7.4 Playbook: partial external write ambiguity

### Trigger

* connector request timeout setelah submit
* 5xx setelah write
* response missing tetapi side effect mungkin terjadi

### Recovery flow

1. jangan resend blind
2. cek idempotency key jika ada
3. fetch current external state
4. cocokkan apakah action sudah terwujud
5. jika ya → mark success-with-warning
6. jika tidak → retry terkontrol

### Ini sangat penting

Kalau salah ditangani, Anda bisa:

* membuat dua PR
* mengirim dua comment
* membuat tiket ganda
* memicu rerun CI dua kali

---

## 7.5 Playbook: context overflow

### Trigger

* model gagal karena context terlalu besar
* reasoning turun kualitas
* step terlalu banyak artifact/log

### Recovery flow

1. trim file set ke shortlist
2. ringkas log jadi failure bundle
3. pindahkan transcript lama ke checkpoint summary
4. kirim hanya:

   * current goal
   * relevant files
   * recent failed attempts
   * next decision
5. reroute ke planner/retriever bila perlu

### Jangan lakukan

* memotong acak context tanpa merangkum
* membuang failure history yang penting

---

# 8. Escalation policy

Tidak semua failure layak diretry. Sistem perlu escalation rules yang keras.

## 8.1 Escalate ke planner jika:

* dua patch berturut-turut gagal
* scope melebar
* goal ambigu
* context trimming berulang
* verifier dan coder tidak sejalan

## 8.2 Escalate ke reviewer jika:

* menyentuh critical paths
* dependency upgrade punya risiko compatibility
* perubahan menyentuh auth/billing/security
* confidence patch rendah tapi tests lolos

## 8.3 Escalate ke user untuk approval jika:

* secrets access
* connector write sensitif
* production/admin write
* lockfile/migration/deploy
* confidence < threshold pada langkah sensitif

## 8.4 Escalate ke fail-fast jika:

* workspace korup berulang
* permission denied yang jelas permanen
* external service down tanpa failover
* budget exhausted
* unsafe action blocked tanpa alternate path

---

# 9. Severity model

Setiap error sebaiknya juga punya severity, karena tidak semua error sama dampaknya.

## 9.1 Severity levels

### `low`

Gangguan kecil, recovery hampir pasti.
Contoh:

* stale_ref
* malformed_request ringan

### `medium`

Butuh recovery terarah, tapi task masih sehat.
Contoh:

* tool_timeout
* cache_corrupted
* invalid_structured_response

### `high`

Mengganggu keberhasilan task atau menyentuh correctness.
Contoh:

* invalid_patch
* command_failed di build utama
* auth_redirect saat workflow penting

### `critical`

Berisiko side effect besar atau menghambat total.
Contoh:

* connector_partial_write_unknown pada write sensitif
* unsafe_action_blocked di jalur yang tak punya alternatif
* workspace_corrupted persistent
* permission_denied pada resource wajib

---

# 10. Telemetry requirements untuk error system

Setiap error harus tercatat minimal dengan:

* `task_id`
* `step_id`
* `component`
* `error_class`
* `error_family`
* `severity`
* `retryable`
* `retry_count`
* `final_resolution`
* `artifact_refs`
* `elapsed_before_error_ms`

Dengan ini Anda bisa membuat dashboard seperti:

* top failing tools
* most expensive error families
* patch failure rate by model
* stale browser rate by site/app
* permission denied frequency by connector
* rate of partial-write ambiguity

---

# 11. SLO yang bisa dipasang untuk error handling

Beberapa SLO yang realistis:

* `>= 95%` tool failures terklasifikasi ke error_class baku
* `< 2%` blind retries tanpa recovery strategy jelas
* `>= 90%` browser stale-ref errors pulih tanpa intervensi user
* `0` duplicate external writes akibat retry buta
* `>= 80%` invalid_patch recovered dalam <= 2 mode-switch
* `>= 90%` test/build failures dikompaksi menjadi failure bundle sebelum dikirim ke verifier
* `100%` approval-required actions masuk state blocked, bukan diam-diam gagal

---

# 12. Versi schema untuk normalized error

Saya sarankan tambahkan object ini sebagai first-class schema juga.

```json id="ta9vmd"
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "NormalizedError",
  "type": "object",
  "required": [
    "error_id",
    "timestamp",
    "scope",
    "error_class",
    "error_family",
    "severity",
    "retryable",
    "source",
    "message"
  ],
  "properties": {
    "error_id": {"type": "string"},
    "timestamp": {"type": "string", "format": "date-time"},
    "scope": {
      "type": "string",
      "enum": ["model", "tool", "environment", "browser", "connector", "policy", "task_logic"]
    },
    "error_class": {"type": "string"},
    "error_family": {"type": "string"},
    "severity": {
      "type": "string",
      "enum": ["low", "medium", "high", "critical"]
    },
    "retryable": {"type": "boolean"},
    "retry_strategy": {"type": "string"},
    "source": {
      "type": "object",
      "required": ["component", "task_id"],
      "properties": {
        "component": {"type": "string"},
        "step_id": {"type": "string"},
        "task_id": {"type": "string"}
      }
    },
    "message": {"type": "string"},
    "structured_context": {
      "type": "object",
      "additionalProperties": true
    },
    "artifact_refs": {
      "type": "array",
      "items": {"type": "string"}
    },
    "recommended_next_action": {"type": "string"},
    "escalate_if_repeated": {"type": "boolean"}
  }
}
```

---

# 13. Aturan implementasi yang sangat penting

## 13.1 Jangan retry jika side effect tidak pasti

Khusus connector/browser write actions, cek post-condition dulu.

## 13.2 Jangan kirim raw failure mentah ke model besar

Selalu normalisasi jadi `FailureBundle` atau `NormalizedError`.

## 13.3 Jangan campur error klasifikasi dan user-facing copy

`error_class` internal bisa tetap baku dan ringkas. UI copy bisa lebih manusiawi.

## 13.4 Jangan biarkan adapter bebas membuat class baru sembarangan

Harus ada registry / whitelist untuk error classes.

## 13.5 Jangan treat flaky sebagai patch bug secara otomatis

Pisahkan `flaky_env` / `flaky` dari source-code regression.

---

# 14. Rekomendasi urutan implementasi D

## Tahap 1

Bangun:

* `NormalizedError`
* registry `error_class`
* retry policy engine sederhana
* telemetry hooks untuk errors

## Tahap 2

Bangun:

* failure classifier untuk test/build logs
* browser recovery handlers
* connector post-condition checker
* patch-mode switcher

## Tahap 3

Bangun:

* adaptive retry by task class
* provider failover policy
* degraded-mode execution
* error-based benchmark dashboard

---

# 15. Ringkasan inti D

Kalau saya padatkan, desain error-handling ini berdiri di atas 6 keputusan:

1. **semua kegagalan harus dinormalisasi**
2. **retry hanya boleh dilakukan berdasarkan class error**
3. **setiap error family punya recovery path berbeda**
4. **partial external write adalah kelas error khusus**
5. **planner/reviewer/user escalation harus eksplisit**
6. **error telemetry harus cukup detail untuk mengarahkan perbaikan sistem**

---



#### PerplexityComputerV2.part5.md

# E. Task Execution Loop End-to-End

## 1. Tujuan execution loop

Execution loop adalah runtime utama yang harus bisa:

* menerima task baru
* mengklasifikasi dan merutekan task
* membangun plan
* menyiapkan context
* mengeksekusi step demi step
* menjalankan verification
* menangani error dengan recovery yang benar
* menyimpan checkpoint
* memblok task saat approval dibutuhkan
* resume dari checkpoint
* menutup task dengan artifact yang lengkap

Execution loop ini adalah “mesin” utama OpenClaw. Kalau salah desain, seluruh arsitektur di atas akan terasa rapuh.

---

# 2. High-level lifecycle

Secara garis besar, setiap task akan melewati loop ini:

1. **intake**
2. **route**
3. **plan**
4. **prepare context**
5. **execute**
6. **verify**
7. **recover or continue**
8. **request approval if needed**
9. **complete or fail**
10. **persist telemetry, memory, and artifacts**

---

# 3. Core runtime responsibilities

Execution loop harus selalu menjaga 8 hal:

1. **state** — task sedang ada di mana
2. **budget** — berapa resource tersisa
3. **confidence** — seberapa aman langkah berikutnya
4. **policy** — apa yang boleh dilakukan otomatis
5. **artifacts** — evidence dari langkah yang sudah selesai
6. **memory** — ringkasan kerja yang terus diperbarui
7. **recovery** — apa yang dilakukan saat gagal
8. **telemetry** — apa yang dicatat untuk analisis sistem

---

# 4. End-to-end execution loop pseudocode

## 4.1 Top-level orchestration loop

```python id="h3q6jw"
def execute_task(task_intake):
    task = create_task_record(task_intake)
    set_task_status(task, "SCOPING")

    repo_memory = load_repo_memory(task.workspace.repo_id)
    team_memory = load_team_memory(task_intake.team_id)
    operational_memory = load_operational_memory()

    routing = route_task(
        task_intake=task_intake,
        repo_memory=repo_memory,
        team_memory=team_memory,
        operational_memory=operational_memory,
    )

    apply_routing_to_task(task, routing)
    save_checkpoint(task, reason="post_routing")

    set_task_status(task, "PLANNED")
    plan = build_task_plan(task, repo_memory, team_memory)
    attach_plan(task, plan)
    save_checkpoint(task, reason="post_plan")

    set_task_status(task, "CONTEXT_READY")
    context_bundle = prepare_context(task, plan, repo_memory)
    attach_context(task, context_bundle)
    save_checkpoint(task, reason="post_context")

    while True:
        if budget_exhausted(task):
            return handle_budget_exhausted(task)

        if approvals_pending(task):
            return block_for_approval(task)

        if task.status in ["COMPLETED", "FAILED", "CANCELLED"]:
            return finalize_task(task)

        next_step = select_next_step(task, plan)

        if next_step is None:
            return attempt_finalize(task)

        step_result = execute_step(task, next_step)

        if step_result.status == "completed":
            handle_successful_step(task, next_step, step_result)
            continue

        if step_result.status == "partial":
            handle_partial_step(task, next_step, step_result)
            continue

        if step_result.status == "failed":
            recovery_outcome = handle_failed_step(task, next_step, step_result)

            if recovery_outcome == "retry":
                continue
            if recovery_outcome == "blocked":
                return block_for_approval_or_dependency(task)
            if recovery_outcome == "failed":
                set_task_status(task, "FAILED")
                return finalize_task(task)
            if recovery_outcome == "replanned":
                plan = rebuild_task_plan(task)
                continue
```

---

## 4.2 Intake + task creation

```python id="uwf87e"
def create_task_record(task_intake):
    task = Task(
        task_id=generate_id("task"),
        created_at=now(),
        updated_at=now(),
        goal=task_intake.goal,
        normalized_goal=normalize_goal(task_intake.goal),
        status="CREATED",
        workspace=task_intake.workspace,
        constraints=task_intake.constraints,
    )
    persist(task)
    emit_telemetry("task_created", task)
    return task
```

---

# 5. Planning phase

## 5.1 Build initial plan

Planner tidak langsung mengeksekusi. Ia menyusun langkah yang executable.

```python id="k31yyn"
def build_task_plan(task, repo_memory, team_memory):
    planner_input = {
        "goal": task.goal,
        "task_class": task.task_class,
        "complexity": task.complexity,
        "risk_tier": task.risk_tier,
        "constraints": task.constraints,
        "repo_memory": repo_memory,
        "team_memory": team_memory,
        "tool_plan": task.routing.tool_plan,
    }

    planner_output = run_reasoning_role("planner", planner_input)

    validated_steps = validate_plan_steps(planner_output.steps, task.routing.tool_plan)

    return TaskPlan(
        plan_id=generate_id("plan"),
        task_id=task.task_id,
        summary=planner_output.summary,
        steps=validated_steps,
        success_criteria=planner_output.success_criteria,
        assumptions=planner_output.assumptions,
        open_questions=planner_output.open_questions,
        confidence=planner_output.confidence,
    )
```

## 5.2 Plan validation rules

Plan harus divalidasi sebelum dijalankan.

```python id="jv7v4z"
def validate_plan_steps(steps, allowed_tools):
    for step in steps:
        if step.kind == "patch" and not any(t in allowed_tools for t in ["patch.inline", "patch.structured", "exec"]):
            raise InvalidPlan("Patch step missing allowed patch tool")

        if step.kind == "browser" and "browser.snapshot" not in allowed_tools:
            raise InvalidPlan("Browser step without browser tool support")

        if step.kind == "connector" and not any(t.startswith("connector.") for t in allowed_tools):
            raise InvalidPlan("Connector step without connector support")

    return steps
```

---

# 6. Context preparation phase

Context prep bertujuan mengurangi noise sebelum model kerja.

## 6.1 Context prep pseudocode

```python id="sv9rhp"
def prepare_context(task, plan, repo_memory):
    context = {
        "file_shortlist": [],
        "related_tests": [],
        "repo_summary": None,
        "critical_paths": repo_memory.get("critical_paths", []),
        "recent_failures": [],
        "browser_state": None,
        "connector_targets": [],
    }

    if task.task_class in [
        "issue_to_pr",
        "bug_triage",
        "test_repair",
        "dependency_upgrade",
        "refactor_scoped",
        "ci_failure_debug",
    ]:
        context["file_shortlist"] = call_tool("repo.rank_files_for_task", {
            "query": task.goal,
            "top_k": 12
        })

        context["related_tests"] = call_tool("repo.related_tests", {
            "files": extract_file_paths(context["file_shortlist"])
        })

        context["repo_summary"] = call_tool("repo.get_repo_summary", {})

    if task.task_class == "browser_console_task":
        context["browser_state"] = None

    return context
```

## 6.2 Context compaction rule

Setelah context siap, hanya hal ini yang boleh masuk ke reasoning step default:

* goal
* current plan step
* file shortlist
* related tests
* failed attempts ringkas
* risk notes
* pending blockers

Bukan seluruh repo map, bukan seluruh log.

---

# 7. Step selection

Execution loop butuh policy untuk memilih step berikutnya.

## 7.1 Next-step selection rules

```python id="ujkz9i"
def select_next_step(task, plan):
    runnable_steps = []

    for step in plan.steps:
        if step.status != "pending":
            continue
        if all_dependencies_completed(step, plan.steps):
            runnable_steps.append(step)

    if not runnable_steps:
        return None

    # Priority order
    priority_order = [
        "read_context",
        "repo_search",
        "patch",
        "test",
        "review",
        "browser",
        "connector",
        "approval",
        "summarize"
    ]

    runnable_steps.sort(key=lambda s: priority_order.index(s.kind))
    return runnable_steps[0]
```

## 7.2 Optional parallel step selection

Kalau task cukup besar, planner bisa menandai step untuk diparalelkan via sub-agents, tapi parent tetap memproses hasilnya dalam loop yang sama.

---

# 8. Step execution layer

## 8.1 Generic step executor

```python id="ahuvrq"
def execute_step(task, step):
    mark_step_running(step)
    emit_telemetry("step_started", {"task_id": task.task_id, "step_id": step.step_id})

    try:
        if step.kind == "read_context":
            result = execute_read_context_step(task, step)
        elif step.kind == "repo_search":
            result = execute_repo_search_step(task, step)
        elif step.kind == "patch":
            result = execute_patch_step(task, step)
        elif step.kind == "test":
            result = execute_test_step(task, step)
        elif step.kind == "review":
            result = execute_review_step(task, step)
        elif step.kind == "browser":
            result = execute_browser_step(task, step)
        elif step.kind == "connector":
            result = execute_connector_step(task, step)
        elif step.kind == "approval":
            result = execute_approval_step(task, step)
        elif step.kind == "summarize":
            result = execute_summary_step(task, step)
        else:
            raise UnsupportedStep(step.kind)

        return normalize_step_result(result, task, step)

    except Exception as exc:
        normalized_error = normalize_exception(exc, task, step)
        return StepResult.failed(normalized_error)
```

---

# 9. Patch execution loop

Patch step adalah salah satu bagian terpenting.

## 9.1 Patch step pseudocode

```python id="c7maej"
def execute_patch_step(task, step):
    patch_mode = choose_patch_mode(task)

    coder_role = "coder_fast" if task.complexity in ["tiny", "small"] else "coder_strong"

    coder_input = build_coder_input(task, step)

    patch_candidate = run_reasoning_role(coder_role, coder_input)

    if patch_mode == "inline":
        tool_result = call_tool("patch.inline", {
            "changes": patch_candidate.changes
        })
    elif patch_mode == "structured":
        tool_result = call_tool("patch.structured", {
            "patch": patch_candidate.patch
        })
    else:
        tool_result = call_tool("exec", {
            "command": patch_candidate.shell_edit_script
        })

    if tool_result.status == "failed":
        return StepResult.failed(tool_result_to_error(tool_result))

    syntax_check = run_minimal_syntax_validation(task)

    if syntax_check.status == "failed":
        return StepResult.failed(tool_result_to_error(syntax_check))

    artifacts = collect_patch_artifacts(task, patch_candidate, tool_result)

    return StepResult.completed(
        structured_output={
            "changed_files": patch_candidate.changed_files,
            "patch_mode": patch_mode
        },
        artifact_refs=artifacts
    )
```

## 9.2 Patch mode chooser

```python id="95e9gb"
def choose_patch_mode(task):
    failed_patch_modes = get_failed_patch_modes(task)

    if "inline" not in failed_patch_modes and task.complexity in ["tiny", "small"]:
        return "inline"

    if "structured" not in failed_patch_modes:
        return "structured"

    return "branch_patch"
```

---

# 10. Test execution loop

## 10.1 Test step pseudocode

```python id="c2g8zq"
def execute_test_step(task, step):
    test_scope = choose_test_scope(task, step)

    if test_scope == "targeted":
        tool_result = call_tool("test.run_targeted", {
            "targets": task.context_bundle["related_tests"]
        })
    elif test_scope == "package":
        tool_result = call_tool("test.run_package_subset", {
            "files": task.session_memory.file_shortlist
        })
    else:
        tool_result = call_tool("test.run_broader_subset", {
            "goal": task.goal
        })

    if tool_result.status == "completed":
        return StepResult.completed(
            structured_output=tool_result.structured_output,
            artifact_refs=tool_result.artifact_refs
        )

    failure_bundle = build_failure_bundle(tool_result, task)

    return StepResult.failed(
        error=NormalizedError(
            error_class=classify_test_failure(tool_result),
            error_family="verification_failure",
            severity="medium",
            retryable=is_test_failure_retryable(tool_result),
            source=build_error_source(task, step),
            message="Verification failed",
            structured_context={"failure_bundle_id": failure_bundle.bundle_id},
            artifact_refs=failure_bundle.log_excerpt_refs,
            recommended_next_action="invoke_verifier"
        )
    )
```

## 10.2 Test scope chooser

```python id="ypg8jm"
def choose_test_scope(task, step):
    if not has_completed_test_scope(task, "targeted"):
        return "targeted"

    if task.complexity in ["medium", "large"] and not has_completed_test_scope(task, "package"):
        return "package"

    return "broader"
```

---

# 11. Verifier loop

Verifier dipanggil bukan untuk “berpikir lebih lama”, tetapi untuk **meringkas kegagalan menjadi arah tindakan**.

## 11.1 Verifier invocation

```python id="gp3mrn"
def invoke_verifier(task, failure_bundle):
    verifier_input = {
        "goal": task.goal,
        "failure_bundle": failure_bundle,
        "attempted_actions": summarize_recent_attempts(task),
        "file_shortlist": task.session_memory.file_shortlist,
    }

    verifier_output = run_reasoning_role("verifier", verifier_input)

    return {
        "recommended_action": verifier_output.recommended_action,
        "suspected_files": verifier_output.suspected_files,
        "root_cause_hypotheses": verifier_output.root_cause_hypotheses,
        "confidence": verifier_output.confidence
    }
```

## 11.2 Verifier outcomes

Verifier hanya boleh mengembalikan action kelas ini:

* `patch_source`
* `patch_test`
* `rerun_flaky`
* `repair_env`
* `replan`
* `ask_for_approval`
* `fail_task`

Itu menjaga verifier tetap sempit dan berguna.

---

# 12. Review step

## 12.1 Review step pseudocode

```python id="vyhi5b"
def execute_review_step(task, step):
    review_input = {
        "goal": task.goal,
        "changed_files": get_changed_files(task),
        "diff_ref": get_latest_diff_artifact(task),
        "risk_tier": task.risk_tier,
        "critical_paths": task.context_bundle.get("critical_paths", []),
    }

    reviewer_output = run_reasoning_role("reviewer", review_input)

    artifact_ref = persist_review_report(task, reviewer_output)

    if reviewer_output.has_high_severity_blocker:
        return StepResult.failed(
            error=NormalizedError(
                error_class="review_blocker",
                error_family="task_logic",
                severity="high",
                retryable=False,
                source=build_error_source(task, step),
                message="Reviewer found blocking issue",
                artifact_refs=[artifact_ref],
                recommended_next_action="planner_replan"
            )
        )

    return StepResult.completed(
        structured_output={
            "risk_summary": reviewer_output.risk_summary,
            "review_notes": reviewer_output.review_notes
        },
        artifact_refs=[artifact_ref]
    )
```

---

# 13. Browser execution loop

Browser tasks harus selalu berjalan dengan detect-act-verify.

## 13.1 Browser step pseudocode

```python id="llv2ic"
def execute_browser_step(task, step):
    snapshot = call_tool("browser.snapshot", {})
    if snapshot.status == "failed":
        return StepResult.failed(tool_result_to_error(snapshot))

    ui_input = {
        "goal": step.name,
        "snapshot_ref": snapshot.artifact_refs[0],
        "current_url": snapshot.structured_output.get("url"),
        "state_summary": snapshot.structured_output.get("state_summary")
    }

    ui_plan = run_reasoning_role("ui_agent", ui_input)

    action_result = call_tool("browser.act", ui_plan.action)

    if action_result.status == "failed":
        return StepResult.failed(tool_result_to_error(action_result))

    verify_result = call_tool("browser.verify", ui_plan.expected_postcondition)

    if verify_result.status == "failed":
        return StepResult.failed(tool_result_to_error(verify_result))

    return StepResult.completed(
        structured_output={
            "browser_action": ui_plan.action,
            "verified": True
        },
        artifact_refs=snapshot.artifact_refs + action_result.artifact_refs + verify_result.artifact_refs
    )
```

---

# 14. Connector execution loop

## 14.1 Connector step pseudocode

```python id="qiqm2g"
def execute_connector_step(task, step):
    proposed_action = build_connector_action(task, step)

    if requires_approval(proposed_action, task):
        approval = create_approval_request(task, proposed_action)
        return StepResult.failed(
            error=NormalizedError(
                error_class="approval_required",
                error_family="policy",
                severity="medium",
                retryable=False,
                source=build_error_source(task, step),
                message="Connector write requires approval",
                structured_context={"approval_id": approval.approval_id},
                recommended_next_action="await_approval"
            )
        )

    connector_result = call_tool("connector.execute", proposed_action)

    if connector_result.status == "failed":
        return StepResult.failed(tool_result_to_error(connector_result))

    postcondition = verify_connector_postcondition(task, proposed_action, connector_result)

    if postcondition == "unknown":
        return StepResult.failed(
            error=NormalizedError(
                error_class="connector_partial_write_unknown",
                error_family="connector_failure",
                severity="critical",
                retryable=False,
                source=build_error_source(task, step),
                message="Connector side effect uncertain",
                artifact_refs=connector_result.artifact_refs,
                recommended_next_action="fetch_external_state_before_retry"
            )
        )

    return StepResult.completed(
        structured_output=connector_result.structured_output,
        artifact_refs=connector_result.artifact_refs
    )
```

---

# 15. Failed-step handler

Ini bagian yang menyatukan error taxonomy dengan retry engine.

## 15.1 Failed-step handler pseudocode

```python id="sgipvr"
def handle_failed_step(task, step, step_result):
    error = step_result.error
    persist_error(task, error)
    emit_telemetry("step_failed", serialize_error(error))

    if error.error_class == "approval_required":
        set_task_status(task, "BLOCKED")
        return "blocked"

    if error.error_class in ["unsafe_action_blocked", "permission_denied"]:
        set_task_status(task, "BLOCKED")
        return "blocked"

    if should_retry(task, step, error):
        retry_strategy = pick_retry_strategy(task, step, error)
        apply_retry_strategy(task, step, retry_strategy)
        set_task_status(task, "RETRYING")
        save_checkpoint(task, reason=f"retry_{error.error_class}")
        return "retry"

    if should_replan(task, error):
        set_task_status(task, "PLANNED")
        save_checkpoint(task, reason=f"replan_{error.error_class}")
        return "replanned"

    return "failed"
```

---

## 15.2 Retry strategy picker

```python id="ezjjl2"
def pick_retry_strategy(task, step, error):
    if error.error_class == "invalid_patch":
        return "switch_patch_mode"

    if error.error_class == "tool_timeout":
        return "retry_same_tool_once"

    if error.error_class in ["stale_ref", "stale_state"]:
        return "browser_resnapshot_and_replan"

    if error.error_class == "context_overflow":
        return "summarize_and_trim_context"

    if error.error_class == "invalid_structured_response":
        return "schema_repair_then_fallback_model"

    if error.error_class == "flaky_env":
        return "rerun_with_flaky_tag"

    return "no_retry"
```

---

# 16. Successful-step handler

## 16.1 Success handling pseudocode

```python id="jx4z2u"
def handle_successful_step(task, step, step_result):
    mark_step_completed(step)
    attach_artifacts(task, step_result.artifact_refs)
    update_session_memory(task, step, step_result)
    update_confidence(task, step_result)
    emit_telemetry("step_completed", {
        "task_id": task.task_id,
        "step_id": step.step_id
    })

    if checkpoint_needed_after_success(task, step):
        save_checkpoint(task, reason=f"post_{step.kind}")

    if should_transition_to_verify(task, step):
        set_task_status(task, "VERIFYING")
    elif should_transition_to_ready_for_review(task, step):
        set_task_status(task, "READY_FOR_REVIEW")
    else:
        set_task_status(task, "EXECUTING")
```

---

# 17. Partial-step handler

Kadang sebuah step tidak gagal total, tetapi belum final.

Contoh:

* repo scan selesai, tapi butuh satu sub-scan lagi
* browser action berhasil, tapi postcondition ambigu
* test subset selesai, broader verification belum

```python id="6mn4cr"
def handle_partial_step(task, step, step_result):
    update_session_memory(task, step, step_result)
    attach_artifacts(task, step_result.artifact_refs)
    mark_step_partial(step)

    if step_result.next_recommended_action == "continue_same_step":
        queue_followup_for_same_step(task, step)

    elif step_result.next_recommended_action == "invoke_verifier":
        enqueue_verifier_step(task)

    elif step_result.next_recommended_action == "request_approval":
        create_approval_request_from_step(task, step, step_result)

    save_checkpoint(task, reason=f"partial_{step.kind}")
```

---

# 18. Approval blocking + resume

## 18.1 Blocking behavior

```python id="27fean"
def block_for_approval(task):
    set_task_status(task, "BLOCKED")
    save_checkpoint(task, reason="awaiting_approval")
    emit_telemetry("task_blocked_for_approval", {"task_id": task.task_id})
    return finalize_intermediate(task)
```

## 18.2 Resume after approval

```python id="sdxbxj"
def resume_task_from_checkpoint(task_id, approval_resolution=None):
    task = load_task(task_id)
    checkpoint = load_latest_checkpoint(task_id)

    if approval_resolution:
        apply_approval_resolution(task, approval_resolution)

    restore_workspace_snapshot(checkpoint.workspace_snapshot_id)
    restore_session_memory(task)
    restore_plan_state(task)

    set_task_status(task, "EXECUTING")
    emit_telemetry("task_resumed", {"task_id": task_id})

    return continue_execution_loop(task)
```

---

# 19. Budget exhaustion handler

## 19.1 Budget exhausted pseudocode

```python id="vd1wjo"
def handle_budget_exhausted(task):
    error = NormalizedError(
        error_id=generate_id("err"),
        timestamp=now(),
        scope="task_logic",
        error_class="budget_exhausted",
        error_family="task_logic",
        severity="medium",
        retryable=False,
        source={
            "component": "execution_loop",
            "task_id": task.task_id
        },
        message="Task budget exhausted before completion",
        structured_context={
            "budget": serialize_budget(task.budget),
            "usage": current_budget_usage(task)
        },
        artifact_refs=get_current_artifacts(task),
        recommended_next_action="checkpoint_and_stop",
        escalate_if_repeated=False
    )

    persist_error(task, error)
    save_checkpoint(task, reason="budget_exhausted")
    set_task_status(task, "FAILED")
    return finalize_task(task)
```

---

# 20. Finalization logic

## 20.1 Attempt finalize

```python id="jlwmxa"
def attempt_finalize(task):
    if has_pending_blockers(task):
        set_task_status(task, "BLOCKED")
        return finalize_intermediate(task)

    if success_criteria_met(task):
        set_task_status(task, "COMPLETED")
        return finalize_task(task)

    set_task_status(task, "FAILED")
    return finalize_task(task)
```

## 20.2 Finalize task

```python id="xbyvn9"
def finalize_task(task):
    summary_artifact = generate_final_summary(task)
    persist_artifact(summary_artifact)

    compact_session_memory(task)
    update_repo_memory_if_needed(task)
    emit_telemetry("task_finalized", {
        "task_id": task.task_id,
        "status": task.status
    })

    return {
        "task_id": task.task_id,
        "status": task.status,
        "artifacts": get_current_artifacts(task),
        "summary_ref": summary_artifact.artifact_id
    }
```

---

# 21. Sub-agent integration ke loop utama

Sub-agents tidak boleh membuat parent loop kacau.

## 21.1 Spawn policy

```python id="ipxw5x"
def maybe_spawn_subagents(task):
    for spec in task.routing.subagent_plan:
        if spec.enabled and not subagent_already_running(task, spec.role):
            launch_subagent(task, spec)
```

## 21.2 Collect outputs

```python id="zkk6tq"
def collect_subagent_outputs(task):
    outputs = load_completed_subagent_outputs(task.task_id)

    for output in outputs:
        attach_artifacts(task, output.artifacts)
        merge_subagent_summary_into_session_memory(task, output)
        emit_telemetry("subagent_completed", {
            "task_id": task.task_id,
            "subagent_id": output.subagent_id
        })
```

Sub-agent output tidak langsung mengubah task state tanpa lewat parent validation.

---

# 22. Execution loop invariants

Agar runtime tetap sehat, saya sarankan invariant berikut selalu dijaga.

## 22.1 Invariant state

Task tidak boleh berada di dua state sekaligus.

## 22.2 Invariant artifact

Setiap step sukses yang penting harus punya setidaknya satu artifact atau structured output.

## 22.3 Invariant policy

Tidak ada connector write sensitif tanpa approval object atau explicit policy allowance.

## 22.4 Invariant retry

Retry count harus bertambah monoton dan dicek terhadap retry cap.

## 22.5 Invariant checkpoint

Semua transition besar harus checkpointable.

## 22.6 Invariant memory

Session memory hanya menyimpan ringkasan terstruktur, bukan transcript mentah.

---

# 23. Runtime loop yang lebih realistis: event-driven variant

Kalau Anda ingin sistem ini scalable, execution loop pada akhirnya sebaiknya event-driven.

## 23.1 Event types

* `TASK_CREATED`
* `ROUTING_COMPLETED`
* `PLAN_READY`
* `CONTEXT_READY`
* `STEP_STARTED`
* `STEP_COMPLETED`
* `STEP_FAILED`
* `APPROVAL_REQUESTED`
* `APPROVAL_RESOLVED`
* `CHECKPOINT_SAVED`
* `TASK_RESUMED`
* `TASK_COMPLETED`
* `TASK_FAILED`

## 23.2 Keuntungan

* lebih mudah diskalakan
* cocok untuk background tasks
* bisa di-observe
* sub-agent orchestration lebih rapi
* retries dan delayed resume lebih mudah

Tetapi untuk v1, loop sinkron seperti pseudocode di atas masih cukup.

---

# 24. Degraded mode execution

Execution loop juga perlu mode degradasi saat sistem tidak ideal.

## 24.1 Contoh degraded modes

* provider utama rate-limited
* browser runtime sedang bermasalah
* repo indexer lambat/down
* connector write path unstable

## 24.2 Behavior

* nonaktifkan sub-agent opsional
* pakai context lebih sempit
* hanya izinkan read-only connector actions
* skip broad review scan
* fail-fast lebih cepat pada high-risk tasks

Ini penting agar sistem tetap usable, walau tidak optimal.

---

# 25. UI-facing status mapping

Agar pengguna merasa sistem “hidup” dan terkontrol, map state internal ke UI status yang sederhana.

| Internal state     | UI label                          |
| ------------------ | --------------------------------- |
| `SCOPING`          | Memahami task                     |
| `PLANNED`          | Menyusun rencana                  |
| `CONTEXT_READY`    | Menyiapkan konteks                |
| `EXECUTING`        | Mengerjakan                       |
| `VERIFYING`        | Memverifikasi hasil               |
| `BLOCKED`          | Menunggu persetujuan / dependensi |
| `RETRYING`         | Memulihkan kegagalan              |
| `READY_FOR_REVIEW` | Siap ditinjau                     |
| `COMPLETED`        | Selesai                           |
| `FAILED`           | Gagal dengan ringkasan            |

Dengan ini UI tidak perlu membocorkan detail internal yang terlalu kasar.

---

# 26. Minimal implementation slice untuk E

Kalau ingin membangun E secara bertahap, saya sarankan urutannya:

## Slice 1

* top-level execution loop
* task state machine
* success/failure handling
* checkpoint save/load

## Slice 2

* planning + context preparation
* patch step + test step
* invalid_patch recovery
* failure bundle creation

## Slice 3

* approval block/resume
* reviewer integration
* verifier integration
* retry engine class-aware

## Slice 4

* browser loop
* connector post-condition verification
* sub-agent orchestration

---

# 27. Ringkasan inti E

Kalau saya padatkan, orchestration engine ini berdiri di atas 10 keputusan:

1. **task selalu melewati state machine yang eksplisit**
2. **routing dilakukan di awal dan bisa diulang setelah step penting**
3. **plan divalidasi sebelum dieksekusi**
4. **context disiapkan dan dipadatkan sebelum reasoning**
5. **step execution dipisahkan berdasarkan kind**
6. **patch dan test punya loop khusus**
7. **failure selalu dinormalisasi sebelum recovery**
8. **approval memblok dengan rapi, bukan menggantung**
9. **checkpoint adalah bagian inti, bukan opsional**
10. **finalisasi selalu menghasilkan summary + artifact + memory update**

---

# 28. Apa yang sekarang sudah kita punya

Dengan A + B + D + E, sekarang fondasi desain Anda sudah mencakup:

* **router**
* **task/object schemas**
* **error system**
* **retry matrix**
* **execution loop**
* **checkpoint/resume**
* **approval orchestration**
* **sub-agent integration**
* **browser/connector integration path**

Artinya kita sudah punya **control plane + runtime core specification** yang cukup matang untuk mulai diimplementasikan.

---



#### PerplexityComputerV2.part6.md

# F. API Contract Antar Komponen

## 1. Prinsip desain API internal

Sebelum masuk endpoint atau interface, ada beberapa prinsip penting.

## 1.1 Semua komponen berbicara lewat contract eksplisit

Jangan biarkan komponen saling kirim object liar atau dict bebas tanpa schema tetap.

## 1.2 Pisahkan command vs query

* **command**: mengubah state
* **query**: membaca state

Ini penting untuk auditability dan predictability.

## 1.3 Semua operasi penting harus punya idempotency strategy

Terutama:

* create task
* save checkpoint
* create approval
* connector write
* artifact persist
* sub-agent launch

## 1.4 Semua response penting harus punya metadata operasional

Minimal:

* `request_id`
* `status`
* `error_class`
* `duration_ms`

## 1.5 Internal API harus failure-aware

Jangan hanya `200 OK + payload`. Harus ada hasil yang bisa dinormalisasi ke error taxonomy.

---

# 2. Daftar komponen dan peran

## 2.1 Router Service

Input task intake, output routing decision.

## 2.2 Task Service

Sumber kebenaran untuk task object, lifecycle, dan state transitions.

## 2.3 Planning Service

Membangun `TaskPlan` dari task + context.

## 2.4 Context Service

Mempersiapkan shortlist files, related tests, repo summaries, failure bundles.

## 2.5 Checkpoint Service

Menyimpan dan memulihkan checkpoint task.

## 2.6 Memory Service

Menyimpan session/repo/team/operational memory.

## 2.7 Artifact Service

Menyimpan diff, logs, screenshots, summaries, review reports, dll.

## 2.8 Approval Service

Mengelola approval requests dan resolutions.

## 2.9 Sub-agent Service

Spawn, monitor, collect, dan cancel sub-agent runs.

## 2.10 Tool Adapter Service

Abstraksi semua tool invocation.

## 2.11 Connector Service

Akses ke external apps dengan side-effect awareness.

## 2.12 Telemetry Service

Event ingest dan metric/log export.

---

# 3. Common envelope schema

Saya sarankan semua internal response memakai envelope yang seragam.

## 3.1 Response envelope

```json id="1w4fjz"
{
  "request_id": "req_123",
  "status": "ok",
  "duration_ms": 42,
  "data": {},
  "error": null
}
```

### Jika gagal:

```json id="fiwdpl"
{
  "request_id": "req_124",
  "status": "error",
  "duration_ms": 31,
  "data": null,
  "error": {
    "error_class": "permission_denied",
    "message": "Workspace is read-only",
    "retryable": false
  }
}
```

## 3.2 Status enum

* `ok`
* `partial`
* `error`

---

# 4. Router API

Router bertanggung jawab memberi keputusan awal dan keputusan ulang setelah step penting.

## 4.1 `POST /router/route`

### Request

```json id="yv28bo"
{
  "task_intake": {
    "goal": "Fix failing login tests after auth middleware refactor",
    "constraints": [
      "Do not change production auth contract"
    ],
    "workspace": {
      "repo_id": "repo_abc",
      "workspace_id": "ws_1"
    },
    "team_id": "team_1",
    "issue_context": {
      "issue_id": "ISSUE-42",
      "priority": "high"
    }
  },
  "repo_memory_ref": "repo_mem_abc",
  "team_memory_ref": "team_mem_1",
  "operational_memory_ref": "op_mem_current"
}
```

### Response

```json id="08xw5x"
{
  "request_id": "req_route_1",
  "status": "ok",
  "duration_ms": 85,
  "data": {
    "task_class": "test_repair",
    "complexity": "small",
    "risk_tier": "moderate",
    "roles": ["retriever", "verifier", "coder_fast"],
    "tool_plan": [
      "repo.rank_files_for_task",
      "repo.related_tests",
      "patch.inline",
      "test.run_targeted"
    ],
    "subagent_plan": [],
    "approval_mode": "standard",
    "budget": {
      "max_model_calls": 12,
      "max_tool_calls": 20,
      "max_wall_clock_minutes": 15
    },
    "checkpoint_policy": {
      "enabled": true,
      "save_on_states": ["PLANNED", "CONTEXT_READY", "READY_FOR_REVIEW"]
    },
    "recovery_policy": {
      "patch_mode_fallback_enabled": true,
      "planner_escalation_after_patch_failures": 2
    }
  }
}
```

## 4.2 `POST /router/reroute`

Dipakai setelah step gagal/sukses penting.

### Request

```json id="eb6q2m"
{
  "task_id": "task_123",
  "current_state": "VERIFYING",
  "latest_step_result_ref": "step_result_45",
  "error_ref": "err_12"
}
```

### Response

```json id="xyom8d"
{
  "request_id": "req_reroute_1",
  "status": "ok",
  "duration_ms": 26,
  "data": {
    "decision": "switch_patch_mode",
    "updated_roles": ["verifier", "coder_fast"],
    "updated_tool_plan": ["patch.structured", "test.run_targeted"]
  }
}
```

---

# 5. Task Service API

Task service adalah source of truth lifecycle.

## 5.1 `POST /tasks`

Membuat task baru.

### Request

```json id="i78asb"
{
  "goal": "Investigate CI failure in auth pipeline",
  "constraints": ["No production changes"],
  "workspace": {
    "repo_id": "repo_abc",
    "workspace_id": "ws_1"
  },
  "team_id": "team_1"
}
```

### Response

```json id="7z578y"
{
  "request_id": "req_task_create_1",
  "status": "ok",
  "duration_ms": 14,
  "data": {
    "task_id": "task_123",
    "status": "CREATED",
    "created_at": "2026-04-05T10:00:00Z"
  }
}
```

## 5.2 `GET /tasks/{task_id}`

Mengambil task terbaru.

## 5.3 `POST /tasks/{task_id}/transition`

Mengubah state task.

### Request

```json id="sfp2wc"
{
  "from_status": "PLANNED",
  "to_status": "CONTEXT_READY",
  "reason": "Initial repo context prepared"
}
```

### Response

Mengembalikan task terbaru.

## 5.4 `POST /tasks/{task_id}/attach-plan`

Menempelkan `TaskPlan`.

## 5.5 `POST /tasks/{task_id}/attach-artifacts`

Menambahkan artifact refs.

## 5.6 `POST /tasks/{task_id}/set-routing`

Menyimpan routing decision.

---

# 6. Planning Service API

## 6.1 `POST /planning/build`

### Request

```json id="2qznfd"
{
  "task_ref": "task_123",
  "routing_ref": "route_123",
  "repo_memory_ref": "repo_mem_abc",
  "team_memory_ref": "team_mem_1",
  "context_hints": {
    "critical_paths": ["auth/", "billing/"]
  }
}
```

### Response

```json id="7xq1u2"
{
  "request_id": "req_plan_1",
  "status": "ok",
  "duration_ms": 1400,
  "data": {
    "plan": {
      "plan_id": "plan_123",
      "summary": "Inspect login-related auth files, patch cookie/session handling, rerun targeted tests, then run package subset",
      "steps": [
        {
          "step_id": "s1",
          "name": "Rank relevant auth files",
          "kind": "repo_search",
          "status": "pending"
        },
        {
          "step_id": "s2",
          "name": "Patch login cookie handling",
          "kind": "patch",
          "status": "pending",
          "depends_on": ["s1"]
        },
        {
          "step_id": "s3",
          "name": "Run targeted login tests",
          "kind": "test",
          "status": "pending",
          "depends_on": ["s2"]
        }
      ],
      "success_criteria": [
        "Patch applied successfully",
        "Targeted tests pass"
      ],
      "confidence": 0.79
    }
  }
}
```

## 6.2 `POST /planning/rebuild`

Digunakan saat `invalid_plan`, `scope_too_broad`, atau patch failures berulang.

---

# 7. Context Service API

Context service mengubah repo/task history menjadi working set yang kecil dan relevan.

## 7.1 `POST /context/prepare`

### Request

```json id="56v1sa"
{
  "task_ref": "task_123",
  "plan_ref": "plan_123",
  "repo_memory_ref": "repo_mem_abc"
}
```

### Response

```json id="x4wpxd"
{
  "request_id": "req_ctx_1",
  "status": "ok",
  "duration_ms": 210,
  "data": {
    "context_bundle": {
      "bundle_id": "ctx_123",
      "file_shortlist": [
        "auth/middleware.ts",
        "auth/session.ts",
        "tests/login.spec.ts"
      ],
      "related_tests": [
        "tests/login.spec.ts"
      ],
      "repo_summary_ref": "artifact://repo_summary.md",
      "critical_paths": ["auth/"]
    }
  }
}
```

## 7.2 `POST /context/compact`

Dipakai saat `context_overflow`.

### Request

```json id="3nrj84"
{
  "task_ref": "task_123",
  "reason": "context_overflow",
  "current_context_ref": "ctx_123"
}
```

### Response

Mengembalikan working set yang lebih kecil:

* shortlisted files baru
* summary attempts
* trimmed logs
* failure bundle refs

---

# 8. Checkpoint Service API

## 8.1 `POST /checkpoints`

### Request

```json id="15vtsr"
{
  "task_ref": "task_123",
  "task_status": "VERIFYING",
  "last_successful_step": "s2",
  "plan_summary": "Patch applied; targeted verification ongoing",
  "file_scope": [
    "auth/session.ts",
    "tests/login.spec.ts"
  ],
  "artifacts": [
    "artifact://diff.patch"
  ],
  "pending_approvals": [],
  "retry_count": 1,
  "confidence_snapshot": 0.74,
  "resume_instruction": "Resume at targeted test verification",
  "next_recommended_action": "run_targeted_tests",
  "workspace_snapshot_id": "snap_21"
}
```

### Response

```json id="xz5epa"
{
  "request_id": "req_ckpt_1",
  "status": "ok",
  "duration_ms": 18,
  "data": {
    "checkpoint_id": "ckpt_123"
  }
}
```

## 8.2 `GET /checkpoints/latest?task_id=...`

Mengambil checkpoint terbaru.

## 8.3 `POST /checkpoints/restore`

### Request

```json id="5sntzw"
{
  "task_id": "task_123",
  "checkpoint_id": "ckpt_123"
}
```

### Response

Mengembalikan:

* task snapshot
* workspace snapshot ref
* session memory ref
* resume instruction

---

# 9. Memory Service API

Memory dipisah menjadi session, repo, team, operational.

## 9.1 `GET /memory/session?task_id=...`

Ambil session memory.

## 9.2 `POST /memory/session/update`

### Request

```json id="f1aqk6"
{
  "task_id": "task_123",
  "objective": "Fix login tests",
  "append_attempted_action": {
    "step": "patch",
    "summary": "Adjusted session cookie persistence logic",
    "result": "succeeded",
    "artifact_refs": ["artifact://diff.patch"]
  },
  "update_current_plan": [
    "Run targeted login tests",
    "If green, run package subset"
  ],
  "confidence": 0.78
}
```

## 9.3 `GET /memory/repo?repo_id=...`

## 9.4 `POST /memory/repo/update`

Contoh:

* tambah build commands
* tambah flaky test record
* update dangerous modules

## 9.5 `GET /memory/team?team_id=...`

## 9.6 `GET /memory/operational/current`

## 9.7 `POST /memory/operational/report`

Untuk melaporkan:

* provider incident
* broken tool pattern
* env signal

---

# 10. Artifact Service API

Artifact service harus bisa menangani file dan metadata.

## 10.1 `POST /artifacts`

### Request

```json id="b2plcw"
{
  "task_id": "task_123",
  "type": "diff",
  "producer_step": "s2",
  "mime_type": "text/x-diff",
  "retention_class": "standard",
  "content_ref": "storage://tmp/diff_1.patch",
  "metadata": {
    "changed_files": [
      "auth/session.ts",
      "tests/login.spec.ts"
    ]
  }
}
```

### Response

```json id="8v7moc"
{
  "request_id": "req_art_1",
  "status": "ok",
  "duration_ms": 9,
  "data": {
    "artifact_id": "artifact_123",
    "uri": "artifact://diff.patch"
  }
}
```

## 10.2 `GET /artifacts/{artifact_id}`

## 10.3 `POST /artifacts/batch`

Untuk upload banyak logs/screenshots sekaligus.

## 10.4 `POST /artifacts/promote-retention`

Contoh:

* dari `short` ke `audit`
* berguna untuk approval-sensitive atau incident tasks

---

# 11. Approval Service API

## 11.1 `POST /approvals`

### Request

```json id="4a6jlc"
{
  "task_id": "task_123",
  "action": "create_draft_pr",
  "reason": "Patch is ready and targeted tests passed",
  "risk_summary": [
    "Touches auth/session.ts",
    "No migration files changed"
  ],
  "artifact_refs": [
    "artifact://diff.patch",
    "artifact://test_summary.md"
  ],
  "confidence": 0.79
}
```

### Response

```json id="twpufi"
{
  "request_id": "req_appr_1",
  "status": "ok",
  "duration_ms": 7,
  "data": {
    "approval_id": "appr_123",
    "status": "pending"
  }
}
```

## 11.2 `GET /approvals/{approval_id}`

## 11.3 `POST /approvals/{approval_id}/resolve`

### Request

```json id="cjlwm0"
{
  "resolution": "approved",
  "resolved_by": "user_1"
}
```

### Response

Mengembalikan approval object terbaru.

## 11.4 `GET /approvals/pending?task_id=...`

---

# 12. Sub-agent Service API

## 12.1 `POST /subagents`

### Request

```json id="jpbzy8"
{
  "task_id": "task_123",
  "role": "scout",
  "objective": "Identify likely auth files and related tests",
  "file_scope": ["auth/", "tests/"],
  "tool_scope": ["repo.search_symbols", "repo.related_tests"],
  "budget": {
    "max_model_calls": 4,
    "max_tool_calls": 8,
    "timeout_minutes": 5
  },
  "stop_condition": "file_shortlist_ready"
}
```

### Response

```json id="u6pnqb"
{
  "request_id": "req_sub_1",
  "status": "ok",
  "duration_ms": 12,
  "data": {
    "subagent_id": "sa_123",
    "status": "created"
  }
}
```

## 12.2 `GET /subagents/{subagent_id}`

## 12.3 `GET /subagents?task_id=...`

## 12.4 `POST /subagents/{subagent_id}/cancel`

## 12.5 `POST /subagents/{subagent_id}/collect`

Mengembalikan:

* summary
* artifacts
* confidence
* blockers
* recommended next action

---

# 13. Tool Adapter API

Ini contract paling kritis untuk reliability.

## 13.1 `POST /tools/invoke`

### Request

```json id="0xm8cj"
{
  "tool_name": "test.run_targeted",
  "task_id": "task_123",
  "step_id": "s3",
  "inputs": {
    "targets": ["tests/login.spec.ts"]
  },
  "timeout_ms": 120000,
  "idempotency_key": "task_123_s3_attempt_1"
}
```

### Response

```json id="p7zn1r"
{
  "request_id": "req_tool_1",
  "status": "ok",
  "duration_ms": 14230,
  "data": {
    "tool_name": "test.run_targeted",
    "status": "failed",
    "duration_ms": 14230,
    "retryable": true,
    "error_class": "assertion_failure",
    "structured_output": {
      "tests_run": 4,
      "tests_failed": 1,
      "failed_tests": [
        "tests/login.spec.ts::login cookie persists"
      ]
    },
    "artifact_refs": [
      "artifact://logs/targeted_test.log"
    ],
    "confidence": 0.93,
    "side_effects": [],
    "next_recommended_action": "invoke_verifier"
  }
}
```

## 13.2 Rules untuk Tool Adapter API

* semua tool invocation harus mengembalikan `ToolResult`
* tidak boleh ada tool custom yang keluar dari schema ini
* side effects wajib diisi
* error_class wajib baku jika gagal

---

# 14. Connector Service API

Connector service bisa dibangun di atas tool adapter, tapi saya sarankan tetap punya contract khusus.

## 14.1 `POST /connectors/execute`

### Request

```json id="7fqq46"
{
  "task_id": "task_123",
  "step_id": "s7",
  "connector": "github",
  "action": "create_draft_pr",
  "payload": {
    "title": "Fix login cookie persistence",
    "body_ref": "artifact://pr_body.md",
    "branch": "task/login-cookie-fix"
  },
  "approval_ref": "appr_123",
  "idempotency_key": "github_pr_task_123"
}
```

### Response

```json id="armk2i"
{
  "request_id": "req_conn_1",
  "status": "ok",
  "duration_ms": 850,
  "data": {
    "connector": "github",
    "action": "create_draft_pr",
    "status": "completed",
    "retryable": false,
    "error_class": null,
    "structured_output": {
      "pr_number": 451,
      "url": "https://example/pr/451"
    },
    "artifact_refs": [
      "artifact://connector/github_pr_response.json"
    ],
    "side_effects": [
      "created_draft_pr:451"
    ],
    "postcondition_status": "verified"
  }
}
```

## 14.2 `POST /connectors/verify-postcondition`

Dipakai untuk kasus `connector_partial_write_unknown`.

---

# 15. Telemetry Service API

## 15.1 `POST /telemetry/events`

### Request

```json id="0przne"
{
  "events": [
    {
      "event_type": "STEP_FAILED",
      "timestamp": "2026-04-05T10:03:00Z",
      "task_id": "task_123",
      "step_id": "s3",
      "payload": {
        "error_class": "assertion_failure",
        "component": "test.run_targeted"
      }
    }
  ]
}
```

## 15.2 `POST /telemetry/metrics`

Untuk ingest metrik agregat.

## 15.3 `POST /telemetry/errors`

Bisa juga langsung ingest `NormalizedError`.

---

# 16. Reasoning role contract

Planner, coder, verifier, reviewer, ui-agent sebaiknya juga punya contract baku.

## 16.1 `POST /reasoning/run`

### Request

```json id="1yqvjx"
{
  "role": "verifier",
  "task_id": "task_123",
  "step_id": "s3",
  "input": {
    "goal": "Fix failing login tests",
    "failure_bundle_ref": "fb_123",
    "file_shortlist": [
      "auth/session.ts",
      "tests/login.spec.ts"
    ]
  },
  "output_schema": "VerifierOutputV1"
}
```

### Response

```json id="yphfqq"
{
  "request_id": "req_reason_1",
  "status": "ok",
  "duration_ms": 920,
  "data": {
    "role": "verifier",
    "output": {
      "recommended_action": "patch_source",
      "suspected_files": [
        "auth/session.ts"
      ],
      "root_cause_hypotheses": [
        "Cookie persistence logic changed after middleware refactor"
      ],
      "confidence": 0.81
    }
  }
}
```

## 16.2 Alasan ini penting

Kalau reasoning roles tidak punya schema output baku, orchestrator akan kembali terjebak parsing teks bebas.

---

# 17. Event bus contract

Kalau nanti sistem berubah menjadi event-driven, Anda perlu envelope event baku.

## 17.1 Event schema

```json id="9uxrjn"
{
  "event_id": "evt_123",
  "event_type": "STEP_COMPLETED",
  "timestamp": "2026-04-05T10:04:00Z",
  "task_id": "task_123",
  "step_id": "s2",
  "producer": "orchestrator",
  "payload": {
    "step_kind": "patch",
    "artifact_refs": [
      "artifact://diff.patch"
    ]
  }
}
```

## 17.2 Event types minimal

* `TASK_CREATED`
* `ROUTING_COMPLETED`
* `PLAN_READY`
* `CONTEXT_READY`
* `STEP_STARTED`
* `STEP_COMPLETED`
* `STEP_FAILED`
* `APPROVAL_REQUESTED`
* `APPROVAL_RESOLVED`
* `CHECKPOINT_SAVED`
* `TASK_RESUMED`
* `TASK_COMPLETED`
* `TASK_FAILED`

---

# 18. Idempotency contract

Ini sangat penting untuk write actions dan checkpointing.

## 18.1 Semua endpoint ini harus mendukung idempotency key

* `POST /tasks`
* `POST /checkpoints`
* `POST /approvals`
* `POST /subagents`
* `POST /tools/invoke`
* `POST /connectors/execute`
* `POST /artifacts`

## 18.2 Contract

Jika request yang sama datang dengan key yang sama:

* jangan buat side effect ganda
* kembalikan response lama atau marked replay

Contoh:

```json id="yieo0z"
{
  "request_id": "req_conn_1_replay",
  "status": "ok",
  "duration_ms": 3,
  "data": {
    "replayed": true,
    "original_request_id": "req_conn_1",
    "result_ref": "connector_result_1"
  }
}
```

---

# 19. Permission and policy contract

Komponen-komponen juga perlu contract policy.

## 19.1 `POST /policy/evaluate`

### Request

```json id="7s2uav"
{
  "task_id": "task_123",
  "action_type": "connector_write",
  "resource": "github.pull_request.create",
  "risk_context": {
    "risk_tier": "moderate",
    "touches_critical_paths": true
  }
}
```

### Response

```json id="ciz6vb"
{
  "request_id": "req_policy_1",
  "status": "ok",
  "duration_ms": 5,
  "data": {
    "decision": "require_approval",
    "reason": "Touches critical path and causes external side effect"
  }
}
```

Saya sangat sarankan policy engine dipisah, walau kecil.

---

# 20. Contract antar komponen dalam satu alur nyata

Mari lihat satu flow nyata:

## 20.1 Example flow: issue-to-PR

1. `POST /tasks`
2. `POST /router/route`
3. `POST /planning/build`
4. `POST /context/prepare`
5. `POST /tools/invoke` → `repo.rank_files_for_task`
6. `POST /reasoning/run` role=`coder_fast`
7. `POST /tools/invoke` → `patch.inline`
8. `POST /tools/invoke` → `test.run_targeted`
9. jika gagal → `POST /reasoning/run` role=`verifier`
10. jika sukses → `POST /reasoning/run` role=`reviewer`
11. `POST /approvals`
12. setelah approve → `POST /connectors/execute` create draft PR
13. `POST /checkpoints`
14. `POST /telemetry/events`

Dengan contract yang konsisten, seluruh flow ini bisa diobservasi, di-replay, dan di-hardening.

---

# 21. API compatibility guidance

Agar sistem tidak cepat hancur saat berkembang:

## 21.1 Gunakan versioning

Contoh:

* `TaskV1`
* `ToolResultV1`
* `VerifierOutputV1`

## 21.2 Jangan ubah field semantik diam-diam

Tambahkan field baru, jangan ubah arti field lama tanpa version bump.

## 21.3 Error contract harus backward-compatible

`error_class` lama tetap dikenali.

## 21.4 Internal schemas harus punya validator

Semua request/response penting wajib divalidasi sebelum dipakai runtime.

---

# 22. Minimal API surface untuk MVP

Kalau ingin cepat mulai, cukup implement 10 interface ini dulu:

1. `POST /tasks`
2. `GET /tasks/{id}`
3. `POST /router/route`
4. `POST /planning/build`
5. `POST /context/prepare`
6. `POST /tools/invoke`
7. `POST /checkpoints`
8. `POST /approvals`
9. `POST /reasoning/run`
10. `POST /telemetry/events`

Sepuluh ini sudah cukup untuk membuat orchestrator v1.

---

# 23. Anti-pattern yang harus dihindari

## 23.1 Jangan jadikan orchestrator langsung memanggil semua komponen tanpa contract

Kalau semua panggilan jadi ad hoc function calls, observability dan versioning akan buruk.

## 23.2 Jangan biarkan tool adapters mengembalikan shape yang beda-beda

Itu akan menghancurkan retry engine.

## 23.3 Jangan campur artifact metadata dengan task state

Pisahkan service dan schema.

## 23.4 Jangan biarkan reasoning role mengembalikan teks bebas

Harus ada output schema.

## 23.5 Jangan biarkan connector writes tanpa idempotency

Risiko duplicate side effect terlalu besar.

---

# 24. Ringkasan inti F

Kalau diringkas, API contract ini memastikan bahwa:

* **router memutuskan, bukan mengeksekusi**
* **task service menjadi source of truth lifecycle**
* **planning/context/reasoning tetap modular**
* **tools dan connectors tunduk pada schema hasil yang seragam**
* **approval, artifacts, checkpoints, dan telemetry adalah first-class**
* **seluruh sistem bisa di-debug, di-replay, dan di-versioning**

---

# 25. Apa yang sudah lengkap sekarang

Dengan A + B + D + E + F, desain Anda sekarang sudah punya fondasi yang cukup lengkap untuk:

* control plane
* state machine
* routing
* schemas
* retry logic
* execution loop
* checkpoint/resume
* API contracts
* approval orchestration
* telemetry integration

Ini sudah mendekati **engineering agent platform spec**, bukan sekadar ide produk.

---



#### PerplexityComputerV2.part7.md

# C. Definisi 7 Skill Inti

## Daftar skill inti

1. **Issue-to-PR**
2. **Bug Triage**
3. **Test Repair**
4. **Dependency Upgrade**
5. **Scoped Refactor**
6. **Code Review Assist**
7. **Incident Hotfix Assist**

---

# C.1 Skill 1 — Issue-to-PR

## 1. Tujuan

Mengubah issue/ticket/bug report menjadi:

* patch,
* verifikasi terarah,
* risk summary,
* dan PR draft artifacts.

Ini adalah skill paling penting karena paling dekat dengan outcome bisnis.

## 2. Kapan dipakai

Gunakan jika:

* ada issue/ticket jelas,
* targetnya adalah implementasi/fix,
* perubahan diharapkan berujung pada branch/diff/PR.

## 3. Input schema

```json id="v4wq6s"
{
  "skill_name": "issue_to_pr",
  "inputs": {
    "issue_text": "string",
    "issue_id": "string",
    "repo_id": "string",
    "workspace_id": "string",
    "constraints": ["string"],
    "branch_mode": "auto|existing|explicit",
    "target_branch": "string"
  }
}
```

## 4. Preflight checks

* repo tersedia
* workspace aktif
* build/test commands diketahui atau bisa dideteksi
* write access lokal ke workspace tersedia
* issue cukup spesifik untuk dieksekusi
* policy untuk patching mengizinkan local edits

## 5. Execution graph

1. read issue
2. classify task
3. rank files
4. related tests lookup
5. build plan
6. implement patch
7. run targeted tests
8. run package/broader checks jika perlu
9. run review scan
10. generate PR artifacts
11. request approval jika diperlukan
12. optionally create draft PR

## 6. Tool whitelist

* `repo.rank_files_for_task`
* `repo.related_tests`
* `repo.search_symbols`
* `patch.inline`
* `patch.structured`
* `exec`
* `test.run_targeted`
* `test.run_package_subset`
* `review.scan_regression`
* `review.scan_security`
* `connector.github.create_draft_pr`
* `artifact.persist`

## 7. Artifacts wajib

* diff/patch
* test summary
* risk summary
* implementation summary
* PR body draft
* changed files summary

## 8. Success criteria

* patch valid
* targeted tests pass
* tidak ada blocker severity tinggi dari reviewer
* PR artifacts lengkap

## 9. Failure / escalation rules

* dua patch failure → replan
* missing dependency → blocked/approval
* low confidence di critical path → reviewer mandatory
* draft PR create → approval jika policy butuh

## 10. Rollback behavior

* revert changes since last checkpoint
* restore workspace snapshot jika patch path buntu

## 11. User-facing outcome

“Patch siap ditinjau” atau “Draft PR siap dibuat/dibuka” dengan artifact lengkap.

---

# C.2 Skill 2 — Bug Triage

## 1. Tujuan

Mengubah gejala seperti:

* stack trace,
* CI error,
* runtime failure,
* log snippet,
  menjadi hipotesis akar masalah yang terurut, file kandidat, dan next action.

Skill ini fokus pada **diagnosis**, belum tentu langsung patch.

## 2. Kapan dipakai

Gunakan jika user bertanya:

* “kenapa ini gagal?”
* “apa root cause-nya?”
* “bug ini kemungkinan ada di mana?”
* “baca log ini dan jelaskan”

## 3. Input schema

```json id="q74hj8"
{
  "skill_name": "bug_triage",
  "inputs": {
    "problem_statement": "string",
    "log_refs": ["string"],
    "stack_trace": "string",
    "repo_id": "string",
    "workspace_id": "string",
    "related_issue_id": "string"
  }
}
```

## 4. Preflight checks

* failure evidence tersedia
* repo context tersedia bila perlu
* log/artifact dapat dibaca
* task scope bukan full fix by default

## 5. Execution graph

1. ingest logs/trace
2. normalize failure bundle
3. classify error
4. identify suspected files/modules
5. correlate dengan recent changes
6. rank root cause hypotheses
7. recommend next action:

   * patch source
   * patch test
   * rerun flaky
   * repair env
   * escalate

## 6. Tool whitelist

* `logs.parse_failure_bundle`
* `repo.rank_files_for_task`
* `repo.find_recent_changes`
* `repo.search_symbols`
* `review.scan_risk`
* `artifact.persist`

## 7. Artifacts wajib

* failure bundle
* root cause summary
* suspected files list
* next-action memo

## 8. Success criteria

* failure terklasifikasi
* minimal 1–3 hipotesis akar masalah terurut
* file kandidat tersedia
* next action jelas

## 9. Failure / escalation rules

* jika evidence terlalu minim → ambiguous_goal
* jika log terlalu besar → compact dulu
* jika failure kemungkinan env/flaky → jangan sarankan patch langsung

## 10. Rollback behavior

Tidak relevan besar karena biasanya read-only.

## 11. User-facing outcome

“Penyebab paling mungkin adalah X; file kandidat: A, B, C; next best action: Y.”

---

# C.3 Skill 3 — Test Repair

## 1. Tujuan

Memperbaiki test yang gagal secara cepat dan aman, dengan membedakan:

* test yang brittle,
* test yang outdated,
* code bug yang nyata,
* env/flaky problem.

## 2. Kapan dipakai

Gunakan jika:

* target utama adalah mengembalikan test ke green
* failing target sudah diketahui
* scope tidak ingin terlalu lebar

## 3. Input schema

```json id="xvp1ey"
{
  "skill_name": "test_repair",
  "inputs": {
    "failing_tests": ["string"],
    "log_refs": ["string"],
    "repo_id": "string",
    "workspace_id": "string",
    "constraints": ["string"]
  }
}
```

## 4. Preflight checks

* failing tests diketahui
* test commands tersedia
* workspace bisa menjalankan targeted tests

## 5. Execution graph

1. ingest failing tests + logs
2. classify failure type
3. identify source/test ownership
4. decide patch target:

   * test only
   * source only
   * both
5. patch
6. rerun targeted tests
7. if green, run package subset
8. review regression risk
9. summarize repair

## 6. Tool whitelist

* `logs.parse_failure_bundle`
* `repo.related_tests`
* `repo.rank_files_for_task`
* `patch.inline`
* `patch.structured`
* `test.run_targeted`
* `test.run_package_subset`
* `review.scan_regression`

## 7. Artifacts wajib

* failure bundle
* patch diff
* test run summary
* repair rationale

## 8. Success criteria

* failing tests target green
* package subset tidak membuka regression baru
* repair rationale jelas

## 9. Failure / escalation rules

* flaky evidence kuat → jangan patch source langsung
* lebih dari dua repair attempts → replan
* jika repair butuh broader architecture change → handoff ke Issue-to-PR

## 10. Rollback behavior

* revert patch jika repair membuat test lain rusak

## 11. User-facing outcome

“Test repaired” atau “test tampaknya flaky/env-related, bukan bug source langsung.”

---

# C.4 Skill 4 — Dependency Upgrade

## 1. Tujuan

Melakukan upgrade dependency secara terkendali:

* version bump,
* compatibility check,
* lockfile awareness,
* targeted verification,
* risk summary.

## 2. Kapan dipakai

Gunakan jika task berisi:

* bump package
* migrate version
* update dependency
* fix vulnerability via upgrade

## 3. Input schema

```json id="pn3n6e"
{
  "skill_name": "dependency_upgrade",
  "inputs": {
    "packages": [
      {
        "name": "string",
        "from_version": "string",
        "to_version": "string"
      }
    ],
    "repo_id": "string",
    "workspace_id": "string",
    "constraints": ["string"],
    "allow_lockfile_change": "boolean"
  }
}
```

## 4. Preflight checks

* package manager terdeteksi
* manifest files ditemukan
* lockfile policy jelas
* upgrade target valid
* relevant test/build commands tersedia

## 5. Execution graph

1. inspect manifests + lockfiles
2. map dependency impact
3. identify touched modules/tests
4. plan upgrade
5. apply manifest change
6. handle install/update with approval if needed
7. run targeted build/tests
8. run broader compatibility checks
9. review risk
10. generate upgrade summary

## 6. Tool whitelist

* `repo.dependency_impact`
* `repo.rank_files_for_task`
* `patch.inline`
* `patch.structured`
* `exec`
* `test.run_targeted`
* `test.run_package_subset`
* `review.scan_risk`
* `review.scan_security`

## 7. Artifacts wajib

* manifest diff
* lockfile diff jika ada
* impact summary
* test/build summary
* risk memo
* migration notes jika perlu

## 8. Success criteria

* dependency updated
* relevant tests/build pass
* risks terdokumentasi
* lockfile change sesuai policy

## 9. Failure / escalation rules

* install packages → approval
* lockfile changes → approval jika policy mengharuskan
* high compatibility risk → reviewer mandatory
* if cascading breakages كبير → re-scope atau fail with report

## 10. Rollback behavior

* revert manifest + lockfile to checkpoint
* clear caches jika perlu

## 11. User-facing outcome

“Upgrade siap ditinjau” atau “upgrade diblokir oleh compatibility risks berikut.”

---

# C.5 Skill 5 — Scoped Refactor

## 1. Tujuan

Melakukan refactor yang terkontrol dan scoped:

* memperbaiki struktur code,
* mengurangi duplications,
* rename / extract / reorganize,
  tanpa mengubah behavior secara tidak perlu.

## 2. Kapan dipakai

Gunakan untuk task seperti:

* cleanup module
* extract helper
* simplify function
* reduce duplication
* rename symbols scoped
* move code safely

## 3. Input schema

```json id="6a4w6o"
{
  "skill_name": "refactor_scoped",
  "inputs": {
    "objective": "string",
    "scope_paths": ["string"],
    "behavior_must_remain_same": "boolean",
    "repo_id": "string",
    "workspace_id": "string"
  }
}
```

## 4. Preflight checks

* scope path jelas
* refactor objective cukup sempit
* symbol/reference tools tersedia
* tests yang relevan bisa diidentifikasi

## 5. Execution graph

1. map symbols/references
2. identify affected files
3. build minimal refactor plan
4. apply refactor
5. run syntax/type/lint
6. run targeted tests
7. run review/regression scan
8. summarize changed structure

## 6. Tool whitelist

* `repo.search_symbols`
* `repo.find_references`
* `repo.rank_files_for_task`
* `patch.inline`
* `patch.structured`
* `exec`
* `test.run_targeted`
* `review.scan_regression`

## 7. Artifacts wajib

* diff
* symbol impact summary
* test summary
* refactor rationale

## 8. Success criteria

* no behavior regressions found in targeted verification
* references valid
* syntax/type/lint clean
* scope tidak melebar liar

## 9. Failure / escalation rules

* jika refactor menyentuh terlalu banyak files → split task
* jika scope melebar → planner rescope
* jika critical path terpengaruh → reviewer mandatory

## 10. Rollback behavior

* revert to checkpoint jika symbol graph menjadi tidak stabil

## 11. User-facing outcome

“Refactor scoped selesai dengan behavior-preserving checks.”

---

# C.6 Skill 6 — Code Review Assist

## 1. Tujuan

Menganalisis diff/PR/patch dan menghasilkan review yang actionable:

* regression risks
* style concerns
* security concerns
* missing tests
* compatibility concerns
* approval guidance

Skill ini tidak mengubah code secara default.

## 2. Kapan dipakai

Gunakan jika user meminta:

* review PR
* review patch
* scan regression
* security review
* apakah diff ini aman?

## 3. Input schema

```json id="2a9jph"
{
  "skill_name": "code_review_assist",
  "inputs": {
    "diff_ref": "string",
    "repo_id": "string",
    "workspace_id": "string",
    "review_mode": "general|security|regression|release"
  }
}
```

## 4. Preflight checks

* diff/PR context tersedia
* changed files bisa diidentifikasi
* repo context tersedia jika dibutuhkan

## 5. Execution graph

1. ingest diff
2. map changed files to repo context
3. identify critical paths
4. run regression scan
5. run security scan if requested/relevant
6. identify missing test coverage risk
7. produce review report with severity tags

## 6. Tool whitelist

* `review.scan_regression`
* `review.scan_security`
* `repo.dependency_impact`
* `repo.find_references`
* `artifact.persist`

## 7. Artifacts wajib

* review report
* severity-tagged findings
* test coverage concern summary
* approval recommendation

## 8. Success criteria

* findings terstruktur
* severity jelas
* ada recommendation: approve / revise / investigate

## 9. Failure / escalation rules

* jika diff terlalu besar → split review by module
* jika repo context kurang → mark uncertainty secara eksplisit

## 10. Rollback behavior

Tidak relevan besar, skill read-mostly.

## 11. User-facing outcome

“Review siap” dengan blocker, warning, dan saran tindak lanjut.

---

# C.7 Skill 7 — Incident Hotfix Assist

## 1. Tujuan

Membantu jalur hotfix saat incident/urgent prod issue:

* triage cepat,
* cari candidate patch,
* verifikasi minimum yang aman,
* siapkan hotfix artifact,
* jaga approval boundaries.

Ini bukan berarti auto-deploy. Fokusnya adalah **hotfix preparation under pressure**.

## 2. Kapan dipakai

Gunakan jika:

* ada incident aktif
* urgent prod bug
* sev1/sev2
* user butuh fix cepat dengan scope sempit

## 3. Input schema

```json id="yzq9b7"
{
  "skill_name": "incident_hotfix_assist",
  "inputs": {
    "incident_summary": "string",
    "severity": "sev1|sev2|sev3",
    "symptom_logs": ["string"],
    "repo_id": "string",
    "workspace_id": "string",
    "constraints": ["string"]
  }
}
```

## 4. Preflight checks

* incident summary tersedia
* severity jelas
* relevant logs/traces ada
* critical path awareness tersedia
* approval mode minimal guarded

## 5. Execution graph

1. ingest incident evidence
2. rapid bug triage
3. shortlist high-probability files
4. create narrow hotfix plan
5. patch with minimal blast radius
6. run fastest safe verification set
7. run reviewer scan mandatory
8. generate hotfix memo
9. request explicit approval for external write / PR / deploy-adjacent actions

## 6. Tool whitelist

* `logs.parse_failure_bundle`
* `repo.rank_files_for_task`
* `repo.find_recent_changes`
* `patch.inline`
* `patch.structured`
* `test.run_targeted`
* `review.scan_regression`
* `review.scan_security`
* `artifact.persist`
* connector write actions only with approval

## 7. Artifacts wajib

* incident triage summary
* patch diff
* verification summary
* blast-radius memo
* rollback notes
* hotfix PR body / handoff note

## 8. Success criteria

* hotfix patch candidate tersedia
* minimal safe verification pass
* reviewer scan tidak menemukan blocker kritis yang belum dipahami
* rollback notes tersedia

## 9. Failure / escalation rules

* semua external writes sensitif → explicit approval
* jika blast radius tidak bisa dipersempit → stop dan handoff
* jika evidence tidak cukup → output investigation memo, bukan patch paksa

## 10. Rollback behavior

* revert workspace ke checkpoint
* rollback notes wajib dibuat bila patch candidate dibuat

## 11. User-facing outcome

“Hotfix candidate siap ditinjau/diangkat” atau “evidence belum cukup untuk patch aman.”

---

# C.8 Skill selection rules

Agar router bisa memilih skill yang benar, tambahkan policy pemilihan seperti ini.

## Heuristik pemilihan skill

### Pilih `issue_to_pr` jika:

* ada issue/ticket
* targetnya implementasi/fix
* outcome yang diinginkan berupa patch/PR

### Pilih `bug_triage` jika:

* outcome utama adalah diagnosis
* belum jelas apakah harus patch source/test/env

### Pilih `test_repair` jika:

* failing tests sudah jelas
* target utama adalah green tests

### Pilih `dependency_upgrade` jika:

* task fokus pada package/library version changes

### Pilih `refactor_scoped` jika:

* objective adalah restructure tanpa feature work besar

### Pilih `code_review_assist` jika:

* user membawa diff/PR dan ingin analisis

### Pilih `incident_hotfix_assist` jika:

* ada urgency incident/prod failure
* blast radius harus ketat
* approval boundaries lebih ketat

---

# C.9 Skill contract umum

Semua skill harus mengikuti contract umum ini:

```json id="8slxx9"
{
  "skill_name": "string",
  "version": "string",
  "maturity": "experimental|beta|trusted|autonomous_narrow",
  "input_schema_ref": "string",
  "preflight_checks": ["string"],
  "tool_whitelist": ["string"],
  "execution_graph": ["string"],
  "required_artifacts": ["string"],
  "success_criteria": ["string"],
  "rollback_behavior": "string",
  "escalation_rules": ["string"],
  "default_routing_hints": {
    "roles": ["string"],
    "approval_mode": "standard|guarded|explicit"
  }
}
```

---

# C.10 Maturity model per skill

Saya sarankan tiap skill punya level kematangan:

## `experimental`

* belum stabil
* banyak human review
* benchmark coverage rendah

## `beta`

* workflow sudah usable
* sebagian recovery sudah baik
* masih perlu approval sering

## `trusted`

* benchmark stabil
* telemetry sehat
* failure modes sudah dipahami

## `autonomous_narrow`

* aman untuk lane sempit tertentu
* approval minimum
* high confidence + strong guardrails

Contoh target awal:

* `bug_triage` → bisa lebih cepat mencapai `trusted`
* `code_review_assist` → juga cepat ke `trusted`
* `issue_to_pr` → awalnya `beta`
* `incident_hotfix_assist` → lama tetap `guarded`/`beta`

---

# C.11 Prioritas implementasi skill

Kalau harus memilih urutan implementasi:

## Wave 1

1. `bug_triage`
2. `test_repair`
3. `issue_to_pr`

Ini memberi jalur tercepat ke nilai produk.

## Wave 2

4. `code_review_assist`
5. `dependency_upgrade`

## Wave 3

6. `refactor_scoped`
7. `incident_hotfix_assist`

Karena dua terakhir lebih risk-sensitive.

---

# C.12 KPI per skill

## Issue-to-PR

* patch success rate
* targeted test pass rate
* draft PR readiness rate
* cost per resolved issue

## Bug Triage

* root cause precision
* next-action usefulness
* time-to-diagnosis

## Test Repair

* failing test recovery rate
* regression escape rate
* retries per repair

## Dependency Upgrade

* upgrade success rate
* compatibility failure rate
* lockfile approval friction

## Scoped Refactor

* scope containment
* regression rate
* references preserved

## Code Review Assist

* blocker precision
* false-positive rate
* reviewer acceptance usefulness

## Incident Hotfix Assist

* time-to-hotfix-candidate
* blast-radius containment
* manual intervention count
* rollback-readiness completeness

---

# C.13 Anti-pattern dalam skill design

## Jangan lakukan ini:

* membuat skill yang hanya prompt panjang tanpa execution graph
* membiarkan skill memanggil tool di luar whitelist
* membuat success criteria yang kabur
* menggabungkan diagnosis dan patching tanpa decision boundary
* membuat incident skill auto-write ke sistem produksi
* menjadikan review skill otomatis approve tanpa severity gating

---

# C.14 Ringkasan akhir C

Dengan 7 skill ini, OpenClaw sekarang punya **unit of work produk** yang nyata:

* dari diagnosis,
* ke perbaikan,
* ke verifikasi,
* ke review,
* ke hotfix preparation.

Dan yang penting, semua skill ini bisa berjalan di atas fondasi yang sudah kita bangun:

* router,
* schemas,
* retry engine,
* execution loop,
* API contracts,
* approvals,
* telemetry.

Artinya desain Anda sekarang sudah mendekati bentuk **platform spec yang implementable**.

---



#### PerplexityComputerV2.part8.md

# H. Benchmark / Evaluation Framework Design

## 1. Tujuan evaluation framework

Evaluation framework harus menjawab 6 pertanyaan inti:

1. **Apakah task selesai?**
2. **Seberapa sering berhasil pada percobaan pertama?**
3. **Berapa lama dan berapa mahal untuk berhasil?**
4. **Apakah keberhasilan itu aman dan benar, atau hanya “terlihat jalan”?**
5. **Bagian mana dari sistem yang menyebabkan kegagalan?**
6. **Versi router / skill / model mana yang terbaik untuk jenis task tertentu?**

Framework ini harus mengukur tiga hal sekaligus:

* **quality**
* **efficiency**
* **reliability**

---

# 2. Prinsip desain eval

## 2.1 Outcome > vibes

Jangan ukur “jawabannya terlihat bagus.”
Ukur:

* patch valid atau tidak,
* tests pass atau tidak,
* side effects benar atau tidak,
* issue terselesaikan atau tidak.

## 2.2 Skill-aware, bukan hanya model-aware

Karena OpenClaw adalah sistem multi-layer, evaluasi tidak boleh berhenti di “model A vs model B”.
Yang harus diuji:

* router version
* skill version
* prompt/schema version
* tool adapter version
* retry policy version
* sub-agent policy version

## 2.3 Offline + replay + production

Eval harus punya tiga sumber:

* benchmark offline statis
* replay dari task nyata
* telemetry dari produksi

## 2.4 Granular failure attribution

Kalau task gagal, framework harus bisa bilang:

* gagal karena routing salah,
* verifier salah,
* patch mode salah,
* browser recovery buruk,
* tool timeout,
* approval friction,
* atau benchmark memang terlalu ambigu.

## 2.5 Regression gate wajib

Setiap perubahan signifikan harus diuji terhadap benchmark subset minimal sebelum release.

---

# 3. Tiga lapisan evaluation

## 3.1 Component eval

Menguji komponen individual:

* router
* verifier
* reviewer
* failure classifier
* patch mode selector
* approval policy

Contoh:

* apakah router memilih skill yang benar?
* apakah verifier mengklasifikasi failure bundle dengan benar?
* apakah reviewer memberi blocker yang masuk akal?

## 3.2 Workflow eval

Menguji skill end-to-end:

* issue_to_pr
* bug_triage
* test_repair
* dependency_upgrade
* refactor_scoped
* code_review_assist
* incident_hotfix_assist

## 3.3 Long-horizon / system eval

Menguji:

* resumability
* interruption recovery
* multi-agent coordination
* browser flows
* connector side effects
* degraded mode behavior

---

# 4. Eval object model

Saya sarankan Anda punya entitas ini di framework evaluasi:

* `BenchmarkSuite`
* `BenchmarkTask`
* `BenchmarkRun`
* `RunVariant`
* `EvaluationJudge`
* `RunOutcome`
* `FailureAttribution`
* `RegressionReport`

---

# 5. Benchmark suite taxonomy

## 5.1 Coding suite

Untuk lane utama OpenClaw.

Sub-kategori:

* small bugfix
* multi-file bugfix
* issue-to-pr
* refactor
* test repair
* dependency upgrade
* code review
* CI failure debug

## 5.2 Browser/admin suite

Untuk task berbasis browser yang relevan dengan engineering.

Sub-kategori:

* CI dashboard inspection
* artifact download + summarize
* rerun failed job
* admin setting change non-prod
* docs/navigation behind auth

## 5.3 Connector suite

Untuk external side-effect reliability.

Sub-kategori:

* create draft PR
* comment on issue
* fetch artifacts
* update tracker ticket
* post summary to chat

## 5.4 Long-horizon suite

Untuk daya tahan sistem.

Sub-kategori:

* task > 20 menit
* interruption + resume
* sub-agent coordination
* budget pressure
* partial failure recovery

## 5.5 Safety/policy suite

Untuk guardrails.

Sub-kategori:

* should ask approval
* should block unsafe action
* should avoid duplicate external write
* should stop on ambiguous critical action

---

# 6. BenchmarkTask schema

Saya sarankan setiap benchmark task punya schema seperti ini:

```json id="atgk5c"
{
  "benchmark_task_id": "bt_123",
  "suite": "coding",
  "category": "test_repair",
  "difficulty": "medium",
  "title": "Repair failing login cookie persistence tests",
  "goal": "Make failing login tests pass without changing production auth contract",
  "inputs": {
    "repo_fixture_ref": "fixture://repo/login-auth-1",
    "workspace_seed_ref": "seed://workspace/auth-login-fail",
    "failing_tests": [
      "tests/login.spec.ts"
    ],
    "constraints": [
      "Do not change public auth API"
    ]
  },
  "oracle": {
    "required_outcomes": [
      "targeted_tests_pass",
      "no_public_api_change"
    ],
    "forbidden_outcomes": [
      "modify_public_auth_contract"
    ],
    "expected_artifacts": [
      "diff",
      "test_summary"
    ]
  },
  "grading": {
    "success_weight": 0.5,
    "correctness_weight": 0.3,
    "cost_weight": 0.1,
    "latency_weight": 0.1
  },
  "tags": ["auth", "tests", "cookie", "regression"]
}
```

---

# 7. Benchmark run schema

```json id="pt3kp9"
{
  "run_id": "run_123",
  "benchmark_task_id": "bt_123",
  "variant": {
    "router_version": "router_v3",
    "skill_version": "test_repair_v2",
    "model_bundle": "bundle_b",
    "retry_policy_version": "retry_v2",
    "tool_adapter_version": "tools_v5"
  },
  "started_at": "2026-04-05T10:00:00Z",
  "ended_at": "2026-04-05T10:04:12Z",
  "outcome": {
    "status": "completed",
    "score": 0.86,
    "success": true,
    "first_pass_success": false
  },
  "metrics": {
    "wall_clock_seconds": 252,
    "model_calls": 11,
    "tool_calls": 17,
    "retry_count": 2,
    "human_interventions": 0,
    "cost_estimate": 0.84
  },
  "artifacts": [
    "artifact://diff.patch",
    "artifact://test_summary.md"
  ],
  "failure_attribution": []
}
```

---

# 8. KPI utama framework

## 8.1 Primary metrics

### Success rate

Apakah task mencapai outcome utama.

### First-pass success

Berhasil tanpa retry besar / replan / human intervention.

### Correctness rate

Apakah hasilnya benar, bukan hanya selesai.

### Median wall-clock time

Waktu total.

### Median time-to-first-working-patch

Khusus coding tasks.

### Cost per resolved task

Token + tool + compute cost.

### Human intervention rate

Seberapa sering task perlu campur tangan manusia.

---

## 8.2 Secondary metrics

* retry count
* replan count
* checkpoint resume success
* artifact completeness
* review blocker rate
* browser action count
* connector side-effect uncertainty rate
* invalid patch recovery rate
* context compaction count

---

## 8.3 Safety metrics

* approval miss rate
* unsafe action attempted rate
* duplicate external write rate
* sensitive path touched without reviewer rate
* forbidden outcome rate

---

# 9. Skill-specific scorecards

Setiap skill butuh rubric sendiri.

---

## 9.1 Issue-to-PR scorecard

Metrics:

* patch generated
* targeted tests pass
* broader checks pass
* PR artifacts complete
* no forbidden files touched
* review blocker absence
* cost
* time

Example weighted score:

* task success: 35%
* correctness: 30%
* review quality/safety: 15%
* time: 10%
* cost: 10%

---

## 9.2 Bug Triage scorecard

Metrics:

* root cause top-1 precision
* root cause top-3 recall
* next action usefulness
* suspected files precision
* diagnosis time
* false patch recommendation rate

---

## 9.3 Test Repair scorecard

Metrics:

* failing tests repaired
* no extra regression introduced
* target scope containment
* retries used
* time-to-green
* flaky misclassification rate

---

## 9.4 Dependency Upgrade scorecard

Metrics:

* upgrade correctness
* lockfile policy compliance
* compatibility regression rate
* impacted modules identified
* migration notes completeness

---

## 9.5 Scoped Refactor scorecard

Metrics:

* behavior preserved
* scope stayed bounded
* symbol/reference correctness
* lint/type/test hygiene
* review risk severity

---

## 9.6 Code Review Assist scorecard

Metrics:

* blocker precision
* blocker recall
* false positive rate
* severity calibration quality
* usefulness to human reviewer

---

## 9.7 Incident Hotfix Assist scorecard

Metrics:

* time-to-hotfix-candidate
* blast radius containment
* rollback readiness
* correctness under urgency
* approval correctness

---

# 10. Oracles and judging

Sistem eval yang baik harus punya **oracle** dan **judge**.

## 10.1 Oracle

Oracle adalah sumber kebenaran untuk benchmark task.

Contoh oracle:

* expected files changed
* expected tests pass
* forbidden files untouched
* expected PR body fields
* expected browser postcondition
* expected connector side effect

## 10.2 Judge types

### Deterministic judge

Dipakai jika outcome bisa dicek dengan aturan keras.
Contoh:

* tests passed?
* file X berubah?
* file Y tidak berubah?
* duplicate PR tidak terjadi?

### Heuristic judge

Dipakai jika butuh rule set yang lebih kaya.
Contoh:

* apakah scope tetap sempit?
* apakah blast radius memo cukup lengkap?

### Model-assisted judge

Dipakai hanya untuk hal yang sulit dideterminiskan, misalnya:

* kualitas review summary
* kualitas incident handoff note

Model-assisted judge harus selalu dibatasi dan sedapat mungkin dikombinasikan dengan deterministic checks.

---

# 11. Grading pipeline

Saya sarankan grading dilakukan bertahap:

1. **validate artifacts**
2. **run deterministic checks**
3. **run policy/safety checks**
4. **run semantic/judge checks jika perlu**
5. **compute weighted score**
6. **generate failure attribution**

## 11.1 Pseudocode grading

```python id="ccs83x"
def grade_run(benchmark_task, run):
    checks = []
    checks += run_deterministic_checks(benchmark_task, run)
    checks += run_safety_checks(benchmark_task, run)
    checks += run_artifact_checks(benchmark_task, run)

    if benchmark_task_requires_semantic_judging(benchmark_task):
        checks += run_semantic_judges(benchmark_task, run)

    score = compute_weighted_score(benchmark_task.grading, checks, run.metrics)
    success = determine_success(checks)

    return {
        "score": score,
        "success": success,
        "checks": checks
    }
```

---

# 12. Failure attribution framework

Ini sangat penting. Jangan berhenti di “run failed”.

Setiap kegagalan benchmark harus diatribusikan ke salah satu domain:

* routing failure
* planning failure
* context failure
* patch failure
* verification failure
* reviewer failure
* browser execution failure
* connector failure
* approval policy failure
* budget exhaustion
* environment instability
* benchmark ambiguity

## 12.1 FailureAttribution schema

```json id="ojthyc"
{
  "failure_attribution": {
    "primary_cause": "routing_failure",
    "secondary_causes": [
      "context_overflow"
    ],
    "evidence": [
      "Router selected bug_triage instead of test_repair",
      "No related_tests lookup occurred before patch step"
    ],
    "recoverable": true
  }
}
```

## 12.2 Mengapa ini penting

Tanpa attribution, tim akan:

* salah memperbaiki komponen,
* menyalahkan model padahal router salah,
* menyalahkan skill padahal tool adapter rusak.

---

# 13. Offline benchmark design

Ini benchmark yang dijalankan di fixture yang terkendali.

## 13.1 Struktur benchmark offline

Setiap task offline harus punya:

* repo fixture
* initial workspace seed
* input prompt/issue/log
* expected outputs
* evaluation scripts

## 13.2 Kelebihan

* repeatable
* murah
* cocok untuk regression tests
* bisa dipakai untuk PR gating

## 13.3 Kekurangan

* tidak menangkap semua kegagalan dunia nyata
* bisa overfit jika task terlalu sedikit

## 13.4 Minimum target awal

Saya sarankan minimal:

* 100 coding tasks
* 25 browser tasks
* 25 connector tasks
* 20 long-horizon tasks
* 20 safety/policy tasks

---

# 14. Replay benchmark design

Replay benchmark memakai task nyata yang sudah pernah terjadi, tetapi direplay dengan fixture yang disanitasi.

## 14.1 Contoh

* CI failure nyata minggu lalu
* issue bug nyata yang pernah fixed
* dependency upgrade yang dulu menyebabkan regressions
* PR review nyata yang punya blocker penting

## 14.2 Kelebihan

* lebih representatif
* lebih dekat ke production
* membantu menghindari benchmark yang terlalu akademik

## 14.3 Kekurangan

* lebih sulit disanitasi
* oracle kadang kurang bersih
* fixture maintenance lebih mahal

---

# 15. Production eval design

Offline benchmark tidak cukup. Anda juga perlu production eval.

## 15.1 Production eval yang harus dikumpulkan

* task success outcome
* approval counts
* retries
* latency
* cost
* failure classes
* rollback counts
* resume success
* user acceptance / follow-up edits

## 15.2 Shadow eval

Untuk versi router/skill baru:

* jalankan pada task real secara shadow
* jangan ambil side effect
* bandingkan decision path dengan sistem aktif

## 15.3 Canary eval

Rilis ke porsi kecil task:

* 1–5% traffic/task category
* monitor KPI
* rollback jika regression terlihat

---

# 16. Regression gate design

Setiap perubahan besar harus melewati gate.

## 16.1 Perubahan yang wajib gated

* router rules
* skill contracts
* retry policy
* model bundle
* tool adapter changes
* connector write logic
* browser recovery logic

## 16.2 Gate minimum

Contoh aturan:

* success rate tidak boleh turun > 2%
* duplicate write rate harus tetap 0
* approval miss rate harus tetap 0
* cost per resolved task tidak boleh naik > 10% tanpa approval
* high-severity regression blockers tidak boleh naik

## 16.3 Gate tiers

### Tier 1: smoke

Subset kecil, cepat, wajib per commit penting.

### Tier 2: regression

Subset sedang, per PR / pre-merge.

### Tier 3: release candidate

Suite penuh, sebelum release bundle/router/skill.

---

# 17. Router eval design

Karena router sangat penting, beri benchmark sendiri.

## 17.1 Tugas router

Dari intake task, router harus memilih:

* task class
* skill
* roles
* budget
* approval mode
* sub-agent plan

## 17.2 Metrics router

* task-class accuracy
* skill-selection accuracy
* over-escalation rate
* under-escalation rate
* unnecessary reviewer rate
* missing reviewer rate
* cost inefficiency rate

## 17.3 Failure examples

* memilih `bug_triage` padahal harus `test_repair`
* memilih `coder_strong` untuk task tiny
* lupa menandai approval requirement untuk lockfile change
* tidak memanggil reviewer pada auth path change

---

# 18. Verifier / reviewer eval design

## 18.1 Verifier metrics

* root cause top-1 precision
* top-3 recall
* correct next-action recommendation
* false “patch source” recommendation rate
* flaky detection quality

## 18.2 Reviewer metrics

* blocker precision
* blocker recall
* severity calibration
* false positive rate
* missing critical risk rate

---

# 19. Browser eval design

Walau UI/UX tidak Anda prioritaskan, browser plane tetap perlu diukur.

## 19.1 Browser metrics

* task success rate
* stale-ref recovery rate
* action verification rate
* auth interruption handling correctness
* side-effect certainty rate
* action count efficiency

## 19.2 Benchmark examples

* buka CI dashboard
* cari latest failed run
* buka failed job
* unduh artifact
* ringkas failure
* rerun failed job dengan approval jika perlu

---

# 20. Connector eval design

Connector write correctness harus sangat ketat.

## 20.1 Metrics

* write success rate
* duplicate side effect rate
* postcondition verification rate
* permission handling correctness
* rate-limit recovery quality
* approval correctness

## 20.2 Hard requirement

Untuk connector write:

* **duplicate side effect rate target = 0**

---

# 21. Long-horizon eval design

Ini yang sering membedakan agent demo vs agent produk.

## 21.1 Yang diuji

* interruption
* checkpoint restore
* memory compaction
* multi-agent coordination
* budget handling
* resumption correctness

## 21.2 Metrics

* resume success rate
* context-loss rate
* artifact continuity rate
* plan consistency after resume
* duplicated work rate after resume

## 21.3 Benchmark example

Task:

* multi-file fix
* one forced timeout
* one forced approval block
* one resume after checkpoint
* expected successful completion

---

# 22. Safety / policy eval design

Karena OpenClaw akan menyentuh code, browser, dan connector writes, safety eval wajib ada.

## 22.1 Safety benchmark cases

* lockfile change without approval should block
* prod/admin write should block
* ambiguous critical task should ask
* duplicate connector retry should not resend blindly
* secrets access should always require explicit approval

## 22.2 Metrics

* approval miss rate
* unsafe auto-action rate
* incorrect blocking rate
* user-annoyance overblocking rate

---

# 23. Cost eval design

Jangan hanya mengejar success rate. Cost matters.

## 23.1 Cost dimensions

* model token cost
* tool compute cost
* browser runtime cost
* connector/API cost
* storage/artifact cost
* wall-clock operational cost

## 23.2 Cost metrics

* cost per successful task
* cost per skill category
* cost per retry
* wasted cost on failed tasks
* model mix efficiency

## 23.3 Cost-quality curve

Setiap bundle router/skill/model harus diplot sebagai:

* success rate vs cost
* latency vs cost
* safety vs cost

Tujuannya agar tidak terus mendorong model mahal tanpa return nyata.

---

# 24. Eval dataset curation rules

Benchmark akan cepat membusuk kalau dataset buruk.

## 24.1 Wajib ada variasi

* language/framework berbeda
* repo kecil/sedang/besar
* deterministic vs flaky failures
* simple vs ambiguous tasks
* single-file vs multi-file

## 24.2 Jangan overfit ke benchmark mini

Kalau task benchmark terlalu sedikit atau terlalu mirip, sistem akan “belajar tes” bukan “belajar kerja”.

## 24.3 Refresh cadence

* review bulanan untuk task usang
* tambah task dari production failures
* tandai benchmark yang terlalu mudah / tidak representatif

---

# 25. Leaderboard internal

Saya sarankan buat leaderboard internal per kombinasi:

* router version
* skill version
* model bundle
* retry policy
* toolchain version

## 25.1 Contoh row leaderboard

* `router_v3 + test_repair_v2 + bundle_b + retry_v2`
* success: 78%
* first-pass: 61%
* cost/success: $0.82
* median time: 4m12s
* duplicate writes: 0
* approval miss: 0

Dengan ini tim bisa berhenti berdebat pakai opini.

---

# 26. Weekly eval report

Saya sangat sarankan sistem menghasilkan laporan mingguan otomatis.

## Isi minimal:

* success rate per skill
* cost per skill
* top 10 error classes
* top regression causes
* router misclassification examples
* duplicate/uncertain side-effect incidents
* newly flaky benchmark tasks
* benchmark deltas vs minggu lalu

Ini akan jadi “control tower” kualitas OpenClaw.

---

# 27. Eval runner architecture

Struktur minimal:

* `BenchmarkRegistry`
* `FixtureManager`
* `RunOrchestrator`
* `JudgeEngine`
* `ScoringEngine`
* `AttributionEngine`
* `LeaderboardStore`
* `RegressionGate`

## Flow:

1. load suite
2. hydrate fixture
3. run variant
4. collect artifacts + telemetry
5. grade
6. attribute failures
7. store results
8. compare vs baseline

---

# 28. Pseudocode eval runner

```python id="9u1vpa"
def run_benchmark_suite(suite_id, variant):
    suite = load_suite(suite_id)
    results = []

    for benchmark_task in suite.tasks:
        fixture = hydrate_fixture(benchmark_task)
        run = execute_variant_on_fixture(variant, benchmark_task, fixture)
        graded = grade_run(benchmark_task, run)
        attribution = attribute_failures(benchmark_task, run, graded)

        results.append({
            "task_id": benchmark_task.benchmark_task_id,
            "run": run,
            "graded": graded,
            "attribution": attribution
        })

    report = aggregate_suite_results(results)
    store_report(report)
    return report
```

---

# 29. Release gating policy yang saya sarankan

Sebelum versi baru router/skill dirilis:

## Wajib lolos:

* no increase in duplicate external write rate
* no increase in approval miss rate
* success rate minimal sama atau naik
* cost increase dalam batas
* no new critical safety regression
* long-horizon resume rate tidak turun signifikan

## Boleh toleransi kecil:

* latency sedikit naik jika correctness naik bermakna
* cost sedikit naik jika success naik cukup besar

---

# 30. Rekomendasi target awal benchmark

Kalau Anda butuh target numerik awal yang realistis untuk 90 hari pertama:

## Coding

* success rate: 55–70% baseline tergantung difficulty
* first-pass success: 35–50%
* invalid patch recovery: >70%
* targeted verification artifact completeness: >90%

## Router

* skill selection accuracy: >85%
* missing-reviewer rate: <3%
* over-escalation rate: <15%

## Safety

* duplicate external writes: 0
* approval miss rate: 0
* unsafe auto-action on blocked actions: 0

## Long-horizon

* resume success: >75%
* context-loss rate: <10%

Ini bukan target final, tapi baseline yang cukup bagus untuk sistem awal.

---

# 31. Anti-pattern dalam evaluation

## Jangan lakukan ini:

* hanya mengukur success, tanpa correctness
* hanya mengukur benchmark offline, tanpa replay/production
* hanya mengukur model, tanpa skill/router/toolchain version
* membiarkan judge model menjadi satu-satunya penilai
* tidak menyimpan failure attribution
* tidak punya regression gate
* menghapus benchmark susah karena “merusak angka”

---

# 32. Ringkasan akhir H

Kalau saya padatkan, evaluation framework ini bertumpu pada 8 keputusan:

1. **eval harus menilai outcome, bukan impresi**
2. **eval harus skill-aware dan system-aware**
3. **benchmark harus dibagi menjadi offline, replay, dan production**
4. **setiap task butuh oracle dan grading rubric**
5. **failure harus diatribusikan ke komponen yang tepat**
6. **router, verifier, reviewer, browser, connector harus punya eval sendiri**
7. **release harus dijaga dengan regression gates**
8. **weekly report dan leaderboard internal wajib ada**

---

# Apa yang sekarang sudah lengkap

Dengan A + B + C + D + E + F + H, Anda sekarang sudah punya kerangka desain yang hampir lengkap untuk:

* arsitektur runtime,
* routing,
* schemas,
* retry/error handling,
* execution loop,
* API contract,
* skill definitions,
* dan evaluation framework.

Artinya, secara spesifikasi, Anda sudah mendekati **blueprint platform OpenClaw untuk engineering automation**.

---



#### PerplexityComputerV2.part9.md

# G. Data Model + Database / Storage Design

## 1. Tujuan storage architecture

Storage layer OpenClaw harus mendukung 8 hal:

1. **task orchestration**
2. **checkpoint & resume**
3. **artifact persistence**
4. **memory hierarchy**
5. **approval workflow**
6. **sub-agent coordination**
7. **telemetry & evaluation**
8. **auditability**

Artinya tidak cukup hanya satu database “serba simpan.”
Kita perlu memisahkan storage berdasarkan pola akses.

---

# 2. Prinsip desain storage

## 2.1 Pisahkan operational state dari large artifacts

Task state, checkpoint metadata, approval, routing, dan retry state harus tersimpan di storage transaksional.
Logs, screenshots, patches, junit, DOM snapshot, dll harus masuk object/blob storage.

## 2.2 Event-sourcing parsial, bukan total

Tidak perlu full event sourcing murni untuk semua state, tetapi semua transition penting harus tercatat sebagai event.
Current state tetap disimpan sebagai materialized row/document supaya query cepat.

## 2.3 Task-centric indexing

Hampir semua debug dan observability akan berawal dari:

* `task_id`
* `workspace_id`
* `repo_id`
* `run_id`
* `approval_id`
* `checkpoint_id`

Jadi storage harus diindeks terutama berdasarkan axis itu.

## 2.4 Memory dipisah berdasarkan scope dan retention

Session memory, repo memory, team memory, operational memory tidak boleh dicampur jadi satu koleksi blob bebas.

## 2.5 Artifacts immutable by default

Diff, logs, screenshots, summaries, failure bundles idealnya immutable.
Kalau ada revisi, buat artifact baru + pointer relation.

## 2.6 Evaluasi butuh analytical path

Operational DB tidak ideal untuk leaderboard, weekly reports, regression analytics.
Perlu jalur sink ke analytics store / warehouse.

---

# 3. Rekomendasi storage topology

Saya sarankan 4 lapisan penyimpanan:

## 3.1 Operational relational database

Untuk:

* tasks
* task plans
* step runs
* checkpoints metadata
* approvals
* subagent runs
* normalized errors
* routing decisions
* policy decisions
* memory metadata
* artifact metadata

Pilihan cocok:

* **PostgreSQL**

Kenapa:

* strong transactions
* relational integrity
* query fleksibel
* JSONB untuk payload semi-terstruktur
* cocok untuk workflow engine

## 3.2 Object/blob storage

Untuk:

* logs
* patches
* screenshots
* DOM snapshots
* network traces
* junit/xml/json reports
* PR body drafts
* review reports
* failure bundle raw attachments
* repo summaries
* benchmark fixtures artifacts

Pilihan cocok:

* S3-compatible object storage / GCS / MinIO

## 3.3 Search / indexing layer

Untuk:

* full-text search pada summaries/log snippets/errors
* artifact content search
* benchmark failure pattern search
* audit queries lintas tasks

Pilihan cocok:

* OpenSearch / Elasticsearch / Postgres FTS pada fase awal

## 3.4 Analytics / warehouse layer

Untuk:

* eval reports
* weekly leaderboard
* KPI trend
* cost analysis
* failure distribution
* router/skill comparison

Pilihan cocok:

* BigQuery / ClickHouse / Snowflake / DuckDB pipeline awal

---

# 4. Logical data domains

Secara logis, data dibagi menjadi domain berikut:

1. **Task domain**
2. **Plan & execution domain**
3. **Checkpoint domain**
4. **Artifact domain**
5. **Approval domain**
6. **Memory domain**
7. **Error & retry domain**
8. **Sub-agent domain**
9. **Telemetry domain**
10. **Evaluation domain**

---

# 5. Core relational schema

Di bawah ini saya sarankan tabel-tabel utama.

---

## 5.1 `tasks`

Source of truth task state.

### Columns

* `task_id` PK
* `created_at`
* `updated_at`
* `goal`
* `normalized_goal`
* `status`
* `task_class`
* `complexity`
* `risk_tier`
* `workspace_id`
* `repo_id`
* `team_id`
* `issue_id` nullable
* `current_plan_id` nullable
* `current_checkpoint_id` nullable
* `approval_mode`
* `latest_confidence`
* `budget_json`
* `routing_version`
* `skill_name`
* `skill_version`
* `final_outcome`
* `completed_at` nullable
* `cancelled_at` nullable
* `failure_reason` nullable

### Indexes

* `(status, updated_at)`
* `(repo_id, created_at desc)`
* `(workspace_id, created_at desc)`
* `(team_id, created_at desc)`
* `(task_class, status)`
* `(skill_name, skill_version)`

---

## 5.2 `task_constraints`

Agar constraints bisa di-query terstruktur.

### Columns

* `id` PK
* `task_id` FK
* `constraint_text`
* `constraint_type` nullable
* `created_at`

Index:

* `(task_id)`

---

## 5.3 `routing_decisions`

Menyimpan output router, termasuk reroute.

### Columns

* `routing_decision_id` PK
* `task_id` FK
* `created_at`
* `decision_type` (`initial`, `reroute`)
* `task_class`
* `complexity`
* `risk_tier`
* `roles_json`
* `tool_plan_json`
* `subagent_plan_json`
* `approval_mode`
* `budget_json`
* `checkpoint_policy_json`
* `recovery_policy_json`
* `source_step_id` nullable
* `source_error_id` nullable
* `router_version`

Indexes:

* `(task_id, created_at desc)`
* `(router_version)`
* `(decision_type)`

---

## 5.4 `task_plans`

### Columns

* `plan_id` PK
* `task_id` FK
* `created_at`
* `updated_at`
* `summary`
* `success_criteria_json`
* `assumptions_json`
* `open_questions_json`
* `confidence`
* `planner_version`
* `is_active`

Indexes:

* `(task_id, created_at desc)`
* `(task_id, is_active)`

---

## 5.5 `plan_steps`

### Columns

* `step_id` PK
* `plan_id` FK
* `task_id` FK
* `name`
* `kind`
* `status`
* `step_order`
* `depends_on_json`
* `tool_candidates_json`
* `expected_artifacts_json`
* `confidence_gate` nullable
* `created_at`
* `updated_at`

Indexes:

* `(task_id, status, step_order)`
* `(plan_id, step_order)`
* `(task_id, kind, status)`

---

## 5.6 `step_runs`

Satu step bisa berjalan beberapa kali karena retry.

### Columns

* `step_run_id` PK
* `task_id` FK
* `step_id` FK
* `attempt_no`
* `started_at`
* `ended_at`
* `status` (`completed`, `failed`, `partial`)
* `reasoning_role`
* `tool_name` nullable
* `model_bundle` nullable
* `input_ref` nullable
* `output_ref` nullable
* `structured_output_json`
* `confidence`
* `error_id` nullable
* `duration_ms`
* `retry_strategy` nullable

Indexes:

* `(task_id, step_id, attempt_no)`
* `(task_id, started_at desc)`
* `(tool_name, started_at desc)`
* `(reasoning_role, started_at desc)`

---

# 6. Checkpoint domain

---

## 6.1 `checkpoints`

### Columns

* `checkpoint_id` PK
* `task_id` FK
* `created_at`
* `task_status`
* `last_successful_step`
* `plan_summary`
* `file_scope_json`
* `artifacts_json`
* `pending_approvals_json`
* `retry_count`
* `confidence_snapshot`
* `resume_instruction`
* `next_recommended_action`
* `workspace_snapshot_id`
* `reason`

Indexes:

* `(task_id, created_at desc)`
* `(workspace_snapshot_id)`

---

## 6.2 `workspace_snapshots`

Kalau Anda benar-benar ingin resumability yang kuat, snapshot harus jadi objek first-class.

### Columns

* `workspace_snapshot_id` PK
* `workspace_id`
* `repo_id`
* `branch_name`
* `snapshot_ref` (pointer ke blob/object store atau VM/container snapshot)
* `created_at`
* `snapshot_type` (`git_ref`, `filesystem_archive`, `overlay`, `container_state`)
* `metadata_json`

Indexes:

* `(workspace_id, created_at desc)`
* `(repo_id, created_at desc)`

---

# 7. Artifact domain

Artifact metadata di relational DB, content di blob storage.

---

## 7.1 `artifacts`

### Columns

* `artifact_id` PK
* `task_id` FK nullable
* `step_id` FK nullable
* `step_run_id` FK nullable
* `subagent_id` FK nullable
* `type`
* `created_at`
* `producer_component`
* `producer_step`
* `uri`
* `mime_type`
* `size_bytes`
* `retention_class`
* `sha256`
* `metadata_json`
* `is_immutable` bool default true

Indexes:

* `(task_id, created_at desc)`
* `(type, created_at desc)`
* `(retention_class, created_at desc)`
* `(sha256)`

---

## 7.2 `artifact_links`

Untuk relasi antar artifact.

### Columns

* `id` PK
* `src_artifact_id`
* `dst_artifact_id`
* `link_type` (`derived_from`, `summarizes`, `replaces`, `paired_with`)
* `created_at`

Indexes:

* `(src_artifact_id)`
* `(dst_artifact_id)`

---

## 7.3 Object storage layout

Saya sarankan struktur key seperti:

```text
/tasks/{task_id}/artifacts/{artifact_id}/{filename}
/benchmarks/{suite_id}/{benchmark_task_id}/{artifact_id}/{filename}
/workspaces/{workspace_id}/snapshots/{snapshot_id}.tar.zst
/subagents/{subagent_id}/artifacts/{artifact_id}/{filename}
```

Ini membuat cleanup, retention, dan audit lebih mudah.

---

# 8. Approval domain

---

## 8.1 `approvals`

### Columns

* `approval_id` PK
* `task_id` FK
* `step_id` nullable
* `action`
* `reason`
* `risk_summary_json`
* `artifact_refs_json`
* `confidence`
* `status` (`pending`, `approved`, `rejected`, `expired`)
* `requested_at`
* `resolved_at` nullable
* `resolved_by` nullable
* `approval_mode`
* `policy_decision_ref` nullable

Indexes:

* `(task_id, status, requested_at desc)`
* `(status, requested_at desc)`
* `(resolved_by, resolved_at desc)`

---

## 8.2 `approval_resolutions`

Audit trail terpisah.

### Columns

* `id` PK
* `approval_id` FK
* `resolution`
* `resolved_by`
* `reason` nullable
* `created_at`

Indexes:

* `(approval_id, created_at desc)`

---

## 8.3 `policy_decisions`

### Columns

* `policy_decision_id` PK
* `task_id` FK nullable
* `created_at`
* `action_type`
* `resource`
* `decision` (`allow`, `require_approval`, `deny`)
* `reason`
* `risk_context_json`
* `policy_version`

Indexes:

* `(task_id, created_at desc)`
* `(decision, created_at desc)`
* `(policy_version)`

---

# 9. Memory domain

Pisahkan data “working memory” dari “knowledge memory”.

---

## 9.1 `session_memory`

### Columns

* `task_id` PK / FK
* `updated_at`
* `objective`
* `constraints_json`
* `assumptions_json`
* `file_shortlist_json`
* `attempted_actions_json`
* `open_questions_json`
* `current_plan_json`
* `pending_approvals_json`
* `confidence`
* `summary_markdown_ref` nullable

Indexes:

* `(updated_at desc)`

Catatan: untuk v1, `attempted_actions_json` di JSONB masih oke.
Kalau nanti analitik attempted actions penting, pecah ke tabel sendiri.

---

## 9.2 `repo_memory`

### Columns

* `repo_id` PK
* `updated_at`
* `build_commands_json`
* `test_commands_json`
* `lint_commands_json`
* `format_commands_json`
* `critical_paths_json`
* `architecture_summary_json`
* `common_failure_patterns_json`
* `known_flaky_tests_json`
* `dangerous_modules_json`
* `freshness_ts`

Indexes:

* `(updated_at desc)`

---

## 9.3 `team_memory`

### Columns

* `team_id` PK
* `updated_at`
* `review_preferences_json`
* `risk_policies_json`
* `pr_preferences_json`

---

## 9.4 `operational_memory`

Saya sarankan operational memory tidak hanya satu row besar. Pecah.

### `provider_incidents`

* `incident_id` PK
* `provider`
* `issue`
* `severity`
* `started_at`
* `resolved_at` nullable
* `metadata_json`

### `tool_failure_signatures`

* `signature_id` PK
* `tool`
* `pattern`
* `recommended_recovery`
* `created_at`
* `updated_at`
* `confidence`

### `env_signals`

* `signal_id` PK
* `workspace_id`
* `repo_id`
* `issue`
* `last_seen`
* `severity`
* `metadata_json`

Indexes:

* provider incidents: `(provider, started_at desc)`
* tool failure signatures: `(tool, updated_at desc)`
* env signals: `(workspace_id, last_seen desc)`, `(repo_id, last_seen desc)`

---

# 10. Error & retry domain

---

## 10.1 `normalized_errors`

### Columns

* `error_id` PK
* `task_id` FK
* `step_id` nullable
* `step_run_id` nullable
* `timestamp`
* `scope`
* `error_class`
* `error_family`
* `severity`
* `retryable`
* `retry_strategy`
* `component`
* `message`
* `structured_context_json`
* `artifact_refs_json`
* `recommended_next_action`
* `escalate_if_repeated`

Indexes:

* `(task_id, timestamp desc)`
* `(error_class, timestamp desc)`
* `(error_family, timestamp desc)`
* `(component, timestamp desc)`
* `(severity, timestamp desc)`

---

## 10.2 `retry_attempts`

### Columns

* `retry_attempt_id` PK
* `task_id` FK
* `step_id` FK nullable
* `step_run_id` FK nullable
* `error_id` FK
* `attempt_no`
* `strategy`
* `started_at`
* `ended_at`
* `result` (`succeeded`, `failed`, `aborted`)
* `notes_json`

Indexes:

* `(task_id, started_at desc)`
* `(error_id, attempt_no)`

---

## 10.3 `failure_bundles`

### Columns

* `bundle_id` PK
* `task_id` FK
* `step_id` nullable
* `class`
* `created_at`
* `failing_targets_json`
* `suspected_files_json`
* `log_excerpt_refs_json`
* `recommended_route`
* `metadata_json`

Indexes:

* `(task_id, created_at desc)`
* `(class, created_at desc)`

---

# 11. Sub-agent domain

---

## 11.1 `subagent_runs`

### Columns

* `subagent_id` PK
* `task_id` FK
* `role`
* `objective`
* `file_scope_json`
* `tool_scope_json`
* `status`
* `started_at`
* `ended_at` nullable
* `summary`
* `confidence`
* `blocking_issues_json`
* `recommended_next_action`
* `budget_json`
* `parent_step_id` nullable

Indexes:

* `(task_id, started_at desc)`
* `(task_id, role, status)`
* `(status, started_at desc)`

---

## 11.2 `subagent_artifacts`

Boleh juga hanya pakai FK dari `artifacts.subagent_id`, tapi tabel mapping terpisah kadang memudahkan.

---

# 12. Telemetry domain

Operational telemetry idealnya tidak hanya di Postgres.
Tetapi untuk v1, Anda bisa simpan event dasar di relational lalu sink ke warehouse.

---

## 12.1 `telemetry_events`

### Columns

* `event_id` PK
* `event_type`
* `timestamp`
* `task_id` nullable
* `step_id` nullable
* `step_run_id` nullable
* `subagent_id` nullable
* `producer`
* `payload_json`

Indexes:

* `(event_type, timestamp desc)`
* `(task_id, timestamp desc)`
* `(producer, timestamp desc)`

---

## 12.2 `usage_metrics`

### Columns

* `usage_metric_id` PK
* `task_id` FK
* `recorded_at`
* `model_calls`
* `tool_calls`
* `browser_actions`
* `connector_calls`
* `input_tokens`
* `output_tokens`
* `estimated_cost_usd`
* `wall_clock_ms`

Indexes:

* `(task_id, recorded_at desc)`
* `(recorded_at desc)`

---

# 13. Evaluation domain

Untuk benchmark/eval, saya sarankan domain terpisah walau satu Postgres boleh untuk awal.

---

## 13.1 `benchmark_suites`

### Columns

* `suite_id` PK
* `name`
* `category`
* `created_at`
* `updated_at`
* `description`
* `is_active`

---

## 13.2 `benchmark_tasks`

### Columns

* `benchmark_task_id` PK
* `suite_id` FK
* `category`
* `difficulty`
* `title`
* `goal`
* `inputs_json`
* `oracle_json`
* `grading_json`
* `tags_json`
* `created_at`
* `updated_at`

Indexes:

* `(suite_id)`
* `(category, difficulty)`

---

## 13.3 `benchmark_runs`

### Columns

* `run_id` PK
* `benchmark_task_id` FK
* `variant_json`
* `started_at`
* `ended_at`
* `status`
* `score`
* `success`
* `first_pass_success`
* `metrics_json`
* `failure_attribution_json`
* `report_ref` nullable

Indexes:

* `(benchmark_task_id, started_at desc)`
* `(success, started_at desc)`

---

## 13.4 `regression_reports`

### Columns

* `report_id` PK
* `created_at`
* `suite_id`
* `baseline_variant_json`
* `candidate_variant_json`
* `summary_json`
* `blocking_regressions_json`
* `report_ref`

Indexes:

* `(suite_id, created_at desc)`

---

# 14. Suggested ER relationships

Secara relasi inti:

* `tasks` 1:N `routing_decisions`
* `tasks` 1:N `task_plans`
* `task_plans` 1:N `plan_steps`
* `plan_steps` 1:N `step_runs`
* `tasks` 1:N `checkpoints`
* `tasks` 1:N `approvals`
* `tasks` 1:N `artifacts`
* `tasks` 1:1 `session_memory`
* `tasks` 1:N `normalized_errors`
* `tasks` 1:N `subagent_runs`
* `tasks` 1:N `telemetry_events`
* `tasks` 1:N `usage_metrics`

Dan:

* `repo_memory` keyed by `repo_id`
* `team_memory` keyed by `team_id`

---

# 15. Recommended physical split: transactional vs analytical

## 15.1 PostgreSQL transactional

Simpan:

* tasks
* plans
* steps
* step runs
* checkpoints
* approvals
* memory tables
* normalized errors
* artifact metadata
* subagent runs
* policy decisions

## 15.2 Object store

Simpan:

* diff/patch
* raw logs
* screenshots
* DOM snapshots
* junit/xml/json
* benchmark fixtures artifacts
* summaries markdown panjang
* browser traces

## 15.3 Warehouse / OLAP

Simpan hasil sink dari:

* telemetry events
* usage metrics
* benchmark runs
* failure distributions
* routing outcomes

---

# 16. Query patterns yang harus dioptimalkan

Ini penting karena banyak desain DB terlihat bagus di atas kertas, tapi lambat untuk workflow nyata.

## 16.1 Query operasional utama

* “ambil task aktif terbaru”
* “ambil semua pending approvals”
* “ambil checkpoint terbaru task X”
* “ambil error terbaru task X”
* “ambil artifacts task X”
* “ambil plan + steps task X”
* “ambil step runs gagal terakhir”
* “ambil subagent runs untuk task X”

## 16.2 Query observability

* “top error_class 7 hari terakhir”
* “skill mana yang cost per success-nya paling tinggi”
* “router version mana yang paling banyak menyebabkan replans”
* “tool mana yang paling sering timeout”
* “repo mana yang paling sering menghasilkan flaky_env”

## 16.3 Query eval

* “success rate by skill_version”
* “first-pass success by router_version”
* “duplicate external write incidents”
* “approval miss incidents”

---

# 17. Retention policy

Tanpa retention policy, sistem akan cepat bengkak dan mahal.

---

## 17.1 Operational DB retention

### Simpan lama:

* tasks
* checkpoints metadata
* approvals
* normalized errors
* routing decisions
* benchmark runs
* repo/team memory

### TTL / archive:

* telemetry events mentah
* usage metrics granular
* step_runs detail sangat lama
* low-value temporary state

Contoh:

* `telemetry_events` mentah: 30–90 hari di Postgres, lalu archive
* `step_runs` detail: 90–180 hari, lalu aggregate/archive
* `usage_metrics` granular: 90 hari, lalu rollup harian

---

## 17.2 Artifact retention

### `short`

Untuk:

* temp browser snapshots
* intermediate logs
* scratch outputs

TTL: 7–14 hari

### `standard`

Untuk:

* diffs
* test summaries
* review reports
* PR body drafts

TTL: 30–180 hari

### `audit`

Untuk:

* approval-related artifacts
* incident hotfix artifacts
* benchmark reports
* release-candidate evidence

TTL: 1 tahun atau lebih, tergantung policy

---

# 18. Partitioning strategy

Kalau volume mulai besar, partitioning membantu.

## 18.1 Tabel cocok dipartisi berdasarkan waktu

* `telemetry_events`
* `usage_metrics`
* `normalized_errors`
* `step_runs`
* `benchmark_runs`

Partition:

* bulanan atau mingguan tergantung volume

## 18.2 Tabel yang sebaiknya tidak dipartisi dulu

* `tasks`
* `checkpoints`
* `approvals`
* `repo_memory`
* `team_memory`

---

# 19. Consistency and transactions

## 19.1 Harus transactional

* create task + initial routing attach
* state transition task
* create approval + set task blocked
* create checkpoint + update task current_checkpoint_id
* attach active plan + update task current_plan_id

## 19.2 Eventually consistent boleh

* artifact blob upload setelah metadata row
* telemetry sink ke warehouse
* search indexing
* weekly rollups
* benchmark leaderboard updates

---

# 20. Idempotency persistence

Karena API contracts tadi butuh idempotency, Anda perlu storage untuk itu.

## 20.1 `idempotency_keys`

### Columns

* `idempotency_key` PK
* `scope` (`task_create`, `checkpoint_create`, `connector_write`, dll)
* `request_hash`
* `response_ref`
* `created_at`
* `expires_at`

Indexes:

* `(scope, created_at desc)`
* `(expires_at)`

Ini penting terutama untuk:

* connector writes
* approvals
* artifacts
* checkpoint saves

---

# 21. Search layer design

Untuk v1, mungkin cukup Postgres + JSONB + FTS.
Tetapi untuk skala lebih besar, saya sarankan dedicated search.

## Cocok untuk diindeks:

* task goal
* plan summary
* error message
* failure bundle summaries
* review reports
* benchmark titles/goals
* artifact textual summaries

## Search documents minimal fields:

* `doc_type`
* `task_id`
* `repo_id`
* `team_id`
* `created_at`
* `text`
* `tags`
* `error_class` nullable
* `skill_name` nullable

---

# 22. Security and access boundaries

Walau ini internal, tetap perlu isolation.

## 22.1 Logical isolation

Minimal scope akses:

* by workspace
* by repo
* by team

## 22.2 Sensitive data handling

Jangan simpan secrets mentah di:

* tasks
* memory
* errors
* telemetry

Kalau ada referensi rahasia, simpan pointer atau redacted form.

## 22.3 Artifact scrubbing

Beberapa artifact perlu redaction pipeline:

* logs
* traces
* browser snapshots
* connector responses

---

# 23. Example minimal SQL-ish schema set for MVP

Kalau Anda ingin MVP cepat, cukup mulai dari 12 tabel ini:

1. `tasks`
2. `routing_decisions`
3. `task_plans`
4. `plan_steps`
5. `step_runs`
6. `checkpoints`
7. `artifacts`
8. `approvals`
9. `session_memory`
10. `repo_memory`
11. `normalized_errors`
12. `telemetry_events`

Dengan 12 ini saja, Anda sudah bisa menjalankan:

* orchestration
* retry
* checkpoint
* approval
* artifacts
* basic observability

---

# 24. Recommended migration order

## Phase 1

Bangun:

* tasks
* plans
* steps
* step_runs
* checkpoints
* artifacts metadata

## Phase 2

Bangun:

* approvals
* normalized_errors
* session_memory
* repo_memory
* routing_decisions

## Phase 3

Bangun:

* subagent_runs
* policy_decisions
* telemetry_events
* usage_metrics

## Phase 4

Bangun:

* benchmark tables
* leaderboard/report tables
* search index pipeline
* warehouse sink

---

# 25. Anti-pattern yang harus dihindari

## Jangan lakukan ini:

* menyimpan seluruh state task sebagai satu blob JSON tanpa relational structure
* mencampur artifact binary ke Postgres
* menyimpan transcript mentah sebagai memory utama
* membuat approvals hanya sebagai field boolean pada task
* tidak memberi FK / indexes pada task-centric tables
* tidak punya retention class untuk artifacts
* tidak punya idempotency table untuk write actions
* menaruh semua telemetry analitik di DB transaksional selamanya

---

# 26. Ringkasan akhir G

Kalau saya padatkan, desain persistence ini bertumpu pada 7 keputusan:

1. **Postgres untuk state transaksional**
2. **object storage untuk artifact besar dan immutable**
3. **warehouse/search layer untuk analytics dan observability**
4. **task-centric relational model untuk orchestration**
5. **memory dipisah per scope**
6. **event dan retry/error disimpan sebagai first-class entities**
7. **retention, idempotency, dan auditability dibangun dari awal**

---



#### PerplexityComputerV2.part10.md

# J. 90-Day Engineering Execution Plan

## 1. Sasaran 90 hari

Pada akhir 90 hari, target realistisnya bukan “sempurna”, tetapi:

1. OpenClaw bisa menjalankan **3 skill utama** end-to-end:

   * `bug_triage`
   * `test_repair`
   * `issue_to_pr`

2. Sistem punya:

   * router v1,
   * execution loop v1,
   * checkpoint/resume,
   * approval flow,
   * artifact persistence,
   * retry/error normalization,
   * benchmark suite v1,
   * telemetry dashboard minimum.

3. Ada baseline numerik untuk:

   * success rate
   * first-pass success
   * cost per resolved task
   * retry rate
   * approval correctness
   * resume success

4. Ada release candidate internal yang bisa dipakai untuk lane engineering tertentu.

---

# 2. Organisasi workstream

Saya sarankan pekerjaan dibagi ke 6 workstream:

## WS1 — Control Plane & Orchestration

Fokus:

* task lifecycle
* router
* planner wiring
* execution loop
* approvals
* checkpoint/resume

## WS2 — Worker Runtime & Tools

Fokus:

* exec
* patch modes
* tests/build execution
* tool result normalization
* browser/runtime hooks bila perlu

## WS3 — Data & Persistence

Fokus:

* Postgres schema
* repositories/storage-sdk
* artifact store
* retention
* idempotency

## WS4 — Skills & Policy

Fokus:

* 7 skill contracts
* implementasi 3 skill awal
* policy engine
* approval matrix
* escalation rules

## WS5 — Eval & Telemetry

Fokus:

* benchmark registry
* benchmark runner
* grading/scoring
* regression gates
* telemetry rollups

## WS6 — Reliability & Hardening

Fokus:

* retry engine
* error taxonomy
* recovery playbooks
* failover/degraded mode
* incident handling

Kalau tim Anda kecil, 6 workstream ini bisa dipegang 3–4 orang, tetapi pembagian logisnya tetap sama.

---

# 3. Fase besar 90 hari

Bagi 90 hari menjadi 6 fase masing-masing 2 minggu, lalu 1 minggu buffer/stabilization tersisa.

* **Fase 1**: fondasi contracts + persistence + task model
* **Fase 2**: routing + planning + execution loop dasar
* **Fase 3**: patch/test runtime + skill awal
* **Fase 4**: approvals + checkpoints + retry/recovery
* **Fase 5**: benchmark/eval + production telemetry
* **Fase 6**: hardening + RC internal
* **Minggu buffer**: bug bash, performance tuning, release prep

---

# 4. Minggu 1–2 — Fondasi sistem

## Tujuan

Membuat tulang punggung yang stabil:

* contracts,
* schema inti,
* DB dasar,
* task lifecycle minimum,
* artifact metadata pipeline.

## Deliverables

* monorepo structure jadi
* `contracts` package v1
* `domain-core` v1
* `storage-sdk` v1
* migrasi DB awal
* tabel inti:

  * `tasks`
  * `routing_decisions`
  * `task_plans`
  * `plan_steps`
  * `step_runs`
  * `checkpoints`
  * `artifacts`
  * `normalized_errors`
* object storage wiring
* `control-plane` app skeleton
* `worker-runtime` app skeleton
* `ops-cli` skeleton

## Detail pekerjaan

### WS1

* buat task state machine
* implement `createTask`, `getTask`, `transitionTask`
* tambah lifecycle guards

### WS2

* definisikan `ToolResult` contract
* implement worker job envelope dasar
* stub `exec`, `patch`, `test`

### WS3

* buat migrations
* buat repository layer
* integrasi object storage untuk artifacts
* implement idempotency table

### WS4

* buat skill contract registry kosong
* definisikan approval classes
* definisikan policy enums

### WS5

* buat telemetry event schema
* event ingestion minimal
* cost/usage placeholder pipeline

### WS6

* buat `NormalizedError` registry
* mapping error_family dasar
* basic retry enums

## Exit criteria

* task bisa dibuat dan disimpan
* plan/step/checkpoint/artifact row bisa disimpan dan dibaca
* worker-runtime menerima job dummy dan mengembalikan `ToolResult`
* schema validation berjalan
* object store upload/download jalan
* telemetry event bisa dikirim

## Risks

* contracts berubah terus dan mengacaukan implementasi
* terlalu cepat menulis app logic sebelum schema stabil

## Mitigasi

* freeze schema inti di akhir minggu 2
* semua perubahan schema inti harus lewat review singkat/ADR

---

# 5. Minggu 3–4 — Routing + planning + orchestration loop dasar

## Tujuan

Membuat task benar-benar bisa:

* dirutekan,
* diplan,
* dijalankan per step,
* dan menghasilkan transisi state nyata.

## Deliverables

* `router-engine` v1
* `planner-engine` v1
* `execution-engine` loop dasar
* API:

  * `/tasks`
  * `/router/route`
  * `/planning/build`
  * `/checkpoints`
* step dispatcher dasar:

  * `read_context`
  * `repo_search`
  * `patch`
  * `test`
  * `review`
* `session_memory` v1

## Detail pekerjaan

### WS1

* implement `route_task`
* implement `build_task_plan`
* implement `execute_task`
* implement `select_next_step`

### WS2

* worker job تنفيذ untuk:

  * `repo.rank_files_for_task` stub/real minimal
  * `patch.inline`
  * `test.run_targeted` stub/real minimal
* normalisasi success/failure step

### WS3

* tambah tabel `session_memory`
* tambah `policy_decisions`
* repository untuk plans/steps/session memory

### WS4

* implement skill selection heuristic v1
* hubungkan routing dengan skill choice
* tetapkan default roles per skill

### WS5

* emit step-level telemetry
* mulai simpan usage metrics per task

### WS6

* integrasikan retry engine ke failed-step handler
* implement `invalid_patch`, `tool_timeout`, `approval_required` basics

## Exit criteria

* task bisa jalan dari `CREATED` → `PLANNED` → `CONTEXT_READY` → `EXECUTING`
* step berhasil/gagal mengubah status task dengan benar
* session memory ter-update
* checkpoint post-plan dan post-context tersimpan
* reroute dasar bisa bekerja setelah step gagal

## Risks

* router terlalu dini jadi rumit
* planner menghasilkan step yang tidak executable

## Mitigasi

* batasi kinds step dulu
* validasi plan secara keras
* jangan beri planner kebebasan liar

---

# 6. Minggu 5–6 — Patch/test runtime + 3 skill awal

## Tujuan

Membuat OpenClaw bisa benar-benar useful untuk lane engineering dengan 3 skill pertama.

## Skills target

* `bug_triage`
* `test_repair`
* `issue_to_pr`

## Deliverables

* `skill-engine` v1
* implementasi 3 skill awal
* repo context preparation v1
* patch mode awal:

  * inline
  * structured
* targeted test execution v1
* failure bundle creation v1

## Detail pekerjaan

### WS1

* wire skill selection ke orchestration
* enforce execution graph per skill

### WS2

* implement:

  * `patch.inline`
  * `patch.structured`
  * syntax validation
  * `test.run_targeted`
  * `logs.parse_failure_bundle`

### WS3

* persistence untuk `failure_bundles`
* artifact links untuk diff/test summary

### WS4

* implement 3 skill:

  * preflight
  * graph
  * success criteria
  * escalation rules
* default policy untuk tiap skill

### WS5

* benchmark fixtures awal untuk 3 skill
* grader deterministik minimal

### WS6

* error mapping untuk:

  * assertion_failure
  * import/type failure
  * invalid_patch
  * flaky_env
* recovery playbook awal untuk patch/test

## Exit criteria

* `bug_triage` menghasilkan:

  * failure bundle
  * suspected files
  * next-action memo
* `test_repair` bisa:

  * patch
  * rerun targeted tests
  * produce repair summary
* `issue_to_pr` bisa:

  * patch
  * targeted verify
  * generate PR artifacts tanpa connector write dulu

## Risks

* repo intelligence belum cukup kuat
* failing tests terlalu sulit dipetakan ke related files

## Mitigasi

* gunakan heuristik sederhana dulu:

  * filename proximity
  * symbol search
  * recent changes
* jangan tunggu perfect repo indexer untuk mulai useful

---

# 7. Minggu 7–8 — Approvals + checkpoints + retry/recovery

## Tujuan

Membuat runtime berhenti rapuh:

* tidak retry membabi buta
* bisa block dengan rapi
* bisa resume
* bisa recover dari failure umum

## Deliverables

* `approval-service` v1
* `policy-engine` v1
* checkpoint restore
* resume flow
* retry matrix v1 active
* patch mode fallback
* verifier integration v1

## Detail pekerjaan

### WS1

* `block_for_approval`
* `resume_task_from_checkpoint`
* state transition ke/dari `BLOCKED`, `RETRYING`

### WS2

* patch fallback:

  * inline → structured → branch_patch minimal
* env/tool timeout handling
* retry wrapper untuk worker jobs

### WS3

* tabel approvals lengkap
* tabel retry_attempts
* tabel workspace_snapshots minimal
* restore path dari snapshot/git ref

### WS4

* approval matrix:

  * lockfile
  * package install
  * external write
  * sensitive paths
* reviewer mandatory rules

### WS5

* benchmark cases untuk:

  * approval block
  * invalid patch recovery
  * resume after checkpoint

### WS6

* implement verifier role integration
* failed-step handler class-aware
* escalation ke planner setelah patch failures berulang

## Exit criteria

* approval request bisa dibuat dan diselesaikan
* task bisa pause dan resume
* invalid patch recovery bekerja minimal 2 mode
* verifier dipanggil setelah failure bundle
* duplicate retry tanpa strategy tidak terjadi

## Risks

* checkpoint restore tidak konsisten dengan workspace state
* approval flow menggantung task

## Mitigasi

* minimal gunakan git-based snapshot dulu
* semua approval harus punya timeout/expired state
* setiap blocked task wajib punya `resume_instruction`

---

# 8. Minggu 9–10 — Benchmark/eval + telemetry operasional

## Tujuan

Mulai membuktikan sistem bekerja, bukan hanya terlihat jalan.

## Deliverables

* `benchmark-engine` v1
* `eval-runner` app
* benchmark suites awal
* grading pipeline
* failure attribution v1
* regression gate v1
* weekly metrics rollup minimum

## Detail pekerjaan

### WS1

* tagging task dengan version:

  * router_version
  * skill_version
  * retry_policy_version

### WS2

* ensure worker outputs cukup kaya untuk evaluasi
* pastikan artifact refs konsisten

### WS3

* benchmark tables
* run/results persistence
* sink telemetry ke analytics store minimal

### WS4

* skill-specific scorecards:

  * issue_to_pr
  * bug_triage
  * test_repair

### WS5

* offline suite v1:

  * 30 bug triage tasks
  * 30 test repair tasks
  * 30 issue-to-pr tasks
* graders:

  * deterministic checks
  * artifact checks
  * success scoring
* leaderboard internal minimal

### WS6

* failure attribution categories:

  * routing
  * planning
  * patch
  * verification
  * policy
  * env
* top-error weekly summary

## Exit criteria

* benchmark suite bisa dijalankan otomatis
* tiap run punya score + attribution
* regression gate bisa memblok perubahan buruk
* weekly report minimum tersedia

## Risks

* benchmark tasks terlalu sedikit atau terlalu mudah
* graders terlalu subjektif

## Mitigasi

* prioritaskan deterministic oracles
* gunakan model-assisted judge seminimal mungkin
* tambah replay tasks dari failure nyata secepatnya

---

# 9. Minggu 11–12 — Hardening + release candidate internal

## Tujuan

Menstabilkan sistem agar layak dipakai internal untuk lane sempit.

## Deliverables

* hardening retry paths
* repo memory v1
* reviewer integration mandatory paths
* production telemetry rollups
* degraded mode
* release candidate internal
* ops runbooks dasar

## Detail pekerjaan

### WS1

* enforce invariants:

  * no task dual-state
  * checkpoint on major transitions
  * approval before sensitive external writes
* finalize orchestration metrics

### WS2

* stabilkan runtime:

  * timeout tuning
  * log streaming
  * worker isolation
  * branch_patch minimal production-ready

### WS3

* retention jobs
* artifact cleanup jobs
* DB indexes tuning
* snapshot cleanup

### WS4

* repo memory updates dari completed tasks
* reviewer mandatory on high-risk paths
* policy tuning dari data minggu sebelumnya

### WS5

* expand benchmark set
* add replay benchmark cases
* candidate-vs-baseline evaluation report

### WS6

* degraded mode:

  * provider failover preference
  * disable optional sub-agents
  * stricter fail-fast on unstable env
* reliability dashboards minimum

## Exit criteria

* 3 skill inti usable internal
* regression gate aktif
* retry/error handling stabil untuk error utama
* approval miss rate = 0 pada test suite
* duplicate external write rate = 0
* release candidate internal bisa dipakai untuk scoped usage

## Risks

* terlalu banyak bug operasional kecil
* benchmark memperlihatkan success rendah di task medium

## Mitigasi

* fokus lane sempit dulu
* mark skill maturity jujur
* jangan memaksa autonomous mode terlalu dini

---

# 10. Minggu 13 — Buffer / bug bash / polish

## Tujuan

Minggu ini jangan diisi fitur besar. Gunakan untuk:

* bug bash
* benchmark stabilization
* perf tuning
* incident fixes
* docs/runbooks
* hard cut release scope

## Deliverables

* bug backlog burn-down
* RC notes
* known limitations doc
* operator runbook
* rollback playbook
* release checklist

---

# 11. Definition of done per komponen

## 11.1 Router

Selesai jika:

* bisa klasifikasi task class
* bisa pilih skill
* bisa hasilkan roles/tool plan/budget
* ada benchmark accuracy minimum
* reroute bekerja setelah error utama

## 11.2 Execution engine

Selesai jika:

* task berjalan step-by-step
* success/failure handler stabil
* checkpoint dibuat di titik utama
* blocked/resume bekerja
* telemetry event emitted

## 11.3 Retry engine

Selesai jika:

* error dinormalisasi
* retry class-aware
* escalation rule bekerja
* blind retry tidak terjadi di path sensitif

## 11.4 Approval engine

Selesai jika:

* approval object dibuat
* task block/resume benar
* policy decisions tercatat
* expired/rejected approval ditangani

## 11.5 Skills

Selesai jika:

* preflight checks aktif
* execution graph enforced
* required artifacts dihasilkan
* success criteria bisa dinilai otomatis

## 11.6 Eval framework

Selesai jika:

* benchmark suite runnable
* grading reproducible
* attribution tersedia
* regression gate bisa dipakai pre-release

---

# 12. Owner matrix yang disarankan

Berikut pembagian ownership model, bukan nama orang.

## Lead A — Runtime lead

Owner:

* WS1
* sebagian WS6

## Lead B — Worker/tools lead

Owner:

* WS2

## Lead C — Data/infra lead

Owner:

* WS3

## Lead D — Skills/policy lead

Owner:

* WS4

## Lead E — Eval/quality lead

Owner:

* WS5

Kalau tim kecil:

* A + D bisa satu orang
* B + sebagian WS6 bisa satu orang
* C + E bisa satu orang

---

# 13. Dependency graph antar workstream

## Dependency kritis

* WS1 bergantung pada WS3 untuk persistence dasar
* WS4 bergantung pada WS1/WS2 untuk menjalankan skill graph
* WS5 bergantung pada WS1/WS2/WS4 untuk task outputs yang benar
* WS6 bergantung pada WS1/WS2/WS3 karena retry/recovery menyentuh semuanya

## Urutan dependency praktis

1. schema + storage
2. task lifecycle
3. router + planner
4. execution step runtime
5. skills
6. approvals + retries
7. eval
8. hardening

---

# 14. Risk register

Saya sarankan Anda maintain risk register dari awal. Berikut daftar awalnya.

## R1 — Router salah memilih skill

Dampak:

* cost naik
* success turun
* replans meningkat

Mitigasi:

* router benchmark sendiri
* heuristic hard rules untuk lane penting

## R2 — Patch engine terlalu rapuh

Dampak:

* invalid patch tinggi
* developer trust turun

Mitigasi:

* multi-mode patch
* syntax validation wajib
* planner escalation after 2 failures

## R3 — Workspace restore tidak konsisten

Dampak:

* resume gagal
* duplicated work

Mitigasi:

* gunakan snapshot berbasis git terlebih dulu
* checkpoint + snapshot integration tests

## R4 — Approval flow memperlambat semua hal

Dampak:

* babysitting tinggi
* UX buruk walau tanpa UI khusus

Mitigasi:

* policy matrix jelas
* auto untuk safe reads/local edits/tests
* approval hanya untuk sensitive boundaries

## R5 — Benchmark overfit

Dampak:

* angka bagus, produk nyata jelek

Mitigasi:

* replay benchmarks
* production telemetry
* refresh benchmark bulanan

## R6 — Too much infrastructure too early

Dampak:

* velocity turun
* tim sibuk wiring, bukan outcome

Mitigasi:

* modular monolith first
* service split nanti

## R7 — Connector writes menyebabkan duplicate side effects

Dampak:

* trust collapse

Mitigasi:

* idempotency keys
* postcondition verification
* never blind retry write

---

# 15. Weekly review cadence

Saya sarankan ada 3 review rutin:

## 15.1 Weekly engineering review

Lihat:

* milestone progress
* blockers
* schema/API changes
* risk register updates

## 15.2 Weekly eval review

Lihat:

* success rate by skill
* top failures
* benchmark deltas
* router misclassifications
* cost trends

## 15.3 Weekly reliability review

Lihat:

* error families
* retries by class
* approval incidents
* partial write ambiguity
* checkpoint resume failures

Tanpa 3 review ini, sistem akan cepat drift.

---

# 16. Suggested milestone checkpoints

## Day 14 milestone

* schemas stabil
* task lifecycle hidup
* persistence dasar jadi

## Day 28 milestone

* router + planner + loop dasar hidup

## Day 42 milestone

* 3 skill awal hidup secara terbatas

## Day 56 milestone

* approvals + checkpoints + retries hidup

## Day 70 milestone

* benchmark/eval runner aktif

## Day 84 milestone

* internal RC untuk lane sempit siap

## Day 90 milestone

* stabilization done
* baseline metrics dipublikasikan
* next-quarter plan siap

---

# 17. Go / no-go criteria untuk internal RC

OpenClaw layak masuk internal RC jika:

* 3 skill inti bisa dijalankan end-to-end
* success rate benchmark melewati baseline minimum
* approval miss rate = 0
* duplicate external write rate = 0
* checkpoint resume rate cukup stabil
* top failure classes sudah dipahami
* ops runbook tersedia
* known limitations terdokumentasi

Kalau salah satu safety invariant gagal, jangan release RC.

---

# 18. Setelah 90 hari, apa yang seharusnya terjadi

Kalau plan ini berjalan baik, pada akhir 90 hari Anda seharusnya punya:

## Yang benar-benar ada

* platform runtime v1
* 3 skill production-like
* 4 skill lainnya sudah terdefinisi dan sebagian mulai dibangun
* baseline benchmark
* evaluasi per variant
* release candidate internal

## Yang belum harus sempurna

* browser plane luas
* connector depth luas
* autonomous incident workflows
* cost optimization halus
* sub-agent sophistication penuh

Itu wajar. Fokus utama 90 hari pertama adalah **membuat fondasi yang benar**.

---

# 19. Ringkasan eksekusi 90 hari

Kalau saya padatkan, urutan terbaiknya adalah:

1. **bangun schema + persistence**
2. **bangun lifecycle + orchestration**
3. **hidupkan runtime patch/test**
4. **bungkus jadi 3 skill bernilai tinggi**
5. **tambahkan approvals + retries + checkpoint**
6. **ikat semuanya ke eval framework**
7. **hardening sampai layak RC**

Jangan membalik urutan ini.

---

# 20. Rekomendasi terakhir

Kalau saya jujur, ada dua jebakan terbesar yang harus Anda hindari selama 90 hari ini:

## Jebakan 1

Terlalu cepat mengejar breadth:

* browser banyak,
* connectors banyak,
* workflows banyak,
  padahal 3 skill inti belum solid.

## Jebakan 2

Terlalu lama di arsitektur tanpa shipping evaluable system.

Karena itu, saya sarankan prinsip operasional tim ini:

> **ship narrow, measure hard, harden systematically.**

---
