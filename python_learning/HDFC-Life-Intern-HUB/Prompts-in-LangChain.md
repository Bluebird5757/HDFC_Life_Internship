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
fetched_at: '2026-11-11T03:43:57.691Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZT4H2OV%2F20261111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261111T034353Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC2Kt1VcYqAr3yTq1zMnMo4qTCqBTsEPlCS0KyxKFfyogIgT3ORLhtnUZv09ZvdguqiD7GYZqIni49ko4EOHnlU7swq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDDCZ%2BW238ChHeJkB7SrcAyCvLvbKpTPoDCQ1NVb09W1iK9X3MAEXD%2Bdt%2FPVMhwKAhHIb8LwKr3rURkwSJhX4ipZFJp34caa2eF48Z8DzgT5SwGDDvGFEY1hi3wsJLm1ZXcXUpblFhKOwM4ngXxygmzrV2DeNx1bAogoh61IhnyCby3YZRT%2FBpd3wqnn32q6k913drlJDr%2FrV1wNhThAHCZqNZ%2FngROjloZKoMO%2ByFIPGEwhVXyUDk%2Fhw1NeAQGs1atg1GFDFrxpLP%2FsSiQCwC8%2FwFEIZttra60fWADyPtsk%2Bqt9oV4g%2BkdD7wAkJqlQuER5hGmHP7rYsOR5Ews1SIWVC559L7EKPfYMtShO7RDMiuzSn1YgO6imRCTqMo6PYIO5bhHEavCVJmbVg25q2eFVmJxYElLM5f%2FcP%2FBzaUpYrqigf2NLCG8dQOH0JvWbb2W6%2B9aMcyOdk22xE%2B7SVDD%2F%2Byy0o7XHo1CLzDl0jhGKNJb%2B%2BGPquDvqFhXkwxT%2F7tEd6r1h1IzW0%2BTQ8TUQfa1BX%2BASxJ7Y2sUOPpHtm9sf89VO6oOiaGSW8pFhcaFs%2FDW9y0nsGRedMfhsmwOX3kecxwMpF%2Fm9cQ2SjiVaf%2F2uEH%2Fl3X9tSvn16IBDjx9HHvXmGlMzqL1ImauroMMeAz9cGOqUB8Fg%2Fd633KofrdHKvYWYrZghhWMCf6FsxXSfCu8LzL0f3pQG%2BRE5aORhl2Ti7zmz449tMeY7IcDBSvxMoON8FHogwFZ5e5mA1DvmkFZYisaujzWoC9LU8NMbLsaH3WU1qJeODPnJCSid%2BvuN%2FLmQ8Qq05vgJaJDx4wj2TH5hcmGWUlUvJZlI%2FvNW3krGx6R4%2BxB5df6Psy3Avi5dDGw%2Fs4CPqOnnR&X-Amz-Signature=75df431da35f30d500987903854c81082cbd23568cfbb6f21561e14d32c16347&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
