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
fetched_at: '2026-08-25T00:39:19.852Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662LK6UDI5%2F20260825%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260825T003917Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJGMEQCIEBP6fDa%2BttRs5edSozOG0p0wKhMue4YMc7n%2BOdZHPx8AiA0EM3X9%2FHnw7idAWq2n5ieDc7MiNXoBgrCZ44KMff8CCqIBAj5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMEYmxqX2%2BH0nHQEAmKtwDDi4bhpfAr18GGtvLhvFALv0hMwcFPRNCMdMgj%2F5qtStSQGB%2B%2FhQE6iRdoVbtL%2BvilDXU1JGrsQaZLxRfz8G3r3IXqwyB4PCw83cGGGnVOFe8nX22PeEaHSCrnKrvAhAxQL1%2F2pwBBgisXl%2FlgEdej2Rc0oL49xxI8PUTIBTYyNrO9fbMhbf%2BZCpBhv6GxEVeYhOZf7zyWKzv2iST%2B7lTQrI3Xa8rHeqZOkB8QLIvcFgrq45bqpAXwZDsXKu7nVI30mmOrgE%2FOUjjV%2BtdoXWOubeBnVX1ihGv%2FcRYIIed4CW%2FYAz6bmuG5dPpGfP9LdNIFrTOTdoFt%2FPLBAHiX7efmGe8XkgCJaTkSGesP5XaGqfLDX%2FctycJPaoEQAHeHEpvfxigzojy8cosqP6iu3y8DL4YJPZfa3swzdNyipJT41vWutxix62MNhjQP%2FO24I8S%2BnBlWWtRzFmcI87jXmhngHc8tKJ%2FG2FTgZMl5YDzo2wJA2VU7haF7FCKZ6aykfbESYvytnGu9IShDLoufF5%2BqPv41iP0W0SDG8IRsDdKG7rCk4YRLuiSsd4AhtCrg5voZJkLQpPUhGXHfnaHgj7K%2F6NLqwis4dHhPiThVkKmZ%2BYisFqHjJh%2FOpoVipMw0rWz1AY6pgHCwzyukouqlc75DvKJ0aIHIkt6qGyo2ixuig8sbbOKfaABv5TE9CW6nMZME%2FBSVclLYs4%2BbnAxoH%2BN%2FuuKOoTYK1j4Bqe7f%2BA6YnWERbpxPY4zxJyLxspaZ96ZizDVtDTh1tWUQehfQhgC%2FdpqGV3PFe%2B3ZIR%2BUVintXxiee9oAyHOWBLESH8V1MjwYLPpCijmBgyce7h5RPY8kaSpn%2F%2B7sp0bQ6WI&X-Amz-Signature=ce828407618243d21301519f09b71877c3e0a338e82573c223a2ce3b449e5817&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
