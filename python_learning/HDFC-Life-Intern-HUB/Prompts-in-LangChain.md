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
fetched_at: '2026-11-25T06:02:22.726Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WLLJFZX6%2F20261125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261125T060208Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDhnDjJd9L6ynLwmhOcxjC5LG94BCjjAI1QEHPLml90pAiEA4ZJpjapmDCXWTkssnpF%2B9aMLsq4rRBMla%2F1soLeGi4wqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAqiVNw%2FsVcXYa1Y8yrcA6gpMC25ZicrHwMb66o27ukz8P%2BwVggiWG9dkkEmjt3IWd1YC3Qps%2BTJRVpYFvs%2FPA67znuaU%2BwpvSJHwo92HbZbpKJ6sesXJBp3NirlEL2vso8%2BU%2BDt8oJ1L5oK8qb0bFFqG2gDA76X4HUDGAnWBo33ftV6FiLJ2d3lnsb6890nRs8PKVSaNAhtxE7iGrKvHINWSmw4L%2B%2BfNWLyv8xU%2FzJ%2BAR5g6q2EjNNqtKw2otffrTZh%2BPsk5U8rDyo1R%2FJB3U2wj4InMAEhAfzYhsN2QJ%2B%2FbatsrPlS%2FdyPwnVgFhP53ZYCZsdLHEhg3YgJisL6pE7k9LXD99%2FV75XZtetsZ7UAf%2BxxX%2BMpPTCryNOxoYjVBgqOykLVVZYZLqUNDwSG5UAZ4kyjV8bxuqDEj%2F8uOPMSylTgsgND7lsYnFpiE3dZcyw0TO16U3eVEzaKKGFdQgKJgdbiJfP9mdQyH5P5Y7gmYi6t8tXUU1x1%2FPmHaZrO4%2BgfvvTSH6HCcBn3lStpIrB0yMsB6RQ4e2imWlT%2FvFU%2BXipLBAqg6y8ZrE5FEJMa7gc356DvWlqlxb5Cp%2B2GkqPJNeAVWVl%2F7gZ9ZdxeHmD9l3lhwl9ehDnSM9ubfrE3%2BF7Fgr1jgEqWPE%2BxMOilmdgGOqUB7ZIL4b2Vi55xMgzzSMkJ9yEJjjeQZ8bo2jcsjRpOC%2B10cRoaJz6zeenzatonKpBbEhMT84X06aa7ioeU1AnXPCIO%2Fr9uwBvnx%2FoMS6UwmdWutdCBqhdlW%2Fx1QEI1m3YAznIDqvvA4%2BYd5WJHDZg1MzjLq5TfQ6fvMc6oPToKT0BF1YAf4PgmA6JbIzOI7bZnPohbzs64W6Ujzhl2UFPxzNSvHrKA&X-Amz-Signature=48322199795f4aadd77da12e578c6d4ca0f1bc1f75c2c41cc76257eb17a139c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
