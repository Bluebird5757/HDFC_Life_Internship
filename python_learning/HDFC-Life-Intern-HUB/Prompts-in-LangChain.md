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
fetched_at: '2026-10-13T04:30:10.946Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZVSNC3U%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043004Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDfgiZxJwHpQKaVvHqgdUnslUtfr5q8UgQQKYEl9z%2BhDAIhAMB7qELP9V1xFkssYyFsYJOEeu2lBijmC1x0EwzHxDBDKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwTTkgiw%2BTmzWKGWNsq3ANrjQE8jnDr9b0nX2SVGu7hgBohfDLuVGZB694SLyLZfZR0kMBkB5kxM2FsuE9AfEq1ePFyY8iiTZ%2BdgkuGP27VCuhkiku8fCMOu5XCFzcWQTiEzicDCAOGL1SyguC37tHc1hOVu%2BuhFtOce1dfKY3ibptref4LiIRcW7OoUr%2FhxmK4cbzaT7rwNiDdqobDQvsA%2F96JyPLoizrgZ8TTmxQWZwiA5tFK0wFDbWRzSapmbyjTGXLuzz%2F5kShCLHCYF3qrzJKv0hcJeWjL8J6hnS6QKpdx2JiumPW7Krka9c6RXRhSG46NLWxkbJvpVJQsxLrbpLooqmOncDo%2BAAW6FEg4VnHiw7MEXC%2FQyUvD1U9P9HWOvYG2kYQeTfvHyksxKPK55qqi8YtFFHCgfA2aqATWZYIAfpyieoau2S%2Fkg3o5NDZ%2B6eH64R6bNDIPVLmbB3d0a9A%2Fq49hSeJYFF4DWNtgXMdX1OuQFmxi%2BFOc6tSAaBX46vbGk%2BPYpES7t1EHkFR%2Fia3x3XZ380KNed6FBF%2F7wh2Dd%2FUA02rDeLyWlezN1nhG51b7KJrf70%2BGDXmiiJz%2BlYpqnolOwhfyXtAr9OPh8pSxFMV1N1acEcag9T2nRXHB90olGE5pv75k2TCH4LbWBjqkAUUJUjsPlUEYRaIDquxlCh0faOXH5K%2FNb2wO3oIuIN%2FWt1iihOS9kw83CqMCcOPL16sum5QGyFK3Nrd6YhGDiydGVbL4ytboCXEPEMWxKph8QwSJaHKe8m%2FShghhM0f%2Ft9KCEh%2BFJte3Iro%2FkDeswm%2Bkx0tMPa%2FwzNlXe5VzNinjigi06wF5%2F%2F1RXt%2FuLL8AQSG4QTr2IX5mX5bCxeDaHqzswwHs&X-Amz-Signature=b91a56d7455c7e7d364ed2b4525749509f0f6d515a56d874d1f194600f1f43b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
