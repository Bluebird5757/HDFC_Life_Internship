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
fetched_at: '2026-10-26T02:16:19.465Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZMXA5REA%2F20261026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261026T021614Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQCQlxI9H1QS46quKiKKnXClqaM5SqXj1rRmiikomqwHEAIhAOx5NU4oWrrpyxoaaSzJkFsTL3lRjDtDPUCAbxOQswbhKogECMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxQjDxT3a%2BbPoHWz9oq3AOJ9QawRMJFMO37oq2wvfH9luDUGOeRmtF21SmzQwZt6LOqT3C82n2hM1wxebWuEPxt4UbNU0SBTSWPuSDht9B28T0U0hN2bZipgH1imiGGThEJenKQ7eDZNnCW%2BSjFNB5T5Q75Ru2ItVQm75naoTQfQw3M9IJ83d6edV%2FpIHEIiXC9r7yUJmcYREWmKwBqidC6kxp9vPnkSeshjC7XGjQIRgGz%2BTwM%2FAxq39LJRVVtDJ862EHFzU4kOchWZefUc1l91q%2BSqNU1%2B453hquum6cGZvmMXvlFWOsWKOmvjjCHROvXKIb8qzd5Gj69RaQMRxD2MH6dKDxQElSspe32KfDixH8l888BUDaQIyht1Y9555SdDizSUvUgoYROxsVVmGWiqjcLuk0rX3AkecZoZ4e5EkZHxIqy2Htl4XxvTd2zg6FzXaqwjnTZ28OZ0S%2BKkI0MPymaJS78RI7U9Up2ukRI2hPm657ER6JWCUrsXfqbPjp5gXcPBfzx3lvt4k%2FCiL0OfsQi6ShyUWUPnuBdDxR8pMEsnIgnm9iddaVEj35Z2mjlgBJWwinWp4auRl5tEaVm62sALqb0Vf%2FV2PSI%2F3FcwNM81hv072rRQK9E%2Bl3UNaHt6e5%2F57YMO%2Bp6WTDsxfrWBjqkAQXsG2j%2FuDScin3Cw%2BKAq9BejdqE8QMn%2FipO4ErQhGX0l3chItanFdA3JyiByfxT90rQw%2B8r4TPZHSBQY%2FGbMH01l%2FzpnLOWfHJvckRUYMtLLB6hYTSozbX05MQKVPcld7jNLxTlJQXyZnZnOLwGQrH0vyxXW1sgR7jle1%2Fu4ZjoxyVpJhLAmoiosXj7dg42ytsHoHQrUQx7Gb9q7nhUV3SbPJGM&X-Amz-Signature=f592bd699adacb0a47f863db62a082323e3eaa34431f23836fe8fa13a5dc1744&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
