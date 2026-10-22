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
fetched_at: '2026-10-22T02:25:38.717Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XQKH5ZU%2F20261022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261022T022533Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDnEY5lykVwRwwAv01enDXgZpmjcCifIybvsWroRPWvBAIgQF2yvk0kU2mQ%2BQA3U%2F0zzs%2FJTacfQ%2BTz8%2BCkz3s4A8kq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDFwstvpH%2FavuqTVbJSrcAyv0du7QMEDoJ5luqWKCm2pfouYgGaezfzNHDIcXDIh44PtGddOpoGY5fCQNoBSPMdTbonOHiqPzBTFkChVom8ET8n%2FdzDnYBPxhWyME76jUQB5q9HQNL9oPbKVVgD%2BBbeKyagzaQtd9Wak5dhv2pdi0Ari9RjvhWq6eUHNrk5XLrJfsoNX4fw3ZyNRRfMZCUjMTOt5bzAlRel1Q1Z%2Bqw3DEGeTKr5LNeoqSkOMDX8D5wLLTBy1axZeKQJFqBNkoM7yghum4y%2FbbEAjQTyl0X2KDV3qsyvR6F4bD5S95prTsyz8U5HAOQrXK1NAxV2wEQr109VjvWNsOHufG%2Fb5Nlp58pdReFjErp%2BwO2GDpA2%2FvCmBDLaiPP6ktKu548qmBZnlUbqtVh%2B02lS64R4SI38VzAgnlY14a953njV2mcV7MeosdE4M8Jbx%2F2LOBcPn30inlTXY0f0LbgztiE8yIEjggvuuEfSS776ZJb1OYt7AISf%2FbQmeRT8wMtDMCk9Ou3NK37zolf%2FZ65QSn%2BBk9%2Ft7IaIJ%2FoLmtJDE%2F6swX5F9QpCJ%2FezJwnYzJYW09sGU5rPzeXKCkQrtQupOHfD5m7JEVFJvYtTUhJCEbnMeIEgZDx8BTJQbI2yB6lw8pMNme5dYGOqUBiK6opqLm1uAKvnusUXRRyMXXDoHLqmWYWstqgo4zSJWtQ%2F5OLXT3P51j9MLPo8or0n8NGUjDPgB3mKvFEsyGDuTH050gS6aoOyrnK3Vh5ZzQ451Vj7I%2BvjAuUTm0fWljXuPvXHHJPkrbddeOlkT5diu1vt6JWwx561NvyjU8vQeGImwQjmoGv0uYjZeceT689KK3cb%2FBUHn7ivC0i8C%2Fz%2FGJ%2Fyhm&X-Amz-Signature=10ff646ecb59c485ac158b16281a38f4447685dc1580a1624e5d9fd9749b6e68&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
