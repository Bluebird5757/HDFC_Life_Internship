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
fetched_at: '2026-11-03T04:32:57.530Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYXTJZJ5%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043252Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDLnCQIMbYhSYJVGDl3MxXygYly5l6rGyYmKCiIYpKZjQIhAM1iyf4dNmwsSNOVnKDjUvuUgX2S0DRnhZu%2BKDFBh0djKogECIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw0rBR8w8pkdC0wZXoq3APRD%2BGA5YjEzwuEUErt2mELjPXBE7%2BITCFV%2BJhtMyeb0v4sVNAOUepwkawcGfjGtLqY8dj%2BwV8GPhuJktAYYDZKxUV8m92u8n7Ok8MkPeSn5Cxruq3%2BNnAc81se46sBw2Tt8XWYZr%2BGC8EvXNn511T1llNEv8SixriSeCmyMXJ1FUsuM5zQQJYvbqVo6JK3HcvimlCPS71EvBtX7X6sMJ5fY2IvbROjUdTsWgGbZG4dviPJp8rDk6VV2rKrBXOcQntlh03tysB9YmOZ09pLAzaI%2BMx0%2B9qEP%2BNCajZUG5i6qyq0y%2BuwrgrzUzmeCtGygR8kSBnScd2gzDnn4%2B3Q3vaSLDIMhpfFAzp0%2F%2FLJGvd6XVet9gB3%2FVgwoqEA4YgTikHsX%2B%2FHM78G%2FDPnLqO48ZeSC26o7C4WIDfmkQKUyammjG1R3FOoH8XhiJVMaUZazte5xoc%2FDfcGVrR1ZHVsdqVAEMtzR%2FcHFRsH6MuVAfBOolOqpr6rjp%2FLNVb%2BRp5lI2ofEeBWmpp3Isr1iAS%2F7Jht%2FEY0E08P0uJfOQZpuhH8ifAz1TjBBd3PVIr5P6kdhcV1NuwQNC7JzM4T23SWDdFlK2BmUpXmuyH%2FXJU4LR%2FNB1NDXbsVKy1B6ZkosDCPl6XXBjqkARTUqJtG5CkRGSLnhf4OJxFbi%2BvuUB7G5fv9I%2FWKvHyVFDIKfsuE3yMS9q%2FFWjLOgwB38zd1jaftZQ61J8w6gsgYJe75bj5JvCKSpXKIbIBSsmMJGCpKIk%2FJjeEgXXgg0B2ZIgg2M6Ks%2FG85z5mcnSBEU6ockyiCb5ZyW9K3j95agntDTw4iZmblxSLPIamcuRW5UmwurlL4NlAO8SnWVMaOeVQU&X-Amz-Signature=df654800e96654a6d2954fc885d18aa0e28575ef9ea00ed4813df0c0e05e3404&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
