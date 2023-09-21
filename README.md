# DomainLLMs
Domain large language models exhibit better performance compared to the general ones. However, when asking questions that are out of the domain, they may perform in a bad and unreliable way. 

Our goal is to analyze the OOD generalizability of domain LLMs and propose ways to improve the SOTA OOD detection methods for LLMs.

## A survey on Domain LLMs

| Name | Base Model | Size (para/tok) | Injecting Stage | Training Dataset | Evaluation | Domain |
|---|---|---|---|---|---|---|
| [HuaTuo](https://github.com/SCIR-HI/Huatuo-Llama-Med-Chinese/tree/main) | LLaMA | 7B | Instruction-tuning | [Chinese medical knowledge graph](https://github.com/SCIR-HI/Huatuo-Llama-Med-Chinese/tree/main/data) | Safety, usability, and smoothness (SUS) | Biomedical |
| [BioGPT](https://github.com/microsoft/BioGPT) | GPT-2 | 1.5B | Pretrained | [PubMed](https://pubmed.ncbi.nlm.nih.gov/) | Safety, usability, and smoothness (SUS) | Biomedical |
| [ChatLaw](https://github.com/PKU-YuanGroup/ChatLaw) | OpenLLaMA, BERT | 13B, 33B | Instruction-tuning | [Original legal data, legal regulations, and legal consultation data](https://github.com/PKU-YuanGroup/ChatLaw/tree/main/data) | ELO score | Law |
| [LaWGPT](https://github.com/pengxiao-song/LaWGPT) | LLaMA | 7B | Pretrained + Intruction-tuning | [Legal Q&A](https://github.com/pengxiao-song/awesome-chinese-legal-resources/tree/main) | n/a | Law |
| [BloombergGPT](https://arxiv.org/abs/2303.17564) | Bloom | 50B | Pretrained | Web, News, Filings, Press, Bloomberg9, The Pile, C4, Wikipedia | Few-shot methodology, heldout loss, financial tasks (F1 score), BIG-bench Hard, win rate, accuracy | Finance |
|[Minerva](https://arxiv.org/pdf/2206.14858.pdf) | PaLM | 8,62,54B/164,109,26B | Pretrained+fine-tuning | Math web pages, arXiv, general natural language data || math |                      