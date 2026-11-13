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
fetched_at: '2026-11-13T04:03:40.256Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BYUDNW6%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040335Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFzRPHBMhpyqXhBF5ALrXOVocF0%2BBkMfRE9ObH0StKSbAiB%2Bk5mcFrXUZve3YJ94JxUnjdUqVOCxN0ZbLw41sf9snCr%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMzmMlz%2B8bxuo99vqZKtwD4WxcPEXbUTqfM062qJ8XOX0LKoYP%2FwiRfDJlRUAXS82htkDjFdZhhfdGCXhlMPnKsPbbg57Cz8qVa3TVQGhQdTVrqKvnOiX7hNpGSQUtGLmrqmLTlAIWKrWMSA0NkFM%2FuI%2BKrgwW%2BLqSEOhab0gGHhXOhlvla%2BB3PN5STpt4gc0glDDGdtlrIxaImDMuckkZZ2EnDG%2FL3R%2B725TUIZ8HKrhNfyxzVa5Er1Hf4x2IaTKnZfAqupBfDp6xd4s6HRMromx1EUUvRJmhkOZ18dzcku8HQZZdk9%2FxY1BpfVLBjVs0fJezkXMwMnILzJY4EqXsncTw7K%2B7bTp8jIFYS%2B2TgORl9MPxbW9E8hqQ7OA86I%2FnI0xo%2BR1ofArCLFX6ZCgAcSn%2BUDQDPkY7CQT2GN9oIKgnIVUCsjaX3z2CmmNYbrHJoVx0yOQxCeK1TBTHAkAKtiI2jOZBH2G15NmJU65u4qvhP7FYrUw5ZhUSZU3BWiBEimBi0OTMlzvu0kEqkQJ%2BQ6bX1l4fLHcfXap8JMmb4kERMp%2FfzOLhujqCFp2btK2EREZpmLLTC0ySarSiRvV%2Bxt%2BDEAeMfQByDgEIotF0Vj9NYZTGS2BiLrgaVCwCeBkXr3pkw%2Fhv6Xc25tow8NnZ1wY6pgE4HbfcJC6QJcGAxrXnlihCSW1%2Fq2PPmpvQ%2FCw96LrBDEFYn9jO%2F%2B7G%2BUKtOrxmmmGl%2FAaMZcr5Z6n1Xa6YAxRhrdhnd9E4uvEIXq3XFm%2BKYjt6oU82iexj6FrWocC7SnPAtV4wEXFBsPJXwv%2Bl9CCmCGU%2F%2BEKj19kRlIju3e9liVQX9WcMlYuL9uiXOFl7gf723LCoblhIq1RQoUUZcTOvyOjZRawo&X-Amz-Signature=dcf8ed0c8ca2c449021276e0fb86832a3f9bc7f14a5c6f36f59e46579bde7851&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
