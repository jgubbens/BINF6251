
# Project Proposal: Using Spanning Trees to Sort Blood Cells by Maturity and Type

## Research Question

### Question:
Can Kruskal's algorithm be utilized with a minimum spanning tree to predict the order in which blood cells mature using only gene expression profiles?

### Background:
The process of development for blood stem cells involves changes in gene activation. While this process cannot be tracked through a single cell, a large sample of cells from all different points of development can be gathered. In this project, gene expression data of large cell samples will be mapped on a minimum spanning tree using Kruskal's algorithm. This is an application of Kruskal's algorithm on real cell data with the goal of sorting cells by maturity and understanding where they split into white and red blood cells.

## Algorithm and Algorithm Class

### Algorithm Class:
This is an example of a graph algorithm, explored in Module 5.

### Algorithm:
Specific algorithm

### Justification:
Why it's a good fit

## Data Plan

### Data Description

### Data Source

### Data Types

### Licensing or Access Considerations

### Prototype Data Plan
What minimal dataset will be used and how it relates to ultimate realistic dataset

## Success Criteria

### What Success Looks Like

### Expected Outputs

### Result Validation
how to check if result is reasonable

## Pitfall Scan
3 of each. explain 1. why it's a realistic concern adn 2. strategy for detecting and mitigating it
### Data-related issues

### Algorithmic issues

### Evaluation issues

## Planned Repository Structure

I plan to use uv for dependency and virtual environment control.

```
.
├── PROPOSAL.md               # Project proposal
├── README.md                 # Overview and Quick Start
├── uv.lock                   # Automatically generated for dependency and virtual environment control with uv
├── pyproject.toml            # Handwritten for dependency and virtual environment control with uv
├── .gitignore                # List of files to exclude from the repository
├── docs/
│   ├── PSEUDOCODE.md         # Pseudocode and explanations
│   └── PROGRESS.md           # Implementation report and reflection
├── src/
│   ├── graph.py              # Cluster centers and distance graph
│   ├── algorithm.py          # Kruskal's algorithm and union-find
│   ├── tree.py               # Tree building, traversal, lineage paths
│   ├── predictions.py        # Placing cells on the tree and predicting development time
│   └── main.py               # Main project running script
├── scripts/
│   ├── get_data.py           # Download full dataset and generate prototype data
│   ├── preprocess.py         # Normalization and preprocessing of raw dataset
│   └── run_benchmarks.py     # Generate all tables and figures
├── data/
│   ├── prototype/            # Prototype dataset, included in repository
│   ├── raw/                  # Full downloaded datasets, excluded from repository through .gitignore
│   └── processed/            # Preprocessed and normalized data, excluded from repository through .gitignore
└── results/
    └── figures/
```

## Generative AI Disclosure

I used Claude to assist me in creating this proposal. Specifically, I used it in the following sections:

1. I used Claude to find relevant datasets that are usable in my project goal. I prompted, "I want to apply minimum spanning trees and Kruskal's algorithm in a way that tracks development or state transitions in cells. I have no access to wet lab resources or way to generate my own dataset. Find publicly available datasets that contain sufficient per-cell information suitable for this project." From the list of datasets it outputted, I explored which ones would be most suitable for my project and interested me most, and I chose the blood cell gene expression dataset.

2. I used Claude to generate the above Planned Repository Structure. I prompted, "Generate me a sketch of repository structure containing the following files," and I provided the planned file names and directories. I then added comments to its output, explaining what each file and directory is. This was done to display the planned repository structure in a way that is more clear to a reader.

Any use of AI tools in later parts of the project will be disclosed at that time.