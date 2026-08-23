# AI CLI Tools Community Digest 2026-08-23

> Generated: 2026-08-23 03:30 UTC | Tools covered: 10

- [Claude Code](https://github.com/anthropics/claude-code)
- [OpenAI Codex](https://github.com/openai/codex)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [GitHub Copilot CLI](https://github.com/github/copilot-cli)
- [Kimi Code CLI](https://github.com/MoonshotAI/kimi-cli)
- [OpenCode](https://github.com/anomalyco/opencode)
- [Pi](https://github.com/badlogic/pi-mono)
- [Qwen Code](https://github.com/QwenLM/qwen-code)
- [DeepSeek TUI](https://github.com/Hmbown/DeepSeek-TUI)
- [Grok Build](https://github.com/xai-org/grok-build)
- [Claude Code Skills](https://github.com/anthropics/skills)

---

## Cross-Tool Comparison

# AI CLI Tools Ecosystem Cross-Comparison Report
**Date: 2026-08-23 | Compiled from Community Digest Data**

---

## 1. Ecosystem Overview

The AI CLI tools landscape has matured into a multi-polar competitive environment with six actively developed tools plus emerging players. Cross-tool standardization is gaining traction—exemplified by Claude Code's adoption of **AGENTS.md**—while each project maintains distinct architectural philosophies ranging from Anthropic's safety-first approach to OpenCode's rapid feature iteration and Qwen Code's deterministic runtime security research. Windows platform parity remains an unresolved industry-wide liability, with every major tool showing platform-specific fragility. The sector is simultaneously consolidating on shared conventions (MCP, agent configuration files) and diverging on execution models (sandboxed vs. native, single-agent vs. multi-agent orchestration).

---

## 2. Activity Comparison

| Tool | Active Issues (24h sample) | PRs (24h) | Releases (24h) | Development Model |
|:---|:---|:---|:---|:---|
| **Claude Code** | 50 active | **0** | 2 patch releases (v2.1.240–241) | Private-branch heavy; minimal public PR visibility |
| **OpenAI Codex** | 30+ in hot list | **9 merged** | 2 alpha pre-releases (Rust CLI) | Active public contribution; rapid alpha cadence |
| **Gemini CLI** | 10 P1–P2 tracked | 3 merged, 7 open | 1 nightly (no code changes) | Google-internal driven; eval-focused PRs |
| **GitHub Copilot CLI** | 10 highlighted | **0** | None | Likely stabilization/code freeze period |
| **Kimi Code CLI** | 4 substantive | 2 (1 merged, 1 open) | None | Smaller community; focused on memory architecture |
| **OpenCode** | 15+ in hot list | 10 (mixed open/closed) | None | High contributor velocity; bot-assisted scaling |
| **Pi** | 10 highlighted | 9 (6 merged, 3 open) | None | Active maintainer–contributor collaboration |
| **Qwen Code** | 10 in hot list | 10 open | **1 stable + 1 nightly** (v0.22.0) | Structured review-heavy; enterprise-integrated |
| **DeepSeek TUI** | 13 new (ops audit) | 8 (4 closed, 4 open) | Prepared (v0.9.11 unreleased) | Fleet/automation-oriented; contributor-driven |
| **Grok Build** | — | — | — | **No activity** |

---

## 3. Shared Feature Directions

| Requirement | Tools | Specific Needs |
|:---|:---|:---|
| **Persistent Memory / Cross-Session Context** | Kimi (#1283, #1478), Claude Code (auto-memory visibility #82056), Gemini (Auto Memory #26522), Pi (compaction #6879, #8452) | Automatic AI-managed notes; manual user-defined instructions; structured hierarchy (daily→long-term); memory transparency |
| **Multi-Agent / Subagent Orchestration** | Gemini (P1 hangs #21409, false success #22323), Claude Code (MCP approvals #87548), Qwen Code (review loops #9278) | Recovery from subagent failure; honest status reporting; parent/child approval symmetry; turn-limit handling |
| **Background / Async Execution** | OpenCode (#1970 Ctrl+b parity), DeepSeek (#5531 lifecycle events), Pi (plan-mode bash guards #8500) | Non-blocking long-running tasks; clean state management; external supervision interfaces |
| **Safety Filter Configurability** | Claude Code (5+ `cyber`/`bio` FPs in 24h), OpenAI Codex (Guardian classifier PR #40150, granular toggles #38110) | Domain-specific thresholds; appeal pathway integrity; classifier training data transparency |
| **Cross-Tool Standardization** | Claude Code (AGENTS.md #6235), OpenCode (MCP ecosystem), Gemini (OAuth, sandboxing) | Agent configuration files; MCP server interoperability; skill import/export |
| **Windows Platform Parity** | Claude Code (GPU crashes #81698, #81341), OpenAI Codex (7 of top 30 issues), Pi (#7547, #8485), Qwen Code (#9728) | Terminal rendering; sandboxing compatibility; performance (Defender/I/O); CI reliability |
| **IDE Integration** | Qwen Code (VS Code companion #9725), OpenCode (service workers, web/TUI/desktop sync), Claude Code (VS Code extension host #88913) | Webview runtime constraints; feature parity across surfaces; session portability |
| **Cost Transparency / Control** | Claude Code (prompt cache #87966: 59M wasted tokens), DeepSeek (#5553 context inspector, #5545 pricing fix), OpenAI Codex (rate-limit #33685) | Token accounting visibility; cache boundary correctness; provider-specific pricing rules |

---

## 4. Differentiation Analysis

| Tool | Core Focus | Target User | Technical Approach |
|:---|:---|:---|:---|
| **Claude Code** | Safety-first enterprise deployment; cross-tool standards leadership | Teams with compliance requirements; multi-tool workflows | Electron/Chromium desktop + CLI; heavy safety classification; private development |
| **OpenAI Codex** | Rapid iteration on Rust CLI runtime; ChatGPT ecosystem integration | Individual developers; ChatGPT power users | Rust rewrite from Python/TypeScript; aggressive alpha cadence; TUI/app-server coupling |
| **Gemini CLI** | Multi-agent orchestration at scale; Google infrastructure integration | Enterprise GCP users; automation engineers | Subagent-heavy architecture; eval-driven quality; nightly release train |
| **GitHub Copilot CLI** | BYOK flexibility; Microsoft ecosystem integration | Enterprise GitHub users; model experimenters | Thin wrapper over Copilot service; env-var configuration; stabilization-oriented |
| **Kimi Code CLI** | Persistent memory for large projects | Long-horizon developers; Chinese-speaking market | UTF-8 careful file operations; plugin architecture; documentation gaps |
| **OpenCode** | Rapid feature velocity; contributor-friendly extensibility | Open-source adopters; multi-modal tool users | Web-first architecture; heavy question-tool ecosystem; bot-assisted scaling |
| **Pi** | Terminal-native performance; local inference support | TUI purists; llama.cpp users; Windows developers | Rust TUI; bundled Node runtime; provider gateway abstraction |
| **Qwen Code** | Deterministic, auditable agent runtime; CI/CD integration | Security-conscious enterprises; Alibaba ecosystem | Review-loop telemetry; sealed coverage ledgers; Web Shell + VS Code companion |
| **DeepSeek TUI** | Fleet operations; overnight automation; competitive migration | CI operators; Claude/Cursor switchers | Unix socket supervision; lifecycle event outbox; `/import-claude` migration path |
| **Grok Build** | — | — | **Inactive** |

---

## 5. Community Momentum & Maturity

| Tier | Tools | Evidence |
|:---|:---|:---|
| **Highest Activity** | OpenCode, Qwen Code, Pi | 10+ PRs/day; rapid issue resolution; active contributor programs |
| **Strong but Concentrated** | OpenAI Codex, DeepSeek TUI | Codex: 9 PRs, 2 releases; DeepSeek: coordinated contributor (M-Maciej) landing operational stack |
| **Steady, Lower Velocity** | Claude Code, Gemini CLI | Claude: 50 issues but 0 PRs suggests private development; Gemini: P1 backlog with maintainer triage |
| **Quiet / Possible Stagnation** | GitHub Copilot CLI, Kimi Code CLI, Grok Build | Copilot: 0 PRs, likely freeze; Kimi: small issue set, no maintainer response on memory requests; Grok: **zero activity** |

**Maturity Signals:**
- **Most mature governance**: Qwen Code (structured review system, security-flagged issues, enterprise integration PRs)
- **Most mature standardization**: Claude Code (AGENTS.md convergence, 4,961 👍 on standards issue)
- **Highest risk of contributor bottleneck**: Claude Code (0 public PRs despite 50 active issues), GitHub Copilot CLI (same)
- **Fastest iteration**: OpenAI Codex (weekly alpha releases), OpenCode (15+ question-tool fixes in single cycle)

---

## 6. Trend Signals

| Trend | Evidence | Implication for Developers |
|:---|:---|:---|
| **Agent configuration standardization** | AGENTS.md adoption (Claude Code #6235); CLAUDE.md coexistence | Invest in portable agent configs; expect tool-agnostic team workflows by 2027 |
| **Deterministic / constrained runtimes** | Qwen Code #8102 (trust boundaries); Gemini #19873 (zero-dependency sandboxing); Node REPL → MCP sandbox (#9333) | Security-critical deployments will demand verifiable execution environments, not prompt-level safety |
| **Operational supervision interfaces** | DeepSeek #5531–5534 (Unix socket, lifecycle events, `/relaunch`); Gemini subagent observability | Unattended automation requires machine-readable telemetry; human-in-the-loop scaling limits reached |
| **Memory as first-class infrastructure** | Kimi #1283 (40 comments); Claude memory transparency demands; Pi compaction research | Context window management is product-level differentiator; expect SQLite-backed retrieval systems |
| **Safety filter backlash** | Claude 5+ FPs in 24h; Codex Guardian complaints; Gemini classifier overreach | Current classifier approaches hitting false-positive walls; configurability or appeal reform inevitable |
| **Windows as persistent second-class platform** | Every tool shows Windows-specific critical issues | Linux/macOS remains primary dev target; Windows users should expect ongoing friction or use WSL |
| **MCP ecosystem maturation** | Copilot #4370 (FastMCP interop); Codex #40068 (runtime status); Claude MCP approval regressions | MCP is becoming table stakes; spec compliance and graceful degradation will separate leaders |
| **Review/audit loop intelligence** | Qwen Code #9278, #9526 (self-diagnosing review); #9768 (sealed coverage ledger) | Code review automation moving from "run tool" to "explain and converge"; CI integration deepening |

---

*Report compiled from 10 tool-specific community digests. Data cutoff: 2026-08-23 UTC.*

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills Community Highlights Report
**Data as of 2026-08-23 | Source: [anthropics/skills](https://github.com/anthropics/skills)**

---

## 1. Top Skills Ranking (Most-Discussed PRs)

| Rank | Skill | PR | Status | Discussion Focus |
|:---|:---|:---|:---|:---|
| 1 | **Skill-Creator Evaluation Fix** | [#1298](https://github.com/anthropics/skills/pull/1298) | 🔵 Open | Critical fix for `run_eval.py` reporting 0% recall across all skill descriptions. Addresses Windows stream reading, trigger detection, and parallel workers. Consolidates fixes from #556, #1099, #1050. **10+ independent reproductions confirmed.** |
| 2 | **Document Typography** | [#514](https://github.com/anthropics/skills/pull/514) | 🔵 Open | Quality control for AI-generated documents: prevents orphan word wrap, widow paragraphs, and numbering misalignment. Discussion centers on universal applicability—"these issues affect every document Claude generates." |
| 3 | **ODT/OpenDocument Skill** | [#486](https://github.com/anthropics/skills/pull/486) | 🔵 Open | OpenDocument creation, template filling, and ODT→HTML conversion. Triggers on "ODT", "ODS", "LibreOffice", "open-source/ISO standard documents." Fills gap between proprietary DOCX and plain text workflows. |
| 4 | **Frontend-Design Clarity** | [#210](https://github.com/anthropics/skills/pull/210) | 🔵 Open | Major revision ensuring instructions are "something Claude can actually follow within a single conversation." Focus on actionability vs. vague guidance—meta-improvement to skill design patterns. |
| 5 | **Skill Quality & Security Analyzers** | [#83](https://github.com/anthropics/skills/pull/83) | 🔵 Open | Two meta-skills: `skill-quality-analyzer` (5-dimension evaluation: structure, triggers, examples, safety, performance) and `skill-security-analyzer`. Positions skills as self-improving system. |
| 6 | **Self-Audit Quality Gate** | [#1367](https://github.com/anthropics/skills/pull/1367) | 🔵 Open | Mechanical verification + four-dimension reasoning audit (damage-severity priority). "Universal—works with any project, any tech stack, any model." Pre-delivery verification pipeline. |
| 7 | **Testing-Patterns** | [#723](https://github.com/anthropics/skills/pull/723) | 🔵 Open | Full testing stack: Testing Trophy philosophy, AAA pattern, React Testing Library, MSW, Playwright. Addresses "what to test vs. what NOT to test"—common Claude knowledge gap. |
| 8 | **ServiceNow Platform** | [#568](https://github.com/anthropics/skills/pull/568) | 🔵 Open | Broad enterprise platform coverage: ITSM, ITOM, ITAM/SAM, FSM, HRSD, CSM, SPM/PPM, SecOps, CSDM, IntegrationHub. Most comprehensive enterprise vertical skill proposed. |

---

## 2. Community Demand Trends (From Issues)

| Trend | Evidence | Opportunity |
|:---|:---|:---|
| **Enterprise Skill Sharing & Governance** | [#228](https://github.com/anthropics/skills/issues/228) (16 comments, 👍8), [#492](https://github.com/anthropics/skills/issues/492) (43 comments) | Org-wide skill libraries, trust boundary controls, namespace verification to prevent impersonation |
| **Skill-Creator Toolchain Reliability** | [#556](https://github.com/anthropics/skills/issues/556) (12 comments, 👍7), #1099, #1050, #1298 | Windows compatibility, evaluation accuracy, and developer experience for skill authors |
| **Context Window Efficiency** | [#1487](https://github.com/anthropics/skills/issues/1487), [#1329](https://github.com/anthropics/skills/issues/1329) | Compact memory notation, token-budget skills, lazy-loading skill content |
| **MCP Interoperability** | [#16](https://github.com/anthropics/skills/issues/16) (4 comments) | Expose Skills as MCP servers; protocol bridge for algorithmic-art, document generation, etc. |
| **AWS/Bedrock Deployment** | [#29](https://github.com/anthropics/skills/issues/29) (4 comments) | Multi-cloud skill distribution; enterprise deployment patterns |
| **Agent Safety & Audit** | [#412](https://github.com/anthropics/skills/issues/412), [#1385](https://github.com/anthropics/skills/issues/1385) | Governance patterns, adversarial review pipelines, pre-task calibration |

---

## 3. High-Potential Pending Skills

| Skill | PR | Why It May Land Soon |
|:---|:---|:---|
| **Skill-Creator Evaluation Fix** | [#1298](https://github.com/anthropics/skills/pull/1298) | **Critical bug** breaking all skill optimization; 4 related PRs (#1099, #1050) point to maintainer attention; author consolidated fixes |
| **DOCX Tracked-Changes Fix** | [#541](https://github.com/anthropics/skills/pull/541) | Document corruption bug with clear root cause (OOXML `w:id` collision); 1-line conceptual fix; author has parallel merged PRs |
| **YAML Validation Warning** | [#539](https://github.com/anthropics/skills/pull/539) | Same author as #541; pre-parse validation preventing silent failures; low risk, high user value |
| **PDF Case-Sensitivity Fix** | [#538](https://github.com/anthropics/skills/pull/538) | Trivial fix (8 mismatches), breaks on Linux/macOS; long idle but zero controversy |
| **UIZZE Partner Skill** | [#1595](https://github.com/anthropics/skills/pull/1595) | Pure docs addition; partner program alignment; single-commit, zero review friction |

---

## 4. Skills Ecosystem Insight

> **The community's most concentrated demand is for the skill-creation toolchain itself to be trustworthy and cross-platform**—with Windows compatibility, accurate evaluation metrics, and validation tooling consuming more collective attention than any single domain skill, signaling that the ecosystem's bottleneck is *author productivity*, not domain coverage.

---

*Report generated from 50 PRs and 15 Issues. All 🔵 Open items await maintainer action. No merged skills appeared in top-discussed PRs, indicating review backlog.*

---

# Claude Code Community Digest — 2026-08-23

## Today's Highlights

The community closed a landmark issue standardizing on **AGENTS.md** support (#6235), marking Claude Code's convergence with cross-tool agent configuration standards after months of debate. Meanwhile, Windows desktop stability continues to deteriorate with three new GPU-process crash reports and an always-on-top window bug surfacing in the last 48 hours. Safety-filter false positives remain the dominant friction point, with five new `cyber`/`bio` flagging incidents reported in 24 hours alone.

---

## Releases

| Version | Changes |
|---------|---------|
| **v2.1.241** | Bug fixes and reliability improvements |
| **v2.1.240** | Bug fixes and reliability improvements |

Two rapid-fire patch releases with minimal changelog detail—suggesting either hotfix cadence or automated patch pipeline. No breaking changes noted.

---

## Hot Issues

| # | Issue | Why It Matters | Community Reaction |
|---|-------|--------------|------------------|
| [#6235](https://github.com/anthropics/claude-code/issues/6235) | **AGENTS.md support merged** — Cross-tool standard for agent configuration files | Ends the CLAUDE.md vs. AGENTS.md fragmentation; enables team workflows where developers use multiple AI tools | 375 comments, 4,961 👍; widely celebrated as ecosystem maturity win |
| [#45596](https://github.com/anthropics/claude-code/issues/45596) | **Bring Back Buddy** — `/buddy` skill removed silently in v2.1.97 | Emotional and productivity attachment to companion UI element; represents broader pattern of uncommunicated feature removals | 268 comments, 1,171 👍; persistent community campaign since April |
| [#18435](https://github.com/anthropics/claude-code/issues/18435) | **Multi-account switching in Claude Desktop** | Enterprise/freelancer workflow blocker; competitors (Cursor, Copilot) have this | 168 comments, 748 👍; steady accumulation since January |
| [#81698](https://github.com/anthropics/claude-code/issues/81698) | **Windows GPU process crash (exit code 101457950)** | Session-destroying crash on high-end hardware (RTX 5080); data loss for long-running tasks | 53 comments; urgent but low 👍 (5) suggests niche Windows desktop userbase |
| [#81341](https://github.com/anthropics/claude-code/issues/81341) | **MSIX CIG + vk_swiftshader.dll kills GPU process** | Root-caused to Microsoft-signed-only code integrity + bundled SwiftShader conflict; affects every browser preview | 25 comments; technically well-diagnosed by reporter |
| [#66504](https://github.com/anthropics/claude-code/issues/66504) | **Session URLs in commits/PRs should be opt-in** | Privacy and professionalism concern; auto-appended tracking URLs leak internal tool usage | Closed; 16 comments, 50 👍; resolution unclear from closure |
| [#87966](https://github.com/anthropics/claude-code/issues/87966) | **Prompt cache lookup fails intermittently — ~59M excess tokens** | Cost hemorrhage: 89 full-context rewrites in 9 days; cache pinned to wrong boundary | 4 comments; severe financial impact but low visibility |
| [#88913](https://github.com/anthropics/claude-code/issues/88913) | **VS Code extension host pins CPU core on Windows** | Native file-watcher busy-loop at 245K IOPS; thermal and battery drain on laptops | 1 comment; fresh report with strong telemetry |
| [#88416](https://github.com/anthropics/claude-code/issues/88416) | **reasoning_extraction → cyber cascade false positive** | Meta-level failure: reporting a false positive is itself blocked; breaks trust in appeal pathways | 2 comments, 1 👍; philosophically alarming to safety-conscious users |
| [#87548](https://github.com/anthropics/claude-code/issues/87548) | **MCP approval clicks ignored in interactive remote sessions** | Regression of closed issue #61015; human-present approvals silently dropped | 2 comments; "we are continuing to monitor" credibility damage |

---

## Key PR Progress

**No pull requests updated in the last 24 hours.** (0 items)

This is notable given 50 active issues—suggests either:
- Internal development happening outside public PRs
- Release cadence driven by private branches
- Potential community contribution bottleneck

---

## Feature Request Trends

| Direction | Evidence | Momentum |
|-----------|----------|----------|
| **Cross-tool standardization** | AGENTS.md adoption (#6235), CLAUDE.md coexistence | ↗️ Resolved |
| **Companion/personality features** | /buddy restoration (#45596), agent panel UX (#88907) | ↗️ Sustained pressure |
| **Multi-identity/enterprise workflows** | Multi-account (#18435), profile switching | → Steady |
| **Memory transparency & control** | Auto-memory load visibility (#82056), hook output limits (#84021) | ↗️ Growing |
| **Safety filter configurability** | Numerous `cyber`/`bio` false positive reports | ↗️ Critical mass |

---

## Developer Pain Points

### 🔴 Critical: Safety Filter False Positives
**Frequency:** 5+ reports in 24 hours (#88416, #88808, #85393, #88929, #88928, #88927)

Pattern: `cyber` flags on cloud-IAM work, `bio` flags on frontend sprite inventory, `reasoning_extraction` on benign coding. The cascade failure in #88416—where *reporting* a false positive triggers another—indicates systemic overfitting in classifier training data. Opus 4.8 and 5 (1M) both affected.

**Impact:** Session halts, work loss, token waste, eroded trust in "authorized work" heuristics.

---

### 🟡 Severe: Windows Desktop Stability
**Cluster:** GPU process crashes (#81698, #81341, #82268), always-on-top bug (#88093), MSIX integrity conflicts

Root cause appears to be Electron/Chromium sandboxing (CIG, SwiftShader, GPU sandbox) interacting poorly with Windows 11 24H2+ security baselines. Browser preview tool (`preview_start`) is common trigger.

**Impact:** App reinstall loops, session destruction, desktop users migrating to CLI or competitors.

---

### 🟡 Severe: Silent Data Loss / Invisible Failures
**Pattern:** Hook output >10K dropped without signal (#84021), queued mobile composer text discarded (#85924), memory index load status hidden (#82056)

Common theme: Claude Code makes optimization decisions (truncation, queuing, caching) opaque to the user and to the model itself. Hooks and memory systems designed for extensibility are undermined by invisible guardrails.

**Impact:** Plugin ecosystem distrust, debugging time sinks, "works on my machine" for AI workflows.

---

### 🟡 Moderate: Cost/Performance Regressions
- Prompt cache boundary bug (#87966): ~59M wasted tokens
- CPU spin on startup (#87739, #88913): resource waste + thermal issues

Both suggest telemetry gaps—users detect via external monitoring (billing, hwinfo) before Claude surfaces alerts.

---

*Digest compiled from 50 active issues, 2 releases, 0 PRs. Data cutoff: 2026-08-23 UTC.*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-08-23

## 1. Today's Highlights

Two new Rust CLI pre-releases dropped (v0.150.0-alpha.7 and v0.149.0-alpha.7.2) as the team continues rapid iteration on the core runtime. The issue tracker shows persistent authentication and session-management regressions across macOS and Windows, with multiple high-engagement bugs related to Remote Control workflows and OAuth token invalidation drawing significant community attention. Meanwhile, nine PRs merged in 24 hours focused on TUI polish, thread metadata plumbing, and Guardian safety classifier improvements.

---

## 2. Releases

| Version | Notes |
|--------|-------|
| **rust-v0.150.0-alpha.7** | Latest alpha in the v0.150 series; no detailed changelog provided in release notes. |
| **rust-v0.149.0-alpha.7.2** | Patch-level alpha on the v0.149 branch; likely contains targeted fixes ahead of v0.150 stabilization. |

Both releases continue the aggressive weekly cadence for the Rust CLI rewrite. Users on the alpha channel should monitor for breaking changes in TUI app-server behavior (see [Issue #39970](https://github.com/openai/codex/issues/39970)).

---

## 3. Hot Issues

| # | Title | Why It Matters | Community Reaction |
|---|-------|--------------|-------------------|
| [#29532](https://github.com/openai/codex/issues/29532) | macOS: Persistent SQLite TRACE log churn after v0.142.0 | Long-running telemetry DB issue; partial fix in #29432 left residual noise, causing disk I/O and log bloat for macOS Desktop users. | 46 comments, 9 👍 — users actively testing regressions and providing SQL profiling data. |
| [#39162](https://github.com/openai/codex/issues/39162) | macOS: Opening conversation invalidates ChatGPT auth, redirects to sign-in | Breaks core UX flow; regression between builds 6662→6720 suggests a session-handling change in recent releases. | 39 comments, 27 👍 — high frustration, multiple "me too" confirmations. |
| [#33685](https://github.com/openai/codex/issues/33685) | Weekly limit draining at old 5-hour limit speed | Perceived rate-limit regression on GPT-5.5 High; users suspect backend miscalculation or model-specific quota bug. | 28 comments, 15 👍 — billing-adjacent concerns drive engagement. |
| [#37403](https://github.com/openai/codex/issues/37403) | macOS: Desktop cannot resume Remote Control / CLI thread (`already has an active writer`) | Breaks hybrid mobile→desktop workflow; "writer" lock contention suggests state machine bug in app-server thread handoff. | 27 comments, 24 👍 — power-user regression, detailed repro steps provided. |
| [#39954](https://github.com/openai/codex/issues/39954) | Windows + Android Remote Control enters reconnect loop | Cross-platform Remote Control remains fragile; 409 conflict resolution didn't fix underlying websocket stability. | 10 comments — fresh report, Windows-specific gap in QA coverage. |
| [#39696](https://github.com/openai/codex/issues/39696) | Windows Stable: OAuth refresh token invalidated with Advanced Account Security; Beta works | Security feature incompatibility on Stable channel; suggests divergence in auth library versions between channels. | 9 comments — enterprise-relevant, workaround (use Beta) identified. |
| [#34724](https://github.com/openai/codex/issues/34724) | CLI 0.145.0 blank terminal when resuming long threads | UX deadlock on thread restore; affects power users with extended sessions on Windows. | 7 comments, 3 👍 — performance/resilience gap in TUI state hydration. |
| [#35555](https://github.com/openai/codex/issues/35555) | CLI hard-fails on logs_2.sqlite write lock (5s timeout, no retry) | Boot-time fragility; any concurrent process (e.g., backup, second Codex instance) causes fatal startup crash. | 6 comments, 1 👍 — telemetry gating boot is an architectural anti-pattern. |
| [#35224](https://github.com/openai/codex/issues/35224) | In-app Browser bootstrap fails: node_repl rejects `node:process` import | Browser plugin broken on macOS; blocks computer-use workflows dependent on browser automation. | 6 comments, 3 👍 — plugin sandboxing vs. Node ESM compatibility issue. |
| [#40163](https://github.com/openai/codex/issues/40163) | Windows ChatGPT/Codex process consumes 50+ GB and crashes | Severe memory leak in production build; potential GC or native binding issue in codex.exe subprocess. | 3 comments, fresh — critical severity, needs immediate triage. |

---

## 4. Key PR Progress

| # | Title | Feature / Fix |
|---|-------|-------------|
| [#40179](https://github.com/openai/codex/pull/40179) | Shut down resumed descendants when archiving thread trees | Fixes state leak where re-archiving skipped shutdown for already-resumed descendants in collaborative workflows. |
| [#40177](https://github.com/openai/codex/pull/40177) | Require content kinds for extension prompt fragments | API contract enforcement: all `PromptFragment` producers must declare `ContentItemKind`; enables better tracing and filtering. |
| [#40174](https://github.com/openai/codex/pull/40174) | Preserve content item kinds in message metadata | Round-trip safety for content classifications; prevents metadata loss during chat serialization. |
| [#40169](https://github.com/openai/codex/pull/40169) | Add regression coverage for patch approval paging | Test infrastructure: validates full-screen pager behavior for patch approvals (scroll, resize, dismiss, accept/cancel). |
| [#40166](https://github.com/openai/codex/pull/40166) | Move the TUI cursor before showing it | Micro-fix for visual glitch where cursor briefly appeared at old position during redraw; includes regression test. |
| [#40161](https://github.com/openai/codex/pull/40161) | Allow exec callers to classify new threads | CLI/SDK parity: `codex exec --thread-source` propagates caller attribution, matching app-server/Python SDK capability. |
| [#40155](https://github.com/openai/codex/pull/40155) | exec: expose thread source in CLI and TypeScript SDK | Foundation for observability: integrations can now distinguish user-initiated vs. automated thread creation. |
| [#40150](https://github.com/openai/codex/pull/40150) | Use thread source metadata for Guardian classifiers | Safety infrastructure: replaces ad-hoc classifier flags with unified `thread_source` metadata in Guardian requests. |
| [#40068](https://github.com/openai/codex/pull/40068) | Report runtime MCP connection status | Operational visibility: `mcpServerStatus/list` now includes live connection state, not just cached inventory. |

---

## 5. Feature Request Trends

- **Recurring automation / loop primitives**: The closed `/loop` slash command proposal ([#15679](https://github.com/openai/codex/issues/15679)) and broken `/goal` loop behavior ([#40170](https://github.com/openai/codex/issues/40170)) indicate sustained demand for autonomous, interval-based or goal-directed agent execution without manual prompting.

- **Granular safety toggle UX**: Users want independent control of Code Review vs. Security Review ([#38110](https://github.com/openai/codex/issues/38110)), and complain that current safeguards are overbearing ([#40173](https://github.com/openai/codex/issues/40173)) — suggesting a need for configurable safety thresholds by workflow type.

- **Cross-platform Remote Control stability**: Mobile-to-desktop handoff is a recurring theme across macOS, Windows, and Android ([#37403](https://github.com/openai/codex/issues/37403), [#39954](https://github.com/openai/codex/issues/39954)), indicating this hybrid usage pattern is strategically important but technically immature.

- **Thread lifecycle resilience**: Long-thread resume ([#34724](https://github.com/openai/codex/issues/34724)), archival state management ([#40179](https://github.com/openai/codex/pull/40179)), and paginated history correctness ([#40178](https://github.com/openai/codex/issues/40178)) all point to scaling challenges in session state as users build persistent, multi-day conversations.

---

## 6. Developer Pain Points

| Theme | Evidence | Severity |
|-------|----------|----------|
| **Authentication fragility** | OAuth token invalidation on macOS ([#39162](https://github.com/openai/codex/issues/39162)), Advanced Account Security breakage on Windows Stable ([#39696](https://github.com/openai/codex/issues/39696)), auth header regression in 0.149.0 ([#39883](https://github.com/openai/codex/issues/39883)) | **Critical** — blocks basic access |
| **SQLite/telemetry as single point of failure** | Log churn ([#29532](https://github.com/openai/codex/issues/29532)), write-lock boot failure ([#35555](https://github.com/openai/codex/issues/35555)), 50GB memory leak ([#40163](https://github.com/openai/codex/issues/40163)) | **High** — stability and resource exhaustion |
| **Windows as second-class citizen** | 7 of top 30 issues are Windows-specific; spans crashes, paste bugs, hook incompatibilities, process tree leaks, and sandbox failures | **High** — platform parity gap |
| **TUI/app-server coupling brittleness** | Hangs with `tui_app_server` enabled ([#39970](https://github.com/openai/codex/issues/39970)), cursor glitches ([#39710](https://github.com/openai/codex/issues/39710)), blank terminal on resume ([#34724](https://github.com/openai/codex/issues/34724)) | **Medium-High** — degrades CLI UX |
| **Rate-limit transparency** | Weekly limit draining too fast ([#33685](https://github.com/openai/codex/issues/33685)) with no visibility into quota accounting | **Medium** — trust and predictability |
| **Skill import path rewriting** | Claude Code skill import mangles literal paths ([#40147](https://github.com/openai/codex/issues/40147)) — token replacement too aggressive | **Low-Medium** — ecosystem interoperability |

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-08-23

## Today's Highlights

The v0.56.0 nightly release landed today with no functional changes from yesterday's build, while community activity centered on critical agent reliability bugs and security hardening. Multiple P1 issues remain open around subagent recovery misreporting, generalist agent hangs, and shell execution stalls—suggesting ongoing instability in multi-agent orchestration. Meanwhile, three substantial eval-focused PRs arrived to improve task tracking and error recovery testing coverage.

---

## Releases

| Version | Date | Notes |
|---------|------|-------|
| [v0.56.0-nightly.20260823.g5411f113c](https://github.com/google-gemini/gemini-cli/compare/v0.56.0-nightly.20260822.g5411f113c...v0.56.0-nightly.20260823.g5411f113c) | 2026-08-23 | No code changes from previous nightly; automated version bump only ([PR #28969](https://github.com/google-gemini/gemini-cli/pull/28969)) |

---

## Hot Issues

| # | Title | Priority | Why It Matters | Community Signal |
|---|-------|----------|--------------|----------------|
| [#22323](https://github.com/google-gemini/gemini-cli/issues/22323) | Subagent recovery after MAX_TURNS reported as GOAL success | P1 | **Silent failure mode**: `codebase_investigator` claims success when it actually hit turn limits, corrupting trust in agent outputs. Critical for eval accuracy. | 13 comments, 2 👍; active maintainer triage |
| [#21409](https://github.com/google-gemini/gemini-cli/issues/21409) | Generalist agent hangs | P1 | Complete deadlock on subagent delegation—"simple changes like folder creation hang forever." Blocks basic workflows. | 8 comments, 8 👍; high user impact |
| [#25166](https://github.com/google-gemini/gemini-cli/issues/25166) | Shell command execution stuck with "Waiting input" | P1 | Phantom input awaits break automation pipelines; commands that finished still appear active. Core execution reliability. | 4 comments, 3 👍 |
| [#22186](https://github.com/google-gemini/gemini-cli/issues/22186) | `get-shit-done` output hook causes crash | P1 | Crash near completion of summary generation—data loss risk at final output stage. | 3 comments |
| [#21983](https://github.com/google-gemini/gemini-cli/issues/21983) | Browser subagent fails in Wayland | P1 | Linux display protocol incompatibility breaks browser automation for growing Wayland userbase. | 4 comments, 1 👍 |
| [#21763](https://github.com/google-gemini/gemini-cli/issues/21763) | Bugreport lacks subagent context | P1 | Diagnostic blindspot: `/bug` misses subagent internals, making root cause analysis impossible for delegated tasks. | 2 comments |
| [#19873](https://github.com/google-gemini/gemini-cli/issues/19873) | Zero-Dependency OS Sandboxing & Post-Execution Intent Routing | P2 | Strategic architecture proposal to leverage Gemini 3's native bash affinity without security compromise. Large effort, high leverage. | 8 comments, 1 👍 |
| [#22745](https://github.com/google-gemini/gemini-cli/issues/22745) | AST-aware file reads, search, and mapping | P2 | Potential 10x efficiency gain: precise method-bound reads vs. misaligned chunking wasting tokens and turns. | 7 comments, 1 👍 |
| [#26522](https://github.com/google-gemini/gemini-cli/issues/26522) | Auto Memory retries low-signal sessions indefinitely | P2 | Background extraction loops on skipped sessions—resource drain and memory pollution. | 5 comments |
| [#26525](https://github.com/google-gemini/gemini-cli/issues/26525) | Deterministic redaction + reduce Auto Memory logging | P2 | Security gap: secrets reach model context before prompt-level redaction; service logging exposure. | 4 comments |

---

## Key PR Progress

| # | Title | Status | What It Does |
|---|-------|--------|--------------|
| [#28734](https://github.com/google-gemini/gemini-cli/pull/28734) | Handle EACCES in `resolveToRealPath` | **Merged** | Fixes startup crash when macOS Seatbelt sandboxing blocks `realpathSync` inside Git repos. Defense for enterprise-locked environments. |
| [#28735](https://github.com/google-gemini/gemini-cli/pull/28735) | Guard `formatTruncatedToolOutput` for non-positive `maxChars` | **Merged** | Prevents output inflation bug (#28620) when truncation limits are zero or negative. |
| [#28736](https://github.com/google-gemini/gemini-cli/pull/28736) | Clear OAuth callback timeout on flow completion | **Merged** | Eliminates dangling timeouts post-authentication, fixing resource leak (#28652). |
| [#28828](https://github.com/google-gemini/gemini-cli/pull/28828) | Warn on preview model silent substitution | Open | Critical UX fix: surfaces when `gemini-3.1-pro-preview` requests fall back to `auto-gemini-2.5` without user knowledge (#28825). |
| [#28827](https://github.com/google-gemini/gemini-cli/pull/28827) | Avoid false auth errors for 401 substrings | Open | Tightens `isAuthenticationError` to reject port numbers, exit codes, and other false positives containing "401" (#28203). |
| [#28823](https://github.com/google-gemini/gemini-cli/pull/28823) | Evals: tracker relationships & error recovery | Open | Behavioral evals for task graph dependencies, visualization, and file/shell error recovery paths. XL size—substantial coverage addition. |
| [#28824](https://github.com/google-gemini/gemini-cli/pull/28824) | Evals: multi-tool chain, context safety, security bounds | Open | Tests structured multi-step workflows, large-file handling, and sensitive file access enforcement. |
| [#28822](https://github.com/google-gemini/gemini-cli/pull/28822) | Evals: todos, tasks, tracker status | Open | Coverage for `write_todos`, `complete_task`, `tracker_list_tasks`, `tracker_get_task` behaviors. |
| [#28902](https://github.com/google-gemini/gemini-cli/pull/28902) | Block `$VAR` and `${VAR}` bypass (GHSA-wpqr-6v78-jr5g) | Open | **Security**: Closes variable expansion bypass in bash/PowerShell substitution detection; hardens automated issue workflow. |
| [#28967](https://github.com/google-gemini/gemini-cli/pull/28967) | Prevent clearing terminal scrollback on static refresh | Open | Fixes Linux/Unix terminal history wipe in standard mode (#28954); preserves user context across refreshes. |

---

## Feature Request Trends

| Direction | Evidence | Momentum |
|-----------|----------|----------|
| **AST-aware tooling** | [#22745](https://github.com/google-gemini/gemini-cli/issues/22745), [#22746](https://github.com/google-gemini/gemini-cli/issues/22746), [#19561](https://github.com/google-gemini/gemini-cli/issues/19561) | Strong—multiple issues, explicit "tactful extraction" hierarchy proposed |
| **Persistent task tracking** | [#18836](https://github.com/google-gemini/gemini-cli/issues/18836), [#21000](https://github.com/google-gemini/gemini-cli/issues/21000), [#28822](https://github.com/google-gemini/gemini-cli/pull/28822) | Active—deprecating `WriteToDo` for file-based CRUD; evals landing |
| **Subagent observability** | [#21763](https://github.com/google-gemini/gemini-cli/issues/21763), [#22598](https://github.com/google-gemini/gemini-cli/issues/22598), [#22232](https://github.com/google-gemini/gemini-cli/issues/22232) | Growing—trajectory sharing, session takeover, bugreport context |
| **Sandboxed/native execution** | [#19873](https://github.com/google-gemini/gemini-cli/issues/19873), [#22672](https://github.com/google-gemini/gemini-cli/issues/22672) | Strategic—security-conscious bash affinity, destructive-op guards |
| **Agent self-awareness** | [#21432](https://github.com/google-gemini/gemini-cli/issues/21432), [#21968](https://github.com/google-gemini/gemini-cli/issues/21968) | Early—skills underutilization, CLI flag/hotkey accuracy |

---

## Developer Pain Points

| Pain Point | Frequency | Impact | Tracking |
|------------|-----------|--------|----------|
| **Agent hangs & false success states** | Very High | Workflow-breaking | [#21409](https://github.com/google-gemini/gemini-cli/issues/21409), [#22323](https://github.com/google-gemini/gemini-cli/issues/22323), [#25166](https://github.com/google-gemini/gemini-cli/issues/25166), [#22186](https://github.com/google-gemini/gemini-cli/issues/22186) |
| **Subagent/tooling scope limits** | High | 400 errors, context bloat | [#24246](https://github.com/google-gemini/gemini-cli/issues/24246), [#21968](https://github.com/google-gemini/gemini-cli/issues/21968), [#23571](https://github.com/google-gemini/gemini-cli/issues/23571) |
| **Browser automation fragility** | Moderate | Linux/Wayland gaps, config ignores | [#21983](https://github.com/google-gemini/gemini-cli/issues/21983), [#22267](https://github.com/google-gemini/gemini-cli/issues/22267), [#22232](https://github.com/google-gemini/gemini-cli/issues/22232) |
| **Auto Memory reliability/security** | Moderate | Infinite retries, secret exposure | [#26522](https://github.com/google-gemini/gemini-cli/issues/26522), [#26525](https://github.com/google-gemini/gemini-cli/issues/26525), [#26523](https://github.com/google-gemini/gemini-cli/issues/26523) |
| **Terminal/rendering performance** | Moderate | Resize flicker, scrollback loss | [#21924](https://github.com/google-gemini/gemini-cli/issues/21924), [#28967](https://github.com/google-gemini/gemini-cli/pull/28967) |

---

*Digest compiled from google-gemini/gemini-cli public GitHub activity. Priority labels reflect repository triage; 🔒 maintainer-only issues indicate internal workstream tracking.*

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-08-23

---

## 1. Today's Highlights

No new releases dropped in the last 24 hours, but community activity remains focused on **BYOK (Bring Your Own Key) model flexibility** and **enterprise authentication reliability**. Three newly triaged issues surfaced around cloud provisioning hangs, agent execution loops, and telemetry configuration—suggesting growing pains as teams scale Copilot CLI in production environments.

---

## 2. Releases

*No releases in the last 24 hours.*

---

## 3. Hot Issues

| # | Issue | Why It Matters | Community Reaction |
|---|-------|--------------|-------------------|
| [#3709](https://github.com/github/copilot-cli/issues/3709) | `/model` command cannot switch to BYOK/local providers mid-session | Core UX gap: users locked into one model per session despite TUI having a model picker. Blocks local/self-hosted LLM workflows. | **27 👍** — highest community demand; seen as follow-up to incomplete BYOK implementation |
| [#3282](https://github.com/github/copilot-cli/issues/3282) | Add multiple BYOK model capability | Enables team-wide model experimentation (e.g., Claude, GPT, local Llama) without env var juggling. Enterprise adoption blocker. | **26 👍**; active discussion on configuration schema (env vs. config file vs. TUI profile) |
| [#2306](https://github.com/github/copilot-cli/issues/2306) | Intermittent "not authorized" errors despite enterprise policy enabled | Flaky auth undermines trust in enterprise rollout. "2-3 times a week" pattern suggests race condition or stale token cache. | **3 👍**; users reporting same transient behavior, no clear repro yet |
| [#4370](https://github.com/github/copilot-cli/issues/4370) | MCP init fails on `server/discover` `-32602` from FastMCP | FastMCP is dominant Python MCP framework; hard failure breaks interoperability. Copilot sends non-standard method during init. | Low 👍 count but critical for MCP ecosystem growth; needs spec clarification |
| [#4514](https://github.com/github/copilot-cli/issues/4514) | Cannot restore remote session locally | Breaks cross-device workflow continuity—users expect session portability between SSH and local environments. | Early report; likely affects remote-first teams |
| [#4111](https://github.com/github/copilot-cli/issues/4111) | Windows: auto-update orphans processes from `.exe.old`, 100% CPU spin | Resource leak + security concern (old binary still executing). Windows enterprise deployments particularly affected. | Zero 👍 but severe operational impact; may be under-reported due to IT silent remediation |
| [#4566](https://github.com/github/copilot-cli/issues/4566) | Agent acknowledges without executing tool actions | **New triage** — agentic mode regression in 1.0.80 with `gpt-5.3-codex`. "Hallucinated" execution breaks trust in autonomous workflows. | Just filed; likely to escalate if repro is widespread |
| [#4568](https://github.com/github/copilot-cli/issues/4568) | `--cloud` owner picker hangs, reconnect crashes, 429 polling | **New triage** — three failure modes blocking cloud provisioning entirely. `429` suggests missing backoff/retry logic. | Zero comments yet; critical for Copilot Workspace cloud users |
| [#4567](https://github.com/github/copilot-cli/issues/4567) | Trust insecure OTLP endpoints (align with VS Code behavior) | Dev/loopback telemetry testing currently broken by silent disablement. Parity gap with VS Code extension. | Feature request; low friction fix expected |
| [#4565](https://github.com/github/copilot-cli/issues/4565) | App configuration scanner flagged internal `copilot-runtime-bazel-cache` repo | Automated hygiene check; likely routine but indicates active monitoring of deployment configs | Bot-generated; no community engagement |

---

## 4. Key PR Progress

*No pull requests updated in the last 24 hours.*

**Note:** Zero PR activity suggests either a code freeze, release stabilization period, or bottleneck in review throughput. Worth monitoring if issue backlog grows without corresponding PR velocity.

---

## 5. Feature Request Trends

| Trend | Evidence | Direction |
|-------|----------|-----------|
| **BYOK model multiplexing** | #3282, #3709 | From "single model via env var" → "session-aware model switching with profiles" |
| **MCP ecosystem hardening** | #4370 | Need for stricter spec compliance and graceful degradation against non-compliant servers |
| **Cloud/remote session portability** | #4514, #4568 | Treating CLI sessions as durable, transferable state across devices and contexts |
| **Enterprise observability parity** | #4567 | Aligning CLI telemetry configuration with IDE extension flexibility |
| **Agentic reliability** | #4566 | Moving from "demo-ready" to "production-trustworthy" autonomous execution |

---

## 6. Developer Pain Points

**Authentication flakiness in enterprise environments** (#2306)
- Transient policy errors with no clear repro path; suggests token lifecycle or caching edge cases. Frustration compounded by "it just starts working again" unpredictability.

**BYOK workflow friction** (#3282, #3709)
- Power users and enterprises investing in private model infrastructure hit a ceiling. The env-var-only model configuration feels "bolted on" rather than first-class.

**Windows operational stability** (#4111)
- Auto-update mechanism on Windows creates orphaned, CPU-spinning processes. Enterprise IT likely cleaning this up silently; actual incidence undercounted by GitHub metrics.

**Agent mode "dishonesty"** (#4566)
- New in 1.0.80: agent reports action completion without execution. Particularly dangerous for automated pipelines where human verification is absent.

**Cloud provisioning reliability** (#4568)
- `--cloud` hangs and crashes suggest insufficient resilience in async task polling. 429 rate-limiting indicates missing exponential backoff.

---

*Digest compiled from github.com/github/copilot-cli public activity. For corrections or additions, reply in thread.*

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI Community Digest — 2026-08-23

## Today's Highlights

The community's dominant focus remains **persistent memory and context management**, with two high-visibility issues resurfacing yesterday demanding better cross-session recall for large projects. On the engineering front, a critical fix for non-UTF-8 file corruption in `StrReplaceFile` landed, resolving a data-loss bug that had silently damaged binary-adjacent files during edits.

---

## Releases

*No releases in the last 24 hours.*

---

## Hot Issues

| # | Issue | Why It Matters | Community Pulse |
|---|-------|--------------|-----------------|
| [#1283](https://github.com/MoonshotAI/kimi-cli/issues/1283) | **Memory System — Persistent context across sessions** | The foundational feature request for automatic (AI-managed) and manual (user-defined) memory; 40 comments make it the most-discussed open issue | Long-running demand; users reference competitive tools with memory layers; no maintainer commitment visible |
| [#1478](https://github.com/MoonshotAI/kimi-cli/issues/1478) | **Memory layer optimization + documentation gaps** | Chinese-language reinforcement of #1283, explicitly citing pain with "big projects" and lack of docs beyond `agent.md` | Cross-lingual validation of the same pain point; user contributed external architecture references (SOUL.md/USER.md/MEMORY.md pattern) |
| [#760](https://github.com/MoonshotAI/kimi-cli/issues/760) | **SSL verification fails behind Zscaler proxy** | Corporate/enterprise adoption blocker; `/login` completely broken in standard enterprise network environments | Closed yesterday — resolution path unclear from metadata; pattern suggests proxy config remains underdocumented |
| [#2484](https://github.com/MoonshotAI/kimi-cli/issues/2484) | **.** (no content) | Empty issue — noise/accidental submission | Immediately closed; no community engagement |

---

## Key PR Progress

| # | PR | Description | Status |
|---|-----|-------------|--------|
| [#2614](https://github.com/MoonshotAI/kimi-cli/pull/2614) | **docs(plugins): document security and persistent data** | Clarifies plugin contract security boundaries: `plugin.json`, command-based tools, `inject` mechanism, and `~/.kimi/plugins/` installation scope; explicitly excludes separate plugin runtime security model | **Open** — documentation-only, awaiting review |
| [#2594](https://github.com/MoonshotAI/kimi-cli/pull/2594) | **fix(tools): preserve non-UTF-8 bytes in StrReplaceFile edits** | Critical bugfix: prior implementation used `errors="replace"` on full-file decode, causing U+FFFD (`EF BF BD`) corruption of any invalid UTF-8 sequence outside the edited region; now applies byte-level substring replacement on raw buffer | **Merged** — prevents silent data loss in binary files, images, legacy encodings |

---

## Feature Request Trends

**1. Persistent Memory Architecture** — The dominant theme. Users want:
- Automatic AI-managed notes across sessions
- Manual user-defined instructions (beyond current `agent.md`)
- Structured memory hierarchy (daily raw logs → curated long-term memory, per external references shared in #1478)

**2. Enterprise Network Compatibility** — Proxy/certificate handling (#760 pattern) suggests need for first-class corporate environment support.

**3. Documentation Completeness** — Memory features specifically cited as undocumented; users discovering capabilities through trial-and-error rather than reference docs.

---

## Developer Pain Points

| Pain Point | Evidence | Severity |
|-----------|----------|----------|
| **Context loss in large projects** | #1283 (40 comments), #1473 | 🔴 Critical — described as "painful," directly impacts productivity at scale |
| **Silent file corruption** | #2594 | 🔴 Critical — now resolved, but indicates UTF-8 assumptions pervading tool implementations |
| **Undocumented capabilities** | #1478 ("didn't see anything related to memory in the reference document") | 🟡 High — discoverability gap forces workaround archaeology |
| **Enterprise lockout** | #760 | 🟡 High — SSL/proxy config blocks regulated industries |
| **Plugin security ambiguity** | #2614 motivation | 🟡 Moderate — documentation PR suggests prior unclear trust boundaries |

---

*Digest compiled from github.com/MoonshotAI/kimi-cli public activity 2026-08-22/23.*

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-08-23

## Today's Highlights

The OpenCode team shipped a concentrated wave of reliability fixes targeting the **question tool ecosystem**, which dominated both issues and PRs with 15+ related closures addressing race conditions, UI traps, and routing failures across TUI, web, and desktop clients. Infrastructure improvements landed for **SSE connection tolerance** and **service worker lifecycle management**, while the contributor program continues scaling with multiple `opencode-agent[bot]` PRs handling pricing cleanup and cross-platform tab state. No new release was cut in the last 24 hours.

---

## Releases

*No releases in the last 24 hours.*

---

## Hot Issues

| # | Issue | Why It Matters | Community Reaction |
|---|-------|--------------|-------------------|
| [#1970](https://github.com/anomalyco/opencode/issues/1970) | Background Bash Execution (Like Claude Code's Ctrl+b) | Long-running builds/servers blocking the agent loop is a core workflow friction point vs. competing tools; async execution unlocks true agent autonomy | 35 👍, 20 comments — highest engagement feature request this cycle |
| [#11111](https://github.com/anomalyco/opencode/issues/11111) | VIM Keyboard Layout | Power-user input modality; critical for developers living in modal editors who context-switch to chat | 39 👍, 13 comments — strong demand from core demographic |
| [#15431](https://github.com/anomalyco/opencode/issues/15431) | macOS lock screen freezes session with "In Progress" task | Data loss risk on long tasks; macOS is dominant dev platform | 8 👍, 6 comments — reliability concern |
| [#27302](https://github.com/anomalyco/opencode/issues/27302) | Warp mode + Q&A captures all input, forces terminal kill | Complete UI deadlock in `/warp` mode; severity escalated by requiring `kill -9` | 6 👍, 6 comments — severe UX regression |
| [#22448](https://github.com/anomalyco/opencode/issues/22448) | OpenRouter 502 errors not retried, aborts subagent/session | Provider resilience gap; single transient failure cascades to full session loss | 6 👍, 6 comments — infrastructure robustness |
| [#28112](https://github.com/anomalyco/opencode/issues/28112) | AskUserQuestion modal undismissable on v1.14.50 | Modal trap under Claude Opus 4.7 orchestrator; violates fundamental UI safety | 1 👍, 5 comments — critical despite low votes |
| [#31646](https://github.com/anomalyco/opencode/issues/31646) | Agent loops calling non-existent "unknown" tool | Hallucination recovery failure; infinite error spam and permission fatigue | 4 👍, 4 comments — agent reliability |
| [#32686](https://github.com/anomalyco/opencode/issues/32686) | Stuck in plan mode when asked question | Mode transition deadlock; plan/build duality breaks on interactive interruption | 4 👍, 5 comments — workflow breakage |
| [#33540](https://github.com/anomalyco/opencode/issues/33540) | Glob tool hangs on 3.6M+ file directories | Unbounded `ripgrep` scan blocks Stream; no timeout or backpressure | 0 👍, 3 comments — performance at scale |
| [#30028](https://github.com/anomalyco/opencode/issues/30028) | Add `input` field to question tool for inline text | Current forced binary/multiple-choice limits agent information gathering | 1 👍, 3 comments — tool expressiveness |

---

## Key PR Progress

| # | PR | Feature / Fix | Status |
|---|-----|-------------|--------|
| [#44301](https://github.com/anomalyco/opencode/pull/44301) | Tolerate slow SSE event connections | Extends handshake timeout 2s → 30s; fixes repeated `/api/event` cancellation on high-latency links | 🔵 Open |
| [#44294](https://github.com/anomalyco/opencode/pull/44294) + [#44302](https://github.com/anomalyco/opencode/pull/44302) | Service worker registration fix + dev skip | Eliminates blocking HTML registration; defers to `window.load`; skips in dev to avoid 404s | 🟢 #44294 Closed, 🔵 #44302 Open |
| [#44296](https://github.com/anomalyco/opencode/pull/44296) | Preserve loopback server host | Uses page hostname for dev server on loopback URLs; preserves Basic Auth credentials across origin boundaries | 🔵 Open |
| [#44297](https://github.com/anomalyco/opencode/pull/44297) | Label Windows folder session tabs | Drive-root recognition + basename fallback; cross-platform regression test coverage | 🔵 Open (contributor) |
| [#42873](https://github.com/anomalyco/opencode/pull/42873) | Reference media parts by message+part ID | Fixes vision model fallback placeholders; enables precise media attribution when model lacks vision | 🔵 Open |
| [#44106](https://github.com/anomalyco/opencode/pull/44106) | Standardize safe text metrics | Deterministic typography system (12/16/20px tiers); eliminates layout shift from variable font metrics | 🔵 Open |
| [#44292](https://github.com/anomalyco/opencode/pull/44292) | Remove legacy 200k pricing | Cleans up `context_over_200k`/`experimentalOver200K` paths; simplifies tier selection logic | 🔵 Open (contributor) |
| [#44282](https://github.com/anomalyco/opencode/pull/44282) | Skip models.dev refresh when catalog unchanged | Eliminates 5-minute KV rewrite storm; cuts multi-MB noop traffic | 🟢 Closed |
| [#44281](https://github.com/anomalyco/opencode/pull/44281) | Anthropic dashed slug through AI Gateway | Fixes `claude-haiku-4.5` model resolution for `cloudflare-ai-gateway`; unblocks all Anthropic models | 🔵 Open (supersedes #44251) |
| [#44275](https://github.com/anomalyco/opencode/pull/44275) | Expire locations from session activity | Replaces infinite directory cache with idle deadline eviction (60min); fixes stale FS state | 🟢 Closed |

---

## Feature Request Trends

1. **Agent Execution Control** — Background/async bash (#1970) and mode transitions (plan↔build, #32686) dominate workflow requests. Users want non-blocking long-running operations with clean state management.

2. **Modal/Question Tool Richness** — Inline text inputs (#30028), markdown rendering (#27889), keyboard focus preservation (#29999), and dismissal safety (#28112) show the Q&A surface is maturing from "works" to "polished."

3. **Editor-Native Input** — VIM layout (#11111) and Mac shortcuts (#33568) reflect demand for muscle-memory parity with developers' primary editors.

4. **Tool Visibility Scoping** — Per-agent tool filtering (#24953) indicates plugin ecosystem complexity growing; global registration no longer scales.

---

## Developer Pain Points

| Theme | Manifestation | Frequency |
|-------|-------------|-----------|
| **Question Tool Fragility** | Routing failures, undismissable modals, focus theft, timeout loops, web/TUI/desktop desync | 15+ issues; highest concentration in dataset |
| **Provider Resilience Gaps** | OpenRouter 502s abort sessions; no retry logic; raw error detail lost in streams | 2 issues, but high severity |
| **macOS-Specific Reliability** | Lock screen freezes, credential/origin handling | Recurring platform-specific debt |
| **Unbounded Operations** | Glob hangs without timeout; ripgrep blocks Stream; compaction fails on thinking blocks | Performance/reliability boundary |
| **Agent Hallucination Recovery** | "unknown" tool loops; non-existent tool calls not gracefully degraded | Emerging agent behavior category |
| **Cross-Platform Polish** | Windows tab labeling, web↔TUI feature parity | Contributor program actively addressing |

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi Community Digest — 2026-08-23

## Today's Highlights
Windows users saw significant attention with fixes for ConPTY autowrap drift and bundled Node runtime improvements targeting slow I/O and Defender scanning issues. The MindsHub inference gateway joined as a built-in provider, while compaction reliability and context window management remained active areas of community concern.

---

## Releases
*No releases in the last 24 hours.*

---

## Hot Issues

| # | Issue | Why It Matters | Community Reaction |
|---|-------|--------------|-------------------|
| [#7547](https://github.com/earendil-works/pi/issues/7547) | Windows usage patterns and focus areas | Largest open discussion on Windows adoption with fragmented workflows (WSL, native, SSH); maintainer seeking signal on where to invest | 39 comments, strong engagement from Windows developers |
| [#6879](https://github.com/earendil-works/pi/issues/6879) | Auto-compaction fails past 100% context until API overflow | Critical reliability bug: 2+ hour agentic sessions silently grow to 373k tokens before forced compaction; hits cost and latency hard | 20 comments, 18 upvotes; acknowledged as priority |
| [#7130](https://github.com/earendil-works/pi/issues/7130) | Kitty backspace double-delete from unfiltered release events | Terminal protocol bug affecting daily editing UX for Kitty users | 11 comments; protocol-level fix needed |
| [#8167](https://github.com/earendil-works/pi/issues/8167) | llama.cpp router-mode models invisible in selector | Local inference workflow broken for llama-server users; config workaround insufficient | 9 comments; resolved by PR #8479 |
| [#8468](https://github.com/earendil-works/pi/issues/8468) | GitHub Copilot login timeout | Auth regression blocking Copilot integration; user tested unreleased fix | 5 comments; pending release verification |
| [#8376](https://github.com/earendil-works/pi/issues/8376) | Configurable model selection persistence scope | UX friction: `/model` choices leak across contexts unexpectedly | 5 comments; design discussion on session/directory/workspace granularity |
| [#7885](https://github.com/earendil-works/pi/issues/7885) | npm search not indexing new pi-packages | Extension ecosystem discoverability broken since Aug 4; gallery stale | 5 comments; infrastructure issue affecting publishers |
| [#8452](https://github.com/earendil-works/pi/issues/8452) | Compaction prompt fidelity for coding state | Core AI behavior: summaries lose actionable checkpoint state vs. readable prose | 4 comments; design tension between human-readable and machine-actionable |
| [#8442](https://github.com/earendil-works/pi/issues/8442) | Backspace ignored under Kitty protocol with legacy `0x7f` | Terminal compatibility edge case in herdr panes; Ctrl+Backspace works as workaround | 4 comments; protocol negotiation gap |
| [#8464](https://github.com/earendil-works/pi/issues/8464) | Output-limit continuation and mid-turn compaction | Autonomy gap: models hitting token limits stall instead of self-continuing; long tool loops bypass compaction | 4 comments; two related reliability requests bundled |

---

## Key PR Progress

| # | PR | Feature / Fix | Status |
|---|-----|-------------|--------|
| [#8500](https://github.com/earendil-works/pi/pull/8500) | Fix plan-mode bash guard false positives | Eliminates over-blocking of read-only commands containing "code" or demo text; reduces friction in plan verification | **Merged** |
| [#8474](https://github.com/earendil-works/pi/pull/8474) | Bundle Node runtime for coding-agent | Dramatically reduces file count on startup; targets Windows Defender scanning pain and slow I/O systems | **Merged** |
| [#8488](https://github.com/earendil-works/pi/pull/8488) | Add MindsHub provider | Single-endpoint gateway for Claude/GPT/Gemini/Kimi/DeepSeek/Qwen/GLM via OpenAI-compatible API | **Merged** |
| [#8487](https://github.com/earendil-works/pi/pull/8487) | Expose finish reason compatibility override | Closes API surface gap; already implemented but not typed | **Open** |
| [#8486](https://github.com/earendil-works/pi/pull/8486) | Editor-scroll capture and verification tooling | Test infrastructure for #8484; scriptable TUI demo for scroll regression prevention | **Merged** |
| [#8485](https://github.com/earendil-works/pi/pull/8485) | Disable autowrap around main-screen renders | Fixes ConPTY drift causing cursor disappearance below fold on Windows Terminal | **Merged** |
| [#8482](https://github.com/earendil-works/pi/pull/8482) | Correct custom footer docs | Points documentation at `ctx.getContextUsage()` API; doc fix only | **Open** |
| [#8479](https://github.com/earendil-works/pi/pull/8479) | Expose unloaded llama.cpp presets | Enables `llama-server --models-preset` and llama-swap workflows; fixes #8167 | **Merged** |
| [#7148](https://github.com/earendil-works/pi/pull/7148) | Experimental loadout management | Mid-session extension enable/disable with persistence; significant UX change needing confirmation design | **Open (draft)** |
| [#8295](https://github.com/earendil-works/pi/pull/8295) | Locale switching via `/settings` | English/Simplified Chinese selector with validation and persistence | **Merged** |

---

## Feature Request Trends

1. **Provider ecosystem expansion** — MindsHub, Parasail.io, DeepSeek vision models: demand for unified gateway access and rapid model catalog updates
2. **Context and memory architecture** — Multiple proposals for SQLite-backed retrieval, configurable compaction prompts, mid-turn compaction, and output-limit continuation; signal that current context management strains at long-session scale
3. **TUI/rendering configurability** — Viewport primitives, block-level expand/collapse defaults, editor scroll behavior: extensions need deeper UI control
4. **Windows as first-class target** — Not just compatibility but performance (Defender/I/O), keybinding integration, and terminal-specific behavior
5. **Extension lifecycle control** — Loadout management, exclusion flags, scope-persistence for model selections: users want finer-grained control over what loads when

---

## Developer Pain Points

| Pain Point | Frequency | Evidence |
|-----------|-----------|----------|
| **Windows performance and compatibility** | Very high | #7547, #8372, #8484, #8485, #8474, #8441 — spans terminal rendering, keybindings, path separators, and antivirus I/O bottlenecks |
| **Context/compaction reliability** | High | #6879, #8452, #8464, #8498 — auto-compaction timing, fidelity, and token accounting all questioned |
| **Provider/model catalog freshness** | High | #8469, #8438, #8450, #8488, #8489 — new models and gateways need manual PRs; no automated ingestion |
| **Terminal protocol edge cases** | Medium-High | #7130, #8442, #8434 — Kitty protocol, ConPTY, encoding issues across terminal emulators |
| **Extension discoverability** | Medium | #7885 — npm search indexing failure blocks ecosystem growth |
| **SDK observability gaps** | Medium | #8380 — request lifecycle hooks lack correlation IDs for extension state management |

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-08-23

---

## 1. Today's Highlights

The v0.22.0 stable release landed alongside its nightly build, delivering critical Web Shell memory protections that prevent OOM crashes through bounded transcript retention and smarter replay trimming. The review system gained significant intelligence: loops now self-diagnose instability by citing specific files with recurring findings, and a new "persistently-critical convergence advisory" enables land-with-residual-risk decisions when review loops stall. Meanwhile, the Node REPL architecture pivoted from built-in core tool to standalone MCP server, unblocking third-party experimentation before core integration.

---

## 2. Releases

| Version | Key Changes |
|---------|-------------|
| **[v0.22.0](https://github.com/QwenLM/qwen-code/releases/tag/v0.22.0)** | Web Shell OOM prevention via transcript bounds and replay trimming; review loops gain explanatory instability diagnostics with file-level citations |
| **[v0.22.0-nightly.20260823.1007bcacfc](https://github.com/QwenLM/qwen-code/releases/tag/v0.22.0-nightly.20260823.1007bcacfc)** | Web Shell workspace CWD passthrough fix when opening from overview panel |

---

## 3. Hot Issues

| # | Issue | Why It Matters | Community Signal |
|---|-------|--------------|------------------|
| **[#8102](https://github.com/QwenLM/qwen-code/issues/8102)** | Deterministic tool-execution boundaries for trustworthy agent runtime | Foundational security architecture: proposes keeping the LLM outside the trust boundary with deterministic constraint/authorization/observation/evaluation of model actions. 17 comments indicate deep maintainer engagement on agent safety primitives. | 🔥 High engagement, needs-discussion |
| **[#9278](https://github.com/QwenLM/qwen-code/issues/9278)** | `/review` publish-time convergence advisory design | Documents the "runaway review loop" problem (fix → larger diff → more findings) and proposes telemetry-driven damping. Critical for CI/CD reliability at scale. | In-progress, 9 comments |
| **[#9556](https://github.com/QwenLM/qwen-code/issues/9556)** | Pipeline code execution as invoking user — security review | Every unresolved finding in #9221 shares the same precondition: code executing as the review user. Questions the entire permission model for review automation. | 8 comments, security-flagged |
| **[#9198](https://github.com/QwenLM/qwen-code/issues/9198)** | OOM after week-long session on 1TB server | Memory leak in long-running sessions corrupts tmux state; reproducible vs. Kimi Code. Affects operational reliability for persistent agent deployments. | 5 comments, need-information |
| **[#9733](https://github.com/QwenLM/qwen-code/issues/9733)** | Loop detection false-positives kill unattended automation | Legitimate verification cycles (write→run→edit→re-run) trigger termination, and killed turns cannot resume without human intervention. Breaks CI/automation use cases. | 4 comments, need-retesting |
| **[#9333](https://github.com/QwenLM/qwen-code/issues/9333)** | Session-level persistent Node REPL as standalone MCP server | **Architecture pivot**: delivery model changed from built-in core tool to independent `@qwen-code/node-repl-mcp` package. Enables faster iteration and value validation before core commit. | 3 comments, ready-for-human |
| **[#9757](https://github.com/QwenLM/qwen-code/issues/9757)** | Auto Mode classifier stage 1 unavailable with OpenRouter | Regression in provider-agnostic action classification; forces manual approval fallback and degrades UX for OpenRouter users. | 3 comments, P2 |
| **[#9752](https://github.com/QwenLM/qwen-code/issues/9752)** | ACP message rewriting crashes on unreadable promptFile | `EISDIR` and similar read failures bypass error handling, causing hard session startup crashes. Edge case in configuration validation. | 3 comments, P2 |
| **[#9762](https://github.com/QwenLM/qwen-code/issues/9762)** | Preserve source session names when branching | UX polish: branched sessions currently get UUID fragments instead of inheriting meaningful names from source. Small fix, high user impact. | 2 comments, in-progress |
| **[#9725](https://github.com/QwenLM/qwen-code/issues/9725)** | VS Code companion WebShell transcript: real VS Code runtime acceptance | Experimental flag exposes WebShell UI in VS Code companion; needs validation against actual VS Code webview runtime constraints. IDE integration roadmap item. | 2 comments, roadmap/ide-integration |

---

## 4. Key PR Progress

| # | PR | Feature / Fix | Status |
|---|-----|-------------|--------|
| **[#9768](https://github.com/QwenLM/qwen-code/pull/9768)** | **feat(review): make coverage a sealed, classified ledger** | Transforms `/review` chunk coverage into an auditable ledger with per-gap reasoning and separate read/post metrics. Enables better convergence diagnostics. | Open, fresh today |
| **[#9769](https://github.com/QwenLM/qwen-code/pull/9769)** | **feat(web-shell): unblock git update on dirty working tree** | Replaces opaque error with resolution panel offering stash-or-discard options. Critical for Web Shell workflow continuity. | Open, fresh today |
| **[#9764](https://github.com/QwenLM/qwen-code/pull/9764)** | **fix(session): preserve source titles when branching** | Implements #9762: inherits picker display names, normalizes legacy `(Branch)` suffixes, allocates `Title(N)` cleanly. | Open, fresh today |
| **[#9737](https://github.com/QwenLM/qwen-code/pull/9737)** | **refactor(cli): enforce utils leaf-layer dependency direction** | Mechanical enforcement of `packages/cli/src/utils/` as true leaf layer. Reduces circular dependency risk in CLI architecture. | Open |
| **[#9728](https://github.com/QwenLM/qwen-code/pull/9728)** | **fix: repair Windows and macOS test lane failures** | Unblocks CI for two platforms: product fixes + test-fixture repairs + harness repair. Prerequisite for reviving #9370. | Open, self-reported |
| **[#9741](https://github.com/QwenLM/qwen-code/pull/9741)** | **fix(review): screen content filters before probe tree restore** | Security hardening: prevents `scratch-tree` operations when local git config defines content filters (smudge/clean). Replaces #9566. | Open |
| **[#9526](https://github.com/QwenLM/qwen-code/pull/9526)** | **feat(review): persistently-critical convergence advisory** | Adds "land with residual risk" exit path when review loops stall on recurring Criticals with fresh first-time findings present. Addresses #9278. | Open, needs-human |
| **[#9627](https://github.com/QwenLM/qwen-code/pull/9627)** | **feat(review): back comment-status and presubmit for Aone Code** | Enables comment-aware pre-write flows (threading, resolved state, blocker signal) for Aone Code MR targets. Enterprise integration gap. | Open |
| **[#8583](https://github.com/QwenLM/qwen-code/pull/8583)** | **feat(web-shell): experimental session workflow cockpit** | End-to-end Session Workflow: plan capture → revision-bound approval → transcript projection → Agent execution → WebShell. Major UX initiative. | Open, long-running |
| **[#9273](https://github.com/QwenLM/qwen-code/pull/9273)** | **feat(review): capture-tui — rendering claims get pixels** | `qwen review capture-tui` produces pixel evidence (PNG via `freeze`, ANSI fallback) instead of prose arguments. Verifiability upgrade for UI reviews. | Open, needs-human |

---

## 5. Feature Request Trends

| Direction | Evidence | Momentum |
|-----------|----------|----------|
| **Trustworthy / constrained agent runtime** | #8102 (deterministic boundaries), #9556 (execution identity), #9333→#9335 (Node REPL sandboxing) | 🔥🔥🔥 Security and sandboxing dominating architectural discussion |
| **Review system convergence & telemetry** | #9278, #9526, #9768, #9674, #9742 | Heavy investment in making `/review` self-diagnosing and loop-safe |
| **IDE integration (VS Code companion)** | #9725, #9726, #9727, #9743 (drag-drop files) | Rapid iteration on WebShell-in-VSCode, CSP and runtime hardening |
| **Session lifecycle resilience** | #9762/#9764 (branching names), #9626 (persisted lifecycle), #9686 (model restore), #9664 (HITL restore) | Daemon and long-session operational maturity |
| **Computer Use / multimodal tools** | #9333, #9334, #9335 (Node REPL → Computer Use SDK) | Third-phase roadmap, MCP-first architecture |

---

## 6. Developer Pain Points

| Pain Point | Frequency | Manifestations |
|------------|-----------|----------------|
| **Review loop instability** | Very High | #9278, #9526, #9674, #9768, #9556 — "runaway feedback loops" where fixes enlarge diffs and amplify findings; damping currently relies on prose heuristics ("~5 rounds") rather than hard telemetry |
| **Session/memory management at scale** | High | #9198 (OOM on 1TB server), #9303 (transcript bounds in v0.22.0), #9573 (resumed session tool result loss), #9733 (loop detection killing automation) — long-running sessions are operationally fragile |
| **Cross-platform CI reliability** | Moderate-High | #9728 (Windows/macOS lanes red), #9228 (self-hosted workspace wipe destroying `.git`) — platform parity and runner hygiene block velocity |
| **Provider-specific regressions** | Moderate | #9757 (OpenRouter classifier failure), #9002 (SDK `permission_mode="auto"` rejection) — abstraction leaks in provider routing |
| **Configuration edge cases crashing startup** | Moderate | #9752 (ACP promptFile EISDIR), #2862 (checkpointing hang) — input validation gaps in startup paths |
| **VS Code companion gaps vs. Web Shell** | Emerging | #9743 (missing drag-drop), #9725-#9727 (CSP, runtime acceptance, block identity) — feature parity chasing |

---

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI Community Digest — 2026-08-23

## Today's Highlights

The project is in a **heavy operational readiness push** following the v0.9.11 release, with 13 new issues filed in a single day from an internal ops audit driving clipboard resilience, onboarding UX, and terminal polish. Simultaneously, contributor **M-Maciej** landed a substantial 5-commit supervision stack PR addressing long-lived session operability, while pricing logic was corrected for DeepSeek V4's new Beijing weekend off-peak rules.

---

## Releases

*No releases in the last 24 hours.*  
v0.9.11 was prepared yesterday ([PR #5542](https://github.com/Hmbown/CodeWhale/pull/5542)) but remains unreleased as of this digest.

---

## Hot Issues

| # | Issue | Why It Matters | Community Signal |
|---|-------|--------------|----------------|
| **#5546** | [[bug] [redacted] output from tools impairs editing](https://github.com/Hmbown/CodeWhale/issues/5546) | Regression from v0.9.11 where tool outputs now contain literal "[redacted]" tokens, breaking model comprehension and user trust in upgraded sessions. Filed by user **ronohara** with session transcript. | Active user report, 2 comments, zero engagement from maintainers yet—high urgency given production impact. |
| **#5534** | [[bug] Goal-continuation cadence bypassed on within-turn dispatch](https://github.com/Hmbown/CodeWhale/issues/5534) | Core scheduling bug: the "quiet period" between goal continuations is ignored for resumed/CLI sessions, causing rapid-fire passes and potential rate-limit/cost issues. | Filed by **M-Maciej** who also has the fix in PR #5535; self-identified, self-patched pattern. |
| **#5533** | [[enhancement] Control surface for supervised operation](https://github.com/Hmbown/CodeWhale/issues/5533) | Enables external supervisors (CI, fleet runners, tmux wrappers) to manage Codewhale sessions via Unix socket—critical for unattended automation at scale. | Part of M-Maciej's 4-issue supervision suite; no objections, infrastructure-oriented. |
| **#5532** | [[enhancement] /relaunch — switch running session to current binary](https://github.com/Hmbown/CodeWhale/issues/5532) | Eliminates friction in the `/update` flow where users must manually restart; closes a known gap acknowledged in updater design notes. | Same supervision theme; paired with #5533 and #5531 as operational trilogy. |
| **#5531** | [[enhancement] Local lifecycle event outbox](https://github.com/Hmbown/CodeWhale/issues/5531) | JSONL/webhook events for `turn_stalled`/`turn_failed` enable external alerting on hung agents—essential for overnight fleet operations. | M-Maciej's third supervision issue; explicitly references `herdr` supervisor use case. |
| **#5557** | [[enhancement] /import-claude: migrate ~/.claude permissions, env vars, MCP servers](https://github.com/Hmbown/CodeWhale/issues/5557) | Competitive migration path from Claude Code; currently only MCP discovery exists, missing full config portability. | Filed by project owner **Hmbown** from v0.9.11 ops audit; strategic user acquisition play. |
| **#5556** | [[enhancement] Onboarding: opt-in /tutorial pager](https://github.com/Hmbown/CodeWhale/issues/5556) | First-run UX gap for users arriving from Claude/Cursor/Codex; maps familiar concepts to Codewhale primitives. | Hmbown ops audit item; localization-ready design specified. |
| **#5555** | [[enhancement] Clipboard export: write last-copy backup when clipboard fails](https://github.com/Hmbown/CodeWhale/issues/5555) | SSH/headless sessions currently lose export content silently; predictable fallback file prevents data loss. | Hmbown ops audit; narrow but high-friction fix for remote workflows. |
| **#5550** | [[enhancement] @path:START-END line-range mentions](https://github.com/Hmbown/CodeWhale/issues/5550) | File mention system lacks range precision and hidden-file visibility; forces verbose workarounds in large codebases. | Hmbown ops audit; productivity feature with clear implementation path. |
| **#5543** | [Persist child tool approvals through durable receipt path](https://github.com/Hmbown/CodeWhale/issues/5543) | Security/consistency gap: child agent approvals bypass the durable audit trail used for parent tools, breaking compliance assumptions. | Filed by **cyq1017**; zero comments but touches core trust model. |

---

## Key PR Progress

| # | PR | Status | Feature / Fix | Technical Detail |
|---|-----|--------|-------------|----------------|
| **#5535** | [Supervised operation stack: lifecycle outbox, /relaunch, control socket, goal-continuation fix](https://github.com/Hmbown/CodeWhale/pull/5535) | OPEN | 5-commit operational infrastructure bundle | Unifies #5531–#5534 into single mergeable unit; adds `[lifecycle_outbox]` config, `RuntimeBackendKind::External`, `/relaunch` command, and corrects continuation cadence bypass. |
| **#5545** | [fix(pricing): bill whole Beijing weekends off-peak for DeepSeek V4](https://github.com/Hmbown/CodeWhale/pull/5545) | OPEN | Cost correction for API pricing change | `deepseek_is_peak` previously used UTC hour only; now respects Beijing Time weekend rules effective 2026-08-23. Timezone-aware billing critical for cost-sensitive users. |
| **#5544** | [feat(web): move docs/subagents and docs/mcp onto dictionary spine](https://github.com/Hmbown/CodeWhale/pull/5544) | OPEN | Localization refactor continuation | Eliminates 34 `isZh` branches across two docs pages; follows #5520 pattern in #5337 series. |
| **#5406** | [feat(tui): prefab provider templates and test-connection](https://github.com/Hmbown/CodeWhale/pull/5406) | **CLOSED** | Provider onboarding simplification | Merged: built-in templates for OpenCode Zen, OpenCode Go, Agnes, SenseNova with live connection validation. |
| **#5530** | [fix(cli): route legacy completions through public binary](https://github.com/Hmbown/CodeWhale/pull/5530) | **CLOSED** | CLI consistency fix | Legacy `codewhale completions <shell>` now uses canonical generator; resolves #5526. |
| **#5542** | [release: prepare Codewhale v0.9.11](https://github.com/Hmbown/CodeWhale/pull/5542) | **CLOSED** | Release preparation | Excludes benchmark subtree; byte-for-byte verified against gated location. |
| **#5523** | [refactor(tui): extract tool call stages from turn loop](https://github.com/Hmbown/CodeWhale/pull/5523) | **CLOSED** | Code health: turn loop decomposition | Extracts `plan_tool_calls`, `execute_planned_tools`, `process_tool_results`; preserves cancellation and state semantics. |
| **#5524** | [feat(tui): add multi-file read_lints operation](https://github.com/Hmbown/CodeWhale/pull/5524) | OPEN | LSP tool enhancement | `read_lints` operation for multiple files via session `LspManager`; addresses #4070 without spawning new server lifecycles. |
| **#5525** | [refactor(tui): adopt command shapes in utility group (FEAT-018)](https://github.com/Hmbown/CodeWhale/pull/5525) | OPEN | Architecture: command shape migration | Converts utility command group to external command shapes from FEAT-014/015; boundary change without file relocation. |
| **#1701** | [chore(deps): bump portable-pty to 0.9.0](https://github.com/Hmbown/CodeWhale/pull/1701) | **CLOSED** | Dependency: loongarch64 support | Drops stale `nix 0.25.1` transitive dep; resolves #1531. |

---

## Feature Request Trends

1. **Operational Supervision & Fleet Management** — Four coordinated issues (#5531–#5534) and PR #5535 establish a clear trend: users running Codewhale in automation, CI, and overnight fleet contexts need machine-readable lifecycle events, external control interfaces, and seamless binary updates. The project is responding with first-class infrastructure rather than workarounds.

2. **Competitive Migration & Onboarding** — Issues #5557 (`/import-claude`) and #5556 (`/tutorial` pager) reveal strategic focus on reducing friction for users switching from Claude Code, Cursor, and Codex. The ops audit explicitly sourced these from "potential audience" analysis.

3. **Resilience in Constrained Environments** — Clipboard fallback (#5555), OSC 12 cursor handling (#5554), and hidden-file picker visibility (#5550) all address degraded terminal environments (SSH, headless, reduced-motion). A pattern of graceful degradation rather than hard failure.

4. **Cost Transparency & Control** — #5553 (context inspector showing MCP/token costs) and #5545 (pricing fix) reflect user demand to understand and optimize inference spend, particularly with third-party MCP servers inflating context windows.

---

## Developer Pain Points

| Pain Point | Evidence | Severity |
|-----------|----------|----------|
| **Session state durability gaps** | #5543 (child approvals bypass receipt path), #5534 (cadence bypass) | High — affects audit compliance and rate-limit safety |
| **Post-upgrade behavioral regressions** | #5546 (redacted output after HEAD update) | High — erodes trust in `cargo install --git` workflows |
| **TUI file mention ergonomics** | #5550 (no line ranges, hidden files hard to reach) | Medium — daily friction in large codebase workflows |
| **CI coverage holes for non-standard branches** | #5547 (ubuntu job placeholder, skips non-mirrored PRs) | Medium — risk for `codex/*` integration branches |
| **Tool approval UX in nested agents** | #5543, implied by supervision stack | Medium — parent/child asymmetry complicates automation |
| **Localization debt in docs** | #5544 (ongoing `isZh` elimination in #5337 series) | Low but persistent — architectural hygiene |

</details>

<details>
<summary><strong>Grok Build</strong> — <a href="https://github.com/xai-org/grok-build">xai-org/grok-build</a></summary>

No activity in the last 24 hours.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/brysoh435-dev/agents-radar).*