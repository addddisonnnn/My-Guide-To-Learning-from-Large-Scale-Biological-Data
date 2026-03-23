## Session 4: Protein Language Models
### Learning from Large-Scale Biological Data
### Monday March 23, 2026

#### From Genome to Proteome
- Intermediate steps from genes to proteins such as transcription to form mRNA which is then translated into a protein sequence of twenty amino acids which is then folded up into proteins. If this is such a defined process, why hasn't it been easier to computational simulate structural protein prediction? We'll get a foundational understadning of protein sequence of these amino acids, fine tune this for predictive tasks. 

#### A Brief History of Protein Function Prediction
- People have been studying proteins and their function since 1875. We've matched sequenced of proteins before DNA matching. Then predictions of proteins have been formalized and in 2019, it has been heavily researched and developed. 

#### Protein Language is Hierarchally Structured
- There are more words in proteins (20 amino acids) compared to four based in genes, these 20 words reflect more closely to the alphabet. The folds of alpha and beta sheets form some grammar in secondary sturcture. Tertiary is the sentence and quaternary is the combination of these sentences. 

#### Protein language supports many downstream tasks
- Predict the effect of a mutation
- Fine tuned models useful to understand and predict structure. A branch under that is to understand the underlying function, molecular, and biological process. 
- Can utilize these models more as proteins are more applicable. 

#### Pre-Transformers pLMs
- LSTM's were the model used prior to deep learning. UniRep 2019 performed auto-regressive task of just using previous tokens. Treat each AA as a token. 
- This is the whole structure of the model - really important dataset called UniRef50 with 24 million organsims. Think of amino acids as semi-independent codes for proteins. DNA sequences need to account for the entire genomes. So we have more data to work with in protein universe. Proteins are a universal language across animals. The UniRef50 is a representative dataset to present a diverse set of proteins, instead of being biased.
- Each amino acid gets a score. They average all the representations of the learned sequences. Using the encoding only, see how proteins are together and their distances between each other. Can it predict secondary structures and the stability and function. These are three specific tasks. The semantic similiarity is different. 
- Able to gather universal information. Keeps zooming into the context and can they look at the similiarity between close by and local proteins. If they just look at global context, can't make predictions. If fine-tune to learn the small set of proteins (engineering context) evotuned model, the original wasn't too good but the finetuned model did better and if you evotune on a random dataset over the global context, the evotuned random is better than a finetuned global model. This is 2019, hopefully things get better.

#### Pre-Transformers pLMs
- Soft Symmetric Alignmen (SSA) - A lab at MIT 2019, was thinking more about feature embeddings and downstream tasks. A hard task is aligning and if a model can understand the language of proteins, it should be able to align well. It did predict masked amino acids bidirectional. Not interested in the language model itself, more interested in protein aligning. 
- Need to learn two things: the context, once sequence is folded, what is in contact? The other is . Now they have two extracted features, which are used for pairwise comparison. And get's the most-likely route (alignment) to get the predicted similiarity score. And using the observed score, it got a similiarity loss score. 
- What's the performance - embedded of different k-mers and of different models. 
    - SSA (no contact prediction, no LM) - don't find the two feature extractions. Using ME, UA, SSA, and SSA (full), able to get higher and higher accuracy. There's something we're quite not learning. 
- People have been aligning proteins for a while, SSA full perform naive aligners such as HMMER and HHsuite (statistics based alignments), show context models can provide more information.

#### Pre-Transformers pLMs part 2
- Constrained semantic change search (CSCS, 2021) - same MIT lab, still use a bidirectional language model. If you consider semantic embedding and in silico mutatagenesis. The two contrast each other and don't have the same directional effect. But still need to encode both to understand the language. They fit a loss function with these two features. 
- Used this 2D knowledge and applied to COVID, and wanted to see which proteins were able to evade the immune system. The virus is most similiar to Bat and Pangolin. Model is learning properties of protein language, local mutational profile and if minute mutations affect profile. 

#### Evolutionary-Scale Models
- Families of ESM's of Transformer models such as ESM-1 2021 and ESM-2/ESMFold 2023. The data has grown a lot. People are thinking of these as PLMs and similiar to NLMs. These models are important not just because it can represent AAs well, but also understand the meaning and language through self-supervision. 
- Compare models - Transformers outperform LSTMs. They show if you remove 10% of the Transformer model, you get similiar or still better performance than LSTMs. 

#### Evolutionary-Scale Models p2
- Now let's think about downstream tasks. Took this language model and finetuned to learn protein task/function from sequence. This came out right after AlphaFold came out. This ESMFold doesn't perform as well as AlphaFold. ESMFold just use only protein sequences. These others use multiple sequences types as alignment. ESMFold performs better when you don't perform MSA. Shows importance of this second component of multiple sequence alignment. MSA to figure out how protein structure is conserved, almost impossible to solve, many hueristics as function is constrained. 
- Can we use a transformer to learn directly from MSAs? So far, we've been feeding each line here of alignment, can we learn alignment. 
- If you concatenate M sequences of length L, requires (ML)^2 which is extremely computationally extensively and impossible. There are shortcuts. We can simplify by paying attention to the row as well as attention on column, alernate two sets of attention and can get a pretty good job of leanring alignment.
- Doing so brings cost down to O(LM^2) per column and row attention. 
- They make another observation - you can use rows together to learn pairs of sequneces which is better than treating them independently. One row to another nearby row gives information, column to column doesn't. This reduces to row attention to O(L^2)
- AlphaFold which won the Nodel Award, a tiny part is MSA, that without MSA, AlphaFold2 is a bad structure predictor. Protein language is not just about the sequence of proteins and more about families of proteins. ESMFold can get pretty good results without performing MSA. 
#### Do PLMs Learn Structure Beyond Sequence Similiarity?
- Are PLMs learning more than sequences? Every protein is made of structural components. ChEMBL is a database of structural annotations. We should be able to predict structural units. The red is protien structure learning models. Green is squence aligners of just sequence similiarity. Blue is k-mer based. Sequence similiarity is comparable to these PLMs. 
#### Do PMLs Learn Protein Structure?
- It looks like ESM2 doesn't learn the structure. Plot the alpha and beta (two types of secondary structure), doesn't distinguish the two proteins. It's not wrong about distingishing, but it's not learning that these two proteins lead in impacting the structure. 
- Authors show if you consider AA as a stuctural and AA language (tokenize both sequence and AA), associate each sequence (uppercase letter) with a structure (lowercase). This leads to alpha and beta proteins separating. 
#### Benchmarking PLMs
- ProteinGym - score various PLMs on a set of tasks such as predicting outcomes of mutation, function. Color by what kind of knowledge the PLM had when training. 
- Green is what we just used. 
- This is on zeroshot performance of predicting one-base substitutions. See that the top performing models, require MSA and structure, usually both. Maybe we should be thinking of these PLMs as structural models, instead of language models. 
- Predict function of protein - function activity, binding expression, ... The top require strucutre, which is crucial for the performance of these models. 
- Also saw how these models improve as there is more data, subsequently more tokens. Found that these models hit a scaling wall at a model size around 18. Something new though is they found retrieval augmentation (search external databases for relevant, up-to-date information before making a prediction, not just initial dataset).
#### Can pLMs predict PPIs?
- Proteins can't interact on their own, require pair of proteins. Depends on if you're training on independent tasks and datasets. 
- Prtoein-protein binding affinity estimation - if we look at how well they predict protein biniding where you want low mean squared error. Strange to see Green does worse than Red (expert CNN). Came up with three explanations.
1. Inclusion of PPI testing proteins in the training of pLLMs inflate performance, suggesting data leakage. When you're finetuning, you usually split the test/train, if you don't know which proteins are interacting with each other when training, there may be bias in terms of having a proteins' binding partner. More strict split. Performance ended up going down. These models are learning from data leakage. Control showed that performance didn't go down for tasks related to PPIs. 
2. pLMs struggle to make out of distribution PPI predictions - we study mammalian proteins a lot, we don't study viruses well, but is important to understand. Graph shows that if you train a model to learn PPI, the curves show predictions in humans and viral organisms. We see either way they're not able to make out of distribution of PPIs predictions ~ generalizing to human predictions. 
3. pLMs are not sensitive to small but important changes in amino acid sequences. One single AA change can break a pocket or docking site. Shows the change in predicted interaction probability, how they should change and actuall change so how less likely they'll bind. 

#### An Everything Model?
- Why aren't these models based on the genomic model? As long as you have the DNA, shouldn't the model learn the mRNAs and codons to understand the protein language better? Showed models (in gray) what use the DNA nucleotide sequence perform the worst in practice. 

#### Reading for Next Class
- More about co-functional network approaches centered around Baysian approaches. 