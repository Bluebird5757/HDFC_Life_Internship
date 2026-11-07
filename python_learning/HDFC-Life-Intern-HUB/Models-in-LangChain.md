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
fetched_at: '2026-11-07T03:42:25.335Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RI73EXEZ%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIBoxP9qUddX1aiG3QG%2FJa0r3bAvpvCM%2B0rpfU0ffqFM2AiAzAFy4bSJ0WiPywqsA4zSmGYMzc4dD02NZH5brSs9kFCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMD0T0YfP56HnMpFJKKtwD3HDMGmbPSdQ8us7K%2BYKuqPlSN97%2BH8CX093aH%2BnK31SMEpk9ZiUjm1xBjm1fxMA9fzbWPNBL9Z0aOmSYOTUazOdhuWIINa9lPKUwVlWxGCMdyC2vSdcAY7Vz%2BSGuHjF7UYkaj0zkeszvwgX5bsfKdHfCO4qBWrA%2BdVW88CySMnBNzY2p%2F9GqGOR00XU6MmXT52xlmML2aVFQSmyXnweaEqXi1NkFq8wbByYJvSr9lyeGq6YNUsXiqNfxJMUmRfUDad7xqzkgGxKQL3dM32lzPzpaZBjwke6VjYWjJf%2FFDW4V%2BtoJc5e02uEGCUgBbLjdealiZqBtruLEiFH3Vnor2lyPchU9jGorMZ7ownyIcN3tqXCLDWp03QZKoC2oULeQdRGqrN8l7yG0pp8JHkGzR9VXPy4%2BL2Eshh5ft66eA073i%2FZD2FM%2F79dnRqBZnSQ4fOmPAq6UfFB30eW3l%2BIYw11nGX7s2kTujM7eum%2BtqHgFiioq3co8PRkGUDjJnHT1FgX0ARRnF%2Bm8MP9buILTffsa9WAHCZvHDbBr0HM9OQf8PiIpie517VuF27wH8cJxnK8iTdkUo3sGdNi20sraYcFo0BHH%2BukAUqQ3ZO4bSvvi2L2RByVsSZOCixgwg5a61wY6pgGQAvm0spfyZfWOF7s8QX6qgp3OFH2bLqBwr52qFJQv84rwo%2BUA8RI8JMkPvPBKxYzgYoVYoSSJvQGSGl%2FdcV4EPLYgb71p8y5OTLS6Hsm7bwjTo3SidNq7yUQhoe2XWWDusG2DHOdeKmpIGVoHaIi2DNpc0oW5%2F2N0nNV1Yv3Zag%2B2HN2kS1fp6hvNHaZyMc4nufZ3p7dYSbc8yfixyeg%2FUwOR3oWY&X-Amz-Signature=7fa9030d9fe15f1752660faac3ca233cd6fc43b32cdade16d4713ee33113be8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
