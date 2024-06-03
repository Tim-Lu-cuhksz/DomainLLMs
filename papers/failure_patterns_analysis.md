# Failure Pattern Analysis of MCQs
We have paraphrased **multiple choice questions** by either *back translation* or prompting GPT-3.5 and obtained questions pairs from 3k MMLU questions.

## Two Prompts to Analyze Patterns
We use the following two prompts to analyze failure patterns.

### Prompt 1
```
system_prompt = '''You are a helpful assistant. You will be given a series of pairs of sentences.
In each pair, the first sentence is the original question stem for a multiple-choice question, and the second sentence is its paraphrased form.
An example could be:
###
[question_1, question_1_after_paraphrasing]
[question_2, question_2_after_paraphrasing]
...
###
You should try to find different ways the original sentence is paraphrased and summarize common features that different paraphrasing has.
Note that you need to consider the pairs where the meaning of paraphrased sentence does not differ significantly from the original sentence.
Please try to give as many features as possible.
'''

user_prompt = ''' Here are pairs of questions for you to summarize failure patterns:
### 
[q1, q1_paraphrased]
[q2, q2_paraphrased]
...
###'''
```

### Prompt 2
```
system_prompt='''You are a helpful assistant. 
You will be given two groups of questions which exhibit different features.
You should try your best to summarize the differences between questions in the two groups.
An example could be:
###
Group 1:
[{question1},
{question2}]

Group 2:
[{question1},
{question2}]
###

user_prompt='''Here are the two groups of questions, based on which you should summarize the differences:
###
Group 1:
[{question1},
{question2}]

Group 2:
[{question1},
{question2}]
###
You could consider differences from linguistic aspects, length of the questions, etc. 
The differences you give may include but not limited to the aforementioned ones.
```

## Results of GPT-4
We use 50 pairs of the paraphrased question pairs for evaluation.

### Results of Prompt 1
The following is the results given by [prompt 1](#prompt-1).
```
1. Synonyms and Antonyms: The paraphrased sentences often use synonyms or antonyms to replace words in the original sentence. For example, "Oedema is the presence of" is paraphrased as "Swelling occurs when".

2. Change in Sentence Structure: The paraphrased sentences often change the structure of the original sentence while maintaining the same meaning. For example, "A producer of skin care products has created a new formula for its cream to cure acne" is paraphrased as "What is the appropriate statistical test to conduct in order to determine the significance of the difference in the effectiveness of the new acne cream formula compared to the old formula".

3. Active to Passive Voice or Vice Versa: The paraphrased sentences often change the voice of the original sentence. For example, "Roya paid $48 for 12 cartons of orange juice" is paraphrased as "What is the cost per carton of orange juice that Roya paid when she bought 12 cartons for a total of $48?"

4. Use of Different Phrases: The paraphrased sentences often use different phrases to express the same idea. For example, "Suppose that $a,b,$ and $c$ are positive integers satisfying $(a+b+c)^3 - a^3 - b^3 - c^3 = 150$. Find $a+b+c$." is paraphrased as "Determine the sum of the positive integers $a$, $b$, and $c$ if their cubes sum up to $150 more than the cube of their sum."

5. Change in Word Order: The paraphrased sentences often change the word order of the original sentence. For example, "In DNA adenine normally pairs with" is paraphrased as "In DNA, adenine typically forms pairs with".

6. Use of Different Verb Tenses: The paraphrased sentences often use different verb tenses to express the same idea. For example, "A football team entered into a 10-year lease with a city for use of the city's athletic stadium" is paraphrased as "How should the court decide on the motions made by both parties regarding the order of trials for the permanent injunction claim and the counterclaim seeking damages?"

7. Use of Different Grammatical Constructions: The paraphrased sentences often use different grammatical constructions to express the same idea. For example, "A woman had spent many years converting her back yard to an authentic Japanese-style garden" is paraph
```

### Results of Prompt 2
The following is the results given by [prompt 2](#prompt-2).
```
The two groups of questions exhibit differences in their structure, complexity, and context. 

Group 1 questions are generally more complex and lengthy, often providing a detailed context or scenario before posing the question. They tend to require a higher level of comprehension and critical thinking skills. Many of these questions are based on specific scenarios or information excerpts, requiring the reader to understand and interpret the provided context to answer the question. For example, the question "The inclusion of the Congo in Nkrumah's argument is intended to do which of the following?" is based on a specific excerpt and requires the reader to understand the context and implications of the excerpt to answer the question.

In contrast, Group 2 questions are generally more straightforward and concise, often asking direct questions without providing a detailed context. These questions are more focused on factual knowledge and less on interpretation or critical thinking skills. For example, the question "Which of the following karyotypes is diagnostic of Down syndrome?" is a direct question that requires factual knowledge to answer.

Moreover, Group 1 questions often use more formal and complex language, while Group 2 questions use simpler and more direct language. For instance, the question "A person is prescribed Ropinirole 1.5 mg divided into three doses. How many micrograms is each dose?" in Group 1 is more complex and formal compared to the similar question in Group 2 "Each dose of Ropinirole is equivalent to how many micrograms when a person is prescribed 1.5 mg divided into three doses?"

In summary, the main differences between the two groups of questions are in their complexity, context, and language. Group 1 questions are more complex, context-dependent, and use more formal language, while Group 2 questions are simpler, more direct, and use more straightforward language.
```