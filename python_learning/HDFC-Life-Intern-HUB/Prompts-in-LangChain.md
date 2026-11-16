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
fetched_at: '2026-11-16T03:47:11.924Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SHUGUZWR%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034707Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCab2L1uDujOf0pdGFHWsU7v4lGXvD1NkmBrP7%2FJpTFxgIhAN4gq899V2EnKtRajEs2gbzybrRzIC5goEPTdsENZ6JoKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyvrqdmgZY8y4TLgCQq3ANR2EAXKB%2BA4h2W2sMpRyDaqwXSa86JPvI5p9Xd1CX8asMe%2BpmZUNsgAXLp%2F8yw55z4S7NjHxSJVCL42E1ABv1ZOOA6gIqAp7KVc85xo4yPP5oqgbZZuCl3Yn%2BCG8KSS%2FDWp66ih%2BHYc9J9I%2FYDqHgdHwG8pV9g8DWAw59lKJ2DCU08lHfA%2B2rU3eKDTgB3p%2Ba%2FWXzJn0FA3SYiHN1wCit%2BmJa0e%2FXpQZ0uEYBt5KBNARL%2B39xAptKXyeisNR7cVfJYaLm1kYJEVN5ipvrgXJbGirE%2Fy9R79LsECG%2BJcpMTLjIFc%2FCyHo4klb1n7p99oNJIAocZd0VnJoxOSTuYefo%2F29CAYEheJbbyALKeOX3LsEX6lBzPkzAezfSIUpIEIvB20WFqSfXyLIfaD59jxycIGrYoD4Qk5gIipnijhOpp1%2BHXGe20YfzHEWZgqYAsB4VB3x%2F7Wn5NkF75Pqo%2FlmaZsv%2FJJ6OJ%2BFVotlR%2BAxYuPrg0FSRLak7M%2B5EhZWcE%2B4nfDLPhgB92svAMRlxPHRZKLAlZ3OhhFl2zrPqfapyBiYGjgFY%2FRW9Q68yzrEq7HDpe%2BrsjYkwWh%2FwoqbUoCeWhhJYLc5SMSaLKUUNeVMwiUG9ouEqKXksXNWyehzC0y%2BnXBjqkAcAfQ7JOe33kO61NopmWXpUDzCjoU%2FQuOko7tXd5c9lu%2BDKKsEwnG4YjrTP%2F1AyKw5QE5gBHwsXPIdVvpsynPVkroKDDiThHyJ6Bd4mbK8OxEeU%2FxbQ6rzkbXhORsXACGh4CGuVlsdZe0J7uU4%2FffS1DBVHQ9y%2FJIBWPOc6Du2BXiL0voYyQ%2BA%2BbZV7QPIgqS1eznaLZ8zfHH6gy9SL9LAj67YkN&X-Amz-Signature=80253408481593c85dcf8244270a9dcf73e27712db006d68d7cee2916ac263eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
