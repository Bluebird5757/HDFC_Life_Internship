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
fetched_at: '2026-11-01T03:11:24.821Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/889b216e-28db-436b-9576-bbecb5dd4467/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OYVWA33%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCz7XZp%2FY1dmV4MMJUOHn9j42oIV2A42m%2BqUMlk05QYogIhAJOGCCG40TIL5xel5zrfP0iyJ14mCxZsstBje8VBQeagKv8DCFwQABoMNjM3NDIzMTgzODA1IgxR8aVlwkpbX%2F7ARnQq3APAqK89oIvqt%2FyydCfPQqHgmpDguPxMMwCqZAkIj1IniOC7oEr4RBN2s9001VTM9gms9y9oznSJl0A9Dx5CcqsKgzd07OHeKe9h1ECejo4aD33tpWt%2FXT2AA%2BhYSGP6TYwbg9dtyiMngjJccsCsMhTaQe2TF0xBdml7j%2FTvzdb9fWETsLKLjQ%2Ffianiilha48Gpp4FgXIJnrGpXPQdbebQ2i1FA4o0%2FqwoTRIIetnWm9sn7DecjwL1WVPFArngvq19l3uW%2BJDE%2FuWA4UI7wtMCu2BDLbMHFe8SJ1UmvAvOGSg%2F%2F29lXXDOsiIIdNudSEccas4MTflVYTTCvinAa9rsKmolNO4EcyrGMMOn%2FT%2FTdo%2FIf%2FivIKu4T7Bh0QhJnyoXM%2FkEefY8hG7oPNVNUgGgN2fdExvdDHgrU8iVm0WfDoqYIXhU1UGmOZEJfzV6mmAQlCMT%2BiBf7JVRonrdr79KWw3dZdo5nTeYFpuHZxm7F1mRAXsxMrbbmGEU2nrndAty00T9t1H43FGksohNtuNl4ofmvgYdS%2FsgMRSFJZFaSjjn4mNWQew07pUA%2FhcRp9PbnbAeRbY97t96NxJVCcH783QgBguaHNjjcKoP%2FSpxPYCFe1Gj1KUCoqhhxcDD82ZrXBjqkAaMrycA2DC%2BZfTRx3w7em%2B2L5IVgRiui4EcxsijfHRoIEalSoFXQM0%2FYuMXHZyD5aZIT0%2BVGcYNgsUncaKVwxe0FI1b8rgpr%2BIxFYKU4ImVBBwr28rdjpGn4nsuoH78TdCGkhd7vLy%2FsemLTyK5UivNAepLSUq%2BtgY98b%2F2SHs6Rw6YTFMaqylv3SPhvGC8LVHVtahD2jDoicyUVwNokmM3jHxpM&X-Amz-Signature=1125b84d590f120b6e61c234892f1fcca729e962e2cdfd14d155352ffc38f6b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
