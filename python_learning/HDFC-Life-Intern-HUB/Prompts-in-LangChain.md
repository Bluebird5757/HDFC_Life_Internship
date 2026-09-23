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
fetched_at: '2026-09-23T02:23:06.015Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667O3BY3F7%2F20260923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260923T022259Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDTo2zL9BdJj%2FTKXL%2Bv5nueQ785ye398U2mA0laeebUEAIhAPljq%2B6wDj30mkqVHWqn7c2EXgRQTFbYwkbPBJAfTViZKogECLP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw2SbAuPnJZqTQICesq3APjejtnCHJt2mhU8gyZQMZ1CuhXx1R0uKfiKuOlmrWBlSULh8gnb25lqfdEpwYdiwr2Bbv3ZPmjj%2FSzfMOYKSbMwT5gjfyPZDg%2F%2BsPqXXRz08BtNZagpeVVRe%2FGnuCRAD8QOC8i%2BhfVuNPA5R%2BHYyB6Cit59GfS%2BFJF%2Bi2xZeH5%2FdX3PXz%2B8XnQeKgOe5IWcNs3u5qbdxw9OopUSa23LxWdS0Kyexa0feEttP21ieZoVKoYR9kpCa1nXcGzUikrReth8HBMJFIMUA6orLweikM2vKGTZ8%2BdcDMhYthB6yMo3i4ktJXx%2FsTjeTs2hVUEUlwkMa%2BgqCHI1TfSTyscvIYWjSaaJOaDdgQdR%2Bb9rrjRo8C2LV4CarP%2FV9B0gX2EQRwJGsHVjbU6OoIs83bDwr753H0tALYcc05cdNIni2dRbpk4U%2BnckJF%2B3lLhhxmOaYLLkIg7jfYkWSXBN9eoHfGdsTS550NpBpvWNKXG%2FBp67xcaus%2BYZaTnh79MF74szPdmlYE7DvftNUrpMlwiUwscit4%2BjoWXXsSSevHRsep78a2IE8WSfVw%2FpVW%2BBFda3lU6tRRTvd9O%2FtuYT4aM%2BuqK7PszoTh7nAkcBXDZtwOM9HcQ1882Q%2FiP9qP0fzDM1szVBjqkAe4%2F%2FF2JmKZ3kwp7Y2dO8aSPTJFSBiDuGeo05IkYpNfYYCx8CfFHnPZ1t%2FcmJ7ejJ7IQTalds3n0X8iro1y5f%2BUuJO%2Br2bMNpd2WAMIm0NCtx1ctwDH%2F%2BHY54K2hfYLUmPmMVzGwrohDmnWUlVL2V1zHSkwLyOK2q0cQ1zIea1%2BYilcv3xC%2FlGXnrzmfGUuUCMWmHKH8BjlaZIouCjLmyA1v79qY&X-Amz-Signature=55486dd787bfff5e752b8a1f5d8951377f86ef9888c9c010cb05d52d263e716f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
