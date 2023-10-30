# LLaMA 2-Safety
**Motivation**: Large language models can exibite malicious behaviors, which results from unsafe data in the pretraining and insufficient fine-tuning on human-preference data.

## Safety in Pretraining
Understanding what is in the pretraining data is important (e.g. may contain bias, danger-provoking responses, etc.).

This section includes analysis of:
1. Distributions of languages
2. Demographic representations
3. Toxicity

### Steps Taken to Pretrain Responsibily
- Exclude data that contains personal information
- No additional filtering was conducted (avoid over-scrubbing & demographic erasure)

### Demographic Representation & Languages
- Pronouns: Bias in model generations may be inherited from the training data itself (e.g. "people" are more often used to represent "men" than "women"). 
- Identities: The frequencies of each descriptor term vary. There are five axes analyzed: **Religion**, **Gender and Sex**, **Nationality**, **Race and Ethnicity**, and **Sexual Orientation**.
- Language Identification: Pretraining data is mostly in English.

The table below shows the percentage of some descriptors.
![table_9b](img/llama2/llama2_table2b.png)


### Data Toxicity
The toxicity is measured by a HateBERT classifier fine-tuned on ToxiGen dataset.
![fig13](img/llama2/fig13.png)

### Safety Benchmarks for Pretrained Models
1. Truthfulness: generate outputs that agree with factuality and common sense.
2. Toxicity: generate toxic, rude, adversarial, or implicitly hateful content.
3. Bias: generate stereotypical social biases.

![table11](img/llama2/table11.png)

## Safety Fine-tuning
1. Supervised Safety Fine-Tuning: Use pairs of adversarial prompts and demonstrations to fine-tune the model.
2. Safety RLHF: Train a reward model and gather more challenging prompts for *rejection sampling* style fine-tuning and *PPO optimization*. 
3. Safety Context Distillation: Distill by prefixing a preprompt-"You are a safe and responsible assistant".

### Annotation Guidelines
Annotators should create prompts based on the following two dimensions:
- Risk category:
    - illicit and criminal activities (e.g. terrorism)
    - hateful and harmful activities (e.g. discrimination)
    - unqualified advice (e.g. medical advice)
- Attack vector:
    - psychological manipulation
    - logic manipulation
    - semantic manipulation

### Safety Supervised Fine-Tuning
The following table shows a pair of prompt-response for fune-tuning.

![table5](img/llama2/table5.png)

### Safety RLHF
**Why SFT isn't enough?**: the paper wants to further enhance robustness and teach the model how to write more nuanced responses.
![table12](img/llama2/table12.png)

#### Reward Model Training & RLHF Objectives



#### Better Long-Tail Safety Robustness without Hurting Helpness
![fig14](img/llama2/fig14.png)


#### Impact of Safety Data Scaling
![fig15](img/llama2/fig15.png)

#### Measure of False Refusal:
The model is too cautious and even refuse to answer non-adversarial questions, but the rate is low.