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
fetched_at: '2026-10-16T02:12:14.766Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WK4WBYC6%2F20261016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261016T021209Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIF811QRYQxBBfOQa3wsYDnYRfU8HMV8vqizxz48ZzOZEAiEAtBsBasGyNxTTATBBNUTMyfZRxy1p3VjajKcNcn6TiRYqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCz%2BkmTDpYa3Z9aXKCrcA96jBgHD3Mxrudc8ccA2WleSbvhxHCMcKEhQMU6Jha5EKUOV31ezzMn3uvBcJMVw67BFmu%2BlkIzZno%2FPWDISsPgrN345Yi6JVUKfayuGWXKc%2BQzFCTXDaBdP9nhEQU9Yph7pwldeWj%2FCGxY%2BnHS25j1fDmr55BRlulsY5tneeJjY8A%2F6qT%2Fj9UtPAWZVtwVVBKJtp%2BTd%2FKS4AOutYAX7Ti9P0SoR8V%2BQLifqMmqWV0CPUqV6jV3FEwjf%2FmctGgHWPLcqc6Tk8q%2BI5REep4uUXR52FijfiHORYsDI8zmNVRDCLbLhd8SwE2ILQWywZ0ys5yTE6tBzxRIqG3JDpCd9OTytKdzbAcDQ7H%2Btx358xXopIAf8PItZs86IWekSbKlwC5Xv0f5gokVYRYD50a5vWuCFTS5Z%2Bb17Hc09LirzYPP4fxR6RJVB9IGmIPdmIUMLbWS33precANdykLVB%2BA8wzjgkGokffx6I5s88jgSOO%2BSUnqNkQ4f%2FTuTxDvs%2B5BUUA0JJ7miuoLfbpADXrr%2F9lWZbfujyIE0nLCtM13GWbM0tlUy%2B8oJ%2Fxor5TjTkM%2FtymSpmmeqtAUSimahzMA9g5PF5C%2BIpmmjYEcaPIAcoP42TWNaq%2FWe49kGte6UMNTrxdYGOqUBm0p0Z6rFzoYEWhaW12e51oxvpvLsAyGnL9YWOmjDvxqh%2Bc%2B9tZrx%2BetFnHrHEAfYLBgcBv3wcSZpudA%2FttxcAdGY2Y3sjyPN3DLH0QYf1Z601gEBAyXMhJTImpuv1M%2Fh4CjOq24qLgicv%2B2sXHa9w%2FsrtREtddzsA16uj7kttHSUM8wa0DpZlJVgRDJSSATQjRoS6tVzwZ%2Fb4bzdOh4l1788Mgdf&X-Amz-Signature=e522c1e844ca4f4ce394519597734365052af4ee492b275083150f021ba7b86a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
