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
fetched_at: '2026-11-10T05:10:56.073Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665CI2RRWK%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJGMEQCIDID%2B5gVP71FPVQO0KeRNV8Z8iK4T9Q%2Bm9iLEQyKTw8MAiB%2FlwaLz2Hsew8Tbf6a%2FO5Y715V5Pp0h1jSi0AFjTF9ISr%2FAwgwEAAaDDYzNzQyMzE4MzgwNSIMR5HIVxCzjwLi5VjhKtwDtwNbAclZCB3xGXmWEmZTOWZoYBK0TYD1HWND129ammihCWESko5qai0HaficjvWxQxQ4uepDh7CGDkIYXJOW8tqajautWeg4aKJzPQhNmldc7SBDeO1kjr7L5GcNcTuvBHGEB7MiiIZ%2BTHd32EyWO8vNgOPrL6tsZighl8EAy0Q2OQreVC%2FNXtJj%2BcKRO49BXEKXgiQyj58ETzQm%2FztzTZN9BHTTIeVx9oxDGaljbk2Lfx0Iaz4%2F45N3khvi00dHmY60%2FxGAp4FxkMk%2B06Ecm9Ob0O87lLILWlw51h37LTyVVdCEHBflvE5xjiWmyw%2FWTXtK1CQ0%2BRXCF2SsK9UfunfEJUdJuNl01ygBAHctBr0EL6g2OmYtMVjQvO6xWK%2FLU39LsV5rrfsp2R2Pc2O9I9NE%2BhWLQKf06%2BLHaGpZl9h6nQIuVH7A5uRRFayEK6uU5LVFXAezutY4xyZ%2FhcZ2J%2Fb%2BXODcVw2i9ct229vaXLg8JDUhT8jJs3fM3A0AlYHivPNrSIH5ji3Di6l48rBK9W4YmTHzP3qfncW6h4TdVdPZJ8RotLzPTT0gWQ1CxqmCuPrGYzzqLaM65VX6SqMKGZ%2FhoW6mmM1fwLdBZiNcnyAaU13ZG5JbmU4htOAwrbnJ1wY6pgGPnb4Pk1EgZ7kASoYvfIBHtC77s%2B9mvEBek0B5FfzakCW%2BXQOeAQUK4jzkrjq%2Fb0cXnqpWbkPaYejHpL3lMhISQfP309YufGX%2FzqWvFtpxnhKuMDISfELHQyMyfdhHq6hHmDuunWjlkYxLQdbtvTBOcWRvaPaZzfd9uDHjvdF%2FShIwDqv7YN7%2F4SMlKGR1XocegPjRkReHeIPzoPEA3TeYphtJPjSh&X-Amz-Signature=f561493c14c0bddffa4bd606d8a5523b1f1a80d3bbd0590c8d45c70fb394e3f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
