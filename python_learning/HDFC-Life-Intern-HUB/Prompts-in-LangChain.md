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
fetched_at: '2026-10-03T02:52:51.458Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RNP7HMGQ%2F20261003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261003T025246Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC6Ate27HrBxsvVCF8g04uvEcg2DUHJ5QPdOa%2FBduTExAIhALqH8Yq%2Bx9FFC9fnTx1J7EZ2f9mODgIXOkfFTGN57rVgKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxW6Td3g1OtJ%2F1yHZMq3APCc%2FfhO1DrLkWPEZL5%2F3UlCZA9E5h5R4koVV1TzWLe4O20SxNy6%2F6Jz2444vmfUFKemSRzFbvQCHFuIwWKoNs%2BP26IMo7R698qwp2TxHYA%2F0m3KBtTl6EQqRbqQUs40LsD1WEZG4%2FVC4RA5ROQ3Fdxy6VN38y4BraU4YZZdc56B39rscBesOAcS2fBl%2F%2FV8ViRq8s7Zx3F1AVorzFy1QSb4qSu3Q%2Fw0EzJZPyzZCtosUCQLUUdjO53xlBqoOvMoybIMEcV40YDsC%2BFvztLeGYiqfkV%2BxUtIbbJkm4JJXts8OqEATJZTtywcm59qLvi0z3s%2B2D7cl5kByZQAWpplSNt5Z%2BRRfhJQRt2mm2m%2B30LBP5J5QsnbB7Na20iJtiDmXCz2%2Fw8gpXU7tCOz0IRaUzaSYouFkNs%2FQwEiH3cbcS8aS%2B5f5BFXk09s0AuQUcWt1IpX3IfeH5aIuLg5a%2BflbNYoQIjbK40melxXJ9YaNyfYDkO%2F%2FeYfVM0mPcUzGX2ImHjmOK1G0pXxOZN5wHIGh6HkDqhET4%2B8%2BFNU5bCbZMU4ql7seH2TefRTJutpkoguzvWTyYxMPhbTK2REuL08kQHy%2Fn0jXQ3tbFovPHIb%2FagA6epc5MdmDkkWWMk0TDLz4HWBjqkAbD4DFIvu4ra0BEVtN2VLzO%2FuLIOlIeP6XE2bfBT0L%2BmaCLNgpp6aUjhJI6G8E6HwFPrsddvU486jNOTKn0DdSmSy3pB0QKA3fGKoxXJYik7%2BKbTduvOkW5Jx2H6Bvqepr%2F2C90AVryhro7TAhkaZe0%2BEViIBktSvg68oA206clSlxPYjQpcp2tt7iBxSLyX1XKOvRm1n5Y3%2FMev%2BgA3wT%2BNbHM7&X-Amz-Signature=4c69b56a12c62575bd7c8c859b9e350671ec32026f14e7130ff2d6955f630202&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
