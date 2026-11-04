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
fetched_at: '2026-11-04T03:52:49.856Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VN6GSKHL%2F20261104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261104T035244Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNbOaLtt6UbZa0xDeQrmbBVndVM%2BXLoy%2BnfIFcGSMruQIhAPr9k05sTSa%2F6soy9Jj1LSmv4Hj5dJ%2FjqCgeskwYYkkVKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyaskDtMwDanME%2F09cq3AP56mY2kQ94wnD3Op0j1pISITIMVDtIM0ZJ6bAmvClBAr7nXjmSdB6heJmPC5XeYWJrb3CLzh0a9NDxxFD8wEjfSQhKBLOzmyv76i%2FkqQt57LFrmt6Q4T2WQIKH88t4Ou9B3H9xgI4WDOFZFlv2Ga5NxxSfHuBBr%2Fnh9He6q%2FNq9Eyb9b8GGSwvebMmXLbF482Bit1Rmdizltb8plXxu3AYmyj9w3nCoLOcHhAEGJPl2E1fDdnnYmrkx2vPFqiFIPMWOxixKrXRsEDM0KwG%2BbN0%2BVUkJl0VZ7cEAM6KIUF20oVEDKVS3p237JR584g1X7MTeE%2FYjWMerOqT9xA7jthk792W4HW5jXinRAz73Y7wD0NoRA%2Bxr4gL0M28MbcCeIikqoBkbzerJnfFlO%2FRCjlN%2BNmDtIupHFoSIUwX1D2H60k2CyfU%2FCegbBJ%2Fe3k68zIRHYgR%2Ffv5KyBdvTkatrczC9%2Bh3hftOnz4ww5R0tVhPjIBIkTuX4qQAcMb9uYwgtF1it%2BlHUQXgwul50jbpto00i5tDMIKcYDBwARTaPITNxwX9y2rgO4X%2BNVYE%2FP%2FZT9K8%2FBrt7uvJqT0048P9n9LaH1JGriblkUbGWCAicPavCE0kVUk4ize93%2BlRjDUrarXBjqkAddlyOrWZuhf8nOirv0qM0k0j2oD6r5h6G6K1DosHzO7xNplGZpLZ5LLSoe6frFq3hIPx7t9TzIAmNCL0hV1x3f1JG7xeKtnKSEdT6tc%2B4hckb0upfXwT%2BdzyVtFsk%2FcPZY6HQlpjrLaGSiYF04DP98K4wY8GFKwdyWdjIYsWzI9FSwW7ZzxxJBma%2F07qt3jYdrbGxqIMInYj5LzDagN8ejUpGT3&X-Amz-Signature=3b2d80422a3da72b2430d1fdb0081c089b1682e2753d248ef744b2cae3bddd18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
