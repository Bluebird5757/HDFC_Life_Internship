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
fetched_at: '2026-10-10T03:18:42.381Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664GGM4AO3%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031838Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFGytTIpS3yC71HKVHxF%2Bf7xaPFVzcKCng4fmwWqk9YfAiBD1arOIPReCDIVlN3Kc0OrajniKJHmT1SyVOMaMv9lGCr%2FAwhLEAAaDDYzNzQyMzE4MzgwNSIM03%2FbwZU5UJoAyiirKtwDTQPZuJJZo7pgP0aiTZ4X%2FhKHHv8GLF7SLVJovcSv%2FQGF3mqGL3nPdu6vSpg8OgDYJK4U7Ku0cWd2LXUgOeQQGDiudzeP6LR1kwFC%2BAnkCLuQyx%2FOjN5b41uw8sZj1RWGAq4DRn3E%2BRNNtpspI9dKT%2BeF9hO%2FDVIgtULtPfk0QEA%2FntDvmcQo1v1MbJ53CvKYar13%2FgXqwNW7kP3%2BIk84qLOo%2FxdRQW2NS2gaprRtZBqxIoBbCX6cb7e9eX0XlT%2FQOCyprJ%2BhrJuUnH1y77E38YY8RCbqOzs1Z2CqU9sAjuOtmPU3dBMkg%2BvYUmBdKf12aAZvH%2FeKQIo8LPBvyXDlFlRWJmv9P5QQIfI240xXVlZ0mTudkEVSOdnxkxASbLqTg%2FZHVUbx2sFkH1ks0ix3D46G9wGA0bXlHjOXIVxndG2N%2Fnx0pB%2BkNfjrOlfCa0cy6C1rhze2UtwoexDdSR3pRCt7aUGrkAcjoPdUziEVA%2F7Ynwa3pG3cMtKzN1a4s5F9OCSBWGiKbKi2mE5bSdi1FOymgw4QwlGPYCrV4p%2BDDa7vWVPHkUsJmfrNEokAHd8HUJnCxnlzvt6MA2T5HhUfBIv3HMfCmtioKhIjy5VMLPUVMKXpJyAjKeDK1bIwhMWm1gY6pgGwZvUPAjJoYOL98CqaKZ8sV3XyuqZFsB5zF1M3mnw5V5HUMWbgKB%2FUFaW1mE7z06z7rb7efc%2FC7oD%2BQhc8%2FtsSq%2FEZy86aHf9lHqe8SIw9q5BCx8IUM5p%2FqZFLOYC2n9yPQnpFQVMrXWq0FXzpBKbpT1TX4jK%2Fa5VjlExn4OgO3i6Dr5yXnqd%2BL6UmWK568rUbqftE9Nguqxp0XtPHPEeEfGdKLTuP&X-Amz-Signature=d887c0eb0c1f03395004db5c3149973c4e3b893266194e9aaff868d39ee0e9bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
