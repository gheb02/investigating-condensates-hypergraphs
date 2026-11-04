# Investigating Biomolecular Condensates' Topology Using Hypergraphs

This repository contains the code and analyses developed to investigate
the topology of biomolecular condensates through **hypergraph
representations**. The project explores how proteins are organized
across condensates, characterizes higher-order connectivity patterns,
and investigates potential relationships between protein sequence
properties and network structure.

## Overview

Biomolecular condensates are membraneless cellular compartments formed
through the association of proteins and other biomolecules. Since
individual proteins can participate in multiple condensates, and each
condensate can contain multiple proteins, their organization naturally
lends itself to a higher-order network representation.

Rather than relying exclusively on pairwise interactions, this project
models the protein--condensate system as a **hypergraph**, where:

-   **Nodes** represent proteins.
-   **Hyperedges** represent biomolecular condensates.
-   Each hyperedge connects all proteins associated with a given
    condensate.

This representation preserves the multi-protein membership structure of
condensates and enables the investigation of higher-order topological
properties.

## Repository structure

``` text
investigating-condensates-hypergraphs/
│
├── nestedness/
│   └── ...                       # Nestedness-related utilities
│
├── dataset_creation.ipynb        # Data preprocessing and dataset construction
├── hypergraph_analysis.ipynb     # Hypergraph topology and structural analysis
├── connectivity_analysis.ipynb   # Connectivity and protein sequence features
│
├── preprocessing.py              # Data preprocessing and hypergraph construction
├── utils.py                      # Analysis, statistical and visualization utilities
│
├── LICENSE
└── README.md
```

## Analysis workflow

The project is organized into three main stages.

### 1. Dataset construction

`dataset_creation.ipynb`

This notebook prepares the protein and condensate datasets from the
available source files.

The preprocessing pipeline includes:

-   Loading and cleaning protein information from JSON and CSV files.
-   Extracting protein identifiers, names, species information and
    disorder-related features.
-   Processing condensate information and filtering records according to
    relevant criteria.
-   Integrating protein and condensate datasets through protein
    identifiers.
-   Preparing the protein--condensate membership information required
    for hypergraph construction.

The main preprocessing functions are implemented in `preprocessing.py`.

### 2. Hypergraph construction and topological analysis

`hypergraph_analysis.ipynb`

This notebook constructs and investigates the protein--condensate
hypergraph.

The hypergraph is built from the protein--condensate membership mapping,
retaining condensates containing at least three proteins by default.

The analysis focuses on several structural properties:

**Basic topology** - Number of proteins (nodes). - Number of condensates
(hyperedges). - Incidence density. - Connected components. - Clustering
coefficient.

**Connectivity distributions** - Protein hyperdegree: number of
condensates associated with each protein. - Hyperedge cardinality:
number of proteins belonging to each condensate. - Complementary
cumulative distribution functions (CCDFs). - Power-law distribution
fitting and characterization.

**Nestedness**

Nestedness is investigated through the NODF (Nestedness based on Overlap
and Decreasing Fills) metric, considering both protein and condensate
dimensions.

**Null-model comparisons**

The observed structural properties are compared against randomized
hypergraphs to assess whether the observed patterns can be explained by
simpler structural constraints.

The implemented null models include:

-   Edge-size-preserving randomization: preserves condensate
    cardinalities while randomly assigning proteins.
-   Node-degree-preserving randomization: preserves protein hyperdegrees
    while randomizing condensate assignments.
-   Fill-only randomization: provides a less constrained baseline.
-   Fixed-row/fixed-column incidence-matrix randomization: preserves
    both protein hyperdegrees and condensate cardinalities for
    nestedness comparisons.

The analysis also considers alternative structural scenarios, including
the exclusion of the largest condensate.

### 3. Connectivity and protein sequence features

`connectivity_analysis.ipynb`

This notebook investigates the relationship between protein connectivity
and sequence-related properties.

Two complementary connectivity measures are considered:

-   **Hyperdegree:** number of condensates in which a protein
    participates.
-   **Projection degree:** number of distinct proteins connected to a
    given protein in the one-mode projection of the protein--condensate
    bipartite network.

These measures are compared against protein sequence features,
including:

**Intrinsic disorder** - Mean IUPred score. - Standard deviation of
IUPred scores. - Number of disordered regions at different thresholds. -
Maximum disordered-region length.

**Sequence composition** - Sticker fraction (Y, F, R residues). -
IDR-related residue fraction (S, G, Q, N residues). - Reactive residue
fraction (Y, F, R, S, G, Q, N residues).

Associations are evaluated using both Pearson and Spearman correlation
coefficients.

To assess the robustness of the results, multiple analytical scenarios
are considered:

1.  All proteins.
2.  Excluding proteins with hyperdegree equal to one.
3.  Excluding the largest condensate and recomputing the connectivity
    measures.

Multiple-testing correction is applied within each scenario. The
implementation supports Benjamini--Hochberg, Holm and Bonferroni
corrections.

## Code organization

  -----------------------------------------------------------------------
  File                                Description
  ----------------------------------- -----------------------------------
  `preprocessing.py`                  Loading, cleaning and integrating
                                      protein and condensate datasets;
                                      hypergraph construction.

  `utils.py`                          Hypergraph statistics, nestedness
                                      calculations, null-model
                                      procedures, correlation analyses
                                      and visualization utilities.

  `dataset_creation.ipynb`            Dataset preparation and
                                      preprocessing workflow.

  `hypergraph_analysis.ipynb`         Structural characterization of the
                                      protein--condensate hypergraph.

  `connectivity_analysis.ipynb`       Statistical analysis of
                                      connectivity and protein sequence
                                      features.
  -----------------------------------------------------------------------

## Requirements

The project is implemented in Python and uses the following main
libraries:

-   [NumPy](https://numpy.org/)
-   [Pandas](https://pandas.pydata.org/)
-   [SciPy](https://scipy.org/)
-   [NetworkX](https://networkx.org/)
-   [XGI](https://xgi.readthedocs.io/)
-   [HypergraphX](https://github.com/HGX-Team/hypergraphx)
-   [Matplotlib](https://matplotlib.org/)
-   [Seaborn](https://seaborn.pydata.org/)
-   [Powerlaw](https://github.com/jeffalstott/powerlaw)
-   [Jupyter Notebook](https://jupyter.org/)

The repository also includes nestedness-related utilities under
`nestedness/`.

Install the main dependencies with:

``` bash
pip install numpy pandas scipy networkx xgi hypergraphx matplotlib seaborn powerlaw jupyter
```

## Getting started

Clone the repository:

``` bash
git clone https://github.com/gheb02/investigating-condensates-hypergraphs.git

cd investigating-condensates-hypergraphs
```

Install the dependencies and launch Jupyter:

``` bash
pip install numpy pandas scipy networkx xgi hypergraphx matplotlib seaborn powerlaw jupyter

jupyter notebook
```

For a complete execution of the analysis, follow the notebooks in this
order:

1.  `dataset_creation.ipynb` -- prepare the datasets.
2.  `hypergraph_analysis.ipynb` -- construct and characterize the
    hypergraph.
3.  `connectivity_analysis.ipynb` -- investigate the relationship
    between connectivity and protein sequence features.

**Data availability:** The source datasets are not included in this
repository. The notebooks expect the original data files and
intermediate datasets to be available in the corresponding paths
referenced in their configuration cells. In particular, the analysis
uses the CD-Code dataset and associated protein--condensate mapping
files.

## Reproducibility

Randomized analyses use a fixed random seed (`42`) to improve
reproducibility.

The number of random realizations used in null-model analyses can be
adjusted through the corresponding configuration parameters in the
notebooks.

Some analyses may require substantial computation depending on the
number of random realizations and the size of the incidence matrix.

## License

This project is distributed under the [MIT License](LICENSE).
