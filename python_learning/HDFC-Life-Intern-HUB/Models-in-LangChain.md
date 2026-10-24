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
fetched_at: '2026-10-24T02:28:44.382Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDXOISFL%2F20261024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261024T022839Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDG8BuVMfvdN4YfSaxmCvAz%2BdVAXwvY0zJioni%2FmIGY2gIhANAA%2FAJieM8V9mgFO3K7zF%2FQQt5CY3rRcxjQomHoPaniKogECJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz6MAtT4FFcHKcBAlAq3AO90o6OgA0HpEs84AiMzGyy2IzBAzqQxKL3fXz4rSRIaHGHEyVym9ytkxqgL4326ZrEsJeEtqS2C%2Bt9KBlcNXZMOVTHsA2mNzgHIxHFeYgHpAGnL2YEd4MtoI%2FDerDBut%2BUr6e56T53ftA8pCusVn97cyEXspT%2FmtB5U4HCyJHnkeFscpc2XX7kWpCwpMWa3lvaA3HjugMBoyidnAufWH921vX2Av7%2BS741A1yQeel4aF69HzzW9Y0Ve9jDilLIPGRwIovHFWLWn5KvBUwtD9iVISgoktU66DXZHnRP1azWHszMlNKhE7kYONdM9FPCP8VuEYm%2FEDvoR18hg9gK8LOlaUVswX0Rm0tNZcjpChwUYwbymdnnWp19JtHUWcQZMtxCPbWmSz2qpWAMdpe59zx2LOXKPHUScQQtvRXmYkxb%2Frhzg4YZ8SCk9aqil6bdQ55XyRKVsut6UC6CGYFQdXmb4U9q%2FilJsmrcwYr0fiQxmqM1K40X4vUG%2B6mIZG6ahrs%2F09A8X1QKW9YmuljmZhEsc0ZjmhZDc9kUDm%2FSkO3IQmjwpv1S6qOJmUL5Z7P5x%2Bv6DfKRddY7hkSmoJ3qIbLvHzdUPDxsXtYNDHBx57AMN79kL4sRG5tUSzTVMTCGrfDWBjqkAUMTJeQWFpM9MpBZwvkyFw%2BvJMrvWPEdT%2BK0y%2BmY0riBvyxIiUnAEvhpMiE6hUY1Ekv2czopUOnYDiZ9M8lO1HsbRpsWha841Q8x5cHPu7Ona8z%2BrInknzEMBnT%2FxnbFnjxGNTJ84S2kjt2vgu8Ckk5M8k1N2vCfyHQbFo1XN7DwirpZLLfLqxX%2FpKXuZOmzWHeCRezNBFZQZX7zV4GKROhGz3%2BC&X-Amz-Signature=50930f9d664263bb55c5b8f73295baffcb6003e769c66477e5cce129005cca2a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
