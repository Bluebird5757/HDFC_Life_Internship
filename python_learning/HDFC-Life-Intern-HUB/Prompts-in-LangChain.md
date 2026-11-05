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
fetched_at: '2026-11-05T04:12:24.529Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667YKNO5XT%2F20261105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261105T041220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCTI8Jtt4qqoBkMJhjdVPGDGUUMG%2F2%2BxFire6bGFJfQXAIgbzZfvJqZsV5MgsJ37kFKLrlYj2crz%2F6fww5dzYf0GI8qiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMKglTs6MCR7t%2FS%2FByrcAxrMKQACwfIvi8jm0XJtPxiCfkXX%2BjqmRQYPjkmq5llfXIvkKSD9OBzclnJ5X%2B1DJFGKlmTDvK1L%2B696kLlIPTqKymsPvBj%2FxG8aKS5IP0fG7FtVVMZa5VkpGgCHZYLFlXD3D2hgerNjHhO721Yyj9YUI9exaLBZlDycH%2BxQGPonTw7ilLPRLBKX3atWheYAYaWwRpvsbjENkYJCbK2shaA33r5dEMLjWWERA3Napar6%2FblYFqSuyDqBzDmdtgcREDnwDbDooXc4Pvh%2FmHC2SwF90Or83eOXQHxSvfrqPqDirhH59SwHVKOwsKo3TMNzfh5FuGFJBQQx00ZwaXrM3cQWr7EVlsNu7yl3qZmjMt9WRBcLq%2FVmtXhoY3SRPX4W6DXJR1kcpGPAwxiCKgUprNzaaYL1B8OTOkf7H95nVg8fm9y9025ZpfsCb4d23X7X%2F0QthpDKn9EVSOO8N7QidFR1smQxgz4%2BQHjMF%2B0emCyEHlVkipWgPEv6mXVBJVFRjBtmC7akxuK83VAWz6poK%2BYF2UI8oGwMNXY4UbdarqoCpuceQG5fV%2Bdr1qbqNLisC5Cp5wzCjGAYhAjDZz%2Fs%2FVPusJ5e9Jq4%2B%2FwheI6Lo8rp%2Bi8JCX3ZgieJNsL3MOrtr9cGOqUB%2BNAXA5Be0P9Q5G8EFtXy4tlcySPZSocNMMSrZ%2Bu%2BmBTRlxxGak%2FQA4Ack1ctiS5hrA5sufLxd0LNmDZTSnApK%2BlEUSjD8tvxLBIOc3As677hG%2BMYcOKsMo%2F7vNdHQcqmYw%2B4IBcA7lHuunseGCocv3cEu7%2FGqaU1jpFo4uhhUzye2F5OB4rbrPJ8KjKJ9AgkFaqpiO1GtXbJTA547G9tojPR6I6O&X-Amz-Signature=831be8c81b92e52b1e2850d69757e60eb195c2a33aee85290195fb227c0c3787&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
