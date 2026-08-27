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
fetched_at: '2026-08-27T05:43:17.109Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RSJSSYW5%2F20260827%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260827T054312Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQC1%2F7J5z0a4VFpG%2Be0OfNkG0nrlcAx2V8kMz8rGEaJv%2BgIgFHRygU1muawVbn1hUduUMCpGrmGQ4FBgzkufnjfmhhEq%2FwMILBAAGgw2Mzc0MjMxODM4MDUiDMdTPyVuTHRzii8v9CrcA7nvXWfEk5dP6XLyba2fEESvJyKlXtw%2B71Z8drA%2BspIHjpGM5H06GpafAqCyy6TV5%2B2j6l7hQQYYnwMqofIs0PcLF1hqHHUs9y8pmBTz9Q8WDr5nmiozlziHdXdQTr1Tnl%2Bm%2Fif8WcJxYITx6DU%2BBWIkTEhY%2BUdArXrVTh4J0jrnnX%2FyV%2BepP2Zf9xFtRPTb75pNtzLRwaoXTYlObUf0pWQy1ii3YRDJ4TDlFOI5Ce7eqaRZE6EOySTqtAfNtPdhgloLS00M0nPJkOIlxf3rUr1i0SnKX3eaormUNNR0RRDhMxq5F8Da5GFERdfr38FGH0Svncu4wz6K%2FzKUVHm7iaPLEE9REmhRz%2FECF7Pi1HFK%2BipzmcGfnOsbLLrRVNcS5%2FM6OiX%2BiWr3uFSBqOHShArifh7cpVOmzxm4f8MO4G7%2FvQ%2Bf2vLFnC%2BVZEv4FDOX23cexran8UMgXCia5E3zcVnKGz8b7A8FeJ7%2BsIg6r3QhnOZtVwcw3OO1bo7ISB%2B2Xz3qaYsGEzyQ868AwAuOIeKHJgBlxRlN8IizTZw%2Bz0Qr5xU607zVQrv7DvojKfd7C3tYz3Dz2KmD7%2BM9CFytyEMtgnkshj1O9qeSnaXsBhetQc%2Br3xF3W6gHkr6tMIDbvtQGOqUBOUKWnSAy3lNau2WVP%2FKiBFSKiLaurcYn8NvMf5FxR3aeEASgQofhN4ybNSAr8wzVAwsC1XB8ft2MECXtILAYL5okO0gc5UVjWVDocieIi2X56JPlqVaet%2BmuAAWSqET79va5wiulozdL2OR8tI5eGcDRtY34R6lfRCD977BtRELcqSowB7eZbaO6PBaRobG64Lv0PWH5bstbwluCDNE6plT0IeLs&X-Amz-Signature=51da398a102e8e48a552d81671bbeef91e6ddde9abdeb1396b17e43ea9c57e79&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
