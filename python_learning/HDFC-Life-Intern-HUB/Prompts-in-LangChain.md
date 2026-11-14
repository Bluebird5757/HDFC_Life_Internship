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
fetched_at: '2026-11-14T04:00:32.219Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664AQ5QM7A%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040027Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDKC%2Bl9mvmjaWnPagZBjziqWbq6JbXWjnmJb3H%2BdE%2BTrAIhAIXjwk1zeeVLGC13hYl8Nom6Sx1zq0u0%2FEWGfc0g%2FCbQKogECJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgysBJIZh8kcFjgJWVIq3ANlAFMBd93yar07%2BNNrvmH93z46XiUuIPmxwxenFu6F9%2FybkhdTXOAhAeojS4Li%2Bkjt5o4Q427upM7TJzWd0GVC0uPORNGV8iI5t6pPVPa25LvaYk2Uw%2BhovTeD4PPam51r%2BUK%2FHjIQQtxu2xoyUzXgAnZcOAbumpoVR%2Ft%2FaBzXR9X6i5HBui1h%2B%2BBOHzk2V4NgcX2AFmeod1fj9qEvQ7KCDnSDHK28B6PxRDR3WmIQO%2Bz1KnMD9IDzqZRGzEVlb5e1xWQvMktZ04%2BUOp7cgBK0bSIS9v%2Fli7G6hAphSWt8kBkmaKPasFERq4L4lfRzqFAzQH%2FH9z%2BC02hg4DxpCuKMVb0IMdsJ8UWcx70Jg8svmqkOhfnsYSytNGCtsWBB0MUwntpw4T0bpPto7iwdLbDl%2FpyCbn2fGwlnIixYMRXTigRBbWusKOTTj6yKPB2RM7HCrq7feNfcy%2BKGwtsKXBvlZyIyB497rSXWFOsroOYnJOS7E8ckfdUEzayjouCXwSf5sn4UjL7Td%2FfV7qDIYbCnWQKeXfxq66hYA6G0NFXeMJyQFiGk2xHjTMEDDv3Gmkacvqs2CUcA5T7k5XiDQb9zXmpUvTpDdgPl5r06D6MoTBgu6aqQ1bbpwHXfVjC13t7XBjqkAecHz6O1%2BoV6QjOTLYZQSXuuQIj0klGDN83nszE3568SBEWK5lmdRj0QwvaXxfnplMuThpztPYexFk67Da%2FM8fuj8CYr7M7dew5VOA5RqYb7ffDoUvBw6az5vcapjUw1L%2Bb6dczKl%2FuUfyKEewRzXApc5KMOjRdRyDw6A%2Fw5ClfvYlXGDMZI0IP7lb3XxARAd4Bax35jEpvxclLMvCRSiYeGWyqj&X-Amz-Signature=8dce1821c82c23425225a6fd8306a09cb2c8807a9574b4433c84131fd16ab441&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
