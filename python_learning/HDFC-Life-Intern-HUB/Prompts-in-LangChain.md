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
fetched_at: '2026-11-09T03:38:48.026Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLD2XTUH%2F20261109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261109T033845Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJGMEQCIEp7frjnP4n9p4I%2BKPQUITdYUBNnFIXsoIEzuZGX%2BiFSAiBJovxIE4z1Vo%2FmaY28DAKfbLEVxaxbQnzniMQtSK3iqCr%2FAwgcEAAaDDYzNzQyMzE4MzgwNSIMyd13bNtCUVo%2Fh7gyKtwDj%2BrR6JF20cKuPBtSxSXBQ%2BS5mWBI6pMZICRXE%2Bk2E7QtvpTcUl3fNIGdCYbSPG2rtqSpHGVFA0G%2FHH97H%2BA9vsZIwJJqyx3V5rx4p9tJjv75Eg%2Fbcui%2Fp1XS4ypEu3Lcqz%2FK44kE3Uw7MhPxdU5yH6wGdip7LNePOtv%2Bt0%2BVYHTtWup4xmJh7Cz0dUGf8bKTbHcgpcD3vqUkx40T4UngWV1RPNf1DXqerdisqY%2BhASnmHefMW3v9JQoGhMd93XlnaBXcvLOeiUYLHUnXct3eoB9OJHC5TpBctzF4f10J8ruk6W0hUG40xwBQWfaLBbh%2FKq3c5hew1tConbLm0kEY3422q1iO4CIeiJmEZynMTueERsBYJPcqaX2IP9O75CsHNiY4LwZq85rhk4cRxJ%2BAK7zuUJ%2B%2FjhuaMhHUiMPxGoHeMEEzpwPLunYnK5ECtiFGtnvoYt%2F65QuXqa%2FFg25IFF8CX9UMz2Zn4Nrx9LaEknLlaao8WpKybZtxJ9yYnovhv0575wuKJdRrIGgbPFIcOO4NhSHNFl4gQBbAs3I6l5saNk7tGw5S%2FuvLOI5iR7iAO5DeRCxcneR0gPBfE2tUMSY%2FrC4gt%2BpJSqru0dwkljdHlUo%2BT8vt7TveqXcwnvbE1wY6pgGE6gCLg0rjtF%2FtXE%2FwhDP%2FvXV4t0A5K9QHFa%2FbXYk9VAdcyhUbPeKrHw60U4vbb3bAiEayiN6x74%2F5A%2B6btFGxaKUehvkiSZHrm04sOu2tgphNwi1RftFIXGudVnxaFff7zoSxILs2VQTN1PvJwO22xd2FqmW3tsTUSqOJEAFPGMgZyoiaKAsBtUimHbe084hj5Kuq6M8yFU8d%2FN2PhBFIPbimayxM&X-Amz-Signature=91985029893e3e77860a77b4db1c67301567ae7714d74e4f83bc4dcd59a326e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
