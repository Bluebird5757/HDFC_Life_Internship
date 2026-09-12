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
fetched_at: '2026-09-12T02:06:38.365Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46644UYIL67%2F20260912%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260912T020629Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEqrWF8XtruiKizJhLHAMmHmenyZabv%2FOsQe8AbXBQOYAiEA%2F3sIKPWPLfM%2FJ2PpaLO4%2Fw4K%2Bll8ahENvcCp%2BJl3efwqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEuHYURrgUycZrFNHyrcA1Qfq7rMwUSNvJ6TR%2F4AL2QK2jq93OdK%2Bs%2FhLBEqcZpFbS9Sn%2FhumZeekpMmQioleMxt2khT9ul3f9%2BiR%2B0R8JQEWrcM8%2FdSPYwZkKJY%2FpSUZzP6zt%2FpCoe5BcSwN0ntKuybHYCz3dQj9wiKzasZWrex2EzTZNzYBAwOoZ3AMYSw2Vo%2Bf%2FkYnI9%2BbKIirXE4iSe9Q1IIfL3ZFK%2BG4KVLjc83T8dU7I8Riscmyik%2FHdlkKMAuoVaQii6trdFoG230JKJMVQw2%2B0ndtTlWd6Ae7G3gCxogCR8Gl3WW2w27ffqH62rNkteTo7f7UNlkthCKjAzCjqjRqHSepuyv1rX8QzOPocINJOdPJzKrwlcbZbxx%2B3Gp3apMlEwXZIiO0RSOViCdiT9pKyRPZqDW8f15sdidpphi4vpqOPcFBbVOSZIqvTWk8m11SBdwvLRYNF6ABMRmI40mJruFWbwPf3tDRGLZbABq%2BI2sbF7w4ACTX0jodoNojeVRCnke6cf7zA2v6SIp3bASOK8ce8cjrltXM6DggGkE0pMZIdCHO0I5Se1mPNumwrxcNBcgva%2F2KEgyCWV85iikuB7RhNF8g8xzcOqn2gQcH02bnH0IPbt9o0SL9Rg8kTLrSXl4s4deMIrSktUGOqUBRsZFGydv7F5BYfdPZDVf%2F6Q%2FQMGWyBQriZb4689SYAa53hvmSHlg5sd8YtX14vMFFbABt3G5ZuYrPymhXStcrH6kNjp%2Bv3cGl%2F8FE2pLQfbekqkmy5gDgJsSgGtm5Hg00fwGD%2BxIBe6XwbHXX9DHT9KvAorArObxifYARBL0ZIwH4h4WILn4cxhXjVXEFWiCg0XwY5xZ9k1kSAbOzBvV1blK7HIG&X-Amz-Signature=9649efbb35d7fab5241685f983b5143b00a12498901e47dcdbbc781960e1ccf1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
