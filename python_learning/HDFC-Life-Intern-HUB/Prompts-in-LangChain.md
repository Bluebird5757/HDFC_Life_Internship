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
fetched_at: '2026-09-20T02:19:38.983Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QSSJ3HMX%2F20260920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260920T021933Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICqwPXXgrADYK5aEPnEva23nGH6m11iagsh4eD2kprewAiApQ9vIBKnen3QP2AjaCt3Ci4knw0s4ISUEor8sVUHj9ir%2FAwhnEAAaDDYzNzQyMzE4MzgwNSIMBTAG23iKAcc0lp%2FqKtwDoYfbEJ%2Fvu2AjS8aoBB%2FILugLPOb2Po8dkInOVxXZU57R%2BBkX2gtRsd4H5nI9XG2%2B2giQ2J5UdoaXUpy99qypMoHXGOZAXc1KTiE2OJp6c490usgaPjiCwTMh46SrChYNKPvDMpGCEOXnPQN07fGWLjFifRqkZNVmxygZlynElRC7eSvsl%2BREH6AK9BnHGdDe1ysOQIFGvYDrHg%2BQUB52ZfaXQjAdZrV3dvGbs%2FOAFoM%2B9ViqfWRoM%2BsOURHDcqMiPPnX1sgs45FMr%2F7wTbCYTn2Xyv7wCSbqb1Go8LhD0VscLHnyghVss13jOoYN6kdUFCrEPQHP1swTIN3lPYFm97aPg2eocs0kD3wj6HKfSOHCvcAwBeprMpYan1fYFzEEXo%2Bpb7m5OqwZKXJ6KYoqX12g4nvY8qDMyc5ECdz%2F%2FshDuqGrAzOlcZknQH8TaRwHGnjiOarky6ICRcZDHtZZfdx4i8xcntkttfbzH9MHWQ9PfMMGjUi1%2FWTpZ%2BF4C2uD6mfvegEPv4TT%2BfYunXZHaD%2B5zR2o43ixp%2F8VWwoPZmvamtXuxlH2woqFDtpcgUVD8XQphHAcpFZmE7w81G2IjH3yhzJoEEKwfYD7P6NddV8pDOezrZy00drScQYw1pO81QY6pgH%2FM0CPnKEei2AyOIaDKUuJpfzb79QNWrejom6HGb4mUGI7m2Wt3wqOb%2BH1IdAHcn5m20NwrCft3Air7dT3nYvxrtZlVlHPN0IFhOy9x3iwrht1O%2BpEP4vrJtsX2Jab6oZE2aJeKvAkwnT8nQvyR0E1byqnrWe2dSX5oCL82N93D%2BkQtW80vT4wfelCoFN7g3N1qPw3ofRqwKzTLfjqoWGqgK8kgTuy&X-Amz-Signature=3021a05eee480a427211149299e01a3f6d237ac48e5ec680f41e03b4f6a9a52f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
