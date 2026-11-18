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
fetched_at: '2026-11-18T05:05:49.659Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CWL4DCB%2F20261118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261118T050543Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJGMEQCIGuzKdpVx0FXzdpND5DZ7NvOONEb7uMONmFrSZ5v7cwCAiBdgqvBHKxcElvVB3mi87qIQjEfFU9PGfDP16U%2B51ux9iqIBAj2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlani%2FlXxVRcSdZWwKtwDaroDsHLAsueuDM3VvDNirzWZPjZtzX8xjaRj9Kqg4VK0ZwR8gu5eGRYyHGng9nY3%2Fnqz2jKU0b%2BxDeLnKro191bMl66JsKizHfebzpG9sx1KA9lY%2FHjN2sPpmJ%2FqQ55Kw2ynEeY%2FK14KhM6Syqd8iJjEkE%2FF2Iw3brz6V83Y%2FjR6R3FVKlWAgfslHqny7noiTdYL7d4DVbS4DQDb0dRH2DpWc0GAvW51L9D5PUwGJRq7OlaN3U4koOVCm8ZRVdo0b5B8zRA7wuaJ8vPOnDjYpiPqeVKzpzVAvayd0jhj8w%2FLRXVGvN0D2U33kFuSVgSbVcSO%2B1r99yYfB8jjOPvY1aXXdCe4ozANbTJruu8WuAzDhKNbJKBNwgRwoRkyCUr7JG9tNiC05idom%2Bw%2FhYGWckQjfczSgexuy8RounPIHz%2FdlSkyLq1WPo%2Bcpfp3YndqGrjqWX5p%2BY8H0UciaoZhpGRbWD98tF9uGnInNE40NWegRKkHQhTaCECZRZUS50yFec4%2B4aMyV1acUNwmgJPzYArAYwyeV2eMo7TsRH0agWWL2JReJfJRq2YhMYu5Nyrle%2FUnOvGMCAG1epx%2FFV%2BewJGGVZGgTSvT8cW1Y9h22kOIwP73cvcrNR9tfSAwpN301wY6pgGwBDzVFQu%2B78r9CQwZ5E0MG8f56V5tFAXUw2E5hhyv4GrCehtnOm4%2Fkjt%2FCFGbiHZmM0uDS9EjYdtXtVd3CfUCiyM95ikwRrdgFWcu3skP78R03se%2FmhTvcugHIXPk%2B%2BoIz7JPr820asfUVC2ejqKALeOLpF0wbrGPGOCSWzF9sMDfM3mJXN5k2RZ2LihqxdisdwwWOGV35IctxlZo3qe7m1M1%2B42q&X-Amz-Signature=6d9d0d235e0c3717e955f4339f9e5bf835176e012d847c001ee80dd1ede4a162&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
