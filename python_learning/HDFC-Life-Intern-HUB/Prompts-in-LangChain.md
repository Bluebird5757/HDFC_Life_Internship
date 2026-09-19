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
fetched_at: '2026-09-19T02:12:19.472Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663NAIJW7O%2F20260919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260919T021213Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD4DyV95T95ICSpoyslKkI%2BOH5lK84sykFM4QPV0wiBqgIgK6qAI%2BH2dymWVsBF%2BA8HPGqbKUS%2Fjt5qQxe2M5Ytlf4q%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDKrxpbcJfznCYqA9XyrcA%2BFKDHs%2FPP6SclSlrwxPv85f8b4SyBRu4cIBvVb995zOu3cv4SD0cZAxkiwFgjzyYQCi9TkaqsOqoF0V4%2BvoGT9H6O%2B643iq0%2Fs0NG3p3c799Hc5jZPPtZweNW%2FCiDpVf56t8ZwPH5qHkqOtyhd1COxO6yJWRq6oG%2F80erOGqpf%2FPEvTUmKiwOMQlWz12e5yKbsJZHt%2FUQx%2F7Y8wIlV54jZCH2ej%2BQhXPs%2BsgxlHG7FXZphwTM%2FKbqTffO5wt9quYtbMAvDUvqsQI%2FUMHah6XiRme8oBGuM3bbU2JBFNjc2TjMN2yy%2BOJqj2kp%2BBloD7e%2BjptWOkMmMKlXvTHS6y37duL%2FH4b1QVTwvYcSCVhPTFJgPl7J%2FbQcZ%2BqbxB%2BT8KG8mOP4uSiTn9kelxir%2FOezQPpLw1p%2Fsaqnq4SGR51%2F120RRj%2FPZ4y7P3UlM63icQlf0UmmY7rt107eT5JcWL6CBTlxWa4wpekVCiK1Uk9qz3lEuQ2P34J833ahbtg2WPnj%2BwjNSTgfO6D6BosBAy2m2rJhs%2B4cadAgnBHLLHhtcLJY1IEnHAOm9MTaD7Dq4lZ2w4pqplJVXSf6pWGj03nb3nrSccbtZvkBEnjSOADgF%2F6AL4NLJ%2FW769ZwvFMJGwt9UGOqUBHOBLC066gDvegbz7gESFCryHZOZO1RCIj2oE6XHPY96hGnBRs925UxH1QyBKWfJc6BuW8%2BiJ0ZKD1FEZ0IYP0PKupeRxNqEfv%2FrPWaUf0rQ9N8hFQS4VCoU9WDGvCkhCmbkSMdGZTNx2SdbOhfKbjchMA6ldbbemAtQo1xSLL%2FnEU1V3MDAoQrVaM9unz3VY58zrPj1BGOjyAvPqtAfrl5OdGYde&X-Amz-Signature=18bd4442ca32b2c572d55bd87a1fed80dd2fc1949ddff74fb284c56090c52099&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
