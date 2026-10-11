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
fetched_at: '2026-10-11T02:51:40.487Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXB45K3T%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025135Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFh8kZ2a66Rl7CsDviHoXm3TrvPf8oEWNe%2BV4hzxJqYtAiEA4VnV29IRf4hNVpwUeKZ1JPUOpGu6esBGcMb3SEzOicUq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDLru2zwtcsz4GfWE3yrcAz5UqR3rb6TtFmivwDD6Ibi7RInZeu5pJGKPCTUFoD1MWRUYwcz60e5CBmvqqEUgw5ya33F8rhezp01BR71VfWR1qo0sMEzNWKFuQGGi7DWrcRXJxmdV5NqbA1OMYjIISSm2Xo5QIkJHQ4P3tdwxENOrz7SeAbVb56rZ1SWMkdKpKs8dvHqL8DpJfjjKfq1QGTHDXRuO3Qw8y4YOUN48fYRV%2BXuMkEGvt5RLhqEcbl4mg%2BVYNWGZw83DLa0uzLLAh8xvuGayByzs9eUtIqxy%2BcPJmGAejpWGehWnFgx9woJbMtxSDUEeYn1zuqopXuVwviyaMBS72KrFgVIznof%2FzYy8ZhAdR%2BmJWTCvRs3t7Uv0ItIxO9%2BwYGn5SbRKh4RVlj7fAA9VTVEoUmpZgXNn15yPBE5eVY9Ph671iddOuBe88bHPaIw2toems4X0XJhePTZssKFzedFrY89mGqeoiaAuToht1RM%2BAw4EyfQ8hfPi1O0o0TBmVZgXUlgI5%2BDuMGq2l55XKj7gfj%2B21bccWqxUXAOG4jh5DexKOeEVIxnvA%2B2CandYb0CltJSY7O52nFJtH%2FsfTHtbwbR41cg0mD8QskDdhFCKnmLOaeJiOUyoONdQh%2FwV0dYncQifMLPsqtYGOqUBsNSY4rthiCCJt0zx5cD3G5uyVIz8HsIh2pFGX8fHhk%2Frm0DBbqzCcStmKua6QaFtEP7vgT7ekolKUhy5sJfpJCXkJjclWvrl60TKSOsg3aJDDaLOH2C3e5kizTLePAg2Mp4xay9ceO%2FvI1W%2BkuIOxvvsZvYffyq5GBml%2BRRU3SwbhoSsk%2F07P%2FiNwPG8ioWQ7Mqq1AwnP5iE9LOv%2BfuQ4pHO%2FGkp&X-Amz-Signature=3a8bbb59aadaa5d996b2a46df53ba6b9e641b3807ceb1371f148a5170baf6106&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
