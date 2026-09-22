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
fetched_at: '2026-09-22T02:22:56.750Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ILZDOUK%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022250Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEFndu5ptyRGJKOrM1Olk6JfrMPSWsqzOl8j2jGQl7GrAiB8QOQGpVcfajBEzMgj1GFG64tAW7Pq8%2BHcoKEJXc5XvSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlVHY3WM3UdJ2xTrEKtwDDWO1EUFoIvzT4U9hKJf5Ei%2BiMdOkcD8vRdhUkWnWwr2g5YKvhzotgcxHF%2BJmNJ5aJr%2F4sPw6twx4rGfWENuv3%2BwZ6SyuHcFV3Z2b0dF7x85afTrj7hlvLqyIDbYvTy4jUMljS3IDD3ImH8tcbm%2BZo9kh1%2Bdhl%2FvQ13s3E7csyhA4jxdCno25acUnAXoBk4PGihfwfqk4eXC1z8enOdnQbmLymd0NB2VoGQZYSYTRQLU9NDMBvOcs61ZP%2B2%2Bc9T1zhJg3bHocE9KQUtZZ2tfCapy139KqO%2BFVnV9M8JRv%2F9w1622W0OCWCWq%2F2tI75ax%2F4YoD%2BGJ70OD6UyxDJnMxwmtPWd9zGaZRBR9tC85aBB9X61lL8YrzEanKzLaGUiUe72nExpXazgt3aXDhYSYrCUqOdUdFeOYD5SrMBOB3gQXq4Jsx4EEzRvpjpGnUT9jxATPKmbDz3OM65m%2F37xb9Igl69gFDlXJxO2dL05E8NlcadVgIYwCXEqrMSFmVX9DB7A0gpqcTiaDkNAPC%2FOhWx2CJtS3p%2BzDf1DQ5NuwNjeS9ihpB%2BmkgF2PjcW9c47u3SCB28vxFvVVufEWoWyWH26R8lHOYREShNTmHhWPlB4pCoBe0%2Fg2z5oAqeuAwvqDH1QY6pgF8NeDd4uB7q2Mtx9Oo%2B%2F4lqfG1z7hzQc8pn0GPAPGbZkuIs3O29vxXt019TZULsJ6VPpubd%2FCUr9TNDkoBjF1tbusCsakGlpV3D1DxXSinmujKPEuVZBnlh1R7p0eCMpULhbAKfYxkheYFbNJQpLHXD6pz6XcIaKh%2FI%2FXogvh667e09w0gDRHI9X2WRHVrzRv%2FSPHoQMOBHFJoc8e9r0EFInow4YD6&X-Amz-Signature=c83366a3af963edc012f0bd74f60c2cd68b80b62ba1d885b8f6bdd8180d8cb0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
