[[LLM]] can perform various natural language processing tasks, such as **sentiment analysis**, **emotion recognition**, and **information extraction**, by taking a text as input and generating results. 

This eliminates the need for collecting labeled datasets, training and deploying multiple models, and performing post-processing. With a single prompt, large language models can extract multiple fields of information from a piece of text, such a set of emotions manifested in a text.

Let's try this with the following prompt, which extracts a list of five emotions from a user's review.

```python
lamp_review = """
Needed a nice lamp for my bedroom, and this one had \
additional storage and not too high of a price point. \
Got it fast.  The string to our lamp broke during the \
transit and the company happily sent over a new one. \
Came within a few days as well. It was easy to put \
together.  I had a missing part, so I contacted their \
support and they very quickly got me the missing piece! \
Lumina seems to me to be a great company that cares \
about their customers and products!!
"""

prompt = f"""
Identify a list of emotions that the writer of the \
following review is expressing. Include no more than \
five items in the list. Format your answer as a list of \
lower-case words separated by commas.

Review text: '''{lamp_review}'''
"""
response = get_completion(prompt)
print(response)

Output:
happy, satisfied, grateful, impressed, content
```

[[LLM]]  can also infer topics from a long piece of text and determine which topics are covered in a news article. This can be useful for indexing into different topics and generating news alerts.** Zero-shot learning algorithms** can be used to perform these tasks without any labeled training data. 

For example, let's see what the model makes out of the next story.

```python
story = """
In a recent survey conducted by the government, 
public sector employees were asked to rate their level 
of satisfaction with the department they work at. 
The results revealed that NASA was the most popular 
department with a satisfaction rating of 95%.

One NASA employee, John Smith, commented on the findings, 
stating, "I'm not surprised that NASA came out on top. 
It's a great place to work with amazing people and 
incredible opportunities. I'm proud to be a part of 
such an innovative organization."

The results were also welcomed by NASA's management team, 
with Director Tom Johnson stating, "We are thrilled to 
hear that our employees are satisfied with their work at NASA. 
We have a talented and dedicated team who work tirelessly 
to achieve our goals, and it's fantastic to see that their 
hard work is paying off."

The survey also revealed that the 
Social Security Administration had the lowest satisfaction 
rating, with only 45% of employees indicating they were 
satisfied with their job. The government has pledged to 
address the concerns raised by employees in the survey and 
work towards improving job satisfaction across all departments.
"""

prompt = f"""
Determine five topics that are being discussed in the \
following text, which is delimited by triple backticks.

Make each item one or two words long. 

Format your response as a list of items separated by commas.

Text sample: '''{story}'''
"""
response = get_completion(prompt)
print(response)

Output:
government survey, job satisfaction, NASA, Social Security Administration, employee concerns
```

Prompting allows both skilled machine learning developers and beginners to quickly build and start making inferences on complicated natural language processing tasks.