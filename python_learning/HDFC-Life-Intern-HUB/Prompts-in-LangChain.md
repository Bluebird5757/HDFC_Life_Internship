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
fetched_at: '2026-11-19T04:59:38.911Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UILFOT6C%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045933Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIFSglyWl2IAlbez%2F1pkvlyugs80OWaaZPNlkBlO6EDMXAiEA3XYnsAnQ1pxElEofninEVcy%2BbxUikgBe4Q%2Fy2035Cosq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDDsX5nmACbY6iiF2PCrcA559huz9ReswDn2x%2FTiKYm0Cil1%2FPsq%2FvMaMv7zAKgN7WRzHpqErlN7MoRsli%2FSYccAucaTQ51WTSvhSkiHZWWSXfiqAFskFfCHPoALXERmjGSEU0Ifakl5ywJ4d3AsGTQk%2Fs1xURh80RDU96AmALVUgqtfr1ocS8nrxdKpupXDhBM7cc46Rd7momb25kTQQ87chA%2FQj9TJezlW9h24zWrmYrAQ7Q540lLheK0IUxd%2FzdAoh2t1XgzDiNfo%2BkT6xXIvYP5i3UU6201fyBT%2F%2FRJLz%2BIySsQNYkU0ArVm2Q0KCfwjcgv7cVZ%2FmGRlDJf6vOXrkG4hvyDEB6omwVEr6kldw20U6%2FThQU269hKh%2FExXJZxlMQ2q7apqKY5kCcHBt1907o%2BfGKcCWhHaHvC0nv9PyAKScsaLmpmPf0Q94wG%2BQPcL1OPtrmg%2B4ED7s%2BQXds7n0%2BpHNHOHDc%2BOTMx1u4uOeb2hwGN2IaG4T0EBfrceysmK0%2BitLr8gTFqCOrkODEsJc1pUfLTvm17wR5eUSbiXgrHR%2F8FUKSAewaVFywByjxjCte%2BO0u62wD1fkC2EUheiJSxiwLTSlh4IOTLrd6BJ%2BvjJxomb1je%2BPOMN7ngSdWjuEhVKz6PkDN%2Fm9MIDv%2BdcGOqUB2Y9AZWvE%2FjbKpmcRi5JarcF7eO1iRU7UnBrHhbNOfdvj5MNFDQsse0IXRveMqLjGLlCfJeIBTmiCP%2BD5IuLCW%2BtCZM05l9PVuJT9ZyxQKlCwbV2t6B7uS6nPzEs25LxIVfmjiu77l4ysyAyyg9lwoDBjxtFBPslAiV9wEvDP%2B9fe3szw3wd7tK5Uy0MWtemu9iMPn9Q%2F9bd4DHrbYV3BCXtlC77H&X-Amz-Signature=149de333de412eb1d5339725d6bc99fb0c8af927b29aaeff7f7562e02f0a0900&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
