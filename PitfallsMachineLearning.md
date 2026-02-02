## Session 1: Pitfalls of Machine Learning in Genomics
### Learning from Large-Scale Biological Data
### January 21, 2026
We'll be delving into the common pitfalls of mahcine learning in gneomics, scenairos of this and how to circumvent these setbacks. 

**I. The Five Pitfalls**

1. Distributional Differences

2. Dependent Analysis

3. Confounding

4. Leaky Pre-processing

5. Unbalanced Classes


### Pitfall 1: Distributional Differences

**What is happening?**
- Training set: Examples and associated outcomes used to fit a supervised machine learning models
- Test set: examples and associated outcomes used to evaluate model performance
    - Training and test sets are disjoint
- Prediction set: A third set of examples whose associated outcomes are truly not known, where a fitted model is applied to make predictions
- Cross-validation: 
    - Estimates a supervised (ML) model performance on held-out data, examples are randomly split into k groups, known as folds. 
    - The model is fit on k−1 groups (the training set) and then applied to the remaining group (the test set). 
    - By comparing predictions with actual outcomes across many random test sets, model performance on unseen data can be estimated.


**Defintion & Examples of Distributional Differences**

- Distributional differences occur when examples are not identitcally distributed
    - The probability of observing a given value is not the same across examples
- Example of identically distributed - repeated coin tosses where the probability of H and T is the same
- Example of not identically distributed - online search results

**What happens when the training and test sets have one distribution, and the prediction set has a different distribution?**

You would expect that performance will be higher in cross-validation (same setting) than on the prediction set (different setting)

- Because the prediction and test sets have distributional differences, the relationships between features and outcomes that are learned during model fitting may not hold in the prediction setting
- Generalization Error: how accurately a model predicts outcomes in data it has never seen before

**Batch Effects**

One common source of distributional differences is batch effects
- If the training and test sets are a mixture of examples from every batch, performance on the testing set will be much higher than on a new batch
- To fit a model that will generalize to new batches, training and test sets should be composed of different batches

**Case study**: Model the sequence preferences of DNA-binding proteins

- Key biases:
    - which sequences are assayed
    - which other proteins, if any, are present during the experiment

- The observed distribution of bound sequences for a given transcription factor is different across assays

**Weirauch Example**

Weirauch et. al (Nat Biotech 2013) evaluated models trained to identify sequence preference. They observe:
1. A model trained on PBMs achieves an AUC 0.4 higher in cross-validation than it does on in vivo ChIP–seq data
    - They note that the reverse is also true so it’s not that either type of data is superior; they are simply different.
2. Performance also drops when transcription factor binding models are applied to make predictions on a different species from the one on which they were trained.
3. The performance differences are not inherently bad! Analyzing the sources of differential performance can reveal interesting biological differences between the settings, such as the presence of cofactors or cooperativity in ChIP–seq that is absent from PBM data.

**Potential Reasons**

With biological data, distributional differences arise for many reasons (you will want to look out for these through
the rest of the semester!) 

1. Batch effects
    - Study design and technical factors such as time, place, personnel, reagents, or instruments 
    - Hospital-based biases have been observed in medical images and electronic health records

2. Model is trained and applied in different biological contexts
    - Different cell types, species, or assays, or in vitro versus *in vivo*
    - Epigenetic profiles differ between euchromatin and heterochromatin
    - Proteins belong to functional categories, each with distinct expression patterns and physical interactions.
    - Population genetic structure creates distributional differences known as “ascertainment bias”: models are trained on individuals with different ancestry from the population being tested
    - Drug repurposing models do not perform well on new drugs or rare diseases where distributions differ from databases on which the models were trained.
- Most large biological datasets have some distributional differences from these various sources!

**So, what can be done? Assess Feature Distributions**

Ideally, the marginal distributions of both outcomes and features should be examined... But the outcome is typically unknown in the prediction setting, so one can only assess feature distributions.

- Approach 1: Visualization, either by projecting the data into two dimensions and making scatter plots or by comparing
distributions of feature values 

- Approach 2: Statistical tests to detect when distributions differ (e.g., binomial test for binary features, Kolmogorov–Smirnov test for univariate continuous features, or Maximum Mean Discrepancy for multivariate continuous features)
- Approach 3: Model-based techniques for outlier and anomaly detection

**So, what can be done? Accounting for Differences**

Accounting for Differences, many approaches including:
- Batch correction such as mean correction, quantile normalization, and empirical Bayes adjustment for measured variables
- Surrogate variable analysis - estimating and correcting for unknown sources of noise
- Canonical correlation analysis - identify common patterns across batches
- Adversarial learning - model trained to predict the dataset that each example came from can be used to generate penalties for the primary prediction task
- Note that sometimes performing these corrections can inadvertantly cause information to leark between training and test splits in cross-validation (Pitfall 4)
- Accounting for distributional differences is still an area of open research

### Pitfall 2: Dependent Examples

**Defintiion**

Dependent Examples occur when examples are not independent ~ the values of one example depends on another example
- Independent Example: Repeated coin tosses ~ the probability of H and T is the same each time
- Dependent Example: Repeated draws from a card deck without replacing the drawn card (the probability of the next card depends on what has already been drawn)
- Failing to account for dependencies between examples can lead to biased models and overly optimisitc estimate of model performance
    - Note: randomized cross-validation does not protect against this problem and overestimates performance

Dependent Examples
- Note: randomized cross-validation does not protect against this problem and overestimates performance, **but why?**
    - Because examples in the test set can be correlated with training examples and bring information into the test set that should not be there
- Generate random bipartitie graphs with ranodm node vectors
    - Each node contains a vector of features. Each edge indicates an interaction
    - The Goal is to predict interactions. We encode edges by concatenating the features of the node paits that define them
    - To estimate model performance, edges are randomly split into training and test sets
- Measure the auPR Curve
    - Baseline - random guessing, accurate assessment, because there is no relationship between features and the outcome in the data
    - However, ML models Random Forest and Logistic Regression are higher than 
    - Things like this happen all the time in the real world

**Case Study 2**: Predicting 3D enhancer-gene interactions
- H-C measures 3D contact
- The examples are enhancer-gene pairs, whcih are labelled as physically interacting or not, using Hi-C data
- Enhancers allow for context specific gene expression
- Key issue: Many enhancer interact with the same gene
    - Humans have approximately 20K genes but 100K-1M enhancers

**Whalen's Proposed Solutions**

- Based on the biology
    - Genomes are split into chromosomes across which enhancer-gene interactions (almost) never occur
    - Solution: Block testing and training by chromosome (e.g., train on chromosome 1 data and test on chromosome 2 data)
    - Downside: There is chromosome-specific biology that'll be completely discarded (i.e., this blocking is very coarse)

- Purely computational
    - Solution: Ignore enhancer and gene locations, instead make pairs of genomic windows the examples
    - Downside: While this will it will greatly reduce the number of times each gene is included it will still potentially include dependent examples

- In both cases, performance was worse in training, but the model should be more generalizable
- Both models picked out the same significant features

**Consider biological structure case by case**

In biological data, due to the interconnected and at times, redundant nature of biological systems, dependence is **pervasive** and unfortunately, hard to recognize
- Like Distributional Differences, we must consider **biological structure case by case** (which requires domain knowledge)

Interactions
- protein-protein
- enhancer-promoter
- regulator-gene
- drug-protein

Genotyping
- genomic loci are in linkage disequalibrium which creates a dependency between variants at those positions
- The independence and identical distribution assumptions are entangled, for example genotyping results for family members are dependent and also may differ distributionally from other families

Predicting Function
- functional activity measurements from several cell types or tissues, genomic loci themselves are dependent
across samples because the underlying functional activity is generally shared
- family of a protein is predictive of its function, and so measuring true generalization performance of a
computational method may require ensuring that a protein and its entire family fall on the same side of the
train–test split.

Dependence relationships are not always known and even known dependencies have a tendency to be ignored

**So, what can be done?**

1. Identify

There is no magic bullet; you will need to try to explicitly consider the underlying dependencies in your data
One intuitive way of doing this is to visualize the dependencies as a graph
- nodes represent biological entities (e.g., genes, proteins, regulatory elements or chemicals) 
- edges represent associations or interactions between nodes (e.g., Genomic proximity, protein complexes, transcriptional networks and metabolic pathways 

What to look for:
- directly — and even indirectly — connected nodes are dependent, as are edges that share a node.
- Nodes with many edges (high degree) create groups of correlated nodes

2. Mitigate
- Approach 1 Blocking (put all dependent examples in the same fold of test/train split)
    - does not reduce dependence but it does not prevent inflated performance
- Approach 2: Downsampling/downweighing (don’t include all edges for high degree nodes)
    - can make the modified data biologically unrealistics
- Approach 3: Explicitly model the covariance between examples (e.g., mixed effects models, time-series models and autocorrelation models)
    - may not scale to large genomic datasets
- Approach 4: Reformulate the problem to exclude problematic features
- Approach 5: Acknowledge dependence and mitigate overfitting at the model evaluation stage

### Pitfall 3: Confounding

**Definition**
- Recall: “Dependent Examples” occur when examples are not independent, where the values of one example depends on another example
- “Confounding” occurs when an unmeasured variable (~confounder) induces dependence between features and the outcome
    - Example: Do ice cream sales lead to shark attacks? Temperature is the reason why
- The error is that the confounder is not measured or not thought to be important and is therefore not included in the model
    - cross-validation does not protect against confounded effects, because the confounding is present in both the training and test sets

Note: Confounders may have little or no effect on the accuracy of predictions, but they leads to incorrect interpretations
of the learned feature–outcome relationships and poor performance when the model is applied in a new context in
which the confounder is absent or is distributed differently than in the original context.

**Case study**: Predicting histone modifications
- H3K27ac (acetylation of the lysine at position 27 of the 3rd histone) makes DNA more accessible and is a marker of active enhancers 
- H3K27ac can be measured with ChIP-seq
- One key feature of ChIP-seq (and all sequencing-based assays) is the
number of reads sequenced (“read depth”~the average number of times a nucleotide is sequenced in a genomic region)
- The same library can be sequenced to different depths, but the
depth is not a biological signal
- The effect of sequencing depth
on peak height is nonlinear

Due to this:

1. Applying a ML model trained on data with one sequencing depth to a prediction context with a different depth will result in systematic misprediction of signal values.
2. ML models that aim to predict peaks or learn features enriched in significant peaks would be biased
when applied to data with a different sequencing depth if they did not account for the confounding.

Pitfall 5: Unbalanced Classes

- if you have a task where you have the same number of negatives and positives, you'll learn both. But if you learn more of negatives, you'll learn more about negatives. Not reallly a problem with learning/training but more about the evaluation. 

- Problem with unbalance between test and prediction. You want your prediction to reflect the test or reality.
- One side of your coin, you want prediction/test to be balanced. The other side, how do you even look at tests to see if something is useful in the prediciton

1. AUC or auROC - closer to one is better
    TPR vs FPS
2. Precision vs Recall
fraction of all your positives that are tru. 
Fractionof all positives recalled (same as TPR), how many of them did you get
-   For auROC - problem due to lots of negatives. This isn't seen for auPR. The difference is seen in the demonminator.

**So, what can be done?**

The key is rebalancing. Three types
1. Oversampling the minority class- duplicate existing data or creating exmaples, get more of the plausible
2. Undersampling majority
3. Weigh your samples - use the inverse of the proportion. The model will be trained to put more emphasis on minority
- oversampling con - bigger data ~ problem
- undersampling con - data is valuable
- weights - new hyper-parameter - pain in the butt - can use your domain knowledge, but this can be difficult to accomplish
These approaches should improve performance, assumed you want to improve rebalancing. 

**Biology Examples**
It is pretty common for there to be tons of negatives in biology. Assumed that the data is unbalanced. look into the non-coding genome. Not a lot of it is doing anything to the predicition set. 

- measure peaks or fractions of the genome. The non-coding represent a lot of negatives. 
- no matter what you're doing, this should be done inside cross validation or you might be leaking data.

- when annotatig proteins, most of their functions are unknown most are negative. need to think of some way to deal with this. 

- drug targets on the other hand are mostly positive

Come up with your example - Predicting protein function
1. Distributional differences - New and old technology
2. Dependent - familiies
3. confounding - new and old technology such as mass spectometry from ten years ago and today or even with different company's mass spect or procedures
4. Leaky preprocessing - Processing the test and train the same
5. most are negative 
