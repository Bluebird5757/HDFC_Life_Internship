---
notion_id: 3c44fa76-9938-80f9-aa74-d8168b43904d
notion_url: https://app.notion.com/p/Models-in-LangChain-3c44fa76993880f9aa74d8168b43904d
title: Models in LangChain
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-23T11:33:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-09-04T01:57:44.916Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X4KHSOFL%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015739Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJGMEQCIHWCu2gcQZIs1mKQAaTb4gu4HHrGcDQh1k1ya2tAT0g3AiA6qMOsRiSsr%2F8gU%2F5S9cPucrXTUDeMLOj4tCQkA1Ej1yqIBAjp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMreP2sApRMMHFM9JIKtwDleojHF%2FSglAKF3pAxPYvSkWf%2Bs98Dz1NUZ92E1GqHY6ryFSBwJm1tzNWNXh%2BwZkG8mEAsEl0b3OXZfQ%2Bl7T5yad1qgu9TKYZZM0RZ3EqWj4sO9NqtQYWznzF3q%2BB8N95iUBNcL5k%2Bf1WPHOIu4sjIZDALn0HotQ8hFm0Zc9IckQxcZT3hJRd4HrKjZbODFh7TGmqCNT5ORtpJeaLWlWLeghlyvXIP2nieaLjILjyc3OEshkaKS8nD3Oc7Ez0FTouiAR9qYkioIx3WtwsDU4KBLUIihH%2BvkDA6gAo6dmQd2UPThR%2B6U1udYGY04TONJcABXdIRQvehcMXTw%2BxtCcL1MhWpCz4Maw2jIu%2Bl5JlMwiNOZlG0pSMF4RIXq1jSSFJAjETSTpuPG5bVt027cD8gGZnf2r8ChZ%2B1UvQCc%2BDwm%2FdGA5KGLyVA%2BO%2ByEB0QKAgl%2Bms7ukMA8gu%2FPM6rOUR6gMdbSAvIYnt%2Bi8VflxAY9g9vF0vY3qr9GPsPM5nRl9ReBmq4QAS2u3gPd%2FzUIfKfgIvqy2l6ssxzOR5W3XfMPROnFrCedyz3UiDD0D5%2B69%2FzOraCj2D1Db%2BolxmzGo2ZwUkNo3EpKG%2F%2BbmCLWNErk35tyH4nLSMDZY1ceUw4ovo1AY6pgGT0Vz8bZgEiRYRzcZWz2e2sS8HqrIW2z1ONkSD%2BNNg9UQmovWmEEvUKIH7Eb5yv%2BvCQdHlF%2FQ0n4mW38PeTOOr1dDJiuvtJ4eoCx3BTJGy445IL3Yg%2BqhgxymC2b2RXcoOqhA6iiGXinT7xNL4uT5yaY8PH6KmjabgsdspdrlK9NF1oQ29leVIpjcCSU4FygYm0iTbyQ7rSUC1V8pQBIABR1COgHO8&X-Amz-Signature=43ee7da2adfbcd92e505ed1eb04723015004cf1f94da8491eddcd3c94741c2f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


there are open source and closed source models and to get the open source models we can get to huggingface which is the largest repository of open source llms


so how to use open-source models :- either we can run them locally after downloading them from huggingface or use hf interface API(this also has free tier)


there are different paramaters while making an object of the LLMs or Chatmodels those are mainly temperature and max_tokens where temperature is the creativity of the output with increasing it, its value is from 0 to 2 and the max_tokens is the number of tokens it give as an output, keeping temperature 0 will give the same result everytime


Example for LLMs sample code ➖


```python
from langchain_openai import OpenAI
from dotenv import load_dotenv

load_dotenv()

llm=OpenAI(model='gpt-3.5-turbo-instruct')

result=llm.invoke("what is the capital of india")
print(result)
```


this is the openai code you need an api key in your env and like this you can just change the model you are importing in the langchain like anthropic just two to three words will change rest of the code will remain same only


Example of ChatModels:-


```python
from langchain_openai import ChatOpenAI
from dotenv import load_dotenv

load_dotenv()

model=ChatOpenAI(model="gpt-4",temperature=0.2,max_completion_tokens=10)
# we can set the temperature here
result=model.invoke("What is the capital of india")
print(result)# this will print all the metadata and the content and everything so to fetch only the content or the result we can so
print(result.content)
```


this is of closed source models template for hugging face:-


```python
from langchain_huggingface import ChatHuggingFace,HuggingFaceEndpoint
from dotenv import load_dotenv

load_dotenv()

llm=HuggingFaceEndpoint(
    repo_id="Qwen/Qwen3-4B-Thinking-2507",
    task="text-generation",
    max_new_tokens=256
)

model= ChatHuggingFace(llm=llm)

print(model.invoke("who is the president of india").content)
```


this is for the api and for running it locally:-


```python
from langchain_huggingface import ChatHuggingFace,HuggingFacePipeline
from dotenv import load_dotenv
import os 

load_dotenv()
os.environ['HF_HOME']='D:/huggingface_cache'# this is to make sure the cache goes into the D drive
llm=HuggingFacePipeline.from_model_id(
    model_id='deepseek-ai/DeepSeek-R1',
    task='text-generation',
    pipeline_kwargs=dict(
        max_tokens=100,
        temperature=0.5
    )
)
model=ChatHuggingFace(llm=llm)
print(model.invoke("capital of india").content)
```


Example for embedding models:-


closed source 


```python
from langchain_openai import OpenAIEmbeddings
from dotenv import load_dotenv
load_dotenv()

embedding=OpenAIEmbeddings(model="",dimensions=32)
print(str(embedding.embed_query("Delhi is the capital of india")))
```


open source huggingface:-


```python
from langchain_huggingface import HuggingFaceEmbeddings
from dotenv import load_dotenv

load_dotenv()

embedding=HuggingFaceEmbeddings(
    model="sentence-transformers/all-MiniLM-L6-v2"
)

print(str(embedding.embed_query("Delhi is the capital of india")))
```


Now code for the document parser and to compare the query in the documents


```python
from langchain_openai import OpenAIEmbeddings
from dotenv import load_dotenv
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np
load_dotenv()

embedding=OpenAIEmbeddings(model='',dimensions=300)
documents=["","",""]
query=""
embedding_doc=embedding.embed_documents(documents)
embedding_query=embedding.embed_query(query)
scores=cosine_similarity(embedding_doc,[embedding_query])
index,score=(sorted(list(enumerate(scores)),key=lambda x:x[1])[-1])
print(query)
print(documents[index])
print(score)
```
