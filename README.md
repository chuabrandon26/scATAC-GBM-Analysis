<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/visual-abstract-dark.svg">
  <img alt="Visual abstract: a Python scATAC-seq workflow (TF-IDF, LSI, Leiden clustering, UMAP, gene activity, differential accessibility and machine learning) applied to 22,873 cells from five glioblastoma patients in GEO GSE138794. Each tumor splits into four to nine chromatin states, patients share conserved programs, and an LSI-based logistic regression reaches about 0.79 balanced accuracy." src="assets/visual-abstract-light.svg" width="100%">
</picture>

<h1 align="center">scATAC-seq Analysis of Glioblastoma</h1>

<p align="center">
  Single-cell chromatin accessibility of five glioblastoma patients (GSE138794), from raw peak matrices to cell states, regulatory elements and machine learning
</p>

<p align="center">
  <img alt="scATAC--seq: GSE138794" src="https://img.shields.io/badge/scATAC----seq-GSE138794-4a3aa7">
  <img alt="Python: Scanpy · AnnData" src="https://img.shields.io/badge/Python-Scanpy%20%C2%B7%20AnnData-1f8f6a?logo=python&logoColor=white">
  <img alt="ML: scikit-learn · PyTorch" src="https://img.shields.io/badge/ML-scikit--learn%20%C2%B7%20PyTorch-e09a00">
  <img alt="motifs: HOMER" src="https://img.shields.io/badge/motifs-HOMER-d6457a">
  <img alt="license: MIT" src="https://img.shields.io/badge/license-MIT-566573">
</p>

<p align="center">
  <a href="#overview">Overview</a> &nbsp;·&nbsp; <a href="#results">Results</a> &nbsp;·&nbsp; <a href="#biological-interpretation">Interpretation</a> &nbsp;·&nbsp; <a href="#installation">Installation</a> &nbsp;·&nbsp; <a href="#usage">Usage</a>
</p>

---

## At a glance

| **22,873** | **5** | **4 to 9** | **~20k** | **≈ 0.79** |
|:---:|:---:|:---:|:---:|:---:|
| cells after QC | glioblastoma patients | chromatin states per patient | variable peaks per patient | balanced accuracy, LSI classifier |

> **Take-home:** five glioblastomas share conserved chromatin programs, yet each tumor splits into four to nine accessibility states spanning tumor, OPC-like and immune compartments. Moreover, a simple logistic regression on LSI coordinates separates these states with about 0.79 balanced accuracy.

## Skills demonstrated

| Area | Evidence in this repository |
|---|---|
| **Single-cell epigenomics** | scATAC-seq quality control, TF-IDF normalization, latent semantic indexing, gene activity scoring and differential accessibility |
| **Bioinformatics engineering** | Matrix Market parsing, sparse-matrix optimization, AnnData workflows and multi-patient integration |
| **Statistics** | Wilcoxon rank-sum testing with Benjamini-Hochberg correction, cross-validation and cluster evaluation (Leiden, Louvain, k-means) |
| **Machine learning** | Logistic regression and random forest classifiers, plus a PyTorch autoencoder with an 8-dimensional latent space |
| **Scientific computing** | Python, NumPy, pandas, Scanpy, SciPy, scikit-learn, PyTorch and HOMER |

## Overview

A single-cell ATAC-seq analysis pipeline applied to human glioblastoma patient data from GEO dataset GSE138794. This project implements a full Python-based scATAC workflow, from raw peak-by-cell Matrix Market files through TF-IDF normalization, LSI dimensionality reduction, UMAP clustering, gene activity scoring, differential accessibility, and machine learning, mirroring the logic of established tools such as ArchR, MUON, and SnapATAC2.

---

### Project Overview

- **Data loading**: Parses Matrix Market peak-by-cell exports from scATAC.Explorer for multiple glioblastoma patients and converts them into AnnData cell-by-peak format
- **Quality control**: Filters low-depth cells (<200 accessible peaks) and rarely accessible peaks (<3 cells)
- **TF-IDF normalization**: Downweights broadly accessible peaks while highlighting discriminative regulatory elements
- **LSI dimensionality reduction**: TruncatedSVD applied to TF-IDF matrices with depth-associated component removal
- **UMAP visualization**: Embedding of chromatin accessibility landscapes
- **Clustering**: Leiden, Louvain, and K-means clustering comparisons
- **Gene activity scoring**: Accessibility-based proxy for gene expression
- **Differential accessibility analysis**: Wilcoxon testing between clusters and patients
- **Machine learning**: Logistic Regression, Random Forest classification, and PyTorch autoencoder embeddings
- **Multi-patient integration**: Cross-patient chromatin state comparisons

---

### Dataset

| Property | Value |
|----------|--------|
| GEO Accession | GSE138794 |
| Species | Homo sapiens |
| Disease | Glioblastoma multiforme (GBM) |
| Modality | Single-cell ATAC-seq |
| Patients Included | SF11612, SF11949, SF11956, SF11964, SF11979 |
| Excluded Patient | SF12017 (insufficient cells after QC) |
| Largest Sample | ~6,284 cells |
| Features | ~20,000 variable peaks per patient |

Dataset source:

https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE138794

---

## Results

### Per-Patient UMAP Clustering

Each patient was processed independently through TF-IDF normalization, LSI dimensionality reduction, neighborhood graph construction, Leiden clustering, and UMAP visualization.

The number of recovered chromatin states ranged from four to nine clusters depending on the patient, reflecting substantial intra-tumoral heterogeneity.

![Per-Patient UMAP Comparison](images/comparison_all_patients.png)

---

### Quality Control Metrics

Distribution of accessible peaks per cell before and after filtering.

#### Before Filtering

![QC Before Filtering](images/qc_distributions_before_filtering.png)

#### After Filtering

![QC After Filtering](images/qc_distributions_after_filtering.png)

---

### Pre vs Post QC UMAP

Filtering low-depth cells and low-frequency peaks improves neighborhood structure and reduces noise within the embedding.

![Pre vs Post QC UMAP](images/umap_pre_post_qc.png)

---

### LSI UMAP Clustering

Cells projected into LSI space and colored by Leiden cluster assignment.

Distinct chromatin accessibility states emerge that are consistent with known GBM cellular programs.

![LSI UMAP Clusters](images/umap_lsi_clusters_patients.png)

---

### Cross-Patient UMAP

Cells colored by patient identity reveal both shared chromatin programs and patient-specific states.

Strong overlap between patients suggests conserved regulatory programs across tumors.

![LSI UMAP Patients](images/umap_lsi_clusters_patients.png)

---

### Clustering Method Comparison

Comparison of Leiden, Louvain, and K-means clustering approaches applied to the same LSI embedding.

![Clustering Comparison](images/umap_lsi_leiden_louvain_kmeans.png)

---

### Gene Activity Analysis

Gene activity scores were calculated from promoter and gene-body accessibility.

Marker genes associated with major GBM cellular programs include:

- SOX2
- EGFR
- PDGFRA
- CD44
- PTPRC

These scores help distinguish tumor-associated and immune-associated chromatin states.

![Gene Activity UMAPs](images/umap_gene_activity_marker_scores.png)

---

### QC Metric Visualization in LSI Space

UMAP embeddings colored by quality-control metrics.

This visualization confirms that clustering is not driven primarily by sequencing depth or technical artifacts.

![LSI QC Metrics](images/umap_lsi_qc_metrics.png)

---

### t-SNE Visualization

Alternative dimensionality reduction performed using t-SNE on LSI coordinates.

![t-SNE LSI QC](images/tsne_lsi_patient_qc.png)

---

### Differential Accessibility Analysis

Wilcoxon rank-sum testing was performed between clusters.

Significant peaks were identified using:

- Benjamini-Hochberg FDR correction
- q-value < 0.05
- |log2FC| > 0.5

#### Cluster 0 vs Cluster 1

![Cluster 0 vs Cluster 1](images/diff_peaks_cluster0_vs_cluster1.png)

#### Cluster 4 vs Cluster 2

![Cluster 4 vs Cluster 2](images/diff_peaks_cluster4_vs_cluster2.png)

Differential peaks can be exported as BED files for downstream HOMER motif enrichment analysis.

---

### Cluster × Patient Composition

Fraction of cells contributed by each patient within each cluster.

This helps identify:

- Shared chromatin states
- Patient-enriched clusters
- Potential batch effects

![Cluster Patient Heatmap](images/cluster_patient_heatmap.png)

---

### Machine Learning Classification

Classification models were trained using LSI coordinates as input features.

Models evaluated:

- Logistic Regression
- Random Forest

Cross-validation performance indicates that chromatin state structure is more predictive than patient identity.

![Classifier Cross Validation](images/ml_classifier_cross_validation.png)

---

### Autoencoder Latent Space

A PyTorch autoencoder was trained on LSI coordinates to learn a nonlinear latent representation.

The model compresses chromatin accessibility patterns into an 8-dimensional latent space.

#### Reconstruction Loss

![Autoencoder Loss](images/autoencoder_reconstruction_loss.png)

#### Autoencoder UMAP Embedding

![Autoencoder UMAP](images/umap_autoencoder_clusters_patients.png)

---

### PCA vs UMAP Comparison

Comparison of linear and nonlinear dimensionality reduction approaches.

![PCA UMAP Comparison](images/pca_umap_comparison.png)

---

### Individual Patient Embeddings

#### SF11612

![SF11612](images/GSE138794_Glioma_Patient_SF11612_umap.png)

#### SF11949

![SF11949](images/GSE138794_Glioma_Patient_SF11949_umap.png)

#### SF11956

![SF11956](images/GSE138794_Glioma_Patient_SF11956_umap.png)

#### SF11964

![SF11964](images/GSE138794_Glioma_Patient_SF11964_umap.png)

#### SF11979

![SF11979](images/GSE138794_Glioma_Patient_SF11979_umap.png)

---

## Biological Interpretation

The chromatin accessibility landscape recovered by this pipeline is consistent with known glioblastoma biology.

### 1. Tumor Heterogeneity

Distinct chromatin states are observed both within and across patients, highlighting substantial intra-tumoral diversity.

### 2. Conserved Regulatory Programs

Cross-patient mixing indicates common accessibility programs shared across GBM tumors.

### 3. Tumor and Immune Compartments

Gene activity analyses identify clusters enriched for:

- Tumor-associated programs (SOX2, EGFR)
- OPC-like programs (PDGFRA)
- Immune-associated programs (PTPRC, CD44)

### 4. Differential Regulatory Elements

Cluster-specific peaks identify candidate regulatory regions associated with GBM cell states and provide input for motif discovery workflows.

---

## Repository Structure

```text
scATAC-GBM-Analysis/
│
├── scATAC_GBM_analysis.ipynb
├── README.md
│
└── images/
    ├── autoencoder_reconstruction_loss.png
    ├── cluster_patient_heatmap.png
    ├── comparison_all_patients.png
    ├── diff_peaks_cluster0_vs_cluster1.png
    ├── diff_peaks_cluster4_vs_cluster2.png
    ├── ml_classifier_cross_validation.png
    ├── pca_umap_comparison.png
    ├── qc_distributions_after_filtering.png
    ├── qc_distributions_before_filtering.png
    ├── tsne_lsi_patient_qc.png
    ├── umap_autoencoder_clusters_patients.png
    ├── umap_gene_activity_marker_scores.png
    ├── umap_lsi_clusters_patients.png
    ├── umap_lsi_leiden_louvain_kmeans.png
    ├── umap_lsi_qc_metrics.png
    ├── umap_pre_post_qc.png
    ├── GSE138794_Glioma_Patient_SF11612_umap.png
    ├── GSE138794_Glioma_Patient_SF11949_umap.png
    ├── GSE138794_Glioma_Patient_SF11956_umap.png
    ├── GSE138794_Glioma_Patient_SF11964_umap.png
    └── GSE138794_Glioma_Patient_SF11979_umap.png
```

---

## Installation

```bash
conda create -n scatac_gbm python=3.11 -y
conda activate scatac_gbm

conda install -c conda-forge \
scanpy \
anndata \
scipy \
scikit-learn \
seaborn \
matplotlib \
pytorch \
-y

conda install -c bioconda homer -y
```

Install HOMER genome annotations:

```bash
perl ~/miniforge3/envs/scatac_gbm/share/homer/configureHomer.pl -install hg38
```

---

## Usage

Clone the repository:

```bash
git clone https://github.com/chuabrandon26/scATAC-GBM-Analysis.git

cd scATAC-GBM-Analysis
```

Launch Jupyter Notebook:

```bash
jupyter notebook scATAC_GBM_analysis.ipynb
```

Download raw peak matrices from GEO and place patient directories into the project root before running the notebook.

---

## Key Dependencies

| Package | Purpose |
|----------|----------|
| Scanpy | Neighbors, clustering, UMAP |
| AnnData | Single-cell data structure |
| SciPy | Sparse matrix operations |
| scikit-learn | TF-IDF, LSI, classifiers |
| NumPy | Numerical computing |
| Pandas | Data manipulation |
| Matplotlib | Visualization |
| Seaborn | Statistical plotting |
| PyTorch | Autoencoder |
| HOMER | Motif enrichment |

---


---

## License

MIT License.

See the LICENSE file for details.

---

*Built as a personal portfolio project focused on single-cell chromatin accessibility analysis, computational cancer genomics, and modern bioinformatics workflow development.*
