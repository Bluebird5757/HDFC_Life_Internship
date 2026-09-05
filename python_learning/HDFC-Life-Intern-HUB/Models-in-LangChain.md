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
fetched_at: '2026-09-05T01:58:09.236Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q75FGY5T%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015803Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJGMEQCIGS9mKdG9waqkt97mKwvfbsr30dyUFa%2F6XypG%2Bm9E%2BdGAiBODanttpvqkpYedoK8qjKtmqV0jfp5n6pI01rxajRNKCr%2FAwgDEAAaDDYzNzQyMzE4MzgwNSIMI7CiSi%2BkTm9%2B04HhKtwDUqdtEcyZXzT5o6B5W26arJW8v%2BTP84qHfqGyl2QbZUwNNoU7q7latqOcPSLB38g2ujPr5LExUNby8IHSu5t0auDKlP9hYFt7%2FnQMNR9y0NndDqtcK2ZdI7HfTwL5nBoT%2F5qO%2FN62FwtW9UNDvI3NmkhrfRzupdEv9oGScmmWzCqQ6nHKw3qjQHnW1XC3yBBkS5n77N6C6%2B%2FtHLfLAL3heBGM%2FxEqPlGaCfadSE5QxAegF4u7u9yTt%2FxWqSZ2xyTqU8jzCB7PXvydXqHxELG05mXNl%2FNG5V1Zh2dC4%2BvAIAXCTrJHxqIo8q70jwbUVWiDLhJSfb%2Fc9ktd7Ff2dPAGzg0Pcr%2BLzNrPa9kBEJi1GYKAHrCARPuPRb1AH9jd3AIXvGgimdTzZBIsx1ZQ8X7mECRlXUti2Rmzm5%2FeYSB3dMkNyL6SghMU7yx%2FcWaWKG7tlzI%2BxIV4tzxgqk%2BAehoYKAbppk24NCgkoqQMYM%2F7N3CjXab4uuygvGFog2wXtzBjMXDu8uCXbCOattWAfXbV1vL0bPvzc7QKyWxE5BGgfkbkWvjxPtHzhZ9P4SJxa4RRrqVAtHdYOEduCME6gsi89vl2YAUYPodoTb9lBQRZolzb2CzDM2hhYceSeG4wpOjt1AY6pgEzms033VJ7O1r4kHmB28w6XKgIJeOoIzd3x5Y0idBWPr6nIZYwp4gzy6uuFQc9SCovoOY%2B7NU5gZS5kh%2FOGpqg0ucqFXgHj8DLEqq%2FuMVzF3elXRQWZ1RfTmDVmHyrDDCjoxdmK5SdYJdE2NXYfChJmcxEwPfcwtEzyktrilZgz%2BKQktPbaXdex9C59xJAY0%2B0YUvTNgzS%2BL3RovQOYKaRyZT0fTqe&X-Amz-Signature=43110c0c86f8f1d618714ad474f55b8c1bd0bd93d0cd2b00e535e1520933c4a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
