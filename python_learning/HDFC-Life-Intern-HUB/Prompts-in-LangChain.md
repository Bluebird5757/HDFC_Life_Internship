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
fetched_at: '2026-09-11T02:01:23.449Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CCGNJNI%2F20260911%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260911T020116Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEOVZYKKqMeiaa0GtIM%2F%2FkQNWtD5EcKK8aQyRBoyLcOsAiEAmz%2BR2wHKhdjj7%2BqECX72fEIg6RBR5kItKECkfYSR5KoqiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH53BawASJnw6d%2BiuSrcAxuIRJLqN7YqUBgjX5kypv76M27PDpd1%2FyKdL4jA0h3EhJxjCeVp6CZyRXXT77Cu49ymQwgnxyNwQzPNrgXWQHzhnIGxNzuN3Y%2B0UiN0%2BNRbi9g9UpGIjiB%2FSdLwgbZ3m2znEbiRS1Nv%2FKb%2FPPCddhBOej9E1c9i%2BQo%2BHkfls1kaIuBRwzRwG1eNm%2Ffqb1SJqWHkg%2FlfevBZ%2BdrV1417ts72LxloYyk6iu8RSnAExa7AuZCu%2Bzq5allVVQc1FH%2FLtm3bOSTL9AwdeLMlKUKpEYbhcPl6ZolXbAc1gABrgaybxupAXOEpB9Iu1OpQfhG8cj7%2BFbBCgCuLpWa4SDVXd4nIpC5jBtHvA4SrJjtXegrWt8aZtdVBYHwi3IhZ0KOeOSVvO5h%2BWLbKlFnUIbkqDc9dTdQdfbL9wyUVGImAHzBBOcCjYyq%2FBd5GUx6C0nTZ%2BVjQXiZj7q%2BclDsZpOB0UV50ggZar38XvYrtwfEHd9TOnc6SM1YlckHCYWlV33wMlQa2rMGgulV%2FyhohgzL3yVJsYIrsuy%2BgTVgxpUFXBtr3IB7AExQNcqIbOvbHzAgmmkOlq2zzGtpUOfb5hHkFT72YcRwQFRhd9vd6c5Ie3luWp%2F%2BoferFakHLYk2RMOmujdUGOqUBcgilJO4fmGkyuLUHaJWd%2FPCqR%2Fu%2FCFL%2FccnJb4RmkwpBEbxw73FFQIn8Dj81wTg7OBSYEt6b3wtdMHWNwV8VmB5BD%2F5jRwOqNOx1TSPuSs0SDMeCRCSkmL4oe9xzuqfD2VXu9xMDupFtztWDCAhdgkS9NSyxVUaoULo23s9oqylSNvoJeG%2BbeB48AcsfLMih0B24n4%2Fk6dntN2V3Qxy%2B82lITZ1x&X-Amz-Signature=d019101a5b70fda85290831205bcd35288413a568d5a935b084fd738165c0d67&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
