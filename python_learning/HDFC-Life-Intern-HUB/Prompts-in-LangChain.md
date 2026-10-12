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
fetched_at: '2026-10-12T03:15:00.999Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46625IW46UN%2F20261012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261012T031457Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID7AzYLnhGH2NW6aCsSboAUE4WlxIyr8nO68MihX3SdvAiEA1dcdwgTElzA2mxUtvwS%2FKpgCtuTyZx9eoW8ncorWu%2Boq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDAiPUTNfXsR5mePvJircA7UTv7pXAabVDqvIIUxjDwc7VuC6i9dzk6%2BRPxAvMlul0miTTShLJBlajF%2B7S752oN6BpvtkIZRuN0G%2FmgJVMatAu%2F%2FbbOvvvpsE7fMDBwzQ6PxAamhqUUejQeYhYCi19it4E3XVsCsd4Hibp5CikuJMzsrbGqZs52scbPrKsTsTn57yqpHERnz%2FnQDPI8toWy%2FgQmf3zDa7OSZ2gJ1po0VxeKCAYGajSe0hNAMVIpJveNJo9S7PxSiFCsldcXtNmr3wQUGIKLXfxwI1K1Pt1esiUhtAjhYeGQuru7mMms%2Flsqyn8g4dk1liGuxtqkH44gZQRXh3zwnCooAEDM7vmQS%2B44sRgx%2BegZVIJiJqPoSx0g%2B6srtlkxyQwXADb%2BCWkxuXh1JoB6tELNgFVgoUW%2BGLN5Wa79lqyh26omZDJvVpVjywlQ%2BxNayWJZHmYHIqkR%2FRXQOSOimuku%2BNBEwupIETL%2BcayklMxtjmg9v%2FRzOpSGoCLXps8zx5cXEBw4AowOfHH%2BWkjqdmLJtIWsBbH4Q7IrDiejUK6LrHYx7u0%2BGV%2FITYtsCdj9NOaJ43xt3g3Gh%2BOTlsTAOjM8fd%2FHeF%2B8lWl4ZICbrWABFaufZpKH5wIL2D9Ltvp86KFHTDMLmfsdYGOqUBehgI84EJC0x2WG2hU7%2FDTraH5V3i9O93DAWCYIouDXrjirSCSyaZV%2FIw9gY6WYg0ynClkJtf8S%2BTfI1neCC19Rrrf0o8CKG%2B5mGrTZkSvvE0raCiHcBvZ4hhtyqLa2vnEhf9tgrrDXeAwMIIZBfv5z%2Fys5JtJ%2F2Gdl5aiKPhj7dSUGOjhvjT%2BYUI24R9RE6aW3TRguagteqZATDi2rFDUuE6Bm%2BY&X-Amz-Signature=414f68ea0dc7b44714ee4b1188b6bfe404360362864f9f3bf6d62a0b5417e1d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
