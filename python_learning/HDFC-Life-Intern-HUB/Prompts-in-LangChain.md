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
fetched_at: '2026-09-03T02:01:58.343Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YCMTV4KW%2F20260903%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260903T020153Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIDGF2ElFFUAu5DG1RT7b%2BbnjN5x94piIPdhSnArpNYN3AiEAubkWZ2gLoqSqR7i00eayNqJWlKi9w6y3bgWIgJVE%2BmoqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHg7fXWts94E80kkpyrcA%2F7AFYMVjtu1E2N6uexgZUh9ODvKZu8IkuqvGPJHWB4cQ8SJwm6knTDGOVPz6RahBln6muKVvZHV7AFUAxqi2YK%2FVAfwuTi2wlwGLWdXxUdGSufEP5fEqAwuKNkbXwD%2FNaWqasZ8Bn07gBZb9LhE4XU9FsXW155I7FZ%2Fy8BD4R7Ox9jA2Wbolk9qdGOgapSBTws6rAIYVXuLIWZO0P62XymxJNL7DqQmOrpX5Tm2UjFGu4WRZ9fAQiP9v8AX2cTIZjg6dFBWTUhjKSTker5e9GInlLlmqrlZAZBqbzlwkXFhJFD9nnCZPkwaJuYSe1dHTFBe6VHuvRSZgwUJWla5djCJVVdPXQIQm07lS6kTmxTq%2B7mKziFm892fF5CobQKhiGlklOwFDGx9AYpJEtFMKLvMocmbJefVD9%2Bt9G6KrlRaIlKkW68Q8gB4ANir3QOhyGAWPC8WM6zh%2FJ9JEVaEf6Lm5FALIoa6bI3xv121Hlwbf6dSO7GPiz6o5iOwDoEqcteUAGydtJ959MAYuwdrOCSBzGRkpewaflivTPFwYpnfi8hii1CJFp0I7GEJLyMvh46k5ab81B6j1Axp8k631wu1h8RZ3pg4kqA0sOucyFCLZTNM3AhGGviSdSpiML2e49QGOqUBmvQLZMTSQxX6NrG8AS3BgkSKxQfJmxmKhSNHVJgLoosFO%2BjAqv7CeekSL7wrILwqEh4iTbwhlOe0pQxVyLv9QMgLgZw6PEkJ0ezihnBIHi32GCYJ5DHtQsfiPKBjgebyU7gW%2F3m3fNe3%2BG7lcF60GXlVn5JxhBMYARftl42QLSiM%2FupW%2BGUt7V0hcgfabzq5f5ZOAKG4bZfTD8MIaA23AdoVgzc%2F&X-Amz-Signature=2cb6764611c553942e60346960300bf15a3169fd8284416797fc35d0e315acdb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
