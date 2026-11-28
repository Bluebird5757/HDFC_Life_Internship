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
fetched_at: '2026-11-28T05:29:17.576Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XN3NIKIT%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052911Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJGMEQCIAyD3v5D%2BjWDW6NE98adVnngwtV7%2FtyUXsQib%2Fa%2F3yspAiBrMrW2zuIetX4ves1r8tI02uBQ1P7TarDQM39i7AlJjyqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsKN8F0iAY%2FkTJVA7KtwDW1r28903q94ULSl3TpV7woFJxlhXnMUvsEl0unpKFyk2si4w2u4XlOU8odHIpZaz2zBRWd22uDk%2F%2B1UnEVAXCnpr%2BGZaFW1hm1onHysFHB0rqv6A%2BZ%2FP26f7Uj4pYcsTpwnvctBKE2b72%2F4L4t0iao%2Btxz1g2o%2F7Xb447gNmXgyS0X4v5VDiOj6mym4evKK6s3VkyFyLf2vpON7WrueHtCVkoSd2bTjLHN9uuu8d%2B%2BxhD%2BYC%2BHVPQWm%2Bg8X1hB17Auy4tQNjfOmGbxXzSp833VvlZ65csaNrMSn0tTSA0VBI4b62wzq6gw4dlSrn8KU4FiqvhXsjTmtJYkY60PN4Vq%2FKoUum0Jpqx5kFoenIY25z8ZMOdlxanttoP9Nt0SxbcZDpEFTcWxPlITcDgOFjTUaQvKEZ4wsIuvGmZmHLW6AFQyzZDSWHvdNmTleVNtsOUruG1HRtS95HtCWDM%2FSRs4vplIKMt4Xdinv%2BiiNqZ2MBLoMdIIyfe5Bn5t6GRDfQuBczkeRlclLrd4hClzfWSbHcZ6Ljo2u6SsWNhtgp8J30xyq3Y3cZgdlr9%2B0tbXSppraoCko6xgSEm7%2BRZ0IAatKLammSMHQiTabQo3DUslcn2w%2B16pkJvHE5nqIwnsKo2AY6pgFCKKG9VjHlm1dJZLPg5FS8%2F7oFUBTqSQ74enVT%2FKQFe3kHXAcwdkZzwlLbYnWO23EgnMtNmvkF3dEiqYm1TNlwADBc8xWADb0jOITOlKhCdAIY9nJ27KY1Dsyyf27XcENZWm9MW0XOEF0IT4cDOj%2FFE9%2FmBt7udg5rQ2Mhf3lhaz2%2Fz5QN4722Fql8ilnTclBj5QGv91wLMbWtb6d00J3yHtWdNH90&X-Amz-Signature=61f0a50ad5a239c0ebb6e81ef30bf0d8d71421553b96555bf2289ec9d3405ab9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
