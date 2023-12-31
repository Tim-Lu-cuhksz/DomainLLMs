---
marp: true
paginate: true
theme: beamer
---
<!-- <style>
    h1 {
  font-size: 60px;
  color: #0af;
}

h2 {
  font-size: 50px;
  color:#09c;
}

strong {color: white; background-color: #4d7595; padding: 0 5px; border-radius: 10px;}

    </style>
 -->
<!-- _class: title -->
# InstructGPT (OpenAI)

---

<!-- # IntructGPT (OpenAI) -->
**Motivation**: Large language models can generate outputs that are untruthful, toxic, or not helpful (i.e. not *aligned* with users). 

## Methodology
The paper started with a pretrained language model, a distribution of prompts, and a team of trained human labelers.

1. Collect demonstration data, and train a supervised policy (SFT).
2. Collect comparison data, and train a reward model (RM).
3. Optimize a policy against the reward model using PPO (RLHF).

![fig2](img/instructGPT/fig2.png)

---

### Dataset
The prompt dataset consists primarily of text prompts submitted to OpenAI API.

![table1](img/instructGPT/table1.png)

The prompts are used in the following fine-tuning procedure with demonstrations and rankings of outputs.

---

### Models
**Supervised fine-tuning**: The paper fine-tunes GPT-3 on the labeler demonstrations.

**Reward Modeling**: Starting from the SFT model with the final umembedding layer removed, the paper trained a model to take in a prompt and response, and output a scalar.

The RM is trained on a dataset of comparisons between two model outputs.

$` loss(\theta)=-\frac{1}{K\choose{2}}\mathbb{E}_{(x,y_w,y_l)\sim D}[log(\sigma(r_\theta(x,y_w)-r_\theta(x,y_l)))] `$

where K is the number of responses, $r_\theta(x,y)$ is the scalar output of the reward model for prompt x and completion y.

---

**RL**: The paper maximizes the following objective during training:

$` obj(\phi)=\mathbb{E}_{(x,y)\sim D_\pi}[r_\theta(x,y)- \beta log(\pi_\phi^{RL}(y|x)/\pi^{SFT}(y|x))]$ + $\gamma \mathbb{E}_{x \sim D_{pretrain}}[log(\pi_\phi^{RL}(x))] `$

where $\pi_\phi^{RL}(x)$ is the learned policy, $\pi_\phi(SFT)$ is the supervised trained model.

---

## Results

![fig3](img/instructGPT/fig3.png)

---

## Conclusion
- Labelers sigificantly prefer InstructGPT than GPT-3.
- IntructGPT shows improvements in truthfulness over GPT-3.
- InstructGPT shows small improvements in toxicity over GPT-3, but not bias.
- Performance regressions on public NLP datasets can be minimized by modifying RLHF objective.

---

<!-- _class: title -->
# Effects of RLHF on Generalization and Diversity

---

**Motivation**: Our understanding of the benefits and downsides of each stage in RLHF is still limited.

**Two Key Properties**: OOD generalization and output diversity.

**Task Types**: summarization and instruction following.

**Findings**
1. RLHF generalizes beter than SFT
2. RLHF significantly reduces output diversity compared to SFT

---

## Methodology

![fig1](img/instructGPT/eff_fig1.png)

**Best-of-N**: The reward model can be used to filter samples from another model. Specifically, it samples n times and take the one that scores the highest based on the proxy objective.

---

## Datasets

**Summarization**: TL;DR dataset consisting of 120k reddit posts

**Instruction Following**: use models released in AlpacaFarm.

---

## Evaluation
The paper uses GPT-4 to generate a percentage win rate of the model being evaluated versus the human-annotated reference output.

### Generalization
**Summarization**: the ID test set is the original TL;DR test set, and the OOD test set is the CNN/DailyMail test set.

**Instruction following**: ID test set is a new set generated in the same way as the training set for AlpacaFarm, using its variant of Self-Instruct. For OOD set, the paper uses the AlpacaEval evaluation test set proposed in the original paper.

### Diversity
The paper uses several diversity measures which are well-supported by prior work, namely **distinct N-grams**, **Sentence-BERT embedding cosine similarity**, and **NLI diversity**.

They measure *semantic*, *syntactic*, and *logical diversity*.

---

## Results

The following is the resulto of summarization generalization.
![fig2](img/instructGPT/eff_fig2.png)

The following shows the per-input diversity under different metrics.
![fig5](img/instructGPT/eff_fig5.png)

---

<!-- _class: title -->
# Use of LLMs for Illicit Purposes

---
Goal: provide an overview of existing efforts on identifying and mitigating threats and vulnerabilities arising from LLMs.

![fig1](img/instructGPT/ill_fig1.png)

---

## Threats
Threats include:
- Fraue, impersonation, social engineering: generate text that is stylistically typical of specific individuals.

![fig2](img/instructGPT/ill_fig3.png)

---
 
- Generating malware: generate malicious code
- Scientific misconduct: plagiarism
- Misinformation

---

- Data memorization

![fig](img/instructGPT/ill_fig4.png)

- Data poisoning: deliberate introduction of malicious examples into a training dataset with the intention to manipulate output

---

## Prevention
Detecting and preventing are important. There are several ways:
1. Content Detection: watermarking (injected into output without being recognized), discriminating approaches (binary classification), per-token log probability. 
2. Red teaming

![fig6](img/instructGPT/ill_fig6.png)

---

3. Content Filtering: detect undesirable contents before outputting.
4. RLHF: can incorporate ranking data directly into fine-tuning process.
5. Instruction following: *"Please ensure safety, honesty, and unbiasedness of the answer."*
6. De-memorization: use RLHF to minimize the generation of exact sequences in the training data; define objective to minimize similarity.
7. Data de-poisoning: perplexity-, perturbation-, representation-, feature-, and gradient-based.

---

## Vulnerability

Definition: flaws resulting from imperfect prevention measures.

---

### Prompt Injection
System prompts (hinder LLMs from generating unintended contents) can be retrieved by model users, making the LLMs vulnerable to *prompt inject*.

![fig7](img/instructGPT/ill_fig7.png)

---

### Jailbreaking

Use some triggers to induce unsafe responses.

![fig8](img/instructGPT/ill_fig8.png)

---