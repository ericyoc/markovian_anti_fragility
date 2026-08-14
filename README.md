# Markovian Anti-Fragility POC

This repository contains a Google Colab notebook for evaluating Markovian transport under progressive edge damage on real network data and an observed agent/tool interaction graph.

Main notebook:

```text
markovian_anti_fragility_poc.ipynb
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

## Notebook Cells

The notebook contains six main code cells.

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

Computes descriptive Spearman associations between sustained anti-fragility and network characteristics such as:

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

## Figures

The notebook produces primary and confirmatory figures and collects the final PNG files.

The Exgentic cell creates:

```text
exgentic_agenttool_preferential_vs_random.png
exgentic_agenttool_lcc_retention.png
exgentic_agenttool_size_matched_ht.png
exgentic_agenttool_global_efficiency_pref_vs_random.png
exgentic_agenttool_global_efficiency_size_control.png
exgentic_agenttool_node_type_composition.png
```

## Python Requirements

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

Install manually if needed:

```bash
pip install numpy pandas scipy networkx matplotlib requests datasets pymupdf
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
```

