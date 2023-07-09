---
layout: post
title: "ChatGPT Prompt Engineering for Developers Notes"
date: 2023-07-03
categories: blog
tags: [Machine Learning]
description: "Notes for machine learning course: ChatGPT Prompt Engineering for Developers"
---

# **Guidelines**
## **Principle 1**
Write clear and specific instructions:

- Tactic 1: Use delimiters like quotes, backticks, etc.
- Tactic 2: Ask for structured output like HTML, JSON
- Tactic 3: Check weather conditions are satisfied and check assumptions required to do the task (error handling)
- Tactic 4: Few-shot prompting - give successful example and then ask model to perform the task

## **Principle 2**
Give the model time to think:

- Tactic 1: Specify the steps to complete a task
- Tactic 2: Instruct the model to work out its own solution before rushing to a conclusion

## **Model Limitations**
### **Hallucination**
Makes statements that sound plausible but are not true

### **Reducing hallucinations:**
First find relevant information,
then answer the question based on the relevant information.

## **Notes on using the OpenAI API outside of this classroom**
```
To install the OpenAI Python library:

!pip install openai

The library needs to be configured with your account's secret key, which is available on the website.
You can either set it as the OPENAI_API_KEY environment variable before using the library:

 !export OPENAI_API_KEY='sk-...'

Or, set openai.api_key to its value:

import openai
openai.api_key = "sk-..."
```

# **Iterative**
## **Iterative Prompt Development**
Idea --> Implementation (code/data/prompt) --> Experimental result --> Error Analysis --> Idea ...

**Prompt guidelines**
- Be clear and specific
- Analyze why result does not give desired output
- Refine the idea and the prompt
- Repeat

### **Iterative Process**
- Try something
- Analyze where the result does not give what you want
- Clarify instructions, give more time to think

### **Issue 1: The text is too long**
Limit the number of words/sentences/characters.

### **Issue 2: Text focuses on the wrong details**
Ask it to focus on the aspects that are relevant to the intended audience.

### **Issue 3: Description needs a table of dimensions**
Ask it to extract information and organize it in a table.

## **Notes on load Python libraries to view HTML**
```
// Add description in prompt as "Format everything as HTML that can be used in a website. Place the description in a <div> element."
Then,
from IPython.display import display, HTML
display(HTML(response))
```

# **Summarizing**
**Summarize with a word/sentence/character limit**
Summarize the review below, delimited by triple backticks, in at most 30 words.

**Summarize with a focus on shipping and delivery**
Summarize the review below, delimited by triple backticks, in at most 30 words, and focusing on any aspects that mention shipping and delivery of the product. 

**Summarize with a focus on price and value**
Summarize the review below, delimited by triple backticks, in at most 30 words, and focusing on any aspects that are relevant to the price and perceived value.

**Try "extract" instead of "summarize"**
From the review below, delimited by triple quotes extract the information relevant to shipping and delivery. Limit to 30 words.

**Summarize multiple product reviews**
~~~
review_1 = """
abc...
"""

...

reviews = [review_1, review_2, review_3, review_4]

for i in range(len(reviews)):
    prompt = f"""
    Your task is to generate a short summary of a product \ 
    review from an ecommerce site. 

    Summarize the review below, delimited by triple \
    backticks in at most 20 words. 

    Review: ```{reviews[i]}```
    """

    response = get_completion(prompt)
    print(i, response, "\n")
~~~

# **Inferring**
**Sentiment (positive/negative)**
What is the sentiment of the following product review.

**Identify types of emotions**
Identify a list of emotions that the writer of the following review is expressing.

**Identify anger**
Is the writer of the following review expressing anger?

**Extract product and company name from customer reviews**
Identify the following items from the review text: 
- Item purchased by reviewer
- Company that made the item

**Doing multiple tasks at once**
Include all above

**Inferring topics**
Determine five topics that are being discussed in the following text, which is delimited by triple backticks.

**Make a news alert for certain topics**
~~~
topic_list = [
    "nasa", "local government", "engineering", 
    "employee satisfaction", "federal government"
]

prompt = f"""
Determine whether each item in the following list of \
topics is a topic in the text below, which
is delimited with triple backticks.

Give your answer as list with 0 or 1 for each topic.\

List of topics: {", ".join(topic_list)}

Text sample: '''{story}'''
"""
response = get_completion(prompt)
print(response)
~~~

# **Transforming**
## **Translation**
ChatGPT is trained with sources in many languages. This gives the model the ability to do translation.
- Direct translate
- Detect language
- Translate into multiple languages
- Translate into formal/informal expression

## **Universal Translator**
Imagine you are in charge of IT at a large multinational e-commerce company. Users are messaging you with IT issues in all their native languages. Your staff is from all over the world and speaks only their native languages. You need a universal translator!

## **Tone Transformation**
Writing can vary based on the intended audience. ChatGPT can produce different tones.

## **Format Conversion**
ChatGPT can translate between formats. The prompt should describe the input and output formats.

## **Spellcheck/Grammar check**
To signal to the LLM that you want it to proofread your text, you instruct the model to 'proofread' or 'proofread and correct'.

~~~
------------------------------
Correct and highlight
------------------------------

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

from redlines import Redlines

diff = Redlines(text,response)
display(Markdown(diff.output_markdown))

------------------------------
Change to more compelling
------------------------------

prompt = f"""
proofread and correct this review. Make it more compelling. 
Ensure it follows APA style guide and targets an advanced reader. 
Output in markdown format.
Text: ```{text}```
"""
response = get_completion(prompt)
display(Markdown(response))
~~~

# **Expanding**
**Customize the automated reply to a customer email**
~~~
import openai
import os

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file

openai.api_key  = os.getenv('OPENAI_API_KEY')

def get_completion(prompt, model="gpt-3.5-turbo",temperature=0): # Andrew mentioned that the prompt/ completion paradigm is preferable for this class
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=temperature, # this is the degree of randomness of the model's output where 0 is least random
    )
    return response.choices[0].message["content"]

# given the sentiment from the lesson on "inferring",
# and the original customer message, customize the email
sentiment = "negative"

# review for a blender
review = f"""
abc...
"""

prompt = f"""
You are a customer service AI assistant.
Your task is to send an email reply to a valued customer.
Given the customer email delimited by ```, \
Generate a reply to thank the customer for their review.
If the sentiment is positive or neutral, thank them for \
their review.
If the sentiment is negative, apologize and suggest that \
they can reach out to customer service. 
Make sure to use specific details from the review.
Write in a concise and professional tone.
Sign the email as `AI customer agent`.
Customer review: ```{review}```
Review sentiment: {sentiment}
"""
response = get_completion(prompt)
print(response)
~~~

**Remind the model to use details from the customer's email**
response = get_completion(prompt, temperature=0.7)

# **Chatbot**
Use the "message" structure instead of the "prmopt" to achieve more detailed behavior

~~~
import os
import openai
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file

openai.api_key  = os.getenv('OPENAI_API_KEY')

def get_completion(prompt, model="gpt-3.5-turbo"):
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]

def get_completion_from_messages(messages, model="gpt-3.5-turbo", temperature=0):
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=temperature, # this is the degree of randomness of the model's output
    )
#   print(str(response.choices[0].message))
    return response.choices[0].message["content"]

messages =  [  
{'role':'system', 'content':'You are an assistant that speaks like Shakespeare.'},    
{'role':'user', 'content':'tell me a joke'},   
{'role':'assistant', 'content':'Why did the chicken cross the road'},   
{'role':'user', 'content':'I don\'t know'}  ]
~~~

**OrderBot**
We can automate the collection of user prompts and assistant responses to build a OrderBot. The OrderBot will take orders at a pizza restaurant.
~~~
def collect_messages(_):
    prompt = inp.value_input
    inp.value = ''
    context.append({'role':'user', 'content':f"{prompt}"})
    response = get_completion_from_messages(context) 
    context.append({'role':'assistant', 'content':f"{response}"})
    panels.append(
        pn.Row('User:', pn.pane.Markdown(prompt, width=600)))
    panels.append(
        pn.Row('Assistant:', pn.pane.Markdown(response, width=600, style={'background-color': '#F6F6F6'})))
 
    return pn.Column(*panels)
import panel as pn  # GUI
pn.extension()

panels = [] # collect display 

context = [ {'role':'system', 'content':"""
You are OrderBot, an automated service to collect orders for a pizza restaurant. \
You first greet the customer, then collects the order, \
and then asks if it's a pickup or delivery. \
You wait to collect the entire order, then summarize it and check for a final \
time if the customer wants to add anything else. \
If it's a delivery, you ask for an address. \
Finally you collect the payment.\
Make sure to clarify all options, extras and sizes to uniquely \
identify the item from the menu.\
You respond in a short, very conversational friendly style. \
The menu includes \
pepperoni pizza  12.95, 10.00, 7.00 \
cheese pizza   10.95, 9.25, 6.50 \
eggplant pizza   11.95, 9.75, 6.75 \
fries 4.50, 3.50 \
greek salad 7.25 \
Toppings: \
extra cheese 2.00, \
mushrooms 1.50 \
sausage 3.00 \
canadian bacon 3.50 \
AI sauce 1.50 \
peppers 1.00 \
Drinks: \
coke 3.00, 2.00, 1.00 \
sprite 3.00, 2.00, 1.00 \
bottled water 5.00 \
"""} ]  # accumulate messages


inp = pn.widgets.TextInput(value="Hi", placeholder='Enter text here…')
button_conversation = pn.widgets.Button(name="Chat!")

interactive_conversation = pn.bind(collect_messages, button_conversation)

dashboard = pn.Column(
    inp,
    pn.Row(button_conversation),
    pn.panel(interactive_conversation, loading_indicator=True, height=300),
)

dashboard

messages =  context.copy()
messages.append(
{'role':'system', 'content':'create a json summary of the previous food order. Itemize the price for each item\
 The fields should be 1) pizza, include size 2) list of toppings 3) list of drinks, include size   4) list of sides include size  5)total price '},    
)
 #The fields should be 1) pizza, price 2) list of toppings 3) list of drinks, include size include price  4) list of sides include size include price, 5)total price '},    

response = get_completion_from_messages(messages, temperature=0)
print(response)
~~~

# *Conclusion*
**Principles**
- Write clear and specific instructions
- Give the model time to "think"

**Iterative prompt development**

**Capabilities**
- Summarizing
- Inferring
- Transforming
- Expanding

**Building a chatbot**