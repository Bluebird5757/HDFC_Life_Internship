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
fetched_at: '2026-10-27T02:52:39.747Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XN7TRVB6%2F20261027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261027T025235Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQC%2BMx8qSI3BXs%2FD9bMHwzxlwsblODdQtUncJQHxVJezLgIgTU9kupBTxmYrNuRhsZrnnhMXS7izdbyvhaI0MnNr5qYqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIFmreS%2BAJB3LotbyyrcA%2FdLyEKgt%2BRJp5IsZxtBXMJ3gwnKgycxfNa3EEy402snwvllq2fMba6v9wTCF8OvOEP%2Bg8Lhgz7nvUFj%2Fq78wHSzH5IAp0RSG5Wu5NCLeMUWW1vWh969NFQMzSibbNRMwqrnrtlWDU8ljO0MNYlBp%2BhY9Hikh1EspatP4gGfVxEA5ueXyVT017i386sqK%2BoAkzunQioMS%2BJyEyTn8IHSxGsmngAjxVRJGEdAMS32a55o7FBXGN9RAVyUe4YhSLW%2FFeDItykzTb7YA2fKqfO6tSpZbcXxP9K7rom0VfOJJHh6s%2B34N5jcg8RSAQP%2BUv3LrzBpPE5JG%2FX6AHAnLXG0ykInxFn6mawVVo5o4Kj02JmFrR5r%2F3XreA0zRLON9pBCEuEtTWOTZr5dNOSDZmcdqp1EUDqrQ04u1o7hWjUaLcnS9FDAx6Kv87ijdOHqmBAt9dUVOJUhICoZNWIRTEZXhm1GopOrXBoad9qwsCQ69N8pYdH7PwLW%2FuuYbKGa%2F%2B9yaL0U6Pt0ATYifoY%2BnUogDsPKvlmKkklMxv%2BVzUXP%2BBxyNh1yIPCHS6FXjF%2FXBIhVvrY%2B5J1L%2FphX%2F01pOY2e6IWvewCh2SrTDXTjwd%2B6pciUvfQwEzODZHN3yevhMKX5%2F9YGOqUB%2FSakm154OxeH2h9KLron3M3hyMJAX0uL2nBd%2BXKOZ3kITEp4cJgfiblookTaT%2BaqmtdxD88iJkiqFY43LCfBbaZ9uxl6K%2B9WKHyfqWUa9B0%2F7Ze2N93e6tZoMc1RKAT9q7zrjdcMdaSw6iNSrcROKaGKP7Scl4GGyI3C1y05mjCVkv%2F%2Bz1whuHDPRoWj3N28Io%2FCJ%2FCxDYC1H7DcyC3UImj5jCk0&X-Amz-Signature=33e64355f798a5d7e252b94f89d1b8be02b2ede4557eb786ad213fbce72637a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
