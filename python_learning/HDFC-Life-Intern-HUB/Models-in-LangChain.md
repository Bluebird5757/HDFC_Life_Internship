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
fetched_at: '2026-09-17T02:23:13.248Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YETPD7HK%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022305Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJIMEYCIQCM5JDh8RitrqThACdKhODI0TGjcWTjBByTMa2iUjYEkAIhAOxeUuJUYUMi40W047P%2BkFgbZn02Oxq5DVZJNjWn6OQpKv8DCCEQABoMNjM3NDIzMTgzODA1Igwc%2Bbpd8P%2FhekUBwGsq3AOd0cYj%2BrdNf7eAzZjwkr52OgbbSvnls3VkWsmBJhRqOTNxlD%2BulZA1RzsaVplgSwENGImc3SeuAJOq8KAXylffovUwDcrEAo4aIReE9Z6%2B2YvwxHi4%2B3yA3ODHqnsR6eDPcikJD%2F62pNo8xyDNU2JyrPCnSXbwWlNVnmVmZS0BFOZXVrrwGw39ZP21%2Fk%2FcVp6jhGpujx57D3gn7B8rLPrGSxuZJYFDMmps01stL1ibT5mhKtsr9aEzQRiBZlJQXaQ50bgc9slxDb1D%2F8R13ItcAVPPWpyrrscareSKuv2DPYacDnmUXKXc2Q%2B9QdCQINgI%2BDhQw7FQsJj8NKiXJkOREf7m2IdcYCFpcyQn%2BuYr%2BHHloXOH6loNRDX8t1rrPS7tf%2Fro5rRW5fmDzw4VxveAEa4FDvQ7V%2Fcg%2FqGoUCutqnAL8lC6z4Nvauwe7jJat1mV5DmSu7GJox1VvIDWSbnO6wvmQAoKkRWpFwxe3xsOokbgP54A03LzJKQFs5UtfeUCBT2CfR%2FWI10ME6wOK2aggZpeGygJGS7AGn%2FDDR0m9ckBYarvrLcNBwQ7PIVlVmuKFkcxCJrm8MXP1eBvpb24AAnJEkolZw2VuJa8oahD%2FA50lBSqnjKgEbsDSDC85KzVBjqkAWpOkppzZbdv9YtRUkovVbky3zP8K9z%2FGzDAlbU3FBJrabiNec3%2Fu7mHUKM37Z5UrZaDFrgrdgDZJhaQ5P18Iai87ruDK1vRsXIiLh4RH0u3DxRYBp3hRP46Veihlik%2FGxgIOOTTJ1FCviJpo%2Fq2sVtyQlZF4eZplyz7BczBW0RGjmU2pyU3%2F5zwG4tNf%2FLv%2BziG24y8BgRqNvg%2BhxdFEdP1UHcN&X-Amz-Signature=7a6fa81deaaad29da8efb33dc4a4dc01eb0bf5c16f2c0bfcf4083c889835425e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
