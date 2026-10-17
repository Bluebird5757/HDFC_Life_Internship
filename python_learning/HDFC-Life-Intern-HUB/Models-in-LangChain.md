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
fetched_at: '2026-10-17T02:05:45.296Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMCLMA3Y%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020538Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJIMEYCIQCTF96snL3z7YLTp0UZWbRJsOYdKE%2FK7kBb4hCpnpFmlwIhALatKV0a9dGmlBC%2BUAFn9LGvpT4JVAo80Z9LJegW%2F1fQKogECPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwnuQ%2FAb0bYYGvKIFAq3ANnQrmYLZdZT0fEzQ7cJS7IcqtLT9%2BtVDIminX3mBu74SxQHgI5SgVWXzfgmOYt7kfUwkDi59k5LfYDqLxptRZFyca%2BosLQcf9XA1Nee%2BfFoj58QjyMz%2FXdo2e%2BTjGWJRSlT8pjPR%2FuD%2Bou0FcWwsPdKukkh%2BmiffKjaPKPmx7GPmEx5lHATHtsYjVxRHuiTLqXciEOyjTsK%2B3WiazX3jiprrJQsEI5wa2gHOm8ix7rFA1hzNT7oAnjnoht%2BC9D60hWzeTzey%2F14LQaHjVUdfIXWQr%2FIfYYXe6Yz5Im2yRabMNNHIGtV02%2BZKekbbKeRpe%2BiOuyKMdTRk90bkiR3iT1z39%2BA40x3CRNkevBJxjIKAOsGHk2ZhPX7z9Q5NqoUrPV255HxabXBW5NBbWj8ltRauVhGefVihxRvY%2B4ZqmcjTg8QMFFYC0KCIX1uYUc%2FLWArfLAdw4FJfzy1Bb6v85IGMIS3fJylySC3qetSQya7jyZYwS7xpHCQU%2Ftvsg8EHhUjIxp9QJUgDK3gr8OvlkdiC2Yy8e1YAlLyiTLKPARfJEmvyGfRiS3cXd9OU%2BXdOOdrQurqhAE6yLAp%2BYF%2BrBD0Hbsnzitc%2B3WV%2FNxfsS2T8TsTon%2BcdHgf8MgIjDN4srWBjqkAWRvHCJ%2BSpxZ0cvKiCypiZIh7f32tg1Vn5%2BhrCRTV52%2BJfK8IhB0M8TRVUtRrLUlYWh2Qk%2BfRzkDzDcmWFVF2tQKBCGOcLGneUtQW5pKGX3HEN5hPFfDXjxxHB9cqa0PJeaw40rIl%2F9ZL2UXOYe1eyQ1c%2BdfIbM4IK4RNT30Qk%2FHiVm7OPRbpNwBFjmZMO7ZpGjEPOvHgiROPF3NTx%2B5Nc2xOtM6&X-Amz-Signature=2e9ad1391300a5f67dff3dfa13e5b8ec25fe6b50e2c06c5cae4c8d9534ba7e70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
