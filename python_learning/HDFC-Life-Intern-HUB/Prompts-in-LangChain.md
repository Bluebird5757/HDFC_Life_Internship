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
fetched_at: '2026-09-17T02:23:14.066Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMQTPR5B%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022306Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJHMEUCIQD9R1fDw%2Fs5I0G2Nv4ueAYWaTxFMMcZlQATtJZD2FyV2QIgIbi6q8g%2BZUmC2cKxhbYnaRcvrfczHwd0RpUNwbQ88nQq%2FwMIIRAAGgw2Mzc0MjMxODM4MDUiDLtzUIxLn37rzao2nSrcA8EkVrVt9izsSBsbU4arAQoc1AGr4YC2ALGrcdp8O4U7pw6AQufclvXg4R2LkB4qHUp9VXkr5EZSmMmmP4J7Uqxummq%2FLKc%2FvA0%2FCbqdKJIxDDR280xc3i%2FxPad8xz9xkpvGO%2Bzi6vPJfqM3DlX7tt9Q20hl4QsSmJY%2FW8d3xfnEGz0833nIge%2B1YMRi6X1jofbWq3uGBbZOvPgfcLm4G1L1tGdOvY6EV8L3nMX5zoVp7XtOzVpys3Q21MWo0W7B18NH%2Bxx5l4lj7EzRfPJNddKaJPzDSZ96Qut7epi9eDc90NxnTw8Nix58%2FQtfHpmfu1sa%2F8nb1EY3VAPnKp4vBFMlQEizEg3GKnlkOO9yJ4FYzVqIt1UToChQHg7PUfMsaM7PhcTRnJOO7A8DoFFM0WUBwnnwOtHOGgLYw1NtOk7lYRnJPp%2BuHMtMHP%2B93vbU%2FkfLHov7cRCE7WB2bFeDgseqOU9TNZAnmxoDE5bnKjLAHAeA321oTuznwOqEjJrMvEvzhBKZPKo%2FUalTDx5o9ZFP4Axu%2F%2B%2BPPLVbnHtSCOsz82D1XXlewFueEmazAtnFoNribAqy6dx0BCE1v1P32jSiwCpFfA1W%2FpywRMaxfO2tYkDoruHK1nLUm81RMLPirNUGOqUBBun%2BQi2VKzhn7MRACzaOtYAEFHP4ZRm1W%2F8XP83p4HuzZA%2BP4EIVM5LdJjeLBIwQtH0vw2gd%2B3L%2FTOecdV0oENKM599gyffsZ4o%2FQXzGKW5lsZ4qINH02BWgvaZt3q6kJZsLWRsoG5RN7%2B2GyS3%2FnXjkLGPXJBJ9JdALxNAklc6V%2B9jrSy0Gmx%2BguMDeM2jl3RK6JHxQrF2%2FY%2FaqgAfClZ7wPykr&X-Amz-Signature=2c88eaaad03c7228144045ce37ac43efcb9d5f7a615312163c5746ac01e8d3c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
