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
fetched_at: '2026-10-30T03:29:26.185Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XOHBPGUX%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032920Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJHMEUCICe6SunGrf9GizUMr%2FpdXO%2F9g8go%2FceVuWF%2FsSxv356hAiEA%2BrvVWU3RQbqdW%2FHQgU3sIgScFxrNW84BBh3rzEBuxccq%2FwMIKhAAGgw2Mzc0MjMxODM4MDUiDIo6fbMkctGEv4OYwCrcAyhZedAbzaBv%2BjS2ppITyZ50NaNYEj5w3xWQ9e%2Ba%2FximDxB6HYTRvTnGsu8qgH6m3Y83jJv04nrPsRDBCZQGOp0vbTNg%2BGdjQqNYwz57Pp80dsjmfVSk2MtD2w5USw%2FjEpasngdT5Bmb53slouIAOEBs2lapcDXfBaFCe4oQVUjQgx6GgKt%2BGNOP9Flh4NWH7%2FB%2BgTniAz4zQqL8vl6s%2BDOdf3vWZ6p%2FY7vrOO0t5FGkkG8wu%2B5c4vixzM2cSoFVb%2B2CUIH%2FY0El96shMmJ1Ihh1lO0YBj5%2FWKHqZ8J52PbC29bhR8WhsxAgmh3L%2FUSN%2FARajmJ2OIm62wjJzTdDcoHVxFJgRJK3I82dYztmNdsAq34DyavMEqv9caHR4l5GPMFCf3vh8qgSrGmVAbviXjO7d7q8w%2BBw3B4le5ifudqthv39q5rrgdG307zUVRPP86A51lVdiQXm5%2B7E2Oi2DIYVHD7JiPQQbfmagSvxnTrcH1xZFU0xoGyIsK3AV7l4UDdbvyzZlnVFgX8K5nf8Y57CTXPshcvaAra%2BXVXvVgSJx4raQOCC0DJYpbEd0f7KqRMHQFkN0hdscWmcPcwFO9nloKFzJP7dFV2I8%2FeoXKc%2F25YmpSesY9SZNXP5MMfNj9cGOqUB%2BmmGLXqUQykymaMbctjSp6v%2B5MTyAsie66LZQxP5VpRT1Vb03mhSo4gzAunsQKtoPhAJmD1maKeUj%2FCy2IxX1oWTXgJO%2F5WVoVjGwreGcgKZ6%2FErxAERf8FvpjI2wZ1VGo8J3G88toCLqYCvR7qNTUV4fhvLCTz25OY2RhayOaU2y0kz4C0AnYfKZvM%2FeGwuGAoLUP1jwfZCKdSmVckt0As5XFOL&X-Amz-Signature=1a7673c4014f31ebb7b9aa5b6da6650e184fb31581c3de3b6e7db7f3abf24a8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
