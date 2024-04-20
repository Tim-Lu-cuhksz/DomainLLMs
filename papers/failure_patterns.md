# Failure Pattern Analysis for MCQs
We would like to analyze failure patterns for MCQs evaluated by GPT-3.5-turbo.

## Prompt for Paraphrasing
We use back translation and GPT-3.5-turbo to paraphrase the question stem.

```
system_prompt="""You are a helpful assistant. You will be given a sentence and your job is to paraphrase it properly without changing its meaning.
Please output the paraphrased sentence in the end.
Do not output anything else apart from the paraphrased sentence."""

user_prompt=f'''Please paraphrase the sentence: [{question}]'''
```

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

## Testing Effectiveness of The Mode
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

## What to do next?
From previous trials, we found out that the prompt might not be properly designed, resulting in sentences being paraphrased based on multiple modes.

Next, we try to constrain the prompt a little bit.

First, the modes summarized by GPT-4 tend to be too general, we promt GPT-3.5 to give more details. 
```
system_prompt: f'''You are a helpful assistant.'''

user_prompt: '''You will be given a specific way of paraphrasing a sentence and its explanation. Please tell me more about this way and give several examples of paraprasing sentences using this way.
The specific way and explanation is {mode_with_explanation}'''
```

Once we obtain the result, we would like to use it to improve the prompt in the previous section (both zero-shot and few-shot).

```
system_prompt = """You are a helpful assistant. You will be given a mode of paraphrasing a sentence and its an explanation. 
[You will also be given few examples of paraphrasing a sentence using the given mode]
An example of the format could be:
###
Mode: some_specific_mode: explanation

Example 1:
Original sentence: xxx
Paraphrased sentence: xxx

... more examples ...
###
You should understand the paraphrasing mode properly (from the examples)."""

user_prompt = f'''Please paraphrase the following sentence based on the given mode and output your result.
You should not paraphrase using other modes.
Do not output any prompts or contents apart from the paraphrased question
###
Mode: {some_specific_mode}

Original sentence: {the_question_stem}
Paraphrased sentence:
###'''
```

## More Attempts to Analyze Patterns

### Attempt 1
We would like to redesign the prompt.
```
system_prompt='''You are a helpful assistant. You will be given two groups of questions which exhibit different features.
You should try your best to summarize the differences between questions in the two groups.
An example could be:
###
Group 1:
[
{question1},
{question2},
...
]

Group 2:
[
{question1},
{question2},
...
]
###
'''

user_prompt='''Here are the two groups of questions, based on which you should summarize the differences:
###
Group 1:
[
{question1},
{question2},
...
]

Group 2:
[
{question1},
{question2},
...
]
###
You could consider differences from linguistic aspects, length of the questions, etc. 
The differences you give may include but not limited to the aforementioned ones.
'''
```

The following is the result for the modified prompt:
```
The two groups of questions exhibit several differences that can be summarized as follows:

1. **Clarity and Coherence**:
   - Group 1 questions are often convoluted and lack clear context or logical progression. For example, "The great general had suffered his first defeat, it now seemed there was truly nobody his opponent couldn't what?" lacks clarity compared to the more straightforward questions in Group 2 like "What was the consequence of his decision to engage in combat with the enemy, resulting in a serious injury?"

2. **Grammar and Syntax**:
   - Group 1 questions frequently contain grammatical errors, run-on sentences, and awkward phrasing. In contrast, Group 2 questions are generally well-structured and grammatically correct. For instance, "What did each of them decide to do while sitting on the stools?" in Group 2 is grammatically sound compared to the awkwardly phrased questions in Group 1 like "The all decided to sit down on the stools, and each order a what?"

3. **Relevance and Focus**:
   - Group 2 questions are more focused and directly related to the main topic. In contrast, Group 1 questions often meander or include irrelevant details. For example, Group 2 asks "What did the woman do with her tasks in the evenings as she worked hard for herself?" which is more focused compared to the wandering nature of questions in Group 1 like "James spent a lot of time walking long distances. He walked not because he liked it, it made his feet sore, nut because it was his only means of what?"

4. **Length and Complexity**:
   - Group 1 questions tend to be longer and more complex, making them harder to follow. Group 2 questions are generally shorter and more concise. For example, Group 2 asks "What are some common uses for metal rods in welding, home improvement, and manufacturing?" which is more concise compared to the lengthy questions in Group 1 like "You'd find a metal rod or two in welding, home improvement, and manufacturing whats?"

5. **Context and Clarity**:
   - Group 2 questions provide clearer context and background information, making them easier to understand. In contrast, Group 1 questions often lack context or provide confusing scenarios. For example, Group 2 asks "What did the owner do to prevent the new kitten from climbing on things in most rooms?" which sets a clear context compared to the vague scenarios in Group 1 like "The new kitten hadn
```

### What are the ways to compare two sentences?
```
system_prompt = '''You are a helpful assistant.'''

user_prompt = '''What are the different ways to compare two sentences. List as many ways as you can.'''

Output:
There are several ways to compare two sentences. Here are some common methods:

1. **Semantic Comparison**: This involves comparing the meaning of the sentences to determine if they convey the same or similar information.

2. **Syntactic Comparison**: This involves comparing the structure of the sentences, including the arrangement of words, phrases, and clauses.

3. **Grammatical Comparison**: This involves comparing the grammar of the sentences, including the use of tenses, parts of speech, and sentence patterns.

4. **Stylistic Comparison**: This involves comparing the writing style of the sentences, including the use of language, tone, and literary devices.

5. **Length Comparison**: This involves comparing the length of the sentences to see if one is longer or shorter than the other.

6. **Complexity Comparison**: This involves comparing the complexity of the sentences, including the use of complex sentences, clauses, and vocabulary.

7. **Contextual Comparison**: This involves considering the context in which the sentences are used to determine if they are appropriate or relevant.

8. **Coherence Comparison**: This involves comparing the coherence of the sentences, including how well the ideas flow and connect with each other.

9. **Clarity Comparison**: This involves comparing the clarity of the sentences, including how easily the meaning can be understood.

10. **Conciseness Comparison**: This involves comparing the conciseness of the sentences, including how efficiently the information is conveyed without unnecessary words or phrases.

These are just a few ways to compare sentences, and the method used may vary depending on the specific context or purpose of the comparison.
```

**Key words**: semantics, syntactics, grammar, style, length, complexity, context, coherence, clarity, and conciseness.

### Attempt 2
We modify the prompt in *Attempt 1* by including the **key words** obtained in the above section.

```
Output:
The two groups of questions exhibit several differences in terms of coherence, clarity, conciseness, and context.

Group 1 consists of questions that are lengthy, convoluted, and lack clear context or coherence. The questions often contain unnecessary details or tangents that make them difficult to follow. For example:
- "The great general had suffered his first defeat, it now seemed there was truly nobody his opponent couldn't what?"
- "If you learn something about a person that you're talking to, what is it called?"
- "The mother knew her children to be messy, so she always kept extra kleenex in her what?"

In contrast, Group 2 questions are more concise, focused, and to the point. They provide clear context and are easier to understand. For example:
- "What was the consequence of his decision to engage in combat with the enemy, resulting in a serious injury?"
- "Where might it be challenging to do your homework if you have removed the dust from this place?"
- "What did the woman do with her tasks in the evenings as she worked hard for herself?"

Overall, Group 2 questions are more coherent, concise, and contextually clear compared to the lengthy and convoluted questions in Group 1.
```
The paraphrased sentences are better because the original ones are of poor quality. 