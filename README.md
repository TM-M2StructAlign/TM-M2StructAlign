# Multiobjective Optimization for Structural-Aware Sequence Alignment of Transmembrane Proteins

[![Dataset DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23243974.svg)](https://doi.org/10.5281/zenodo.23243974)

_An optimization framework integrating AlphaFold2-guided structural constraints._

## 📌 Overview

This project implements a transmembrane protein sequence alignment framework combining:

- **Sequence information** (similarities, clustering).
- **Structural data** derived from AlphaFold2 predictions and topology comparisons.
- **Multiobjective optimization** to balance accuracy, stability, and complexity.

The major innovation is the **Structural Aware Alignment** module, which incorporates structural data into alignments and evaluates solutions with metrics such as TM-score, RMSD, or predicted topologies.

## Project ecosystem

TM-M2StructAlign is maintained together with the benchmark dataset and the manuscript sources:

| Resource | Repository |
| --- | --- |
| **TM-M2StructAlign source code** | [TM-M2StructAlign](https://github.com/TM-M2StructAlign/TM-M2StructAlign) |
| **GPCR structural benchmark and experimental resources** | [Dataset_Structural_GPCRs](https://github.com/TM-M2StructAlign/Dataset_Structural_GPCRs) · [Zenodo v1.0.0](https://doi.org/10.5281/zenodo.23243974) |
| **Main manuscript and supplementary material** | [TM-M2StructAlign-Paper](https://github.com/JOELITO07/TM-M2StructAlign-Paper) |

### Related publications

1. Cedeño-Muñoz, J., Zambrano-Vega, C., and Nebro, A. J. (2025). **TMP-M2Align: A Topology-Aware Multiobjective Approach to the Multiple Sequence Alignment of Transmembrane Proteins.** *Algorithms*, 18(10), 640. [https://doi.org/10.3390/a18100640](https://doi.org/10.3390/a18100640)
2. Cedeño-Muñoz, J., Zambrano-Vega, C., and Nebro, A. J. **TM-M2StructAlign: A Multiobjective Tool for Structure-Guided Multiple Sequence Alignment of G Protein-Coupled Receptors Using AlphaFold2-Derived Constraints.** Manuscript prepared for *Computational Biology and Chemistry*. Source and supplementary material: [TM-M2StructAlign-Paper](https://github.com/JOELITO07/TM-M2StructAlign-Paper).
3. Zambrano-Vega, C., Cedeño-Muñoz, J., and Nebro, A. J. (2026). **A Curated Dataset of Human GPCR Sequences, AlphaFold-Predicted Structures, Transmembrane Topologies, and Reference Alignments for Structural Bioinformatics** (Version 1.0.0) [Dataset]. Zenodo. [https://doi.org/10.5281/zenodo.23243974](https://doi.org/10.5281/zenodo.23243974).

A companion data article documenting the benchmark resource is maintained in [TM-M2StructAlign_Dataset_paper](https://github.com/JOELITO07/TM-M2StructAlign_Dataset_paper).

### Versioned benchmark dataset

The benchmark dataset associated with TM-M2StructAlign is versioned as **v1.0.0** and assigned the version-specific Zenodo DOI **[10.5281/zenodo.23243974](https://doi.org/10.5281/zenodo.23243974)**. The corresponding GitHub release is [Dataset_Structural_GPCRs v1.0.0](https://github.com/TM-M2StructAlign/Dataset_Structural_GPCRs/releases/tag/v1.0.0), based on source snapshot `75287f1da907248aa29b8fb5adf17a4c63891797`.

For reproducible analyses, cite the Zenodo version-specific DOI and use the archived v1.0.0 dataset snapshot rather than a later state of the live GitHub repository.

---

## Key Features

1. **Alignment strategies**
   - Reference alignments (`.msf`, `.fasta`) and support for external tools (MAFFT, Kalign, ClustalW, etc.).
   - Custom `TM-M2StructAlign` module with mutation and crossover operators tailored to transmembrane sequences.

2. **Structural Aware Alignment (SAA)**
   - Loading predicted topology files (`*_predicted_topologies.3line`).
   - Penalties/bonuses based on agreement between topology and generated structure.
   - Utilization of AlphaFold2 data: model superposition and TM domain extraction.
   - Specialized objectives: topology concordance, helix overlap, Cα distance.

3. **Multiobjective optimization**
   - Evolutionary algorithms run over populations of MSAs.
   - Pareto front evaluation and non-dominated individuals.
   - Metrics included: Baliscore, gap weight, structural similarity.

4. **Benchmarking and testing**
   - Datasets in `resources/benchmarks/ref7` and `custom_tests`.
   - Precomputed results in `precomputed_solutions`.
   - Test scripts under `tests/` for integrity and method comparison.

5. **Lightweight web interface**
   - MSA browser with `libs/msabrowser.js` and `style.css`.
   - Interactive visualization and exploration of results.

6. **Result generation and analysis**
   - Outputs in TSV, FASTA, and MSF formats.
   - Computation of reference fronts (`referenceFronts/*.csv`).
   - Batch evaluation against GPCRdb references with Pair F1, TM-Pair F1,
     and TM-gap rate (see [`docs/evaluation-metrics.md`](docs/evaluation-metrics.md)).

## Installation

```bash
# Requires Java 17+ and Maven
git clone https://github.com/TM-M2StructAlign/TM-M2StructAlign.git
cd TM-M2StructAlign
mvn clean package
```

The runnable JAR will be in `target/` and can be executed with:

```bash
java -jar target/tm-msaligner.jar [options]
```

## Usage

Example executions:

```bash
# standard multiobjective alignment
java -jar tm-msaligner.jar \
  -input resources/benchmarks/ref7/7tm/7tm.tfa \
  -topology resources/benchmarks/ref7/7tm/7tm_predicted_topologies.3line \
  -mode structural-aware \
  -generations 1000

# generate TSV report
java -jar tm-msaligner.jar -report results/FUN.tsv
```

Parameters include:

- `-mode structural-aware` to enable structural constraints.
- `-topology` path to predicted topology file from AlphaFold2.
- `-obj` multiple objectives configurable (`baliscore`, `topology`, …).

Refer to the documentation in `src/main/java/org/...` for additional details.

## Repository Structure

- `src/main/java/org/…` – main source code.
- `resources/…` – example data, benchmarks, and tests.
- `precomputed_solutions/` – reference solutions for comparison.
- `tests/` – unit and integration tests.

## ✨ Structural Aware Alignment Changes

- Initial implementation of the **structural evaluation function**.
- Reading and normalization of 3-line topology format.
- Adaptation of genetic operators to consider TM information.
- Dedicated module `AlignmentStructuralEvaluator` with superposition algorithms.
- Configuration interface for selecting “hard” vs “soft” constraints.
- Added metrics: helix agreement, intra-RMSD, and TM-score.
- Performance improvements via distance caches and parallelism.

## ✅ Example Results

- `resources/benchmarks/*` contain reference alignments.
- `custom_tests/` allows reproducible experiments with custom data.
- Pareto fronts can be visualized with the web tool.

## 🧪 Adding New Data

1. Copy `*.tfa` sequences and `*_predicted_topologies.3line` into a new folder under `resources/`.
2. Add the path to the configuration file or provide it via command line.
3. Run the JAR with `structural-aware` mode.

## 📄 License & Credits

Project licensed under the [LICENSE](LICENSE).  
Developed as part of the transmembrane structural alignment initiative.

---

> 📝 **Note:** The project’s focus is on the Structural Aware Alignment module; the README details its components, activation, and recent modifications.
