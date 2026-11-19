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
fetched_at: '2026-11-19T04:59:38.740Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665RET6K5V%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045933Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQDf1PwmKMxgvHdKKfnM9SOvha%2BzA9%2BlaNU717SsLv6z2AIgaD%2F2hTvJQ0wemDZCIfMeSigoKjjXE5jebJr6t3IuQ4sq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDNl17NMv8jQEuOdukyrcA3oUv6q6sz2SAesNUNteObGGeSwHRvvLLIg3ELCTi7HQNfbc5W90DWmKNXEjQYIbZKkcM7SHhWI%2BPI2m7PjoP%2FgdpOs%2FkFFpjB27bwTVS1cd9w0zTjbc7kjh0XcA0wN2Nb59fj%2BR1Dsp8iumvEWY%2FdGqkENlKgrWzX5DFthTmrOrVncX9zQ2o2deZrA94iMZABnMma9BZ8eJiOMyrC1Rj3PZEA4YYxvBxQTq7LFsFzeuDiHd8Z%2Bb0Xm4NDAvGyARPJB2q8zYLIlqTcvvBGuZSllUKQWfC%2BnjOGXrd1a4cWBK4U%2F1i4CNUP1JqKs2yj%2FNlAJfJkUP2yvFvzusrTMkwc2boQlARCx5LTLUqlNZSINjHfB3FUuCrUM2Q%2FgrQk%2BEmpC0GDKE7JIE%2FjS1GP1FNjJx9RPIXL0uxpoAlHOraC8fl4KUKeMSd%2FLKLdLcY4hBeP%2FycXlW7hxDXNz5P9vlbjJNL8kPLZcFxoh0dwdoR%2FpUWw3BOovBTsmOaJWtnBNsd0v4oHahlCypZ8pfstQQEkDwo3h0qPWz9G5i4%2BZq22GrajGa614WVIxRhFBjm26Ed1%2BEMPSyR55BKsgO8lYNscH5rW1hV0V32eczOh8Hkz9fx3a%2FAqR2JPCRmm44MMzu%2BdcGOqUBaPmrecspfPxDcU95aPwVFPdfIN7E3EU%2FotkKLxf5qcDzr3jY3SFtm%2Fxocmv%2B3%2FOUZYOKhnzzQxHbdwnH4ph8hdUhy2RQxH96AAePH8OWldaeDMsfyW6JwcbwmYLf5Oq25S9cZvr%2FiqtK3m7BlE29SqYP0Xcb4KRNkFLhKaHfcaBWpS6vi84VIjSJedddBFM0RkruXXOCDlu55P9HIkN089k9gOWz&X-Amz-Signature=0b25d59d799f64cc7ae034dfbdf92dc5f577a15b04f34f89fd2747537e29c683&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
