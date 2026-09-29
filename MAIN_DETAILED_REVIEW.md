# Detailed Scientific and Editorial Review of `main.tex`

**Scope.** This review does **not** modify `main.tex`. It audits the current manuscript for narrative coherence, scientific support, statistical interpretation, LaTeX consistency, and bibliography coverage. Suggested text is illustrative and should be adopted only after checking against code and raw experimental outputs.

## Priority map

- **P0 - Must fix before submission:** correctness, unsupported causal/statistical claims, broken references, experiment-description inconsistencies.
- **P1 - Strongly recommended:** narrative clarity, stronger methodological framing, missing citations central to the paper.
- **P2 - Optional strengthening:** broader context, secondary citations, stylistic polish.

---

# 1. Global assessment

The paper has a clear potentially publishable core: **a controlled architectural study of a classical RL portfolio policy in which a single latent block is removed, implemented classically, or implemented as a PQC**. The manuscript is strongest when it stays close to that question. It becomes weaker when it tries to infer a quantum mechanism from descriptive performance differences or when it treats non-rejection of a statistical test as equivalence.

The most important editorial principle for the next revision should be:

> **Observed result -> statistical evidence -> interpretation -> mechanism hypothesis**

Do not collapse these four levels into one sentence.

The manuscript should avoid presenting this as a quantum-advantage study. The current conservative wording in the Introduction and Limitations is appropriate and should be propagated consistently into Results, captions, and architecture descriptions.

---

# 2. Abstract

## P0 - Kruskal-Wallis wording

**Current idea:** the 2-, 4-, and 8-qubit distributions are “not separated” / “indistinguishable”.

**Issue:** a non-significant Kruskal-Wallis result does not establish equivalence. It only indicates that the omnibus test did not reject the null under the tested sample.

**Recommended wording:**

> “For the quantum register-width sweep, the Kruskal-Wallis test did not detect a difference among the final-value distributions for 2, 4, and 8 qubits ($H(2)=2.13$, $p=0.345$).”

If equivalence across widths is an intended result, use an **equivalence/non-inferiority design** or a pre-specified tolerance, not a standard null-hypothesis significance test.

## P0 - Verify the reported statistics

The values $H(2)=2.13$, $p=0.345$, and $arepsilon^2=0.002$ were inherited from the original manuscript. Before submission, recompute them from the raw 20-run outputs. The same applies to the EI3 result $H(2)=7.66$, $p=0.022$, $arepsilon^2=0.099$.

## P1 - Avoid “substantially” unless quantified

“Substantially smaller run-to-run dispersion” is defensible descriptively because the reported CVs differ strongly, but a more neutral abstract would say:

> “the tested quantum configurations show lower run-to-run dispersion than the tested classical configurations.”

This avoids implying an inferential conclusion before the between-model test is reported.

---

# 3. Introduction

The current Introduction is substantially improved and should remain literature-aware, but it can be made more efficient.

## P1 - Reduce duplication with Background

The Introduction already covers QML, quantum finance, QRL, RL portfolio management, benchmarking, and the research question. The Background then repeats several of these topics. The two sections should have different jobs:

- **Introduction:** field -> gap -> research question -> contribution.
- **Background/Related Work:** closest prior methods and concepts needed to understand the experiment.

Avoid re-explaining general QML limitations in both places.

## P1 - Add RL experimental-design references

The statement that RL outcomes vary across random initializations is central to the entire paper but is currently insufficiently anchored in the general RL methodology literature.

**Recommended sentence:**

> “Because deep-RL results can be sensitive to random seeds, implementation details, and evaluation protocol, comparisons based on a small number of runs can substantially misrepresent algorithmic differences~\cite{Henderson2018DRLMatters,Agarwal2021StatisticalPrecipice,Patterson2024EmpiricalDesign}.”

These references directly support the repeated-run philosophy of the manuscript.

## P1 - Add the closest QRL portfolio paper

The recent preprint by **Gurgul, Chen, and Lessmann (2026)** is directly relevant because it applies variational QRL to dynamic portfolio optimization. It should be acknowledged in the Introduction or Related Work as a close contemporary study, with its preprint status made explicit.

Suggested positioning:

> “A recent preprint has also investigated variational QRL for dynamic portfolio optimization using quantum analogues of DDPG and DQN, reporting competitive risk-adjusted performance together with practical deployment limitations~\cite{Gurgul2026QRLPortfolio}. Our study differs by isolating a single interchangeable latent block within a fixed EI3-derived policy.”

Do not reproduce their claim of “parameter efficiency” as an established fact without explaining the architectural comparison.

## P1 - Add critical DRL portfolio evidence

The manuscript should not cite only positive DRL portfolio results. **Kruthof and Müller (2025)** provide directly relevant negative evidence: SAC did not systematically beat $1/N$ across seven datasets and suffered under transaction costs.

Suggested use:

> “Recent large-scale evaluations also show that strong DRL portfolio results can be sensitive to dataset choice, turnover, and transaction costs, reinforcing the need for conservative validation~\cite{Kruthof2025CanDRLBeat1N}.”

This strengthens the paper rather than undermining it.

---

# 4. Background and Related Work

## P0 - The QRL subsection is currently commented out

The source contains a commented `Quantum and hybrid reinforcement learning` subsection. As a result, the active Background jumps from PQCs directly to Experimental Testbed.

This is a structural problem because QRL is the methodological bridge between the PQC description and the proposed architecture.

**Recommendation:** restore a concise active QRL subsection, but keep it focused on papers directly relevant to policy/value approximation and benchmarking.

Suggested structure:

1. Chen et al. (VQC in deep RL).
2. Jerbi et al. (parameterized quantum policies).
3. Skolik et al. (variational quantum DQN).
4. Sequeira et al. (policy gradients).
5. Meyer et al. / Kruse et al. (benchmarking and limitations).
6. Gurgul et al. (dynamic portfolio QRL - preprint).

## P0 - Remove source clutter after deciding final text

The Background contains long commented blocks of old Introduction/Related Work text. They do not affect the PDF but make source maintenance difficult and increase the risk of reintroducing outdated claims. Move them to version control history instead of keeping them in the manuscript.

## P0 - Resolve the `n` versus `Q` notation

The active PQC subsection contains an author note:

> `[Use n for the qubit dimension]`

while the architecture and results predominantly use $Q$.

Choose one convention. I recommend:

- $Q$ = latent width / number of qubits in the quantum instantiation.
- $d$ or $n_{\mathrm{lat}}$ if a neutral dimension is needed for both classical and quantum blocks.

Do **not** overload $n$ because $n$ is also commonly used for number of runs/samples in the Results.

## P1 - PQC references: enough for the narrative, not a mini-review

The active PQC subsection only needs citations that support:
- PQCs as trainable hybrid models;
- variational algorithms/PQCs;
- optionally data encoding if you discuss why $R_Y$ encoding matters.

Current Benedetti + Cerezo is enough for the basic definition. Add a trainability review only if trainability is explicitly discussed.

**Optional citation:** Larocca et al. (2025) review of barren plateaus. Use only if the manuscript discusses trainability; otherwise it creates a new narrative obligation.

## P1 - Add current portfolio-RL reviews

For the first Background subsection, consider citing:
- Rezaei & Nezamabadi-Pour (2025) - experimental/taxonomic review of DRL portfolio management.
- Rout et al. (2026) - systematic review of RL for automated equity portfolio management.

These help establish that evaluation heterogeneity and reproducibility remain open issues.

---

# 5. Experimental testbed

## P0 - “Test set” is not strictly held out

The manuscript says:
- the 2020 environment is used for periodic validation;
- online learning is enabled during the evaluation episode.

This makes “strict out-of-sample test set” potentially misleading.

**Recommended terminology throughout:** **2020 evaluation window**.

Suggested rewrite:

> “Training uses 2017-2019 data and evaluation uses the 2020 market path. Because the evaluation environment is also used for periodic validation and online updates are enabled during evaluation, we use the neutral term ‘evaluation window’ rather than ‘held-out test set’.”

## P0 - “20 times independently” is too strong given the PRNG description

The manuscript states that runs are independent while also stating that they draw sequentially from a single pseudorandom stream.

Suggested rewrite:

> “Each campaign contains 20 training runs. A pseudorandom stream is initialized once at the start of the campaign, and successive runs consume different portions of that stream.”

Then state explicitly whether there are additional stochastic sources (action noise, minibatch sampling, environment stochasticity, optimizer randomness).

## P0 - Reproducibility needs qualification

“the batch as a whole is reproducible by re-executing the procedure” is only safe if the software stack and kernels are deterministic.

Suggested rewrite:

> “The fixed campaign seed is intended to make the initialization sequence reproducible within the reported software configuration.”

If bitwise reproducibility was actually tested, document software versions and hardware.

## P0 - CV is descriptive, not automatically “appropriate”

Current wording:

> “the coefficient of variation, which is the appropriate scale-free measure here...”

This is too categorical.

Suggested wording:

> “We additionally report the coefficient of variation (CV) as a scale-normalized descriptive measure of run-to-run dispersion because mean final values differ across configurations.”

Do not use CV alone as evidence that one algorithm is statistically “more stable.”

## P0 - “Differences are therefore attributable to architecture” is causal

Identical hyperparameters reduce procedural confounding but do not prove causality, especially when:
- seeds are not paired;
- parameter counts differ;
- stochastic streams are consumed differently.

Suggested rewrite:

> “The unified protocol reduces procedural differences across model variants and supports a more controlled architectural comparison.”

## P0 - “Best known” / “strongest available EI3” is unsupported

Unless a systematic hyperparameter search was conducted, write:

> “Among the EI3 training configurations evaluated in this study, the 120-episode / batch-200 setting attains the highest observed mean final value.”

## P0 - Seed depends on qubit width

The manuscript correctly recognizes the confound but then says it “strengthens a claim of stability.” That inference is not justified.

Recommended text:

> “Because the pseudorandom stream changes with $Q$, register width and initialization stream are confounded. The width sweep therefore describes the three tested campaigns but does not isolate a causal effect of register width.”

If width invariance is important, rerun every $Q$ using the same set of campaign seeds.

## P1 - Cite general RL experiment methodology here

This subsection is the ideal place to cite Henderson (2018), Agarwal (2021), and Patterson (2024). These references justify repeated runs, uncertainty reporting, careful baseline construction, and hypothesis-test discipline.

---

# 6. From EI3 to a hybridisable architecture

## P0 - “Each step changes exactly one thing” is too strong

The transition from EI3 to the scaffold changes multiple elements: latent projection, scaling, decision head, and residual design.

Suggested opening:

> “The evaluated architectures form a sequence of controlled modifications intended to expose the contribution of the latent block while keeping the surrounding policy structure as similar as practicable.”

## P0 - Duplicate citation

`\cite{RLportfolio,RLportfolio}` should be `\cite{RLportfolio}`.

## P0 - Invalid multi-reference syntax

`\ref{fig:circuit-q2,fig:circuit-q8}` is not valid LaTeX cross-reference syntax.

Use:

> `Figs.~\ref{fig:circuit-q2} and~\ref{fig:circuit-q8}`

## P0 - Undefined `sec:discussion`

The text says:

> “we return to it in \ref{sec:discussion}”

but there is no Discussion section with that label.

Either:
- add a Discussion section, or
- refer to `sec:limitations`, or
- remove the forward reference.

## P0 - Residual-branch claims require evidence

Current claims include:
- residual is “standard practice for trainability”;
- the model can reach good performance while ignoring the block;
- removing the residual makes the ablation “identifiable”;
- optimization becomes harder.

Some are plausible, but they are not empirically established here.

Suggested rewrite:

> “We remove the explicit bypass so that the decision head must consume the output of the latent block. This is a design choice intended to make the block operationally relevant to the downstream policy. We do not claim that this design is optimal for trainability.”

If the team observed training failure with the residual, report the ablation or leave it as an implementation observation.

## P0 - No-Layer caption makes a causal claim

Current:

> “the difference in behaviour ... is attributable to the latent block alone.”

Replace with:

> “This configuration provides the closest architectural reference for assessing the addition of the latent block.”

## P0 - “Essentially the same capacity” is not supported by parameter count

Similar parameter count does not imply similar function-class capacity.

Use:

> “The two variants have similar total parameter counts in the illustrated two-wide configuration.”

## P0 - Classical-block description appears inconsistent

Body:

> “a dense layer of width $Q$ with $\tanh$ activation.”

Caption:

> “$\tanh$, a linear map and $\tanh$.”

The parameter count 72 at $Q=8$ corresponds to an $8\times8$ affine transformation (64 weights + 8 biases), but the exact placement of activations must be checked against code.

**Action:** verify the implementation and make body, figure, and caption identical.

## P0 - Architecture-diagram dimensionality note is unresolved

The source contains:

> “diagramas ... diferença na dimensionalidade ... gerado com base nos qubits”

This must be resolved before submission. A classical layer should not be described using “qubits.”

Use a neutral label such as **latent width $Q$** in the classical diagram and **qubits $Q$** only in the quantum diagram.

## P1 - Parameter-count asymmetry is not a result by itself

At $Q=8$, 16 circuit parameters versus 72 classical-block parameters is an architectural fact. It does not establish parameter efficiency.

Suggested text:

> “The quantum and classical latent blocks have different parameter counts under the chosen constructions; this asymmetry should be considered when interpreting the empirical comparison.”

---

# 7. Implementation and simulation regime

## P0 - Undefined `tab:transpiled`

The caption references `\ref{tab:transpiled}`, but no such label exists.

Remove the reference or add the table.

## P1 - Statevector wording

“allows the architectural comparison to be made without introducing hardware effects” is reasonable. Avoid stronger claims such as “upper bound” or “isolates only initialization variance.”

Exact expectation values remove finite-measurement sampling from the quantum readout, but training may still contain other stochasticity.

Suggested wording:

> “Exact statevector expectation values remove finite-measurement sampling from the quantum readout; the reported run-to-run variation may still reflect other stochastic components of the training pipeline.”

## P1 - Specify versions

For reproducibility, report versions of:
- Qiskit;
- Qiskit Machine Learning;
- PyTorch;
- Python;
- RLPortfolio commit/tag;
- optimizer configuration;
- simulator/backend.

---

# 8. Results

## P0 - Opening sentence claims causal isolation

Current:

> “Each step isolates one factor.”

Replace with:

> “The sequence is designed to make the architectural changes explicit; not every comparison constitutes a causal isolation because some campaigns use different pseudorandom streams and model parameterizations.”

## P0 - Baseline Kruskal-Wallis interpretation

$H(2)=7.66$, $p=0.022$ is an omnibus result. It shows evidence that at least one distribution differs, not which training schedule is statistically different from another.

Do not use it alone to justify “the strongest configuration.”

Suggested text:

> “The omnibus Kruskal-Wallis test detects a difference among the three final-value distributions ($H(2)=7.66$, $p=0.022$). The 120-episode / batch-200 setting has the highest observed mean among the evaluated configurations.”

If pairwise claims are important, perform corrected post-hoc comparisons.

## P0 - Dispersion does not monotonically “grow with budget”

Reported SDs: 31,608 -> 28,132 -> 55,409.

This is not monotonic. Write:

> “The highest-budget configuration also has the largest observed standard deviation.”

## P1 - “A single run is a weak measurement”

The metaphor is unnecessary and can confuse statistical readers.

Suggested:

> “The magnitude of run-to-run variability motivates reporting distributions across repeated training runs rather than relying on a single trajectory.”

## P0 - Scaffold “raises the level” is causal wording

Use:

> “The scaffold has a higher observed mean final value than the evaluated EI3 configurations.”

Do not infer the restructuring caused the increase without a controlled paired design.

## P0 - “Same trajectory shape / scaled common policy” is unsupported

Visual similarity of portfolio-value curves does not prove that learned policies are the same up to scaling.

To make this claim, quantify:
- correlation/distance between allocation trajectories;
- action-vector similarity;
- turnover similarity;
- policy-output similarity on a shared state set.

Otherwise use:

> “The trajectories visually share market-driven inflection points while differing in final level.”

## P0 - Classical No-Layer vs 8-neuron statement lacks reported test

Current:

> “the final-value distributions are not distinguishable.”

The manuscript does not report the corresponding test.

Either add the test/effect size/CI, or write:

> “The observed mean and CV change from ... to ..., but the present manuscript reports these differences descriptively.”

## P0 - Register-width: p > 0.05 is not equivalence

Replace “indistinguishable” with:

> “The Kruskal-Wallis test does not detect a difference among the three final-value distributions at the chosen significance level.”

## P0 - Do not say width “explains” variance

`\varepsilon^2=0.002` from an omnibus rank test is not enough to make a causal variance-explanation statement, especially with seed-width confounding.

Use:

> “The reported effect-size estimate is small for these three campaigns.”

## P0 - Seed confound does not strengthen stability

Delete this interpretation. Different streams do not provide a controlled robustness test when they are perfectly confounded with $Q$.

## P0 - “Dispersion due to initialization alone” is too strong

Even with exact expectation values, run-to-run variability may include:
- action-noise draws;
- minibatch order/sampling;
- optimizer stochasticity;
- online-learning dynamics;
- other PRNG-dependent operations.

Use **training stochasticity** unless the code confirms initialization is the only changing source.

## P0 - Matched-width comparison needs inferential statistics

The central numerical observation is:

- classical: mean 301,294, CV 0.217;
- quantum $Q=8$: mean 369,224, CV 0.030.

This deserves a pre-specified statistical comparison.

Recommended additions:
- bootstrap CI for difference in means/medians;
- robust effect size;
- a test appropriate to the independent campaigns;
- a separate dispersion comparison if stability is a key claim.

Do not use parameter count as evidence of efficiency without a performance-vs-resource experiment.

## P0 - `eq:seed` is undefined

The summary-table caption uses `\eqref{eq:seed}`; the actual label is `eq:seed-rule`.

## P0 - Multiple labels inside one `\ref{}`

The manuscript uses constructs such as:

- `\ref{subsec:res-scaffold,subsec:res-classical-width}`

These are not valid standard LaTeX references. Write the references separately.

## P0 - BenchmarkClassical figure is under-specified

The figure includes EI3, EIIE, “reference benchmark,” and the proposed classical model, but the Methods do not fully define:
- training protocol for every curve;
- run count for every curve;
- whether hyperparameters are comparable;
- whether the same evaluation/online-learning settings were used.

Unless this is documented, remove the figure or move it to supplementary material.

---

# 9. Captions

Captions should report **what is plotted and what is directly measured**, not provide the strongest interpretation of the figure.

Priority fixes:

1. Replace “distributions are indistinguishable” with “the test did not detect a difference.”
2. Remove “dispersion ... is due to initialization alone.”
3. Remove causal wording such as “effect of the layer” unless the comparison supports it.
4. Use “evaluation window” consistently.
5. State logical circuit gate counts as logical counts, not end-to-end cost.
6. Ensure every statistical number in a caption appears identically in the Results/table.

---

# 10. Limitations and missing Discussion/Conclusion

## P1 - Add a Discussion section

The manuscript currently moves from Results directly to Limitations. A short Discussion would improve the scientific narrative.

Suggested Discussion structure:

1. What is directly observed.
2. What cannot be inferred causally.
3. Relation to QRL benchmarking literature.
4. Relation to Gurgul et al. dynamic-portfolio QRL.
5. Why lower dispersion is scientifically interesting but mechanistically unresolved.
6. Experiments needed to discriminate explanations.

Do **not** speculate that Fourier spectra, bounded outputs, entanglement, or “quantum regularization” explain the effect unless an experiment tests those mechanisms.

## P1 - Add a concise Conclusion

There is no explicit Conclusion section.

Suggested content:
- restate the narrow research question;
- summarize only demonstrated empirical observations;
- state the main limitation;
- name the next experiment: matched seeds + multiple market windows + inferential comparison.

---

# 11. LaTeX/source consistency audit - P0

The current source contains several issues that should be fixed before submission:

- `\cite{RLportfolio,RLportfolio}` - duplicate key.
- `\ref{fig:circuit-q2,fig:circuit-q8}` - invalid multi-label reference.
- `\ref{subsec:res-scaffold,subsec:res-classical-width}` - invalid multi-label reference.
- `\ref{sec:discussion}` - undefined.
- `\ref{tab:transpiled}` - undefined.
- `\eqref{eq:seed}` - undefined; likely `eq:seed-rule`.
- active `\sam{...}` author notes remain in the Background.
- unresolved TODOs remain in architecture diagrams and benchmark figure.
- old commented versions of multiple sections remain embedded in the source and should be removed after final decisions.

These are not only cosmetic: undefined references can appear as “??” in the compiled manuscript and signal incomplete editorial control to reviewers.

---

# 12. Recommended references, ordered by priority

## P0 - Essential / directly strengthens the current paper

### 1. Meyer et al. (2025) - Benchmarking Quantum Reinforcement Learning
**Status:** already in `ref.bib` and already cited.  
**Why essential:** directly supports caution about QRL outperformance, sample complexity, and statistical validation.  
**Best location:** Introduction, QRL Related Work, Discussion.

### 2. Henderson et al. (2018) - Deep Reinforcement Learning That Matters
**Status:** **NEW - add to `ref.bib` if adopted.**  
**Why essential:** foundational reference on seed sensitivity, reproducibility, significance metrics, and standardized RL reporting.  
**Best location:** Introduction paragraph on run-to-run variability; Experimental Protocol.  
**Verification:** AAAI DOI 10.1609/aaai.v32i1.11694. VERIFIED-2.

### 3. Patterson et al. (2024) - Empirical Design in Reinforcement Learning
**Status:** **NEW.**  
**Why essential:** comprehensive modern reference on performance variation, stability, hypothesis testing, multiple-agent comparisons, baselines, and hyperparameter bias.  
**Best location:** Training protocol/statistical treatment and Discussion.  
**Verification:** JMLR 25(318):1-63. VERIFIED-2.

### 4. Agarwal et al. (2021) - Deep Reinforcement Learning at the Edge of the Statistical Precipice
**Status:** **NEW.**  
**Why essential:** directly relevant to uncertainty under limited numbers of runs and reporting interval estimates instead of relying on point estimates.  
**Best location:** Training protocol/statistical treatment.  
**Verification:** NeurIPS 2021 canonical record + arXiv. VERIFIED-2.

### 5. Gurgul, Chen & Lessmann (2026) - Variational Quantum Circuit-Based Reinforcement Learning for Dynamic Portfolio Optimization
**Status:** already in `ref.bib`, currently underused. **Preprint.**  
**Why essential:** closest current study to the paper's application domain.  
**Best location:** QRL Related Work and Discussion.  
**Caution:** describe as a preprint and attribute its performance claims to the authors.

### 6. Kruthof & Müller (2025) - Can deep reinforcement learning beat 1/N?
**Status:** already in `ref.bib`.  
**Why essential:** strong negative/critical portfolio-DRL evidence; shows sensitivity to transaction costs and benchmark choice.  
**Best location:** Portfolio-RL Background and Discussion.  
**Verification:** Finance Research Letters 75, 106866; DOI 10.1016/j.frl.2025.106866.

## P1 - Strongly recommended

### 7. Rezaei & Nezamabadi-Pour (2025) - Taxonomy/review + experimental study of DRL portfolio management
**Status:** already in `ref.bib`.  
**Use:** frame heterogeneity of datasets, reward definitions, baselines, and evaluation protocols.

### 8. Kruse et al. (2025) - Benchmarking Quantum Reinforcement Learning
**Status:** already in `ref.bib`.  
**Use:** complementary QRL benchmarking perspective; supports caution about claims beyond artificial problem settings.

### 9. Rout et al. (2026) - Systematic review of RL for automated equity portfolio management
**Status:** **NEW.**  
**Use:** recent systematic map of portfolio-RL architectures and evaluation practices.  
**Caution:** use mainly as landscape/context; do not rely on its aggregated performance numbers as direct evidence for this dataset.  
**Verification:** Discover Computing 29, 431; DOI 10.1007/s10791-026-10336-1. VERIFIED-2.

### 10. Larocca et al. (2025) - Barren plateaus in variational quantum computing
**Status:** **NEW.**  
**Use only if** the paper discusses PQC trainability or ansatz design.  
**Why:** authoritative recent review; better than adding scattered barren-plateau citations if trainability is a secondary point.  
**Verification:** Nature Reviews Physics 7, 174-189; DOI 10.1038/s42254-025-00813-9. VERIFIED-1.

## P2 - Contextual / optional

### 11. Chiv, Nayyar & Kim (2026) - Survey of quantum computing approaches for portfolio optimization in FinTech
**Status:** **NEW; in press/corrected proof.**  
**Use:** only for broad quantum-portfolio context; not central to QRL.  
**Verification:** ICT Express; DOI 10.1016/j.icte.2026.07.025. VERIFIED-1.

### 12. Buonaiuto et al. (2023) - Portfolio optimization on real quantum devices
**Status:** already in `ref.bib`.  
**Use:** optional context when distinguishing statevector studies from hardware-oriented quantum portfolio optimization.

### 13. Thakkar et al. (2024) - Improved financial forecasting via QML
**Status:** already cited.  
**Use:** useful finance-QML context; keep concise because it is not an RL study.

### 14. Scursulim et al. (2026) - Multiclass portfolio optimization via VQE with Dicke-state ansatz
**Status:** already cited.  
**Use:** useful portfolio-quantum context; explicitly distinguish static/combinatorial optimization from sequential RL.

---

# 13. Suggested revision order

1. **Fix all P0 LaTeX references and unresolved notes.**
2. **Restore a concise active QRL Related Work subsection.**
3. **Rewrite Experimental Protocol to remove causal language and clarify seed structure.**
4. **Audit all statistical claims against raw run-level data.**
5. **Rewrite Results so non-significant tests are not described as equivalence.**
6. **Add Henderson, Agarwal, Patterson, and the closest QRL portfolio paper to the narrative.**
7. **Add a short Discussion and Conclusion.**
8. Only after these changes, polish captions and final prose.

---

# 14. Highest-value additional experiments

These are not required merely to improve writing, but they would materially strengthen the paper:

1. **Matched-seed design:** run classical and quantum models over the same set of campaign seeds.
2. **Width x seed factorial sweep:** evaluate every $Q\in\{2,4,8\}$ under the same seed set.
3. **Multiple market windows:** evaluate more than one realized market regime.
4. **Primary head-to-head inferential test:** predefine endpoint and effect size before comparing classical vs quantum.
5. **Policy-similarity analysis:** only if the manuscript wants to claim that trajectories represent similar policies.
6. **Residual ablation:** only if residual removal is used as an empirical argument rather than an architectural choice.

The current paper can remain publishable without all of these if its claims are kept appropriately descriptive. The first two would provide the largest gain in causal interpretability.
