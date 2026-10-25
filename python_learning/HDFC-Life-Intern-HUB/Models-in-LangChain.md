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
fetched_at: '2026-10-25T02:15:59.418Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HI3LB67%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021556Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHHxBMwaSHTsR%2BRR1ZhPfrsk5M8Nu2uP5Xp5FDfy%2FxFJAiEA78ECDGq1D1jBmv6n54w9C3s71d%2F1Ne%2BH2wPdgZsixN4qiAQIrv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPLKAODtQI23sJU2MircA6evCS1ZMKyARVdlWXwKZdUcp037IZcXxx%2BPmapi1B6g9Zz%2Fg6p0JvRuH4473oiIz70l%2BAJbq4oQ6EyktwVJi7LroNw8VjfkGts36DIFFrNPT57oBQxO8tl1pV7lHGXS68NS7FEYLtvTcBQ88frnjmPjE%2BYLuyaxh%2FLtnA%2Bg9ubIWfTfn1f6JBhgnVkUrHDhcYKVZ%2Bf4P1shCr0DhCTiuQQr6M9Oj59uuilwa9JVNBMl%2FtNyTzIAJmEEgse2pW3Lsv84kWbPoTY4d%2F5ClkvCYupHCK%2FnKYW1jwoMtAnaI%2FuiwqDDEqC3ThbG824CbWEEhq8OivgGzL4EQWI46lJzp2zL9GPQUBPJ%2BdmSoCNxb9Wwl5LK%2BXm1ItRO1MZddNj%2FVX9W4Jmh1jZzK5KLdugNvWHSSoEd9vO7uzdxbnWQE1JE0%2FxAegm8ruDAi%2Bc2hwXv8jr5n6eTeI7dge0xwNR02TslMONAChrETKmoiFf6D1BIx3GfvouRN9Vs4GB4dGMNd%2Fd7C5mCmguywKzDfGa0%2B8XElD5XiArEQKiYR%2F3%2FQbHW8q5Y%2FLjLjv3qVdk3KaQT43N25URCnv8oiQrqayGS%2BK8ybG8H9XJaXfB2kfB5KTilUXIZuiIz5cSTZMX5MJTU9NYGOqUBemJZxYoRdrwfTjWyR845qD8dSNIc4%2FVqmvY1zEbp5fa45KRUClf7CDdYo3aBebaDqWPxmctietZu3hIFvQQRewH00ECNtOCfUoHnTQ27WjBr9bD2sNKVAPJJM6ci4YwXlKwLgZhG%2FHkD9BDvzL3vraJdhmV37cS2I%2Frns36LE5ppGSO2HjNP775re9VoCXKg%2BFHavOY9QsIcG6W6psqQANGidGjI&X-Amz-Signature=c1247163988172a75b487b982c1c7cd0168c8f89dafa5e067d3731437579245a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
