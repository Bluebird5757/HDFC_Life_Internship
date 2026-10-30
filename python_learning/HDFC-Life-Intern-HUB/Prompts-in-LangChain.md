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
fetched_at: '2026-10-30T03:29:26.340Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662SSIAJBP%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032920Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJGMEQCIBT%2Bvva6VGYwoTKfX7Cabr2bZ2wk4jOtLxjWhyRiSlcfAiBul8Wj3vxyGcadKu3sB6BDBjxVfEJ0JDN9mpaAgG1nBCr%2FAwgsEAAaDDYzNzQyMzE4MzgwNSIM8DU%2BKi2TqrP2%2F0e%2BKtwDUFhjOI9UUcwgb3reve68h6fyUfHyttwEoBIEhwsbVSl3hkILQ6DdvP5QBdJoeI9brvlWjrIhr7I8%2FD4evlDD6DsPKBfkiCVIqR851FeJZ%2BWFJUCSdggbQz2Xcbty%2FPBKRjK0YFhEoJH7UgMoeoNJ6kK6%2BFUrwCBDDczKQdlRA7s2YQmCfJt25wRfUM305qqPp3ZaVRHmXDclBPEmTNrE6kOUGL6nNntcbxNjX40opiskqZmxf2ca3raC%2FedEqGDp4rFtJc1XdxQQuiXH1kdktzQb%2BoweW6rHVbEHwLwy1NXutPbGqqs%2FfiBi2Ut6FMkuKLpFl3G%2FcO9KkMIpvQGddr5AmfUETB%2F6uVgd%2Bmi3%2FeUHUNGEgG51SR5DIcfQsmSpLVY9XN9oeqzGBsSX%2BMWjwv5s1MplU0iLVzEUGHAzCxuyGY%2ButPFcxW3N1FiJCOKw3VWtH5uzFIBRQGohJ5TITgk9nAa0MzExrDeHFB2lFdQLWsvi3NBWS%2FwOsPkeMFyMkdrFPiBRItlJ5VrdTNAPV5cWb3qCGERelxi57tyvEl4ctUjEFT6lXtXVWh3KEQdadk%2FQH9y5ZtjonFdtErhSme3hZbHE%2F3Nc%2BUaX%2BXvgRDajet2Ws%2BvEJj%2Fg8DAwz5yQ1wY6pgHGP1GWk6V%2F58Yen83OGL%2BDvROSz3a46SHmSq%2BBK9bRLSIa8OxXN6AZJCkPJnvCVWIzEVrWeyRo5WFhQCcwUYybjITui8cs2s5FyPFzR%2FoBXNtshScgscc3pGNbFRtPMfIjB0oeY98RlB6%2F0WU186j%2F9ILA9O4pDBJYvxdxLgpPW6twLDX6P3BdQWb9f5i34aH8c7vZZ%2Bwiny4dxFZeICstV3m1ove2&X-Amz-Signature=7b288fc3c2af686217c1f6b7f31cd40ed0f9448306485d0d98b1d4655fd60a43&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
