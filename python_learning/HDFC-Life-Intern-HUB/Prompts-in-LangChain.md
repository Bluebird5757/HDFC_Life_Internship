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
fetched_at: '2026-09-22T02:22:56.916Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7YGHAWZ%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022251Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHFw%2F%2FZ3RDv7L6GYwNCEt46GVM%2BoLBuHt99QPwMUmyiQAiEA263vYMeZBtQoXam9EozKQpbkbx2%2BtzTrJwC81%2BFSNuMqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIu1hM%2FvrhPTz2t%2BgSrcAyXE1afZuLY9XTOPNWXEuGjzp6BX%2BkBtoSfCjjnV9%2B846Iibak8GKFskg%2B5eMu8%2FBc%2F4Pvy7986ykMZXyCHurf%2FnZCrT4ltKOlDtXGpzVwQmQlD369xRP87cgvKPk95LzE7MHFZP5Kpm7cjJfaGt%2BEbOhEJnbspvHYmXOeo4iAtdMe65%2FukA5C0kZ9sguWrD%2BAxAsdMLjBbBrII5G0z4GPssI%2BawB8Wmz7BF9xSBO%2BBuaDq%2FfkgPOpOT%2FbCvMHHB7SzbrKZ1MJu46xNshDQFDqw9ExkDDpRwazGp0LImk1tEPKiJn%2Byvt75ryhHVyqlHUr0QGbks0MYBp4QzD4XLMSb7ReyqCVhw0mqvIrbwPX2Schuzv9HOirF3UyLg%2BABw%2FrHNr2Lf3b%2BxKpXzJJgw5C0pQ1cbNtb5tj6Wkp%2FreLxmPu3SugT37mgydg4x94a2rhW211%2BZVAygRvf19KUmxkMyVRHdGbhfoiTa6B1cIGZ%2BAfjt6%2FkcBzHTfpE1Wjm8qJhD46sPlQ8lnn4KfhMM4F5SuegT6AkDSBg63xuJ1eGuXO%2FJkHRZJZ2pM1WuoW5f34jsMwDdmV6DLQ7oF3gDjIo7j4tUc651sdyYOmEtWV2vNrEm9JMM1Duz5E63MJKZx9UGOqUBbxsPw2wfbZZxleE5lyFtfagoiXrmKvunpklhi9ZeaXzECw%2FmEt5rcl%2BJYMqJIV%2FmaDSfJy3nwFOwn0CmRkVFuN9u56zla0AbR120in1ihNvYlG%2BUvqJdKvGFHTMMebbAebCyEiF4LeoepnLZ35cUgjCA1fzDCmGtij0sIMJ1BAUvMEJSC13Q8QuZaecsglE3lguD2fm%2FJtLgGxVJ9yNRc2SCwzWC&X-Amz-Signature=73a24e07c70a36ac7d43925886184b104b3f182bd847d0509eab9441dc73a216&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
