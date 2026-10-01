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
fetched_at: '2026-10-01T03:03:44.365Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q7JJPPEB%2F20261001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261001T030337Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCTaxHAwMgU1w%2B6zaGcP1YZHgX7xfRgkIortQrnFQY7NgIhANSw3RmT9Nkt7d%2FYz0RS2YraYBZe8uEB4VTfkrrE3NFoKv8DCHMQABoMNjM3NDIzMTgzODA1IgymtMkMYX3zYDnSk58q3ANylDdtm4%2BCAALXlrHcoX%2F4FEUy7RfyONw4kqHujT0ryzYNA2QcPpjvyokMDgxiiRnfoR62dcRJ9tgvqSEHMHTd8CfrqWZ9YL%2BOx6vWebG1nC2Uri%2FnIJp%2FND8ktR0VbwXTuJpUTem5bOeUNIGdfJJC%2B9%2B5%2FRPK5%2B4WelEUrdSt0JaFTmWM4jE6xPNEOSeBgnWrTSwGgZsEKbSfwzjn5JXp%2Fg0WEQtaRLQNtE4gU5ag%2BsLB3Jc1sR4KS6ETrCNXWp3NTXD%2FAFvuBTShwFErWqlxa2BfNZi8D3DRnM%2Fs9EUzUEL8SdzqMS4zGZyeY7hDZ5iare8Gr2kw6BkArMmtyJV7zsP8E2ucV%2F9XMb3cuHVI%2FNEaBdvaJ6goJ9%2FB3Eunc7NJJjBfRgSFnW1CtR81cy9v%2FcIO52j6gJLQKjL5VDmn3hE8K3va%2BcASyoOdTDaipmppSBsg3c7trZtfZi0S%2FvMQgKKA4vQAiz2Hf0%2FEj%2B7qhpYtVkpyoa1pLvGsTrw8IG9jkDT25AC1GAjPRxSm0kCnsm640weYwXWoV9bhvvw5q2b2EYWtSPWTInlUpuOH2JHsKdvaqG%2BkQMIyfCt2tztHdah6kmwHGhaBjBd41oSeik2zgsNChzS6LD4lejDr8%2FbVBjqkAXyexc3jYsnzQjZpRljCF0gnWWXUahMDkb%2Bbly7U%2F41fo4ou9ghJAurPinYfoDFfQOgcGmPvKpnMwMujZjiEvCBEWEPQFleyVhb3k8Jd3B4Gy02%2BlmnKKhNo%2FXJxd0T8eW3tc9Pge4Hhq7TASLO6S58Vr3Wu4uaLhxrUZAA1bykmt7sx2UAMFj%2FYLAP105Z7pgt0J5MOeB1FvBKj00UHwjNVOHAG&X-Amz-Signature=d1ef39a6ebabcdc65f6eeaba3abeed43013c5298bc05a3cf601ed44b16f9195b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
