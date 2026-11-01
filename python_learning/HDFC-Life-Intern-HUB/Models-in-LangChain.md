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
fetched_at: '2026-11-01T03:11:24.681Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W2LYRT6X%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDMSCqGqlOPh%2FjCi%2FiCLN3Vdlj7cfGhMSEEpIs9rXtRVwIhAJjp9kQrv2gK5TtRfCdkFfl4MBtBrTfuMK0cxgge2paBKv8DCFwQABoMNjM3NDIzMTgzODA1IgyEItIYVYyyfzy78kAq3AM%2Bj7rBOmcS%2F1egXy35AOfpFXAnpNUE64Ph52iSBUntB%2BOMC6KgfiPvKbpyxBYZ4R7RJ2dcMYO%2FksxCVAlEPpX9pOPZYPzmjuOUUjML%2FlIll5RHYcIo4y3GqeJKGfiUZrdac7%2Bx9MWBQGCOVbZe6SEP57GvYIdOxD1caQdA%2FDqLcYbHu4MLHp%2BDKtQx5udv%2FHrdhcdQQmHw6RJguXSZH6TzGchZqw1U2GHauBhB3P6eUGbBbR5SafxCsix9ybRH%2FqNCcvTHiwAvcPA4LnGt9H83vxV9Q5%2BHWoGA14fiCUKzs%2B1PTdiZRXeET1EbOFWW6EPlOpj3JcAx0VHUE%2FZBM2QcYJ5aIkR6yAqEFv4UD9%2BkSjNo5AluwPEay34Ts8aartoMsuyGsu4ZjO6wxVbtWkjK%2Bcgy60ATT2t0%2FRtUTo7N2Lvf6evwolSRdk8J9A1tytN5G%2FQXGNSPv%2BPUPsHJ1vFErgv12%2F7TKb%2Bw5BbKt08Xmd7MxcgMTfqf2LyxSOMtmYh3mJLQoEt3un6UmWRdyLsBkovC6xOaHguE5GNgrAvARrQMsOcv8S9CRQbGQjstTohs2z9f3w79afKoJym7ypO6v2IoYmqc11gVy7QHI4OXhP1oFBXUlkUk9qsqhTD%2B2ZrXBjqkAXZPS8diUYzZlyUCIrRvow1gswHRDf782oye2ZAFOdmJKIroEFmCbC9VpIas3qJAcceLkDPkJGUQDVJBKkgvACFoH%2F%2F8L4OjDy%2Fn%2FXFoI0%2Fr3fbTzPdEwlt2cR3yZpelSkkyVi7uq%2B2vQLHKz7gyTWXlCqE%2FoNpD7hQ71VKszqOhv%2B8cWwZnjRrjKr4mP8eVeFG0l2mjPa9soIqHBb%2FkcWPrg%2Btt&X-Amz-Signature=933933c57e9ee02c323c71abf2bfae3d241873c6ccd5d5115bd52a5cd6101bcc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
