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
fetched_at: '2026-09-14T02:19:31.849Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVHNBSYU%2F20260914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260914T021925Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIAF72NiABB8OZ9tVNhuaTSCbSW2SI0JEZ8nvmHFleSL3AiEA9XBdkGacxaszQGqEi6XADH4pHgfiBpAIM90PElE%2B8i0qiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOKQIVARvy6UwITN4SrcA9yHmK23gA5T2KQ5Riv9VBjtNpCWWwfNo4qwbLcHOwXmsvHVwxulI03zKMJV1d8BpcoUzQv3vZ0%2FHnQuDWe8gsw8QH7%2FvhHb8ZN1%2BJg4Ildq50azQXBmd1e0enXWhNHCuHi65vU3jjgf8Gwpv4m620fGAP78%2Bj%2BFqZgAtqgHFPC7sXGIt4m63GUb%2FEhNHho%2FVfxkZ50vPhiGNl4ATC4yPdin4sLbsgfF3qHYAodG4i1xYj1Vh1I3deZPrOKLVyNNbgHHH%2FV8S6V6YKzZDMozpm%2FfKtsLIHK61k4wQQXLWCF3dnmYr9lCyTt6IKOYn9OPxDgAD3OyBXXeiLM1w0%2FyUR%2Fo%2FUxxLeaaA%2BZ0vkeMiRSu0qjOLLoR2%2BA%2FjY7Kqp88MekcDOlfG83B2NU79qASu2VjX%2B2Z56uM3KNWnitNhtb%2B99bOCnSDIFky8CD0gHztdWNhZtCFzdFhRtxAn0BPb%2Bz6Ulh6sW4OiUxoHccSFiFKZ6q%2FGgYK3ECibc080fjJKDP3UHAzUIloGF310BsYbp4LsbelcPdZrHTTv5BvH3Dy%2B%2BvKZ4y8y%2BNewoxnMqwd5oJHs%2BY4l5JCPmwpkggoofbzuZInTqtuSzuavCJCZMHQk10x3f%2BJ1wk0cfGTMKmDndUGOqUBQL8qMJ%2BFdPn%2BXEHty8tJBTGazOswrMXrTdYzfEas2bka%2BZLVmDAlBejsLnBBdegO1M6%2FmHFCEsh0EQCsB0cjw6CKD%2BnFslDz2D%2B4ANiuQlNzc9we2FTz7V8QYdE%2BoKYEDNFT7tY%2FZzPrtV%2Bd62Umi%2Fkyp4oOi7Grnt1%2F3mUyzBC9L0%2F8nXDTavLP90U2Lmhwia1lX9o4AHymO75Yn%2BMfYWe%2BRRx5&X-Amz-Signature=de4b96d5517523e200a4e082928963324862f9a6c9543fc662a255a891d100cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
