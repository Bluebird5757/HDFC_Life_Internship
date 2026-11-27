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
fetched_at: '2026-11-27T05:49:31.920Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V5ZSTBEL%2F20261127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261127T054925Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJIMEYCIQDkmB5kIK7YXmDr0aax8IaS9hhaA1zGxHtJw8UKHhshnwIhANkbPlhAu8av6OKFkMVb566w6uXXa1dUzX1iHzG5Z11WKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgydjMnZPd%2ByFpdVIfgq3AP3Iq8C9Cad%2FV4IaoIrgWw1%2BX34qDwSZC5AkfntI5Q28E3ULVZO7Pe6JmH3GBKpv6%2FJEjpJayraCfrWhJjixLwRKCDvEfHnNtsHlQlA1T%2F2Ti%2BiIiCNtTo3mDTqf0nN8uvjwIdYcE1DJxhBQV4lqpclSHEr7lx8jKTtukUSbW1I%2B6zvXiyvOPzgaGLSsDP531KNJ8skpQ%2B2F3Ga8zZsPtDXz76UtfmL%2FWq9QgjlBJzWGewnlpaCtdVrmuXHh7F7%2FqQsz6Ky4JGw0P5FqGKi7ftoSNF4VPPMEZdqvaLFqFAAvQBvXi503Ea0BhvP71%2B1AaLhlQ28TSGeFcTQJj2sIEshULj9si9Wx8GKfMDoODSGJdFHXPq%2BfVJ%2BVaofdjhpNRY2mkcGu4xorFk%2F0F779LAzLCG86HspxRDMZB%2F0fgo7wFvtZLNGpvuidDl7JQFwwZ49IAZN2i3hdy%2FV2shVSflMOilNiVKJFKIhD5Zv%2BpKTc1%2FwEtleZZOh6Cqk4td88isStTp619QhMgY3M0rVRIcOZLwKav0DAmWvFHaW4%2B61BldoGFdEcyDrJ2HGYSsr42tkGDAZuF8BYOUOIHsP9twTMj0%2Ftlx%2BUqyvog7Kx99YBPEXWHGB6A2tvtHfNDDQt6PYBjqkARe1y%2FID9EMyB%2FgaK5ZS7Lgce1k6ZrNz1VgfASLBQud2ZeMF2iduDAaHTYkGK3M9%2BdJSiy8veMPDgTMmSY6YbgZDi9CSF2SbqDA6627XIOyanYYe%2BBOYeuLMLU4eqpt1LyszAZX0fhthQMdrJ4A6JN0ysPmk%2BwaR8xxxX7BFWpoCQ1CB8vwFMu6YcBfx%2B8ati5teAGPdpBbyByRNwJ8KjfKPiI%2Bz&X-Amz-Signature=5d94accd03119edbd51e1b5796552e0e9c318295d44c0019df1a1b4ec29b8c25&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
