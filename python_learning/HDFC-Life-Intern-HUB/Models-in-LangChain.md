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
fetched_at: '2026-10-14T02:40:02.270Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ULQDXQI%2F20261014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261014T023958Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDiQor7%2B8iC8mj8KPZCaB%2BFFAkeSBg3%2FkZRd02fLY0CpAiAM1CjmVl95kcomyvA%2B5pwgEF%2FsEPH5jI0gObjhiVcCDSqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAij1LBihpJxC4shiKtwDzG9DQBhA6NCMDBxQpRaqo0mQ9x0xINnvyNfp68BvWPh%2Brpa1hI%2BHZ6dOv%2BbHlEwJOgceJZ6AZyc04mYq3buBOo67SeHeOPbMiTz%2F9CB4Xzm3H%2FSCHrbCKIIv1xy82npuEyheFvgXbqgk6KymsMQA7uyoeOzrFb88GY%2F0RFqorV9SItzOjkJh21eDag%2BZahkXR1mHAwLsH7Uy0wetIgUHx%2B9he%2B3XfjzTDwftyAXbi4Nbh6cPoFOpIbdp5byUE%2FwsK0xo2oa47AFuHIfKo3uVPy6a%2FiG%2FqzGCZT8HIVyLcRRW%2F0%2BEiDXApKBDx2dN26Rjkr%2FG0JOafdqMjO5KBcxMLOzGfmrK%2BWUT14dJfRml5ha82TRKOV1zWEj3uLP8z%2Fvqr6YWPYXyuldbaKTQPS4iwep6W2fbRvQk1HY82Vu5ZiRBqWjHyMb8NfNiUiZaDBsywpyoEXAAAHbSMD%2FoL4b2WYv9Qej0l4mp%2FGmBSN%2BHM64R%2FgNiS57g1pPV0S2Pw4HT4HH4XfFy3NIFUhcIkoORP3zS8mpbXQf2Zo%2BOc8SfVWeg2wWfSOO%2Brk7PHX4G0x5Ui%2FfgvASPkGPDU5h%2FWsVq77n5MzImoTV9mi7BcX8lw%2FFyQMPpJq0QjVdXo9cw%2FrS71gY6pgH58rllOyAOhAw1hYrk768UByiZL1xQ1Z3fCOYXP1CHKHkk2S0kvv%2FSdWj7L2fcMD9TS9u9XZcPSlBLwc4kl2xraC4xNJK5mr7H%2BUQIpNM%2BSEwCIdCI00I%2BO82D6c1rCEyRC6WEtqu4WaEPdr3NwL8NIZOUuQlL1xnVPxB2sww1ojGu9VB5W35cfEYh5jISOU%2FMGIf%2B8GYBDEROGTOJSOx4Np5eoe%2Fd&X-Amz-Signature=96fd472935fe3e1c95721044d31ea3c8bd8d7ce83343a08bbf9407aad3abecdb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
