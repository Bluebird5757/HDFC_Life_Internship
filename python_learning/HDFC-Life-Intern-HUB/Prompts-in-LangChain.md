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
fetched_at: '2026-08-29T04:45:31.689Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W2KZDMLV%2F20260829%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260829T044527Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDDM6IRFYNF8Zt5leb5jgl7FDYTCtMuGvu5%2B9MZwmpEJwIhAIiqRMDhNZIAGahhEuVFgtNRm4QIGyVBb7o%2FbD%2B4g8MxKv8DCF0QABoMNjM3NDIzMTgzODA1IgzjQxncxSu%2FurWDQB4q3AP4mTOghby4RzbET6%2Bmsk1Xl4EiUULqe17IMToUg9QreYfkSzNjD9k%2BtDu5FjU0tf2czHcFebSmGOK6ak6oLG2Z4nho8t4VTUSEOnQX%2F6vDVc6i62AjY3K7mKHAMlJPUKYzWskCX62S9hE7GgvxqFwQk8ulVysf8mSAcNd7SLWo%2FziLbDu5h1vWeyDKGW3YLyFJI7b9JcabYGY4YTa1Mxaz0EvKFWNF31X5JfYI%2BVFTlPbd5fiRXBwwILOe8QKijGuPhf6tnfzqMSgWjkIXDCv6SnHnGWz4acD7HTA46lsPbclJREcYpPFqMph5QtzcfZ4MADDobF7PW1b8bwrgjNGtEFwEViDgY1xGZMEbwPDA%2BQ4VdFUNxBwpCNLTl3J2n1xsE3GblCk0SBH%2BGGFTiAQDaQTXPwkJ%2BnfnG2lnbCtx8401KGIQpqeyI4ZZjJyFGvKhMQetdbe2wu2tDHzGblgDXeU2fPIeFq4OHJf2GX2DCzdLYm8nWywigT%2BOmR3tqiV6SD%2BNlvMcNJ48RsdPp8uQxit8PpcOfQWiGVxbe4C8%2FW%2F%2FR4xMBFZAyGGuZdFWMKwFGF0IGcIp3mzRYrGaW%2F50m8jZbfRzGXr%2BPeCQdF40TPGAnaL4bRYNCmDEcjCgvMnUBjqkAdyShrHSwb7mv%2BH6DSKXUWKdJ7fjRAUxdO%2BRpDr4dvVy333AJUxMbKdPE8vUL78ytbYWYKpTsMu7qE7CRNXMmRtRSVTSkMgweZeoflUtxhWeYIagFT47sB%2F%2FSTE%2FN6jvY1sAjyf9mVvhG1kzNWCeQ7Pzby8y6Dhf4cIYbYh%2FttDuN25SYjJTnhepHWb8ePwxr9pyqtQXEugsDaJvHyX4q5BmSekA&X-Amz-Signature=bd99c77d7a496befc22020d818dca919e8e66338537ac40cf36c1213c9539201&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
