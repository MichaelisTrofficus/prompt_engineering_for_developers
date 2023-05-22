There's so much text in today's world, pretty much none of us have enough time to read all the things we wish we had time to. So, one of the most exciting applications  of large language models is to use it to summarize text. You can do this in the chatGPT web interface. However, in this lesson we'll show how to do this more programmatically. 

Suppose we want to summarize the following text:

```python
prod_review = """
Got this panda plush toy for my daughter's birthday, \
who loves it and takes it everywhere. It's soft and \ 
super cute, and its face has a friendly look. It's \ 
a bit small for what I paid though. I think there \ 
might be other options that are bigger for the \ 
same price. It arrived a day earlier than expected, \ 
so I got to play with it myself before I gave it \ 
to her.
"""
```

The simples prompt we can think of is to simply ask the model to make a summary.

```python
prompt = f"""
Your task is to generate a short summary of a product \
review from an ecommerce site. 

Review: ```{prod_review}```"""

response = get_completion(prompt)
print(response)

[OUTPUT]
"This panda plush toy is soft, cute, and has a friendly face. The reviewer's daughter loves it and takes it everywhere. However, the toy is a bit small for the price paid, and there may be other options that are bigger for the same price. The toy arrived a day earlier than expected.
"
```

The summary is a bit long, right? We can try to reduce the total length by making it explicit in the model.


```python
prompt = f"""
Your task is to generate a short summary of a product \
review from an ecommerce site. 

Summarize the review below, delimited by triple 
backticks, in at most 30 words. 

Review: ```{prod_review}```"""

response = get_completion(prompt)
print(response)

[OUTPUT]
"Soft and cute panda plush toy loved by daughter, but a bit small for the price. Arrived early.
"
```

That's better. However, suppose that I want to make this summary available to the department of shipping and delivery. In this case, we'll have to also tell it to the model.

```python
prompt = f"""
Your task is to generate a short summary of a product \
review from an ecommerce site to give feedback to the \
Shipping department. 

Summarize the review below, delimited by triple 
backticks, in at most 30 words, and focusing on any aspects \
that mention shipping and delivery of the product. 

Review: ```{prod_review}```"""

response = get_completion(prompt)
print(response)

[OUTPUT]
"The panda plush toy arrived a day earlier than expected, but the customer felt it was a bit small for the price paid."
```

One thing you may notice is that **summaries include topics that are not related to the topic of focus.** In case you only want the relevant information, substitute `Summarize` with `Extract`. For example:

```python
prompt = f"""
Your task is to extract relevant information from \ 
a product review from an ecommerce site to give \
feedback to the Shipping department. 

From the review below, delimited by triple quotes \
extract the information relevant to shipping and \ 
delivery. Limit to 30 words. 

Review: ```{prod_review}```"""

response = get_completion(prompt)
print(response)

[OUTPUT]
"The product arrived a day earlier than expected."
```
As we can see, **this output only shows topics directly related to the topic of focus.**




Summarizing