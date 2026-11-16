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
fetched_at: '2026-11-16T03:47:11.776Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROFFOGGU%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034707Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG1zVkOrY6xOwpMV7omw1ZjwcOC9WBXXd1r1SH%2BZTfwVAiAoHm6NBUBuLU%2FCPa9N%2BThBajdaCoI%2FYojRzH8RQ4Fi9yqIBAjD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMRrGAqkeQgRIP0WVaKtwDe6yhYS3Iiqs0EBgVYcAa2%2BWcgYTxE5mZ8WvYjaEOM6RPgThNNij%2BkZmQ4KCcFytbxwd8CutmUS9DHH4gZOZjTXLEo2HXl6iGHQjCxMaQQMSOEMTvmU5jDcD2uHIkPeVvdIKJAPc7UTeaPjrr87ykxER6pSf7LbToyr%2BcTCZpMIAh1u6zzIndYNvMqE0QPnUm5t1z81umSN48s5i1lvyVRG41Y66rTAUOlr7%2B6e2hzZ4R9jQtJ2A3eEFzoZ%2BgVaVz33abWCoB0I8AKl8UFSQORhHt2%2FzFP4dykW%2B7x4Mr8zyk5uBdlWyb8f3%2B0ttK4B3UMWpgazPqAV4nX3oIcp7VKYCH1gJH1FSrR%2Bhc6gGmiRU1i1Sb7QZOo37fhSEWn%2BgDFMIoL1Z9x%2F3U1kqQo56yZ36E%2F8gGGGgJ6hOGpWGHxBUowIOWdQCJruRPrHHcQLb6bV1%2FSnk1HUFm9YzAr9C9jR%2Fhpsxi3y3Nd79GW25fA2arr8rth4PW0twKXrLFtq9%2Fugugaorh5jn%2B6FyXgGw%2FUn%2F7I6aZMSa5Jf1FLdg3a5GHbok7jNofQYeFdVpBEQo2HQ%2BX%2FTj4n079UShcoEOBRlvnMLzhnKnau2kP9ZlcSl48yyhUy%2Btqoa2PatEwqczp1wY6pgFFMbRzCC8m%2BtokIZ6dYZQRFReJ22lrFdTV8fhhfQcTuSnaoxLWKR3PPj%2Fot3lKO%2B0sspVZ1yPFAtolUZQ2zJp6pVYD4nHA8R7YwnYPOVMUfbUz1rn%2FA%2BYqyWCNsSTu%2Fvh1wmQtPBPlv11nt1GECzuTZ69UThPpSMmic8px4I71jFH2ua8cF%2BKfk3lHX2OYb%2FnTPeNMA30sg0ngScq9MafrgceDaqmh&X-Amz-Signature=6c7920ed5e11a67fd8aa5023e0159cf18496239fae11d4618f865a731686bd90&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
