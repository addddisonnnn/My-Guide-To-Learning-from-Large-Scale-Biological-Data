## Presentation 6: SPLASH: A statistical, reference-free genomic algorithm unifies biological discovery
### Learning from Large-Scale Biological Data
### Monday April 6, 2026

### Introduction
- reference genome - digital DNA sequence assembled as a representative example of a species' genome, provides coordinate system for mapping reads, comparing results, and communicating genomic positions
    - applications - de novo assembly, need a comomon frame for comparison
    - limitations - incomplete / missing reference, poor handling of repears and paralogs, dynamic genomes, statistical problems
#### Introduction SPLASH
- Statistically Primary Alignment Agnostiic Sequence Homing - k-mer based algorithm directly analyzes raw sequencing reads to detect sample-specific sequence variation
- Ancor - short, constant k-mer + Target
- applications - works without reference, kmerse capture variation
- discovereis in viral evolution, single-cell regulation, adaptive immune receptors, non-model organisms
### Dataset
- SARS-CoV-2 - viral genomes have high mutation rates
- Smart-Seq2 from human lung cell atlas, major histocompability complex ~ cell by gene matrix
- Immune single-cell RNA-seq dataset - human samples and lemur samples (FASTQ)
- Non-model organism data - octopus and eelgrass samples - bulk RNA-seq

### Computational Methodology
- Main goal is to use not use reference or deep learning - this is a pure statistics approach
- First step is Anchor + Target extraction - identify anchors from samples of a specific length (27-mers) and then you identify downstream targets, count how many times an anchor and its target appears 
- Significance testing - p-values between anchor and targets' variation
- Output - get its effect size and creates a consensus sequences
#### Anchor and Target Extraction
- get every anchor-target pair - maximize the optimal distance from anchor to targets
- assemble contigency table per anchor - columns are each sample and rows are targets, see how many targets per sample
- filter - remove anchors with 1 unique targets, 1 sample, < 30 counts, and sample pairs with < 6 counts
#### Testing for Significant Samples
- Null - row (target) counts are drawn from the same distribution for all samples, test statistic
- did this to not require permuations, allows for closed form p-value bounds
- sj gives each sample's count in total population vs individual, sum up these sj to get s
- f is a binary function over targets, block out certain and include certain counts to prevent overfitting
- want to calculate a p-value bound
    - there's this theorem called Hofkins theorem
- Type 1 error control with bonferroni correction - prevent false positives
- FDR control - benhamini-yekutieli applied - done because there are still a lot of p-values per anchor
- K by L computations and choose the most optimal p-value bound per anchor

### Output
#### Output - Anchor Effect Size
- cj assignment indicates best sample split into groups A+ and A-
- effect size computes variational difference between the two groups - checks if the difference is large enough
#### Output - Consensus Sequence
- per sample consensus sequence is built for the sequence downsream of each significantly variant anchor
- gets the most common base at each position and a confidence score is computed
- allows for biological insights once significant anchors are identified: annotations

### Validation
#### Splash identifies stain-defining and other mutations in SARA-CoV-2 de novo
- even without prior knowledge, able to detect targets of variations and strains in the spike proteins and mutations that are not included in the current definition of the strains
#### Examining coding potential
- protein coding - translate the consensus sequences to amino acids and matches to Pfam protein domain models
- profiles are frequently associated with significant SPLASH anchors, compared with control
#### Rotavirus and overall findings
- Show variation between protein domains and see how they interact in the immune system
#### SPLASH identifies regulated expression of paralogs and HLA in single cell RNA-seq
- Have regulated splicing 
#### MYK12A and MYL12B
- Marks the anchor and two targets for individuals P2 and P3
- Little is known, but show DE and evolutionary conservation
#### Detection of HLA overlap in head-to-head arrangement
- Saw anchors that overlaps at splice junction regions - haplotype specific
#### Detection of allele-specific expression in T cells
#### V(D)J recombination is highly diverse and difficult to capture with reference-based models
- hard to study rearranged segments of sequences are not captured in reference
#### Reference-free Protein Profiling identifies immune recpetors
- Map anchors back to genes
- B cells are strongest, T cells, V sets, signals are not seen in controls

### Pitfalls
- Pseudoreplication, Confounding, no explicit batch correction

### Limitations
1. Cannot distinguish biological mechanism
2. Still depends on references for annotation
3. Low-count anchors are discarded