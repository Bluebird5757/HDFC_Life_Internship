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
fetched_at: '2026-11-02T03:21:09.324Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q7PXE34G%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCnQK4NSU5r2%2BvAU4FBesgZHptzSkIc9SFC8YA01P3vqQIgY8jUieutNmuN1FwAb3TNWMuFWbIKgH6v2FTeorarKUEq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDMJUbro0c0XRsV2ImSrcAyX3b7KLSaoh%2BTUKoxfvDvNbJaU%2BhfQIdJhhODLtB5dQYVRUbG3el7peOqpRrThFjl33C6Lw2xqOJpzkvIsyil0G5thZ4sjzp2qBPooq3hlHptZUFq%2FoeUbWtn7pYkCcipgErj5fDD3GSR73CD77Uft1p%2FVlWE447HYOhsr0%2FGN9AtdRPxOhgDFHCBW3kvVxx1nx7vwOI530mRtacojwPZFerrKU%2FkQX8mLM0JGGkrqVA9P30X4Pk4wsoW4Cj%2FGnq1FNS1X%2BBan%2BR5DWzPepM2KcOdOGHblL1rUNrmrKKS4Obe0EJq0qlCTyODUtC5kc2UNqbHAqorfApMqWsxVrrBFwFOINjov8lqgZQS7phwi5zO0Cq994gbJiUC%2B43Nax00MDXHqsVFvBhY0%2FDYo2bwlOSLeHx%2B2BJ1ERvWjFqDYY9dW2Kf%2B5UZVtDuWZXZScJscnfS0s0Zj%2FluJNI%2B5VzNEHFp16iJlBluIaV%2FQPhzOCCSTRs3EIit7rer9yLg620%2FDsXJ%2Bli2ZjdovKyKNtLGpXDr8%2B8Z9pRYC9o7HC5jQVcIOuoR%2BeDeDFBn6uS7kyiFzV46CMVDcUmhL6XpttEqij%2BNxqPvX8ysbIbMnwpHkxVH%2BkEe59a4oPZ7pgMK36n9cGOqUB9EnJQPO3%2BoG8s8NxuoZ8tkoN5jDu1j%2FVROggFKXJioO1MxoKTPHbQ7BL7G3lJVGbphsNnbMslAbV6b2tqolnCTQqBNpb3C3O30h7SNj8YNA0DxOyghVE%2BF9kRzw5b8GbKndhkNyOnZGLxsM%2FC51nQ%2FzO6CzTvtWW%2Bp1HnMYmmF1iPWE9LwEuluEjVSkfdLz%2FSODVEqZZ40XxaY%2F2c5BNOpiHaCi%2F&X-Amz-Signature=0b2096b092d8d636b17b72bad72dac0110790b0cd827d83b16cb1dab4a9459bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
