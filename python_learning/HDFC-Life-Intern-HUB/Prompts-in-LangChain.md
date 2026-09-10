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
fetched_at: '2026-09-10T02:03:34.452Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ZDNFU2X%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020329Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3jc%2BfizjthvzdQto0TZFJWmIj8ZXv9s9DwVc15J8gzQIgejGVcTWfQIhcozoyfn4qjxKesGdMLGuMk9s%2Fa1fzjHQq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDLi6t0PaeSjnh9luHircA6bB6zcujq1v2IN2dSK8RJ4Fk1TvLLIY2%2BOpMKdBdcvBuHS4d3RLZkHwVD3DVYcXCKz4ZJVnVrmP%2BXKsCiawFvgcZ8ND617jhgd8C%2FIZVNi%2BJQX88ahrC9r%2FLy8LrshlFMCl%2FYnCbSTpDN6hWcme1Jug2d9qyR%2FjL4gryuPGDbqSKWw0juVw10kmhgvVnfzpt1yy5VtuPNZ%2BD7mU0uMUni65SVpeuRIvBz0POnM9WeBMtz7ioql56cSJDsfyZS3MSeFDHOpQMKoCO4siaYN4BG7GBpQKZ2rd7AOQ1aMyXyi3%2F4tDQLbG0786ItqWLy6iZC%2FT7URTQv%2BgmwmINmJPmc0rnkh12ENmpxCvARLI7%2BZNupEqb6hNVQIUKK6ONiSkUCCR5nXsTAHUekc0hpaySWuY39qbMjI2YsVQ6u1%2Bmn24HqlHzLumIQTsv%2BAiMx4EHOB%2F7uHNwpXUnttny8VKTfpEB06P6IVhawdVoLO%2FXiGmuVAoLa5HFwJ%2FDdJaz3DmvEstKwWEYUNTbzaTZhvq1DpyDWpAIcMixxiKq8XHfaKvAPlDgPEco943BwXAk2WnqiJx84ZBMfeg%2BNImAeE5e9YD%2BjIUFHtngz%2BsBwDcxeBOZ9Ft2L%2BYMCpkq3q0ML2QiNUGOqUBnpaxupsTeM1csPBUvZ6%2FJKlRRbzxHumDZ%2BZZ5x6loVwwaiQhFLgHJ%2Bld0o6WV3aCVp%2FV4H%2BSSdVzCpHyKrJP4c88VS%2BEjqo3F0WVBAWVkNjPvVdMEDPA2rjOTOMKdgGDAZlE4r%2FCPVOyfEv7Cl3%2FVXYVqA3eOYHneYlS7ms3qv9%2B1c%2BVeNW87flEHoKK8RhS93jn%2F%2BwQ4D%2Fmj5CWyfaP86WUJyU6&X-Amz-Signature=9cec3bd58d4fef4538bb02da37757a6278d8e61d621320e4c8c6738a97342eeb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
