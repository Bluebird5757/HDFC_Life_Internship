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
fetched_at: '2026-10-27T02:52:39.907Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S2HXR3KH%2F20261027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261027T025235Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQDKazziOmTnV37ejVWXgstwyEP5m4KtfIV9LM5FENI3oAIgD8tcz9DtwJLrymA29zN0S5PDHU%2F0MQWUke1d2L%2BXs1wqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOw2JpwRTDJBrKBh4ircA381XTUdNCBbUNZRkKRO8TYUFc81FmntRAt3NeoTlv0BKWeAtdCeDhs4Wj3w5DBMKh5tfMN0XsvHtmO4EVusVzUgJdEsI%2B%2BFx7P3RhgJhoy7ZwIZx%2FmY%2FJg1Bxdh%2FlApsqnhe23lnim1zWpz4n0AAwL19YRdTdkSd%2FJ3TKf1VeOS56Lp2B0oWN%2BUrb6cQnkLZjnqrzStljEWBM9F6rfkPHKZNyoqOc%2BMrjP8HtHjLYX5rY0%2Bo7ZkwvqpUOfoEGdzk66jWETFPx3AN3GNDsWE9clcLLbYP8g7fY5%2B5J08VVfjnVTiRZhByTpAoTBZ3eBU8m6JlL%2BsnbqyfSCUerNnrhmb5%2BU35N56Dd%2BA7IFyz6e01cv%2BA40aAy0z7j5c8P6AYaJ9qVlbddRrAA61X1GtCkATNTKhayT%2F7XcxiEQpazSuHae%2FnGn9VDUn%2FTWj5%2BVjgwUAtLgrH%2Ba4O9X4WaQDb%2F%2Be9vu79fmbxvWK5nnYEmDn3SZ3CbToYmrJuhROqwd8H%2FNZicUeUGFDl7Mmb413yy5E%2FQfxAZ0swfdSQe6td9%2BiwxNwLrSg%2FlvdRDVFROj0j%2BAr67P9PIibNOA%2FimaXeRev8a6nwvvXcu0dYy5mMX3FtTudpUbDoKWuvdyxMKb5%2F9YGOqUB6p6G58HAXUWVNdLFvPjiecu%2FZGZUbz%2Fkk3%2BuyTGZDZC7tqqtN1jKXGn%2BOPU4L4kk7Jk7HtYa4B3YxCWElyF1yt%2FbMJqwbE5hy51m0vG5HWp9TANWJ8feX6bLWM4aYYl6hSaz1AZK3QLVbbKUkqXiv5BJh1nrgHG3tJKdL%2FxBky6VfKOE%2Fz7sFCb8uE6ZFg%2BFJxy0Jbm0tg7KNWMkHzI9GSiJlGMm&X-Amz-Signature=5a4222846a7c5a0902abb131e012e75c22400ef534337fb598e20eb28c708218&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
