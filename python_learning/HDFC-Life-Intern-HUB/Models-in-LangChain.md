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
fetched_at: '2026-09-01T02:35:26.779Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SI5QTLHD%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023519Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCCUXGCt3n7GUGbWJ%2BSLtWkmIRG42%2BIQINR%2Fe41tvC%2FXgIhAMXMgTZSqu5NlYshNzULKVYEc1%2BLUtQ%2B8yzLtqKjKJIJKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw2aB4wQzlawiFfDs0q3APTlRreS57hKDelX3hiDSRrHFOajk%2BsiOckJlcnsCNqv5pdgVtrVF7Syahx77FdQtPvreWEU%2FgAKzSEzbHI%2BEXZ197SKwaXkVGpKTgT%2F3AhmKcqFzV5Zg4omYCbJFJs47eZhQ7Mfz2Dt%2Bvreng5DNAjS24sNtpaezPqrrxCWW7QnNj9XKhQsht4emqA2LUVymMjcoklweGeuDo6O7ZwU0lFvcQMxhmg5H6ZBG21NnVvWn9JZU7GENg4qX5Lizn1Z5KAwFE2Kki2a%2FOWMZdmF7D6hv68WlQ5uGxPH%2BPE5kUMhGuqTocT6JQhNNsIL%2Fvp0zBODuV5RCRZpSNCSlhmwq7L0YWUs8lIggSP4rUBOF8c8IKR3vdZ%2FwBqnZ7qsCNr3O4GG0hlSGxtt%2FPEQi4nZrVQTu%2FsyyPinkhsh7E80RulRhyeZea3mpLwBgeafSXv2fyN6Zxr85%2BjaRl6%2BMKSvqlCr97gS9TMbP0V5CF2HQhM3YF5ZO19%2Fr4%2FRIanpMF3R5Ggdn9f7yeHJiK%2BUUHulPzkm9d7UBGvAHPSFAdV1%2Bj324HsBIkJxdmSGMU1BGMfHZe1FmEOeRciR23YUH0W4UX2YLxeF84Gsvz0oDwGFPI5s8Qv3rBhivBrTMzmlDDA29jUBjqkAamqXFMXqBlPfrOCWVNJFfC2gBnR7P4xWg%2Bco%2BrXq%2BA0HTJ%2BA2UafXYoiPtG0qTs94X2n%2BCQ5ossWWKdOKk7vpCdAM%2FcAQiubTiLeMiyKRUTJjWyXt6gHwlNLKVzKy61Dop%2BZvZQL6%2F%2BP3kzSnusxazl4Yj0Y9Ioo5mpHtnLURvoyX2qwU2YcXQoiTg7pgG4xwb2zng6LVuKiHecegAsUBW4tPlg&X-Amz-Signature=766513fde53bdc5e264c47f4180de5fca9c569402533a3bef75229833f5c0000&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
