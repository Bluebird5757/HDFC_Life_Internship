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
fetched_at: '2026-09-15T02:25:22.661Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IRLRVOD%2F20260915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260915T022518Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCICeRnTk3gvvtqWanwefOCDVBqu%2B%2BMuFv%2BZkHypS%2Bdv8%2BAiBVThrkRwAOcmG3iH30W3xJrgSRotc7enSjXsiz%2F2%2B%2FSyqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM35iFMZNpz8fSgk%2FfKtwDTz%2FDCYmTrnpaOpYUxcCLkpF6mK9ascwmaq%2Ft2jaCEdLKFSl9i6IO6JNMcLwh1Br9F%2FihHUtA%2Bk8n8Hzm3ikWQM6YkW2PrY63c9s5PLwrM4XYhvvcWTN%2Bgdelt0T%2BJUIZAwg7Q9FqtXixiulmqpQ92YYoDvactV4IdEOENW7cstBmnp8Qq5UmoG2ahnm8yWLpRLJf9eDF3BPOfRjXz5sZK7u%2Bk7uaJN19NHqRIYiOQxKZydAXFDooJYFzrHl%2B5Db0w7rb68O5pkRpaZdf64ays5nwBLeDdQ1XoXjYalcZbkc5odgRWIGGQYQYeQdmkmx5ke7GA73tPyJhDft2unexB9Gcb0cEI5BrFdzWlniWJwM%2FCvKz3SkreKYnV5c2%2B45lmDCdbQBhoDwD67%2B29g4WwjrcqFm6MHdUFUr6zuS38%2B%2BBFx3orjbvD%2F18%2BMhyGn%2BvA1SlMsUzdcGBWGnzw3QmfChpcPMZZtaOCOXkXrq8R7L%2BOEOubhhSCdB9c0w8agGwyoH7D0S%2BGnuibfTe7yRzFaGayhZbkI8hikrHRz%2FOcI4Oi28ArT%2BB%2Foi93H3cfRd7Yp%2BjVAKNu5Zm5XpTS%2FPIJ90eEubz7Np4aFCcc3Vyyuc9d5KxML%2FnCDCJvrYwqqCi1QY6pgHoGnYqxMSqc3Vl4pjtJA9t3f8ExA9NM%2BnctH48S75GUwHiyxi7pHGShI4VU%2B6K8DfY66%2FvTqlyiYhPd%2BHchj4wGNE1TzzSr8AOSsnm0HCd213IDjv8yFtVNvoh1xxpOu5DHmMWwIIz%2BqXbgfGIM5tcnM%2FHehDYjopjCroOKF0C4u6ZhLxKhFCAzqxDeg1qZWf2tV0nUcvwzNW3TDqNFN2fXEEk1cDO&X-Amz-Signature=88db9dc685ba057c7e7d94a52492aa51484da37eeea23bbc89b4d1db0e608197&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
