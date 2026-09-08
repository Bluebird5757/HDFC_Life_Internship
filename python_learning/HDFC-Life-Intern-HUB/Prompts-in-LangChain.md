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
fetched_at: '2026-09-08T02:01:37.151Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBP53FYB%2F20260908%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260908T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICstPjkX8kF3m5Pv3t0f5jtMsq2ybuPAbWGr3vhnp4l7AiEAswXDcUDYsQsDVpJbMFk7RXG03WACUTVtaWaxUrJ%2Fy2Qq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDEclIhsB9I2VievPkSrcAw7Kbd8leZ9Kjbmv0c9aSo5h5U8fgQ7wk5hUKZ9kAT7k%2Ba44mjSCpBGaWkK%2FGfe70w16z5dVTPUal9L6S%2BREISb%2BGM65zHkITlO1%2Fnbo6vtDdYZDJfV%2Fe6%2FCDrW175Kj1IEryOx5w8iwY59m1gmvZmHIxjUOyB7P1EfEjxG64eZZm1nr0k7sXVZjhv%2BJ5ePDc5hSN6gZsXtuXff29SXHlo%2F7x3xYmK8uQybwtf%2B17y9LPbZPDz0mD857Zj3ohj1657ykJ5aXcgT1nEAxjpShnVHKNW%2Fby9kLbjc3TdVWdVto6rHPuFhT%2FpNO0CZ0w8KIkRs4kLZ4hboa3LUzoeFJUnKpTbzDIT%2BuKZU9LZa7m09DjmSbCD7Y7u3lpt%2FzHIhW5cUGnUX3VE3EZLqjXGRNrmYhJoy9byjLUmM5%2FtJYijNor%2FuA9CXw202WpxKBgvbO5OfM3b9Z1LgCB45zgm3KZSMuUZrdYF6DgbhTkCGXKJs05bL%2FH4i%2BLpPF5Nu6ydvevyvgTMcGrqv7x0LzyKf5ed9iQ%2BmCCuoLF2ztuvJD3wrGy5QcqbAUD5dtYN7tQuAUYumOpnVbPbtdLTS32bXKS%2BOs84Ukz1XI1%2FTkO2FPexdFwbeeRBu3k91rTxMCMPXG%2FdQGOqUBXPWJ3SvPDh5TAS8bP3zNNGTkvwVeB4ZiiQCZgDzfBFmKrV7Oi6s7q5GQQYb5jA%2BzTUu2RhquTM%2F41etqzgzUsLx8egIb5WGvLboN3syGLXEqvwT5x3K1OSc6XsEgyto9u8C0V4MyEB%2B2NEi4oBX0AQ%2BcsKZUZhfHuh18Ygr7w67%2BEN2Ufr8U1BPPGbyWU7GnyisEc%2Fu7QlGl9%2FvWSWqf012%2F3Bx5&X-Amz-Signature=1f819bda997f59d6d54a4eca828e3c27f447957f4a519e41a382d2217f7da7f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
