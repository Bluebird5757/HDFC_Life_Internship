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
fetched_at: '2026-08-30T02:22:25.539Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667W6CKO4Y%2F20260830%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260830T022220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEM6AZuwmVxraFU7npfuPmX2PUVmC3hoA3%2BcWPkZ4wCYAiEA9ruc5tH398QKz9ZhLnxokccCuz7fOBAMqWpuMivmfKQq%2FwMIchAAGgw2Mzc0MjMxODM4MDUiDGqVQtc5%2FvO1KJT1YSrcA4IobffA1%2FkRaZtJdne1L4uQSOeoKDIFFvDGMsWEGFLoOOmUHhtK9Y4dXKYlIp00SbJb7XL2AUJ1vXeQDPD4Kxp7APtkQYpg0Idy8YArCxPjXQpTypYqr4AwjIxnvOjk3V9sk7I0y7dPGr3GPzHoDsRUROoqNs6RRUe7KbD%2FGgXrW%2F0kba8aMV30XKtGGuvFiXIDmkLiuzUMk1Tzxc9A%2B2xTnMUqbrQYMzgfzLSEzoCQK%2FbYv15sQIVOet%2FT9Q5O9B9gcTXVMmxxZiTgJZYrA7qSLIwu%2FAzL3rKxbN%2FlPUXdKcF2eM10SUa18OaqPFtqxuzQLeQmRDY5yUvHoItlknmUlKcLkK2f8VG0OTtHdDiEOTdZt9JQGhs12K%2B6NnP%2FsgHEKGQm4T0jxEScxzzhTfu%2FVPA5dd1qSg1GZx8N4muE9GW8u3dpgshru3gzL3nrH2SUDohA2TrCkA97QmdScIgmWR2Z0IoxONBrO%2BG4rPpkmx8nqeEO9NNm8yaqmmzOh9vEfjwhumVNnD8YcanvfFnsU0Xhk9btNlkw3eZcuwXzOSBwOGs7%2BZ8IBax5qXxzv9IgrV5Z1FoSIbAf1bVWufncld5VnWtxDBVHrU61GtE78dHQjDlCwoHCHBgrMLCFztQGOqUBv1kXAjRsJokHSE6W02QJESuuKzA2m2uoPL%2BKNsOH9lF0VA2A1kSHJCtwwEI4tRHJjpgeSu0HATz9h0vhII8dcTPA6aSYdIeMdtBtcEREKRUyjBbvMPKqm3dWOFFxNebeGgjWjOesZb%2B%2BYfcFESl7lQIHzgnPinITxCk2zuhV%2BnQ0owbheINbUKoecplAPUGtsVnJyKLEe99UeRYyFWdh8SrLj5JS&X-Amz-Signature=ceceafed001597b8150ff84fa5b32b1781378549f921bfa1c1044954fb10c34b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
