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
fetched_at: '2026-09-13T02:01:37.906Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XOXOLYF5%2F20260913%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260913T020131Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDC5EfSito9liA0ttfffBKaHrag4ZaqmjmW0rgRM67EjQIgLbd%2BfzbmU4squdA1MH56wee%2FFGK9xKaZ6vQMuwQ4enIqiAQIw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPzrkWsd7iPfok4oiCrcA4ulGbjoHJhTb7quKi%2F0WS1c7a%2F2Vs18OlndA4WOLeB9Z%2FXmXM%2B4tI2WExkA84QK7rpvYFqqE%2B%2F%2BfTHoE4WqE9xRmcOP0cxwcHmFm3nkwnC4GAFVQN7JiVEnTq6cgWG%2FQyKouYhHgTqh9jhVB2UePfFxTGoL8UAV7oZkBIHI2ciyExWPIs3kAASYy1jGW1ryNTxRd%2Bc7kD5pQHawMwAClkLoIGfRza6XimTfDDAW48jebCp7W8fA8eYtqB0BYhb7FvUWlyHS6VXpbVSlesytucm4J1jZ2255eHw%2F939swKaw0aOeZJtu3BhQgSkExpNHszwwb%2F9MnrrJbNP0GeX7ojp8P0eBXinj6IeqZow6vrjTbz%2BqbqTj%2FFL9wQgDNeiREH4H%2BrJvdYIN2vThzsVJt7wRlveKYDp6lUWcIyvyw9l5Pq6lLimEOsB6Xr5mlQnGda3lg7RlxasZIpsKcKcuppyVthA9XSOAw18QTHKlUxJVWOMafGEBXNY%2Bn9WfXSmykVW8eEPsMnR1bC%2BtqGKjhauLVe%2BkP9nljUcf7f26gBZzIjUis3Xeb%2BJmdI8WtQF19RAOjHuJ5hjKM8uWv7wqUi3oJb8E7pMy%2F%2F241zGyhpO6FRo4u6C%2Fp56ZSZo%2FMNL%2Bl9UGOqUBUuZax25cL5kCxKiVIywjV5WlSnwqApEuBZpp2i2NDfYSMOes2bQaoCY%2BLYeahwKR1BVp1jaTvl5UtPQuSSuloNs4VWPgaxL4l2H5QMgXKKyzWu5nGQqw2K7D7FcjnTExIfUXJjxK%2BHy8jWvr4hgRsC0NbgoLiAB4S4hQrNkZoO5eOzq%2B%2FgJGCddRML6oJQ0c51rHGddbziE2Ct7QKYUNXK8DO%2FyB&X-Amz-Signature=365199465c404965e1624270b0184d8b25a33c299c3ee07afc18b65885c8d85b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
