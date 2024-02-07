# Self-Alignment with Instruction Backtranslation

## Introduction
- **Motivation**: The training of instruction-following model requires large amounts of human-annotated instructions with high quality.

- **Challenge**: Annotating instruction following datasets with high quality is HARD to scale.


## Methods
**Self-Alignment**: utilize the model to improve itself and align its response with desired behaviors.

1. **Self-Augmentation**: Fine-tune LLaMA (base model) on a seed dataset with element {$instruct_i, output_i$} in both directions (*output*->*instruct* & *instruct*->*output*), which yields $M_{yx}$.

2. Generate candidate instructions for outputs from unlabelled web corpus.

3. **Self-Curation**: Self-select high quality (quality score $a_i$ ranging from 0-5) demonstrations examples as training data to fine-tune the base model to follow instructions.

4. Iterate **Self-Curation**: Use the curated augmentation and seed data to fine-tune the improved model with tagging ($S_a$ and $S_w$ for seed data and web data respectively), and rescore augmented examples for quality.

## Experiments

### Setup

- Seed data: 3200 English examples from the Open Assistant dataset (chosen from the 1st turn of conversation tree).

- Base model and finetuning: LLaMA & Supervised finetuning (SFT) with less than 3k examples.

- Unlabelled data: 502k English segments of the Clueweb corpus.

- **Baselines**: 
    1. text-davinci-003 (based on GPT-3)
    2. LIMA
    3. Guanaco (same training dataset but using all conversation turns)

- Evaluation: Vicuna, Self-instruct, Open assistant, Koala, HH_RLHF, LOMA, crowdsourced from authors.

### Generation Quality
The paper measures win rates agianst text-davinci-003 via the following two evaluation methods:

**AlpacaEval**: GPT-4-based automatic evaluation

- Non-distilled: without relying on any external model
- Distilled: using data distilled from an external model
- Proprietary

**Human Evaluation**

### NLP Benchmarks
**Commonsense Reasoning**: SIQA, PIQA, Arc-Easy, Arc-Challenge, and Openbook QA

**MMLU**: massive multitask language understanding

## Observations (from ablation studies)
- Training on the selected (curated) data improves instruction following.

- Joint training with both seed and self-augmented data improves performance.

- Using a combined system prompt {$S_a, S_w$} at inference time is better.


## Conclusion
The paper proposes a scalable approach to finetuning LLMs to follow instructions. The model outperfoms all other non-distilled instruction-following models on the Alpaca leaderboard.

