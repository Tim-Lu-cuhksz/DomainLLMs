# Failure Pattern Analysis for MCQs
We would like to analyze failure patterns for MCQs evaluated by GPT-3.5-turbo.

## Preliminary Results given by GPT-4
We use the following prompt (may need modifications in future versions) to analyze the patterns.
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


## Paraphrased using Back Translation
```
1. Substitution of words with synonyms or near-synonyms: For example, "plant" is replaced with "product" or "item" is replaced with "product". 

2. Changing the sentence structure: This includes changing the order of phrases or clauses in the sentence, or changing from active to passive voice or vice versa.

3. Adding or removing details: Some paraphrases may add extra information or context, while others may remove it. 

4. Changing the tense or aspect of verbs: For example, "is" might be changed to "was", or "run" might be changed to "running".

5. Using different forms of the same word: For example, "listening" might be changed to "hear".

6. Changing the perspective or point of view: For example, "you" might be changed to "one" or "a person".

7. Simplifying or complicating the language: Some paraphrases might use simpler or more complex language than the original sentence.

8. Changing the punctuation: For example, a question might be turned into a statement, or vice versa.

9. Changing the level of formality or informality: Some paraphrases might use more formal or informal language than the original sentence.

10. Changing the type of speech: For example, a statement might be turned into a question, or vice versa.

11. Changing the subject or object of the sentence: For example, "he" might be changed to "she", or "the cat" might be changed to "it".

12. Changing the prepositions: For example, "on" might be changed to "at", or "in" might be changed to "on".

13. Changing the conjunctions: For example, "and" might be changed to "but", or "or" might be changed to "and".

14. Changing the adverbs or adjectives: For example, "quickly" might be changed to "fast", or "happy" might be changed to "joyful".

15. Changing the pronouns: For example, "he" might be changed to "they", or "it" might be changed to "this".

16. Changing the quantifiers: For example, "many" might be changed to "a lot of", or "some" might be changed to "a few".

17. Changing the modal verbs: For example, "can" might be changed to "could", or "must" might be changed to "have to".

18. Changing the negations
```


## Paraphrased by GPT-3.5

```
1. Change in sentence structure: The paraphrased sentences often have a different structure than the original ones. This could involve changing the order of the information, converting active sentences to passive ones, or vice versa.

2. Use of synonyms: The paraphrased sentences often use different words that have the same or similar meanings as the words in the original sentences.

3. Change in verb forms: The paraphrased sentences may use different verb forms or tenses than the original sentences.

4. Addition or removal of details: Some paraphrased sentences may add extra details that were implied but not explicitly stated in the original sentences. Conversely, some details may be removed if they are not essential to the main idea of the sentence.

5. Change in perspective: Some paraphrased sentences may change the perspective from which the information is presented. For example, a sentence that originally presented information from a third-person perspective may be paraphrased to present the same information from a first-person perspective.

6. Use of rhetorical questions: Some paraphrased sentences may convert statements into rhetorical questions, or vice versa.

7. Change in tone: The tone of the paraphrased sentences may be different from the tone of the original sentences. For example, a sentence that originally had a formal tone may be paraphrased in a more casual tone.

8. Use of different grammatical constructions: The paraphrased sentences may use different grammatical constructions than the original sentences. For example, a sentence that originally used a relative clause may be paraphrased using a participle phrase.

9. Change in word order: The paraphrased sentences often change the order of words or phrases in the sentence while maintaining the same meaning.

10. Use of different prepositions: The paraphrased sentences may use different prepositions than the original sentences to convey the same meaning. 

11. Change in punctuation: The paraphrased sentences may use different punctuation than the original sentences. For example, a sentence that originally used a comma may be paraphrased using a semicolon. 

12. Use of different conjunctions: The paraphrased sentences may use different conjunctions than the original sentences to link ideas or clauses. 

13. Change in sentence type: The paraphrased sentences may change the type of sentence. For example, a declarative sentence may be changed into an interrogative sentence. 

14. Use of different sentence lengths: The paraphrased sentences may use different sentence lengths than the original sentences. For example, a long, complex sentence may
```

## What To Do Next?
1. Select one of the failure patterns (e.g., "Change in sentence structure") from above and prompt GPT-3.5-turbo to paraphrase the 3k CommonsenseQA questions accordingly.
We use the following prompt for paraphrasing (let me know if you think the prompt is bad): 
```
# Let me know if you think the following prompt is not good enough!!!
system_prompt = """You are a helpful assistant. You will be given a question and a mode with an explanation for paraphrasing. An example could be:
###
Mode: some_specific_mode: explanation
Question: the_question_stem
###
You should carefully read the question and understand the paraphrasing mode. Please paraphrase the sentence according the mode and output your result. 
You should not paraphrase using other modes.
Do not output any prompts or contents apart from the paraphrased question."""

user_prompt = f'''Please paraphrase the following question based on the given mode.
###
Mode: {some_specific_mode}
Question: {the_question_stem}
###'''
```

2. Evaluate the paraphrased questions to see the fraction of inconsistent answers (evaluated wrongly/correctly only after paraphrasing)