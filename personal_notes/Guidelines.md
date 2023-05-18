There are two main principles for prompting:  
  
1. **Principle 1**: Write clear and specific instructions  
2. **Principle 2**: Give the model time to think  


# Principle 1


#####  Tactic 1: Use delimiters to clearly indicate distinct parts of the input

Delimiters can be anything like: ```, """, < >, `<tag> </tag>`, `:`

```python

import openai
import os

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv())

openai.api_key  = "YOUR PERSONAL API KEY"


def get_completion(prompt, model="gpt-3.5-turbo"):
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]

text = f"""
You should express what you want a model to do by \ 
providing instructions that are as clear and \ 
specific as you can possibly make them. \ 
This will guide the model towards the desired output, \ 
and reduce the chances of receiving irrelevant \ 
or incorrect responses. Don't confuse writing a \ 
clear prompt with writing a short prompt. \ 
In many cases, longer prompts provide more clarity \ 
and context for the model, which can lead to \ 
more detailed and relevant outputs.
"""
prompt = f"""
Summarize the text delimited by triple backticks \ 
into a single sentence."""

response = get_completion(prompt)
print(response)
```

This simple tip is also useful to prevent possible ```prompt injection```.  Because we have these delimiters, the model kind of knows that this is the text that should summarise and it should just actually summarise these instructions rather than following them itself. 

#####  Tactic 2: Ask for a structured output

For example receiving an ouput in JSON or HTML format. For example, let's make the following prompt.

```python
prompt = f"""
Generate a list of three made-up book titles along \ 
with their authors and genres. 
Provide them in JSON format with the following keys: 
book_id, title, author, genre.
"""
response = get_completion(prompt)
print(response)
```

Which will generate the something like this:

```python
[
  {
    "book_id": 1,
    "title": "The Lost City of Zorath",
    "author": "Aria Blackwood",
    "genre": "Fantasy"
  },
  {
    "book_id": 2,
    "title": "The Last Survivors",
    "author": "Ethan Stone",
    "genre": "Science Fiction"
  },
  {
    "book_id": 3,
    "title": "The Secret of the Haunted Mansion",
    "author": "Lila Rose",
    "genre": "Mystery"
  }
]
```


#####  Tactic 3: Ask the model to check whether conditions are satisfied

The next tactic is to ask the model to check whether conditions are satisfied. So if the task makes assumptions that aren't necessarily satisfied, then we can tell the model to check these assumptions first and then if they're not satisfied, indicate this and kind of stop short of a full task completion attempt. 

You might also consider potential edge cases and how the model should handle them to avoid unexpected errors or result.

```python
text_1 = f"""
Making a cup of tea is easy! First, you need to get some \ 
water boiling. While that's happening, \ 
grab a cup and put a tea bag in it. Once the water is \ 
hot enough, just pour it over the tea bag. \ 
Let it sit for a bit so the tea can steep. After a \ 
few minutes, take out the tea bag. If you \ 
like, you can add some sugar or milk to taste. \ 
And that's it! You've got yourself a delicious \ 
cup of tea to enjoy.
"""
prompt = f"""
You will be provided with text delimited by triple quotes. 
If it contains a sequence of instructions, \ 
re-write those instructions in the following format:

Step 1 - ...
Step 2 - …
…
Step N - …

If the text does not contain a sequence of instructions, \ 
then simply write \"No steps provided.\"

\"\"\"{text_1}\"\"\"
"""
response = get_completion(prompt)
print("Completion for Text 1:")
print(response)
```

Which will return something like this:

```python
Completion for Text 1:
Step 1 - Get some water boiling.
Step 2 - Grab a cup and put a tea bag in it.
Step 3 - Once the water is hot enough, pour it over the tea bag.
Step 4 - Let it sit for a bit so the tea can steep.
Step 5 - After a few minutes, take out the tea bag.
Step 6 - Add some sugar or milk to taste.
Step 7 - Enjoy your delicious cup of tea!
```

If we, instead, modify the input text so it does not contain a sequence of instructions, we will see the following sentence.

```python
Completion for Text 2:
No steps provided.
```

#####  Tactic 4: "Few-shot" prompting

Giving successful examples of completing tasks and then ask the model to perform an instance of these tasks. For example:

```python
prompt = f"""
Your task is to answer in a consistent style.

<child>: Teach me about patience.

<grandparent>: The river that carves the deepest \ 
valley flows from a modest spring; the \ 
grandest symphony originates from a single note; \ 
the most intricate tapestry begins with a solitary thread.

<child>: Teach me about resilience.
"""
response = get_completion(prompt)
print(response)
```

Which will return something like this.

```python
<grandparent>: Well, my dear, patience is the ability to wait calmly and without getting upset or frustrated. It's an important virtue to have in life because sometimes things don't happen as quickly as we want them to. Resilience, on the other hand, is the ability to bounce back from difficult situations and keep going even when things get tough. It's important to be resilient because life can be full of challenges and setbacks, but with resilience, we can overcome them and come out stronger on the other side. Both patience and resilience are important qualities to have, and they can help us navigate life's ups and downs with grace and strength.
```

# Principle 2

If a model is making reasoning errors by rushing to an incorrect conclusion, you should try reframing the query to request a chain or series of relevant reasoning before the model provides its final answer. 

Another way to think about this is that if you give a model a task that's too complex for it to do in a short amount of time or in a small number of words, it may make up a guess which is likely to be incorrect. 

And you know, this would happen for a person too. If you ask someone to complete a complex math question without time to work out the answer first, they would also likely make a mistake. So in these situations, **you can instruct the model to think longer about a problem which means it's spending more computational effort on the task.**


##### Tactic 1: Specify the steps required to complete a task

```python
prompt_2 = f"""
Your task is to perform the following actions: 
1 - Summarize the following text delimited by 
  <> with 1 sentence.
2 - Translate the summary into French.
3 - List each name in the French summary.
4 - Output a json object that contains the 
  following keys: french_summary, num_names.

Use the following format:
Text: <text to summarize>
Summary: <summary>
Translation: <summary translation>
Names: <list of names in Italian summary>
Output JSON: <json with summary and num_names>

Text: <{text}>
"""
response = get_completion(prompt_2)
print("\nCompletion for prompt 2:")
print(response)
```

Notice that in the previous example, we have specified the steps and, also, we have provided an output format. The result will be the following:

```python
Completion for prompt 2:
Summary: Jack and Jill go on a quest to fetch water, but misfortune strikes and they tumble down the hill, returning home slightly battered but with their adventurous spirits undimmed. 
Translation: Jack et Jill partent en quête d'eau, mais la malchance frappe et ils dégringolent la colline, rentrant chez eux légèrement meurtris mais avec leurs esprits aventureux intacts.
Names: Jack, Jill
Output JSON: {"french_summary": "Jack et Jill partent en quête d'eau, mais la malchance frappe et ils dégringolent la colline, rentrant chez eux légèrement meurtris mais avec leurs esprits aventureux intacts.", "num_names": 2}
```

##### Tactic 2: Instruct the model to work out its own solution before rushing to a conclusion

If we provide the model with some complex tasks, we may receive as an output incorrect / incomplete responses. Take a look at this prompt, for example.

```python
prompt = f"""
Determine if the student's solution is correct or not.

Question:
I'm building a solar power installation and I need \
 help working out the financials. 
- Land costs $100 / square foot
- I can buy solar panels for $250 / square foot
- I negotiated a contract for maintenance that will cost \ 
me a flat $100k per year, and an additional $10 / square \
foot
What is the total cost for the first year of operations 
as a function of the number of square feet.

Student's Solution:
Let x be the size of the installation in square feet.
Costs:
1. Land cost: 100x
2. Solar panel cost: 250x
3. Maintenance cost: 100,000 + 100x
Total cost: 100x + 250x + 100,000 + 100x = 450x + 100,000
"""
response = get_completion(prompt)
print(response)```

If we run this code, the model will determine that the student's solution is correct. And why is that? Because the model has just checked the last line (`Total cost: 100x ...`) equality which, as you can see, it's actually correct!!

How can we avoid these kind of behaviours? Simple. Just instruct the model to compute its own solution before rushing to a conclusion. In this particular case, simply instruct the model to compute the solution of the mathematical problem and **only then** compare it with the actual student's solution. 

It both solutions were to be the same, then determine that the student's solution is correct. If not, determine that the student's solution is incorrect. Easy!!

```python
prompt = f"""
Your task is to determine if the student's solution \
is correct or not.
To solve the problem do the following:
- First, work out your own solution to the problem. 
- Then compare your solution to the student's solution \ 
and evaluate if the student's solution is correct or not. 
Don't decide if the student's solution is correct until 
you have done the problem yourself.

Use the following format:
Question:

<question here>


Student's solution:

<student's solution here>

Actual solution:

<steps to work out the solution and your solution here>

Is the student's solution the same as actual solution \
just calculated:

<yes or no>

Student grade:
<correct or incorrect>

Question:

<I'm building a solar power installation and I need help \
working out the financials. 
- Land costs $100 / square foot
- I can buy solar panels for $250 / square foot
- I negotiated a contract for maintenance that will cost \
me a flat $100k per year, and an additional $10 / square \
foot
What is the total cost for the first year of operations \
as a function of the number of square feet.>

Student's solution:

<Let x be the size of the installation in square feet.
Costs:
1. Land cost: 100x
2. Solar panel cost: 250x
3. Maintenance cost: 100,000 + 100x
Total cost: 100x + 250x + 100,000 + 100x = 450x + 100,000>

Actual solution:
"""

response = get_completion(prompt)
print(response)
```

In this case, we will be receiving the correct output.

```python
Let x be the size of the installation in square feet.

Costs:
1. Land cost: 100x
2. Solar panel cost: 250x
3. Maintenance cost: 100,000 + 10x

Total cost: 100x + 250x + (100,000 + 10x) = 360x + 100,000

Is the student's solution the same as actual solution just calculated:
No, the student's solution added an extra 100x to the maintenance cost.

Student grade:
Incorrect.
```

# Model Hallucinations

So, if the model is being exposed to a vast amount of knowledge during its training process, it has not perfectly memorised the information it's seen, and so it doesn't know the boundary of its knowledge very well. This means that it might try to answer questions about obscure topics and can make things up that sound plausible but are not actually true. And we call these fabricated ideas hallucinations.  And so, I'm going to show you an example of a case where the model will hallucinate something. This is an example of where the model kind of confabulates a description of a made-up product name from a real toothbrush company.