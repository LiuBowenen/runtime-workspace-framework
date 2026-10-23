# Runtime, Workspace, and External Knowledge Sources: A Framework Analysis of the Minimal Primitives of Agent Systems

October 2026

> Note: This framework originates from a practitioner working on Agent deployment in industry; the core judgments are preserved verbatim and marked "author's view" (translated from the original Chinese). Evidence gathering and formalization were completed in collaboration. All references carry accessible links; source grading is documented at the end.

## Abstract

Terminology in AI turns over on a quarterly basis, while structural discussion remains scarce. This paper integrates five streams of evidence — closed-loop self-improvement, runtime and context engineering, workspace and verifiability, distillation economics, and external signals with the experiential turn — and distills four empirical regularities, from which LLM agent systems are reduced to three primitives: the runtime (execution process layer), the workspace (persistent state layer), and the external knowledge source (the world term). On this framework, four structural conclusions are derived. First, the runtime consists of judgment functions and enforcement functions; the former are continuously absorbed by base-model iteration, so the runtime's evolutionary endpoint is thinning. Second, weight distillation is a condition-triggered branch gated by four thresholds, not a default route. Third, the asset hierarchy across generations is model below runtime, runtime below workspace; the only means of value preservation are backup, lineage, and evaluation sets. Fourth, the depth to which business processes are handed to AI is ordered by acceptance executability, and the human position converges to the two ends: specification and acceptance adjudication. The paper closes with eight testable predictions, each with an operational acceptance criterion, over an observation window ending in late 2028. The whole argument compresses into one sentence: the model will not devour everything, but it will keep replacing acceptance-executable, non-physical components.

## 1 Evidence Base

The primary problem in current AI agent research is conceptual drift. Terms such as scaffold, harness, agent loop, and cognitive core turn over quarterly, while claims about "AI self-evolution" routinely conflate different levels of improvement targets and are mutually incomparable. This paper takes a conservative path: fix the empirical evidence first, then derive structure from it. The evidence is organized into five streams, each corresponding to one component of the framework; open-source implementations are listed separately in Section 1.7.

### 1.1 Closed-Loop Self-Improvement (RSI)

On whether AI can improve itself — recursive self-improvement (RSI), meaning a system using its own outputs to improve its own capabilities — a survey covering roughly 1,250 arXiv papers from 2024–2026 divides improvement targets into six levels [1]. The two lowest levels are single outputs (L1, Self-Refine [2]) and cross-task strategy text (L2, Reflexion [3], ACE [10]); above them are prompts, tools, and agent code (L3, STOP [4], ADAS [5], DGM [6], AlphaEvolve [7]); then the improver itself and its evaluator (L4, Agent0 [9], HGM [8], Red Queen [12]); and at the top, end-to-end autonomous training of successor base models (L5), with no public implementation to date. Empirically verified numbers include: after a coding agent rewrote its own code, the Darwin Gödel Machine raised SWE-bench (a benchmark evaluating defect repair on real GitHub repositories) from 20.0% to 50.0% [6]; Recursive's automated research system reached state of the art on NVIDIA's GPU-kernel optimization leaderboard [1]. The boundary is equally clear: when a frontier agent spent six days reproducing an unpublished NeurIPS study, engineering execution was largely completed but original judgment failed, and the original authors rejected the manuscript [15].

### 1.2 Runtime and Context Engineering

Model capability is not the sole determinant of long-horizon performance. The same base model (a pre-trained, non-customized foundation model such as Claude, GPT, or Qwen; hereafter "base model") placed in different runtimes (runtime: one of this paper's two core concepts — the software process layer that wraps the model and executes tasks; also called harness or scaffold in the literature) can differ by up to 48 percentage points on benchmarks — Cursor's measurement was 46% vs. 80% (industry measurement, grade-B source [14]). More notable is error compounding: an agent with a 97% single-turn success rate retains only 54% success over a 20-turn task [14]. Long-horizon failure is therefore driven mainly by state drift and accumulating error, not by insufficient reasoning.

Engineering blogs from the two leading labs provide first-hand evidence. Anthropic's "Effective harnesses for long-running agents" [23] states that the bottleneck of sustained cross-session work lies not in the model but in runtime robustness — network interruptions, API errors, and context overflow (the context window being the total text a model can ingest in a single inference pass, its "working memory") can each zero out a long task; their scheme has an initializer agent set up the environment (feature checklist, progress log, initial commit) while a worker agent advances incrementally, using workspace files (workspace: the second core concept — a persistence layer in the form of a file system holding source, memory, evaluation data, and version history) as the cross-session interface. OpenAI's "Harness engineering" [24] reports the more extreme case: a software product of roughly one million lines written entirely by Codex with zero human-written code, completed in five months across about 1,500 merged PRs, driven by only three engineers — whose role was redefined as designing the environment, specifying intent, and building feedback loops: "humans steer, agents execute." Context loading itself has become an independent research direction: MemGPT ported operating-system memory paging into context management [25], Cartridges compress long-context representations via self-study [26], and Anthropic has published a dedicated piece on context engineering [27].

### 1.3 Workspace and Verifiability

The workspace's decisive role in verification now has large-scale experimental evidence. WorkForge (2026) finds that professional tasks and their verification are inseparable from the workspace: the workspace determines what an agent can explore, and observable facts within the workspace determine what can be proposed and reliably verified [20]. Building on this, the work constructs 16,700 verifiable environments across 40 professional domains, and post-training lifts Qwen3.5-35B from 45.5 to 73.6 on GDPVal (an OpenAI benchmark evaluating economically valuable occupational tasks). Independent measurement of evaluation infrastructure is maturing quickly as well: METR's "task-length horizon" metric (the longest task, measured in human time, a model can complete at 50% success) has doubled roughly every 7 months over six years, with frontier models now at roughly 12–16 hours [18]. The dense publication of harness and evaluation methodology posts by the two labs across 2025–2026 [23,24,27] is an observable side-signal: the bottleneck sits at judgment and acceptance, not generation.

### 1.4 Distillation Economics

Distillation (using a large model as teacher and its outputs to train a small model as student, so the student acquires most of the teacher's capability at low cost) was formalized by Hinton et al. in 2015 [21]: the student learns the teacher's full output distribution rather than only final answers. Two structural changes have occurred recently.

First, distillation has become a standard step in frontier productization: Claude Haiku, GPT-5 mini, and other smallest-tier models are distilled from flagships and carry real production traffic, with OpenAI and Anthropic documenting distillation as a standard workflow. Second, the practical capability of distilled small models is systematically verified. DeepSeek-R1 [22] provides the most complete public evidence: R1-Distill-Qwen-1.5B, a 1.5B student, outperforms the original GPT-4o and Claude 3.5 Sonnet on reasoning benchmarks; 7B and 14B students beat comparators several times their size; the 32B student beats OpenAI o1-mini. Reasoning capability does transfer from large into small models.

The boundary must be recorded equally clearly: distillation mainly transfers in-distribution, patterned capability, and the teacher's biases are inherited along with it; on novel task types and long reasoning chains, students remain significantly weaker than teachers [21,22]. This grounds the distillation-threshold constraints derived later.

### 1.5 External Signals and the Experiential Turn

Shumailov et al. proved in Nature that recursive training on self-produced data accumulates model bias until the model degenerates [11]. Silver and Sutton generalized the point into a paradigm shift: AI is moving from the "era of human data" to the "era of experience," with training signals shifting from static corpora to the products of system-environment interaction [13]. External signals are therefore not a training trick but the precondition of all continued learning.

### 1.6 Four Regularities

The five evidence streams jointly support four regularities, which are the premises of all subsequent derivation.

Regularity 1, the cost-of-verification law (streams 1.1, 1.3): the speed of improvement is determined by the cost of verification in the domain; the dense zone of success coincides with the distribution of automatic verifiability [1,20]. The bottleneck is judgment, not generation.

Regularity 2, the closed-loop structure law (streams 1.1, 1.2): every known self-improving system reduces to a five-step loop of execute, observe, mutate, verify, retain; difficulty concentrates at verify and retain, and the loop's sustainability depends on the robustness of runtime–workspace interfaces [1,23,24].

Regularity 3, the collapse law (streams 1.4, 1.5): closed loops over self-produced data degenerate [11]; real external signals are a precondition, not a supplement, of continued learning [13].

Regularity 4, the scarcity-of-judgment law (streams 1.2, 1.3, 1.4): generation keeps getting cheaper — OpenAI's million-line, zero-human-code product is the extreme case [24] — while failures and bottlenecks concentrate at judgment: evaluation gaps, verification, original judgment [1,15,23].

### 1.7 Open-Source Implementations

Most of the above progress has reproducible public code: OpenEvolve (open-source reproduction of AlphaEvolve, Apache-2.0) [28]; the official Darwin Gödel Machine implementation (runs model-generated code; must be executed in isolation) [29]; Reflexion (MIT) [30]; awesome-rsi (index of RSI papers, benchmarks, and projects) [31]; and METR's evaluation-analysis code (all data and analysis behind the horizon metric) [32]. Quantitative conclusions cited in this paper that involve these systems can be reproduced from the corresponding repositories.

## 2 The Framework: Deriving Three Primitives

The framework starts from the author's structural judgment, preserved verbatim:

> **"The real world is runtime plus workspace."** (author's view)

The judgment has a clear empirical correlate: strip the terminology from existing agent systems and two invariant layers appear — a process responsible for generation and execution, and a file layer responsible for state. The three primitives are not parallel assumptions; each is derived from a regularity and has an independent evidence stream.

**Definition 1 (Runtime).** Derived from Regularity 2: execution, loading, and write-back in the loop must be carried out by a process outside the model, because the model can only emit symbol sequences. The runtime is that process, with four duties. First, loading — deciding what enters the context each turn: the context window is the register, the workspace is the disk, the runtime is the mover; grounded in MemGPT's paging [25], Cartridges' compression [26], and Anthropic's context-engineering practice [27]. Second, system calls — the tool set: the tool boundary is the model's legal operation domain, since the model can only produce symbols. Third, scheduling — task decomposition, parallelism, retries. Fourth, interrupt handling — the signal protocol for interruption and error feedback. The runtime's physical boundary: from user input entering to model output taking effect in the world. The runtime is a process, not a concept: Codex and Claude Code are distributions that will iterate, but the four kernel duties — load, syscall, schedule, interrupt — do not change.

**Definition 2 (Workspace).** Also derived from Regularity 2: improvement at L2 and above requires persistence, and persistence's physical carrier is the file system. The workspace is the only structure that survives across sessions and base-model versions, with four duties. First, layering of source and artifacts: regenerable and non-regenerable content must be stored separately. Second, evidence and adjudication: the eval set (a fixed task suite for checking system performance) and the holdout set (a suite deliberately excluded from all optimization, used only for final checks to prevent "feeling good on practice problems") are the landing points of the loop's verify and retain steps — WorkForge shows verification must trace back to observable evidence in the workspace [20]. Third, configuration and permission boundaries: what may be loaded, what may be rewritten. Fourth, lineage: cross-generation improvement of the DGM type depends on evolutionary archives [6]; a workspace without lineage is a cache, a workspace with lineage is an asset. Notably, both labs' engineering practices have converged on the workspace as the cross-session interface: Anthropic's progress logs and feature checklists [23], OpenAI's in-repo knowledge base and doc-gardening mechanism (a dedicated agent periodically scans stale docs and submits fixes) [24] — workspace curation, productized.

**Definition 3 (External Knowledge Source).** Derived from Regularity 3: the loop needs real signals from outside the system, uncontaminated by business data, so this primitive must sit outside both runtime and workspace, with its own collection cadence, entering the workspace via an explicit loading path. The author's original formulation:

> **"Outside the system there is an independent thing I call the crucible, or knowledge set. Every month I pull good projects in to read and analyze — that is my source of judgment about the external world."** (author's view)

The era-of-experience paradigm [13] supplies this primitive's theoretical position: products of real-environment interaction are the ultimate source of learning signal, and human curation — periodically pulling projects, sedimenting criteria — is its realization at individual and organizational scale.

**Proposition 1 (Scope).** Derived from Regularity 1: adjudication is only cheap inside closed domains, so improvement must happen at the granularity of a closed repository. The author's original formulation:

> **"In project mode, one project is one repository — same as development; in non-project mode, one business domain is one repository."** (author's view)

All of WorkForge's verifiers are anchored inside closed workspaces [20], providing independent validation. The three primitives' division of labor now closes: the external knowledge source handles the world, the runtime handles execution, the workspace handles sedimentation.

## 3 Four Structural Corollaries

### 3.1 Corollary 1: The Runtime Thins

Partition the runtime's duties by reason of existence and the direction of evolution becomes clear. One class is judgment: loading strategy, scheduling, retries, routing. These duties sit in runtime code today only because base models have a temporary judgment gap; the 48-point cross-runtime spread is the present value of that gap, and it narrows as base models iterate [14]. The other class is enforcement: sandbox (an isolated, controlled execution environment restricting file, network, and credential access), permissions, file writes, credentials. Enforcement exists not because "the model is not yet strong enough" but because "a model of any strength needs boundaries" — a model physically cannot absorb the authority to touch the world.

The runtime's evolutionary endpoint follows: judgment is absorbed by the base model or moves up into the workspace — becoming strategy text written in natural language, loaded each turn, executable by the model, and reloadable wholesale across base-model changes — while enforcement persists. The death of orchestration frameworks (development approaches that hard-code task flow in graphs or config files, e.g. Dify, LangGraph) is the same mechanism's precedent: Dify encoded nothing but judgment, and the base model absorbed it natively. WAIC 2026's structure data is its epitaph — of 130 participating entities, 83 built agent applications and only 18 were foundation-model companies [16]. The industry move is already visible: OpenAI's harness practice shrinks the human engineer's role to "design the environment, specify intent, build feedback loops" [24] — judgment moved up into workspace configuration, the runtime retired to a thin execution layer. The surviving runtime converges toward an operating-system kernel: low-frequency, replaceable infrastructure.

### 3.2 Corollary 2: Distillation Is a Conditional Branch

The author's original judgment:

> **"RSI's real value should be using a large model, grounded in your own repository, to train a small language model — both native and personal."** (author's view)

Three lines of evidence support the route. First, it works: SEED summarizes trajectories into hindsight skills and distills them back into policy, beating GRPO by 45.9 points [1]; Agent0 self-evolves at 8B scale with zero external data [9]. Second, the economics hold: distillation is a standard product step at frontier labs [21], and training cost keeps falling with base-model generations. Third — the decisive point — scale fit: individual and organizational repositories typically hold thousands to tens of thousands of samples, and DeepSeek-R1 shows reasoning transferring from a flagship into 1.5B–70B students that beat comparators several times their size [22]. **Here is the quantitative basis for "native and personal."**

Three constraints make it a conditional branch, not a default route. Source constraint (Regularities 3): training data drawn wholly from self-produced output triggers collapse [11]; the loop must contain real usage signals from outside the repository — DeepSeek-R1's distillation data likewise includes real long-chain reasoning corpora, not purely self-produced data [22]. Depreciation constraint (Regularity 4): base models strengthen each generation while inference cost falls 10–100x every 6–18 months, so the advantage window of a distilled artifact shrinks each generation, while evaluation, retraining, and lineage management form a standing liability. Solidification constraint: distillation freezes the system's current judgment into weights, and long reasoning chains are precisely where distillation is weakest [21,22]; while judgment is still evolving fast, editing one line of strategy text is a diff while updating weights is a retrain — premature solidification is a liability, not an asset.

**Proposition 2 (Distillation threshold).** Only when the target path satisfies four conditions simultaneously — high call frequency, stable distribution, automatically verifiable outputs, and sustained superiority over "latest base model plus workspace" on the holdout — does distillation constitute a positive asset; if any condition fails, revert. The four conditions are conjunctive; when call frequency and holdout performance conflict, the holdout prevails — frequency only bounds distillation's upside, the holdout decides whether it is a real asset. In this framework, fixed benchmarks are adjudication's public rental — the only holdout implementation individuals and small organizations can afford. What should be opposed is chasing rankings, not keeping the exam.

### 3.3 Corollary 3: Asset Hierarchy and the Preservation Law

From Definitions 1 and 2 and Regularity 2, the survival time of the three implementations follows directly: the model is a function — it ends with each call, gets cheaper each generation, and is replaceable at will; the runtime is a process — rebuilt with each distribution iteration, but with unchanged duties; the workspace is the file-system persistence layer — the only structure surviving across generations. This is the definitions' direct physical corollary. The asset hierarchy — model below runtime, runtime below workspace — is a normative judgment drawn from survival time: preservation effort should go to the layer that survives longest. This step is a design principle, not an empirical proposition, and is labeled as such. A distilled artifact is the output of one compilation; it depreciates when the compiler upgrades, while losing the workspace zeroes out everything accumulated across generations. **The means of preservation are therefore exactly three: backup, lineage records, evaluation-set maintenance.** They are not engineering habits but the only preservation operations available under this asset structure. Both labs' practice prices evaluation assets directly: Anthropic's long-horizon scheme takes "build evaluation first, then develop" as its first principle [23], and OpenAI turns repository-quality checks into an automated pipeline [24] — the return on evaluation-side assets exceeds any increment on the generation side.

### 3.4 Corollary 4: Handover Is Ordered by Acceptance Executability

From Regularities 1 and 4: the depth to which a business process is handed to AI is determined by the executability of its acceptance criteria. Processes with automatic verification — compilation, testing, evaluation — are handed over first; where acceptance requires human judgment, handover stalls at the acceptance step, because non-executable acceptance degrades "humans write requirements" into "humans make wishes." Two independent lines of evidence sandwich the conclusion. From the front, handover depth is driven directly by the expansion of verifiable environments: after WorkForge spread verifiable environments across 40 domains, GDPVal rose from 45.5 to 73.6 [20]. From the rear, executable acceptance is the very precondition of the zero-human-code project: in OpenAI's case, humans never write code, but humans always translate user feedback into acceptance criteria and verify results [24] — acceptance is precisely the end humans did not hand over.

The author's endgame judgment, preserved verbatim:

> **"Humans will keep working; in their own business domains there will still be process after process — only handed to AI. How? Humans write requirements, models execute; humans write requirements, models distill; humans start producing AI through AI."** (author's view)

**Proposition 3 (The human position).** Humans converge to the two ends of the chain — specification (requirements, constraints, goals) on one end, acceptance adjudication (design authority and interpretive authority over the evaluation set) on the other; the generation end is handed over entirely. OpenAI's "humans steer, agents execute" [24] is the industry-grade formulation of this proposition. Collection judgment for the external knowledge source belongs to the judgment end and remains a human-side function.

## 4 Predictions and Limitations

Eight testable predictions follow, over an observation window ending in late 2028, each with an operational acceptance criterion — scored by the criterion at expiry, not by impression.

| # | Prediction | Tests | Acceptance criterion |
|---|---|---|---|
| P1 | Context-loading strategy moves from runtime configuration to model-driven retrieval within two base-model generations | Corollary 1 | "Handed over" if flagship runtimes' default path no longer depends on manually configured retrieval rules, with the model autonomously deciding what and when to retrieve, and this default becomes the officially recommended configuration |
| P2 | Cross-runtime benchmark spread (currently 48 points, grade-B source [14]) narrows across base-model generations | Corollary 1 | Measured on public, reproducible cross-runtime measurements of the same base model released in the same year (same eval set, same tasks); "narrowing" if two or more independent sources show less than half the current spread. Evidence status: only one grade-B source exists; the test depends on continued public measurement |
| P3 | Enforcement does not die with base-model iteration; the surviving runtime converges toward a kernel | Corollary 1 | "Persists" if mainstream runtimes keep sandbox, permission, and credential layers backward-compatible across major base-model upgrades, with no official "natively replaced by the model" scheme |
| P4 | Graph/config-coded orchestration frameworks marginalize within 12–24 months | Corollary 1 | "Marginalized" if an orchestration vendor officially pivots (product-line shrinkage, pivot to runtime, or acquisition), or public data shows sustained decline in GitHub activity and new production deployments |
| P5 | Distillation degrades from default route to condition-triggered action per Proposition 2 | Corollary 2 | "Degraded" if mainstream agent products and frameworks list distillation as an optional path triggered by explicit conditions (including a "re-run holdout on new base-model release" trigger) rather than a default configuration |
| P6 | Public self-improvement evidence remains concentrated in strongly verifiable domains; L5 has no reproducible public implementation within the window | Regularity 1 | L5 counts as achieved only with end-to-end autonomous successor-base-model training reproducible by third parties on public code and data; otherwise "not crossed." Evaluator independence is judged by the emergence of a public evaluation benchmark independent of the candidate generator |
| P7 | Handover depth decreases with acceptance executability; evaluation-set design and acceptance adjudication become the main human-side job forms | Corollary 4 | "Decreasing" holds if hiring volume for acceptance-automatable process roles (e.g., testing, QA) declines significantly more than for judgment-dependent process roles; the job-form claim holds if "evaluation-set design" appears in mainstream technical job requirements |
| P8 | Workspace operations (backup, lineage, evaluation-set maintenance) become a distinct product category and the primary operations item | Corollary 3 | "Product category" holds if standalone tools or cloud services emerge whose core selling point is workspace backup, lineage management, or evaluation-set maintenance, forming an identifiable market category |

Three boundary conditions remain unsolved. First, loop dynamics have no storage location: error compounding and state drift exist only between adjacent turns — a 97% single-turn success rate decays to 54% over 20 turns [14] — and a layered static structure cannot directly express quantities that decay with turn count; a possible direction is promoting turn trajectories and decay metrics to first-class workspace structures. Second, cross-repository migration has no mechanism: closed scope is the precondition of cheap adjudication and simultaneously the ceiling of migration; the sharing layer between repositories and its version consistency are undefined in this framework. Third, the loading path of the external knowledge source and the sedimentation of its collection criteria likewise await formalization. These three are incomplete parts of the framework, not counterexamples.

## Conclusion

The framework's three definitions, four corollaries, and eight predictions compress into one overall judgment: **the model will not devour everything, but it will replace more and more non-physical components.** Two precisifications are required before the sentence is fully consistent with the framework.

First, "non-physical" is not the dividing line itself but its current shadow. The operative replacement criterion is Corollary 4's acceptance executability: every acceptance-executable information transformation, physical or not, is on the replacement list. Code, orchestration, and process judgment are non-physical and being replaced; driving is physical and on the list too — physical components are not exempt from replacement, only slower, because their verification carries hardware cost in sensors and actuators. Conversely, original judgment and strategic taste are non-physical yet hardest to replace, because acceptance is not executable. The dividing variable is verifiability, not physicality.

Second, what survives is not "physical components" in general but three kinds of structure. First, the authorization boundary: sandboxes, permissions, and credentials are social facts, not capabilities — a model physically cannot absorb the authority to touch the world (Corollary 1's enforcement functions). Second, the persistence layer: the workspace is not a component awaiting replacement but the source itself; only components can be replaced, source is what gets compiled. Third, the world term: the external knowledge source is depended upon, not replaced (Definition 3).

The final picture: the replacement boundary advances along the acceptance-executability axis as base models iterate; what the model devours is information transformation; what remains is permission, source, and the world — and the human at both ends of the chain.

---

**Source grading**: [1]–[13], [15], [17]–[27], [29]–[32] are peer-reviewed papers, official technical reports, official courses, or original code repositories — grade A (the eight 2026 arXiv IDs were individually verified against their abstract pages: 2607.07663, 2606.26294, 2511.16043, 2510.21614, 2610.04906, 2510.04618, 2506.06266, 2607.27191); [14] (Cursor measurement, secondary citation) and [16] (conference report) are industry sources — grades B/C, flagged at each point of use, and never the sole support of any independent conclusion (P2 is the single exception, with its evidence status stated in the acceptance criterion). Industry figures are quoted as published, without cross-source comparability claims.

---

## References

**Papers and official reports (Grade A)**

[1] Recursive Self-Improvement in AI: From Bounded Self-Refinement to Autonomous Research Loops (survey, ~1,250 arXiv papers 2024–2026). 2026. https://arxiv.org/abs/2607.07663
[2] Madaan et al. Self-Refine: Iterative Refinement with Self-Feedback. NeurIPS 2023. https://arxiv.org/abs/2303.17651
[3] Shinn et al. Reflexion: Language Agents with Verbal Reinforcement Learning. NeurIPS 2023. https://arxiv.org/abs/2303.11366
[4] Zelikman et al. Self-Taught Optimizer (STOP): Recursively Self-Improving Code Generation. COLM 2024. https://arxiv.org/abs/2310.02304
[5] Hu, Lu, Clune. Automated Design of Agentic Systems (ADAS). ICLR 2025. https://arxiv.org/abs/2408.08435
[6] Zhang et al. Darwin Gödel Machine: Open-Ended Evolution of Self-Improving Agents. ICLR 2026. https://arxiv.org/abs/2505.22954
[7] Novikov et al. AlphaEvolve: A coding agent for scientific and algorithmic discovery. DeepMind, 2025. https://arxiv.org/abs/2506.13131
[8] Wang et al. Huxley-Gödel Machine: Human-Level Coding Agent Development by an Approximation of the Optimal Self-Improving Machine. ICLR 2026. https://arxiv.org/abs/2510.21614
[9] Xia et al. Agent0: Unleashing Self-Evolving Agents from Zero Data via Tool-Integrated Reasoning. 2025. https://arxiv.org/abs/2511.16043
[10] Zhang et al. Agentic Context Engineering (ACE). 2025. https://arxiv.org/abs/2510.04618
[11] Shumailov et al. AI models collapse when trained on recursively generated data. Nature, 2024. https://www.nature.com/articles/s41586-024-07566-y
[12] Iacob et al. The Red Queen Gödel Machine: Co-Evolving Agents and Their Evaluators. 2026. https://arxiv.org/abs/2606.26294
[13] Silver, Sutton. Welcome to the Era of Experience. In Designing an Intelligence, MIT Press, 2025. https://storage.googleapis.com/deepmind-media/Era-of-Experience%20/The%20Era%20of%20Experience%20Paper.pdf
[15] Kirgis et al. Can AI agents conduct open-ended AI research? Early evidence from two case studies. 2026. https://arxiv.org/abs/2607.27191
[17] Stanford CS329A: Self-Improving AI Agents (course syllabus). https://cs329a.stanford.edu/
[18] Kwa et al. Measuring AI Ability to Complete Long Tasks. METR, 2025. https://metr.org/blog/2025-03-19-measuring-ai-ability-to-complete-long-tasks/ (latest data at https://metr.org)
[19] International AI Safety Report 2026. https://internationalaisafetyreport.org/
[20] Zhang et al. Scaling Verifiable Environments for Long-horizon Work Agents (WorkForge). 2026. https://arxiv.org/abs/2610.04906
[21] Hinton, Vinyals, Dean. Distilling the Knowledge in a Neural Network. NIPS Deep Learning Workshop, 2015. https://arxiv.org/abs/1503.02531
[22] DeepSeek-AI. DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning. 2025. https://arxiv.org/abs/2501.12948 (R1-Distill-Qwen/Llama, 1.5B to 70B)
[25] Packer et al. MemGPT: Towards LLMs as Operating Systems. 2023. https://arxiv.org/abs/2310.08560
[26] Cartridges: Lightweight and general-purpose long context representations via self-study. 2025. https://arxiv.org/abs/2506.06266

**Engineering blogs and official practice (Grade A)**

[23] Anthropic. Effective harnesses for long-running agents. 2025-11-26. https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
[24] OpenAI. Harness engineering: leveraging Codex in an agent-first world. 2026. https://openai.com/index/harness-engineering/
[27] Anthropic. Effective context engineering for AI agents. 2025-09-29. https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
(Related: Anthropic. Harness design for long-running application development, 2026-03-24; Building a C compiler with a team of parallel Claudes, 2026-02-05. Both at https://www.anthropic.com/engineering)

**Code repositories (Grade A)**

[28] OpenEvolve. https://github.com/algorithmicsuperintelligence/openevolve
[29] Darwin Gödel Machine (official implementation). https://github.com/jennyzzt/dgm
[30] Reflexion. https://github.com/noahshinn/reflexion
[31] awesome-rsi (RSI research index). https://github.com/lobehub/awesome-rsi
[32] METR evaluation analysis code. https://github.com/METR/eval-analysis-public

**Industry sources (Grades B/C, side evidence only)**

[14] Best LLM for AI Agents ranking (incl. Cursor 46%/80% cross-runtime measurement and error-compounding calculation). o-mega.ai, 2026-09. https://o-mega.ai/articles/best-llm-for-ai-agents-september-2026-ranking
[16] 2026 CSDI conference report (incl. WAIC 2026 participant-structure data). AIBase. https://www.aibase.com/news/30569
