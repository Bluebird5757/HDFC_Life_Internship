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
fetched_at: '2026-10-20T02:35:18.946Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677VQ6A75%2F20261020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261020T023514Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQCb7J3hv8K0duTNH3%2BShBLoqxUQ3wlZyhnC4RK9%2BY5abQIhAKBpsQgE6y3XleKnJpvBfJbaVJ6jc%2FgdO0Qc2gQ%2BR26xKv8DCDoQABoMNjM3NDIzMTgzODA1Igwuf5WreG4pfz%2FRFYYq3AN21Yz%2BkiPW%2BD57ZMLUbZvhzQ1A%2FPoreVaCrz1pGBpo1TKiqZdlDFmxhSNFUS%2F3IbvANLGowHuU1RHNPbooLPO8sexoDGHh3rhI791S0gnEa1%2FIPZ2mAfNChUoyiHLIMxzKHiNC%2FcON3JYLqyz7jqpgEhhdazFvLgGbh1dEKm1qBqrEk8Q5fMPMVZK3hBWZeAJPQIs7PGsGHUa%2BvV1jeMyU5%2FlEMQbs6cK8BbMAP2ZH2n0SgXeJtwgkYhgEROLKUm2GcEYZTZaXfivC549yGy8V42zFf9eH%2FkoZkLj6wgE7tUkgo%2BZ3hCD3HBvEG06B2pTQhboGjK1If2yz88vDAnNB28%2BcttoygJ3JVtRo53eS6jHEVGiU8qPWL5cslPZfhgcHQS9wxqMVJPndxxyUnBkWGTOlK%2Fkjpd8ia5Q3HmJNunS4QQu953%2FI8xDxMBFEJBfOZnfaRYW7%2FeVkq4dJ2qv%2BXZKpJY5dp05oc8CV%2FjhynsAUFazHDesW2CeCYCdUjTL%2B8l%2F6TmCjw2Iws%2BTtR3sXvs0gLux6dhYW%2BRQP5m%2BYCZ0pZf4%2FOEsErgNIZBqK9bbrW%2FLAotTrHfWDq6biWlXUDJx8wbihNoZhB8WfrViCXxZctU9cLBWVl0A79jCLgNvWBjqkATVAwIUH0fgBBoZq0L23GbVUljbRBpUiPmqsNwbO7jype2Ic5S%2BoBlVOEQlQlLZjWhnN0pbD%2FkwHdrbqBxMBOgG5HhbFpTauNVVRvfxMU3BqE9JD7P3%2BcTWXxGm8XCXxerWKvQIskzZYb6r5DAeY3QphNxrw3wE0t4Y1D1FwMxQP8g998hpy%2B03x4U%2Fd8pbWGaIjA1S6%2FkrYIdOFb7t%2FNstRl2m8&X-Amz-Signature=7ace933269f04e9bc6ff717beccd6dbebbe890a3732bd453cea25c921a917ad0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
