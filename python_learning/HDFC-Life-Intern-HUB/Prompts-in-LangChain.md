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
fetched_at: '2026-09-27T02:28:52.509Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SMUCDB6Z%2F20260927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260927T022846Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIQC9h4ZIwXIwEvb8UZTB4lBlLYNj32LL%2FHoh%2Fk0Qcv%2F2YQIgNKxY3IpDDAZSgHSVN5uu70VuY8gMg20p85bEFaJyioYq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDC8AMMeuyLnjSYPVhircA7xq2SAKFNCULvRQkTSvsjoqK7oB34U39XgrDtnLFViaWROe1E4%2Bk6%2B00nn1QDbVXFLZsxmKxvfcRIwkl7XANR0DSSz26DGwUXgtOY%2BoatKtfYl2%2BmZLq4c5EQgVfn2AFA8%2B%2BoXeCNIMaUpLAcf3layxrDakc3gpBUgaZxRo7jrbnbB9b4%2FHZ6FEeD4NTB%2Fa75TMsBSjRzpWJY%2BRYDvpAPUW5X%2FMaztK%2B%2FuAlw9Jcr87mNxoYDAMRrD2qbrFCa1GUvHK7uxL3Q79neghWG2JyH5dFCciyIy2yKfdxhfcwiMaRKccexCj6YY1uor%2FM88WltQA5cqHivM0Hz7Q%2BkbsYmhG8wGn0G0k2Pyq%2FV0c8EpN1tD0tN1kbdj1TknPxdx%2FA%2BmNtfgVA6n2iZ3k9W9vX8QluJ5PuIvqWuuOW6qGEbkJ1xJ1qsdcyVv901LQEdqLW7IQjuUcbpUlgqfcnogMFmSExa4ROWRJSTFmdvnnbDQ%2F%2F7QFDe39uc%2BVkgfeA3%2B5a009pTv5xsLvW%2FC%2BqRvBWc4I8c%2F5doPrIkgOafwN9lJQldpjq8ItyzxkcBxDw5cs7zbek1e9R5I6eXvs6oh2MDtWOaIkIzTyD4%2B9Ty%2Bn15aIAKb4QsSshMAkUEiyMI7g4dUGOqUBg6kiSXT7t%2Bic5nb0yweJp5G18PU9E4avjnKcl59fAr%2B%2Fro9ja6lYs1zq01MBS82tH%2FSWq3T814MKVarHOLNDvCbb38HjVTxwn0SJfDgBl9dTPC4wUW%2B1%2B3xKDZar84zxKqdEJ%2FOsRsOuBQS7WtHpjJS9ZPE%2FUOaxtAms62Z8n%2FBokA3c0tNhfBEFt%2FLSnlU4D0OpX%2BuJCAirALVrspOtZA5YCQ4S&X-Amz-Signature=2981b0bf883a014882c2be1308ba82daf867a54d80b158957317c6f5cae82b32&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
