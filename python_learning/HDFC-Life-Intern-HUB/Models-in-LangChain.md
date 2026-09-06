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
fetched_at: '2026-09-06T01:53:03.175Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HYXM6BB%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015257Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJIMEYCIQCqv8XpT%2Ffmg9q1Ognj%2FgYbFs6sCoYvu59ScsmgLwzc%2BgIhAKB0emtf%2Fg6fncd039sLpiX5yTaBxFtI3IbAzXNCBGTcKv8DCBoQABoMNjM3NDIzMTgzODA1IgwYsvWQhkMQhLzKUOkq3AMqTY0k3%2FhbXaeFogWq9B3pMe4LBubEkEgICu4b2JXdS%2FRAcYD5Ht9J57J8mpcgrO87r6FPfduYVIPl%2Fr7piRAxo7MXRP95dTNKyOe1RfKkW7xCnrQC22C92Qn9imEZ0GZHUSfMPJpSSDGWoVnjIYKXEB86XEvN8V7DZujJdEX3u%2Fu9545ivYoCos7PEm3V4MYjOBjsdPsn%2FUJv7t9kIF0fX9OJqjfWPO0uN58K7aqmHyDUB%2B852ncTO9h3TPU69fHGCHKCY7LhMH%2FxLV1xwdbl%2BkKvnYgIZ%2FlGD%2FgtOqPi7f3sIJpF8zPylTX9ZLFNzdKKWAOoUpP3UHvuGDKQniRJlGB0js7Xdn3y2C3JsiWNzNGL9Iv5hzovMM8YV5TOFr08EWD7eokOg6BYYYPSTb6p5Jb3gz9CySeT%2B7s1XkNOY5LN2bnUtctDLDA6xBH9GEyIdR3sB6VvTOUe5vML1%2FK8Mw9kf92fI5s4czCprRl5%2BukvgS2XccBW9cOL6LCMu%2BG2g5LEy4vDWHTHh%2B9F2tVeWI1yM1UnLIwkNEoQqO2QAfTrOX49I2ACneHWISQbeyC3My4q%2FTE7liXVugkcYGRmvLYcHSFFEnLqCUPDCCDIhy9%2BMmIj6veZj%2Bj%2BMzDi6fLUBjqkAbZHaMIxE14Ns0jzPTFjjJ3PsEZ0KYCEo5v063UQEaABkbkpTSQC1iP39AfzFhST6dHZlX89wTY38IAVdZwZNYxtCoR7ZJlD01utdof2MVOOfyIuctNXwtvR8hbaBWn2AK%2BE%2BF8MpqNZE%2FxrGY7RGUhzVZbFsocRoiw64auGFVtYZbt498b01ple01Vn6wc837V%2B6ht91lnHEu4VJTa4wih6kHVX&X-Amz-Signature=fc40f28946d6d7f21ba2ded84db716aba327436af01714a5f35f586eaa931030&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
