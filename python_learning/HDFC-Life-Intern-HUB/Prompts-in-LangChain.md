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
fetched_at: '2026-11-04T03:52:49.976Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S47CNGVO%2F20261104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261104T035245Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGrOdGLiL5zP4Eg%2BdeMmQpKdisvHko1xvO2X0joZL3ilAiEAwIHTjFMAkybON6SITryWRx00y0DetGZnFGDQ4ariT5MqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLFXvicd5bB2fF%2BZkCrcA8GubZ%2BxyryM9CEA%2Ftmd97kmq163%2B9NbinaVJw3ZJBmVzbTgbIEWGHQpov52yxpwj2V3heutuK4LimSoh0469j2VXojLymhxs2WPpXNoP1S7W1i2qBvAQfZ1SJxqaqXNpFlGFpOOKA3m9yup%2FRs41OggYwOONsgtWpsLx3fk6jYmpDQDDbU%2FJlHWeV9ZJn8ENzYJmfsD5R%2Bb9Ksc6Q8SDpt6kTA9IsrDY74FEGcXWcYiXBeE%2BTMF9mRr61Vhbn%2Ft9vdM3w6VzT%2BIPD%2FF4KWWzta6ZdzHJLtskWxtEtuSJfoICxparBWDk4NSvjhYes0NQNZOSA9a8E9hfTO2TR1wqwPM2MSsGS%2Bx2j1G8ouNF8vkyzHLjM8Olq07Rai4Abi005K9%2BoitDeckg6k8WTZIorqlM8C3d0%2FLbqbJi%2FYhyg0Ck%2Bol2rWaIIcrBPOXEjfA2q2%2FlZoB%2BjJxA02j737lYob0csJnMKeTr6U%2B2%2F11LI%2BkT8jgMwCMH%2FJUF6EYEHoiUJa7%2B88k8B7wNrsZxmj%2BGtUagB8LB7Zam7kC%2B5SoykzMVWMGBKO645HHIzwc68VTy4cGw2QvL7HDD7U5dbkumwi7E%2F%2B3hcQOwC6RUe5RSozee22h%2FCQ5zmqJ0zlvMNyvqtcGOqUBSQlDGgmvchT6%2BPDroMkA8QKB6H0wDgXqJjCwo1VwXWXL8g26l1N0Jh1couZlFmfhOoxpujlhM7Ju6TSs%2F6kwidVpFSDMbBKpz%2FMkG%2FpVz%2BemO5GASj%2FAUhde0MwaIzyb0DSZOIsgH5Rp3L2kW0P4ONuY3VzjOzkbQ58LmMjPcWFrtUZqbLB9LM06wgmH5dqY4fvtzHXf6rjMaVhfSOL1SqHYN9Oy&X-Amz-Signature=7f0db6f97196de37e7b328ee192b7b7880405456131d7d1748493a79281cb2f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
