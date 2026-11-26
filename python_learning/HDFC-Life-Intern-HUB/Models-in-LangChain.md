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
fetched_at: '2026-11-26T05:55:47.753Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBCQTNSS%2F20261126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261126T055542Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCCgPDhyiGCFpIunDDNUZAyVkLLZcBC%2Bw%2FXm%2BHiXjvE1wIgJ9epsSHjqiusScFIeY90qkdzVa9X0c8%2BU0wUvEZkyjUqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI%2B0SHJYPTD%2BWwLU6yrcAwM6CnTL%2BAoNH8YVmMnlFkhy5ugL7MY8nEgJU4sXAOWRQoShXq3svwRCHq3cWYWk0%2FDhp%2FrbGmKz5HHL3mzbusP%2B9eOR1RrwqL3YpbEKbJoZZHoFknZ7GzBjlxdrGBVZ9QHzwm6meGi4QohzwFjN%2B3W16ZayFGR6gjR0XRA%2Bw5D%2BSOUygDx04uOvGN%2BLWLAA5XB3YibNyUeAUToxLcvY6xFf%2B24LbV8%2F24NHIp%2BNVvlBBY9Y%2Fdg9iuv7xIsvhWLwndZbkamJZ9nXevZmuZjWfLo%2BI2Szdl8bKbUIbPaBq3ryOAlAwd%2BasXyLbNsQWWfEkd3Egb%2BmOuyupjXRQ1CSAamxTaRhVgMjPSUN5i3r7bek9Dk5%2BZ%2BqSBchyyZ4Sh2L3jQU6uh4zOH9nG2v%2FikEbvdhPxxzaOJbJgfO42aWRzpvlQ%2BEA2RHwG0qiHecRfm2%2F7rQ%2FBEVit%2BFGpi6DsRbYX8IV8XR%2BbZ3vF5cjDtN9XsVAunWSgL41zbfipd0gNZHw5dLHGjjHx7GuODJoJ%2BVbDneqLm2D62qR3ougXpF1CPt1DldbxVKklngyyt4hDajtT%2BvvLjlsWlcYKO8W0FdZjaepVsj0NYaL2YZx0IdgBlqITrbQEIj%2BhI%2B904JMNfIntgGOqUB670AKeA%2BCDwJgGJ2zwnzeb8TnYr5wcO%2Fr%2FdNtYV6%2FaxeM8%2BwrA4kJG%2BJdPCtCcG%2BeJDpG9esyH5YmIM1I%2FntmVTqQheokYwkL%2BRpSMRHps0VJC4bJsDlLJ2OehikT4HJwMTTxaAWWFYsTwZgq4m%2B%2FqLS86KlE74AGy4J5VkqkE5R06wWeEaJZMZXzByLsiGCj3vH9PKuNkMbcvAbxs%2Bg9t%2B%2FwYtU&X-Amz-Signature=3fee2d09d9d48c6e778605c2d7503685579ffdcda666960082d83c668753da14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
