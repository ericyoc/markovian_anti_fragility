# Markovian Antifragility POC

This repository contains a Google Colab notebook for evaluating Markovian transport under progressive edge damage on real network data and two observed AI-agent interaction graphs.

Main notebook:

```text
markovian_anti_fragility_poc_FIXED.ipynb
```

## What the Notebook Does

The notebook:

- Downloads six real SNAP network datasets.
- Builds simple undirected analysis graphs.
- Applies progressive preferential edge damage.
- Runs matched random edge-damage controls.
- Computes average random-walk hitting time.
- Tracks largest-connected-component retention.
- Runs same-network size-matched controls.
- Uses independent trial-level inference for nested size-control fractions.
- Computes a confirmatory global-efficiency benchmark.
- Calculates descriptive structural correlations.
- Converts confirmatory figures to PNG.
- Collects the final figure set.
- Builds and evaluates an Exgentic agent/tool interaction graph from real execution traces.
- Builds and evaluates a second AI-domain graph from real Pi coding-agent sessions, using only observed session-tool use and consecutive tool transitions.


## Why This Matters for AI

Modern AI systems are increasingly networked rather than isolated.

Examples include:

```text
multi-agent systems
tool-using LLMs
retrieval-augmented generation pipelines
memory systems
model-routing architectures
distributed inference systems
API and service dependency graphs
```

In these systems, a node can represent an:

```text
agent
model
tool
memory service
retrieval service
database
router
coordinator
external API
```

and an edge can represent:

```text
communication
delegation
tool use
API dependency
service dependency
information flow
```

This notebook studies what happens when links around highly connected components are preferentially disrupted.

That is relevant to AI security because highly connected agents, tools, routers, or services can become important attack surfaces or failure points.

The experiment asks whether a networked system only degrades under targeted disruption, or whether some topologies can show improved stochastic transport after damage.

The two AI-domain experiments make this connection concrete rather than relying only on general-purpose network datasets. Exgentic reconstructs an observed agent/tool graph from LLM-agent execution traces, while Real Pi reconstructs an observed coding-agent graph from real human-AI coding sessions and actual tool use.

Neither AI-domain graph satisfies the strict Antifragility criteria, but they fail for different reasons. Exgentic shows essentially no absolute hitting-time improvement. Real Pi shows substantial nominal improvement and a statistically significant preferential-versus-random advantage, but that apparent benefit does not survive the same-network size-matched control. Together, these results show why nominal post-damage improvement alone is not enough and why the same decision criteria must be retained across domains.

## Important Concepts

### Antifragility

Antifragility is stronger than robustness or resilience.

```text
robustness   -> performance degrades only slightly under stress
resilience   -> performance recovers after stress
Antifragility -> some measured property improves under stress
```

In this notebook, Antifragility refers specifically to improved **Markovian transport**, measured by lower average random-walk hitting time after damage.

It does not mean that the full AI system becomes more accurate, safer, or better at completing tasks.

### Markovian Transport

Markovian transport models movement through a graph as a random walk.

A lower average hitting time means that a random walker can reach destinations more quickly on average.

This can be relevant to systems that involve:

```text
probabilistic delegation
randomized exploration
distributed message passing
stochastic routing
agent-to-agent information propagation
```

### Preferential Edge Damage

Preferential damage targets edges connected to highly connected nodes.

The notebook assigns each edge a weight based on the degree of its two endpoints:

```text
weight = degree(u) + degree(v)
```

This approximates a topology-aware disruption strategy in which links surrounding central agents, shared tools, routers, or services are more likely to be attacked or disabled.

### Matched Random Damage

Preferential damage is compared with random edge removal using the same number of removed edges.

This helps answer an important question:

```text
Is the observed response caused by targeted topology-aware damage,
or would ordinary random failures produce the same behavior?
```

### Largest Connected Component

After damage, the notebook evaluates the largest connected component.

This is necessary because hitting time is defined on connected graphs.

The notebook also records how much of the original graph remains connected so that an apparent improvement is not mistaken for Antifragility when the graph has simply collapsed to a tiny residual component.

### Size-Matched Control

A damaged graph can appear faster simply because fewer nodes remain.

To control for this, the notebook compares a preferentially damaged largest connected component with an undamaged connected subgraph from the same original network containing the same number of nodes.

This is one of the key controls in the experiment.

### Independent Trial-Level Inference

The 10%, 20%, 30%, and 40% size-control points come from the same progressive damage trajectory.

They are therefore nested measurements rather than independent experiments.

The notebook averages those values within each trial first and then performs statistical inference across independent trials.

This avoids pseudoreplication.

### Hutchinson Trace Estimation

Average hitting time requires the trace of the Laplacian pseudoinverse.

For large graphs, computing the full pseudoinverse repeatedly is expensive.

The notebook therefore uses a Hutchinson trace estimator with 24 Rademacher probes.

These probes are used only for numerical approximation.

They are not treated as independent experimental trials.

### Global Efficiency

Global efficiency is a shortest-path metric.

It measures how efficiently nodes can reach one another through shortest paths.

This is different from random-walk hitting time.

The notebook compares both because a network can improve under one transport model while getting worse under another.

That distinction is important for AI systems because different architectures may rely on different communication or routing behaviors.

### Topology Dependence

The notebook does not assume that Antifragility is universal.

Some SNAP networks show controlled improvements in Markovian transport, while others do not.

The Exgentic and Real Pi AI-agent graphs also do not show controlled anti-fragile responses under the strict criteria.

This means the outcome depends on the structure of the network being evaluated and on whether the apparent benefit survives the required controls.

### Metric Dependence

The notebook also shows that a favorable random-walk result does not imply improved shortest-path behavior.

In the SNAP experiments, global efficiency generally decreases even when hitting time improves.

Therefore:

```text
better Markovian transport != better shortest-path efficiency
```

This is an important distinction when evaluating networked AI systems.


## Notebook Cells

The notebook contains seven main code cells.

### Cell 1 — Primary SNAP Experiment

Runs the main experiment on:

```text
ca-GrQc
ca-HepTh
Facebook Combined
email-Eu-core
Wiki-Vote
CollegeMsg
```

This cell handles:

```text
dataset download
SHA-256 provenance
graph construction
baseline network metrics
preferential damage
matched random damage
average hitting time
LCC retention
size-matched controls
statistical tests
primary figures
CSV outputs
LaTeX result tables
data-integrity checks
```

### Cell 2 — Global Efficiency

Runs the confirmatory shortest-path global-efficiency benchmark.

It also applies the corrected trial-level size-control analysis.

### Cell 3 — Structural Analysis

Computes descriptive Spearman associations between sustained Antifragility and network characteristics such as:

```text
modularity
clustering
transitivity
assortativity
degree variation
density
network size
community count
```

### Cell 4 — PDF to PNG Conversion

Converts the confirmatory global-efficiency figures from PDF to PNG.

### Cell 5 — Figure Collection

Collects the final 16 PNG figures into:

```text
/content/drive/MyDrive/Markov_Antifragility_IEEE/article_figures/
```

### Cell 6 — Exgentic Agent/Tool Validation

Loads:

```text
Exgentic/agent-llm-traces-v2
```

and reconstructs an observed agent/tool graph from real execution traces.

The completed run processed:

```text
10,056 traces
241,473 spans
160,669 observed tool calls
```

The baseline largest connected component contains:

```text
497 nodes
3,822 edges
```


### Cell 7 — Real Pi Coding-Agent Validation

Loads the real coding-agent session repository:

```text
MaxDevv/real-pi-coding-agent-traces-sessions
```

The repository contains 1,291 session files. For conservative provenance, the code excludes 38 sessions whose source-dataset names contain `synthetic`, leaving:

```text
1,253 selected real session files
1,130 sessions with observed tool calls
173,267 observed tool calls
46 unique observed tools
```

The observed baseline largest connected component contains:

```text
1,172 nodes
3,328 edges
```

The graph is built only from observed relationships:

```text
session -> tool use
tool -> tool consecutive transition
```

No synthetic graph generator is used, and no synthetic nodes or edges are introduced.

The Real Pi experiment uses the same locked design as the other domain validation:

```text
30 independent trials
5% to 45% progressive damage
matched random damage
10%, 20%, 30%, and 40% size-matched controls
24 Hutchinson probes
independent trial-level inference
confirmatory global efficiency
```

The completed run produced:

```text
maximum mean hitting-time improvement = 17.993564%
best damage level                     = 45%
levels with >=2% improvement          = 7/9
preferential vs random p              = 0.009264740162
H_pref / H_size                       = 1.030131
size-control 95% CI                   = [1.026523, 1.033738]
size-control p                        = 1.0
E_pref / E_size                       = 0.936622
global-efficiency size-control p      = 1.0
strict classification                 = Unsupported under strict controls
```

The result is important because the preferential trajectory beats matched random damage, but the damaged graph does not outperform an undamaged same-size graph from the same real topology.

## Damage Model

For an edge between nodes `u` and `v`, the preferential damage weight is:

```text
degree(u) + degree(v)
```

Higher-weight edges are more likely to be removed earlier.

Damage is applied progressively at:

```text
5%
10%
15%
20%
25%
30%
35%
40%
45%
```

Each trial uses one nested removal ordering.

## Average Hitting Time

The primary transport metric is average simple-random-walk hitting time:

```text
H = [2m / (n - 1)] * trace(L+)
```

where:

```text
n  = number of nodes
m  = number of edges
L+ = Moore-Penrose pseudoinverse of the graph Laplacian
```

Lower hitting time indicates faster Markovian transport.

The notebook uses 24 Hutchinson Rademacher probes to estimate `trace(L+)`.

## Random-Damage Control

Every preferential-damage trial has a matched random-damage trial.

The random condition removes the same number of edges at each damage fraction.

This separates topology-aware damage effects from ordinary random edge loss.

## Size-Matched Control

At:

```text
10%
20%
30%
40%
```

damage, the preferentially damaged largest connected component is compared with a connected induced subgraph from the original undamaged graph containing the same number of nodes.

The raw pair-level results are saved.

Because the four damage fractions come from the same nested trajectory, they are not treated as independent samples.

The notebook first averages the four ratios within each trial and then performs statistical testing across independent trials.

Files:

```text
size_matched_pair_level.csv
size_matched_trial_level.csv
size_matched_controls.csv
```

The same approach is used for global efficiency:

```text
global_efficiency_size_matched_pair_level.csv
global_efficiency_size_matched_trial_level.csv
global_efficiency_size_matched_summary.csv
```

## Trial Counts

```text
ca-GrQc             20
ca-HepTh            15
Facebook Combined   20
email-Eu-core       30
Wiki-Vote           20
CollegeMsg          30
Exgentic             30
Real Pi              30
```

## SNAP Data Sources

The notebook downloads:

```text
https://snap.stanford.edu/data/ca-GrQc.txt.gz
https://snap.stanford.edu/data/ca-HepTh.txt.gz
https://snap.stanford.edu/data/facebook_combined.txt.gz
https://snap.stanford.edu/data/email-Eu-core.txt.gz
https://snap.stanford.edu/data/wiki-Vote.txt.gz
https://snap.stanford.edu/data/CollegeMsg.txt.gz
```

No synthetic graph generators are used for the reported experiments.

## Exgentic Data Source

```text
https://huggingface.co/datasets/Exgentic/agent-llm-traces-v2
```

The graph uses observed relationships among:

```text
benchmarks
agent harnesses
models
tools
tool transitions
```

No synthetic nodes or edges are added.


## Real Pi Data Source

```text
https://huggingface.co/datasets/MaxDevv/real-pi-coding-agent-traces-sessions
```

The raw JSONL session files are downloaded directly from the Hugging Face repository and saved to Google Drive.

For conservative provenance, 38 sessions whose source-dataset names contain `synthetic` are excluded before graph construction. The retained graph uses only observed:

```text
session-tool use
consecutive tool-tool transitions
```

The completed validation uses 1,253 selected real session files, including 1,130 sessions with observed tool calls and 173,267 observed tool calls.

No synthetic graph generators, synthetic nodes, synthetic edges, or hardcoded empirical outcomes are used.

## Google Drive Output Paths

Base directory:

```text
/content/drive/MyDrive/Markov_Antifragility_IEEE/
```

Primary results:

```text
results_real_only/
```

Collected figures:

```text
article_figures/
```

Exgentic results:

```text
ai_domain_validation_exgentic/
```

Real Pi results:

```text
ai_domain_validation_real_pi/
```

Real Pi raw session snapshot:

```text
ai_domain_validation_real_pi/raw_dataset/sessions/
```

Real Pi PNG figures:

```text
ai_domain_validation_real_pi/figures_png/
```

## Main Output Files

Primary experiment:

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

Global-efficiency experiment:

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

Structural analysis:

```text
structural_correlates_input.csv
structural_correlates_spearman.csv
structural_correlates_auto.tex
```

Exgentic outputs:

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


Real Pi outputs:

```text
dataset_provenance.csv
session_provenance.csv
excluded_sources_conservative_filter.csv
session_extraction_audit.csv
network_structure.csv
baseline_transport.csv
real_pi_agent_nodes.csv
real_pi_agent_edges.csv
real_pi_agent_graph.graphml
ai_domain_raw.csv
curve_statistics.csv
trial_sustained_scores.csv
preferential_vs_random.csv
size_matched_pair_level.csv
size_matched_trial_level.csv
size_matched_controls.csv
network_summary.csv
RESULTS_SUMMARY.txt
AUDIT.txt
```

## Figures

The notebook produces primary and confirmatory figures for the SNAP experiments and separate PNG validation figures for Exgentic and Real Pi.

The Exgentic cell creates:

```text
exgentic_agenttool_preferential_vs_random.png
exgentic_agenttool_lcc_retention.png
exgentic_agenttool_size_matched_ht.png
exgentic_agenttool_global_efficiency_pref_vs_random.png
exgentic_agenttool_global_efficiency_size_control.png
exgentic_agenttool_node_type_composition.png
```

The Real Pi cell creates:

```text
real_pi_preferential_vs_random.png
real_pi_lcc_retention.png
real_pi_size_matched_ht.png
real_pi_global_efficiency_pref_vs_random.png
real_pi_global_efficiency_size_control.png
real_pi_node_type_composition.png
real_pi_observed_tool_usage.png
```

All Real Pi figures are written as PNG files only.

## Python Requirements

```text
numpy
pandas
scipy
networkx
matplotlib
requests
datasets
huggingface_hub
PyMuPDF
```

Install manually if needed:

```bash
pip install numpy pandas scipy networkx matplotlib requests datasets huggingface_hub pymupdf
```

## How to Run

1. Open `markovian_anti_fragility_poc_FIXED.ipynb` in Google Colab.
2. Run the cells in order.
3. Mount Google Drive when prompted.
4. Allow each cell to finish before starting the next one.
5. Review the generated CSV files, figures, and summary files in Google Drive.

## Reproducibility Notes

The notebook uses deterministic seeds based on graph name, trial, damage condition, and damage fraction.

The raw pair-level outputs are retained for inspection.

Statistical inference for nested size-control fractions is performed at the independent trial level.

Hutchinson probes are numerical approximation samples and are not treated as independent experimental trials.

The notebook preserves the original embedded outputs, but rerunning the fixed notebook regenerates the corrected trial-level statistical outputs.

Both AI-domain validations use the same strict decision logic rather than tuning thresholds after observing the results.

The Real Pi validation stores the raw JSONL session files, manifest-based provenance, SHA-256 hashes, extraction audit, raw graph files, trial-level controls, and final audit in Google Drive.

## Data Integrity

The notebook checks that:

```text
all configured datasets are present
all expected graph summaries are generated
relative hitting times are finite
LCC fractions are valid
damage types are valid
no synthetic graph generators are used
no empirical outcomes are hardcoded
Real Pi synthetic-labeled source sessions are excluded before analysis
Real Pi graph edges come only from observed session-tool use and tool transitions
Real Pi figure outputs are PNG only
```

## License

Add the repository license of your choice at the root of the repository.

The underlying datasets remain subject to their original source licenses and terms.
