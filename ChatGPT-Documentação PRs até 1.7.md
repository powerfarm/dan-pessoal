# Documentação PRs até 1.7

**User:** Anonymous (dcamarilho@gmail.com)  
**Created:** 10/9/2025 4:14:51  
**Updated:** 10/9/2025 5:37:22  
**Exported:** 10/21/2025 13:59:05  
**Link:** [https://chatgpt.com/c/68e728a5-c250-8327-808f-440703731c3d](https://chatgpt.com/c/68e728a5-c250-8327-808f-440703731c3d)  

## Prompt:
Com certeza. Com base nos arquivos que você forneceu, aqui está uma documentação detalhada e realista para cada um dos Pull Requests (PRs) que foram concluídos até o final do PR 1.7.

A documentação adota um formato profissional, ideal para ser usado como descrição do PR em plataformas como GitHub ou para arquivamento interno.

---

### **Visão Geral do Progresso (Após PR 1.7)**

O projeto está em uma condição excelente. A execução até o PR 1.7 não apenas completou a fundação do **Núcleo**, mas também implementou a primeira peça crítica da **Órbita** (vv-timers) com uma robustez notável (DLQ, backoff). As ferramentas de suporte ao desenvolvedor (Linter, Simulator, Explain para policies) já estão prontas, mitigando proativamente o risco de complexidade futura. O progresso está alinhado com o plano hiper-granular e a qualidade da engenharia é altíssima.

---

### **Documentação Detalhada dos Pull Requests Concluídos**

### **Week 0 — Setup & Fundamentos**

#### **[CICLO-031] docs(arch): ADRs 001–004**

**Closes CICLO-031**

##### Summary
This PR establishes the foundational architectural decisions for the project by creating four key Architecture Decision Records (ADRs). These documents ensure that the rationale behind major technical choices is recorded and accessible to the entire team, both now and in the future, preventing future debates and providing clarity on constraints.

##### Key Changes & Rationale
*   **Created ADR-001-SQLite-vs-Postgres.md**: Documents the strategic decision to use SQLite for its simplicity, zero-dependency deployment, and local-first performance, accepting the trade-off of no native HA. A Postgres adapter is planned for future phases.
*   **Created ADR-002-HTMX-vs-React.md**: Formalizes the choice of SSR with HTMX to reduce frontend complexity, minimize JavaScript bundle sizes, and improve Time-To-First-Byte (TTFB). Acknowledges that Web Components can be used for isolated, richer interactions where necessary.
*   **Created ADR-003-HLC-vs-Lamport.md**: Specifies the use of Hybrid Logical Clocks (HLCs) for event ordering. This was chosen over simpler Lamport clocks to provide timestamps that are both causally consistent and reasonably close to wall-clock time, which is invaluable for debugging and user-facing timelines.
*   **Created ADR-004-Federation-Conflict-Policy.md**: Defines the deterministic strategy for resolving data conflicts in a federated environment. The policy (quorum-wins → state-priority → LWW(HLC)) ensures that merges are predictable and auditable, with unresolved conflicts escalating to a Case.

##### How to Test & Verify
1.  Review the content of each ADR file in the ciclo-coding-main/ directory for clarity and accuracy.
2.  Confirm that the decisions align with the principles laid out in CICLO_Blueprint_v1.3.md.

##### Rollback Plan
*   N/A (Documentation change). Revert the commit if the decisions need to be reconsidered.

---

#### **[CICLO-025] chore(devexp): PR-0.1 - Week 0 (setup & seeds)**

**Closes CICLO-025**

##### Summary
This PR bootstraps the entire CICLO project, setting up the development environment, foundational code structure, and initial data as outlined in WEEK0.md. Its goal is to create a zero-friction "Day 1" experience, enabling any developer to clone the repository and have a fully runnable, testable, and verifiable system in minutes.

##### Key Changes & Rationale
*   **Workspace & CI**: Initialized the Rust workspace via Cargo.toml, defining all core, orbit, and cmd crates. Set up a basic CI pipeline for formatting (cargo fmt), linting (cargo clippy), and testing (cargo test).
*   **Justfile**: Created the central Justfile to standardize development workflows. Commands like dev, reset, seed, and test abstract away underlying cargo commands, ensuring consistency.
*   **Seed Data**: Added initial seed files for one venda and one incidente, allowing for immediate end-to-end testing of core flows without manual setup.
*   **Initial Docs**: Created README.md and TUTORIAL.md to provide immediate context and guidance for new contributors.

##### How to Test & Verify
1.  Run just dev to ensure the Axum server starts without errors.
2.  Run just reset && just seed to verify that the database is created and populated. A ciclodb.sqlite file should appear.
3.  Run just test and just check to confirm the CI checks are passing locally.
4.  Run vv report daily and check that it produces a report based on the seed data.

##### Rollback Plan
*   N/A (Initial project setup). For major issues, delete the repository and re-clone.

---

### **Week 1 — MVP Funcional**

#### **[CICLO-001] feat(core): PR-1.1 - Esquema & Migrações (núcleo)**

**Closes CICLO-001**

##### Summary
This PR lays the physical foundation of the CICLO ledger by creating the initial database schema via SQL migration files. It defines all core tables and establishes critical performance and integrity settings like WAL mode, foreign key constraints, and initial indexes.

##### Key Changes & Rationale
*   **Created db/migrations/0001_init.sql**: Defines the schema for records, transitions, status, attestations, timers, cases, and tasks, directly mapping the models from the blueprint. Column types are chosen for efficiency and compatibility with Rust types.
*   **Created db/migrations/0002_indexes.sql**: Adds performance-critical indexes for the most common query patterns identified in the blueprint, such as lookups by (flow, entity_id), timer lookups (active, due_ms), and attestation lookups by (target_kind, target_id).
*   **Ledger Initialization**: The ledger connection logic now ensures that PRAGMA journal_mode=WAL; and PRAGMA foreign_keys=ON; are executed on every connection, guaranteeing better concurrency and data integrity.

##### How to Test & Verify
1.  Run just reset (which implicitly calls vv migrate).
2.  Verify that a ciclodb.sqlite file is created.
3.  Use a SQLite client to inspect the database. Confirm all tables and indexes from the migration files are present.
4.  Run PRAGMA foreign_key_list('transitions'); to confirm relationships are correctly established.

##### Rollback Plan
*   Migrations are forward-only. If a schema error is found, a new migration file (000X_fix_schema.sql) must be created to correct it. For local development, just reset will drop and recreate the database from scratch.

---

#### **[CICLO-002] feat(core): PR-1.2 - Ledger + HLC + Idempotência de Transição**

**Closes CICLO-002**

##### Summary
This PR implements the heart of the CICLO engine's transaction processing. It introduces the vv-ledger for ACID transactions, Hybrid Logical Clocks (HLC) for consistent event ordering, and a strong idempotency guarantee for all state transitions, a critical feature to prevent duplicate operations in distributed or unreliable client environments.

##### Key Changes & Rationale
*   **crates/core/vv-ledger**: Provides a safe, ergonomic interface for database transactions (begin_tx, commit, rollback), abstracting rusqlite details.
*   **crates/core/vv-record/src/hlc.rs**: Implements a simple HLC to assign a causally consistent and monotonic timestamp to every event.
*   **crates/core/vv-flow/src/apply.rs**: Contains the core apply_transition logic. It calculates an MD5 hash of the transition's key fields (flow, entity_id, from, to, actor_id, ts_hlc) to serve as an idempotency key. It also correctly updates the materialized status view only on Ok verdicts, fulfilling invariant I1.

##### How to Test & Verify
1.  Run the new unit tests for idempotency: cargo test -p vv-flow -- --test idempotency_check.
2.  Use the CLI to send the exact same vv flow move ... command twice. The first should succeed, and the second should fail with an idempotency error.
3.  Verify in the database that only one transition was recorded.

##### Rollback Plan
*   This is a core code change. Revert the commit using git revert <commit_hash>.

---

#### **[CICLO-003] feat(api): PR-1.3 - API/CLI mínimas (transition/status/timeline)**

**Closes CICLO-003**

##### Summary
This PR exposes the core engine functionality through a REST API and a CLI, making the system interactive and testable from the outside world for the first time. It provides the essential endpoints for manipulating and querying workflows.

##### Key Changes & Rationale
*   **crates/core/vv-api**: Implemented Axum handlers for:
    *   POST /flows/:flow/entities/:id/transition: The primary endpoint for driving state changes.
    *   GET /flows/:flow/entities/:id/status: For fetching the current state.
    *   GET /flows/:flow/entities/:id/timeline: For retrieving the full WDTW audit history.
*   **cmd/vv**: Added clap subcommands that mirror the API: vv flow move, vv flow status, and vv flow timeline. This provides a powerful tool for scripting, testing, and administration.

##### How to Test & Verify
1.  Perform an end-to-end flow using only the CLI:
    bash
    vv flow move venda 001 Criado --reason "test"
    vv flow move venda 001 Pago --reason "paid"
    vv flow status venda 001 # Should be "Pago"
    vv flow timeline venda 001 # Should show two transitions
2.  Repeat the same flow using curl against the REST API.

##### Rollback Plan
*   Code change. Revert the commit using git revert <commit_hash>.

---

#### **[CICLO-004] feat(policy): PR-1.4 - Engine de Policies (YAML) + Hot-reload**

**Closes CICLO-004**

##### Summary
This PR introduces the declarative policy engine, moving business logic out of hardcoded Rust and into human-readable YAML files. This is a cornerstone of the blueprint's "evolvability" principle. The engine includes hot-reloading, allowing rules to be updated on-the-fly without restarting the server.

##### Key Changes & Rationale
*   **crates/core/vv-policy**: Created the PolicyEngine which uses serde_yaml to parse .yml files into rule structures. The evaluate method processes the require/doubt_if/not_if blocks against a given transition context.
*   **Hot-Reload**: Integrated the notify crate to watch the policies/ directory for changes, triggering an atomic reload of the active policy set. This is critical for rapid iteration and operational agility.
*   **Policy Files**: Added initial policies policies/flows/venda.yml and incidente.yml.

##### How to Test & Verify
1.  Start the server with just dev.
2.  Make a venda.pagar transition that should be Ok.
3.  While the server is running, edit policies/flows/venda.yml to make the require block always fail (e.g., body.total_eur > 99999).
4.  Observe the server logs for a "Policies reloaded" message.
5.  Attempt the same venda.pagar transition. It should now fail with a Not verdict.

##### Rollback Plan
*   Code change: git revert <commit_hash>.
*   Operational rollback: Revert the changes to the YAML file in the policies/ directory and wait for the hot-reload to pick it up.

---

#### **[CICLO-005] feat(policy): PR-1.5 - Linter, Simulator e Explain de Policies**

**Closes CICLO-005**

##### Summary
This PR builds essential developer tooling around the policy engine to combat the risk of "YAML hell." The **Linter** validates syntax and schema, the **Simulator** tests rules against mock payloads, and the **Explain** feature provides runtime visibility into *why* a decision was made.

##### Key Changes & Rationale
*   **cmd/vv/src/policy_cli.rs**: Added new vv policy lint, vv policy test, and vv policy explain subcommands. The linter uses the policy.json schema for validation.
*   **crates/core/vv-policy/explain.rs**: Implemented the evaluate_with_explanation method, which returns not just the verdict but also a step-by-step trace of which rules were checked and how they contributed to the final decision.
*   **API Integration**: The /transition endpoint now accepts a ?explain=1 query parameter, which embeds the explanation trace in the JSON response.

##### How to Test & Verify
1.  Introduce a syntax error into venda.yml and run vv policy lint. It should fail with a clear error message.
2.  Run the simulator against the doubt.json payload: vv policy test -p tests/policies/test_flow.yml -d tests/payloads/doubt.json .... It should report a Doubt verdict.
3.  Call the API with ?explain=1 for a Doubt case and inspect the JSON response for the detailed explanation.

##### Rollback Plan
*   Code change. Revert the commit using git revert <commit_hash>.

---

#### **[CICLO-006] feat(core): PR-1.6 - Attestation & Quorum (witness)**

**Closes CICLO-006**

##### Summary
This PR implements the "two-eyes" principle by introducing attestations and quorum checks. This allows policies to enforce co-responsibility, requiring confirmation from specific roles (like a witness) within a defined time window before a transition is fully trusted.

##### Key Changes & Rationale
*   **crates/core/vv-attest**: Manages the creation and storage of Attestation records.
*   **crates/core/vv-quorum**: Contains the logic to check if the required number of attestations for a given target has been met.
*   **API Endpoints**: Added POST /attest for submitting attestations and GET /attest/quorum to check the status.
*   **Policy Integration**: The policy engine now evaluates the quorum: block in YAML files to determine the requirements for a transition.

##### How to Test & Verify
1.  Make a venda.pagar transition, which the policy requires a witness for.
2.  Call GET /attest/quorum?target=transition:<id>. The response should indicate that 1 witness is missing (pending).
3.  Use POST /attest (or vv attest add ...) to submit a witness attestation.
4.  Call the quorum endpoint again. The response should now be satisfied.

##### Rollback Plan
*   Code change. Revert the commit using git revert <commit_hash>.

---

#### **[CICLO-007] feat(orbit): PR-1.7 - Timers (batch, backoff, DLQ) + Métricas**

**Closes CICLO-007**

##### Summary
This PR implements a production-grade timer worker in the Orbit layer. This is the engine behind SLAs and the critical Auto-Despacho mechanism. It is explicitly designed for resilience and observability, featuring batch processing to handle scale, exponential backoff for transient failures, and a Dead-Letter Queue (DLQ) for isolating and handling unrecoverable errors.

##### Key Changes & Rationale
*   **crates/orbit/vv-timers**: A new crate containing a Tokio-based worker that polls the timers table every second.
*   **Resilience**:
    *   **Batching**: Fetches up to 256 due timers at a time (LIMIT 256) to prevent overwhelming the system.
    *   **Exponential Backoff**: If processing a timer's effects fails, it is rescheduled with an increasing delay, preventing poison pills from halting the entire queue.
    *   **Dead-Letter Queue (DLQ)**: After a configurable number of retries, a failing timer is marked as "poisoned" and a Case is opened in the special sre lane for manual intervention, as documented in RUNBOOK_DLQ_TIMERS.md.
*   **Observability**: Exposes crucial Prometheus metrics: timer_fired_total, timer_delay_seconds_p95, timer_retry_count, and timer_poison_total.
*   **Schema**: db/migrations/0003_timers_retry.sql adds columns to the timers table to track retries and last_error.

##### How to Test & Verify
1.  Create a transition that arms a short timer (e.g., the 10-minute await_witness timer in venda.yml).
2.  Wait for the timer to fire and observe that the AutoTransition to Despachado occurs and a Case is created.
3.  Check the /metrics endpoint and verify that timer_fired_total has incremented.
4.  (Advanced) Manually insert a timer whose policy effect is designed to fail, and verify that it retries and eventually lands in the sre lane in Powerfarm.

##### Rollback Plan
*   Code change. Revert the commit using git revert <commit_hash>. The migration 0003 is additive and non-breaking, so it can remain.

## Response:
Analisado

````python
# Create a polished Markdown document for PRs 1.0–1.7

doc = r"""# CICLO — Documentação de Pull Requests (PRs) até o PR 1.7

> **Escopo**: Este documento consolida, padroniza e aprofunda a documentação dos PRs concluídos entre a **Week 0** e a **Week 1**, cobrindo do **PR-0.1** ao **PR-1.7** e seus respectivos tickets **CICLO-031** a **CICLO-007**.
> **Uso**: Pode ser colado diretamente como descrição de PR no GitHub/GitLab, anexado ao changelog interno ou utilizado como base de auditoria técnica.

---

## Índice

1. [Visão Geral do Progresso](#visão-geral-do-progresso)
2. [Quadro-resumo (Changelog condensado)](#quadro-resumo-changelog-condensado)
3. [PRs Detalhados](#prs-detalhados)
   - Week 0 — Setup & Fundamentos
     - [CICLO-031 — ADRs 001–004](#ciclo-031-docsarch-adrs-001-004)
     - [CICLO-025 — PR-0.1 (setup & seeds)](#ciclo-025-choredevexp-pr-01---week-0-setup--seeds)
   - Week 1 — MVP Funcional
     - [CICLO-001 — PR-1.1 (Esquema & Migrações)](#ciclo-001-featcore-pr-11---esquema--migrações-núcleo)
     - [CICLO-002 — PR-1.2 (Ledger + HLC + Idempotência)](#ciclo-002-featcore-pr-12---ledger--hlc--idempotência-de-transição)
     - [CICLO-003 — PR-1.3 (API/CLI mínimas)](#ciclo-003-featapi-pr-13---apicli-mínimas-transitionstatustimeline)
     - [CICLO-004 — PR-1.4 (Engine de Policies + Hot-reload)](#ciclo-004-featpolicy-pr-14---engine-de-policies-yaml--hot-reload)
     - [CICLO-005 — PR-1.5 (Linter/Simulator/Explain de Policies)](#ciclo-005-featpolicy-pr-15---linter-simulator-e-explain-de-policies)
     - [CICLO-006 — PR-1.6 (Attestation & Quorum)](#ciclo-006-featcore-pr-16---attestation--quorum-witness)
     - [CICLO-007 — PR-1.7 (Timers + Métricas)](#ciclo-007-featorbit-pr-17---timers-batch-backoff-dlq--métricas)
4. [Pós-Merge: Checklist Operacional](#pós-merge-checklist-operacional)
5. [Anexos e Modelos](#anexos-e-modelos)

---

## Visão Geral do Progresso

O projeto está em **excelente condição**. A execução até o **PR-1.7** consolidou a fundação do **Núcleo** (ledger, HLC, idempotência, API/CLI, policies declarativas com hot-reload, observabilidade) e entregou a primeira peça crítica da **Órbita** (`vv-timers`) com **resiliência industrial** (batch, backoff exponencial, DLQ e métricas Prometheus). As ferramentas de suporte ao desenvolvedor (Linter, Simulator, Explain) reduzem riscos de complexidade futura. O progresso segue o plano hiper-granular com **alta qualidade de engenharia**.

---

## Quadro-resumo (Changelog condensado)

| PR | Ticket | Tipo | Área | Breaking? | Migração DB? | Principais entregas |
|---:|:------:|:----:|:----:|:---------:|:------------:|---------------------|
| PR-0.1 | CICLO-025 | chore | devexp | Não | Não | Workspace, CI básica, `Justfile`, seeds, docs iniciais |
| PR-1.1 | CICLO-001 | feat | core/db | Não | **Sim** (`0001`, `0002`) | Esquema inicial, índices, WAL/foreign_keys |
| PR-1.2 | CICLO-002 | feat | core/engine | Não | Não | Ledger ACID, HLC, idempotência de transição |
| PR-1.3 | CICLO-003 | feat | api/cli | Não | Não | REST (transition/status/timeline) + CLI espelhada |
| PR-1.4 | CICLO-004 | feat | policy | Não | Não | Policies em YAML + hot-reload |
| PR-1.5 | CICLO-005 | feat | policy/devtools | Não | Não | `vv policy lint/test/explain` + `?explain=1` |
| PR-1.6 | CICLO-006 | feat | core/policy | Não | Não | Attestations, quorum, endpoints `/attest` |
| PR-1.7 | CICLO-007 | feat | orbit/timers | Não | **Sim** (`0003`) | Worker de timers (batch/backoff/DLQ) + métricas |

> **Notas**: Migrações são **acumulativas** e **não-quebrantes**; reverter código não exige reverter migrações já aplicadas (manter compatibilidade).

---

## PRs Detalhados

### Week 0 — Setup & Fundamentos

#### **[CICLO-031] docs(arch): ADRs 001–004**

**Closes**: CICLO-031

**Summary**  
Estabelece decisões arquiteturais fundamentais por meio de 4 ADRs, registrando racional técnico e reduzindo ambiguidade futura.

**Key Changes & Rationale**  
- `ADR-001-SQLite-vs-Postgres.md`: SQLite por simplicidade, zero-dep e local-first. Plano de adapter Postgres futuro.  
- `ADR-002-HTMX-vs-React.md`: SSR + HTMX para reduzir complexidade e bundle. Web Components onde for necessário.  
- `ADR-003-HLC-vs-Lamport.md`: Uso de HLC para ordenação causal com proximidade temporal para debug/timeline.  
- `ADR-004-Federation-Conflict-Policy.md`: Política determinística de merge (`quorum-wins → state-priority → LWW(HLC)`), escalando conflitos não resolvidos para `Case`.

**How to Test & Verify**  
1) Revisar conteúdo dos ADRs. 2) Checar alinhamento com `CICLO_Blueprint_v1.3.md`.

**Rollback Plan**  
Documentação: reverter o commit.

**Labels**: `type:docs` `area:arch` `decision:adr`

---

#### **[CICLO-025] chore(devexp): PR-0.1 - Week 0 (setup & seeds)**

**Closes**: CICLO-025

**Summary**  
Bootstrap do projeto: workspace Rust, CI básica, `Justfile`, seeds e docs. Objetivo: experiência **Day 1** sem fricção.

**Key Changes & Rationale**  
- **Workspace & CI**: `Cargo.toml` (crates `core`, `orbit`, `cmd`), checks `fmt`, `clippy`, `test`.  
- **`Justfile`**: fluxos padronizados (`dev`, `reset`, `seed`, `test`).  
- **Seed Data**: `venda` e `incidente` para E2E imediato.  
- **Docs iniciais**: `README.md` e `TUTORIAL.md`.

**How to Test & Verify**  
`just dev`; `just reset && just seed`; `just test && just check`; `vv report daily` gera relatório com seeds.

**Rollback Plan**  
Setup inicial: re-clonar se necessário.

**Labels**: `type:chore` `area:devexp` `onboarding:ready`

---

### Week 1 — MVP Funcional

#### **[CICLO-001] feat(core): PR-1.1 - Esquema & Migrações (núcleo)**

**Closes**: CICLO-001

**Summary**  
Base física do ledger via migrações SQL, criando tabelas nucleares e índices críticos; ativa WAL e FK.

**Key Changes & Rationale**  
- `db/migrations/0001_init.sql`: `records`, `transitions`, `status`, `attestations`, `timers`, `cases`, `tasks`.  
- `db/migrations/0002_indexes.sql`: índices por padrão de consulta (p.ex. `(flow, entity_id)`, `timers(active, due_ms)`, `attestations(target_kind, target_id)`).  
- Pragma por conexão: `journal_mode=WAL` e `foreign_keys=ON`.

**How to Test & Verify**  
`just reset` → cria `ciclodb.sqlite`; inspecionar tabelas/índices; `PRAGMA foreign_key_list('transitions')`.

**Rollback Plan**  
Somente via nova migração corretiva (`000X_fix_schema.sql`). Em dev: `just reset`.

**Labels**: `type:feat` `area:db` `db:migration`

---

#### **[CICLO-002] feat(core): PR-1.2 - Ledger + HLC + Idempotência de Transição**

**Closes**: CICLO-002

**Summary**  
Coração do engine: transações ACID, HLC e idempotência forte para evitar duplicidades.

**Key Changes & Rationale**  
- `crates/core/vv-ledger`: `begin_tx/commit/rollback` (abstrai `rusqlite`).  
- `crates/core/vv-record/src/hlc.rs`: HLC simples para timestamp causally-consistent.  
- `crates/core/vv-flow/src/apply.rs`: `apply_transition` com idempotency key (MD5 de campos-chave) e atualização do `status` apenas em `Ok` (invariante I1).

**How to Test & Verify**  
`cargo test -p vv-flow -- --test idempotency_check`; repetir `vv flow move ...` duas vezes (segunda falha por idempotência); checar uma única `transition` no DB.

**Rollback Plan**  
`git revert <commit_hash>`.

**Labels**: `type:feat` `area:engine` `safety:idempotent`

---

#### **[CICLO-003] feat(api): PR-1.3 - API/CLI mínimas (transition/status/timeline)**

**Closes**: CICLO-003

**Summary**  
Expõe o engine via REST e CLI, tornando o sistema operável externamente.

**Key Changes & Rationale**  
- **API** (`crates/core/vv-api`):  
  - `POST /flows/:flow/entities/:id/transition`  
  - `GET /flows/:flow/entities/:id/status`  
  - `GET /flows/:flow/entities/:id/timeline`
- **CLI** (`cmd/vv`): `vv flow move/status/timeline` espelhando API (scripts/ops).

**How to Test & Verify**  
```bash
vv flow move venda 001 Criado --reason "test"
vv flow move venda 001 Pago   --reason "paid"
vv flow status  venda 001     # "Pago"
vv flow timeline venda 001    # duas transitions
# Equivalente via curl para o REST
````

**Rollback Plan**  
`git revert <commit_hash>`.

**Labels**: `type:feat` `area:api,cli` `auditable:timeline`

* * *

#### **\[CICLO-004\] feat(policy): PR-1.4 - Engine de Policies (YAML) + Hot-reload**

**Closes**: CICLO-004

**Summary**  
Motor declarativo de policies em YAML com hot-reload para evolutividade operacional sem rebuild.

**Key Changes & Rationale**

*   `crates/core/vv-policy`: `PolicyEngine` com `serde_yaml`, avaliando blocos `require/doubt_if/not_if`.
*   **Hot-Reload**: watcher em `policies/` com recarga atômica.
*   Policies iniciais: `policies/flows/venda.yml` e `incidente.yml`.

**How to Test & Verify**  
Rodar `just dev`; executar `venda.pagar` **Ok**; editar `venda.yml` para falhar (ex.: `body.total_eur > 99999`); observar “Policies reloaded”; repetir — agora deve retornar `Not`.

**Rollback Plan**  
`git revert <commit_hash>`; operacionalmente, reverter YAML e aguardar recarga.

**Labels**: `type:feat` `area:policy` `runtime:hot-reload`

* * *

#### **\[CICLO-005\] feat(policy): PR-1.5 - Linter, Simulator e Explain de Policies**

**Closes**: CICLO-005

**Summary**  
Tooling essencial contra “YAML hell”: **Linter** (schema), **Simulator** (payloads mock) e **Explain** (trace de decisão).

**Key Changes & Rationale**

*   CLI: `vv policy lint/test/explain` (schema `policy.json`).
*   `crates/core/vv-policy/explain.rs`: `evaluate_with_explanation` retorna trace detalhado.
*   API: `/transition?explain=1` com explicação no JSON.

**How to Test & Verify**  
Induzir erro sintático em `venda.yml` → `vv policy lint` falha; `vv policy test -p ... -d ...` com `Doubt`; usar `?explain=1` e inspecionar trace.

**Rollback Plan**  
`git revert <commit_hash>`.

**Labels**: `type:feat` `area:policy` `devtools:linter,simulator,explain`

* * *

#### **\[CICLO-006\] feat(core): PR-1.6 - Attestation & Quorum (witness)**

**Closes**: CICLO-006

**Summary**  
Implementa o princípio “two-eyes”: `Attestation` + checagem de `Quorum` para co-responsabilidade por janela temporal.

**Key Changes & Rationale**

*   `crates/core/vv-attest`: criação/armazenamento de `Attestation`.
*   `crates/core/vv-quorum`: verificação de quórum exigido por alvo.
*   **API**: `POST /attest` (submeter) e `GET /attest/quorum` (status).
*   **Policy**: suporte a bloco `quorum:` em YAML.

**How to Test & Verify**  
`venda.pagar` exige `witness`; `GET /attest/quorum?target=transition:<id>` → `pending`; `POST /attest` adiciona; repetir `GET` → `satisfied`.

**Rollback Plan**  
`git revert <commit_hash>`.

**Labels**: `type:feat` `area:core,policy` `control:two-eyes`

* * *

#### **\[CICLO-007\] feat(orbit): PR-1.7 - Timers (batch, backoff, DLQ) + Métricas**

**Closes**: CICLO-007

**Summary**  
Worker de timers da camada **Órbita** para SLAs e **Auto-Despacho**. Resiliência e observabilidade de nível produção.

**Key Changes & Rationale**

*   `crates/orbit/vv-timers`: worker (Tokio) que varre `timers` a cada segundo.
*   **Resiliência**:
    *   **Batching**: busca até **256** timers vencidos por ciclo.
    *   **Exponential Backoff**: re-agenda no erro para evitar “poison pill” global.
    *   **DLQ**: após N tentativas, marca “poisoned” e abre `Case` em `sre` (ver `RUNBOOK_DLQ_TIMERS.md`).
*   **Observabilidade**: métricas Prometheus: `timer_fired_total`, `timer_delay_seconds_p95`, `timer_retry_count`, `timer_poison_total`.
*   **Schema**: `db/migrations/0003_timers_retry.sql` adiciona `retries` e `last_error` aos `timers`.

**How to Test & Verify**  
Armar um timer curto (ex.: `await_witness` 10min em `venda.yml`); aguardar **AutoTransition** → `Despachado` e abertura de `Case`; conferir `/metrics` (incremento de `timer_fired_total`). Caso de falha proposital deve acumular **retries** e cair na **DLQ** (lane `sre`).

**Rollback Plan**  
`git revert <commit_hash>`; migração `0003` é aditiva e pode permanecer.

**Labels**: `type:feat` `area:orbit,timers` `resilience:backoff,dlq` `observability:prometheus`

* * *

Pós-Merge: Checklist Operacional
--------------------------------

*    **Migrações aplicadas** (`vv migrate` / `just reset` em dev).
*    **Variáveis de ambiente** revisadas (API, timers, Prometheus).
*    **Policies validadas** (`vv policy lint/test`) + hot-reload OK.
*    **Idempotência** verificada (teste duplo `vv flow move`).
*    **Quórum/Atestation**: happy-path e pending-path validados.
*    **Timers**: firing, backoff e DLQ cobertos com casos reais.
*    **Observabilidade**: métricas disponíveis e scrapadas.
*    **Runbooks**: `RUNBOOK_DLQ_TIMERS.md` publicado e acessível.
*    **Backups** (se aplicável): snapshot de `ciclodb.sqlite`.
*    **Rollbacks**: plano documentado em cada PR.

* * *

Anexos e Modelos
----------------

### Modelo — Cabeçalho GitHub PR

```
Title: [CICLO-XXX] <tipo>(<área>): PR-x.y - <resumo curto>

Closes CICLO-XXX

Summary
<2–4 linhas de impacto e motivação>

Key Changes & Rationale
- <ponto 1>
- <ponto 2>
- <ponto 3>

How to Test & Verify
1) <passo>
2) <passo>
3) <passo>

Rollback Plan
- <estratégia>

Labels
type:<feat|fix|chore|docs> area:<core|api|policy|orbit|db|devtools> db:<migration?> risk:<low|med|high> observability:<...>
```

### Modelo — Check de Observabilidade (Prometheus)

*    Endpoint `/metrics` exposto e autenticado (se aplicável).
*    Painel com `timer_fired_total`, `timer_delay_seconds_p95`, `timer_retry_count`, `timer_poison_total`.
*    Alertas: **DLQ > 0** (warning), **retry\_count p95↑** (warning), **delay p95↑** (critical).

* * *

**Autor**: Equipe CICLO  
**Revisão**: 1.0 — pacote até PR-1.7  
"""

with open("/mnt/data/ciclo\_pr\_docs\_PR1.0-PR1.7.md", "w", encoding="utf-8") as f:  
f.write(doc)

"/mnt/data/ciclo\_pr\_docs\_PR1.0-PR1.7.md"

Resultado

undefined

ficou redondo. te entrego um pacote pronto pra usar no GitHub (ou wiki interna):

**Arquivo completo (.md):**  
Download: CICLO — PR Docs até PR 1.7

o que fiz:

*   padronizei os PRs em formato profissional (Summary, Key Changes, How to Test, Rollback, Labels).
*   adicionei um quadro-resumo (changelog condensado) com migrações e impacto.
*   incluí checklists operacionais (pós-merge, observabilidade/Prometheus, runbooks).
*   deixei modelos prontos para cabeçalho de PR e para verificação de métricas.

se quiser, eu também gero versões individuais por PR (um `.md` por PR com o template aplicado).



---
Powered by [ChatGPT Exporter](https://www.chatgptexporter.com)