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
fetched_at: '2026-10-10T03:18:42.511Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQISUOIU%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031838Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIApQ60rx1k4Pf9j5AurbhA2sxF%2Fzj6IQQjAfqPqPGNz0AiEAu22Bf2X%2B7%2FLM2XuSXOPaJ%2F6ASykhfJcBH0co%2FhguqAkq%2FwMISxAAGgw2Mzc0MjMxODM4MDUiDGTFgh7ZLB2MhnErdSrcAyULE1bbimriuogG3EShpWkyakYpGpIJPnsEhWQDveDW5yfb08fB%2BHuBLE5noQsPRgG0XHY7NOQJHhpcvVLx8EIdQUKbLejE4x5dB5tE52mT%2BdiUwudY7rVY9mQuR%2BUJN99i1rtZpTabCAIMaaZ%2BpXYAN0XM5bF9aP51%2FGxtIylBZQPInAqQx9SlpkoDCqp22k1fUfQnuA4xRxPkPE13l%2FQC%2Bq%2FYloOZjAqU3P%2BMmHuFGYCkdPT6iQlrS3EHn2fjl2DVII12md%2B52hnMFz1LT3OLhFyAfT57vwy4qoOEKdTk75KHx8Sa4UlNkwwB50JSag9CQvFAgf4ZUPe3WEakSukXT6AmkGtYBczWiK6wiLmalGZPDfNevD6OuxsK9AaxeNiNy18yOa64Jqc%2BN6Z77OsXXS%2FUiWdc5WtUC96uQ4waTicV2rzzPfe6KwbwsB4yBZ1WlM293MLZYQxOMny5iesnAVrt%2B6Gmes4kVmcuCnD%2BRJRnFGRq2YCeDrJqDBy9DLwW2riJspyZo1Kg0f8uw0GydV01nQY7Se5zOBAGl6TcOi%2FCgqeOxHjT1V3h28szJOcUpSuZMFJsDgMocyPSh3oRB3ZmHtFvh9iHXH9F%2BQ8PUJ2KGRUMiZcMBtwaMIbFptYGOqUBmfbHLRxGpUuOkVog30FlGo9KIlEvRJG3GORJr0x%2B7edZA1vtdkg1SJRrJDG6ZwgY4w3WzSdhdmSvpMsIwIFx3Zi3b4yFHr0KbPITtwjtSEePCFBfrdBc51%2FX3M%2Ff%2FdgBsyuyoB6I1tVtYqC6qakn1WGpuTaYAESHm9quiYAUndTUL0vyu6pUGyNXlJihv%2FCsIhERCGwdPDGML4PiGd89ZdA4pjw5&X-Amz-Signature=3dd49cd89e2b1cc35838726a48ed1ce3cac97d4a52dd1a7e4fa570c64e4609f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
