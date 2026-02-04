## Session 1: The "Art" of Single-cell Genomics
### Learning from Large-Scale Biological Data
### Monday February 2, 2026
We'll be delving into the common pitfalls of mahcine learning in gneomics, scenairos of this and how to circumvent these setbacks. 

**Quantification Through High Throughput Sequencing**
Biollogical information is sequenced through a mahcine and then analyzed. 
- It has to be annotated and aligned. Once you've performed alignment, perform quanitification to see level of expression. You'll get a vector and some number of how expressed the gene was in the sample. 

Single-cell Genomics
- When you're sample is one cell. In 2009 saw all the expression of one cell. 
- slowlly over time the technology has really progressed over time

Pipeline
take timor sample, all all individual cell, get the reads with barcodes so you get the read counts --> be able to see how many genes came from that cell. theres a limit to the number of reads that are counted. not able to see every gene from every cell. There's analyses to do this. 
- It can be hard to see data in 20 dimensions. PCA helps this by decreasing the amount of dimensions in a lower feature space. PCA is a linear transform but it isn't the most helpful for this as there isn't a non-linear 
- here we'll look into techniques for non-linear dimensionality reduction to help learn from our cells

**Non-linear dimensionality reduction**
UMAP - reduced to 2 dimension. What ddo people usually look at: clustering. You expect space in between the cluster and label the clusters and see how they relate to each other. You'll see today's lecture, there's way to extract this information and ways you can't learn about the cells 

4 applications - things that are usually looked at.
1. see two data sets and how well theyre integrated
2. expect two clusters to be close or very separarted/far
3. see how genes are expressed
4. see the continous relationship

There needs to be a seubset of proprties wen these features are generated. When looking at a cell, local: look at the nearest neighbors preserved, global: groups of cells have properties preserved, distance
- green diamond - absolutely needed

We will go thorugh this chart 

**Local Distortion**
few embeddings create local distortions. 
- first graph - run PCA as soon as possible, t-SNE and UMAP reduced in two dimensions
x axic jaccard distance between neighbors that are similiar. you want low numbers
- in the UMAP and t-SNE, we see the neighbors don't seem close at all --> shows distortion immeaditely about the nearest neighbors.

**Global Distorion**
we're going to look at how cells in one cluster in nearby clusters.
- looking at nearby, the diifferences aren't even there

**Distance** took triplets of cell that are all equa-distance vs. near and vs far and saw how these triplets saw in UMAP. Space is not conserved at all. No information is captured of these triplets of equal size. 

Shows: compressig isn't conserving neighbor information, global information, distance isn't captured

**Modality**
- first mistake people make. they may perform batch correction and integrate the data. if perfectly batch corrected should be 0.5, half the data came from one half and other.
    - umap orange: take the nearest neighbors are misleading and isn't fully captured

second graph: if you take the raw data and embedded, the embeded doesn't show the underlying features

next two graphs - run code for batch correction

examples show not to trust when performing batch corrected even though the umap looks great.

**Cluster**
scientist may infer this cluster is either close or far.
- accuracy drops of what is predicted vs. actual
- 10 vectors: umap shows no separation. Ambient shows actual separation

**Density**
They may claim they can look at the density of things and get meanings, for exmaple justify changes
- look at the density of cells between condition 1 and 2. May infer there is equal density for two and different density for three, claim difference in conditions. 
- if you replot the umap and change perameters, relationship 3 now looks the same densitiy and different density for relatoonship two

**Trajectoy** 
May imply pseduo time or the progression of cells over time. here are four different ways to embed data. by looking at the neighbors, predict the flow neighbors. Each has its own completely different relatiionships. Changing the parameters, you see different directions and different groups. create distortions of tracking in space.

Look into chari and pachter

**Picasso** https://github.com/pachterlab/picasso

Keeps shape in mind
- keep in mind an elephant. if you check how well it is corelated, it actually performs better than t-SNE and UMAP. The genometry that comes from low embeddings isn't actually meanigful

This is all just to say, UMAP and t-SNE are performed and it is common to make assumptions for 2 dimensional data. 

Today's Lecture
https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1011288

Required viewing and mini quiz. Optional paper that is too much information. 

https://www.cell.com/cell-systems/fulltext/S2405-4712(23)00244-2?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS2405471223002442%3Fshowall%3Dtrue
https://www.youtube.com/watch?v=vsNDxmVMXKQ