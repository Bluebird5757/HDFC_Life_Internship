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
fetched_at: '2026-10-04T03:22:37.618Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TIBFGFS%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032231Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICrvwdgWlPH1Zh0F%2FxcWjqBGl9dqB%2BjlHZu%2FzwPr21WFAiEAt5NTeUKoy5EB8n%2B5Clm4jXywIFhyvwojyvk4GPBL9Y0qiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKlBHIVjiCGO7R1cdSrcAxmrV8kdbDtLrAwkn0xlZsMT5yyjUqLNo47M0pPgzAcGNqa%2BGQrLCfnZ2ftLzLaG23wxq7sduN8jVTc7sXEfHgyr7QpAUquHw7v5m%2BjL7bn0%2BH0O%2BKxpD9wCzGtLIYBjmFAHiTz8CFeDRNo0tza04BdpRLdk7h7sQZYbf6MXrriiD7v9sTiaVLqNgF4EJA9Nd05%2BA0z8S9NKCEZpnaF7r%2FRAxEIHPat0WgoufmlPzodWFR0r5Ws0lPvFzHryoxw1IOU%2FnYOZtniIMPs5kgyTK5hvOJw9Aiit5I7%2BbpNA%2BVkst3wxUg%2Bhhmvh3f9MulheDpwAA6TN0gmdQgrYhAOmV6Vv45UwgGsqJnFEzo4kXPq1GsPpKG6o52seht5G9Xhlst%2Bio9kIxAweLhDoDVmmqIccp%2BwNZkt7oXoq4DMVE6v%2FoNRjPHrqlXUGl0%2BDUhQnQ%2BTW%2FSSIzgseeaW%2FDlaWJpjLJDnKsg6M%2BS%2B1S18HzpdStQVGCmvnPJwPp8z9flZ%2BD8yNGh1Z%2F3sAPBprdEXQelSNfVkySihIYJtAFGAhV7a2UIhlCn%2FdgohqGHc5nv0sUPHXQHUmEdXxijdxKHdRtx3m1%2Bv9ZQPpPWytxINGDuq9F1L6Hp3%2BXhC3pfylMIqEh9YGOqUB0eYM0SiI%2BaZwyMUZidTp9GTzkq2TNou2v72HNLRAOijeG7rbUj7KVcf3KucD0OEzAqIhn1vcR3TqitXHoKm9Bbd61COxt6EamZypNGV%2BtsNZ7zK2IXMh1odjM%2FTG%2FRmULDk22DmBGgoFnWtpIH5OQuaDRP4DG9PslXZYg1r%2B1XpnqZHIlSBjOeSbfsSXJOOGnxhU5hTogmbrRyyv9pEhiJbrV24U&X-Amz-Signature=59a15766f84874c30739b01c3cc6de5393b40fb812c02768fa3e1af113c2aa03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
