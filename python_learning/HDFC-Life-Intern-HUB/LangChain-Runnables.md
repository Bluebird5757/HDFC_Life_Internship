---
notion_id: 3c84fa76-9938-80fd-a16b-f5c2003af0e6
notion_url: https://app.notion.com/p/LangChain-Runnables-3c84fa76993880fda16bf5c2003af0e6
title: LangChain Runnables
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-26T18:21:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-11-01T03:11:25.109Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

Earlier in LangChain there were only components like models and prompts and retreivers and parsers and document loaders then the Devs thought of making chains where we can send in the components that can are kind of being called in every program but they made so many chains that it became difficult to remember them and that also increased the load so they made runnables which are like LEGO blocks for connecting components or chains and making runnables out of them and if we connect one runnable module with another that is called a runnable.


The code without llm chain:-


```python
import random

class NakliLLM:
    def __init__(self):
        print('LLM Created')

    def predict(self,prompt):

        response=[
            'AI stands for Artificial Intelligence',
            'Delhi is the capital of India'
        ]
        return {'response':random.choice(response)}

class NakliPromptTemplate:
    def __init__(self,template):
        self.template=template

    def format(self,input_dict):
        return self.template.format(**input_dict)

llm=NakliLLM()
template=NakliPromptTemplate(
    template='Write a poem on {topic}'
)
prompt=template.format({'topic':'AI'})
print(llm.predict(prompt))
```


with chain:-


```python
import random

class NakliLLM:
    def __init__(self):
        print('LLM Created')

    def predict(self,prompt):

        response=[
            'AI stands for Artificial Intelligence',
            'Delhi is the capital of India'
        ]
        return {'response':random.choice(response)}

class NakliPromptTemplate:
    def __init__(self,template):
        self.template=template

    def format(self,input_dict):
        return self.template.format(**input_dict)

class NakliLLMChain:
    def __init__(self,template,llm):
        self.template=template
        self.llm=llm

    def run(self,input_data):
        prompt=self.template.format(input_data)
        result = self.llm.predict(prompt)
        return result['response']

llm=NakliLLM()
template=NakliPromptTemplate(
    template='Write a poem on {topic}'
)
chain=NakliLLMChain(template,llm)
print(chain.run({'topic':'run'}))
```


but as you can see this is not standardized and like this we have to create different functions such as run , format , parse and much more for different classes but with the help of runnables or like how they created with one standard method invoke mainly this can be eased out


with runnables:-


```python
import random
from abc import ABC,abstractmethod

class Runnable(ABC):
    @abstractmethod
    def invoke(input_data):
        pass

class NakliLLM(Runnable):
    def __init__(self):
        print('LLM Created')
    def invoke(self,prompt):
        response=[
                    'AI stands for Artificial Intelligence',
                    'Delhi is the capital of India'
                ]
        return {'response':random.choice(response)}

    def predict(self,prompt):
        response=[
            'AI stands for Artificial Intelligence',
            'Delhi is the capital of India'
        ]
        return {'response':random.choice(response)}

class NakliPromptTemplate(Runnable):
    def __init__(self,template):
        self.template=template

    def invoke(self,input_dict):
            return self.template.format(**input_dict)

    def format(self,input_dict):
        return self.template.format(**input_dict)

class NakliParser(Runnable):
    def __init__(self):
        pass

    def invoke(self,input_data):
        return input_data['response']

class RunnableConnector(Runnable):
    def __init__(self,runnable_list):
        self.runnable_list=runnable_list

    def invoke(self,input_data):
        for runnables in self.runnable_list:
            input_data=runnables.invoke(input_data)
        return input_data
    
llm=NakliLLM()
template=NakliPromptTemplate(
    template='Write a poem on {topic}'
)
parser=NakliParser()
chain=RunnableConnector([template,llm,parser])
print(chain.invoke({'topic':'AI'}))
```


now with two chains:-


```python
llm=NakliLLM()
template1=NakliPromptTemplate(
    template='Write a poem on {topic}'
)
template2=NakliPromptTemplate(
    template='Write a summary on {response}'
)
parser=NakliParser()
chain1=RunnableConnector([template1,llm])
chain2=RunnableConnector([template2,llm,parser])
final_chain=RunnableConnector([chain1,chain2])
print(final_chain.invoke({'topic':'AI'}))

# or
# you could have done
chain=RunnableConnector([template1,llm,template2,llm,parser])
print(chain.invoke({'topic':'AI'}))
```


### There are two types of runnables:


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TASESVLO%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCfN%2BEm1OzjimsgHNZ6KhbLf%2FPeG1oZqyXHuHajPb4CggIhAPJadYIk0Y75N3d0Cjk8ObAkW8c88lF276wOCtgiEaq3Kv8DCFgQABoMNjM3NDIzMTgzODA1IgzjXvqKYkyhm0MRQKQq3AMG%2FuDF81Zco945o4z2MtAxF21DgJUqMQ7IhIzJloZA4t58UzRSwJtDyiZwkzylxYB39NqM%2BbDXzOjYGTfPZhVdj2DZnwEraIxQa33O9YH3DuMIYfZgipb65vw0JRO2NNyakmiEbSmdeY%2FLs%2B3I6xMYoIUYn2mmpu1XzoctI4LsBCHWhagTTBcK05ckLKSXOZmZwvRBonHyaxGxImWU8Y25p0DGV5xC7dFifyr8NxJdH8mdudtPGD6xxCXzr4mRLrwrouWrol4LbWJD086xtqbL5UsjZ3tNTSeDAz0XvmTcUIAQSAyFYjxYdU%2B7Cohhcv5MjEoQ%2FI1YNANe6r8UKRh3%2FTgG1O%2FQzIWqMef%2Byg5a7pfxKtrJlJat4Avkf9sbuUNuGZLowxtnn1TPXf%2FBUZXik%2FrH%2BGf8L3v0kaBboB4CNzjiyEmxv26anfYJ%2FG6wqVfaz%2FCPu4o2YDZweLzKsh6NFnHeFvBtNYZeXD7MZwGBWEtuGyLf3I4BHV5lAQRILs6DyyHVxmiDrmFOPbEB3rfUnCa6hNDdy6%2F2p0NlEC3eSHKBskIfxjFPG8OCVAelJ5gDSkuPjLfH0gyFhdLIN7LZJr18iJAvIsFFZzKppXGcqozXg%2FO%2B%2BsqL3gQEKDDt%2FZnXBjqkAa4wF%2FzlS8kUF70x9mgZpx0Bpe%2FXTGKG73NP43byEqB%2BpSnnEeTars6iNhfziHuA1tEgZEDYf5%2Fd6VlBgmiiNjqRHfwHhw%2FxgLVAl7dA8X4BSXzULUajxE3CVUHH5Xtt6vfBj1DI4AJMdpvmC8RSn%2Futtca1Ydh%2BsndalZlSMflmh6ROJ9EqZs7ffLOOSnRUIoY3XejDAl8dg2XTZDOmAIQIi2CW&X-Amz-Signature=d396bc8389fec1a7ea6b6c5748f04880db81a5ea6adf5d2498d571eff3c804a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TASESVLO%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCfN%2BEm1OzjimsgHNZ6KhbLf%2FPeG1oZqyXHuHajPb4CggIhAPJadYIk0Y75N3d0Cjk8ObAkW8c88lF276wOCtgiEaq3Kv8DCFgQABoMNjM3NDIzMTgzODA1IgzjXvqKYkyhm0MRQKQq3AMG%2FuDF81Zco945o4z2MtAxF21DgJUqMQ7IhIzJloZA4t58UzRSwJtDyiZwkzylxYB39NqM%2BbDXzOjYGTfPZhVdj2DZnwEraIxQa33O9YH3DuMIYfZgipb65vw0JRO2NNyakmiEbSmdeY%2FLs%2B3I6xMYoIUYn2mmpu1XzoctI4LsBCHWhagTTBcK05ckLKSXOZmZwvRBonHyaxGxImWU8Y25p0DGV5xC7dFifyr8NxJdH8mdudtPGD6xxCXzr4mRLrwrouWrol4LbWJD086xtqbL5UsjZ3tNTSeDAz0XvmTcUIAQSAyFYjxYdU%2B7Cohhcv5MjEoQ%2FI1YNANe6r8UKRh3%2FTgG1O%2FQzIWqMef%2Byg5a7pfxKtrJlJat4Avkf9sbuUNuGZLowxtnn1TPXf%2FBUZXik%2FrH%2BGf8L3v0kaBboB4CNzjiyEmxv26anfYJ%2FG6wqVfaz%2FCPu4o2YDZweLzKsh6NFnHeFvBtNYZeXD7MZwGBWEtuGyLf3I4BHV5lAQRILs6DyyHVxmiDrmFOPbEB3rfUnCa6hNDdy6%2F2p0NlEC3eSHKBskIfxjFPG8OCVAelJ5gDSkuPjLfH0gyFhdLIN7LZJr18iJAvIsFFZzKppXGcqozXg%2FO%2B%2BsqL3gQEKDDt%2FZnXBjqkAa4wF%2FzlS8kUF70x9mgZpx0Bpe%2FXTGKG73NP43byEqB%2BpSnnEeTars6iNhfziHuA1tEgZEDYf5%2Fd6VlBgmiiNjqRHfwHhw%2FxgLVAl7dA8X4BSXzULUajxE3CVUHH5Xtt6vfBj1DI4AJMdpvmC8RSn%2Futtca1Ydh%2BsndalZlSMflmh6ROJ9EqZs7ffLOOSnRUIoY3XejDAl8dg2XTZDOmAIQIi2CW&X-Amz-Signature=c188a47788864f06bc6e2c56ea56604754cae43b497533d3042ec1c64c1b5eb3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TASESVLO%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCfN%2BEm1OzjimsgHNZ6KhbLf%2FPeG1oZqyXHuHajPb4CggIhAPJadYIk0Y75N3d0Cjk8ObAkW8c88lF276wOCtgiEaq3Kv8DCFgQABoMNjM3NDIzMTgzODA1IgzjXvqKYkyhm0MRQKQq3AMG%2FuDF81Zco945o4z2MtAxF21DgJUqMQ7IhIzJloZA4t58UzRSwJtDyiZwkzylxYB39NqM%2BbDXzOjYGTfPZhVdj2DZnwEraIxQa33O9YH3DuMIYfZgipb65vw0JRO2NNyakmiEbSmdeY%2FLs%2B3I6xMYoIUYn2mmpu1XzoctI4LsBCHWhagTTBcK05ckLKSXOZmZwvRBonHyaxGxImWU8Y25p0DGV5xC7dFifyr8NxJdH8mdudtPGD6xxCXzr4mRLrwrouWrol4LbWJD086xtqbL5UsjZ3tNTSeDAz0XvmTcUIAQSAyFYjxYdU%2B7Cohhcv5MjEoQ%2FI1YNANe6r8UKRh3%2FTgG1O%2FQzIWqMef%2Byg5a7pfxKtrJlJat4Avkf9sbuUNuGZLowxtnn1TPXf%2FBUZXik%2FrH%2BGf8L3v0kaBboB4CNzjiyEmxv26anfYJ%2FG6wqVfaz%2FCPu4o2YDZweLzKsh6NFnHeFvBtNYZeXD7MZwGBWEtuGyLf3I4BHV5lAQRILs6DyyHVxmiDrmFOPbEB3rfUnCa6hNDdy6%2F2p0NlEC3eSHKBskIfxjFPG8OCVAelJ5gDSkuPjLfH0gyFhdLIN7LZJr18iJAvIsFFZzKppXGcqozXg%2FO%2B%2BsqL3gQEKDDt%2FZnXBjqkAa4wF%2FzlS8kUF70x9mgZpx0Bpe%2FXTGKG73NP43byEqB%2BpSnnEeTars6iNhfziHuA1tEgZEDYf5%2Fd6VlBgmiiNjqRHfwHhw%2FxgLVAl7dA8X4BSXzULUajxE3CVUHH5Xtt6vfBj1DI4AJMdpvmC8RSn%2Futtca1Ydh%2BsndalZlSMflmh6ROJ9EqZs7ffLOOSnRUIoY3XejDAl8dg2XTZDOmAIQIi2CW&X-Amz-Signature=cee32b27044b7edcd2d6d43e4400ae202877c2f1e774b4e62041fdf23cf9fa75&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TASESVLO%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCfN%2BEm1OzjimsgHNZ6KhbLf%2FPeG1oZqyXHuHajPb4CggIhAPJadYIk0Y75N3d0Cjk8ObAkW8c88lF276wOCtgiEaq3Kv8DCFgQABoMNjM3NDIzMTgzODA1IgzjXvqKYkyhm0MRQKQq3AMG%2FuDF81Zco945o4z2MtAxF21DgJUqMQ7IhIzJloZA4t58UzRSwJtDyiZwkzylxYB39NqM%2BbDXzOjYGTfPZhVdj2DZnwEraIxQa33O9YH3DuMIYfZgipb65vw0JRO2NNyakmiEbSmdeY%2FLs%2B3I6xMYoIUYn2mmpu1XzoctI4LsBCHWhagTTBcK05ckLKSXOZmZwvRBonHyaxGxImWU8Y25p0DGV5xC7dFifyr8NxJdH8mdudtPGD6xxCXzr4mRLrwrouWrol4LbWJD086xtqbL5UsjZ3tNTSeDAz0XvmTcUIAQSAyFYjxYdU%2B7Cohhcv5MjEoQ%2FI1YNANe6r8UKRh3%2FTgG1O%2FQzIWqMef%2Byg5a7pfxKtrJlJat4Avkf9sbuUNuGZLowxtnn1TPXf%2FBUZXik%2FrH%2BGf8L3v0kaBboB4CNzjiyEmxv26anfYJ%2FG6wqVfaz%2FCPu4o2YDZweLzKsh6NFnHeFvBtNYZeXD7MZwGBWEtuGyLf3I4BHV5lAQRILs6DyyHVxmiDrmFOPbEB3rfUnCa6hNDdy6%2F2p0NlEC3eSHKBskIfxjFPG8OCVAelJ5gDSkuPjLfH0gyFhdLIN7LZJr18iJAvIsFFZzKppXGcqozXg%2FO%2B%2BsqL3gQEKDDt%2FZnXBjqkAa4wF%2FzlS8kUF70x9mgZpx0Bpe%2FXTGKG73NP43byEqB%2BpSnnEeTars6iNhfziHuA1tEgZEDYf5%2Fd6VlBgmiiNjqRHfwHhw%2FxgLVAl7dA8X4BSXzULUajxE3CVUHH5Xtt6vfBj1DI4AJMdpvmC8RSn%2Futtca1Ydh%2BsndalZlSMflmh6ROJ9EqZs7ffLOOSnRUIoY3XejDAl8dg2XTZDOmAIQIi2CW&X-Amz-Signature=f9d46b8b5c66ebf79349f21081c195ec60ea739389d4f0c932298a1681a8c26f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TASESVLO%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCfN%2BEm1OzjimsgHNZ6KhbLf%2FPeG1oZqyXHuHajPb4CggIhAPJadYIk0Y75N3d0Cjk8ObAkW8c88lF276wOCtgiEaq3Kv8DCFgQABoMNjM3NDIzMTgzODA1IgzjXvqKYkyhm0MRQKQq3AMG%2FuDF81Zco945o4z2MtAxF21DgJUqMQ7IhIzJloZA4t58UzRSwJtDyiZwkzylxYB39NqM%2BbDXzOjYGTfPZhVdj2DZnwEraIxQa33O9YH3DuMIYfZgipb65vw0JRO2NNyakmiEbSmdeY%2FLs%2B3I6xMYoIUYn2mmpu1XzoctI4LsBCHWhagTTBcK05ckLKSXOZmZwvRBonHyaxGxImWU8Y25p0DGV5xC7dFifyr8NxJdH8mdudtPGD6xxCXzr4mRLrwrouWrol4LbWJD086xtqbL5UsjZ3tNTSeDAz0XvmTcUIAQSAyFYjxYdU%2B7Cohhcv5MjEoQ%2FI1YNANe6r8UKRh3%2FTgG1O%2FQzIWqMef%2Byg5a7pfxKtrJlJat4Avkf9sbuUNuGZLowxtnn1TPXf%2FBUZXik%2FrH%2BGf8L3v0kaBboB4CNzjiyEmxv26anfYJ%2FG6wqVfaz%2FCPu4o2YDZweLzKsh6NFnHeFvBtNYZeXD7MZwGBWEtuGyLf3I4BHV5lAQRILs6DyyHVxmiDrmFOPbEB3rfUnCa6hNDdy6%2F2p0NlEC3eSHKBskIfxjFPG8OCVAelJ5gDSkuPjLfH0gyFhdLIN7LZJr18iJAvIsFFZzKppXGcqozXg%2FO%2B%2BsqL3gQEKDDt%2FZnXBjqkAa4wF%2FzlS8kUF70x9mgZpx0Bpe%2FXTGKG73NP43byEqB%2BpSnnEeTars6iNhfziHuA1tEgZEDYf5%2Fd6VlBgmiiNjqRHfwHhw%2FxgLVAl7dA8X4BSXzULUajxE3CVUHH5Xtt6vfBj1DI4AJMdpvmC8RSn%2Futtca1Ydh%2BsndalZlSMflmh6ROJ9EqZs7ffLOOSnRUIoY3XejDAl8dg2XTZDOmAIQIi2CW&X-Amz-Signature=51e584a362db8a7cf82b54b7fdf1df5625d77dd79569d4c1a59cda677d71e28e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


why do we need it RunnablePassThrough suppose in the examplew where we have to generate a joke on a topic and then explanation of the joke in that if we use RunnableSequence we dont get to see the joke itself so we can make the Runnable like this:-


```python
from langchain_google_genai import ChatGoogleGenerativeAI
from dotenv import load_dotenv
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnableParallel,RunnableSequence,RunnablePassthrough

load_dotenv()

model=ChatGoogleGenerativeAI(model='gemini-3.6-flash')
parser=StrOutputParser()

prompt1 = PromptTemplate(
    template='Generate a joke on the topic \n {topic}',
    input_variables=['topic']
)

prompt2= PromptTemplate(
    template='Give the explanation on the joke \n {joke}',
    input_variables=['joke']
)

joke_gen_chain= RunnableSequence(prompt1,model,parser)
Parallel_chain=RunnableParallel({
    'Joke':RunnablePassthrough(),
    'Explanation':RunnableSequence(prompt2,model,parser)
})
merge_chain=RunnableSequence(joke_gen_chain,Parallel_chain)
print(merge_chain.invoke({'topic':'Black Hole'}))
```


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TASESVLO%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCfN%2BEm1OzjimsgHNZ6KhbLf%2FPeG1oZqyXHuHajPb4CggIhAPJadYIk0Y75N3d0Cjk8ObAkW8c88lF276wOCtgiEaq3Kv8DCFgQABoMNjM3NDIzMTgzODA1IgzjXvqKYkyhm0MRQKQq3AMG%2FuDF81Zco945o4z2MtAxF21DgJUqMQ7IhIzJloZA4t58UzRSwJtDyiZwkzylxYB39NqM%2BbDXzOjYGTfPZhVdj2DZnwEraIxQa33O9YH3DuMIYfZgipb65vw0JRO2NNyakmiEbSmdeY%2FLs%2B3I6xMYoIUYn2mmpu1XzoctI4LsBCHWhagTTBcK05ckLKSXOZmZwvRBonHyaxGxImWU8Y25p0DGV5xC7dFifyr8NxJdH8mdudtPGD6xxCXzr4mRLrwrouWrol4LbWJD086xtqbL5UsjZ3tNTSeDAz0XvmTcUIAQSAyFYjxYdU%2B7Cohhcv5MjEoQ%2FI1YNANe6r8UKRh3%2FTgG1O%2FQzIWqMef%2Byg5a7pfxKtrJlJat4Avkf9sbuUNuGZLowxtnn1TPXf%2FBUZXik%2FrH%2BGf8L3v0kaBboB4CNzjiyEmxv26anfYJ%2FG6wqVfaz%2FCPu4o2YDZweLzKsh6NFnHeFvBtNYZeXD7MZwGBWEtuGyLf3I4BHV5lAQRILs6DyyHVxmiDrmFOPbEB3rfUnCa6hNDdy6%2F2p0NlEC3eSHKBskIfxjFPG8OCVAelJ5gDSkuPjLfH0gyFhdLIN7LZJr18iJAvIsFFZzKppXGcqozXg%2FO%2B%2BsqL3gQEKDDt%2FZnXBjqkAa4wF%2FzlS8kUF70x9mgZpx0Bpe%2FXTGKG73NP43byEqB%2BpSnnEeTars6iNhfziHuA1tEgZEDYf5%2Fd6VlBgmiiNjqRHfwHhw%2FxgLVAl7dA8X4BSXzULUajxE3CVUHH5Xtt6vfBj1DI4AJMdpvmC8RSn%2Futtca1Ydh%2BsndalZlSMflmh6ROJ9EqZs7ffLOOSnRUIoY3XejDAl8dg2XTZDOmAIQIi2CW&X-Amz-Signature=0a4887572b71d073339d852fc7d6b6587cba177ef46351a6068a2af97f4456b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


```python
#example 1
from langchain_core.runnables import RunnableLambda
def word_counter(text):
	return len(text.split())
runnable_word_counter=RunnableLambda(word_counter)
print(runnable_word_couner.invoke('how many words are there?'))

#example 2
from langchain_google_genai import ChatGoogleGenerativeAI
from dotenv import load_dotenv
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnableParallel,RunnableSequence,RunnablePassthrough,RunnableLambda

load_dotenv()
def word_count(text):
    return len(text.split())

model=ChatGoogleGenerativeAI(model='gemini-3.6-flash')
parser=StrOutputParser()

prompt1 = PromptTemplate(
    template='Generate a joke on the topic \n {topic}',
    input_variables=['topic']
)

joke_gen_chain= RunnableSequence(prompt1,model,parser)
Parallel_chain=RunnableParallel({
    'Joke':RunnablePassthrough(),
    'Word_count':RunnableLambda(word_count) # or RunnableLambda(lambda x: len(x.split()))
})

final_chain= RunnableSequence(joke_gen_chain,Parallel_chain)
print(final_chain.invoke({'topic':'cricket'}))
```


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TASESVLO%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCfN%2BEm1OzjimsgHNZ6KhbLf%2FPeG1oZqyXHuHajPb4CggIhAPJadYIk0Y75N3d0Cjk8ObAkW8c88lF276wOCtgiEaq3Kv8DCFgQABoMNjM3NDIzMTgzODA1IgzjXvqKYkyhm0MRQKQq3AMG%2FuDF81Zco945o4z2MtAxF21DgJUqMQ7IhIzJloZA4t58UzRSwJtDyiZwkzylxYB39NqM%2BbDXzOjYGTfPZhVdj2DZnwEraIxQa33O9YH3DuMIYfZgipb65vw0JRO2NNyakmiEbSmdeY%2FLs%2B3I6xMYoIUYn2mmpu1XzoctI4LsBCHWhagTTBcK05ckLKSXOZmZwvRBonHyaxGxImWU8Y25p0DGV5xC7dFifyr8NxJdH8mdudtPGD6xxCXzr4mRLrwrouWrol4LbWJD086xtqbL5UsjZ3tNTSeDAz0XvmTcUIAQSAyFYjxYdU%2B7Cohhcv5MjEoQ%2FI1YNANe6r8UKRh3%2FTgG1O%2FQzIWqMef%2Byg5a7pfxKtrJlJat4Avkf9sbuUNuGZLowxtnn1TPXf%2FBUZXik%2FrH%2BGf8L3v0kaBboB4CNzjiyEmxv26anfYJ%2FG6wqVfaz%2FCPu4o2YDZweLzKsh6NFnHeFvBtNYZeXD7MZwGBWEtuGyLf3I4BHV5lAQRILs6DyyHVxmiDrmFOPbEB3rfUnCa6hNDdy6%2F2p0NlEC3eSHKBskIfxjFPG8OCVAelJ5gDSkuPjLfH0gyFhdLIN7LZJr18iJAvIsFFZzKppXGcqozXg%2FO%2B%2BsqL3gQEKDDt%2FZnXBjqkAa4wF%2FzlS8kUF70x9mgZpx0Bpe%2FXTGKG73NP43byEqB%2BpSnnEeTars6iNhfziHuA1tEgZEDYf5%2Fd6VlBgmiiNjqRHfwHhw%2FxgLVAl7dA8X4BSXzULUajxE3CVUHH5Xtt6vfBj1DI4AJMdpvmC8RSn%2Futtca1Ydh%2BsndalZlSMflmh6ROJ9EqZs7ffLOOSnRUIoY3XejDAl8dg2XTZDOmAIQIi2CW&X-Amz-Signature=870d4cbb89a7067c197e2541f93a6e4b1a944e2a2ffab853b90d20943483451e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TASESVLO%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCfN%2BEm1OzjimsgHNZ6KhbLf%2FPeG1oZqyXHuHajPb4CggIhAPJadYIk0Y75N3d0Cjk8ObAkW8c88lF276wOCtgiEaq3Kv8DCFgQABoMNjM3NDIzMTgzODA1IgzjXvqKYkyhm0MRQKQq3AMG%2FuDF81Zco945o4z2MtAxF21DgJUqMQ7IhIzJloZA4t58UzRSwJtDyiZwkzylxYB39NqM%2BbDXzOjYGTfPZhVdj2DZnwEraIxQa33O9YH3DuMIYfZgipb65vw0JRO2NNyakmiEbSmdeY%2FLs%2B3I6xMYoIUYn2mmpu1XzoctI4LsBCHWhagTTBcK05ckLKSXOZmZwvRBonHyaxGxImWU8Y25p0DGV5xC7dFifyr8NxJdH8mdudtPGD6xxCXzr4mRLrwrouWrol4LbWJD086xtqbL5UsjZ3tNTSeDAz0XvmTcUIAQSAyFYjxYdU%2B7Cohhcv5MjEoQ%2FI1YNANe6r8UKRh3%2FTgG1O%2FQzIWqMef%2Byg5a7pfxKtrJlJat4Avkf9sbuUNuGZLowxtnn1TPXf%2FBUZXik%2FrH%2BGf8L3v0kaBboB4CNzjiyEmxv26anfYJ%2FG6wqVfaz%2FCPu4o2YDZweLzKsh6NFnHeFvBtNYZeXD7MZwGBWEtuGyLf3I4BHV5lAQRILs6DyyHVxmiDrmFOPbEB3rfUnCa6hNDdy6%2F2p0NlEC3eSHKBskIfxjFPG8OCVAelJ5gDSkuPjLfH0gyFhdLIN7LZJr18iJAvIsFFZzKppXGcqozXg%2FO%2B%2BsqL3gQEKDDt%2FZnXBjqkAa4wF%2FzlS8kUF70x9mgZpx0Bpe%2FXTGKG73NP43byEqB%2BpSnnEeTars6iNhfziHuA1tEgZEDYf5%2Fd6VlBgmiiNjqRHfwHhw%2FxgLVAl7dA8X4BSXzULUajxE3CVUHH5Xtt6vfBj1DI4AJMdpvmC8RSn%2Futtca1Ydh%2BsndalZlSMflmh6ROJ9EqZs7ffLOOSnRUIoY3XejDAl8dg2XTZDOmAIQIi2CW&X-Amz-Signature=609a8ac533e0747a37110fab5dc7b05c97f9b419baa34558ac9567e70e917295&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TASESVLO%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCfN%2BEm1OzjimsgHNZ6KhbLf%2FPeG1oZqyXHuHajPb4CggIhAPJadYIk0Y75N3d0Cjk8ObAkW8c88lF276wOCtgiEaq3Kv8DCFgQABoMNjM3NDIzMTgzODA1IgzjXvqKYkyhm0MRQKQq3AMG%2FuDF81Zco945o4z2MtAxF21DgJUqMQ7IhIzJloZA4t58UzRSwJtDyiZwkzylxYB39NqM%2BbDXzOjYGTfPZhVdj2DZnwEraIxQa33O9YH3DuMIYfZgipb65vw0JRO2NNyakmiEbSmdeY%2FLs%2B3I6xMYoIUYn2mmpu1XzoctI4LsBCHWhagTTBcK05ckLKSXOZmZwvRBonHyaxGxImWU8Y25p0DGV5xC7dFifyr8NxJdH8mdudtPGD6xxCXzr4mRLrwrouWrol4LbWJD086xtqbL5UsjZ3tNTSeDAz0XvmTcUIAQSAyFYjxYdU%2B7Cohhcv5MjEoQ%2FI1YNANe6r8UKRh3%2FTgG1O%2FQzIWqMef%2Byg5a7pfxKtrJlJat4Avkf9sbuUNuGZLowxtnn1TPXf%2FBUZXik%2FrH%2BGf8L3v0kaBboB4CNzjiyEmxv26anfYJ%2FG6wqVfaz%2FCPu4o2YDZweLzKsh6NFnHeFvBtNYZeXD7MZwGBWEtuGyLf3I4BHV5lAQRILs6DyyHVxmiDrmFOPbEB3rfUnCa6hNDdy6%2F2p0NlEC3eSHKBskIfxjFPG8OCVAelJ5gDSkuPjLfH0gyFhdLIN7LZJr18iJAvIsFFZzKppXGcqozXg%2FO%2B%2BsqL3gQEKDDt%2FZnXBjqkAa4wF%2FzlS8kUF70x9mgZpx0Bpe%2FXTGKG73NP43byEqB%2BpSnnEeTars6iNhfziHuA1tEgZEDYf5%2Fd6VlBgmiiNjqRHfwHhw%2FxgLVAl7dA8X4BSXzULUajxE3CVUHH5Xtt6vfBj1DI4AJMdpvmC8RSn%2Futtca1Ydh%2BsndalZlSMflmh6ROJ9EqZs7ffLOOSnRUIoY3XejDAl8dg2XTZDOmAIQIi2CW&X-Amz-Signature=b880e5bb260b308b49aeee42598bfa142dfe1180e8aad77978a9feacb7afc390&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
