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
fetched_at: '2026-11-24T07:33:54.891Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHDB4YME%2F20261124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261124T073349Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDRgESOefT3oN0%2FwCROAIINPr19gTkxvzUzO4jbHgnFcAIgP7qwtaaz24rRjhcI57p1oinrO1FhK8oL2JXCTWwEjmYqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCxgv%2FeJ9GEn4GZDpyrcA7KYgWrw4fdXXm8QOhHZw1gLsvPMCU9vZtyIH%2Fc8t0Fs9Iat9R%2Fq%2BufRGzk13vPnOYNFo%2BXKBanJ6SH%2FSl5SAA89le%2F45ZU2okISXxZsjtSbjgVpsyHG8zTIfr1tZm8ZlIJaE6Rqj1vQyL7XNOd0PW7cL43jaXndcrbd7oXYhCxdU7SBEML7e129kjyse0cvYGdVBFfhKb3tQcAmZtnkB8tecC10s8WCnBz%2FLvPNERC5fEECAXSlK%2B3excQ0ZA7Bm5UrZdX%2Bhp%2B3md81%2BK9btskUuBECdTg1A4%2FNgRcHC1eBk1DoNQbjSEnEYt0zx7jTqOKcV71fG4xz%2BaHD18ZwwteNlbVVe8DL05CJtWeLpi1rkIc0pcwJ8sezZr7YS%2FtRG875WH0mMI9Im4EAQ045COsQcVefWCJ3pmCEiK5gqLAci%2BaXMd0Y6L6URYgmpNNDA%2Byn4YsXz5%2FvdzdmYeu4Xfh8KneYNqNgZlRVAS%2BypzWOXL4g3yTefMR4MdZEPUGGjqD6VP8s%2BnpPuVCSEq3pKKIyYw3G8qtaSN79wQA8cylGIWAZyI2%2B9RL7STST28SHl6ZYnUOvRI9oE1Z1%2BuVYKdbqFdxOg5f3Bx555rT9mjcel1ip3mJLt0SddJsjMP%2FBlNgGOqUBhHjJ92l2YtdGJvQvOVxb9N0vNOch5JuNz0RyObDcQlR2Oa82cgCMHfJXX%2BA1iu%2FlGdPZ7XT1oDoN5XgP9yvJI4FWVbky1ZwMZ6rUYSrWvjlr4kaFpB1tgawYTuXE1ZuxjDhhWm%2FSr8KjplSgcAGERnpkluJNYsERXMurTLkYpJXKpUxdkiMTbEGKo3q0358iuD9booN%2B3frKOmWevQ%2Baqq8nkTmW&X-Amz-Signature=050432d74d4fba068191d334ed546c81dd84a0ff6f7350053baa36892f98dfb3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
