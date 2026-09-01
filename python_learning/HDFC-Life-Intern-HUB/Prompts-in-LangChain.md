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
fetched_at: '2026-09-01T02:35:26.954Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZMEVHFW2%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023519Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCM0QQEwcewI%2Bn%2FSiy8BAw5WCugtzvoKEUqFWqIzPh%2BYAIgcyHSnu9V47v%2FHZ0gC%2FFLc%2BFE6gLT5IzTgeqxrosgwaUqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGe%2BxJ9K2iwyOiEL1CrcA%2BgOKcNQrD3AKkfq9q9QTJdKQTx43lH5ZTbuKj7WIaRQ3%2FsBYS5JqVE7%2Bibef81PVeprwvS78aJ6lH7SwGy4ICCQ8hne4VFW7Yg%2FzzTaAwcJ%2BVO5u%2BJcWWL3A9i4EE%2FVd8kUGQhVk9bg1GkUl3Qz%2BuL%2F53%2BvbZWBkxHvVoHREu98919ZTgG91wtGAbd05nt2w7poy9lZwU2VYr9T7e5g3nTsvW40HiEF7c0IJR3UmYlr4e60nyu7tfI5PnJv9VUwdHH83l6hSxgiuLUxy2AuqB61dOjjaqoXzKhQ5h%2BdLqcehu3K%2FycEBYyEUVW1TzS4TkzOuFpMeRN8OKRE995dmwpv0EZtpo%2BoUX%2F%2FOpeHaTkRD%2BLR308I1iKaYIn1DTqvVMWoql%2F8Q2ojB30IfBR9dOOOJwuV35GAJxWaCQhhg9nfg6Pnaj8w6SZcJDqydDEZq4eHRtYVP5L%2B2vem0yUilyf1kBOUOUoyB%2BhryOMnKT4OX1cZWwukQqi8sVQonn%2FsWrZx56HirJ7OcFvMwXDsbOrc6pq4%2BTx1tqGT3%2FG97rbvE7NVtg0XJYvBDNr8Wlvw9qsZsKxEVclZdcEFkkuHYcCZ%2BHZ6Z89ZO8423XhD%2Bwp0vmCuc1QY0aitfk6HMOrc2NQGOqUB6LKys6Q0NNL%2F%2BJjZTl4%2B5cw4XDDW9REA9xn2UeSqTubKRK%2BTICJ59lSP3adCbdnJIJAQ4g9Ayq6da7BEZwkwpMtr1BCnvO33f7LFgxz9rZEUmvqQQddF6qCiys4BKEfz%2BaPNt%2FWwaHgGSvPrQGgJgj%2BGuQYzucRN3l9UKV5kgm1QT%2FO5YrV89i%2B6WuxRF%2Bslm4WxtORZVzMZVGVy%2B7Zg%2BcLhmPwx&X-Amz-Signature=c3775372aa50c51d223736cab3867b3d4f1c5ee0b07992fd5b221c492eaa67c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
