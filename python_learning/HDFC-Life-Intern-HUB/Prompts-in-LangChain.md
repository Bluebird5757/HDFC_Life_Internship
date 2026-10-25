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
fetched_at: '2026-10-25T02:15:59.498Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FVS6USO%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021556Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5TDwIA%2FI%2FPOqtPhzE1Bw8zxAwTlydqCYeILRO7uGlmAIgP5u8dKLN1cc%2Fhhl%2FCjhN4AZlD3oiS7aWhMh342%2Fmn1IqiAQIrv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLpMweSRsi1hFEeCXCrcAyvdMxEXqvDx8vHS%2FjOp2zCsUXJWTM2U9YL0DYb%2BhYJpvEsgx3vfLQi0HZiJGGwKEe1%2B5CazYjrJMGEWaIGYUcn%2FP4sjecteL6Gl9hhnBEjbhnQD96vwZ%2F%2FCQIAZOrVwEs2gvdO9aRUGk3uD4wTmY3RJuMM0DMvp%2BF99Il5L%2BTGRcq2M%2FJo4ibFo6nnBfdLCXCeBJCKCPxi5KZCJyHJ5rQtO5RV3uFmgS9A8AmSyVZqjPp1%2FASN2ok6a8W%2F7WzQFL5JVJmDShXoGubFqfACCVncK0OZN%2FNoMZ0aUpCAUIwuMAT2WdVzFvkM8Y6UBw7PLVhp5rTCTsWDJChw%2F1WUL1xjXrnLY3RfgS%2BxlCLrcCYwDMZuC8bmVKqg4s2qr48jil09QDwNOp9UwffUBWj7f1iksxT46d4K2d04B47mNmrhMWuVBt6BETc4p6KIP8b2se91y2GG9m487DLrm6yUcxEc3iKoVmG1Yq8V2FgsGMRuGZeLxsg6Or3yQm79BXOhQ1PkPuws1J60m8KimVDKKoQeCQOiSI4tCN01%2BfF7hx7I7nXBu1U%2BnRxAeqjK5HE7cf4px%2F813DJVXtnTDkLbB9yDiu1oStlvR%2FU%2BtFzQaqVuk44PNkRyoslPDkFZcMNDV9NYGOqUBI3roc7Y5XGo3%2BvLKgTgPVrcBbI0uVV5HQ7lnabN%2Fii3Y8T8TrzP81UVrwJVk9mFkIh0KNgZ5RLs8FxLHEAeXqj7OKRjpiew%2F9BKro%2BEKII1nSJCYKK0sLhK8ZC2tX1TQ57npN3mC8dqwjskxBkcOoJhC7BEhiL0NdMmPxzoihd%2BlPigyEpd%2BzCvwgDjzVKWE%2BIsf66MSniW2uBnXfhxPo1NFAQ4y&X-Amz-Signature=a4b76e928661626d8f80ca7aa57192c702f2ca9f36e58c47c91cbf9cec5ed521&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
