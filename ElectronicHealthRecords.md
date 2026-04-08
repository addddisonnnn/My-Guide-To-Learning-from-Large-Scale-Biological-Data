## Session 8: Electronic Health Records
### Learning from Large-Scale Biological Data
### Wednesday April 8, 2026

#### Beyond the Human Reference Genome
- Originally we had this model that was a bit limiiting and now we're trying to consider variation with conjunction of the reference. People vary in ways beyond the human reference genome and how can we use this data to make models more efficient? The output of this model would vary even if the initial genomes are the same due to enviornment factors. 
- Now we can use EHR such as vital signs that affect the pieces of measurements (chromatin profiling, gene expression, 3D interactions) of the earlier model, laboaratory results, billing, medication records, and provider notes and reports. The last two are unstructured and the first three are structured. Can we use this to make drug predictions

#### Progression of EHRs in Genomics Research
- It's not the easiest to integrate this EHR data with genomic data to make predictions. Hospitals are collecting this data more and more now and they're stored in biobanks.

#### EHRs in Genomics Research
- Let's think about how we can encode this data.
- We've talked about molecular data that you can process, label, featurize, and embed. Also can take medical imaging data such as a description about a tumor or an MRI where sections are lighting up. Medicial imaging is different from this usual data because of its high spatial correlation, people use CNNs which have shown good performance, and challenges in detection, segmentation, classification, and spatial characterization. 
- Now the other part of a EHR is the text, which has been a challenge to convert as it is poorly or usually not structured and handwritten. This is where we require NLP tools for feature extraction. With the rise of transformers we can start attacking this challenge by contextually embed unstructured data. Some challenges remain that are domain specific as structured and unstructured data are very different and combining and merging them are difficult as they have different labels. 

#### EHRs in Genomics Research P2
- How do we actually identify what they have in common to fuse the data? Learning through three modalities. 
- Early fusion by concatenating feature vectors of different data modalities and only requires the training of a single model. Does require missing data imputation.
- Late fusion of fitting a seperate model for each modality and then integrating their single predictions. Allows the use of a different, often more suitable, model for each modalitity. Makes it straighforward to handle situations when some modalities are missing in the data. But this is ignoring possible synergiees between different modalities

#### And can be Prompted for Medicine
- Tool call Med-PaLM that uses AI and does as well as clinicians. But LLMs produce far more incorrect content while clinicians are more likely to tread the line of what is incorrect. 
- They also asked patients about the output from these LLMs. The LLMs were insightful but the users found the clinicians more helpful. This may be because we are used to seeing the information that is described by humans and clinicians answers. Also maybe because users don't like hallucinated answers. 

#### And continue to get better
- Med-PaLM 2 is performing well on medical school exams and questions. Med-PaLM 2 has been finetuned on MultiMedQA and performing better and better over time. 
- Highlight two things that Med-PaLM 2 has improved greatly in no sighn of incorrect knowledge recall and no sign of incorrect reasoning, but it is still bringing up and producing far more incorrect content. 
- This plot is interesting where the bias with finetuning has more bias as the performance got better. Should we be expecting bias getting worse towards specific subgroups? 
- Something they considered is adversarial questions such as to consider unusual conditions and context where you suggest treatment that goes against the patients choices as well as complex questions related to suicide. Med-PaLM 2 did do a lot better here compared to Med-PaLM. 
- Users are finding it as similiarly helpful as physicians. 

#### And better
- But, GPT-4 outperforms Med-PaLM 2 on these medicial exams. And Med-Gemini does even better. They're claiming that they're reaching a limit of performance where 'they can't even do better than previous'. 
- Med-Geminii is different than the previous as it incorporates multimodal data and can self train itself with web search. 
- First interesting task is genomic tasks that aren't necessarily hard questions as there are software packages that can translate nucleotides and figure out protein functions and alignments. And it's not performing well at all even if it can web search. 
- How about long-form EHR where MIMIC is a test to how it retreives long EHR and makes diagnoses? This is a relatively unsophisticated baseline, but it isn't outperforming its general embedding
- Last thing it understands well is videos, a video would play and it would identify mistakes made by surgeons in real time ~ when safety protocools are violated

#### Discussion Topics
- Would you give your data to LLM, would you trust the diagnosis, are you worried about the inaccurate and irrelevant content, worried about potential bias, advantage of asking for LLM, would you insist on human in the loop?