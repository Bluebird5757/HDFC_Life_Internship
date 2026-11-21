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
fetched_at: '2026-11-21T04:45:31.066Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SDUIB64H%2F20261121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261121T044525Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCIQC%2BN3ipBQOCVQ95NlOIbj6TFQWAaHN6yt0rlzRh5ybm5QIgKMfCNUdSJ4oCip%2BxLm32fO4J530F%2B%2FDYTkTUTam2Zvsq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDE7KGWohgA%2FxzCeZGircA0XoP52sWqQuOYg1KwB0ll9KF1m8EazRFDaO6AEjrZ08r4TCCyT1JNMmPCfgpCz0eRUwfJSnhR3HDFbqWSzusGcQdUTHCcCznCRRjfzvs8rm0WxbzAZmCZcZC6KwVpSsJYFrMf%2BjumM9bpeIoW9vkH6Z3p8si84aQo9DUoTr6Y5zHdhIz7MAa5CX1asQy%2F%2BwofbFOWGUMDsUfqaVRavcLuvBalOK5yysjcHXS969DQx6jQrlZLxpxYUPveaGHc9qfIvjsQtc5vAwmao1OhrlBa4ZpvDDRt1HpdJtTFBB6Ba%2BKRfyVNDt9c45oiOOwQ7iGz%2F%2Flv4OQTYZwaH2b21r%2FkOJrdynFSGAGoh6BOWz0rYB4H108ftC9KqMkLGWcN8nzo0mDtHwr9P7yZJZutq8FTqoW8wWlWPm6o9y4ggvcA0tV7Xgq%2FFfo8ujU%2FInKQs1X%2BEZkGZuqeAZsrCOf%2FPQ1HSu%2F2XclI7hJ84AaIiYiCvEVOejYqbSpMq3z6SYIRVpkmQGWZJ7T%2FK4sazjSk62ea%2BKIVteLKQumltwZGlkjbk%2Fh8kRMrPwA6ZrcO%2FypA3PqWgwpR4yHb5HTQQ%2FMRujvpPbp7iE3wiTZBQ4lOhf1N54fx%2FQh3ixhG9BHzDnMLCOhNgGOqUBwzIWp8iQ9dmyIGFcUn3Pf1VaS3WC6hiONi5r6pRkR%2BcXef0zv5uPxwsbVfyQlzf%2FeBxKtSaL5lXZVCPwICYjUYCjGu7zPmLctzlsfEyQo%2B8zgrlXDJJJXTYLwfXpRUoBMhh7XFps4YRmdFIP1z98eS8qqORxoJfSnm1kqjgStSztqK0LNDrFvCRamRetpv8P%2FI3aPfGGwA5BJb9V9H0R2iV1DLN%2F&X-Amz-Signature=a670d7b761884b7e1cdc13a8ff9142c27a88d7c12953523505e7bd9a180ac241&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
