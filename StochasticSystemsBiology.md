## Session 3: Data Doesn't Come From God: Stochastic Systems Biology
### Learning from Large-Scale Biological Data
### Wednesday February 4, 2026
Look at a direct view of the cell and what is happening

**Single-Cell RNA Sequencing**r
Video talked about getting the gene x cell matrix from this and today's we're looking at this data and studying the mechanisms that generated this data.

What does our data even look like
- Let's plot one row
- Based on what we know, we can assume RNA-transcripts follow a Poisson distribution
- IS gene expression Poission distributed, usually PD has a mean and variance that you can't change. 
- we can look at all genes, get the mean and vairance of all genes and does it follow the distribution
- but when we plot it, at some point the data does not follow the PD, something's missing about capturing the relationship correctly --> this is called over dispersed the variance is higher than the mean, why? what other distrbutions might better capture this? what is happening mechanisticly?
- there are two key phrases and assumptions that must be violated 
    - process occurs at a mean rate
    - independent of the time since the last event

The Central Dogma - What is happening inside each cell?
- Measuring the amount of RNA in the cell 
    - k: a constant transcription rate
- Eventually that RNA has to be degraded
    - y: a constant degradation rate
- Can be measured as the Chemical Master Chemical Equation
    - n: amount
- This matches our intitial intution of PD
- Although this simple model tries to capture what is happening, it's not entirely correct

Next slide
- Let's make the equation not fixed
- K - stochastic rate of transcription - removes assumption about fixed rate of transcription
- What happens when we let it run to convergence?
    - It follows a negative binomial that let's the variance deviate from the mean
    - Negative binomial is more what happens in real life, similiar to PD by -1
- Let's look at a gene with over distribution

Negative binomial fits
- PD is not capturing 
- NB is fitting the gene count better and is a lot more accurate

Next slide with central dogma again
- Turns out we don't need to just change one assumption
- Bursty transcription - genes turn on and off and on and off, RNA is created in bursts
- k can be the burst frequency to capture an even more accurate view
- but this is violates the other assumtpion of independent --> won't follow PD
- Get's us the chemical master equation which gives a steady state of negative binomial distribtion
- Turns out considering the process, we can derive the same distribtion when we plot the raw data and can generate a distribition that looks like the actual read counts

**understand**
- unfortuanely this is more complicated
- i did skip a step, the mRNA transcripts need to processed and spliced prior to translation
- to capture reality better, let's capture splicing

nacsent unspliced inside the nucleus
mature spliced outside of the nucleus

splicing connects these two by removing introns
we want to understand how to separate nascent and mature, let's incorporate now

Incorporate Splicing
- still have constant transctiotion and degradation
- new - constand splicing rate 
- can be written as new chemical master equation
- what would this look like? simple PD because everything is constant
- not only are N and joint distribution is PD
- but still this isn't reality, let's up the stakes

Next slide
- K stochastic, keep constant splicing and degraded rate
- N here is splicing 
- k ~ K is nascent and B is mature
- both marginals M and N are NB
- but now, this is okay at the assumption of stochastic? but we know what is actually biologically happening: bursty transcription

Next slide
- B captures bursty 
- nothing seems to change, what can we expect? We no longer observe NB
- The Splicing part still is NB in k, but B introduces dependence
- the joint distribution becomes comlpicated --> complex 

Summary
- we have one species of one type of RNA --> NG - explains total RNA counts
- Consistent two species model capturing bursty and splicing --> complicated diagram, ongoing research --> next paper tries to use this to capture latent features
- Now we're going to look at a different part of this process: splicing, Nascent vs Mature RNA

Looking at Spliced vs Unspliced Counts
- We take our reads, seuqnenec them, align them, look at them to see if they are spliced (do they have introns or only exons)
- take a gene and look at one cell, is it going to tell us a biological process, we're going to try to understand this graph. These are colored by different cell types, different cells have different splicing ratios. what can we learn from this?

Dynamics of Splicing
- Transcription over time t broken down between splicing and degrading
- Diagram - 
purple - guess of transcription over time without splicing and degrading
blue - bursting happens, reach equiblireum flatten off, then transcription turns off
orange - splicing happens, over time degradation rate 
- we see different peaks and decrease in the rate after the peak, we can learn about time and where we are in the process
- get the slopes over time with derivates
- Diagram on the right - steady state - combine these slopes - increase in the beginnning - there's a burst of expression of creating more nascent than mature --> a prcoes is booting (re booting) up. later on - decrease in speed
    - in general, in genes partaking as housekeepng genes see an increase and decrease

Looking at Splied vs Unspliced Counts
- Now we can add these arrows by inference
    - Induced - increase
    - Repression - decrease
- optimistic take - using unspliced and spliced we infer dynamic processes, unspliced is a leading indicator of what will happen next in the future, this is amazing but this class is about pitfalls, what are the caveats? if you want to look more into this, more caveats in. paper

what is the right unspliced/spliced count?
- biology is more complicated than removing all the introns
- realistically, not all introns are taken out, different versions are created
- we can really get the counts about the genes just from using RNA, a lot of papers ignore this (unsplice/splice). 

Practical - Single-cell Data is very Noisy
- we had this plot of induction and repression arrows, now when we look at raw data
- we don't see those beatufiul curve, we can see when bursting happens, there's so much noise that we are capturing approximate vague snapshots.
- can't use this unspliced/splice counts to infer data, let's try to fix these signals
- k=50 - what happens when we average points with its nearest 50 neighbors
- well what is the right amount of neighbors, but these curves aren't the same
ELAVL4 50 shows a steady rate but 200 has both increase/decrease
- let's make an inference, if you don't smooth the data, you can't make an inference and rate. The inference changes based on the k. Smoothing is doing the inference over k
black captures the upregulated and downregulated
- transcription's degradation and splicing seem like a great use of data in theory but it't not ideal

Averaging velocities creates distortion
- Tumor or blood cells - you want to measure the directionaluty of the aggregation, how do we make these aggragation
- PCA - average velocity of each gene, points at the next cell at the next most highest mature becasue that's the diretion it'll be going into
- some of these arrows (right side) aren't the same between velocity calculation strategy. The aggragation isn't perfectly captured
If we try to do this in compressed UMAP/tSNE
    -  look at the directionality, we said last class tSNE is a pretty misleading, use the embedding, if you try to use the velocity, youll actually observe tota chaos of arrows. Mxing too many biological phenomens at once

Variable Cell Growth Rates
- this was just presented in Dec 2025
- found not only do we need to worry about the priblems of aggragation and unspliced/splied, this ratio actually correlates with the cell growth. 
- when the cell is growing in excess, as the cell grows, all velocities increase. but you get completelt different trends for fast and slwo growing cell. if we dont account for this, those curves from before , it may inflate 

SLide with Sequenicng counting infgerence and pretty graph with induction
We need to keep in account the caveats

Presentation Paper - biophysical modeling
- autocoder based model that tries to use this relationship (nascent/mature counts) to better encode single cell data

Reading/Viewing for next class - [gene regulatory network inference in the era of single cell multi omics - PauBadia-i-Mompel](https://www.nature.com/articles/s41576-023-00618-5)

- Read introduction, inference of GRNs, and downstreme GRN