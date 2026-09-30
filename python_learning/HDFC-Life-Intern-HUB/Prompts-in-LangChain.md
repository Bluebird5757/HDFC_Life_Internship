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
fetched_at: '2026-09-30T02:57:37.105Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663RHGBS66%2F20260930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260930T025733Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF2SuXqIak52gvQg9cTNj%2FDhh3QF9cV4p54SMTty2tKIAiEAme0Kl0aa4T76oSI7uDO9jSxLtR8QKpl%2BhMfWWV1AbT8q%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDDOkq7vR9cppOMmwdyrcA7aroIJVPt%2F1Yqx2d%2F%2Fmt4Mco%2BaBLxGmoig%2BNBE7N2RJBYtB%2FBDGcha9Bnf3rdjJk9Qbd88gC00eM5yy6GFHqRTswK3Znsd92mwnPnMDm7uHQ6022Ic%2Fv8%2B3yMLOsDCbpx%2FHNSLVZ%2FVupaDB6SZCv%2B9LcK9I0oStQW0O8yh0cUqrQ4P%2Bfk7KSJG%2FvqI%2BmGcnHwLBcEZJvBgLCj2GohragpDfKzEF%2BuuP7Uhe0AKjjBLgBELhj%2B5UR1t1UlZLqyuwKKu2Mh5IOQY%2BZWYjxzppuL81jyYBE8Ai201XgrE%2FmavMPchWHtkE%2FWjnIWs%2BWk5xpBWorJyuYrRVAKgG1FPxV%2B59vvwteFbeR9qYRolyYR4M%2FkvAR%2FWZ5X7nx4nZmKmfMtR9zM5dmaRO2VkIuGLnagyqtnyi6liX2OUMZb6HO30e%2B7%2By19xRF%2B22YL2Sit1oqeUJ3Nv%2FVxc2w%2FOOLvZ0avlvvMaqpLXJzFdRcr%2BEeJNOAHNKJJc25vUNISdvvZ0WgHBUZgYWIt15SNHY5ImP7LjSZUwQl4Qe%2B2eAiNS0GZaDPiA%2FMf%2BhcUMq7I9hoYTn%2FchW1FAUEHJgh1fIa7oPHJw%2F9Y30VH%2B8%2BustRIS51gPZVb7c3ZgkuAdGn1j1ML7S8dUGOqUBXgzUKp8MUhenkQfL%2BFeLwDjaljwjrTalSXpzCj3IrNu9M%2BkhDJ6TKm6uhloFt3X7lGKBoZkyV6vVZJgqhBH9lsUFs2YIYAYFuL2AJqntz0r7VeRrE6R2js5%2BAYmJJFqEqzG9%2Bf0YvBlmDQiilA4LY8thTEU%2F1pAZ20F3vnpMKDbvcre9yiOOjYreniR1x7QJVldROneZho3LePWW40VxA5QcBU9v&X-Amz-Signature=633b5100f90c901411ec2f81bf61c8932204db5d297e7dd66a38c3af90c07f17&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
