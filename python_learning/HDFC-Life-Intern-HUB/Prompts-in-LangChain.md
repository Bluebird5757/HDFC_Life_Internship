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
fetched_at: '2026-08-26T00:40:27.612Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SWICGHX2%2F20260826%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260826T004025Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIBgjMVe7wtx%2Biy%2Bgo%2BaTVlPBHVEbbmXa4LBnVJBLJds%2BAiEA3spJQKiT6FCRyRBfwkb1h%2Fz%2BA8ct4i3LIQrj5%2Fctxm4q%2FwMIEBAAGgw2Mzc0MjMxODM4MDUiDLlKOXAkMRH5SmGcOyrcA4xGe4Vwwmrg53P%2FgpkjXJ9NLoIInLMMlX17d9yNPwKTINuygcvsL9fp0x4GUIBvJTBrB8cxRor7BktrRvF6h99z3b5HEuezwTIvWCziFtxptTIMHvEqTG70kPEsCLZJ7C4skBS%2FSZK%2FI5f2xpXBhPKfrF13PAXoYYPtrOx0L8anVJjW7S8cwpDnhT4m9WSacEnsYH9Y0kC4NK1sMsEc3SNe7vEsrNUfVhtU5FsxDjNeekxz0uUGWDAkQSH6Apb6hnRlQKaMo4jyIPCV%2FX4%2FJrWYrv8EEI7RowIYfVx%2F2iHumDSN0P6%2BhR%2BhTrg9xEXDYbXhx%2Bl%2FghnWEUhZDyu8YBylDmyZ5lrQ6%2FFv7o06fXCVOGohGFK8A%2FfIubmYHli%2BrUI9ZHSyLpyky9HBlbIc1EW0zVl05zRPNrHFP6QelGQYoUJQ55zOzwilOOfpj0O2apEGJ3w4TwHkyQZcgwNkFCtkBZcUTTVFGImCt2TGvmsVIPmEX1tRlIOySURcWt6P1dI%2FVdVvGruajmn3J4xVwkLtB%2BoPNLq4fK0lQ3lLIJ6Xu3rByMr%2BxqopErHLy0KLHvYi%2FJR4tHQdnEi%2FiPFQT16m4cYsCjFW0EETT8SPlVsoQ8mSXd7KlbKN4GDvMLC5uNQGOqUBZLWjoayimJ7jNzyTfyLI8w2SdKmXCHyJmij34mUDNu9TieZTeIE8WFXMR5ILIUdpik%2FFgU0SQqA82WRUOOlLxAMlEsxmmB16Fl6SBUTTnGekk5BFg9%2FRHhrQJ7w%2BW%2BMTdJ93M5kwGdK3UKvx3I70kZCUuufBcjH5ZA1SS5oUHmyljISYpcQp8%2B%2F5Qg%2Bu8IluYutyavPJDh6be8q2c9%2BoaZJ8mcfz&X-Amz-Signature=371896f7c28bb209a00334b374d11c2d6b42d70164b85ae0980ddcaeb72caa05&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
