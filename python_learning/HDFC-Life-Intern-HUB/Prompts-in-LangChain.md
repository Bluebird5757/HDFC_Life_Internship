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
fetched_at: '2026-09-16T02:18:57.491Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662KBGFRVD%2F20260916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260916T021853Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIB5AzSGU21UtS%2BclsOoWvFmQqTTrgmrotXiAU1bp%2FYSiAiAM1I%2BAgsXOL7RzkmsYexat%2BhHVY4MdF31cc%2Bre06lkkSr%2FAwgKEAAaDDYzNzQyMzE4MzgwNSIMwn0x75nOUYoNRfYQKtwDna41aGSAyMKwQ07%2Fb1KE1Yjgaaw6XmxuZh%2FUmQgkNsp22kitAmvLJNdou30C0qMZuEbzBjwcTsqOUnYlce9P4ktm0vRPLPnZfRi3%2BuKkiMObV7TQ14urw%2Bdtm9Ojji%2Fof6vV9T5ElL%2BGw3vH0hepq0jSoZDz%2F4HPNEt45B%2FzKFCNUSJXOdSFzEuJDicIFNif3geBhP1gzg3XYBh%2FuCcK2m3xIWsBIFTbIXupY1FAlJMJ7KrK3btzdwm3F9xFzwpT7sZT4iN5pJ%2FyCN%2BqiV9nOkcTDFUlhG%2BpC1uG61h9RxD4trn%2BrVzAftYSLm0WZeYNZA9u1nbWC3vIaKwbvaOYlvMa7eXjazMci%2ByaQUN2XFHlKlu%2BbP1E0Z%2F9fhdiOVPHSh4n3MQxQjwhNajNEry9xx5Rv19KFSRdSOmcJKwXyhAkTAs%2Bnz2GsrIO2B8PM7l%2FHlY1joe68ZQfrpj0C%2BS4xXfeKjqeSGyi%2FwviqjDKHysYn46cawrU5P8umLebQs68X8h4gvHCKpx0ZdMAf0u7CKYcY9EQfGmPHRuwfnXMDT5gm3j0ZeQzh%2BQioj2ZYRNwRWEKnnYfVjwWJ13NNE3eYWsJKX9gsmwFLSLnu%2FD4lEkDlXe3HYbRvz%2FolIUwyNOn1QY6pgGgJd%2FhlKyM2%2FUYsD%2FCgXQyH611TxDm8x6m5rsDd1cgkyfruwAbssbPrWTzqFJrqJ6ZyckoYp3dzs%2BYAUDlLWFenzSoxTlZZwsugNezN8sLPFNbHBE%2FVC9hojGGuYegnemIUdOt9cUWIeU4l3XvGjrz2W9knPNVrjygN3yarVD1M%2FvIJC6k2TZ8PPQpneD2EGHjfxxT%2FpQPIbE32rwX3mIo1r%2F6JH%2BS&X-Amz-Signature=25d7c9512b361864c10eb2a5232ddf830abeb553b0f489fcf179fbb4c1e9e080&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
