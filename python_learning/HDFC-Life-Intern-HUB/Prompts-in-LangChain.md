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
fetched_at: '2026-10-28T03:00:14.667Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QPO52GZS%2F20261028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261028T030008Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIErpvH8kVG2RAK8N3CqOhXEH8Epdyh3paiger2PuItkMAiAiZotuz%2FpV2B5dcbaCEEFT55YqNZr6QUWELz1e7iG%2BoSqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2Fy3KKyNSjQ5xRbdtKtwDOf9JTW4B8xR00zwMuvRNjW%2B9lOBA1Yz3znZMz5B11KSmSd4SFwKEfOhHFm%2F5m0VZ2paGJgNMbYrFFDkuxSsplEvBP9VhZNjtJXRFxxUCalZ3kPUSREPvoPQe6AvGZte8ItQTFt2ZLnEBnmasmK9P9AS7wDIb5WfYAdh%2BvtoqW0dij1ArcAo%2FZETpMadRRreDInXpGlD9fJsMJbpIrXt2VUHxeBMsZedeT3URiJYizIH1%2BY15HnOfMJexWy%2Bd4m%2BfLNyYSSxK6jiurviqslWDIac4Y0po%2FVg%2BSo3LRfWSgODipz25GVLj7n89SdQMb8i0lXC39HsBYPAoUVO7zR20FgJHSOBB%2B66NDpjMGs5sZaCLHsBP3E2usUv347RMO%2FDU4xZ%2B5SDBtGRg4P07O7j8rHr7ROczJj29LkzyuSsoU361utq0kbIbmRKxh2Mjnaa%2FIVz2KJobe8lWLMuxlS7hyoCe1gtsCfdMW5DLk8L3W05mEplJiScPW%2BB9Dhu1UrtRo253TjPZXi0IfYTL0SengSkROTddqdAEp2QZtv%2FUhkqSNvY45r%2FGBSsUB1Fv77%2FZviwSx%2Ffu1%2BF%2B5gRxdQu4y0uIWotOMvc4UMPRPk4Pb2AaqPdQdyCQCzdewYEwnbGF1wY6pgEx%2BXTNY2YPBev0AIPKkWLO3%2BUibbkBgOn1f5yCUjRtj29tYv0s3w6Uo%2Fkz8%2FPMsKmiJGLSl3MZIjbcu%2Bw%2BLOj3d1JxcR6ICpYI9hAr3QEfmEK691upKcftiH4UO5%2Fx9GzVNALZzpZ0sAVbnY5JgRiU2Uybv4%2FsN9mTqL%2F2f%2FWckUwW5HXoRdhHmhRad%2BALFmrsWW6tteVYxANodvf2TgcbRGTo5LVP&X-Amz-Signature=5a4148c8698f56fb7da8a070c03ec9c097916851df21fc9da54d5dbb70cb3a58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
