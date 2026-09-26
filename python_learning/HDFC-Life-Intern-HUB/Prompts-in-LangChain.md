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
fetched_at: '2026-09-26T02:31:25.563Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TC6NRBTR%2F20260926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260926T023119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIQCt8bj872m3WW%2FqhCvNY%2BqwiZHLwrgo64sfzTWFZWWg%2BwIgUMQ86DnTZvXqHRZyE2CyfUe72%2FyrJ1xNV7lU5o1%2B2ksqiAQI%2Bv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD8MtmpXJF8bnhVvXSrcA0w%2BRU3W9418djj9IkJZ7q5wMSvs0%2Bh5ry6VELiCeVY16VWHjVuwdGzshe0fVTGDk%2Bynz7h0WqQS5rDtH9VuRDYpDrU1SFNokYI0yTIfTwu9MDDsuMC4%2Fw0%2Fwuks4C%2B9adlpmhgvU7nPesCynnZ51I364p%2FtO4kg8kjI9LrUZjRCQyyK2EKivOGUA9U%2BJjrn7xyaO1nc57pFUrbq%2FOJc8TOZk9KwjqsZ49rhpvrHf7XGzfK3Wc1Aw%2BMQydWv%2FmLvARekxp3ADfDjeRI5kDpfnBZZwwXYq99iUd0mA6oPZSCIc15okSrDTJ3pDgUz%2BMvjNcIXFv6EyphqOMtvIqnDBvatrykA9JllNa1OWdvku3MpfZOsNT0GlClaoZbrJjaYAgxpaw9Z3f%2B1mbDkhk7J3GSMg0JouKR2ALf7a%2BFLCWPDU0GE9UyBSWNLnqXztivhD0e6VDYioCuah2z0B1FnwlgiNGMVJ0LRjzs29gO7qUCgB%2FIpMa5LUx6Eif%2F5UJSyrZ1Rt5CyzgZkGlgiahnDcpX7O7sQPc7UeLWxFN9OHPbDf3P8ziiUyf8sD%2BcVUfiX6QsiGUlP%2FHaH8wt%2FOQjcNkyTw11hK2WyK5ishvlbL8%2FCIMvk4RDOdgl0V9iIMOeq3NUGOqUB8Mm6ZE2wpjwgbrp5mwNMqsHmaK0U1XbpT5IbVBlmv%2Fv8Cs%2BvwdT6K00n3NUhaeUzkDi2PJteyYZIs9uGfcSp3bwElw4NVTPJvZLIYGJGhf7QkXH2aRkbvKIY5GdWj%2B277hG%2F2bRkp%2FMpt%2Bp1Kvu2blPD%2FFy60059zngM8DIVYHIuqukNoiH9LOm%2BA%2Bo84LGDetjkC07WQZkJQ9jVf5JepvT0Jmke&X-Amz-Signature=fee6cb275c36369db2d3cd4be39f67f5847cea3cc68c6bd78c17de6109f6bbe7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
