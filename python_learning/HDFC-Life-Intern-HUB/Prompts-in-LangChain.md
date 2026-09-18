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
fetched_at: '2026-09-18T02:08:30.139Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3VZYAQ4%2F20260918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260918T020825Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCID3jl24zB0zlocQMFu46esI6mEeqaJ7IMpvwkCIR6ICvAiEAxeIZ2PcqdjeeaN%2Braypjth9JbelJFbfReXe5T%2B05K8wq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDE4zopWNoPeOpG8CWircA5mwiEDsCYz6oK%2F9PK4Pzd8oPnrKSRzn6BYSgXw4AZLNuSOc7e%2B4ROJ3SK%2FBbaeBYuTBq3SwE%2BL4ij64cuoPW7GjbbmrAN66rX3LG6T27pYtJfLG%2BVvXRZ52K91juXgY%2F1%2FL2H9bhovNWCvPdLFPuLdXjxFieAmlxTMgOCC%2BaScmlVjJZ9mqFsQchoHIm3nCPzgsPac4eycSawb9HkMvvO7PRJnfveM9WOjiKmwgpp%2FGGe1OgCtpr7k6e8mxrAES0JVrU5K7IFS4n2Y7EeTmAARjGzcstKAKtvxjgCUOmZIR8JqVFXL7kceTrq7TXa8XS7XFNqhJPpLNCbq7Wwx59jH7NJPdkljbq4B1xbOC7csqS3VST7jrNSzDGZvsGDYW6kLLzeRDC15Vax1nBsNkgN4LsSXZTmVngb1M2Xnm%2FITIt9gUxI4l5L%2Bj6kSNNJ3xazFpSsH4EqXPdHJt8IZzBCimAAh3FpaXM62P%2BiKDQkHedDNRQAJBAYXdW6wJUWnzh1wcB5GlLKsn3RFWrhd%2ByHoJe4jyoF8UX2QFVr4UbOqPDsGEEuaL6t41paFCPFiU0Qid2fOE0QSfY0ftkC1VqyJBSB04vTVOyEV9gVNmWIqRrqJCfyG4Sq7VGD0fMLqTstUGOqUBzasGyEBTwClSKCTdhUIuPgwgvks9kAiXdLUQcIli1QD2lLwgeoPlvLvJLZqOszZYHjZXw%2B0tRk4S%2BdCV6ZzquNOgCFJtzmqpylyZxFaATZmEX7YPdGIn4cg8xFufqOscuK7Yh6mJsDY%2BBxxvy6oPssV9NkUkO%2BHjjo7pGo1A7qvnu0nJM0an2zilNg8WVSq7PaEE7Lnlhao2xPW1Y%2BKnO%2Bxt4qY2&X-Amz-Signature=1e1a5afcb10a75b5b3807e7fa2ec336f0f9b41268050e284d2e72f52d138ba42&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
