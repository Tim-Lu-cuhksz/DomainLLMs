# Large Language Models are Zero-shot Reasoners
This markdown file briefly summarizes the [paper](https://arxiv.org/pdf/2205.11916.pdf). For more details, please click the link provided.

## Introduction
The success of large language models is attributed to **few-shot** or **zero-shot** learning. The method of conditioning on **shots** is called *prompting*. 
- Challenge: pretrained large language models achieves poor performance in hard reasoning tasks.
- CoT: chain of thought prompting uses step-by-step reasoning examples, conditioned on which the LLMs can solve mathematical problems. 
- Motivation: can **zero-shot** learning achive the same results as CoT prompting does?

## Method: Zero-shot Chain of Thought
The paper propses a zero-shot *template-based* prompting for chain of thought reasoning. The novelty is described as follows:
1. It does not require step-by-step **few-shot** examples
2. It differs from most of the prior template prompting as it is *task-agnostic* (solves a wide range of tasks with a single template).

A comparision between different prompting methods are shown as follows:
![image](img/zero-shot%20reaoners/few-shot%20cot%20gpt3.png)


### Two-stage promting
- Reasoning extraction: modify the input question $x$ into a *prompt* $x'$ using "```Q:[X]. A:[T]```", where ```[X]``` is a slot for $x$ and ```[T]``` is a slot for hand-crafted trigger sentence like "Let's think step by step."
- Answer extraction: concatenate three elements together "```[X'][Z][A]```": ```[X']``` for 1st promt $x'$, ```[Z]``` for sentence generated at the first step, and ```[A]``` for a trigger sentence to extract answer like "Therefore, the anwer (arabic numerals) is".

The pipeline is shown as follows:
![pipeline](img/zero-shot%20reaoners/few-shot%20cot%20pipeline.png)

## Experiment
### Setup

- **Tasks and datasets**: the proposal is evaluated on 12 datasets from four categories of reasoning tasks: arithmetic, commonsense, symbolic, and other logical reasoning tasks.
- **Models**: The paper experiments 17 models in total. Main experiments are conducted with Instruct-GPT3 (text-davinci-001 & text-davinci-002), orignal GPT3, and PaLM.
- **Baselines**: Zero-shot CoT vs. Zero-shot promting.
- **Answer cleansing**: filter out the exact answers.

### Results
Table 1 compares accuracy of Zero-shot-CoT and Zero-shot on each task.

![table1](img/zero-shot%20reaoners/table1.png)

Table 2 compares baseline methods on MultiArith and GSM8K.

![table1](img/zero-shot%20reaoners/table2.png)

### Observations
1. Zero-shot-CoT performs better on reasoning tasks than Zero-shot in general.
2. Zero-shot-CoT naturally underpeforms Few-show-CoT, but it substantially outperforms standard Few-shot prompting with even 8 examples per task.
3. Without CoT, the curve performance is mostly flat as model size increases. In contrast, the performance drastically increases with CoT as the model size gets bigger.
4. Zero-shot-CoT tends to output unnecessary steps of reasoning after the correct prediction, which results in changing the prediction to incorrect one.
![img](img/zero-shot%20reaoners/error_analysis.png)

5. Prompt selection can also affect performance.
![img](img/zero-shot%20reaoners/prompt_selection.png)

## Conclusion:
1. Zero-shot-CoT, a single zero-shot prompt can elicit chain of thought from LLMs, outperforms its base lines Zero-shot and Few-shot without CoT.
2. Zero-shot-CoT is task-agnostic: no need to craft the prompt for different tasks.