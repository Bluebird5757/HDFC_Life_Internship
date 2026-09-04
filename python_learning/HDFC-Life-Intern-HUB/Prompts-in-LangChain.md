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
fetched_at: '2026-09-04T01:57:45.035Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YFX55345%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015740Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCID0eDlIsZULtA5JbO%2FriB9e%2FV80KolxMm6Zj5qJhwKjHAiEA7id88JHR%2FhrxP5tGM0Ppby52uyXgntOgbnpLRi1foEkqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIi8ivGhwG48fV%2FznCrcAxRWqvggbk0JlBuIRbOlxTaRNI52uzYLOZLIShP%2Fhdit6r0l4gJA%2B%2B82zvW6RBpYf%2FWV1pnoZ01raZCcF%2Bwb87y1sLgiTlMdf3022HUmZwuVf11sctizsfrJoZDhjkAiygXiy1xyEJG6ls9mj6gb3A9k9Hbplyw%2Bk0EzxSaVIFAF%2BzGH1VAlIMireCsbCd%2Bb3aUzj9VsKjKMsczt%2BQGqX%2FtT4xUOzWSP9aWVEA4VuyYsLT4vZViLNIbgMu36l0eE0IRK8p%2F0iGrG1Lv8w%2BVd5jKaviXZd7o9OR7NoubEfKaXfVOyT5sLGurveMcV65h54GPnvO0hRX3Mln9kDA2cAy8295W%2B2v7VGN5cRzf7l2IZqBDn9ok4EhNYdFtg%2Bj7Xp7jTxrsL2Hj6NiVH4l4NAmrn9KzeoWxPC%2B32gl5VI4MlEuYV0My5E6Pymw7%2BhFmbNHEyYDwbE73PHGF2O2oelRG1UxOaMelymndTrwZ90fsTECfpcfCWUNgnTWgaNJhPXI485YkRK%2B%2FZ0h66kU5FL01K0Ixf%2FC4t3mlI8WCN%2BZ8d5DcU%2F9r3neeRmm9CAyj%2F%2FPyDxeejsM5gsHoD3RKq3lXRNnrfKLbMWZjhJqYceUw56X2%2F92PWfsPkeiCRMOKL6NQGOqUBs3Qr6HuGu3VMc1f%2F%2FHArM1Y0KhuHc78gJwHhMJEO2yDEHR3yqxN%2Fq3tZksZtjUqO1P%2BbMre%2FAsJ2lCKwAsFjuKAXeYCHOBmOAHL0CK5kqWesLWrKx8tXlCpv56tlyC09u2j5ofG4l2NCZz6xTxfOHcAog944LJRaMPBUCBayw%2Fc19wZw%2Bc%2BL1pEf9hxQpDj7shswLbemkWyCGW8HX%2BwF62QrlwV1&X-Amz-Signature=2c96c2c3b9c0adc8f2af6bb3ce6381bfcc6856b3bba6f638f741ac9eb6544eb2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
