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
fetched_at: '2026-10-15T02:19:51.581Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667P7XMRFJ%2F20261015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261015T021948Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJFMEMCIHRhtJjB4FJQVqfV5ZbLWLSCIZGEDG99WZRMkPSpGG32Ah9CFsHy3S5xBK4fglMorrsLyXAtuU%2FgtC831kB4wfijKogECMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyvATgqGAfWJLf5i3cq3AO6YhOGLNjsEZFWyhZwQhnUS3Y8QwgmW37u3CXN%2FpbanXyDwZCvzhZQCvkFgHDsUq%2BXQEVWbAV5EMlKwL4nu9mykDPX3cFyLcwl%2FUFFfbiwEI7QZQc%2FFX5pdw%2Bd%2FDoayyeNsRAmrTggFBzMIptaw2rpMR1fwtAa4PGswXHrhGh9Vx7pRWYBHtes1em7tXQ8f0ms2M9c5OSe0W6AZVaUrifv8aYVu0afwvdHwjKxzvmv1FXiV2yEzhapacpgjsiRu%2BZVWAqpDxiCb5BDpaXXntyCeHAYD86A4ky6JBwUGdxc%2F%2BZxgSxLio%2F4J8C%2B7CPF9D6eQoBE90pLWVc6x8%2FCQA5j9fGru1rg5qyCKCQoz10sugFLHcAqv6v7DsR1neSyO%2F4tNTxsh79aNl%2BAl67eywTxnmEBo4xoBBCZUVbETUilWEBwGdlSMrJVqhQiJ7LudpT58IFCrL5x7Z%2Byy2KMaSAmiRBKPxqnU95Vs%2FF7LGJAoUm9TFsedvreUSeV3dmY6QzPM4%2FC%2BhW%2FLpoPEO4efct%2B7D3PjRVp0e2U6EVbuEN%2BD%2Bk5SWAFhVB6NhDAFiLIzNgN8Drndg%2FXgIvuv7%2F1UUh2yJBRoFKt5m79NDYSMAek%2BHoKf%2FrjVqAdU64p2TCbvMDWBjqnAa0YPtoYdkBm8tb9U5Op2I8CJJ9BZ4M7oIBbLglsCXdYiTjgdBbEqkwrwPUbeM8eoZtx12Ue96VCjthb%2FbCSY9VX8MoPDg0nSIOZzpaj3dj9xidYLTHy58DS53zycFP1lyVPq1XFKhcTMJlxbo0TMXr43DdRsEgB6Z6JQoPstLgqbKdh6ETVwMKjURQmOEab6klABgIqIbSX8dr9qfStm0Dbt%2Fop4L2H&X-Amz-Signature=6eb82138e27037cf51e1e5cce5d1949bfddb402544006e3b286dbeb87d5d07b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
