# DomainLLMs
Domain large language models exhibit better performance compared to the general ones. However, when asking questions that are out of the domain, they may perform in a bad and unreliable way. 

Our goal is to analyze the OOD generalizability of domain LLMs and propose ways to improve the SOTA OOD detection methods for LLMs.

## Our definition of OOD (for domain LLMs)
Let $\mathcal{D}$ be a dataset of all domains, and $d_i \in \mathcal{D}, i=1...N$, indicates all data related to domain $i$. If a domain large language model $\mathcal{M}$ is specifically pretrained or fine-tuned on $d_i$, then data from domains $d_j$, where $i \neq j$ is considered to be OOD with respect to $\mathcal{M}$.


## A survey on Domain LLMs

| Name | Base Model | Size (para/tok) | Injecting Stage | Training Dataset | Evaluation & Benchmarks (bm) | Domain |
|---|---|---|---|---|---|---|
| [HuaTuo](https://github.com/SCIR-HI/Huatuo-Llama-Med-Chinese/tree/main) (Chinese, 3.7k star) | LLaMA | 7B/8k instruction data | Instruction-tuning | [Chinese medical knowledge graph](https://github.com/SCIR-HI/Huatuo-Llama-Med-Chinese/tree/main/data) | Medical QA (human-based expert to rate safety, usability, and smoothness // bm: LLaMA-7B, Alpaca, ChatGLM-6B) | Biomedical |
| [BioGPT](https://github.com/microsoft/BioGPT) (English, 4.1k star) | GPT-2 | 347M/n.a. | Pretrained* | [PubMed](https://pubmed.ncbi.nlm.nih.gov/) | 1. Relation extraction (e.g. the relation between drug1 and drug2 exsists or not exist // f1 score // bm: GLRE, GPT-2, REBEL, seq2rel); 2. Question Answering (given a pair of QA answer yes/no/maybe // accuracy // bm: BioBERT, PubMedBERT, BioLinkBERT, GPT-2); 3. Text Generation (human-based evaluation // bm: GPT-2) |Biomedical|
| [ChatLaw](https://github.com/PKU-YuanGroup/ChatLaw) (Chinese, 5k star) | OpenLLaMA, BERT | 13B, 33B/n.a. | Instruction-tuning | [Original legal data, legal regulations, and legal consultation data](https://github.com/PKU-YuanGroup/ChatLaw/tree/main/data) | 2k judicial examination multiple-choice questions (ELO score, win rate // bm: gpt-4, laywer-llama-13B, gpt-3.5-turbo, OpenLLaMA-13B, LawGPT) | Law |
| [LaWGPT](https://github.com/pengxiao-song/LaWGPT) (Chinese, 5.2k star) | LLaMA | 7B/300k legal QA |Intruction fine-tuning | [Legal Q&A](https://github.com/pengxiao-song/awesome-chinese-legal-resources/tree/main) | n.a. | Law |
| [LexGPT](https://arxiv.org/pdf/2306.05431v1.pdf) (English) | GPT-J-6B | 6B/291.5GiB |Pretrained | [Pile of Law](https://huggingface.co/datasets/pile-of-law/pile-of-law) | 1. Contract provision classification (LEDGAR dataset //  bm: CaseLaw-BERT); 2. Multiple choice questions (CaseHOLD dataset: holdings of US court cases //  bm: CaseLaw-BERT) | Law |
| [BloombergGPT](https://arxiv.org/abs/2303.17564) (English) | Bloom | 50B/363B+345B | Pretrained | Web, News, Filings, Press, Bloomberg9, The Pile, C4, Wikipedia | 1. Public financial tasks (public datasets) 2. Bloomberg financial tasks (NER and sentiment analysis) 3. Big-bench Hard (reasoning and general NLP tasks) 4. Knowledge assessments (testing closed-book info recall) 5. Reading comprehension (testing open-book tasks) 6. Linguistic tasks /// Classification, generative, multiple-choice QA /// bm: GPT-NeoX, OPT-66B, BLOOM-176B | Finance |
| [FinGPT](https://github.com/AI4Finance-Foundation/FinGPT) (English + Chinese, 8.6k) | ChatGLM & LLaMA | 6B,7B,13B/1.4T,2T | Pretrained | Financial news, social media, filings, trends, academic datasets | 1. Financial Sentiment Analysis 2. Financial Report Summary /// bm: FPB, FiQA-SA, TFNS, NWGI | Finance |
|[Minerva](https://arxiv.org/pdf/2206.14858.pdf) (English) | PaLM | 8,62,54B/164,109,26B | Pretrained+fine-tuning | Math web pages, arXiv, general natural language data | 1. Middle school mathematics problem (MATH) 2. Middle school math word problems (GSM8k) 3.STEM problems (MMLU-STEM // multiple-choice for mathematical reasoning // bm: PaLM-8B, PaLM-62B, OpenAI davinci-002, Published SOTA)| Math |               

Note: BioGPT is pre-trained only on in-domain text data. Also, for language indicated in the brackets after the model name is the primary languague that appeared in the pre-training & fine-tuning stage.


## Progress
- Since FinGPT has the greatest Github stars, we try to run experiments using FinGPT (2023 9.26)