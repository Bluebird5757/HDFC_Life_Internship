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
fetched_at: '2026-11-23T04:43:06.373Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UYWAFIP%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044301Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCNdE%2FuuKAcYRXOPUReISZS8AnKHR7G2yIuwnb9HkZadwIgMzkzmhZ9gnhfhgu03Bnvium7JlQUcTGJSFfYOYR0J2kq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDKs2UkT2CnFREWVHwSrcAx9PgXZjcuSH784J%2Fld4iIyIz0cpiWOjclrewwlaANH1slFBNnePnQwr7UM4C%2BpqdAuCrgHmIi%2BQxOu2yoieRqTkuxBIme8hnPAAOz9y1J1huaKPk72A4JB3kOTJrUnNMgxVR%2FjinfJISSjxkLPn8i3OEC3pMbeV0A9axTLUfGI0t9CjSyiVYld1PTmHgenmltQdausxRIyTnlLWQ4opSy%2F9JlMgErpZJ9vfysrRL%2F0ThhhVP%2F70KmEMyAmNqkO7pvFQlzDJMEbj6GTvs%2Fp%2BnCihOBovkEBaZPLOs4cQzj%2FXUiw58%2FvzfdL%2F8wlepCUS7AuSRY34VNP7kqLXRJnNZQoMy%2BcfwVvFCYMnQRPKO2A6NiXvCmdoibKlgMeZQC9pKZYxqQd5WgLdqBUVf8nbkmUoePoJBuYrag2flMbkF%2BkO17ULLDtvledis5EtMJcoT9cQ9E7L9A5MNGVph6bZi4VvlMSQkmg2Hinbn5kaXeIiXrLjEgF8iEbC6cvsKqhVpChin%2BIh3fG9t5N%2BLMf3gXmLodQYxzjtP9EOrUcInrNvZvfed%2FO8yX6xcB86YqPwcymtraumcc7aw5odSRkg5rGIBW11VbzQCXb1jqV5AY3JQl2WhgiwPgQvbe%2FFMJb7jtgGOqUBd7EccwEOreQNqM8C1ph6rdbCkzdwU5Yo6R5Gl4Sg3pBdVs75%2BndP796bs8L3P9HlU7F0evceBpQtVDHOMKilav2thBAptEfp%2BwECLRabefhMV6GUIk8WtrsgR3POOkANARKyRBp5FxXOPUOYWorPyBNbwenBQc%2BiL0fqQV98UV%2Bwj6q8rkfsqmXqncp%2B4KKImPkxSBf19oTDbpk3SH1GqP7lZZ1F&X-Amz-Signature=f8fe4a25c3021ecf56474c0130ed7111f1664c63774d481357a9e8f72dfe92b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
