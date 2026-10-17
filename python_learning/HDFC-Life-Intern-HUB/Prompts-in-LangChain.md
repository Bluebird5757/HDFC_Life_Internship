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
fetched_at: '2026-10-17T02:05:45.397Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662T6DQU3K%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020538Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIQClNznjQpVF5826jowMIrftbq8kklseDMT621Vr2qzDDQIgd%2BOR6qbG2VT5%2BYGfrw%2BJFd6gi33cyP9Qq64RpkkcqPQqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJRRKo2udmxbEbabLCrcA03S1fT1AOEYswiTK71f9wsPdqkh5GiYpyPb5yHsWGKs8F16QnN%2BdT8ja2h18BhBSzTgsPlAwYFrlqcj40is9Kj5K4bMxxS3F12X82AMOM0CzgsO62iiu7rss266IZjRi7TaSNXHxIJZ%2BF7qWShTmAHAJg5ChlMkz5f5yLZLN%2FwBAF1%2FsPoLfZtwzdJ5Zh%2Fp7TDoiPC1QEvNuT8xT41Pm5dRi%2FSfuZNlEYKqCxWtR5j09iv1XUBeD8rgJxEFN9rkCPKOwajkJL6DRLnYoiFVRCAQe%2B78%2B8RzTSkiwfusZQe7IYwOgrDVI2JNnjNyKzGQJ2NgIutVXEbq7H8hXwKHQyGo0cuHw82G5IOX2di1o2v4nV7w05SwAGIklVMNZnaAk1iQzGYzIzdgygnUAC%2B8WXFYCyentzqGMxH1kZmes5v3LmX6FGx5AvfSNPb%2Fucxeq4jGu05520tyO8nnxyWwC%2F%2BrP%2Fcj23bmy%2BZaDT7suZuEWT3kdp%2F1SWcvFoGMdnQuP6Wv3OE14JXj43y7wpUj%2B44u80Zhj%2BYIqJ1gcgU1GWo3MTIcPAnKuyo4zfgFqZjcp9HWBo62EJRsqtdUDbUlnN6DbNCQt2HehWHe62LqGWWji98S4vGfYXb%2BDqZIMLPgytYGOqUBtr3m9AWLLlcgaM9jseIuh4hyPGFbxneg8htA6jbHLV6R14ecw1T1wwMbcv73mkcEMVSJ%2FraESye91K8Zd7680%2BrAkNGiqceGcmhojT1a%2BXqND0k3LWBU3Bd3Owip2ireu24wsWZ%2BbcjGtSHjaQbqTYtojOokJAHpS8wKB5u0REL1lLIYiByLlAlv%2FaiiMxOFk5K7Wab23f2YChTfmmdRXSxpANkN&X-Amz-Signature=1cc61486b6391014f32c4ec7be78ce22a2cfc1be7a6c0df4ac565898a0f3e5e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
