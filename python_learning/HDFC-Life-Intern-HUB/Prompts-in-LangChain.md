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
fetched_at: '2026-10-14T02:40:02.431Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666JKKFPG5%2F20261014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261014T023958Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC1deYOaDZOreZOVaPIS%2FlCL13RSQz%2ByMsH2sGuR4pNogIhAOjyFhv%2Fl8RmpzeuKc4cNCoeTWmmIFwXqoIodL%2F9qcZDKogECKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzYgasTgMe0CVqykjQq3AP9mIvzRv38n8RP%2BS9%2Fl%2F%2BVfAJFAO1Nb6soo%2BWBkdg61TZ2gBNgeUGLz3N9hdxlgxgqlLYmQZB%2BwJI2xbltSstD2WhvnKxKblG8klz3q5NmtIrnNQYUUoM0ydfVtVz%2B1NfYM31KhXQ5VGkUGdOrx4Nmx2YbrttpWQsiJtcFTXO8CHlS9wLnDMuqcRt%2Bp0hw0ev9yg%2BqYvdBv0Id0Ehs724gO%2Bc546BmsbH%2B863fqi%2FD%2FbbqLMXnta9L6U9vqYdR8vAkxTeaAB7mixtKmHWaLvvud969ZE2%2Bgum1Q6db8twU39jlB%2BvU4A3zL4b%2F%2BXI7NZT%2ByhA1Nf8kjnYEfZpHdpzSAPOJDsu83HkxuPgxvVCjW3V%2FxG4mTTPw905TpC%2Fezgdxax9%2Be0yCuOYQqEFjrP0ehe4L7WYR2KMChq9MBOVWBnbZS%2FcdMRBabM5%2FCpnOjwGok4wZRAasvAL1QItJhyWscYNpKr%2BkhyKjayBPjImmSo4so7%2B7rOMSmfwT6rOkFRhqyLnP5vphjKO74e9T%2F12PycRx6uq0fUQwbozMSMC1T1EpgmHffQzcqU%2Fe%2BWCJJ9Gu8nj03%2FmY0%2B2Avw%2FSnmE5Eier0gkBwazwy8cdISmvbnpgTKRgogOCaK9QPDD2s7vWBjqkAa6NsUv3sbyrFlrV4OgmL4YjGzuBaUnM4pox0S92Spp1yqmJ0ewpWfOGHvrduOCQKfNf3SP%2FLVEo8s%2Fx%2FSMjKRk3Y%2B9DJybPBevh5Maoo32x61f4KIIN3y%2BCovhc9BT22Ijq6jyOwuPD1vWVbyvjHk9tScEhALh4cRMBteIoqk28CqaihETaVxOxRKl3%2F6Oh0KLUoryhqx6FaExgvvTbu5ZZLEiX&X-Amz-Signature=2b734e3e932b1ccf28bfcf8e2ccbe4fe3300aedc889383768b30f28111e16802&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
