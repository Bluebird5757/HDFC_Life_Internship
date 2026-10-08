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
fetched_at: '2026-10-08T03:31:59.599Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q4STORBP%2F20261008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261008T033155Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFIaCXVzLXdlc3QtMiJIMEYCIQD1fqhlzucwKrtXXd5SPrnCLpQ4hojunEsGxQVKjG4aDQIhAPGVcirssDy579B7VkLC2XHOxjMrn9t3xGku2OVwNM8BKv8DCBsQABoMNjM3NDIzMTgzODA1IgyxfWzB6hvqAFqYj8gq3APRPpuoVVxeV3%2FA1yBPFp%2BPfbkAaW67AqYvtd46NcnYXYUiP4oPkKU%2F1tMHdlx0FU2K2gw2u3TvQrjMxV1Bk4L4gnMPU2uWWnt6CcZ6LtcPDdzCSJiREuXGcbVyEl87%2BfgNpZtdvdKmAGaljv5bUP9z8P4suI70qArAmJbJo4Qv%2Fme4tUVnNwhRnaHYLokwuyUCY6rXRN3PSioC4dD1El1ZYSeGz5bVT4WvMDEcmYehuQK%2BMq%2FdFu%2BtluyVaoDpIv57%2B7zMDU4wVGMDXtlsWQ4qMagXlSbFsO4VdXZq2sWz6QtWMHGKyR7AcayeU9Mjhj0FejNdcbZKs4GU%2Fln7EvPzZFjlZcIHX1Vcwjwxrt%2BqR0uBGjzNqiX83mR3jueaR%2F1E2OUuiZMJ3waoXzuFy4GZ6%2FSZFbPwjUINi5%2BCWkvySER6R6Nw21mq1ABguoauj6p3BHOT4f8ddL9txxqDQ6nwaI%2F48B9%2FGu3OlbUxtKBfQi5snXLaqwsgf%2FCHxBk5U0%2Buqv9Rq8orIhWzHsGWrf%2Fx52qyvMRm%2F9X9R%2FVrHUDKAGx1hE0JTJSDHTKC17%2Bu%2BKBBKQOeoD%2FRT0tHKVxCYnygZRoLvPtipQkUGcRttFN7owbrUb0auf%2BXW9LteTDW85vWBjqkAU1Ba8lZLaVcQ0NJHQGO06K%2F%2FhOkKy%2FxAc1%2FIetaMt17Fh1VwRMDy3v%2F6pZVOaMYT%2B%2BieSuhUEe%2BZElt0wNXhfMYwgkaag39Lm6f10qQ%2FoNwKIumaLmVPXVxs4MuIZdnIYMUl%2FcDfQZMPFw5hjzevyYV1Edi9xyT7iUoFIbb%2FjLLynpftL4XQ%2Bynr5DVgJ3LL0I8DsjARSFMui5bht7SXYF1VpnU&X-Amz-Signature=ee334e9dc3d20eb55c276e9d1a6d435da2daaa0d55e1bd215a96da534819aaba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
