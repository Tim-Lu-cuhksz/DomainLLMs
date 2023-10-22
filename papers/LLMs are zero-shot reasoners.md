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



