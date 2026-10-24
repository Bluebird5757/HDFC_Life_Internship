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
fetched_at: '2026-10-24T02:28:44.696Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BJX4GSV%2F20261024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261024T022839Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCrvOdJttCo64XJygJSJS%2FaN%2Bo96uozR5JEaWEPt8gmJAIhAIqDOUJBH5W4DiWkaFriyWJlXNGMCrhHekpSF6C3dhN0KogECJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw53BdnzUODE9r7XnEq3AN9jmzpQEksjrEBIQzID63W9xjuMucO2gHLnFnBkZkMu29Ok2LP6sDw5hA0TzwsUZJAGbUEx3QT9Ru1w0PgGFwp%2FW%2FU%2BpoC8PJj%2FN4JJEYjynICdCeMXaW1OUyG6vAROaGNi1tfTmimCnuqWJGJTl7rFsCFz3IJd7GDgQbv9FssOovGx3ce3oA4FG7MLaAlbywJsqQrieahR6kVO4d26xvWPA8IbEQysaUSzuBobMquR6ynd77NFCztO4h3OIxMFshaDVzplEtE3RkMIAg6idEUaia84k6krRcVo%2F2wZvmdDswiZcT8yj%2BZaydX2uSejHsclaejbkUXSrWhOHdnt5vm3bXCEdwiTagMcScJFwym2wUF2jBAyZ3RTS4gVZZtMhwzZ3ZGF0%2FBhI%2Ba%2FZYXPuU3eEeN9gY40nqQ83ZdMBj516AbOGXG%2BZgFywWpUD8jGBu07qyL1am3DfBtfLwulbtQvA8mYXMfHGj%2BrFEBnZ2WmQIz6sGcw2z6UDG2k%2BWxSwNePgUD7BzuC7ATt65niFtdAK80RIYosQA%2Bmaq%2F9iCSioAN%2Bpbmoz6hAcIAs4EZLc%2B1nLzgAtMFEPIKBlpDZMzounQp9hmokl51oDUTC4or4mXb5yZJcUI2PgWoqjCBrfDWBjqkAaxNph3XuGu8%2BfcELWrLk5%2BoJ4KraHM9UjAQN%2BhD3HPQqAMADynIgVIbu%2BuI4ahmu2xCs7gH9DX%2BGSqQZzrMe4UQ4ECBFxkXRTV0olHc7EdSmJjb9i8IbwPGjbriIZlrxj%2FbcJCuegfvj5PhJOzPUplR6wY11Zk%2B1FfVYUOadgxkkBkXFT3vEmFBqJh96X8PxB09kvqFekCpSFnq%2BN57Ys7GMWDJ&X-Amz-Signature=7ac3ea07e87237c24597eddd43dac4e72349d473068c55b9a7d0b48e02a41ba5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
