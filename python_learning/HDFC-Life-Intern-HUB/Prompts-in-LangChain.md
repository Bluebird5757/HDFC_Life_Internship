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
fetched_at: '2026-10-06T03:48:46.960Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SBMVBOBQ%2F20261006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261006T034841Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJHMEUCIQClf4OKwbq2LWJQbVBhyei7OkmxRBFQGAyzuCGDAGHbzgIgfJHEb0Uu4KXsg7UBYgaovr5rlKceVzz20N7NCDzmQ4cqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI%2B%2BYp6HW4lAVLGTUircA540LO5nSPJ1w%2BKkSsvPqd2O9sit2vd0e2XMJ0nQEpF%2B2VBkX%2FVi5slvx4TaH9ImUSWPE00qBo79xXz%2F8v8kXAUs63ntXztlYs8dM9E6Pww9f6kyEqu8RqAYb%2BWoFFXokSvdQX7SArayvgmtqowJJjYu34bTE2QobHWuFKUPJB7noS2Du5vx%2BzLx71hh8qCpSQavaxxoqQjvg7Ylh9BjLjFE2zQGqL82oATNqKRGGrws8dkYOBPgFYlazEJEASXMo0S9AyoPl4%2B2VdpETTpS9V4IT%2BHSDifxR2wV8sD6TmG48S6uSdPr0zJGe1sCRXcsl5GtHNWWtzS26yg4bkgZ6NG04VyCvIxWJdvauyDPu1YGn5QibdjPmVzow0Ia8xOprNyvEdIwRe5yHZVjKbTeNhG%2BCu980j9hXeyjJkBNSq3buCZHqxTEkhkjehPXozHTP3NngTchPeHU4ewb03DOO%2BSGfb%2BBHqozqeECr6pict7XO1ZaZaZ6WyLcCIQxXkyrJxk4VCbG8%2BMArCI5dBIHyJYaI4gC3Yvk4fIo%2FKVrLJlGj2SepaLjM2cmho%2BpWFxWogCQLUTdx0KiWAo8tw46ZU4Gf%2Fre9yB0%2FaHbJp0bV%2BESJHAALjyMBs2RSwAtMKTZkdYGOqUBxJQYdRhTyPQeLTEny6CXEPsPyt3xCXW689PDyAcGNyCzbWuqEITktDytEAFFIPW71CYTmAVp%2BbxlGsxDDAMf%2FEa1Zzsp4BMxMbUBRGfUkS1DSRKrMclKrHX09j79hIOsyqPYCiyhnKr5Qe23OKX1ELxIf4F3y5ga5dzR7s0x417xnEzV4RNE7fsKko0eDce2SIGuhXpQZua5CJFwvAHCeVsIJHcz&X-Amz-Signature=eb9746d3218f52061459618ac6a0d424d1c3d736227b28206abb7a7dfe65bed4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
