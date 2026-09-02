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
fetched_at: '2026-09-02T01:56:38.578Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OLU7BYY%2F20260902%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260902T015634Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCPX5QzYmsh8dG9kXspCyqQYcfxxBWv05NGj69yPhfaZAIhAN7t4Z1RSxRzqyY9FlWxgRCqM9KRd0DARD9fxpq7GNeXKogECLr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwTyY7uU%2F%2BwAD23Y44q3APfd9%2FCjX6ZLafEXv%2B%2BgLTL2mHMFPujrO3KXt4i8qGp7gWBCENronzy7spKAVcdfQPY0FZGr0G8v8WGVfyFtU392esoi%2BM%2FBPFjPANC%2BMvFoBqNPi0IZ0o8vH%2FwGS6DB64nneXw2ylzra7kkTNeyRPtPU6r28gfRnio99FZIPqaMYn%2FaPSCYDxhTI6CYNI0qStpGQD4zz6qrrphLuFMiPPjFwTwtcls5IAWaJoPANFHIIUz%2B4579b99cyZOKlqEBOnkfi72U69zNqIuwBeooJtrtM934CN3L6Hig5egzo6M8akD8UzCHdeo7%2FLmSrd3lyX06TzSudVP7tjvAfaug1JD0prQtPxPQB8sb95hU7DlWuPaNpeyol2wNSgGdMlxK0MK2%2BeIOofTZDknrHAf7TRejQSkOJAC%2FNKkKg%2BthuXCcoAdhmq7iJyfe%2FbH6yj5Sm4Ae7NLIEZzY%2BFq34IPBzhdSP3fSzWEFKeAueZkVXLuA5l8ufggBxHxxJPPpCO3jAbGQ7s90pNzoY2do%2BpSNF2%2BK8pxU9%2FeQWa%2BnHgB%2BC5bDoruYGAe9T9aXRxVq6R2N%2FFDS4H%2B8kJIBRqg7gARCEMUg%2F4fb0WI9Oe3h4o4NIrRjRrkItUWOCo4vHkzpzDD5d3UBjqkAf3li%2Fu0hRKUD3FVzOAAXpRACjM38ohUJAAGctSNwrib4m95rfvztvKcvIjyJWp%2Frh8hB9vVQ%2FACcw13HAH3stVbsW0hVmYDGG0iQ2m4zylJcmGstJohuyFJxJ4Ac3nnaIj1IAqZ9MtGBb5YB%2FrRNbqgesKAF%2B7Ua8W2MvzQSISmI0lr2AItEz5A%2BqSHPo32%2FBQ38hMF1auebCNQIwtW7yiyJ1aF&X-Amz-Signature=6db17a943a0ee9b661629cc2affd05bedfba33ff3b4003937a79e97c4777ca16&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
