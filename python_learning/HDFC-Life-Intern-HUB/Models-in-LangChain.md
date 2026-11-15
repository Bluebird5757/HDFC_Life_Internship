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
fetched_at: '2026-11-15T03:46:24.085Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QORDSUSS%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034618Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHcTIsefnbDsI0G8%2BAHy978ELM3N3kPHKGIoGJeae7KgAiBL%2BDty2d9QQjD6LjzSdNrqLDd6%2BggayE4vcwgphuDZsyqIBAio%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMUIffjnhFnrG4By4%2FKtwDNa0GCnjA8sUWDMMl8KImnPLjqvUJ7wP2eBo2itoQnIRFdJalyusHvrO21YPeSNWukMIcqIUYy%2B1GcgRpWtD89AEclAL%2BzmlE%2FfBKbsQuTpIPxKUQy5yx9B5Bjq%2FWG3FySebQ2f7a4aqcPghKdgnSUmWQHNu0oNvVpyPaV4zNOUEMuBFJO6CPylxNsOeIL6Y08c0QP82kfdNyI31PYwn6GOLgaY8Z1iR0MNKxmBn6zk%2B8o81CN3NG3owly8myeWXWwS%2F4H9tn5xRp9vrbfMVwP%2BXsaMhzoepBZbp3%2FrXVa9a4byfGDNbXiy9VO1QlRMzwkIA0BBUGeLA13C2lcRbCBWGWtnpeILXT6wXngyxj324ludFkMBKQspX3AXXe4jK8CMhpGtXaSnQjcDN7cCQ2Z2PcnM7D39dNYFQ7qanwRGXsFPQzStSxISVIGyNcbxPQvzzKdvg63%2BK6Oj%2B7kVeS7oobQ3pPzPO0VL2VW76uX9UQMKj%2FG3SPbX2XY3JFM9QMCS4igfQdu4qXMxaCUL2i%2Fyg74Qrxnz44o4CP2KL9Wpk4lCejY7utULqt0PLn5zLOlrD9hYuTxmV8VWaQh3ebDFcDp5kavvFsapPGIQzQ8UyqWT6CEipNV%2BShTfww2drj1wY6pgHD%2BPQl5Xi99Ov%2BRpe1ip1nqQTT3FopongUKJaR4DKn06b8%2FYDTD4T0mXsJSuwxEMwcWvCsY%2BvxdOco%2BBTOtC2gHPgHQ0dm69MbIXuNVhUMI0Tz00B2g71A4s0iucwoeyIWzwRLIG5ZsiFQYjtQwfoMeJmU3sscps27la%2FZnHJDMfnG6ErSwCasXF1jsUeJqiIFavPR4gC78J2jHUUnP5HqnNkA7V0Z&X-Amz-Signature=76a0099027bf23bf63d398cf8bdb540b9c7b26c762119be8564fa502f3650d1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
