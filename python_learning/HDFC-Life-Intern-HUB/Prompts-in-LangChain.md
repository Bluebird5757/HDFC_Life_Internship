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
fetched_at: '2026-09-05T01:58:09.386Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664S53I766%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015803Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJGMEQCIAzwbl%2BagnQ06cTMbSuC0%2F9xsFxFnuoywGBSVOIsT9ojAiAvJK%2BO%2FsxyhKYfxZIFW%2B2iRxzHUHLlsTkd%2BqaESDO0oyr%2FAwgDEAAaDDYzNzQyMzE4MzgwNSIMfActTqoyz%2FAfeAYAKtwDJAASrRYqy%2FgVbGwsR9MJ2y2MLVh2Ls6FO5gSkJ6aTpIGsX3Z4AyZXidHQ8ltOvahZzIu9RTgLHNaMUy5uHlRKmZt6bXanYV2MQi5ksql5WrGJutLxG%2FmR%2BolCY3u6uFBQ9Xt0t6iGyXhkUsXLhZF1mtE0ZixOZNpttxgQrw6eNaBF%2FT5hxsTisBvtQrkXDrPZNtARTjXhVLhJCriFXkoX46CPX2Sa0tVcSk%2FgJQpncCtZbzS%2FJeX4ybJ872V8XAr%2FE2rsUY0Uc1%2FV6RNJbzTjceuOmOeY6wlJLsLPHvcDLKA4VRrYcM7kreyAmIfTsVJBHIWIrY87FMOqt%2BWvZxF2gSb2iRZpo4y0Qck1adKehOV1ceLttjqGQiDYr%2FdSrfa7IPzIzQk4VtLiPvxQvlcy4SjSWwakljvsP%2BCHSFQ0WEfPFz7F0z5njS9Z8TPLerCFRA87Ev5tBSM0zEoeQFrQIv%2F9OCAHkCLGrIEgaL23YWgWp2gCOlrdCWK2SiCCqshaoj2o1OAWz5yJMYRTIzIsMWABO8MiPK9zY%2BLg%2FcKBpQmoIgoVifqanDZcjmeHNsuBUuQy2YyiRABqi60SUDg4nICTjn7ezEDccFaABvWB4Wh37%2FQYJR9BKGCXXcw2Obt1AY6pgHVc2cPigFA%2FUjpE6EMvrCQXSaXYuLEsgSxmuF%2BgA3CZQENpfCAr8sRXOQMT8VqOWO%2BuCp8hXpgnbuZrR31Kin347xHhPr2vC7CNXZOSjcH4eU5WaPvsHOpGbWNZLpFBM%2FcUjnKMXtcNkKymoB0hCT51tWVD4h%2Bp77NYwFFpIumFFee6qe2MexRzqa5eh0TfhwcJEZZAlGByjrthpmtmNo9M8mhtcU9&X-Amz-Signature=cd55ac4fc5419fbebcbc94e13276bf70978ea7f7ab1a2976aa226d661b480ed6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
