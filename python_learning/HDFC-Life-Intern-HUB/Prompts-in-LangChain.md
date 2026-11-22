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
fetched_at: '2026-11-22T03:59:59.222Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIUAUOBA%2F20261122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261122T035953Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCkI8WosonODnwmzCYHpEubjNIQd7FCdzrKoCPECKQ%2FFgIgPnmacNuljWbT%2FtPTZ7G9Tdv1%2BG%2FOUTvhoSyVXVQXCdcq%2FwMIUxAAGgw2Mzc0MjMxODM4MDUiDJ%2FEoCTYSP0psjbdWyrcA9OMT6XiJcmeeWQpdXvTBEdBW1GyRyJ3Nw0UJiwpyX4xRPBAQemWl6R%2B2ya1cdHHP0L40N63QAG6MV7qSujdAAoIIFieE%2FeEpAsonNKj86pdkv8yslgGOWsR0WOr8p%2BKkQ9v61qOhLYvNWBfFoYj3Mkq2Gh9CTu5cbxeMd6mYz7M%2Fwjj2EXQuBa0NfL%2F3pnnrRbobdLInGC%2FiXTRDokvaqXm3u24ZIsdR0kEh3ZHsQBeEUmK1skxgPhmme%2Bbr7UaSfweKUNhqeJtC1G25esDjmAqJGoHcyKuppmKswjcrtQJoOrA8VM2SCCUg5xNXkC45zqM6VoydZrXT6J3skfecxX9nXCBXMLcKLP4hAadRjho9NFUlIJI%2FibS575mLBMAJ4OEeiCrXezt2xkBDXVWkA1UAU2wReQXUmTv9np0FrWQUyAv1BG8pm48oegMBV1Wa8urR9i413lAxCvn57k%2B5TcP5mYuoKINPXUm3woWvBc9Iy0pctrnQ90IdoXdWNeNCF57FrfSf4XFS71H%2FI1Saw6YCIsyijRaC3ZtMfU3MaW3KY6HWwVlM9%2FcksGLWvirR5zlyX%2BKKSikHEr91KdJJY1zXaEPHhwgiTMyyk6sdXbIihpJgcnqMGyslPAWMJinidgGOqUBM5o3iO0dRWjIDWWvAgjmi9YsyplYRcwe0Tq3UNiWo1HGdLjIK9A6UuxWtROrIkqBHe68qWTCKN1hkRO13cDM4at7bJEMXbVTQdHG5oHNAbPo0uhWRjpnkJ5NOu%2F8I2t%2B%2FbnuL2C26l1K72vkTUafatnFPYwVQrM2y5CrOFIq0iNJHzcMg9WqYU2O7gMT%2FPRY39E43ym6Z0UlDVpwRI0SpxtACZu8&X-Amz-Signature=219a03857555620161d026329ceae4dde7edc80732b3ce8774d08b960e331a92&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
