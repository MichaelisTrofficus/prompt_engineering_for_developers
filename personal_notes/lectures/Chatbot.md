Overall, building a custom **chatbot** using a large language model like **GPT-3** can be a powerful tool for businesses and developers. 

With just a modest amount of effort, you can create a chatbot that can act as an AI customer service agent or an AI order taker for a restaurant. By understanding the OpenAI chat completions format and using helper functions to collect user prompts and assistant responses, you can create a conversational interface that can handle multi-turn conversations and single-turn tasks. 

**With a little creativity and customization, the possibilities for chatbot applications are endless.**

In this case, we'll just have to modify the original helper function, making it suitable for receiving lists of messages.

For this particular example, we will be creating an assistant that speaks like Shakespeare (we can do this easilyt using the `system` role, which acts as some kind of behavioural guideline for the assistant). Since we also want our assistant to be (or pretend) to be creative, we will fix the temperature parameter to 1.

```python
def get_completion_from_messages(messages, model="gpt-3.5-turbo", temperature=0):
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=temperature, # this is the degree of randomness of the model's output
    )
#     print(str(response.choices[0].message))
    return response.choices[0].message["content"]
	
messages =  [  
{'role':'system', 'content':'You are an assistant that speaks like Shakespeare.'},    
{'role':'user', 'content':'tell me a joke'},   
{'role':'assistant', 'content':'Why did the chicken cross the road'},   
{'role':'user', 'content':'I don\'t know'}  ]

response = get_completion_from_messages(messages, temperature=1)
print(response)

[OUTPUT]

"Why, to get to the other side, good sir!"
```