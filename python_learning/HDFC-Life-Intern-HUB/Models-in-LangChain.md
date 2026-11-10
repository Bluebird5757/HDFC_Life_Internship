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
fetched_at: '2026-11-10T05:10:55.923Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2GAX446%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCz%2BpR9jyROdKm2dPP0iLp%2FQeAYv5s8n4eBfH%2BM3rvQYQIgI1IVn1CFGryCm%2BFi84cFEY%2FWNndAD5taWIvwktJuR7Eq%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDL0HMQmL50B%2FFZ022yrcA1Yd0Zey5qqT7MIcvox5LdkYeDFlS%2Bm5gw5mOvD3fpoigSm3UVQNo6MRLuIVWTKP3NvFe9shzquYG2eowVGkkmxLNm0vlG7uUJwY3Kv3mc9tz31wOYegEn5dvYMMYV2AUq7mSg1Cfw0VUZ1DwzEjkbQwHijuIVK17xv%2FsLOIvdfvdi%2F5QUe%2Bd2hA8y3CJb%2FQLfaMcKyiOeQ%2F91z%2BeWOPFzd1OfaevqnrxTJm9y8k4yzkvwEYqGlAYVh4G1Zs2Ietyn%2BbdVsjEGHdkTiSswRhpLqrezcykH6JAtXOoInC4rRVWEdFXyeDesAOz%2BFZ8ZL%2FZ4QkBZRnPVP8kwQrOORYzBX1qwRdwThs21uge%2FIM6T47f%2B%2F1hNKVMV83woBfsm3GSIe%2BzHdD9B3xMTHjd4k9v5v36%2FGT8jJk2%2F%2B6NOxT9EXI3rgpJmhdAbKoUr5wE%2BmsOAPI6NG9y8sViq%2FTBmNMlZx7g7vN%2F%2Fywc4vimnJEUIxxCF5MRWAMrF2Fmotew81PyC%2BsYXbzhIjBG601FwTRxSTzCE13d5nM%2FqdmST12NBX9tQN6QfbzoL6nnIjmZ%2FG1PsFJRpeeU0IyON8IFqUNELrUR1e19gsUL75OQLDDQ4GeEH3dIjCCarRHBvkqMLW5ydcGOqUBF4vH6Q86MyagIWgLPU11Psabkm4B%2FY9biDUcdCr4n5mpI2x9eQoElm8cQ5WQwF97RfLzcYfdA3JbA7KGz9m6otXCOKyKD4tFrAM1ZwjEg8G%2Bi7RHCPqfFSBY6cwdRAQy%2F6OqDGE%2B%2BQuHlEa7MEuxvzEIvvYSmLX6vJDA8J9f3Jsu3%2BbgmEAlhH%2FjCXqhZ0zaDvG9ZIKWwi1IRBSbYV%2FQFnXwS0oS&X-Amz-Signature=350cc80d84915c3ec221f7a5ab8f1caf13cc6d267ca3f463e3d5968fa38bab61&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
