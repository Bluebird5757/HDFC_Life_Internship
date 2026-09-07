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
fetched_at: '2026-09-07T01:50:19.385Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XYSO47SX%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015013Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQDVpMN9uz%2F%2F%2BTvobXnHjuBj0tGvSLJ02rip%2FSdg9hDs5QIgfo7%2Brg7O6kzlevM72%2FhuuXqF%2FhLvjtNt%2BpDFz%2B21T30q%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDErJOT1TGVxUdgGgYyrcA8CrKepx0JnlAZhEnL2gID%2Ff1OIBLPQDLFhkLSrl5PR1vKn4N3u%2F6T2%2BkF2ruiDE15AKa08Rh5EjJUUWvPkGVA4RxaDZmyuMI1PQHsV9JuWzhmXpghwXiQtgYusKiBvAOOkGo9DsjJpnGLkTijoXohJ2xXDmDxAkdSpNId0O9V9qmuSQxYh1WJnzW4QRm8zJN%2Bl7JsU%2BEWesxcH811wS2h6D4xjfio82bLquQICMVTrXAEXn363jA8CTKq%2B5o1hiJGFlwzOr%2F%2Fiap%2Fs3Q0UxPOloP5QSLRzNxofcyQa%2BfhXRjTtlky7Mge7FA5CXKQWthnXvkN6%2Bdz0c7z0K2E9W9eTbyzaYOB3nZf5ffcor9e3Blt3nUz1hKmkdfns7PftW7TfFIyh8YgAKHX%2BDSvntKKiNAXBsj64%2FZ46SpyvlvQIjFTk6wf2KPd0udPLavsiwAeoNfm%2BSrdVAesvbbh8nQaXg27mI%2FDMoxcA2ySCZHKofkt1FHg2W9nTMJlrPAWPzWKUwJH35HDVTEPmcML0mKSJDJ%2Fu6Or7tsH9DkELF0lRm2UqtDB9a8nrpQ0Xq8SiKy0p7g%2BxMEdExXFPhfJE797%2FFmKNLhk28SXtor0hrngLbFt85rw0uCA9IH61PMK%2Bn%2BNQGOqUB2MaSo6QetupOomBrTwoSqNe6WR%2B5YtI4aG8DdPDHUCT%2Be8fhExcoD2MkuNKEUb5NZd4DpjtIKoeN72F0eUeC%2FLkE2POYGDo7dnvzQGjV3tiMMIzD1RPobH7fdN1cLFr6v7cQxTBPD%2F6pSYLeE462a7Jn%2F1Za7j%2BQ%2FN8vSRiZOELoiDACEeI%2F%2BA7vVd4vyTI7P5aFmRmnxUnjqNcDT5h93SWdacGc&X-Amz-Signature=98b1e4ad6591723265185881f0bb17d5c0c2680eeb11e5494444af171d9da0ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
