# Markovian Anti-Fragility Proof-of-Concept

## Reproducible Network-Science and Agentic-AI Validation Notebook

This repository accompanies the study:

**Antifragile Markovian Transport Under Targeted Edge Damage: Validation on Real and Agentic-AI Graphs**

The main reproducibility artifact is the Google Colab notebook:

```text
markovian_anti_fragility_poc_FIXED.ipynb
```

The notebook contains the complete experimental workflow in **six executable code cells**. It preserves the original embedded notebook outputs while correcting the inferential treatment of the nested size-control fractions. When rerun, the notebook regenerates the empirical CSV files, LaTeX tables, intermediate PDF figures, final PNG figures, and the Exgentic AI-domain validation outputs.

The study uses **real observed datasets only** for the reported empirical results. No synthetic graph generator is used to create the reported network outcomes, and no empirical result is hardcoded.

---

# 1. Notebook Structure

The notebook contains six code cells that are intended to be run in order.

| Notebook cell | Purpose |
|---|---|
| Cell 1 | Main six-network SNAP experiment: download, provenance, graph construction, preferential damage, random damage, hitting time, LCC retention, size controls, primary statistics, figures, and data-integrity audit |
| Cell 2 | Confirmatory shortest-path global-efficiency benchmark and corrected trial-level size-control inference |
| Cell 3 | Descriptive structural-correlate analysis across the six SNAP networks |
| Cell 4 | Convert confirmatory benchmark PDF figures to PNG |
| Cell 5 | Collect the exact 16 main-article PNG figures into one directory |
| Cell 6 | Exgentic agent/tool domain-validation experiment, including its own real-data graph construction, controls, tables, figures, and audit |

The repository does **not** require six separate Python scripts. The notebook already contains all six stages.

---

# 2. Important Note About Preserved Notebook Outputs

The updated notebook intentionally preserves the outputs embedded in the uploaded notebook.

The executable code has been corrected so that the nested 10%, 20%, 30%, and 40% size-control conditions are first aggregated **within each independent trial** before Wilcoxon inference.

Therefore:

```text
Embedded outputs = preserved historical run
Executable code = corrected trial-level inference
```

To regenerate every CSV and table with the corrected inference, rerun the notebook cells in order in Google Colab.

The raw pair-level output files are still preserved and still generated. The corrected workflow adds trial-level summary files rather than deleting or replacing the pair-level data.

---

# 3. Google Drive Layout

The notebook uses:

```text
/content/drive/MyDrive/Markov_Antifragility_IEEE/
```

Primary six-network outputs:

```text
/content/drive/MyDrive/Markov_Antifragility_IEEE/results_real_only/
```

Primary and confirmatory publication figures:

```text
/content/drive/MyDrive/Markov_Antifragility_IEEE/article_figures/
```

Exgentic domain-validation outputs:

```text
/content/drive/MyDrive/Markov_Antifragility_IEEE/ai_domain_validation_exgentic/
```

Exgentic PNG figures:

```text
/content/drive/MyDrive/Markov_Antifragility_IEEE/
ai_domain_validation_exgentic/figures_png/
```

---

# 4. Core Experimental Question

The experiment asks whether progressive topology-aware edge damage can improve stochastic transport in a surviving connected network.

For an undirected edge

\[
e=(u,v),
\]

the preferential damage weight is

\[
w_{uv}=\deg(u)+\deg(v).
\]

Edges adjacent to high-degree endpoints therefore receive greater removal priority.

Each independent preferential trial uses an exponential-race ordering. The same ordering is progressively truncated at:

\[
f \in
\{0.05,0.10,0.15,0.20,0.25,0.30,0.35,0.40,0.45\}.
\]

The damage states within a trial are nested.

The corresponding matched-random trajectory removes the same number of edges at each damage level using a uniform random edge ordering.

---

# 5. Primary Transport Measure

The primary endpoint is average simple-random-walk hitting time.

For a connected undirected graph with \(n\) nodes, \(m\) edges, combinatorial Laplacian \(L\), and Moore-Penrose pseudoinverse \(L^+\),

\[
\overline{H}
=
\frac{2m}{n-1}\operatorname{tr}(L^+).
\]

Relative performance is

\[
R(f)=
\frac{\overline{H}_{f}}
{\overline{H}_{0}}.
\]

Interpretation:

```text
R(f) < 1.00  -> lower average hitting time after damage
R(f) < 0.98  -> at least 2% descriptive improvement
R(f) > 1.00  -> degraded average hitting time
```

The sustained anti-fragility score is

\[
A_{\mathrm{AF}}
=
\frac{1}{|\mathcal{F}|}
\sum_{f\in\mathcal{F}}
\max(0,1-R(f)).
\]

This prevents a single isolated minimum from being treated as sustained improvement.

---

# 6. Cell 1 — Main Six-Network Experiment

Cell 1 performs the complete primary experiment.

It mounts Google Drive, creates the output directories, downloads the six public real datasets, computes SHA-256 provenance information, constructs the analysis graphs, computes network descriptors, calculates baseline hitting time, generates preferential and random progressive damage trajectories, generates size-matched real-network controls, computes primary statistics, writes tables, saves figures, and performs a final data-integrity audit.

## 6.1 Primary SNAP datasets

The cell downloads these exact source files:

```text
https://snap.stanford.edu/data/ca-GrQc.txt.gz
https://snap.stanford.edu/data/ca-HepTh.txt.gz
https://snap.stanford.edu/data/facebook_combined.txt.gz
https://snap.stanford.edu/data/email-Eu-core.txt.gz
https://snap.stanford.edu/data/wiki-Vote.txt.gz
https://snap.stanford.edu/data/CollegeMsg.txt.gz
```

The analysis graph construction is:

| Graph | Raw source | Analysis transformation |
|---|---|---|
| ca-GrQc | undirected | simple undirected graph, self-loops removed, baseline LCC |
| ca-HepTh | undirected | simple undirected graph, self-loops removed, baseline LCC |
| Facebook Combined | undirected | simple undirected graph, self-loops removed, baseline LCC |
| email-Eu-core | directed | observed directed pairs collapsed to simple undirected graph |
| Wiki-Vote | directed | observed vote pairs collapsed to simple undirected graph |
| CollegeMsg | directed temporal | observed message pairs collapsed over time to simple undirected graph |

No unobserved edges are added.

## 6.2 Analysis graph sizes

The primary baseline LCCs are:

| Graph | Nodes | Edges |
|---|---:|---:|
| ca-GrQc | 4,158 | 13,422 |
| ca-HepTh | 8,638 | 24,806 |
| Facebook Combined | 4,039 | 88,234 |
| email-Eu-core | 986 | 16,064 |
| Wiki-Vote | 7,066 | 100,736 |
| CollegeMsg | 1,893 | 13,835 |

## 6.3 Independent trial counts

```text
ca-GrQc             20
ca-HepTh            15
Facebook Combined   20
email-Eu-core       30
Wiki-Vote           20
CollegeMsg          30
```

## 6.4 Hutchinson estimator

Cell 1 uses:

```text
TRACE_PROBES = 24
```

A Hutchinson Rademacher trace estimator approximates

\[
\operatorname{tr}(L^+).
\]

Each probe is projected onto the subspace orthogonal to the all-ones vector and solved using a grounded sparse Laplacian factorization.

Probe-level uncertainty is numerical-estimator uncertainty.

The probes are **not** inferential experimental replicates.

Statistical inference uses independent damage trials.

---

# 7. Largest Connected Component Retention

After each damage condition, Cell 1 extracts the largest connected component.

Retention is

\[
\eta_f
=
\frac{|V(G_f^{\mathrm{LCC}})|}
{|V(G_0)|}.
\]

This explicitly checks whether a favorable hitting-time value is merely an artifact of evaluating a severely reduced surviving graph.

---

# 8. Corrected Size-Matched Real-Network Control

At:

```text
10%
20%
30%
40%
```

preferential damage, the notebook constructs an undamaged connected induced subgraph from the **same observed network** with the same number of nodes as the preferentially damaged LCC.

The control uses randomized frontier growth.

It does not generate a synthetic topology.

For each fraction the raw pair-level ratio is

\[
C_{\mathrm{size}}
=
\frac{\overline{H}_{\mathrm{pref}}}
{\overline{H}_{\mathrm{size}}}.
\]

Values below 1 favor the preferentially damaged topology.

## 8.1 Pair-level output is retained

The notebook still writes:

```text
size_matched_pair_level.csv
```

This preserves all graph/trial/fraction observations.

## 8.2 Corrected independent-trial inference

The four size-control fractions are nested within the same progressive trajectory.

The corrected notebook therefore computes, for each graph and trial:

```text
mean of the 10%, 20%, 30%, and 40% pref/size ratios
```

and writes:

```text
size_matched_trial_level.csv
```

The Wilcoxon test is then performed across those independent trial-level ratios.

The notebook **does not** treat the four nested fractions as four independent experimental replicates.

The final network-level corrected summary remains:

```text
size_matched_controls.csv
```

---

# 9. Cell 1 Primary Output Files

Cell 1 writes:

```text
dataset_provenance.csv
network_structure.csv
baseline_transport.csv
real_networks_raw.csv
curve_statistics.csv
trial_sustained_scores.csv
preferential_vs_random.csv
size_matched_pair_level.csv
size_matched_trial_level.csv
size_matched_controls.csv
network_summary.csv
modularity_association.csv
results_auto.tex
```

The added `size_matched_trial_level.csv` is the trial-level inferential dataset.

The older pair-level CSV is intentionally retained for transparency and reproducibility.

---

# 10. Cell 1 Primary Figures

Cell 1 saves each primary figure as both:

```text
PNG
PDF
```

The primary figure stems include:

```text
ca_grqc_preferential_vs_random
ca_hepth_preferential_vs_random
facebook_combined_preferential_vs_random
email_eu_core_preferential_vs_random
wiki_vote_preferential_vs_random
collegemsg_preferential_vs_random
lcc_retention_all_real_networks
modularity_vs_sustained_af_real_only
size_matched_control_real_networks
```

The PDF files are intermediate/reproducibility outputs.

The final article collector uses the PNG files.

---

# 11. Preferential-vs-Random Inference

Preferential and random trajectories are paired by graph and trial.

For each independent trial, the notebook computes:

```text
sustained AF under preferential damage
sustained AF under random damage
```

and applies a one-sided Wilcoxon signed-rank test in the direction:

\[
A_{\mathrm{AF}}^{\mathrm{pref}}
>
A_{\mathrm{AF}}^{\mathrm{random}}.
\]

Benjamini-Hochberg FDR correction is applied across the six SNAP network-level tests.

---

# 12. Cell 2 — Confirmatory Global-Efficiency Benchmark

Cell 2 loads the saved primary experiment results and evaluates the same damaged topologies using shortest-path global efficiency.

The global-efficiency estimator uses:

```text
EFFICIENCY_SOURCES = 24
```

uniformly sampled source nodes per graph state.

For a graph \(G\),

\[
E(G)
=
\frac{1}{n(n-1)}
\sum_{i\neq j}
\frac{1}{d(i,j)}.
\]

Relative performance is:

\[
E_f/E_0.
\]

Interpretation:

```text
> 1.0  -> improved shortest-path global efficiency
< 1.0  -> degraded shortest-path global efficiency
```

---

# 13. Cell 2 Corrected Global-Efficiency Size Control

Cell 2 writes the raw pair-level file:

```text
global_efficiency_size_matched_pair_level.csv
```

and then aggregates the nested:

```text
10%
20%
30%
40%
```

ratios within each independent trial.

The corrected trial-level file is:

```text
global_efficiency_size_matched_trial_level.csv
```

Inference is performed across independent trials.

The network-level output remains:

```text
global_efficiency_size_matched_summary.csv
```

This is now statistically aligned with the primary hitting-time size-control analysis.

---

# 14. Cell 2 Primary Hitting-Time Effect Sizes

Cell 2 also writes:

```text
primary_hitting_time_effect_sizes.csv
```

The preferential-vs-random effect size is computed at the independent-trial sustained-score level.

The preferential-vs-size-matched effect is also now computed at the independent-trial level after within-trial aggregation of the nested size-control fractions.

---

# 15. Cell 2 Output Files

Cell 2 writes or uses:

```text
baseline_global_efficiency.csv
global_efficiency_raw.csv
global_efficiency_curve_statistics.csv
trial_sustained_global_efficiency.csv
global_efficiency_preferential_vs_random.csv
global_efficiency_size_matched_pair_level.csv
global_efficiency_size_matched_trial_level.csv
global_efficiency_size_matched_summary.csv
primary_hitting_time_effect_sizes.csv
benchmark_classification.csv
confirmatory_results_auto.tex
```

---

# 16. Cell 2 Intermediate Figure Files

Cell 2 initially writes its confirmatory figures as PDF files.

These include:

```text
ca_grqc_global_efficiency_pref_vs_random.pdf
ca_hepth_global_efficiency_pref_vs_random.pdf
facebook_combined_global_efficiency_pref_vs_random.pdf
email_eu_core_global_efficiency_pref_vs_random.pdf
wiki_vote_global_efficiency_pref_vs_random.pdf
collegemsg_global_efficiency_pref_vs_random.pdf
global_efficiency_size_control.pdf
```

These PDFs are intermediate notebook outputs.

They are converted to PNG in Cell 4.

---

# 17. Cell 3 — Descriptive Structural Correlates

Cell 3 loads:

```text
network_structure.csv
network_summary.csv
```

and merges only observed network-level quantities.

It computes descriptive Spearman associations between sustained preferential anti-fragility and structural descriptors such as:

```text
modularity
average clustering
transitivity
assortativity
degree coefficient of variation
density
node count
edge count
mean degree
community count
```

It writes:

```text
structural_correlates_input.csv
structural_correlates_spearman.csv
structural_correlates_auto.tex
```

The six-network structural screen is descriptive and hypothesis-generating only.

It is not used as a causal mechanism test.

---

# 18. Cell 4 — Confirmatory PDF-to-PNG Conversion

Cell 4 uses PyMuPDF to convert the seven confirmatory PDF figures generated by Cell 2 into 300-DPI PNG files.

Expected PNG files:

```text
ca_grqc_global_efficiency_pref_vs_random.png
ca_hepth_global_efficiency_pref_vs_random.png
facebook_combined_global_efficiency_pref_vs_random.png
email_eu_core_global_efficiency_pref_vs_random.png
wiki_vote_global_efficiency_pref_vs_random.png
collegemsg_global_efficiency_pref_vs_random.png
global_efficiency_size_control.png
```

Cell 4 does not delete the PDF files.

It adds the PNG versions.

---

# 19. Cell 5 — Final Main-Article Figure Collector

Cell 5 creates:

```text
/content/drive/MyDrive/Markov_Antifragility_IEEE/article_figures/
```

and validates the exact set of 16 PNG files used by the six-SNAP-network portion of the article.

Expected files:

```text
ca_grqc_preferential_vs_random.png
ca_hepth_preferential_vs_random.png
facebook_combined_preferential_vs_random.png
email_eu_core_preferential_vs_random.png
wiki_vote_preferential_vs_random.png
collegemsg_preferential_vs_random.png
size_matched_control_real_networks.png
lcc_retention_all_real_networks.png
modularity_vs_sustained_af_real_only.png

ca_grqc_global_efficiency_pref_vs_random.png
ca_hepth_global_efficiency_pref_vs_random.png
facebook_combined_global_efficiency_pref_vs_random.png
email_eu_core_global_efficiency_pref_vs_random.png
wiki_vote_global_efficiency_pref_vs_random.png
collegemsg_global_efficiency_pref_vs_random.png
global_efficiency_size_control.png
```

Cell 5 is deliberately strict.

The main article figure directory is expected to contain the required PNG set and no unexpected article-figure files.

---

# 20. Cell 6 — Exgentic Agent/Tool AI-Domain Validation

Cell 6 performs the direct AI-domain validation using:

```text
Exgentic/agent-llm-traces-v2
```

Dataset page:

```text
https://huggingface.co/datasets/Exgentic/agent-llm-traces-v2
```

The cell installs missing dependencies when required and uses Hugging Face `datasets` streaming to process the trace corpus.

---

# 21. Exgentic Graph Construction

The Exgentic graph contains observed categories such as:

```text
benchmark
harness
model
tool
```

The graph uses observed relationships including:

```text
benchmark -- harness
harness -- model
model -- tool
tool -- tool
```

Tool-to-tool edges represent consecutive observed tool invocations.

The graph is collapsed to a simple undirected topology to match the mathematical assumptions of the primary hitting-time analysis.

No synthetic graph generator is used.

No synthetic nodes or edges are added.

---

# 22. Completed Exgentic Run

The completed domain-validation run processed:

```text
10,056 traces
241,473 spans
160,669 observed tool calls
```

Baseline largest connected component:

```text
497 nodes
3,822 edges
```

---

# 23. Exgentic Experimental Configuration

Cell 6 uses:

```text
damage fractions:        5% to 45% in 5% increments
size-control fractions:  10%, 20%, 30%, 40%
independent trials:      30
Hutchinson probes:       24
global-efficiency sources: 24
improvement threshold:   R(f) < 0.98
```

The Exgentic cell already performs the nested size-control inference correctly.

It aggregates the four size-control fractions within trial before Wilcoxon testing.

---

# 24. Exgentic Output Files

Cell 6 writes:

```text
dataset_provenance.csv
network_structure.csv
baseline_transport.csv
baseline_global_efficiency.csv

exgentic_agent_tool_nodes.csv
exgentic_agent_tool_edges.csv

ai_domain_raw.csv
curve_statistics.csv
trial_sustained_scores.csv
preferential_vs_random.csv

size_matched_pair_level.csv
size_matched_trial_level.csv
size_matched_controls.csv

network_summary.csv
domain_validation_table.tex
domain_structure_table.tex
RESULTS_SUMMARY.txt
```

Checkpointing uses:

```text
checkpoints/ai_domain_raw_checkpoint.csv
```

---

# 25. Exgentic PNG Figures

Cell 6 creates PNG figures only in:

```text
ai_domain_validation_exgentic/figures_png/
```

Expected figures:

```text
exgentic_agenttool_preferential_vs_random.png
exgentic_agenttool_lcc_retention.png
exgentic_agenttool_size_matched_ht.png
exgentic_agenttool_global_efficiency_pref_vs_random.png
exgentic_agenttool_global_efficiency_size_control.png
exgentic_agenttool_node_type_composition.png
```

The Exgentic final audit checks that no PDF figure files were generated inside its output directory.

---

# 26. Primary Six-Network Results

After the corrected independent-trial size-control inference, the six SNAP networks are summarized as follows.

| Network | Max mean HT improvement | Levels ≥2% | Sustained AF pref. | Sustained AF random | Pref-v-random BH p | LCC at best | Mean \(H_{pref}/H_{size}\) | Corrected size BH p |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| CollegeMsg | 35.79% | 9/9 | 0.1961 | 0.1830 | 4.72e-07 | 0.907 | 0.862 | 2.79e-09 |
| email-Eu-core | 36.89% | 9/9 | 0.2038 | 0.1803 | 3.91e-08 | 0.970 | 0.819 | 2.79e-09 |
| Facebook Combined | 14.23% | 6/9 | 0.0589 | 0.0061 | 1.43e-06 | 0.982 | 0.950 | 5.72e-06 |
| Wiki-Vote | 43.83% | 9/9 | 0.2440 | 0.2561 | 1.000 | 0.916 | 0.801 | 1.43e-06 |
| ca-GrQc | 19.04% | 9/9 | 0.1234 | 0.0708 | 1.43e-06 | 0.907 | 0.942 | 1.43e-06 |
| ca-HepTh | 12.43% | 8/9 | 0.0795 | 0.0739 | 0.0153 | 0.890 | 0.998 | 0.151 |

Controlled cases:

```text
ca-GrQc
Facebook Combined
email-Eu-core
CollegeMsg
```

Control-dependent cases:

```text
Wiki-Vote
ca-HepTh
```

---

# 27. Corrected Trial-Level Hitting-Time Size Controls

The corrected trial-level statistics are:

| Network | Trials | Mean \(H_{pref}/H_{size}\) | 95% CI | BH p |
|---|---:|---:|---:|---:|
| CollegeMsg | 30 | 0.862105 | [0.859443, 0.864768] | 2.793968e-09 |
| email-Eu-core | 30 | 0.818719 | [0.815094, 0.822344] | 2.793968e-09 |
| Facebook Combined | 20 | 0.950202 | [0.934090, 0.966313] | 5.722046e-06 |
| Wiki-Vote | 20 | 0.801140 | [0.800026, 0.802254] | 1.430511e-06 |
| ca-GrQc | 20 | 0.942385 | [0.938212, 0.946557] | 1.430511e-06 |
| ca-HepTh | 15 | 0.997696 | [0.993335, 1.002056] | 0.151398 |

The correction changes the older fraction-level p-values but does not change the four-network controlled classification.

---

# 28. Confirmatory Global-Efficiency Result

Corrected trial-level mean preferential-to-size-matched global-efficiency ratios are approximately:

```text
Facebook Combined   0.761
ca-HepTh            0.845
ca-GrQc             0.869
CollegeMsg          0.921
email-Eu-core       0.940
Wiki-Vote           0.951
```

All are below 1.

For the prespecified improvement direction:

```text
E_pref/E_size > 1
```

the corrected BH-adjusted p-values are:

```text
1.0 for all six SNAP networks
```

The shortest-path benchmark therefore does not confirm the primary Markovian hitting-time benefit.

---

# 29. Exgentic Result

The Exgentic agent/tool topology does not satisfy the strict anti-fragility criteria.

Primary result:

```text
maximum mean improvement:      -1.4239%
best damage fraction:           5%
levels >= 2% improvement:       0/9
sustained preferential AF:      0.000087
sustained random AF:            0.000000
preferential vs random p:       0.03980790073
rank-biserial:                  0.866667
LCC retention at best:          0.999732
```

Independent-trial size control:

```text
H_pref/H_size:                  1.074018
95% CI:                         [1.068496, 1.079539]
one-sided p:                    1.0
rank-biserial benefit:          -1.0
```

Confirmatory global efficiency:

```text
E_pref/E_size:                  0.820397
95% CI:                         [0.815609, 0.825186]
one-sided p:                    1.0
rank-biserial benefit:          -1.0
```

Classification:

```text
Unsupported under strict controls
```

This is an important negative domain-validation result rather than a failure of the analysis.

It demonstrates that the framework does not automatically label an observed agentic-AI topology as anti-fragile.

---

# 30. Structural Context

The six SNAP networks show descriptive associations including approximately:

```text
modularity vs sustained preferential AF:
rho = -0.600

average clustering:
rho = -0.771

degree coefficient of variation:
rho = +0.543
```

No tested structural descriptor is treated as a statistically established mechanism.

The structural cell is intentionally descriptive because the sample contains only six SNAP graphs.

The Exgentic graph is not pooled into that six-network descriptor screen.

---

# 31. Data Integrity Checks

The primary notebook audit checks that:

```text
all configured real datasets were downloaded
all expected network-structure rows exist
all baselines exist
all network summary rows exist
raw results contain no unknown graph names
damage types are restricted to empirical transformations
no damaged f=0 rows are mixed into the damaged dataset
relative hitting times are finite
LCC fractions are within (0,1]
```

The study records:

```text
Synthetic graph generators used: NONE
Hardcoded experimental outcomes: NONE
```

Dataset source information and hashes are written to:

```text
dataset_provenance.csv
```

The Exgentic cell has its own output audit.

---

# 32. Python Dependencies

The notebook uses:

```text
numpy
pandas
scipy
networkx
matplotlib
requests
datasets
PyMuPDF
```

The Exgentic cell installs missing required packages automatically.

The PDF-to-PNG cell installs PyMuPDF if necessary.

A manual environment can use:

```bash
pip install numpy pandas scipy networkx matplotlib requests datasets pymupdf
```

---

# 33. Recommended Run Procedure

Open:

```text
markovian_anti_fragility_poc_FIXED.ipynb
```

in Google Colab.

Run the cells in order:

```text
Cell 1 -> primary SNAP experiment
Cell 2 -> confirmatory global efficiency
Cell 3 -> structural descriptors
Cell 4 -> confirmatory PDF to PNG conversion
Cell 5 -> final 16-PNG article figure collection
Cell 6 -> Exgentic AI-domain validation
```

For a clean full reproduction, allow each cell to complete before running the next cell.

Cells 1 and 2 can be computationally expensive because they repeatedly factor sparse graph Laplacians and evaluate multiple damaged graph states.

---

# 34. Why Pair-Level Files Are Still Kept

The corrected notebook intentionally retains:

```text
size_matched_pair_level.csv
global_efficiency_size_matched_pair_level.csv
```

These files are useful for transparency, plotting, diagnostics, and checking the nested trajectories.

They are **not** the inferential sample used for the corrected size-control hypothesis tests.

Inference uses:

```text
size_matched_trial_level.csv
global_efficiency_size_matched_trial_level.csv
```

This distinction is important.

---

# 35. Interpretation

The code does not establish that targeted damage generally improves a network.

The supported result is narrower:

> Progressive topology-aware edge deletion can produce controlled improvements in Markovian random-walk transport on some real network topologies.

The same effect does not generalize to shortest-path global efficiency.

The real Exgentic agent/tool graph does not reproduce the anti-fragile hitting-time result.

The phenomenon is therefore:

```text
metric dependent
topology dependent
not universal
```

---

# 36. AI-Security Interpretation

In an AI architecture:

```text
node -> agent, model, tool, memory service, retrieval service, database, router, coordinator
edge -> communication, delegation, API call, tool dependency, service dependency
```

Preferential edge damage approximates topology-aware disruption of links surrounding highly connected components.

The Exgentic experiment is particularly important because it evaluates a topology reconstructed from observed agent/tool execution traces rather than only generic social, collaboration, voting, or email graphs.

Its negative result prevents overgeneralization from the four positive SNAP cases.

---

# 37. Reproducibility Summary

The fixed notebook and this README now agree on all major implementation details:

```text
6 notebook code cells
6 public SNAP testbeds
1 Exgentic agent/tool validation graph
9 progressive damage levels
24 Hutchinson probes
matched random damage
LCC retention
same-network size controls
pair-level outputs retained
trial-level size-control inference
confirmatory global efficiency
descriptive structural analysis
intermediate PDF figures retained
final article PNG figures collected
Exgentic PNG-only domain figures
Google Drive output structure
```

---

# 38. Suggested Repository Layout

A simple GitHub repository can use:

```text
.
├── README.md
├── markovian_anti_fragility_poc_FIXED.ipynb
└── article/
    ├── Markovian_Anti_Fragility_EXGENTIC_INTEGRATED_FINAL.tex
    └── references_FINAL.bib
```

Generated result files do not need to be committed unless desired.

If results are committed, a recommended layout is:

```text
results/
├── results_real_only/
├── article_figures/
└── ai_domain_validation_exgentic/
```

---

# 39. Suggested Citation

Update the journal, volume, issue, pages, DOI, and year when publication metadata is available.

```bibtex
@article{yocam_antifragile_markovian_transport,
  author  = {Eric Yocam and Varghese Mathew Vaidyan},
  title   = {Antifragile Markovian Transport Under Targeted Edge Damage:
             Validation on Real and Agentic-AI Graphs},
  journal = {IEEE Transactions},
  year    = {2026},
  note    = {Manuscript under review}
}
```

---

# 40. Authors

**Eric Yocam**  
Computer Science and Software Engineering  
California Polytechnic State University  
San Luis Obispo, California, USA  
eyocam@calpoly.edu

**Varghese Mathew Vaidyan**  
Beacom College of Computer and Cyber Sciences  
Dakota State University  
Madison, South Dakota, USA  
varghese.vaidyan@dsu.edu

---

# 41. Scope and Limitations

This notebook is a research artifact.

It is not a production security control.

The topology-aware damage process is a controlled experimental threat model and does not represent every possible adaptive adversary.

The SNAP structural analysis uses static undirected graph representations.

The Exgentic graph is a reconstructed agent/tool interaction topology rather than a full production AI-service architecture.

The results should therefore be interpreted as controlled empirical network-science evidence rather than a recommendation to deliberately damage deployed systems.

---

# 42. Final Reproduction Checklist

A complete reproduction should verify:

```text
[ ] Cell 1 completed
[ ] six SNAP datasets downloaded
[ ] dataset_provenance.csv generated
[ ] real_networks_raw.csv generated
[ ] size_matched_pair_level.csv generated
[ ] size_matched_trial_level.csv generated
[ ] size_matched_controls.csv generated
[ ] network_summary.csv generated

[ ] Cell 2 completed
[ ] global_efficiency_raw.csv generated
[ ] global_efficiency_size_matched_pair_level.csv generated
[ ] global_efficiency_size_matched_trial_level.csv generated
[ ] global_efficiency_size_matched_summary.csv generated
[ ] benchmark_classification.csv generated

[ ] Cell 3 completed
[ ] structural_correlates_input.csv generated
[ ] structural_correlates_spearman.csv generated

[ ] Cell 4 completed
[ ] seven confirmatory PNG files generated

[ ] Cell 5 completed
[ ] exactly 16 primary/confirmatory article PNG files collected

[ ] Cell 6 completed
[ ] Exgentic traces processed
[ ] Exgentic node and edge CSVs generated
[ ] Exgentic size_matched_trial_level.csv generated
[ ] Exgentic RESULTS_SUMMARY.txt generated
[ ] six Exgentic PNG figures generated
```

The notebook and README are considered aligned when the regenerated trial-level size-control outputs are used for statistical interpretation while all raw pair-level outputs remain available for audit and visualization.
