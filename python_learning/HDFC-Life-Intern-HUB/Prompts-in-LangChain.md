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
fetched_at: '2026-08-24T00:40:02.068Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ME3AMXB%2F20260824%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260824T003959Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIHpDoWLCz9glFZWN87UntFW37by%2FXrBhDGtH436isUC1AiEAq00B4%2BpINsB2%2FmSVc0gXfe8FKa8srJyGvYGnNZXCqLMqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJbA2otDbCTDC2z%2BtircA3%2B3lP5bcqAG1vj6gvvtyhZ4uCVqvwExWIrsROOT%2BU3g5srB8aBJJuXL41t0d1RA95oPnhVd%2FWw2S5YaDgZfuFOpaQphI%2FlHcVLOQKNzvK%2FL9IVQfRAfaUe2BvfgP%2BNKj7Gw65oqjz8PQrnM2FKDE%2BSFsW7e5O7BcsN84RlGMCsCorNR7SBHqCIPjD%2FgelKdS5FNtZZU%2FvFk31jh0fyQCKw2p%2Fus3hrbIdl%2Fqhyg8Vwgv0MTIdnRgyg9n8lNDIYa2p2UodxAkcYZe1Xp99t4rfONOqRP%2Fb%2FSn7xF1iP06XyGFn1I536I%2Bivi235fM6k6QaFpicCQMsGxrgvdOXP7NS59eDRexqa3cbz0x1LdytOF6pyRPWvugVuzqD%2FHz6YNbid8KR%2FaguhAoU0I1SoonooMI6TBhcpxKTTWmHP%2FQF%2FEma%2FHUpI%2FdLZTVLt6AqvvmJ7y2OXq4GQjuRjaI%2Fe9Xi%2B43EkxKlN0w%2BtNnpWqEBTbrUVZaOpg6MgRMydU0HXUYhVbeCGn7ySY%2B47HMLtDPMeEkxVyft0nDkY2pZGQPqRnVaLpEmUwSC7iA4N307HdIzxP93iDsMfWJ0ba5j2D0Z54c6kYm0wYgmyiYuGqWPeuyvjX8FHmUVN%2B0NFEMJakrtQGOqUBSuhf%2F5DteC%2B1BAQM2rDqxdhkBTqN5BZGHXu03GgFlXvwkV%2BbAZbQnOnmIUIirrBnTjGhbdkrPNQGGCfvWI54rV%2F3x2ggQeWpFtYxPuWSyR9fZvaXnOS2FyRYXl6wwjRgrx6vWHo3%2BP%2B6i4BBvXrlpZNJttAa5TNIYmS3QMfjDm4Cem9szyI4v6%2BFmZtlNwsqM60%2Bd%2FYKY22vd6s8lu8eom6V3M3F&X-Amz-Signature=9abc919528fc0416242390433032524c18a576b187901ac42853a0b05c0db741&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
