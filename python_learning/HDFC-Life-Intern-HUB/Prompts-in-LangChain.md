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
fetched_at: '2026-09-09T02:06:32.105Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFNFVQUH%2F20260909%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260909T020626Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFd2i4FzXYjYvAzt6DrqdDdOTKvkwMUTKBMptsEGKIoiAiBemgATdZQ05JGMlXXtZOSNpwAv7y%2B2chBHI62uudaqxSr%2FAwhiEAAaDDYzNzQyMzE4MzgwNSIMnVUWMjqDyGobsJtFKtwDxK5U90%2BVnFNaEKnWQCtOt3nLa5JH9cs6Qw8x8rO5x7NOmg%2By7P9z9LzLHuCir9MjmxXigYa07oZp2G4uMyXntresDek3Uf7dwxh6iOT089XYqOTXnNjhOHVYmQn1IpIk%2BN0OZb7CDaQzrkv3DY5T%2B1AU4z09%2Fl5%2BjNhCk1UOLZNX83x97v7%2BJyuIkYdncZ7rzgItEfTPLxc4Qx%2BlD3LnoFYjWmTttKLVRlcItjRFuyHokbUSkfRdcK1LrRsl%2FbNSD6PGgIOt4aRRsM%2B31fz6ub5UPy9PmP%2F%2Bfe07YcucTZOUMM1hjKqqZSy3ucjQr88799GpRDjw8uuSgliFAXNkVJy0Erj%2BwdIiffPdqkrayciUR3P8jJKTANAQSugA0%2F4F9NH2Dhf6hV0k26QUAQr%2BaODavGANmYsfsqHj2aUVDVtp0tnPl0oEVEmicBxAcKH%2BZUAVNfjFnIm7v8lGqMUlywpf91R%2FNGEEkGAhTs2fP9y8sR%2FtFZ2%2BdfNM52LGFVxvNQPGPWyAa6Jh3SKk0vHwmgtak1Yim32PYKFGifKWhUWLEgVyTRazG6Ni%2B5pg6eKhR%2FtSRZ6L%2BhlMqdTS2r1nK69eVkYQtSTY46HtD0P65IhB8R77Rv3cLWFUNe4witqC1QY6pgFelJD23Cs4AK%2FzrQi9xYKp%2Fvja%2FmIDUHXyQDZoqqq78ZKklwmAAVqShDq7JoOl8JtU62ZlMhSld%2FSOD14OhofHXoeahut4%2BLbq3uRbOCQWjPDw5qLhd%2BebryPJ2vTuJknjTOlyFQYUI%2FrHFQGyj%2FdWel6PzZO77n9YwWsMNcxDQJb0cmuaakCHkSJUpTUtXuvwlFzTlpJ8goMAtFHsB4lTP4GE%2Bfup&X-Amz-Signature=4b1c3cefa26dc1425ed1ae4b993489fbbbaab813e634b0b7a84c57519e25bc6f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
