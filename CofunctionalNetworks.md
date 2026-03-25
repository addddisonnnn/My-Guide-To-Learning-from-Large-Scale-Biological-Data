## Session : Learning Functional Networks
### Learning from Large-Scale Biological Data
### Wednesday March 25, 2026

#### Functional Genomics
- Key concept - whether or not and is it possible to integrate diverse data to learn the functions and interactions of genes and proteins? 
- Main figure from the paper - there's three key stages
    - Inference - need to represent the relationships of data, do they occur across species, near each other, have simliar domains/homology, and use all types of measurements and experiments' results
    - Evaluation - we don't know for sure the relevance of these links and genes and functions. We want gold-standard links of genes that actually have a function and are in effect with each other
    - Integration - take diverse and confident data and apply it through a genome-scale co-functional networks
- Applications
    - Network connectivity - is a gene a central network or associated with particular functions or are they DEGs
    - Network propagation - how do these functions correlate with other genes
    - Subnetwork analysis - learn something new based on known associations
- Integration step is actually pretty complicated, the paper only talked about using Baysian integration
#### Naive Bayesian Data Integration
- Naive because we assume the data and it's features is independent.
- We're trying to learn how two genes are functionally related - their gene expression should be correlated, have dosage and synthetic lethality, etc.
- We want to calculate the probability of an outcome based on its feature. The outcome is whether or not they're related. The features are the features of the datasets.
- Use the prior probabiltiy of the outcome normalized (Bayes' Theorem). We don't know how all the features and meaturements will lead to potential outcomes. We can use this theorem because the features are independent
- Proportional because we ignore normalizer
- Prior - intiial belief two genes are related
- Likelihood - what we want to learn, probability of likelhood of the two genes being related. 
- How can we calculate this? This is where the gold-standard annoataions come in as they're extremely validated. Then we can count how often these features appear for those annotations. 
- But we know this data isn't always independent. We want to factor in the dependencies
#### Bayesian Network
- Consider the dependency structure of this data. The figure is a bayesian network where one node leads to different independent outcomes
- Next figure - nodes dependent on previous nodes such a sthe physical assocations of PPi can lead to affinity precipation, two hybrid. 
- We have measureable and unmeasureable variables as we have missing data that needs to be use for these networks that are dependent on the previous nodes.
#### Details of a Simple Bayesian Network
- Example - there are three variables - whether it is raining, sprinking is on and if the grass is wet. The grass can be wet from the sprinkler and the rain and the sprinkler is usually off when it is raining. The sprinkler depends on the rain. The grass wet depends on the sprinkler and the rain. 
- Something important for these bayesian networks, there are no cycles ~ accylic
- We need conditional probabilities tables for each variable which tell you the probability of each condition based on the probabilty. When it is raining and sprinkler is off. With a full bayesian network, we can ask questions directly about the outcomes. 
    - What is the probabiltity that is raining, given the grass is wet?
- Equation of the full network, usually we need to do a full joint expansion but here we don't. 
- Need to take the sum of all the probabilities of conditions (given) to get the probabiliity of the outcome. Sum when it is raining and grass is wet from each table. Calculated a 35%. In real life, we'd be summing a lot more. 
- There are two reasons we're able to calculate this: 
- We have all the conditional probabilities and there are only a handful of variables. 
    - Inferring unobserved variables - this is NP-complete - can't get the exact probability of the outcome as we use approximated conditional probabilities. usually solved with stochastic inference.
- The second thing is that we have these probability tables
    - Parameter learning - using maxiumum likelihood if possible, using Expectation Maximumization (EM) otherwise
- One thing I haven't talked about is how we figure out directions of arrows - usually by asking an expert, a biologist
#### Bayesian Data Integration for Functional Genomics
- Use network to figure out if two genes are functionally related
- All the nodes at the bottom are observed variables. The ones towards the top are unobserved variables such as coexpression. There are unobserved variables that aren't too important that aren't considered such as DataNoise Level. 
- Gold standard - GO term annotation that biologists annotated for if two genes are a part of the same biochemical pathway.
- Thanks to this tree-like structure - we're able to show this more simply than the drought example. Each variable encodes whether a gene i has the given relationship j.
#### Bayesian Data Integration for Functional Genomics p2
- Bayeysian probability is more accurate at learning conditional probabilities than experts. Turns out that this performs the same (of considering dependency structure) as a Naive Bayesian network. And experts are better at assigning naive probabilities than complex chains of probabilities. Going forward we're going to learn more towards Naive. 
#### Context-sensitive Integration
- Want to understand when specific conditions happen. We don't know if the're necessarily related uner all conditions. We want to learn the distance. In 2007, it was proposed we keep Bayesian integration to predict two things: Biological context and Functional relationship. Basically, this is a Naive bayes with two dependencies
- For Functional relationship - use GO term annotation for gold standard.
- For Biological context - need one specific to each biological context. Takes GO term annotations and checks for pairwise pairs: positive if the two genes are in the same GO term. Negative pairs is harder. Take the negatives from the non-positive pairs, and at least one of the pairs has to be annotated.
- Learn gene relationships in specific contexts using Naive bayesian integration. People didn't trust just using Naive bayesian integration, what's the best way?
#### Bayesian Regularization 
- Use prior knowledge and experimental data to perform bayesian data integration. 
- Before Naive Bayesian worked so well becasue of how sparse the data was, now we're getting so much data every day and the correlation structure is starting to matter more, but it's impossible to keep in mind all data
- Use experimental data for pairwise mutual information to see which datasets are capturing the same functional relationships. 
- Calculate mutual information - shows how related the datasets are. Another metric is the dataset entropy - how much total information the dataset contains. Use these two variables to hack and scale the ouctomes. 
- Take the previous Naive bayesian equation from before written differently and adjust the likelihood. Concept to regularize the likelihood where alpha is the sum of MI for all other datasets over the entropy of k. This hack shows how related datasets are to each other as there's so much information. Performance grows. 
#### Tissue-specific Networks
- Want to learn how gene behave in specific tissues
- MouseMAP 2012 - prior knowledge annotations and data. Perform MI and bayesian integration.
- Gold standard - need one specific to each tissue. The problem with gold standard is that a small % of data is validated and are true positives. Positive pairs must be co-functional and must be expressed in the same tissue of interest. 
- We can ask specific questions about tissues. See which genes are cofunctional in specific tissues interactions as genes behave differenly in different tissues as well as tissue and disease specific interactions of genes behaving in a diseased individual. 
- GIANT and NetWAS (2015) - function and disease with human tissue-specific network
#### Enhancer Networks
- So far, we've only looked into genes, but what about the cofunction of enhancers. Datasets of enhancer-enhancer and enhancer-TF interactions. Took these two variables and built a tissue-specific enhancer functional network. But we don't have a gold standard. They hack it
- Calculate the total probability in another way. Try to learn enhancer-enhancer interaction and enhancer-gene interaction in the specific tissue. See if there's gene-gene function score, TF cobinding, and chromatin interaction in general (not tissue specific). 
- Able to hack this as V is a two-layer network. Expand this Bayesian example to all conditional probabilities but remove conditional dependent, you get a manageable (six) number of terms that are used to calculate the learn and train the model.
- Now use MCMC to add or remove edges. To calculate the prob of contact given an edge, sum them up (Poisson). Gaussian by summing the Peason's correlations of binding events for cotranscriptional binding sites. 
#### An 'AI Platform for Human Biology'?
- These trained models should make heuristic predictions of the human body in the future. 