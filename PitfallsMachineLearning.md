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