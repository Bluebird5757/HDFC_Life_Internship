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
fetched_at: '2026-11-23T04:43:06.547Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6QY6YT6%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044301Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEUG9elO3lZxikAk49XrXCIQnFZygy0KVBH6enHfUhljAiEAk1w2qGMwsJjVc%2Fddx4n7kCYa0lMdtZt%2B5ls4fD7wGWMq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDJ4h%2BtXuo55f1jQLoCrcA075UCosDqLUFI%2FDnH8NPcebWcI3TnIacRPVq4aKe0PqCbsUZpK0TMmjpr51XwPrwDtV2nj%2FZ3iLf4z1lSGKtWxwmD2TnnqDmNOeD1FYGwHkHCbXvX3kfsSaWa7jI%2FuBJ0F3TKHijIuQcHFvfx%2BFHOPx9TYf5e8TFjr%2BIKfsPEUo4NkbkrXLsl34m8bBm6H9CaI1ZHKBzncCeR7nuroxD%2FFfrPl4h0ml1zdRyK44sOCXxSRi7QNMJWkxGygF12DkPAV0ISMPwQ889FX8oyv66PlS9cDDFLYQYiGjgoGdsrBeqeFThl3hJ2y2jGjRzQyVousyFhaTo%2F1ZUcNSdV%2F1qlQnvRsFpV5clnhS6Mgp1VNOM3RRhG9VwFFV28ctlMavgzA6qU4HW00h9pvhXv%2FTg6avDYVGAPK%2FyhoOf0Noy9Sr4CWyLzD%2BbEUo6Mrdl24%2FitpVZQ6jFiJ3s%2BBDXs5Yv0tzMriBuwJQDSAmeHpNqWmKLWjy%2BYl3oFcEmSVmLKyVjMn9XVxx7GrKb7CCOeaDUwDZmxJh8Fkkl4ERV2hy1Xr6Hoc0m%2FintWamXCuOt3qUc%2F8P0dtP7QBipoMW8ow5tvAWUfmMtkHAcnzuVFtDUp8wzMmwcm1paU91CA8YMOT5jtgGOqUBnjZOzEcO9usLtZ47NbDUKbIdLe%2FxNhA%2BxQdFuO8Ie5vdfLK3KbD7g1tPgG0c%2B9uupMYrO7lYLcjromg8hlXB66FhrzcbdjVLBWWJcz%2FIUgi3zpQyHNlu7G7Z626sN7XD3xCyU9h6JWDKtyKy%2FTUlTq9j06W5Y4227nbQ6V4CEU8Kx3yOH7Lr%2BNZ2AiV%2BLmOuZ47iaZajD1i5bx7kASD8isBDTQ5j&X-Amz-Signature=25f2b946fa2370dc442de18cc8944be1c9ec00bd23ae8dc84aae242130e1992c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
