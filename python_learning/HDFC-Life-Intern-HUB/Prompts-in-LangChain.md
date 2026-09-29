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
fetched_at: '2026-09-29T03:15:09.244Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSHWG42U%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031500Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQC53yoAsvdH%2Fy8nfM5RbJE0tcvlO7Vuxq5QMeETIMPk4wIhAIvQ0Qv%2BQFpYB03V%2BUG3I1%2BpEl9PAqbx1tdG%2BXOp2FxaKv8DCEQQABoMNjM3NDIzMTgzODA1IgzXXHucPN8uixIMwIQq3AP7eK4NypjeGwyz9VLCmWIEp7ZSI7zojyUWJWQfMMoLmVGvUKyrR29%2B3wcrkt4HpXFBnFHkE9m0MQb9gahJHyHUvqFIps6oO7Y%2Fw8cmLajNQIWLeDNR%2FTzuhLgCkoETTm%2B7qtU3sNMt89IHn5jEYl4M1BCdlUXJzurnzk0rWYoca%2BrV2%2BrxVz1msP95mhxb6OgMErIUeZWwtO8P6Qy0mgEwbOpJ%2F4WZLHDA0OHd%2BnwnGzGcuXiD4Z%2Bh6Q6FSIA%2BrYuMd%2BN%2BpNSb8xZWnpjVHIMWbhITvBiOvNyKISML54zgh1U11nl1EswfS2X5rwlRU3dhEZDmULgYcoKoUZ4ZcHv%2B0MBZdCcc%2FOyDzY%2Fon7WCTUfc2L0sBh9F29qlgcKwtYGL9KMkYGDsF1ktvU1KgbSUgrzs2M1%2BQ8KARTQhE6akIngOpp%2Boe%2BNfC8adlQ7rpZQuPbVARvD7AvPJ3DEeN9cATyLfJDL5mHG9Oxj5SeGHtH1%2FlzZQiHnY2%2BjbNfTixcB0nQFAqiZmbeMNOxN%2Fru0vVuJa67zcIF3BLpOyBMo7Vcb%2F2uN3bae5Rd2mjJGa77v41DLoeRIT9RYHYcxRr8VmmaHDxmawSaH1wpkIRPdN0D%2F%2FFyXDHLM3F5u8OzCJyOzVBjqkAVF58t1rmtnYHxWZ1TdgY0qXZVPCQwOgWF4K1kIS%2Fa6fEDqD4zem4Ul5HHXtPZMVmMwz9tfVUfiLFa6jdh7k4BRKdjgHIBeCqLozzfUVMicSZwNVdMGJJC7pTrvrc6AaaRsqUIdeBqU2kBeUSbzRahHgc3XyWUVKZDlKqMhTS0D2oqLf6yFI2nPcO5rduXg4oP8c7atQjfgnhCVX%2B%2FCF0fi5dC4R&X-Amz-Signature=f57d62fc7bbdfe94f36c3962822c9cbbebf630d2d89adc62f7599f9c4d12fa5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
