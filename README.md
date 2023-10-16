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

## Experiment Reproduction
We try to reproduce the experiment results on FinGPT_ChatGLM2-6B fine-tuned (via LoRA) primarily on sentiment instructions.

Below is an example of evaluation on multiple-choice questions on MMLU. There are 10 QA pairs, along with answers, for demonstrations.

```
["Instruction: What is the best option that solves the following multiple-choice question? Please choose an answer from {A/B/C/D}.\nInput: Publishing and selling the creative work of others and protecting property rights of one's own creative work is regulated by A. libel and publication regulations B. access and deregulation C. Securities and Exchange Commission D. copyright and trademark law\nAnswer:", 

"Instruction: What is the best option that solves the following multiple-choice question? Please choose an answer from {A/B/C/D}.\nInput: The market model of state welfare is based on the principle of: A. individuals buying welfare privately, with some means-tested benefits B. regular benefit payments to men as earners of the 'family wage' C. a universalist system of welfare for all, regardless of income D. decommodifying social welfare through state provision\nAnswer:",

"Instruction: What is the best option that solves the following multiple-choice question? Please choose an answer from {A/B/C/D}.\nInput: In what ways can the different relationship between US and European security studies be explained? A. The different relationship to the concept of security: in Europe the debate has stayed as part of politico-self-reflection of the scholar who 'does security;' whereas in the US, the question of the concept of security is seen at the most a necessary 'define your' terms' operation. B. The different relationship to the exact form of knowledge that is valued. The US is more rationalist, Europe the more reflectivist. In the US security studies debate hypothesis is typically examined as cause-effect relationships, tied into general debates and with measurement of ideational variables. C. In the US major competing explanations emphasized, on the one hand, the US's liberal hegemony, and, on the other hand, the purely power based stability of presumed unipolarity in which balancing becomes impossible. D. All of these options.\nAnswer:",

"Instruction: What is the best option that solves the following multiple-choice question? Please choose an answer from {A/B/C/D}.\nInput: This question refers to the following information.Read the following letter.It happened . . . that a Spaniard saw an Indian . . . eating a piece of flesh taken from the body of an Indian who had been killed. . . . I had the culprit burned, explaining that the cause was his having killed that Indian and eaten him, which was prohibited by Your Majesty, and by me in Your Royal name. I further made the chief understand that all the people . . . must abstain from this custom. . . . I came . . . to protect their lives as well as their property, and to teach them that they were to adore but one God, . . . that they must turn from their idols, and the rites they had practiced until then, for these were lies and deceptions which the devil . . . had invented. . . . I, likewise, had come to teach them that Your Majesty, by the will of Divine Providence, rules the universe, and that they also must submit themselves to the imperial yoke, and do all that we who are Your Majesty's ministers here might order them.—Hernan Cortez, Fifth Letter to Holy Roman Emperor Charles V, 1521Which of the following superior technologies was most crucial to the Europeans' success in the New World? A. New sailing technologies like the compass and the astrolabe B. New ships like the carrack and caravel C. Weapons like cannon and war horses D. Better educational systems that taught navigation and warfare\nAnswer:", 

"Instruction: What is the best option that solves the following multiple-choice question? Please choose an answer from {A/B/C/D}.\nInput:  Which of the following does NOT describe the Ka'ba? A. It was visited by Moses. B. It is a haram. C. It was built by Abraham. D. It is a square building.\nAnswer:", 

'Instruction: What is the best option that solves the following multiple-choice question? Please choose an answer from {A/B/C/D}.\nInput: This question refers to the following information.When the Portuguese go from Macao in China to Japan, they carry much white silk, gold, musk, and porcelain: and they bring from Japan nothing but silver. They have a great carrack which goes there every year and she brings from there every year about six hundred coins: and all this silver of Japan, and two hundred thousand coins more in silver which they bring yearly out of India, they employ to their great advantage in China: and they bring from there gold, musk, silk, copper, porcelains, and many other things very costly and gilded.When the Portuguese come to Canton in China to traffic, they must remain there but certain days: and when they come in at the gate of the city, they must enter their names in a book, and when they go out at night they must put out their names. They may not lie in the town all night, but must lie in their boats outside of the town. And, their time expired, if any man remains there, he is imprisoned.Ralph Fitch, an account of his travels to the Far East, 1599 C.E.The description in the second paragraph of the procedures that Portuguese and other foreigners followed when trading in China supports which of the following inferences about trade policy in the later Ming Dynasty? A. The Ming Dynasty was very supportive of and welcoming to all commercial enterprise. B. The Ming Dynasty sought to regulate trade strictly to limit contact with foreigners and ease collection of taxes. C. The Ming Dynasty was too busy battling pirates along its coast to pay any attention to regular merchant activity. D. The Ming Dynasty only allowed Portugal to trade with China through Canton, permitting only one boat a year.\nAnswer:', 

'Instruction: What is the best option that solves the following multiple-choice question? Please choose an answer from {A/B/C/D}.\nInput: An individual with brain lesions in the hippocampus will most likely experience impairment to her A. emotional regulation B. motor coordination C. speech production D. learning\nAnswer:', 

'Instruction: What is the best option that solves the following multiple-choice question? Please choose an answer from {A/B/C/D}.\nInput: For which of these two scenarios does the main character (who uses I/me/my) do something clearly morally wrong, according to ordinary moral standards in the US as of 2020? Scenario 1 | I check his essay before his scheduled course. Scenario 2 | I gave the new teacher a classroom of 55 children. A. Wrong, Wrong B. Wrong, Not wrong C. Not wrong, Wrong D. Not wrong, Not wrong\nAnswer:',

'Instruction: What is the best option that solves the following multiple-choice question? Please choose an answer from {A/B/C/D}.\nInput: Which of the following kinds of therapies would most likely be used by a somatic therapist? A. counterconditioning B. systematic desensitization C. rational emotive behavior therapies D. chemotherapy\nAnswer:',

'Instruction: What is the best option that solves the following multiple-choice question? Please choose an answer from {A/B/C/D}.\nInput: What is the value of the expression 2(3(4^2 + 1)) - 2^3? A. 156 B. 110 C. 94 D. 48\nAnswer:']

----------Generated Anwsers----------
 D.

根据问题，我们需要选择一个选项来解决以下多选题。选项包括A、B、C和D。

根据分析，我们可以得出结论，正确答案是D。因为版权和商标法是保护知识产权的法律法规，而其他选项与知识产权保护无关。

因此，最终答案是D。
====================
 B
====================
 D

The best option that solves the multiple-choice question is option D, which states that all of the options provided in the question are correct. This option indicates that each of the given options is a valid explanation for the relationship between the US and European security studies. Therefore, the correct answer is D.
====================
 B.
====================


根据题目要求，我们需要选择一个选项来解决这个多项选择题。选项A说摩西曾经访问过麦加，选项B说麦加是禁地，选项C说麦加是由亚伯拉罕建造的，选项D说麦加是一个广场建筑。根据我们对麦加的了解，我们知道麦加是一个广场建筑，因此选项D是正确的答案。
====================
 B

Explanation:

1. The description in the second paragraph of the procedures that Portuguese and other foreigners followed when trading in China supports which of the following inferences about trade policy in the later Ming Dynasty?
2. The description in the second paragraph of the procedures that Portuguese and other foreigners followed when trading in China suggests that the Ming Dynasty sought to regulate trade strictly to limit contact with foreigners and ease collection of taxes.
3. The description in the second paragraph of the
====================
 D

The best option that solves the multiple-choice question is D. learning.

The hippocampus is a critical part of the brain that is responsible for learning and memory. It is the area of the brain that is affected in individuals with hippocampal damage, such as those with Alzheimer's disease. Therefore, the best option that solves the multiple-choice question is D. learning.
====================


根据题目，我们需要判断哪个选项是正确的。选项A表示“错误，错误”，选项B表示“错误，不正确”，选项C表示“不正确，错误”，选项D表示“不正确，不正确”。

根据题目中的描述，选项A表示“检查学生的论文”，选项B表示“给新老师一个班级”，选项C表示“不正确，错误”，选项D表示“不正确，不正确”。

根据题目中的描述，选项A和
====================
 B.
====================
 B.

。
。

。 1. First, we need to evaluate the given function using the given options.
2. Option A: 2(3(4^2 + 1)) - 2^3 = 2(3(16 + 1)) - 2^3 = 2(3(17)) - 2^3 = 2(51) - 8 = 102 - 8 = 9

----------Ground-truth Anwsers----------
['D', 'A', 'D', 'C', 'A', 'B', 'D', 'C', 'D', 'C']

```

## Progress
- Since FinGPT has the greatest Github stars, we try to run experiments using FinGPT (2023 9.26).
