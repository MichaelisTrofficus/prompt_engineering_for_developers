[[LLM]] are capable of transforming input text into different formats, such as **translating text from one language to another,** **correcting spelling and grammar**, and **converting between different formats like JSON and HTML**. 

These models are trained on a vast amount of text from various sources, including the internet, which enables them to translate hundreds of languages to varying degrees of proficiency. 

---

One example of using large language models is building a **universal translator** that can identify the language of a user's message and translate it into English and Korean. 

```python
user_messages = [
  "La performance du système est plus lente que d'habitude.",  # System performance is slower than normal         
  "Mi monitor tiene píxeles que no se iluminan.",              # My monitor has pixels that are not lighting
  "Il mio mouse non funziona",                                 # My mouse is not working
  "Mój klawisz Ctrl jest zepsuty",                             # My keyboard has a broken control key
] 

for issue in user_messages:
    prompt = f"Tell me what language this is: ```{issue}```"
    lang = get_completion(prompt)
    print(f"Original message ({lang}): {issue}")

    prompt = f"""
    Translate the following  text to English \
    and Korean: ```{issue}```
    """
    response = get_completion(prompt)
    print(response, "\n")
	
Output:

Original message (This is French.): La performance du système est plus lente que d'habitude.
English: The system performance is slower than usual.
Korean: 시스템 성능이 평소보다 느립니다. 

Original message (This is Spanish.): Mi monitor tiene píxeles que no se iluminan.
English: My monitor has pixels that don't light up.
Korean: 내 모니터에는 불이 켜지지 않는 픽셀이 있습니다. 

Original message (This is Italian.): Il mio mouse non funziona
English: My mouse is not working.
Korean: 내 마우스가 작동하지 않습니다. 

Original message (This is Polish.): Mój klawisz Ctrl jest zepsuty
English: My Ctrl key is broken.
Korean: 제 Ctrl 키가 고장 났어요.
```

This is awesome! With the simplest code we are able to detect the original language and automatically translate it into English and Korean at the same time! 

---

These models can also produce different tones, such as converting slang to a formal business letter.  Let's see this in action!

```python

prompt = f"""
Translate the following from slang to a business letter: 
'Dude, This is Joe, check out this spec on this standing lamp.'
"""
response = get_completion(prompt)
print(response)

Output:

Dear Sir/Madam,

I am writing to bring to your attention a standing lamp that I believe may be of interest to you. Please find attached the specifications for your review.

Thank you for your time and consideration.

Sincerely,

Joe
```

---

Another application of large language models is spell and grammar checking, which is especially useful for non-native speakers. These models can correct grammatical errors and suggest rewrites to improve the text's clarity.  **To signal to the LLM that you want it to proofread your text, you instruct the model to `proofread` or `proofread and correct`.**

For example let's proofread and correct the review below.

```
text = f"""
Got this for my daughter for her birthday cuz she keeps taking \
mine from my room.  Yes, adults also like pandas too.  She takes \
it everywhere with her, and it's super soft and cute.  One of the \
ears is a bit lower than the other, and I don't think that was \
designed to be asymmetrical. It's a bit small for what I paid for it \
though. I think there might be other options that are bigger for \
the same price.  It arrived a day earlier than expected, so I got \
to play with it myself before I gave it to my daughter.
"""
prompt = f"proofread and correct this review: ```{text}```"
response = get_completion(prompt)
print(response)
```

If we check the differences between the original text and the corrected one we'll get the following variations.

<span style="color:red;font-weight:700;text-decoration:line-through;">Got </span><span style="color:red;font-weight:700;">I got </span>this for my <span style="color:red;font-weight:700;text-decoration:line-through;">daughter for her </span><span style="color:red;font-weight:700;">daughter\'s </span>birthday <span style="color:red;font-weight:700;text-decoration:line-through;">cuz </span><span style="color:red;font-weight:700;">because </span>she keeps taking mine from my <span style="color:red;font-weight:700;text-decoration:line-through;">room.  </span><span style="color:red;font-weight:700;">room. </span>Yes, adults also like pandas <span style="color:red;font-weight:700;text-decoration:line-through;">too.  </span><span style="color:red;font-weight:700;">too. </span>She takes it everywhere with her, and it\'s super soft and <span style="color:red;font-weight:700;text-decoration:line-through;">cute.  One </span><span style="color:red;font-weight:700;">cute. However, one </span>of the ears is a bit lower than the other, and I don\'t think that was designed to be asymmetrical. <span style="color:red;font-weight:700;text-decoration:line-through;">It\'s </span><span style="color:red;font-weight:700;">Additionally, it\'s </span>a bit small for what I paid for <span style="color:red;font-weight:700;text-decoration:line-through;">it though. </span><span style="color:red;font-weight:700;">it. </span>I think there might be other options that are bigger for the same <span style="color:red;font-weight:700;text-decoration:line-through;">price.  It </span><span style="color:red;font-weight:700;">price. On the positive side, it </span>arrived a day earlier than expected, so I got to play with it myself before I gave it to my <span style="color:red;font-weight:700;text-decoration:line-through;">daughter.\n</span><span style="color:red;font-weight:700;">daughter.</span>

---

**Overall, large language models have a wide range of applications in transforming and improving text, making them a valuable tool for writers, translators, and anyone who works with language.**