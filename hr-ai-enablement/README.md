# HR AI Enablement

## Project purpose and status

A working project for evaluating and delivering AI-enabled HR capabilities in a global company. The project name is provisional. HR transactions through Claude Desktop and alternative interfaces are the first workstream, not the full project scope.

This workstream is maintained in the ResearchExamples Git repository. GitHub Desktop repository registration is separate from adding a local project in Codex.

## Current direction

Claude is the initial HR workspace for research, reporting, analysis, transactions, and purpose-built apps. Copilot Studio is reserved for future agent efforts. Start with the [workspace, application platform, and SDLC plan](workstreams/02-rapid-development/hr-workspace-app-platform-and-sdlc.md), then review the transaction research below.

## Review order and document inventory

| Order | Document | What to review |
|---|---|---|
| 1 | [Recommendation and decision plan](workstreams/01-hr-transactions/research/hr-recommendation-and-decision-plan.md) | Preferred direction, tradeoffs, first operation, rollout, and findings that would change the recommendation |
| 2 | [Global HR research and plan](workstreams/01-hr-transactions/research/claude-desktop-global-hr-plan.md) | Full evidence, companies/community examples, approaches, wrapper value, identity, governance, and pilot requirements |
| 3 | [Build-versus-buy vendor options](workstreams/01-hr-transactions/research/hr-build-buy-vendor-options.md) | CData, Copilot Studio, Workato, Workday-native, Boomi, MuleSoft, Azure API Management, and procurement tests |
| 4 | [Architecture and enforcement guide](workstreams/01-hr-transactions/research/hr-architecture-and-enforcement.md) | Detailed diagrams, OAuth, distribution, approval state, and acceptance tests by approach |
| 5 | [Workday SOAP versus REST wrapper](workstreams/01-hr-transactions/research/workday-soap-versus-rest-wrapper.md) | Microsoft ESS sample inspection, direct SOAP versus wrapper tradeoffs, and updated authentication assumptions |

The new workstream adds a sixth substantive research document. This README is the seventh document: the project overview and reading guide. The original four research documents were reviewed by specialist subagents; their review records and limitations remain in the documents. This index only organizes that work.

## Where the different approaches are covered

| Approach | Primary reading |
|---|---|
| Claude with remote MCP | Global plan §5; architecture guide §§2–4 |
| Local desktop extension / local MCP | Global plan §5; architecture guide §5 |
| Custom application with API tool calling | Global plan §5; architecture guide §6 |
| Managed integration platform | Architecture guide §7; vendor comparison §§4–5 |
| HR-vendor-native tools | Architecture guide §7; vendor comparison §4.2 |
| Copilot Studio with MCP or a REST connector | Vendor comparison §3 |
| Build, buy, or hybrid over the existing wrapper | Vendor comparison §§5–7; recommendation §§4 and 8 |

APIs, MCP, tools, and agent platforms are different layers and can be combined. The recommendation selects a preferred starting combination; the other documents explain alternatives.

## Workstreams

| Workstream | Status | Scope |
|---|---|---|
| 01 — HR transactions and interface evaluation | Research completed; decisions and pilot remain open | Four reviewed research documents plus the SOAP-versus-wrapper technical note |
| 02 — HR workspace and rapid development | Architecture/SDLC research drafted and reviewed; implementation not started | [Application platform and SDLC](workstreams/02-rapid-development/hr-workspace-app-platform-and-sdlc.md); Azure hosting, shared identity, secure pipelines, and company precedents |
| Further workstreams | To be defined with the project owner | This structure does not assume that HR transactions describe the whole initiative |

Cross-cutting concerns include identity, security, regional data handling, integration ownership, training, support, and measurement. Their initial research is included in workstream 01; project-wide requirements should be established as the broader scope becomes clear.

## Working conventions

Use the documents in this repository for subsequent revisions. Earlier copies delivered in the original Codex workspace remain historical snapshots; they are not synchronized automatically.

Preserve the distinction between verified facts, vendor claims, design recommendations, and untested assumptions. Recheck current product documentation and entitlements before procurement or implementation. Do not place credentials or real employee records in research documents.

No live HR integration, production deployment, procurement, or AskHR retirement has been performed. The existing SOAP-to-REST wrapper has not been inspected. Start development only from an agreed operation and its identity, approval, execution, audit, and recovery requirements.
