## Session 5: Foundation Models
### Learning from Large-Scale Biological Data
### March 16, 2026

#### From Task-specific Models to Foundation Models - General
- Foundation Models - feed a lot of data, tries to understand the underlying connections. once trained, give it specific learning-based tasks, should perform better than sequence models
- Task-specific– Sequence Models - Take the labeled data, encode the data, feed into neural networks or CNNs to train a model on the data and labels, usually chromosomes are separated
- Foundation Models - Take the multimodal data. Be clever on how you encode the data. Tokenization is usually one of the hardest tasks, the tokenization depends on the mode to capture enough context. Pre-train for the model to build a foundation. Most we'll see are transformers. The key thing is that you need to think about how you're going to learn, how do you train if you don't label well. 
    - Can either mask tokens or predict/reconstruct the next tokens. Maybe predict a new modal of data or condition. You don't have all the data, you're auto-completing these sentences. 
- Once a pre-train model can autocomplete, fine tune the model to prediction task

#### Genomic Foundation Models
- If you can learn a lot about genomic sequences, you can make a lot of multi-modal predictions. If you train on a set of enhancers, should be able to make new predictions about enhancers. 
- The first one was DNABERT 2021 - pretain on genome sequences. Turns original sequences and turns into tokens of overlapping k-mers (usually 6-mers). Then random masking of these 6-mers and the model learned to fill in. Keep track of token embedding and positional embeddings, which represent where it came from. 
- This gives it a 'foundation' understanding of sequences.
- Had a list of fine-tune tasks: prediction of promoters, TF binding sites, and splice sites using labeled data. Outperformed these models that were trained just for that tasks.
- DNABERT 2 in 2024 - use byte pair encoding, more clever encoding
- Nucleotide Transformer 2025 - tokens are non-overlapping k-mers. Also masked some of these these non-overlapping k-mers. Then fine-tuned. 
- Claimed performance - TF binding sites figures, claim higher performance for human and non-human species than if the foundation model was trained on just the specific modal of data. Outperformed Enformer which was a leading tool. 
- Also compared with DeepSEA (a CNN) by plotting values. ~1% lower than achieved by DeepSEA. 
- Evo 2024 and Evo 2026 - tries to learn all of evolution, tries to understand how genomic atlas spanning all domains of life
    - used the same tokenization as BERT2. Rather than random masking, they trained on next token prediction (rather than masking). 
    - Zero-shot learning when fine-tuning, gives data without labels (no weights, of course has to give labels). Claims able to predict every modality of data. 

#### Genomic Foundation Models Benchmarked
- This year, someone benchmarked a couple of models on a bunch of genomic tasks such as regulatory elements (promoters, enhancers, TFBS), spatial chromatin interactions (3D genomic interactions) and effect on variance
- Enhancer-target gene prediction 
    - Expert Model - did Activity-by-Contact which is basically mutliplying two numbers of Hi-C and ATAC/H3K27c data
    - CNN such as DeepSEA
    - This multiplication model outperforms AUC over every other model. Not the most fair comparison though, expert uses two data, the others just use one.
- Contact map prediction
    - Expert Model is Akita
    - Predict contact of several different genomic areas
    - Akita outperforms the other foundation models. Not too sure what CNN model was used as Akita is a CNN also. 
- Regulatory sequence activity prediciton - standard prediction
    - Expert Model is Enformer and outperforms. This is funny because these papers claim to outperform Enformer, but not here
- Expression Quantative Trait Loci prediction - variance prediction
    - Expert Model is Enformer and outperforms. Enformer is a transformer-attention based and is trained directly on labeled data. 
- Showed that these expert models may outperform because it is trained on labeled data and can skip the 'foundation' step

#### Single-Cell Foundation Models
- This was a big thing announced on New York Times. They were talking about single-cell foundation model
- The goal is to learn and model single cell cell data
- Used cell atlas and train foundational model on cell atlas. Can it autocomplete on a new cell. 
- What can we predict? Self-supervised learning - Learn the grammar of cells. Cross modality prediction. Remove batch effects. Perturbation response prediction. Supervised learning - cell-type classification, gene network reconstruction, variant calling, disease classification.
- Think of cells as sentences of genes during gene embedding. At the end you get cell and gene embeddings. 
- Geneformer 2023 - Pretraining where genes are tokens on a piece of string. They ranked each gene's expression. Feed into 6x transformers. Masked 15% of the genes and model trained to predict which gene using context of the remaining unmasked genes. Cool result-- in silico gene deletion, by deleting gene embeddings, they can measure how much the embeddings in the vectors. Targets gene-specific have more an impact than general direct genes. Able to capture additive effect of transcription factor binnding. 
    - Fine tune for specific tasks such as cell type annotation - assign cell types, dosage senstivity of TF - how TFs respond to dosage genes. 
    - These models seem to have a lot of data leakage which we aren't going to go into
- scGPt 2024 - code genes and expression values, these expression counts are converted into bins. 
    - Used autoregressive pre-training. There isn't necessarily an order to sequences, did this over masking. 
    - Adds another token representing the cell's properties
    - These tokens can also be peaks from scATAC-seq data
- General Expression Transformer (GET) 2025 - cutting edge, first one to try to understand gene expression and these modalities.
    - Start with motifs of accessible regions. Mask. Fine-tunning to understand gene expression. 
    - Pretty remarkable job of predicting gene expression

#### Single-Cell Foundation Models Benchmarked
- Linear regression model is better at predicting cell types over fine-tuned models for PCMB data, fine-tune barely outperforms for others
- Foundation models do not outperofrm baseline cell embeddings
    - Evaluation tasks - cell type clustering, batch integration, pretraining objective
    - Cell type clustering - can't really find highly variable genes
    - Batch integration - should predict overall not just by its trained batch. A generalized approach (selecting highly variable genes) does better than foundation model
    - Pretraining objective - Objective is to fill in missing tokens by predicting level of expression. General appoach is average ranking of genes. Indistinguishable between general and Geneformer. What even is happening durnig pre-training?
- Foundation models do not outperform linear baselines for perturbation effect predictions
    1. Double perturbation prediction - fine-tune on two perturbations, but individual perturbation is part of fine-tuning
    - Additive model (adding the effects of two perturbations) is a general approach which outperforms these foundation models. 
    - Double knock outs of expected vs. observed. Additive also outperforms foundation models as well as deep learning models GEARS and CPA 
    - If we remove additive interactions, simply predicting 'no change' is best. 
    2. Single unseen perturbation prediction
    - Two baseline models mean and linear regression fit. Try to predict the effect of knocking out RPE1. Baseline outperforms foundation. 
#### Open Problems for Biological Foundation Models
- Biological data has more confounding factors than human generated data (human language, images, etc.)
- Biological questions are not mathematically defined. 
- These foundation models are jacks of all trades, but master of none
- Is there even enough training data out there? 
- How can we gain mechanistic insights? 
- Does it integrate prior knowledge?
- Agentic AI

Presentation paper scGPT

Two mini-quizzes

- Monday:
[Evaluating the advancements in protein language models for encoding strategies in protein function prediction: a comprehensive review](https://www.frontiersin.org/journals/bioengineering-and-biotechnology/articles/10.3389/fbioe.2025.1506508/full)
- Wednesday: [From sequencing data to gene functions: co-functional network approaches](https://www.tandfonline.com/doi/full/10.1080/19768354.2017.1284156#abstract)

