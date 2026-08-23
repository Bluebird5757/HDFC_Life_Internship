---
notion_id: 3c44fa76-9938-80f9-aa74-d8168b43904d
notion_url: https://app.notion.com/p/Models-in-LangChain-3c44fa76993880f9aa74d8168b43904d
title: Models in LangChain
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-23T09:05:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-08-23T11:32:07.058Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

## Language Models


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/e4bf0e3b-35cf-4696-b5f5-4630dee93204/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46652FC52W6%2F20260823%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260823T113203Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIQDHK1d2lNN02jBQID7gPNcYtxSR3ODYXjb7EUkT%2Bi%2BW3AIgVYbYvlgh5n3NYUKvVlmfK8rcEQKt%2B0MXBU6IVwo5brcqiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOKV4g8k0A%2BSc4EKJSrcA6XThKN0gwLErvWl5W1W71LYYhBnb9Qs1J91w03ImaSbAAnfB%2Bs95CU%2Fwwp9Q%2BQzJB7DSSqtVqF4%2F5n79LwWzZA2xN9PX7Ch%2FzjENfjVjm3oSlJXWVI%2BQeraScYbRhF2H2Krg9fdPU2YUwRVewob30Uo98APcvtk1gLQhZ4E1fjrwb4UhpepRv%2FVV4qGygmbauSI6aDsDBCZoDxOD3Mf%2FHcpXmMHwXgNg8Tpyj2hTY0IJ6TUQVtLn7m59EHyvBPceBwXEsyQ1kuXwjQ%2F4AjpSyh79y7jvYJ8NQdYeA6XKRSNACw4%2Br4sD747RRcigFrx67ylLvaaRdspsQMZNnTSFAyeKaWyaP6Z79TW6me%2B1Y0BuP3hOIOzsaV6KGh541EMSOdDBS31bGYsJqwWwhsnaDusX0Qh51%2Bh%2FflTbh85nEykLVjUlsbh2cdrqjCAY3nTTg%2FeRAgomAecIXBKwZV3rV41ObZuVN0nrqZk0%2B0S0z2fp4B7V5FZqjyOpbOriLvsPEJ%2BM2q7yJoZZRyrzJFroQGQ8f8xJzzj5u5%2FBFtAV4MikZVkaPlNEWutezzzRijQsG7yhrY3Vw4D88AyCV%2Fgf3qjMy20494u0XWGw%2BGye97JaUnoScEHXlT%2FF2wmMK%2F3qtQGOqUB5lm54J%2FLrriRn8YQKqFzENc7ilQUYhLIw8%2BhPAOn%2BtUmgtnCA9kQAozMrW4cwoYmFKpqdmJoneo6tW3VXp3YiZtVVa%2BHYSs8lYqNJvV0UXNmiXUdYRuA2jnu3lJAToHLWH191wGU3qC4D2uign9Pxp4US33mUlunmezEluqTY%2FWkSxd5%2Fpfm4nN3VwzJWkyOO%2F3B5rFiFiAf8vHCqwwaJYlvCmZL&X-Amz-Signature=0383a4f2128a7e2331d975ec30e8aebd3cfa555e72ca12119da7cd9474cc6d3c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
