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
fetched_at: '2026-09-24T02:11:03.358Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMAJWHNW%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIQCa7T8Eh8Utimc5ZoxZSf7gP7MlDhf6nnUkBh92iZpm%2FwIgJXWMXjQWOYuItkUs2LL2mVIKiIqBPtGaLv8Vf2%2FcbbIqiAQIyf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBbKDZUxuZPLNeX8pSrcAz0SpVLbslYZxiGkhsKBqNdsQkcqzIKu%2Bfjnblsw%2B8I28QsO8sor8zQP48hbZ15s5vnMKEPtUzriTRRTEDXXJP6vfEs%2FEeGTVRaYLkyHxZ5qcmO8meTV%2BUFbsNNzu5eObB8j4NcyxAZ8mo8jJzKwGkSl1doDWCoMHVkVYktW9FzxuLlqg79WXMNPp%2Fx%2FJ28Y1YHin6qiA7vM2o8SJTzzgEqY%2F8oPozxchX1Vx0KQAu65B4AxsVmNwftlbGj0yjWlM2NVtovNuj8ShRz2%2F0haRDdKT1%2FT3ko%2FP7LFqisEvnBAfvPfOrJgq6Yd54G7pNwC4gDJO%2BjFCLbuyFgyEffJeZpQGHbr8hEZH9J631V2nSCuJfGRfhtrwt%2FVZbFUImyeaV%2Bhv%2FKmsYKWI3MEMxa5NqxusDkeqO1YqeutuOuS6dHEMZCfzfpbT1GWfJPpyr7rN7xH%2B2t0Fax2dsAKd98hbxF96UcPIV%2Bm6waM3cKzOxPOuftvDPkz1PnVi%2F8KWRsmM0qFk4L9u4wKJSE%2Fm9v2F6Cq3Hm5ev2ONpq8oPyTwUUdCUD2jNMjK55qQwrgK%2BIoVwCyTtAZqrj%2BTbTwIRHh%2Ba6oMX7RtCUhoZClkmh%2FjDROHeRQUFkuvxxsqR4bMKjZ0dUGOqUBM71hXz74FSh2n0ye%2FUP5FkG22JSLltuyEvVKbPvWwysoFfOaI2uvA2hE%2FjwWHlbx32SMQCR6RFjPwSL2Edz6PuK4X685Oaqjcotyq2G%2Bxz0sejtpkMZ0cogk6sXXAVTlZE%2B2LE9AEooBzgnnhRJOmuvH8YyRgP%2FeNXPU22vhsLlHtiT870UBr%2F9ZpspDESPZ2SZbxyVWA82U1CGtc69ccuYh%2FmKx&X-Amz-Signature=06950c1a9be71eef71244fe665a24d00d70137882e9e8fd9c3d1838e4456edb7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
