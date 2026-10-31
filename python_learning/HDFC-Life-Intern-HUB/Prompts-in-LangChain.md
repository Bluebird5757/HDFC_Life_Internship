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
fetched_at: '2026-10-31T03:14:00.637Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46662B6CC2Z%2F20261031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261031T031356Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJGMEQCICCURzPK9hs6b9wraz0yl0VmIJ3lkkTf9lg41RXfFCh%2BAiATnr%2FmOUkwm%2Ff6Ne%2BQQb1Dl%2FSVcu9Pktr2zoV%2BLPfd5Sr%2FAwhCEAAaDDYzNzQyMzE4MzgwNSIMQnEFmyhfSbbDlFjSKtwDVMYWpQNOPRzantDNG8YE51lbb6pADidmjuMY4jrhEFMPZ889KXYXOgyf0tYzcnTJoNIupk7atV5b1XCJRhW%2FaZ2mx2owVNSzYHOPbVduajyNk2hD9Sby%2FWXjcKPvVD2lxIMAFwO%2BoeSow%2F3VZ9I1EOK%2F5MXvpOl3qRs81KUMzcYLK2Y2%2F%2F%2FCWGYw%2FbPEdpu6QYOTE6fInn17VXfG4KXe2GVcvordGjKZZCXMyga8BAsEKNPNzURugPmu68CPB%2FIYmxHVa6J7jqTgflhNVZ2AlY%2BujLbOtVnTCaOSgAUYnIMrA7O6oYkIfornoFWYM8Y0En8B0ttxT6voS6Nkb6uVpc0YtlF6OVsIkrHI%2FF6IfJoZgyamPGkYf3pLxzj61Jnjx8fegTutCNXRozJ2m3Lj6Q89KTl6XiKK9oddTtU3bCSuMPHKl5j4uIW4xB2BBbm5KxiF7KWtyl4e47OidkOTEYsHGXmStv%2Fyblc7ip%2BvksXjKFOkbpNK0dr9OPlpFLXY6ImDOzxrn15jhgqT8wcN8QEpjlsIKgs2%2Fe8xdEpF4VoiLVrHGm%2ByHYQuWci%2FSNKLWWoUABGLITU%2Bd6aU0BLM6hFVVba9VhEWjonH99jWFCZmOijXW5FNblV8aecwvZSV1wY6pgGboN0z%2FfUAEUDNNIkNj6OkFv8VUl4C3gQBfRulOAc%2BnlDximUY6GSj2Et9Uqj5lRfQUh%2FKNXk9iYnBLtD5xZ20prqnHGit23UBfGXWNgbOdWHsvQlAuzQioxbPUpzsXKhKUwF3D4OTbjrLQRS7aJ5m7uAt4LBAzZERE8NNOXQKLbnzfvz4uAQdsupJ5BbC2Nw7%2B7seQkk5XF7wNvtkQVSK2828e2tk&X-Amz-Signature=9a60638faa2451bcf1ece92d08122f936b21b1ef4da4311cdbd08d7d2ade6961&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
