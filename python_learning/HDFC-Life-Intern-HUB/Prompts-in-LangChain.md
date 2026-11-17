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
fetched_at: '2026-11-17T05:41:17.656Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SVOG6554%2F20261117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261117T054112Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIQDh2OLnwr8GrlJU%2FkqChSEfhg99B45Jh6sCo0rVaewjzwIgWh7IWN4U5ghwk6B9yqqDHmYn9jFDdwK7Vwk2jFccaAcqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDqXQYshKuhQOOPgMSrcA7T995GojGlln2ylanZ5DEm9dvo8WtA6F%2B9cPauRXH5fF9SgPiLQgBfybYj5bTfpnWCK8TzjSXnapBJ%2Bh8yvXEwtFRcdBWOHBYuVFql3hIP1hT3ybVllR88i0oCVIj1mmt8s%2BrcjFIT%2BlHcc7glTLX6%2BckK%2BLF6rsPffNUcsuVpPl%2BsUcXy687lnTmf5prJ6DBPy0tM4Fb8Bh4%2FAsWdTVWjQwCjOMGbgsuExbSSTrw01EaoP%2F%2FziNTk0taXEcnSTSA0We1qEGIo3KuMAfp1AorS135EBMzW5H4NlbTrtJV1llRoFaclk71aR0Q8xJiPAs%2BvuM8KDccFKnRF9PSaPkfebabYlEhr0x%2FGL%2BkJoeQWq6pRGOgRUwzFbwD3kzVhl166BlbiX%2BBX8SwZHidT02dblu2dfABJht7LXl%2B7FISN0AcujRfhigYYujSC5%2BzcxdDUnDS%2FHXxHuh%2BZCuAa2ZclTTegTwDmsfoPcVP68oWzyMPQLs04qRCoRYSOMxs6gddUbBJgpVfzyMplmAcm9tcfDeMFTINhFNajeN%2B2kOzIc7rQRObwEGdCczcsDvBEYBdt%2BG2TvMI19gTZAFnwcLgqDfVj%2BfcbPMA5T%2FDS%2B4vaMaO1BkD9bHX6qM7fEMLm479cGOqUBOYQMDEBDsc4Ub25lL9Chk%2FQ8Rl2WC63FAgC0a6fcArl%2F9imbQ7C%2Beoky9UsNJmijRgOSCU%2B359pEMn0AaVDH2M5NIKcdHl6Uu85y5r6xqSLGs67SkTcRUUiQ3TZglbO0oZ4bqP7%2BY21YUn7wmR0YxWry4ZNLOmmwrwYmIfWch7TKsHsJfOXzdJbkVF%2BP8qcc%2BaprKn28lXpjJlz98%2BRINiBFUHSm&X-Amz-Signature=c0362ce0ab249b0bb663cd965d8a819ace828a0dd803fab8df7a5c84f107bbc0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
