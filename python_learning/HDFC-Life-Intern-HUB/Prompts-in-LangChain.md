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
fetched_at: '2026-10-02T03:06:17.141Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWDTQZTK%2F20261002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261002T030611Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCxAqYTaNb9GWNYXn989ROBZyZZZe7t7TPxSTeqH4nM7wIhAIt6%2Fijd5DyVz%2B0D045X0IT4vOl%2F3ebWmlU718phCbm6KogECIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxr6XMkONcOXapp%2FV0q3ANURGmDo36IGhVjOf2Z%2FNmgDDoPBqkO3eMyoC0dhheHn6S0xN%2BvsawtVwvNLEsvStQuOcMgZX6YAqEIyP0QXMoOo0rQ7tZzHJdkBSMR03UUDWxWfcm188l%2F2Wr7wzB0sKdeTt3OUlcFdt8DJgl3oYShcUx10KRhQAwuIVEH5kZZEFN8vMe6PJ1vEWqkrJwa4LYcBWc4XUUJJ%2BQuZe6uGj06HTu3Rl5KLg436wjx%2Bj4%2BIE%2Fipb6pH4YRFUUiY0brsHy9pj7PP3KdH%2FfOOzozut0adnHLypjj8wne8cW3Lw9YNZ0i8tkgMx%2FUa2mFvFMZGYSSsOz9klYo9oBSB1I7l7oIKgTzFmuNoG96bFSPsnwHRyXDjNB3JwpAxbqay9plOor95oRjm9TCd%2F2Ynw4YJB6GwexsL%2BVmpywMWo%2FdULYhzhsFqvZLzHxEiCGmP0SP1Q5rQoke6mWxgauqplrFUU1UMCebbpqUNiCI8Ln%2FdOAX25cnZ4fE%2Fd6T551%2FlPwRtoJZArDnzllcLjDgXhERxC9Avd720%2F%2Fgbd9h49t%2FPHuvQJ18Mf5GmuxzgsMyRag9DRvawz8BrnY9CElW84BjJhqREmvQtRhV7wN5%2F%2F3CkTP2rr9oKgiM3baL9i9rCDDQlPzVBjqkAWGMbb2lwCtSbkya90j0RBm7NeNlWtk82UFjS2VufFhCHahz5lcKXQdvvJMnsQPMF6N5GZKTawWxk0fpe9QRxHXekSOv1my019C%2BI1flkD40cjCS%2BvSXaaN%2FGMhkz6MpTftpDjgzIkf0i%2Fgrsph6QP5d2DPdBGR8EsfNRPz7JBx8rmtDRQpenzvpDyqLMSJIMtgF4fcC9CJ8aK%2FnXjBGdtnNbReS&X-Amz-Signature=a422556cb7f30633cd0f51d78e672b87e8e1d99cf62b834dc03cb13935b20553&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
