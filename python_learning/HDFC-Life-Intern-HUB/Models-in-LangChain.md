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
fetched_at: '2026-11-13T04:03:40.086Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664YZYV5U6%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040335Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOpu3wH%2FxvFGyFsNQvzVdz%2B281cE8I9q%2BZmqNTJAvUpAIgIhyAsCjYVFJpMAiPpcbTfyZOK41HhKiNHBAXdXYLONkq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDFSnBRtCqbwovv%2BBiircA8%2FKR3%2BrQRXZuvCSgNYGFwE%2Baxg%2FJqvKGZPg93CQej%2BQOPE4I6ioIg2q7y0N%2BXUe4I6nddVGPuCwmzE6D1Xz5dHpWuO0%2FmmDCPwMXuYN%2FMMQSh7TCWZK8%2F4%2FOHQj285yxRryiHWOq1J7PDf1lh2b3mDnTmHBpfp9MBl5lwIpbekH7hm%2B01uyO7pux%2BLZnyyswl99BZ2HYbcRFyvP1GTZ88HmvxjLzOiALHFRPLPuJZnxYKW3Oz94new%2Fcr2AQ79Out2%2BEDH2B%2BBrbzD82UAgndI1ZzD7CHR%2F8fDRWcmqaM8AmxzT7H%2BI40ix3JlZWVnoGbVFAGOS5lb34%2FTZ89sPDbbfrkqG2O%2BJojo6Yu2sPs7uhAH%2BqRDlTniRM%2BYfxcXyQcnKOKMGnwBJpimXCY6sw1ruRpI6kcz1HKkpmqXTNqF3j5QEKfMRjfu3UzwRA6XkpWgNxRlKniGGbd7lWHMxMRASxc%2BEFrxyutGJiOFEtvlFuZMEOg3YSNBNPHavWgXBzkjyYr3crOH%2F2G4hcgPhHqM24KT5CCW%2FEBEYpVMBmL52BZiUiNB73p%2BX%2BXpoAarB6RAhrUmygo%2FfAjVsINOT%2Fw4pOp48XBRwxkHK5ROIHCK8EJiR9uF6fmFPboqVMKPX2dcGOqUBWB6rJ3FckgOLThBsgAxHrTBWl95O2H1051bhAv3V2WYvZN184zEMWkS4fT6wtXH74ZyosQstTM%2Bf6hrC46ffoiJj2kdpexm9mU9BCMBUoagEZhcwoaEmBKZ3HA5fh3zBN0%2BNjcvB6Yw2FtE1RMF6cHjNAHkg8zuomNEkiPCj6gJg1JPIjQ5lQkCvQHPj3eZ8MxCuFfBAV%2F1GifxcUEGlNOjHt7jv&X-Amz-Signature=a57c3ecc97311cf08feeef8efffa30cf63501f1a271e6a2de37427047955197c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
