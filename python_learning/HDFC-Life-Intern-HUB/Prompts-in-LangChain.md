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
fetched_at: '2026-11-06T04:09:06.019Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YW7RSYSN%2F20261106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261106T040902Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJHMEUCIC5xSwfxjoWarGbspybu3%2BP3s1mrHQt5XQyYy7CIuWpbAiEA73Nbsoag8Bj0vMaZRaeccfEhTXO3sngMrMet8qIWTAwqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPxGtxl54qg4rNH9vyrcA4Bs8%2FkVPXYMaeBJ6DoMdzdTgQpb%2Bon4sTnyRe8YaFu2U8fPUm5EtiQBVMTv6qPYAWPTKtX%2FPvnsNNsvUAn%2BKBp0ecONCLlUxbN1xgzWtuH%2Bj9bqXjabGfdbj4Pe81sy3inEjB8QPCiW8qvWpwf4Ftgn71ESydsnYTaDwYP9u4L9iPt%2B0E12hLneQA14iBR%2F8urXZ25NAQYLKKWfjpFV98nEkc3n5I%2BqS%2FiofLiYMajWUXpKmS83G3NPmk8l89xTA88YWY56hfDX4CElEoLBMKgnrSzMru4gCllTAB8DnEdxnq1o8DfJoU8p6%2FlwcypYRlI%2BHje0Wa6plvK96q9y1YUFpGrFRac%2Ba%2FPON86b5iN24Tp7XMumlyOOhpTb%2BJSn%2BCjKL8Iv00GUFD%2FXFxh%2F6JLbzS68v5uiaFILbPU5zZlnaNa5i4bElIV%2Fmg1Tj2b5DF70IlvfkHgYt%2Fh7IyqBGsN1u1xGXFMr%2FsWjUTFXNf7biiSTHJdIXGe%2BRjAqJKtvQQToBYudFNY3yLU6iJDJH2xD%2BN%2F95wzUvMY9gPBUh7WDFBuqh6HpHVvRWMBrckZDw8pmt4ZfSC2JepHT8v4YTTD4UVdakVE6QBdVEj%2Fjajh0slCHikzlw1ejo%2FqDMKWItdcGOqUBvenSeO98pD4eI43LxtA2riAIlfurztvkL1xSU7ubxvtwkntT%2BDXAiMJp5VuSyzCSvrqobHm7j%2FaXmT4Q6BV%2BE7MPkoGxbRaHASEoYlusrpcx5NjOYAs3JI7pPYEqOmDMoREOW5sGPKX6M0FxxDYKCFFrjL8irzaluoAC6%2FObywAgH928o09ZV3g7OxuHwCONM0x2UOLfoFqVRV%2FQV%2Bdeh1gIVvK2&X-Amz-Signature=859c1ca4a1f978714c2d2448354673c4d57739f3544f525e822577213ff474e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
