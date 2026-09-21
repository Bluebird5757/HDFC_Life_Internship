---
notion_id: 3c54fa76-9938-805f-baa5-efa8aea48e6e
notion_url: https://app.notion.com/p/Prompts-in-LangChain-3c54fa769938805fbaa5efa8aea48e6e
title: Prompts in LangChain
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-23T13:46:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-09-21T02:18:57.363Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

They are the input instructions or queries given to a model to guide its output


there are static prompts and dynamic prompts but static prompts are not good to use because this gives the user too many leniency and the user can make a mistake in giving the prompt or some misspelling so we should have a template prepared for prompts. This makes the model return answer where applicable and if the prompt is wrong then give some answer instead of hallucinating  


so dynamic prompts have dynamic fields where the user give some key fields values


## Prompt Template


A PromptTemplate in langchain is a stuctured way to create prompts dynamically by inserting variables into a predfined template. Instead of hardcoding prompts, Promptemplate allows you to define placeholders that can be filled in at runtime with different inputs.


This makes it reusable, flexible, and easy to manage, especially when working with dynamic user inputs or automated workflows


WHY USE PromptTemplate instead of f strings?

- Default Validation (there is a parameter in prompt template that lets you validate the placeholders)
- Resuable (make the prompt in another file and make its json so that it can be used in multiple files)
- Langchain Ecosystem (chains concept)

## Messages


there are three different messages in langchain

- System Message:- the message initially we send to tell the LLM what is its role (eg:- you are a doctor give answer like this or that)
- Human Message:- the message which is the user gives to the LLM (eg:- what is the capital of india)
- AI Message:- the output from the LLM (eg:- the capital of india is new delhi)

## Chat Prompt Templates


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMNZB2ZT%2F20260921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260921T021851Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFqxqUeyuHQC8Fg8nQtIsZLLFDSuTTjpRdj4TS6cHcBUAiBGwnUiKkHXP3o98AohFCHJhytFAB%2B0v1%2B7RWi6WhA%2B1yqIBAiB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMFNEITqa%2FaOMjj%2BW3KtwD%2BBldIQQMr%2FqmaS653ONYDMeeOA%2F8iB18sov%2Fo%2BJagaTrFfUOeHB8xjH6bfKpi7kPhkPAW9mz2bieoNZyh1%2B73tgZalqh2L%2B30EMnP6vgRIZG9z0ZfaDXFz0Nsta%2BuU4vjMTFs1CuzJRJ6wmBLIrEWQZwGuCSrOAPqubXIeJGP0bC6L9g5fegBpZ6lvy%2F8v32KwBpsnp2CWJVJDYCXoux6AWxOwFn9SytShdgespkMUgdyQiriPTS69HP70pRw0p2yyHOMtamT22tbapDFrc%2Fo8I38m5h2EF6p3BtCMPxsazBlNt962u6k0DCoZtOfTyW53hgCYqt7FBLN37DPsPWLCivatw7sPYLQOxvvaS%2FLSeujDcMk1oyp6tv3iEWqK6jG4bjmrtlCZvTwUZwufdMZSM0m0TkeGjCrOUoLcPTTWWvQ%2BjZtv%2BWB4ZI4Ml0p8bfBb8MpIh2w1tfwzTZZ6VBqHvX3Qse3Q2m9e%2FUdjlMy95INaKZS2Aw5wA3ahUpaaVCL1sfY897SS8WLLuSy0bs6s%2BlEgO6sBDVWXv65QBABoNS%2FWQO%2Blh%2B7Wl69N7%2F3GuBcJLY%2FJgjdx0FD%2FJWBSBnsvEOkt%2BBEe%2Bbu8oeRqy4OpwoiwG7s6b%2FrNTpJHswufDB1QY6pgG%2FUFOEw4rjXyy5hMZBbkdOH1h5tzivkPNDhs3%2Bedk5oGUpy57hXn%2B80xiRPKmk2zHtBUpO%2BXKE6%2BSXNvcWO2syf%2FNpLNCbWmq6YEWnYEvOxhV0Ebe3uNAVHHQkESrLxj0xwqFwgC6QtLxU7nOwkPHSS6%2BZvfYW%2F76nvyTFrRJ%2BA3hL3uvOlrqNxp%2B%2FLZNZ4W5NFxwQcHqFTrmXgihb4HgOIZFO5I9O&X-Amz-Signature=c75ceb5de44f460968a399b4278e381f99d8f923d38ebab7fa6434c5f836c5f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


```python
from langchain_core.prompts import ChatPromptTemplate


chat_template=ChatPromptTemplate([
    ('system','you are a {domain} expert'),
    ('human','explain in simple terms, what is {topic}')
])

prompt=chat_template.invoke({'domain':'cricket','topic':'dusra'})
print(prompt)
```


## Message Placeholder


A message placeholder in langchain is a special placeholder  used inside a ChatPromptTemplate to dynamically insert chat history or a list of messages at runtime


```python
from langchain_core.prompts import ChatPromptTemplate,MessagesPlaceholder

#chat template
chat_template=ChatPromptTemplate([
    ('system','you are a helpful customer support agent'),
    MessagesPlaceholder(variable_name='chat_history'),
    ('human','{query}')
])

chat_history=[]
#load chat history
with open('chat_history.txt') as f:
    chat_history.extend(f.readlines())# extend not append because append will create a nested list

print(chat_history)

#create prompt
print(chat_template.invoke({'chat_history':chat_history,'query':'Where is my refund'}))
```
