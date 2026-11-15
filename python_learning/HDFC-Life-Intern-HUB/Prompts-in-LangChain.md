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
fetched_at: '2026-11-15T03:46:24.216Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZWUYVCGG%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034618Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCMsoXao1PTLqiOhTL7D5e4%2F47F51rY35LJsqEDWiKKfQIhAJ2gn9BYB9PbpdpMzER4FnohQs%2FWIS3OaPMHxGLj7pN0KogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgybnehOQriiCt%2BJt%2FYq3APdTEtuY22oTSciJMRE9A6RA5Lb54VlqvFa9CpKkbuhWbIVj21MlzQQ49odRtkc49xLBKWMOvQrSyMALdPtrjRrKsaui09erAw6Iv3mQpnGGCZat4IhoQeIsj3vhNSNSgBH1XtKJzBxi14uicBM86m4uzAQLl870UigsLLCB%2Fh99ATQMQbCfCwTh%2BtG7l9Dz%2BEJR1SjQtACjdn8tOcX0l4fJY1c%2B33FS%2B3ro27R5sju7uNjTBtqoy0YwmqK9YIGkPevtnshe0Hv9dA6G3138%2Bo21D7fvFFQZkUFNf%2BH6rZ2LHrLl2veAOVx0%2FPtvEnhJr6cUkFsSsioPAsoWdT85jOid05auvrAeeuM7no8Z2eRppmKcgjWODhzm56dywm1FLFiUblJ%2B2Id7xTJ8eyMLIdJCdNxmj1tt9iPqxGnWd1lcTJqtlrW3%2FTysY9C0qA6npeaOyfdRrKknlsyFVqElbkQTtYZlJqrAkB2mQR0SF%2Fmgqq6HqGRGpWdNTdyWYmqDwMYuJ1d4VhU2xtyIity827os6LKdEWlSYFj%2FSE%2BPrtDHdE4fYFzmMaaS7vJm9d%2FBfMG6R%2BfBF5Px7zg4kutLQ%2FpEeMKBNw%2B%2F3ycex1M2FnFtiTBLYwfgI3HWJcabzDu2ePXBjqkAb3sfDby26dtA%2FeKKa39qvGy%2F%2F65AR9QITourCfsdIiD115H0SSBwy%2Fp6uiw8O8DvynL6tT%2FkF63z9AQz5Vb7slcJYCo%2Fuvrrl%2F2O%2Fd157wN6PGVhSl7qFy2QvQyq4J78zZ51taUfBeXnpf69FtPUgfP%2FGjgd%2B%2F0cNmwgnczvTIWJ%2FgryTto2Uf3IgxbYj9SxiQfhMtL3nZF%2F33sNcFzdrpMYtcl&X-Amz-Signature=1422e46006d5ddf94c5222798ea2b0faa8478859f7c4dd58d91684cae3060cff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
