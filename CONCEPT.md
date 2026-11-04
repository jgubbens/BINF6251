# Conceptual Progress Report

## Recap of Project

### Research Question
Can Kruskal's algorithm predict the order in which cells matrue using only gene expression profiles?

### Algorithm
I will use Kruskal's algorithm, a graph algorithm used to build a minimum spanning tree (MST). Cells will be grouped into clusters with similar attributes. Each cluster will be treated as a node in the tree, while edge weights will represent the distance between clusters. The root of the tree will mark the beginning of development, with paths from the root downward representing cell lineage.

### Data
For my prototype dataset, I will simulate single-cell counts in which one cell population splits into two as it matures. Since data will synthetic, each cell will have true maturity and branch labels.

For my main dataset, I will use mouse blood cell progenitors, loaded with ```scanpy.datasets.paul15()```. This dataset contains gene expression data for over 2000 cells, but has no ground truth maturity labels.

## Inputs, Outputs, and Assumptions

### Inputs
- Cell data: 2d array where rows are cells and columns are gene expression
- A specified root cell: integer representing the row of the data array to be used as the root of the created MST
- Cells per cluster: integer representing the number of cells to be placed in each cluster in the tree
- Seed

### Outputs
- ```results/figures/tree_map.png```: plot showing cluster centers and tree edges labeled by maturity (represented by a float between 0 and 1)
- ```results/clusters.csv```: cell cluster data showing how cells were grouped
- ```results/mst.csv```: data representation of MST

### Assumptions
- Gene expression counts are measurable and complete
- Biological development forms a tree, as lineages should split but not merge
- Cell gene expression changes gradually rather than suddenly as they mature
- The root cell given is one of the least mature cells in the sample

## Detailed Pseudocode

### Kruskal's Algorithm

```
function build_edges(clusters):
    edges = empty list
    for each pair of clusters (u, v) with u < v:
        w = distance between centers[u] and centers[v]
        if w is finite:
            append (w, u, v) to edges
    return edges

function find(parent, x):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

function kruskal(k, edges):
    parent = list from 0 to k-1
    size = array of k ones
    mst_edges = empty list
    n_components = k

    for (w, u, v) in edges sorted ascending by (w, u, v):
        a = find(parent, u)
        b = find(parent, v)
        if a != b: # skip the edge if it would form a loop
            attach the smaller of a and b to the larger and update size
            append (w, u, v) to mst_edges
            n_components = n_components - 1
            if n_components == 1:
                break

    return mst_edges, n_components
```

## Complexity and Bottlenecks

### Time and Space Complexity
Kruskal's algorithm runs in $O(nlogn)$ time, where $n$ represents the number of cell clusters. It also uses $O(n^2)$ space, as it stores and sorts the edges.

### Expense Considerations
As a whole, this algorithm is computationally cheap and does not require much to change to account for performance, especially since Kruskal's algorithm is only being run using already grouped cell clusters rather than the large number of cells directly. Performance would only be a problem in datasets with an extremely large (100,000+) number of cells and a small number of cells-per-cluster are inputted. This is not the intended use case of the project, and therefore it does not warrant special consideration.

## Validation and Testing Plan

### Small validation example

Kruskal's tree-building steps on seven cluster centers in 2D, with node 0 as the root.

Data:
| Cluster | PC1 | PC2 |
| --- | --- | --- |
| 0 | 0 | 0 |
| 1 | 5 | 0 |
| 2 | 10 | 0 |
| 3 | 12 | 2 |
| 4 | 14 | 4 |
| 5 | 12 | -2 |
| 6 | 14 | -4 |

Kruskal's algorithm should process the edges in this order:

| Order | Edge | Weight | Result |
| --- | --- | --- | --- |
| 1 | 2–3 | 2.828 | Accept |
| 2 | 2–5 | 2.828 | Accept |
| 3 | 3–4 | 2.828 | Accept |
| 4 | 5–6 | 2.828 | Accept |
| 5 | 3–5 | 4.000 | Reject, as it would cause a loop |
| 6 | 0–1 | 5.000 | Accept |
| 7 | 1–2 | 5.000 | Accept |

Tree:

```text
                       4 (14, 4)
                     /
                   3 (12, 2)
                 /
0 ---- 1 ---- 2
(0,0)  (5,0)  (10,0)
                 \
                   5 (12, -2)
                     \
                       6 (14, -4)
```

### Testing with Synthetic Dataset

Simulated data will be used to test Kruskal's accuracy in predicting maturity of cells. The simulated data will begin as one trunk, which covers the first 40% of developmental time, then split into two branches. The data will be generated such that each cell's gene expression means depend on its true maturity and branch labels, adding random noise. True time and branch labels will be used only for scoring. Results will be scored based on predicted maturity accuracy, branch placement, and number of graphed lineages.

### Expected Results

I expect that Kruskal's algorithm will predict maturity and branch placement with over 90% accuracy on simulated data.

### Automated Tests

Unit tests for data preprocessing functions will be created. An end-to-end test using the above small example will be created to test the overall functionality of the pipeline.

## Updated Pitfall and Risk Log

In the project proposal, I identified a number of potential pitfalls in this project. Several of these have already been addressed in the design process. The following are the ones that still seem relevant at this stage:

1. In single-cell data, there can be zeroes listed in the data due to imperfect detection of molecules. This could affect distances between cells. In this project, cells will all be normalized by total count, avoiding excessively large distances from imperfect data.

2. The full cell dataset has more than two branches, including some cells that are contamination of the sample. I will detect and mitigate this by comparing the tree against given cluster labels to see if detected branches are real. The data preprocessing stage will be written to remove contaminated cells specifically for the Paul15 dataset.

3. The full dataset does not contain a label for true maturity order. Since all cells were collected at the same time, no cell has a known age. As stated in the validatin and testing plan, claims of correctness of cell maturity detection will only come from the simulated data, which will have ground truth labels. On the real data, I will only check the validity of branch placement of each cluster from known marker genes and author labels.