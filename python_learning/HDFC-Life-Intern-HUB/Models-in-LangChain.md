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
fetched_at: '2026-10-11T02:51:40.344Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TQUTASGT%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025135Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCZ5rKYKT2XB8YgYIDOEYXa0jpQv9%2Flpzcsbeu5ssr8mgIhAIssUlAqWvW%2FSYrGPZFRD7q0er0%2FAw9MrhxaHh0OatGEKv8DCGEQABoMNjM3NDIzMTgzODA1IgxFAfrZuXxMkxNgQoIq3AMNlU73Oizc1Q%2FXaja0%2BFBaeJW3fVSU39JY%2BXAgSIoSjlPF5BC3%2BXPOVYR3Ju6h7gwBL4myJXj%2FHRUCRnMri%2FdG8SZn5jC9v0TumQny2Q3a3ZC1MZNCuJYTZLHa%2Bjz9F5wQjN%2BttIj%2BSRojCFjQpsemlcgEYcl7sviz99jGYPsca81okDxrPNnqWE%2FUMdsh8klE1CCqSeI47a5ooxF0ZlDQVneA76GXpGe%2FRJ2%2FmbwgSjmpZYJHO4gIX%2FCsi6yTPrya61uvx%2B0rsLhpOoy8oG9TvuZ%2BhadZtpD%2FwAtAhHWdcqLqbn4FDFYI7E3YUgGPhwNSWag3gufZ9tp%2BuBJH5DmzOiUdoswMD1F7%2BPCGJd%2BFZu%2BMeEABQSKlgp3tB%2FeZf91vlJC7gmyAHMT5v6b40Clrs4yVWyZvpv4lFeSLy1a8v6j3kbT7jKIWLXFjSoOU%2BlflOsB47JRZkAM83Pgrlu3NpZaNV5Eu%2FU2esdJp9S%2BbAYHi3FhJ0CNxzz%2FXuhQjpQshj5kHNIUlKJ1mi7BNCZXfbjP85C%2Fn%2BB%2FtAibMH7HD%2BNQIRmF%2FGJ1YA8Q9XN4Rvt%2BXv8bk97nM%2FXvPV05CP0Bw%2F60C%2FkJaw4JK0xhockUxW1DLhu64pC5p6lj02DCspavWBjqkAcDvWOIE6ffwVY41QX6Q48aHsphPaXxZMpqoaZQxiCw0zIIgEPT8KEH%2FIJNjSN%2BhqobCYWb4g%2FobK5S7%2B6LQ%2BdjkwfVDkcN0KlLvI1VzsLQZBxVVI6NPqRXAyonErM9CiZRCjT7e8YCwPUb9fOuEJ%2FB4jCdjiWTqnIVYCGQ9lwuPNIgyf%2BPXxxPcTu3DQ4GxjZKOlph34vTLP9nfvC736yYegPQS&X-Amz-Signature=bf1bcb228d4130375b9ef20066db7a6484da08e6c6f93a9a482df6bae4241c11&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
