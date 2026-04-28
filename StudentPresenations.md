## Session : Current Research
### Learning from Large-Scale Biological Data
### Wednesday April 22, 2026

### Team 8 - Squidiff
#### Background
- scRNAseq changed how we study cellular heterogeneity and how cells respond to environmental changes, mapping these changes are challenges, but most are designed for specific tasks rather than broad range of scenarios. Require perturbed and unperturbed data as inputs without leveraging underlying biological knowledge
#### Method
- Semantic encoder to form semantic variable z. This and gaussian noise goes through a conditional DDIM. Reconstructs cell x gene matrix
- Interpolation
- Predict cell differentiation and transdifferation
#### Reproduction
- Figure 2 - want to recrate trans intermediate day 1 and 2
- Predicted vs ground truth
- Marker dot plot
- DE gene heatmap - top 50 genes
#### Reproduction on Cerebellar Tumor Data
- Trained Squidiff on mouse hindbrain postnatal samples
- Maps development lineages using single-cell transcriptomic spatial mapping
- No clear separation on day 0
#### Critiques
- Asumes linearity in its semantic latent variables
- May break down in highly complex biological scenarios
#### Extension 1: Activation Functions
- A way to capture gene interactions is to interact
- Bottlenecks we encountered: convergence, nonlinear activations take more epochs and longer to converge, took an extraordinarily long time with many incompatibilities
#### Extension 1: Spherical Linear Interpolation (SLERP)
- Usually would measure gene expression in a linear line, but in reality real life, we see curves
- Follow an arch along the sphere, uses trig weighting to calculate angles betwen angles
- Replaces the interpolation function in the Squidiff sampling model
- Improved pearson correlation between ground truth and predicted

### Team 2 - Exploring the Dark Proteome: Structure-based clustering and discovery of lecting families at scale

#### Introduction
- Massive growth in protein sequence data, most proteins' functions are unannotated ~ dark proteome
- Traditional methods - rely on sequence similarity but falls apart with homologs
- Can we use structure to identify specific types of proteins that sequence-based methods miss?
- Lectins are proteins that bind to glycans - cell surface sugars
    - host-pathogen interaction and immune recognition
- Challenge: most lectins often share structure but not sequence
- Why structure-based methods matter? sequence based methods miss remote homologs while structure based capture remote clusters
#### Methods
- Perform data reduction - filtering 
- Sequence similarity method - all-vs-all MMseqs2, graph where nodes are proteins and edges are similarity, keep top 4 edges per node
- Candidate selection - identify lectins per cluster and filter unknown proteins
- Structural clustering - align structural sequences, compute similarity scores, group proteins in structure space, and look for unknown clustering with lectins
- Structural validation
#### Reproduction
- Figure 2a large scale similarity sequencing - representative network using Cosmograph 
- Downselection for mixed clusters
- Cluster 1 - 51 lectins and 21 dark proteins
- Validation on multifamily dataset

#### Critiques
- Original method requried massive datasets to reproduce

#### Extension
- Find a more meaningful stratgey for sequence cluster downselection
- Re-cluster after downselection
- Use a new structure hit ranking strategy

### Team 4 - CA-CAE - Deep learning
#### Introduction
- Cancer subtype identification is crucial for personalized treatment
- CA-CAE - convolutional autoencoder and channel attention that learns cancer subtypes from multi-omcis data

### Team 1 - CellMentor
#### Introduction
- Clustering and annotation is rich in information but it's massive, sparse, and difficult to interpret raw, clustering divides cell types based on common gene expression levels
- Existing clustering methods - k-means, hierarchical, graph-based, density-based
- CellMentor integrates cell type labels into its dimensionality rediction and utilizes non-negative matrix factorization to determine the most criticial cell types

#### Methods
- CellMentor - reference is decomposed, adds supervision by penalizing within class scatter and rewards between class separation. Learns the features of the cell types and projects this onto a query dataset
#### Extension
#### Results
- Reproducing paper figures not all the code is available
- True and predicted cell type UMAPs
- 6.7% of cells were flagges as potentially novel
- Data processing and clustering
- Cancer fibroblast dataset - fibroblast are a structure within cancers, focused on breast cancer