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
fetched_at: '2026-09-24T02:11:03.190Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46633YQNLAR%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIDubUbJfc3vyujE%2BZmxXhCNQocXDQ92HjJ4oJpN%2BaXPeAiEA4rysoqBHo94j09P%2B2r3rrwh0LCJZ0hlkKrTuhaCSbE8qiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBvebdMrUlC8StBrUCrcA0gjk8FfkvU9eJqrB5V080qiIW0eX%2BQBzxFBQpHBNuQ%2BSy1OsK%2Ft%2FasnYtmmvY6G1oXZuwIHiQ26wrsE7tCCGXMYWweWPd8r8rWJLVDzYrVgY7xDJG0Gv4iEj%2Fll0KP41vTi8OouK1GSmITyJrCAsRRA0RXDNeFmlyQ8SOSNPIOrt7dq50ESPyJFO6Z8azgywWapOcWQLGpC6TJWrYcIRwJ0UHXmIvptb4Za4MCCGnsP4vN8Tcby3nBbqb0JyWsmvFqMmnwo4%2B2GCoIhzpOBWhkEUQ53YKB3Hm2EhYkM8alE%2BNK8oGFnN1KP7XnUEalEYLt1%2BHiyoBgCFni58edydjHE3zs78fKLtAChF3sIpWeDlf4Tifnyc4XLZFqr2qm1uAqiM%2F5vKLWrEdT0YJSKGt6LtvmD3OAfMV7x4c1jDzYpuKjQtqPLsSTTQ6DijKP3n4NAPjEAgVnkbuJS7H40q1uWRBa9QSjzGlHm0vFxZ33Lo0A6ImzegucGUXKXMm5pREQIGHdsIOzQUN%2FqENvXjmnKGqkjBfs7iYgpsdr%2FYHABk0bw0aS%2Fj9CwCschqSSxB73H27smO7Bn2uwBrbVJOpB9Zy5Lx9Jm6ZwBTIa5AID4JMlOAw4JMb%2F%2BbV3YMM7x0dUGOqUBFv7kVLtM3Tt%2By0fum4Akk0zJRbQuGc7tKENo5%2BopyGgKqF0xh3XlJaqkpK3xoWY8ZgkWLeNkmYlYMPCIgpZ4YtVfCU726mV8N8PU2%2FQpdV%2FkQVjM3uJkt9cN9lFvTHhoHp0KCbmluZGO3ifDeP1hkEGLv9LztrpFi0euQ3NUX1ZldKH3uVkikFTcGMpUaWKPb66Zu%2Bu9pXYrx%2F%2FQGCt05RNFHz1R&X-Amz-Signature=1bc131daf8bcb3330fb2e005191131998852e4b2713e6b314691137c21f71149&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
