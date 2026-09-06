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
fetched_at: '2026-09-06T01:53:03.325Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632AIMI5T%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015258Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIQDcjHGDCx1O4WWwtwk%2B1f8Hd7zPF%2F7Lra%2FgrYw2zEFhKAIgeTFKJseCQZ2Qa7RF5gRUS3qYYOeaVsnAKOTJ2t8uwdwq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDLKb%2BfpjbzTHaZ03VCrcA0ryKH0B4RzDrg2YPBnCQ1fkX6rL2qG3h6JBjYsPJiEjV3%2FQCv8rRe81LVnCXlz%2FRo%2BU6TkCks5nYkIXCmgYXpX0jLlmvsomyzFAH%2Bz1KPhRDbUuzmrNOi0VSkUtyeIk%2Bztd7J6vc0vhQVty0VoZ4XCDKW0RQ8tiCxQ0yGf8ZLZVokSttJ48jipqd7hzaN8CnWmo%2Fls2Cm2O21fJTtgTGXwlvGJGCbZ7zm5zuQJ8CpzmNEzV5kwTHAeY9oUrKaYfFmpIXFedIA%2BzTlNoj1xXZ3crPaamUoaFytYU9P57OiQ%2FGxCWPuHCO1%2Bt5NQBD5oLbNhBWt1V3pwddaexqUQTa1Z1Vm8foTNnlpZv%2FDShqFsH%2BD83aNSLTiO9FNsh7MKNMCmPAgbPQ0aXwbmdxjCqoEcvlr5pweqFHqUP%2BcCyguCm%2BPbGH2v3EgCgtoeq1IQF6V3cebZROxONeDwv2jH%2BdWjxEA1qAdpvznXdFk5MOfDkhcxx6o3mYxPkbpozuJK5%2B%2FKW78rjgqdgNZUpokn0UJMnpjmd53hwCHKGY13PGsETx0a7S40ClG1nc47XgFeFeYmH7wSQHqQPyD%2Bp8uW2GfPvhpqENb5ibMCoKgybbuRC8UfLhCnFu%2FYPRfS4MOTp8tQGOqUBtz3adeOE64W7tTMTqhGlU7t%2FKUTOgWA0p4rulLMSI17I10iSP5qtDMPs5MQavliBxrHBYSGEbnNGYsLLaL7zcLVOHLiVAKxaiRdNW9pzZLEaSdz2JHLXi5ajxc%2FHzRVzoEIcU5Cl6CebxqcpUgHwS8nlZ%2BQppIiRaDde%2FWFP5Kj5O6fzkJ8972yvQ6bLfNct5qgRnkcevqZRkmTytg5q9vpc5MOZ&X-Amz-Signature=b79f8a36388b5b8421e1b43e9a13d68344f5bf3ce0ed5e6f6baf96c83b88dfb2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
