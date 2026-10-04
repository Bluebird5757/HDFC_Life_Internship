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
fetched_at: '2026-10-04T03:22:37.764Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ME6LMMI%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032232Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGSX96R%2Fme8Eb67f7XxbdtoyNZWe69qXpTgtGubYIRhwIgCDHo8E%2BUdaWHkpKrUGp%2BwkwUefKscZm4nH2ArK8XAYUqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI2CECNwOfLkPqBqsSrcA%2FI5tL13yXJM2omUrui3Y0l3BfpIe78Md4HUP25U0dyjuRnvzSTWAIO%2F%2FT6Zsm4Tnrp1u3pA2dNAqi5bVVLfWrzS6EguiXSyrpQb6tD1Muavl9SGgR9SMVGvy0HkZhPfpRassThXTUveVGtNmxs80tQ0c4NgF8J%2B59zWUE8jYoLEbaCKEHTKf88ujzP82I9nxaH4tFq3ThT6okARLFx%2Fy65Fq0vmElXkOrsvyZPt1t2mZriKHYd7iZDYF42hbfR79b6CLXNOwbWj1GErwWZqpSpE%2FmzwnKREXQ3VtUSw6b1NmbHeNBYP3uaK8fn4hAqNwzVpBBCnWd3gfnTwIK1sESImgAWTdEaV88xIK5sQvx4PCuNx%2FgeVP%2FcZANInBLsIMJeY6iE64nFNznZ0TWqXn9WWzYqPCPfG4fVYIPxm1EKOe7w7kpXPHyvyHrYwn6qveXTWJ6L2Y00DmuEDvLwgfNyjyy2TID1gU8U9RJERgh2DzlwcivUJm3juZCNIW6G6eA9iEbP3kI4soqNvxsnWSZ6USjmSC0bPIk3RQeYmU9Vj8DqqC3712hK3CeC%2FtuWNnX%2FtwkdjiSfQOkJR1H1znfJ%2B2n19B0A9ImstMOzbZ5TtxQYw5zxqckShoqZZMI2Eh9YGOqUB5SfQf1a3uqyhXWQHIaBYPGO9OXUsRBgTVdpC5TyKNlJTXALDqtR%2FnX3NEZHx%2B47wOGx91t0Y0Gh8DMhS0AR%2BNTtTM30DipdSRkZsFgM0idkGh6cdNn9Y47XJJHQ%2FpCzE8WQ5qr%2B4HEu%2FDyBFprjwxoULUIAg1u6oS86%2FZr29KR7JzVd2RMFH3VPRiUgBIsmn7E9Ezga%2FXdZ4bLJzt4UXcWa8iXMS&X-Amz-Signature=d4002d7c123ef1467dbf4b94c58c5996d304972158c6f34de4b9b77b24297b58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
