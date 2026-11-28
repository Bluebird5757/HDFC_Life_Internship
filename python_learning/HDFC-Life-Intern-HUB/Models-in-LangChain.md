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
fetched_at: '2026-11-28T05:29:17.439Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ZVNEFSQ%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052911Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJGMEQCIF7iE%2FvcQNBj01qAHWvefyNhzbR38zAmfTOh2uyB80nUAiBEjmbtHBDggWIgRW08WBZ6et%2FCIuZG%2BUJR4pVtqAbIXyqIBAji%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6bNvI76FsM6z0jQlKtwDlv8h6kCffQ1eDfIiTcFPdqwgiE3zAOgm0BMZzEyTGArEW7U9%2FNQWTajmR9h80CFuthdhWhSbM2qkrzm%2Fxe4ZnO2TFgxsY%2F2wVya0L6nFxJmgD%2BATOv62mECpA6e%2FYL4No0QjgWogiRa4nYWgrxoDXmKvfIaxzpDThiq%2FmRumDlDbrYXW5GTP%2FSWgeau5JyK4WS4M%2FaPOaJymda4F2N%2BraUA%2BRCIB%2FRp37819WZtx0c2oL36K9SreXBhLVqTFldfFG9Xk%2Bsdb0dgZ%2FGANYJ%2FVkFZkthk0R8QuiMEYyUd0l%2BOL1FMKIJYVA701TMZ%2Fhv0OtYPf88jL3xJPooT7yjw09VK4qT%2FtX3eSlX9GQF%2BqXgWZflzU1AMgh7Ai33k0qpKC6RJgGki7pbZpYk31DWnhFD2DzmjTl8cSarZs3hnSQbJF3ozH1CbziGfoZy1NnkMFcafsBX%2FDbpunmnF2pH8E3hBSWdxZail9jRKY%2B5H2cPZjNwApZPDnYXtP3axhCbVMgxFlxsob%2FV6AKtfXvKIzO3T%2FLg4qplTvU3FD688gclXwz4O2KsU1GmU3qd6Kv2i1%2B%2ByeP9npOeuxX%2BvJOWVhxlrTYGTdNJI0BZ0Z5Om1CXtWyXx5oBzffZ%2FcMn8w2tGo2AY6pgGRufeUlI6gWI%2BMAr4CtfyJ5%2BftnYnqcLkvohiMUZwBa0toil0Bvhh4d%2BFkIxHk0ShucN9PsGA3cP52odq8EyyeXZCjAeVU424YrZjk5NQirRTGOAMswBOmBPR060%2F5PR2Azx0yHYmrzS28bX41vJSo01pwMTIAhFSCV5X0h6pB1t1GMBWmuh%2Bv4KiNb8N3rfJuEh4YCHCX4CJ3YqHqceN1lBD2SNfL&X-Amz-Signature=383ca1ad3b5a1e7426bbbdc2fff826886b2568ae701c6cd1735c875122078725&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
