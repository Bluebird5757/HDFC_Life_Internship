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
fetched_at: '2026-11-12T04:52:17.241Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGZXAZOH%2F20261112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261112T045210Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE%2B9ZPAXfRBjS5idk8Xi2u80lpgOO6TqM3lhZJkh7%2FMmAiEA24xdSufRkGacUeyUOfy9%2BDEoQO5ZGlo4s2cz%2FYjxMNkq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDAOJc%2FJTOoKxvA33tCrcA03%2FKyn52V%2FsN716kcs6BVKOpeAg1t%2F4%2FxNKSWTtWF3dVAH97dGuUfTB1pW8lvQC8P0WtQ2UFHQE%2BFDS0UBTewCpEJjadnY4ysMtIObZamm6eEDXu8FqSwyzutw%2F%2F%2BLHZ0Aytbov95Ofjdyo4kpVGBeP8Y9M6DWCgfkziXESvU%2FpYMY8PDw2vCjsrJ5j5URrVZ37C%2BxO79RkfgNC2WuyDynE6ceJexb%2F9IWWIcyQ0KMgg1fRKPmRXN0IUBZtx8Q5mRExwK8QcTTNwCSuwqmCYx0J81Yzx6uhtKKJLMErrubmoGskYp6jz4e5pyWU4vQKotBZ7cQKqetZTKmNiHGBX2oCqK8VYPT7zNnvRhDlEes8Bz8SIvgmuyqK7wESi3bkSZAE3eXmZCHNPqJnMqD%2FpCgWKY9N%2FrJqdZC8cjIMSIXG0MdMpUCpmFWAQaDJYd5QaSH%2FiPgtiM7B%2Ba%2FKA%2BoLsgLUNhDqvMB7bIiK11uhmVD2OOzoWEGJtvz7ucwh%2FMqlivoejVouAEQIcsmUcaToV6SQmXpETwU3AyyTyf1YActlglSFM%2F9Q26AC%2FWA1Vjs7cY7BbgrHk0LVZRfIYsUq3e0WdSxn7XtAL1PUdNP%2BvM3a%2BKxNK%2Bt91mQRmuGcMIK%2F1NcGOqUBIEH17n8yer%2FfthmtvOggyKXSxV7lbiXIqf7Y%2FZNRaSB9iy3PFjfKlMIhxv%2BdWfVHz4qt515J2x1pTIwSe%2B%2Fq5zNaIRrCUMkY4V0p8PY5zOExDIAk4frCqnPZlS8w8AObo%2FEnhDd5DD0EJwxiK9j%2FW6x1gETdp%2B%2FKV0Eykim%2F0EvPrfCWbV%2FSqQYoY8ApHH8uloT64UhsA188HEZuh9r7%2FLO4%2FCm2&X-Amz-Signature=edbd7627b900fdbcf4607353a99f7619a9beeddfba71bb5d538775e6e410021a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
