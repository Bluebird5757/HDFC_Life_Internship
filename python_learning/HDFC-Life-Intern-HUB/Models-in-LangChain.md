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
fetched_at: '2026-10-05T02:59:34.639Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662XBQHEAN%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025930Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQCirT5tMUEMhsYl%2B9DtBh3GED5kgOXFnKCGxhH8OgllYgIhAKp0Mh2QpkGwG2PJFojLCyE0SVk%2BkpfBxMlpiBZCarbUKogECNL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyInqM4Zll%2BDF4XGm8q3ANERbRCiVv3kxrIhkTb%2FFqdFtq0ZL3QZCL2%2F22q4AE0Q%2B9tDzFO%2FmmAVEDHwVuOKwUvDJsBako9PICe3UVwngdxg6xwIOdA7pTy1ON0lTj%2BoWRh7RQ6fICVhjks4W02pSPizfA6xnlBoK5yxLxgQzBi85oyNfariVDX7iWmaPG7bxSthY7znY1N%2BEUD8maQd18a3IyGXWDTDdsB1zd%2FzioYxT2soy908JyKZ%2FzafxLWm6r0NqkZXeutjZhV1ZYeKxCziPG9YXTzN3p9tLDfJhdgkd4HG83IGfQniUo%2Fr6YiCWdAAlli3RlREQCC6G%2Bp%2F0gyN6qPBB7JbNs5WBxnd1jwx1fCpONs%2F6B%2BzdHPVAx3FSi7IfWO%2F%2FI1216j0azBW2Gmk6TjZ7qVAqtXrD2OrAr48EZ5ZvgWAus2vfS4DwONQq6EowNp6NWdmiI8LGUiX83nKiiovLWGD%2Fpa03cE0K8zOJhjLVdOE3wZxW%2Bm5pVl%2FzscB7aWObbQj7yKEvu3KgriuFE6G52X8z2ij7bov%2Bs3blt1yuafPdyhuLIieIOBTOnuXa%2BsUsPUJh8GbmV%2Bzjrp4xTf9jqONvL57SH2GtsI36%2BqPlSSeSaBF%2BMT5s3JFVbRqkFdvJLMU52NeTDZ%2F4vWBjqkAXUq48kTH0T1CqoBhJ%2BIg8s78FvV0dx0n7cYPmNLYdDanGSy3kwgk%2Bk%2B%2BkX0M5Q7NelQT16Jwt7CZ0drAuGQrE5eo7ZnzHG1sF3ZOS1GctEKECA%2BRRBxnkN4UpZBIS09kxod3yac4Tmo2RzGRXdEJvYKq5EkkYlFeRVUHurnpA5Y%2FZm%2BgQ%2FhczIROIsx%2B%2FseBQJEDNiEsMpKFHp%2FMlCO7JMnM0NX&X-Amz-Signature=622ea7ed71c723d6eda947a9c99e2e5b9426df25c35cf446d519d0986ffe1763&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
