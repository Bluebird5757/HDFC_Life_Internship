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
fetched_at: '2026-08-31T02:17:40.181Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHJRGVYP%2F20260831%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260831T021735Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD7%2FSo8JyMCnz71%2B7gPEFqdmZ2oD4WrZG2gXWc0b3VBQQIhAO%2BaUYP3Yl8SPv1jBlypYzbrUXjS01wFDoZOeO0iGF9NKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxJj70qMjN80awVApEq3AMUyx%2BvRUDmZc2i96y6p0KQtmLnRhHFdE5LJQQjsOwnhB0v0QHaCpiqfu%2BXbZln36s5CW0eRlIaW1XNmg%2F1PIyLE5YH1H0AA%2FEPEmzprWZisV%2B51CFMXk7%2B4BlTx5wntvNK25mVDvJbHfSV79m0JQZSf7ODmPOWyt7IVTGQM54knYKZYJocwwhBPmdQrLj3zydQQ%2FsX1iSnOOgwNuWh177EiEIxIHf%2BStM%2FASJoK2jqKWOQeaTjdS4RsaYPr2HGwnBX8T3h5CG34Zg15%2F2Q0FCtIu1VZGtVaCkOOBcZ%2ByBWnXG9LZ6XmN%2BBsxAi9zKDJ76LcnsIQQcZivqjtKDcZaQ%2B6QEJsisV8eZ%2BpDsE%2FPhjgmAVT%2B%2BtpxGaxp75q6V8vlWKeasEdyn%2FhT7NteW396T6Cs6%2B7cUwPKANHlbaTvk81CT%2BpNS4hK2ejKe3H09O62ZopyF3kulvpqokWfU0BrhUFQ9t1N7g5vkbjDlGVpXcw9uztGAYb%2BY8nazduQfqyN%2FAigCyBKrzYeFbdqSAZqBtI4XxWHwc%2B3G614GFd%2BbeaH%2BIGmanTxkwD6Yuow5QDV%2FH0f5iN0lcRgc39nEbQBvYjk2LKRAlf%2FmckO4khXKO8lYQRfsaLalVwH3z%2BjDhltPUBjqkAeLC%2FWPvCKmPLLHZmFqplnpK4mpy5hsTCy4XnlZBEIWuzq2f9zaP%2Bk70rWEALm9V2MOnCnXaRCneN80SGyTesSHwP6WFQ5MSseLosjGJdXjbzS84YZtGVf4m81b4r5NPaekF67I4Q6gAyb43on3vVmnEDu9K3yzuSe3SkMgFWRT%2F2XjexnqktEY4tmVFd%2F3RluGRB95va7k8ikwR6%2BFOWaKNdb1t&X-Amz-Signature=4701809a03e51fcb797ce60783f92d86c022c9cc66fc6e57c53a140938eab0aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
