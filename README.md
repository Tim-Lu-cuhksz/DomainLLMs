# DomainLLMs
Domain large language models exhibit better performance compared to the general ones. However, when asking questions that are out of the domain, they may perform in a bad and unreliable way. 

Our goal is to analyze the OOD generalizability of domain LLMs and propose ways to improve the SOTA OOD detection methods for LLMs.

## A survey on Domain LLMs

| Name | Base Model | Size | Injecting Stage | Dataset | Evaluation Metric | Domain |
|---|---|---|---|---|---|---|
| [HuaTuo](https://github.com/SCIR-HI/Huatuo-Llama-Med-Chinese/tree/main) | LLaMA | 7B | Instruction-tuning | [Chinese medical knowledge graph](https://github.com/SCIR-HI/Huatuo-Llama-Med-Chinese/tree/main/data) | Safety, usability, and smoothness (SUS) | Biomedical |
| [ChatLaw](https://github.com/PKU-YuanGroup/ChatLaw) | OpenLLaMA, BERT | 13B, 33B | Instruction-tuning | [Original legal data, legal regulations, and legal consultation data](https://github.com/PKU-YuanGroup/ChatLaw/tree/main/data) | ELO score | Law |
| [LaWGPT](https://github.com/pengxiao-song/LaWGPT) | LLaMA | 7B | Pretrained + Intruction-tuning | [Legal Q&A](https://github.com/pengxiao-song/awesome-chinese-legal-resources/tree/main) | n/a | Law |
|   |   |   |   |   |||
|   |   |   |   |   |||                      