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
fetched_at: '2026-09-27T02:28:52.343Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46625GUIEZ4%2F20260927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260927T022846Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJGMEQCIFwzENKOXg9EXYxfXCO4Voa0e02yxcQi7DGIYXDO5LfoAiAfUtw39GZyC2pyDUGcD2p1lTXexbaXR6d5La2QFu8PZir%2FAwgSEAAaDDYzNzQyMzE4MzgwNSIMFv%2BDvYob6j8FowHOKtwD5icNvNRRbJTUJ%2FT9FWe7SjmqBsTtxCCMVeAW8nfYEHWNzuH96T3VkqYgAMIA%2FGBltx6RcdXwLZJ6ShsQFPiq0Q%2BqJ1AJmKGoKFJKE72Fm%2FM5x%2FEUQi1RgALHJl0YEayea%2FfCXmI8lxfUVa5jMdDzwLMkeRdoO8V5ooDYS6k1Oyj1OXjiu9poSh7AtJAWqfXwUZa%2BOEh6%2FSXKPHc1u8bLd0AdEXhRtgZKbnYenBOacTxr5%2F%2FXOwgdKf2iNxadJLe2fHJtl4M9JmzU44TYtvbvo%2By581sYK54JGwVhPoZEJ3bCDFEPcM56JbPHLPjkEcDkrmpV6BaflpJu6bnmfeqxVy8QAeWcG2KLPOJ9%2FKIzky24KuoxJusEb9oFPKgldvI2BoY1M7mpaZ9GqRMEzeifINSWP4hZNyQMblv4agAmthYNL9abpFVAQtP4Bj6wtXm4FtATZZRxe1ZzOEPVL30lvFm%2BH1aIgIk7gh2aeoxrmhwZp7FJET4j0lLmjcQzOd7Q567MOTikvGlC4cdVzet0yCCloaIz%2BZJf%2F9KTQTHdfB2BOAjrkiTnw5NLCIJOKxcH8cuxehRNlvF8KCPZ%2BEKOcnDnOFtm5cWSzhSMwNj2DRRV24%2BbPyI31oDRJcUwut%2Fh1QY6pgEutfwcC3%2B0%2FNDYC5L%2FAEobh5pi0WDVmSd7L6YusnCydNring85vQi2H2YJvu2eBkKq3LTymI2nXsZmVVFCqCTlvxFzH6a%2Bki8rLkr7K1QTfM3TaKt5xC7Mo8NT6TYqnvTIqbE5VHLRISBkPGYJQWbBwt87IBQSzHUU0auTTQnCyRPNfyuTo2DPMV6ShH3p538J86DtJgmP%2F2AyxZ40YZbpj%2B4SGaQg&X-Amz-Signature=e7a19d49d765102506d0f06d2e2ac4fc92a1fdb14a5e611f61159164a1cea9f2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
