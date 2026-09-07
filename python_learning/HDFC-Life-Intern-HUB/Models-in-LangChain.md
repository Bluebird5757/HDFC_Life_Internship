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
fetched_at: '2026-09-07T01:50:19.242Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNJV2M4Z%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015013Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQCEkdak9tIMOOg6unX2nDzBG357e23rOb42M9I7YCTMyQIgZbpb%2FY0VEk9SB4cY9vfTqB48Mz3VyIevrovSBLcGhHcq%2FwMIMhAAGgw2Mzc0MjMxODM4MDUiDK89MUr1inAiKjY1cCrcA1cNhurJ%2BjWROQyK4y6xJGst2jNrfjDkgtIDfQ30VA7mKgtyEp5W%2BvX2KlAqzN8RoqHXnBeozc%2FVsxuay3EV1DNMGvXQ4f0D3900DrRaH5uufYB8qsxiE9hHo9OEDbHvrUPHjDsxdZ5JQN7JA7srAaD%2FEFkkP8%2ByVRZE%2BuXG89cWidigzN%2F%2FPSf0AOkMlHVplXwzxhAsot19wvUbxJsZUM3ooJfAxNzfNvry90uZfP9%2FOY2SlXE7ze%2Bgc06BDB%2BWusvqZuNLL4GuZKGdCizRqdgUaKJBf6KyBmdSrvucll5rSwZoK8FvnjDkSIf3HJyoXAEWdJScL5%2FHbLl%2BEZfbqugJs2Gm55SaaWi4Tnikga4AQGZjjc65V18oPOg3AJyVDtl7hjgT63JYXgULLi4T4Bclsqcl4o7FzIIqNDEV%2BF3Zygxa3ljLCOPIaKD2GJr4guuV5omdlOmhQKBpTDZTPGbOxo8TxC1AhHJlj1iMEH1jzH7W%2F3iQj3PZfgEsrXh6ZDLjvgWQ3SNd3bWIdEfnEW9X0%2Fx9Oubksgby6SJ0ptJlbXCtKwu86z6fNmQ2%2FZvSiosE4rFfja%2BzhD7sJGw%2B0f7NY%2FjtJBzfMnd4jm2VRU7%2Fy%2BmBZxFUXi1O0oTpMPWl%2BNQGOqUByfwUlzPBJqJI3mg%2B1TviK73yE1fmFH09JSnaEPmEplrwa4xV0fCoLdwGehcALO7XAfpWrS9i61ewwlUK8gj3qAMyVOQw%2F7HW%2FC9Z4gh0Aer%2FndHxTep8vG0dWIPNmbYHzN73q%2Ffbm2S06i8mNGGhpWyWHMgCZuXPjFcfhlmPcpM0X1N0gqQQXgjszdCshzpdAgrOlAwvBysH%2Bq1ywKQWxTzztaQr&X-Amz-Signature=7d76993395c015d31f8a8478b38429b77d8ebcaed9be2119f1224863e9fc81ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
