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
fetched_at: '2026-08-28T07:51:43.274Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TQHALX64%2F20260828%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260828T075138Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJIMEYCIQCzlM1Z6zUiMvx4tcf4%2FFNY2Z8RphTl5NR7l%2FcdVfT7twIhAJNxt8g6aM%2BneeN7lsUDPfU2K2UUp8%2FcDlok6C8SDv5rKv8DCEgQABoMNjM3NDIzMTgzODA1IgyQknIPoN0IcIpm7Goq3ANOgeSW%2B0aBjTm5rKkw9HVZhsDwI4qLjGOkm8drl2lLVN%2BMAEjtN%2BhKP0Ck43Yzkiam0UXfhfiDEqkT05hMCHXUS6EpvE%2FfzARdvB3pwyDovQXgO3dpavGkYFA8HCVjqiE5V96XOIQM3oeAfNalXtjIQsPy9SH03SLMmnXHvE%2FE%2Fa%2FeBlGZVUsXlv5ycBU0XwIuvocFARkInyzWSrTgxefHrXAVh0dKcp2YETFkHZkoAYr67kpGFCJuErsGpc8xgwJnCMutCcmzBi2EGV7GhyG1zgF6wdLZoM2GI8ETXs9lEZ5Zv1yq%2BbRSdmRgQSgwY7o%2Bm94H%2FgJ8BJogrh4Ii9lB%2BCoucNKTkSt5jNmcuDHOhB%2BNHWnVqaucwTZxCa%2B4LMt69qAdD4HyMBQdKkkRBGmbeHl%2FtylHkj9p3f0r89S3e074hOWx%2FcWIs59zdUaU%2FzTGSTSOZ3v39wXlM0V%2Fzhk1fRsDi61Z0OV3K6rMuPPPfIU7tIAfP2PXdZvMV4AaWBGvQXw4Yc1wR0Xi%2FQ%2FGKbz08w6ty1aFOBOu2eTKAWLkHJ4veyGzC9l5h8roQT634F1MMPzRzUVNAfN%2FnDeLtb1wVK3EnXIs71LCZqwWGpz40fbbBDCNE1yw64w4dDDU08TUBjqkAZVnphTOERs%2BEq9iijnungyPAbZfxzoZE6%2F0%2Fw%2Fm1YN1LBA7Lu9ui3mqXMVIhxOA9RiYZFNQJAkAq%2FxLLEUti1ihFM%2Bs9QbQe8cPPbAXjXrsRajYC4TaJrudxr8qkMTj4cB2Xr5AscN%2FLwaRyAoZnX%2B2haN0W1qqv8EINi1J5E6TQJ947UAV4vIMHUoeNBzG8hCkjYXCKxUqdQtfepR8%2FmDo2w7l&X-Amz-Signature=3d9e9765357317cf12f75e11f42c3e72e6fabdaab5e6e59a4d0d2a64cbd62106&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
