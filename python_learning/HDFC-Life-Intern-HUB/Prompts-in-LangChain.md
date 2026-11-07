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
fetched_at: '2026-11-07T03:42:25.581Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VQGENQPS%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJIMEYCIQCe2ISt%2F%2BQpOwHLEno24tnHcgTmSQ8WqeLt9IOeKb4z9gIhAOEnDwM%2BmlOW1H2HKii5FgxxW6U2LEE4NIkG9gyIJxiSKogECOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxPEBXbjKuOiZG0kegq3AMzBGjT0CILjyx1AWCJhpv4VvokJcJjaTEDMXLs3kDiWziDin7O7%2Bubyb6ZQryPwUpzJRKmqcxZqzRjrNkme3ndBOd1QSiSKsvsyRwpZtalxNg4aUhR2HSRoPqwLu%2BCOJLm%2FtMihQ%2FwFK8Uda88JfHlEXSHcF2A0KaPHI0eph56B1zh8IUx4puu9IGppo7itW6IZ2sDD2C5OOR9VEwmHYrCcE6LNOhaOSpkoNqpzqgpqgdw2TspzaM6%2FqxUyNDckgYdIQXH3QgzPO27ZPwlEpRUS19pIfySTGVsX8wvItPiE%2B74q%2FW9fzwBFokBrKsiXJ%2FHG7PdMGXSf4EjA%2FLd8xlAxwLYGs4Mvfe%2FiA1fpPsGGq%2Fyyd9kIOzJ0vJyAgKzjh%2F52ynKmsFOojWIoYT15hbP35Q3yc4DFwpITxte7ZEjmPyWfh0TgnPF6j7xfbodo8hUHP3tTKkhRzo%2B8Lnzs3AJgfIi9kJyQ7AKImjES4fqOSgiBJ3hDchbWkYPaE3qq3ssbcOG1C7UjbsrBhG3B1YNcR13TAP9PaQ%2BfbvYXFYUReQeK1CpqZk7KRgFuElvp%2FLRewwMhsmy4sNLNWE439vuCHcrYTvCUabIzZN62kjqAXJzYIY5eyNJZLceHTCNmLrXBjqkAe6eQk4U572tvlai6Fvp0%2Bx%2F3nA7Kj3qCfHg%2Bp42QHUCTjtOysQCb6u1MyfWiQmpZ7u8YMFfrn7dKls3oiytKP%2B8y5V1BV2lTSccvvhYY9dhOBqIyKzaKzexd4Wh8MsrwsJmOv5cA9jPRCLmIcfedTPGsjgDHVbWu4VcfdzRzGQvE55zbtKLpIyl%2FR5%2F8vBQoX6HKJPjCH0KF8SRV%2BZl0fyGVTZ9&X-Amz-Signature=86b7ef9857e14ca1cbdfc4669dc7b04d992eb108cc87012d810eb86f4c574a10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
