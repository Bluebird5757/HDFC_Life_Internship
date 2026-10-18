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
fetched_at: '2026-10-18T02:04:59.176Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FJ2EUMQ%2F20261018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261018T020455Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQCmqqhf8RJ9zpxuhg6kC3xSpN%2F2gKC1BuZ4YYg3UYSKZAIgbW4SpHl%2FHHsP78l6QM%2BVhMtLSHk%2BVz5imZEnLVLySOoq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDIsm8KqG2UX52pvVACrcA29PhdP46sRLyDaPSwdx0SEPbVmIpRMTvhE7hTxvT9PKyRg%2FyUZL1yPJ59CX32TIkce1NQg0d7Y%2B7ASH5xco8XG5lsBKlT83mt7g%2BwEab0pJZxIC4Ksuj20q554C8vN%2BD2xuDHzf67uWFgk4gV%2B9SSqcU27VOZ%2BH1PB4ZI9vOFfaznbtnXJk9G%2F0r0WxqwdJRGnZS90iVJ6wdr%2Bh51Xazfe4LnVo3MtMPD5wgNc5ELTjQqtXTVPq00AJzBV7xeqfOZmTxoYAeqS3%2BMjll2BoAvZz5F76a1baelfgefORru%2FsR83BQ9xW6nfQ6CMExVNl0dmkqK7ZOVXZo6wqn%2FT6IY1FT7j36UiD0cObvZvGWoAtVICAUEi9DAXUCfoPU0k68qUAzlF4wujqQ4OGSi70WDc5%2BP3k5tAkDcjNAaNdgbCNicY5Dsu8YJwY8sm48CfKrR5amMFGPvrYiKLgVnAIcMuc2421ZdBPjB37hhxNvbKwgC8tTfUKKOj9MBEfz35PdiK5f5FOpcRLDWBQ4%2FwWtgYACtDst96BReIwAnAWdlGVMHLSHP5Ey%2BLRjPlaH52IKhvgrR7EtUJVJn16GGbwkQiVxHNgqBIQ1QYYL8jMesa2%2Fd%2BkAfFj1L%2FKlVneMLig0NYGOqUBM6mh%2BT8u1l5b%2FTdspcoOFEQYIJ%2FdryDqW13SCSNiSieN40xo0n0JqORLq7muxeAf2764HjeGmzo4kVIhMsbo1SjNoXRA7S%2FZSfv52UDtUiNwxOA2mLG7mjHOVPxhw4Aj%2F3FQo7ImCtfnjpC0r7qOIH4BkaOTs0w7ovpa01xsHbQNAR2ZlqgRuVvPav%2BtI%2FIL4IeFpWW1NpKe7KdoqbsHFT0g559J&X-Amz-Signature=272a5bdb0766f6eaaf0b671f0080566d748ef4c073c4871bb1092912de595a10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
