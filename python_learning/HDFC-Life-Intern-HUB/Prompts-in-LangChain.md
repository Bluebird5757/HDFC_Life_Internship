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
fetched_at: '2026-10-19T02:21:58.779Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WAGEXIV%2F20261019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261019T022154Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIHQ7aYcyJFtm2AnfdIb9Rly5beNb4hf%2FRJnvJg2APUUSAiEAgh7MzAqygX7kz1HvN3LJgHE7ZzDTVzAYiPXQmpj0Sxsq%2FwMIIxAAGgw2Mzc0MjMxODM4MDUiDBIgICMgPhP7Y5io9yrcA%2Be%2BewB6lx%2FSfjRnrEtfCQBGNjylIzJV4bghKLuUjDUnJsSm8x0j56kyXnDRabuaomql9TCdUEU%2BGsy2BaeW1drVm77%2FxAZuTbCsZZo76ozQxxI7l2PUnVZClITHiqqeHymhKvUo6Lhk3OwVbVqrae6IEOTahCyTLhIj377vrYNCNSkrfvzwNyhmOAQY9sS1pgiihdgCbmbfS2uaDqU2JMfv9r4T7q00R1jUf%2BxUGVJ389hHTrI652nIYnUWqPRKpT3fFuqmVDQbzClfoQX6vZ57%2BfJfwW2h3yvCwgVNb%2BI1MIUWlxuZ2VeCOzTWmdkXvh6RV8AR51jZnRNbdAzYQGJlOV2nqCuKLxzkWxIndVlRNToqskep9J5eK4aWN0HYDHpwUVtUQyCNYRCATfVTJpJa0SR06FOAof%2B9n3FD4XKdXt8whvDOZNcUBDDmnJSAeS5PMXnifA9A0Lm86GpLh1h2h2bMWwFC4jeut9dTkrHXslobFWef240n6kSNgQhRUHl4DmcGh8HhxvOUKN2XpvoLiZ2CEEVx8gDfF7WZxls1BlhlJLZ5yuMO7TZeksy7QC9wSaJ%2FdEQ9gRRZqZRbiFpa8VGE9JwdENAZTN1A%2B3uf54PUjI2fOwa4vNWTMJTu1dYGOqUB1vAvHsGtrF0J%2Bx%2Bk7esqCZr2dPK4myu4YwHjNbr8o1pcJy91IazBCE%2FrVpYdhM0y9ER4tMTvk1aQzdscjX%2BJ9uAk4XCN%2Fx2HBV3Xilkf1%2Bc3bim4jEEPNWSx1sb%2FPTieJfoopFl%2FjJjPLvyzrR0HZ38LYNiN7%2F44b82oPipE%2BkOEnXyBrmhgjqRw0XQ18fABW03WqmLA5qSDSV5GhXZ6DJ2x8Sf8&X-Amz-Signature=4f20b30eacba5579b3ec6f768ab805615fcb7feb7cec5daab8caa84a7fef19e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
