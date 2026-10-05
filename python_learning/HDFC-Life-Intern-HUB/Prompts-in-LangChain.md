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
fetched_at: '2026-10-05T02:59:34.806Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJ7WV72V%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025930Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDHSsx5nKye%2BE1LCYOYBuv7Y5rSRxXVvDZ%2BD7NblI86XQIhANd%2FXvQNQi9zEtEF6dAaK5pvdzXKEegu45%2FJSooFyxOUKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzO2MG%2FXGVmljRRVisq3AP2V%2FHjugEk9xk2qnO0JMIugQTeJv2TGcY2LMgbW2AGC%2FB5K5q95xq8Uf%2FvL%2BEvRqH6l5DFNS1RQbysdR8awl%2BNGgVTnN4uvsynLNM8AcShBGdN3GdhSEsh41gfpKhUdTVi9w6v3f3jf7RNOsGvYKlQKve%2B3xWXHWgpKrcVqrTj8eu9FBlagXrrp9dW6MkwvSd6zzij%2FmyVQjUiPbfxz14c5Df%2Bl%2BfTfHrqAkxiD7jsnqHMDMwB71ZzQBTlZU8z%2BuK2%2BBbj%2Bw3GvhLAxTyCxT6angn3LroI3W690jSR6C87kiFF7Qim7RZ8sbYLi6Jzo59ZSF5hmqpiUl5jlS0GYY7hpYBNUkcnWAkX5F8ZL8wqY7hRg%2FyXqFTwYLxpuuFMP3UdkVtcFDEwzQShaxUFf16WMiwhhSglYLeabnAvrLRxA3M4%2BE%2FAcX37WolNQei%2BUh%2BKcOCY%2BcDPw%2BB2CWs3RQIY0YXlkRaV4zs98a%2BTw17po5SOYrutyvm4Iw9jpKemxPsG9BBKv%2FmW3iq3vfBtODdwu%2BYFmQeaBad5Kr%2BQuF%2Bk8M8pbtvemEassY3sBr1rssq8AKtsgAQGJ4EBGFGOROVXe33tMI8hzaueRQv52gTps7Ad1XIIxQ2XOj8n0TD1i4zWBjqkAX5KRkrmP%2B7vHGvCnkTimq0DIG4ARx50kCN3gV6NvJysTcJCpm3i4xck2RNKIdnF6V%2BgKwwuKlqkb9jTowOwKi88zE4jjxsH%2BbTXRi4UEggwLL51bejt8MU4feW5lGmpiu1BA0loaaeddlmjiqM2Tnxoz6pRwgPoeOi3B9jKduZo2UM41DwqRfnFJhHkV%2BwcESpSsv8CKarsYDtkZbMbz%2Fbz%2BwKc&X-Amz-Signature=9659f8280b2a3d917574eea62f8468fab783d27965b29ffa8c5edf710bce0cff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
