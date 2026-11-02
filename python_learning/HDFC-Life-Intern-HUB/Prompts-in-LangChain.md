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
fetched_at: '2026-11-02T03:21:09.453Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PMRKJHK%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGc7jL%2Fik%2BM84lC4%2BILrYYHXZR9hJl%2BxGIiFhSv8gztuAiBzOR0SP2ZMk%2BRS29NDNBXSjBj29V0%2FXDO8yXI7tponSSr%2FAwhzEAAaDDYzNzQyMzE4MzgwNSIMncE%2F60kmcuXo2TAmKtwDvmdjLH%2BHW%2FCVwuXGivU7oh%2FpoVeyr3iPtgogQQ1CVFyZdaxVkkLvlJouhCqxbdY4VPczxwh1clltXMxM799Hh6mCFNwOmgtPJVNkY5WAMzfrzi6uYZeJgIF%2By2DBNccttXuPOrAziCfK8MIQBd21mOVBPRs4idXM%2Bh7EpGTAPxKp6AOsOcZcrGejk6JRIuA2c%2FzRavyMn4j%2BKLDuO8F%2FH0llsdpJUCszmj3t7VQTbRgwtZLQZ2n1lDpG%2FHToAjf1jHSVHMT1XnJZN3x71U04JsVkJNmdz%2BwIaSNIudKOJ3f1XC9zpUrokHuHo%2BlDvuKdxOSm14h13lR5CkCWg7tgFE8vuNtmMQN%2Bu3r5sYXG%2F6Wd1ES3OCax8vQzg1x3eI7P4AOjP0ZRqRcgjYD4CRpnd2dX2BHX%2BQQ2xnumP6KZN8Ao%2FB%2BzQzCKdYrTv%2FVN9ScLGhbXodaF41HIo51Kd0so1Ps%2BaKNGRf%2FcXUQ7jFCOV0Eq%2Bc9yYjZ9YMZXGLCgAnENZYsEn3%2B1JdE72zE8GYcDxiQs0tMTzvLTP5pfYlRf6LV0jLEk4ArprRiAK1tN0jLTF5eFKPrSIVsaPrx9jiAMCkJ9gcKfT1%2BzQDbNhjC3SwsWZXr7F1OyWOqlWWUwoPqf1wY6pgGxLYCuSM0ocZJZKUk8d6OpfW7W%2Fnts1Ui2V51TTVuVzSjx%2FWi2v7EJUEKZBHmciaKy%2F70SKRNgVhXyKQoDIDYsFMWjWfn1KX6VwU5mhLdmoS2PzC1A3eTNe%2BwqZx0nymoogJIzT2bdy%2BgY7cPd9oH6%2BRY5ATt3Cq8kPHKCjtv6yEGY2AADEsf6ycpuMc7GZosSSvqkxIZ52IDNs%2ByWBwNKkCyY8Qsn&X-Amz-Signature=1a96a577b55a21039c1fe86ecc7586b025434b51f93d4d0346681f6350c5cb70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
