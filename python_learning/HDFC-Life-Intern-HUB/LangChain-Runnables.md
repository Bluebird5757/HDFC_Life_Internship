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
fetched_at: '2026-09-05T01:58:09.728Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PCOSKDI%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015804Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJIMEYCIQCOMx3u1LBwbyejRydeCbS8kHDzjJmyRFVa9fcJKDKy9wIhAMq43s7p5H7iYGpxAZlz71xGRMw2p%2F4JGcFtv5ew7WRXKv8DCAMQABoMNjM3NDIzMTgzODA1IgwBI%2BWNAHtLhTy%2FmrQq3ANUrd3ahs1kzgT0a5h%2FeyaFQxhwfTQUoak0t6R4xNZtrx%2BxtRipaZ7RkP4LKS5FE10g7zainerIV8NaDtiBrvXM0hLWu2mFTpYVNnJNjHLdLZDtih877RnDpKKRd1X%2Bj4AHFO4HTqWwHO6LNcN0knwt2VtDHFLFHjfn3%2FrrlQWV7rCNtgXNBo0wyJoE3z2z3%2Bi978cfirCxgBrEwI4FalpM7mRChoDdMvqQUe5sjALJiZX%2Fi1E6VZk3l6GmggsLQ1gKbSALSEsHqnbbGBE5kYL9L1q0RxsYYAcx03tIuVeMN3vINpGEJpbT3zpCirf%2FLdCmgQNBItMXVlLP8fWL4XIn4zycZtbyTKKyooBQ1cssxHKLBxKyiPwAl%2FGskyihzcwV2H9dz4JIXxyTIEDFfrgZLs58BYEu3v%2F91axYPYXBbIqliECAX3zdK4JQGZrvhYe0DrNBgltQJKtTWRk5qyBT6EKjXVxQz%2FxVOD%2F2DWj1EXarAdVGl82CE0XuN%2F%2F3cDsAd5lfKdlLlnV8D4AWseyhmb3m4Gp%2Bt7iskILA0HiMKKZA%2Ft50XLR7YgXrT%2Fszjei6%2FrbCeOdkLGMvhzXOzGQPhQDPjAxk4BZc%2BZgiDcAVFZU%2FedpBQlJBt4HiTDCw5u3UBjqkAZ2XP9uSxtjoaQBrAcGmmTPk9CnHSc0YiNJWXtFhenbInSwOHdKnel4wuG77ujE9%2B3Ix1TSBD0Pz3GC1EWEzzoOdm9EoK8vod3W3Vv22qAHD6I9oJRS8mx5%2Fp%2BNEklIGm%2BnUoUTX2oVO5HDP4tpbcpxP2RCSJ4l3Y6iivONuPuFmbrSw8zJZtpDOksci9oRG89VitNOyF7stYmX%2FcckrHNxy4y1d&X-Amz-Signature=91a04681fd496339632ad8fe55c8273376db06a2ab8675f2cf6c489baa8fe817&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PCOSKDI%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015804Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJIMEYCIQCOMx3u1LBwbyejRydeCbS8kHDzjJmyRFVa9fcJKDKy9wIhAMq43s7p5H7iYGpxAZlz71xGRMw2p%2F4JGcFtv5ew7WRXKv8DCAMQABoMNjM3NDIzMTgzODA1IgwBI%2BWNAHtLhTy%2FmrQq3ANUrd3ahs1kzgT0a5h%2FeyaFQxhwfTQUoak0t6R4xNZtrx%2BxtRipaZ7RkP4LKS5FE10g7zainerIV8NaDtiBrvXM0hLWu2mFTpYVNnJNjHLdLZDtih877RnDpKKRd1X%2Bj4AHFO4HTqWwHO6LNcN0knwt2VtDHFLFHjfn3%2FrrlQWV7rCNtgXNBo0wyJoE3z2z3%2Bi978cfirCxgBrEwI4FalpM7mRChoDdMvqQUe5sjALJiZX%2Fi1E6VZk3l6GmggsLQ1gKbSALSEsHqnbbGBE5kYL9L1q0RxsYYAcx03tIuVeMN3vINpGEJpbT3zpCirf%2FLdCmgQNBItMXVlLP8fWL4XIn4zycZtbyTKKyooBQ1cssxHKLBxKyiPwAl%2FGskyihzcwV2H9dz4JIXxyTIEDFfrgZLs58BYEu3v%2F91axYPYXBbIqliECAX3zdK4JQGZrvhYe0DrNBgltQJKtTWRk5qyBT6EKjXVxQz%2FxVOD%2F2DWj1EXarAdVGl82CE0XuN%2F%2F3cDsAd5lfKdlLlnV8D4AWseyhmb3m4Gp%2Bt7iskILA0HiMKKZA%2Ft50XLR7YgXrT%2Fszjei6%2FrbCeOdkLGMvhzXOzGQPhQDPjAxk4BZc%2BZgiDcAVFZU%2FedpBQlJBt4HiTDCw5u3UBjqkAZ2XP9uSxtjoaQBrAcGmmTPk9CnHSc0YiNJWXtFhenbInSwOHdKnel4wuG77ujE9%2B3Ix1TSBD0Pz3GC1EWEzzoOdm9EoK8vod3W3Vv22qAHD6I9oJRS8mx5%2Fp%2BNEklIGm%2BnUoUTX2oVO5HDP4tpbcpxP2RCSJ4l3Y6iivONuPuFmbrSw8zJZtpDOksci9oRG89VitNOyF7stYmX%2FcckrHNxy4y1d&X-Amz-Signature=d7505304a4e9aa63f45214a3390ab329fd1a7ee6e19bd699ecc86e265f25d550&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PCOSKDI%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015804Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJIMEYCIQCOMx3u1LBwbyejRydeCbS8kHDzjJmyRFVa9fcJKDKy9wIhAMq43s7p5H7iYGpxAZlz71xGRMw2p%2F4JGcFtv5ew7WRXKv8DCAMQABoMNjM3NDIzMTgzODA1IgwBI%2BWNAHtLhTy%2FmrQq3ANUrd3ahs1kzgT0a5h%2FeyaFQxhwfTQUoak0t6R4xNZtrx%2BxtRipaZ7RkP4LKS5FE10g7zainerIV8NaDtiBrvXM0hLWu2mFTpYVNnJNjHLdLZDtih877RnDpKKRd1X%2Bj4AHFO4HTqWwHO6LNcN0knwt2VtDHFLFHjfn3%2FrrlQWV7rCNtgXNBo0wyJoE3z2z3%2Bi978cfirCxgBrEwI4FalpM7mRChoDdMvqQUe5sjALJiZX%2Fi1E6VZk3l6GmggsLQ1gKbSALSEsHqnbbGBE5kYL9L1q0RxsYYAcx03tIuVeMN3vINpGEJpbT3zpCirf%2FLdCmgQNBItMXVlLP8fWL4XIn4zycZtbyTKKyooBQ1cssxHKLBxKyiPwAl%2FGskyihzcwV2H9dz4JIXxyTIEDFfrgZLs58BYEu3v%2F91axYPYXBbIqliECAX3zdK4JQGZrvhYe0DrNBgltQJKtTWRk5qyBT6EKjXVxQz%2FxVOD%2F2DWj1EXarAdVGl82CE0XuN%2F%2F3cDsAd5lfKdlLlnV8D4AWseyhmb3m4Gp%2Bt7iskILA0HiMKKZA%2Ft50XLR7YgXrT%2Fszjei6%2FrbCeOdkLGMvhzXOzGQPhQDPjAxk4BZc%2BZgiDcAVFZU%2FedpBQlJBt4HiTDCw5u3UBjqkAZ2XP9uSxtjoaQBrAcGmmTPk9CnHSc0YiNJWXtFhenbInSwOHdKnel4wuG77ujE9%2B3Ix1TSBD0Pz3GC1EWEzzoOdm9EoK8vod3W3Vv22qAHD6I9oJRS8mx5%2Fp%2BNEklIGm%2BnUoUTX2oVO5HDP4tpbcpxP2RCSJ4l3Y6iivONuPuFmbrSw8zJZtpDOksci9oRG89VitNOyF7stYmX%2FcckrHNxy4y1d&X-Amz-Signature=c5fe20ca5be787b141e1594285149c519e87f33f0f0c197d58a4b42a4f147f6a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PCOSKDI%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015804Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJIMEYCIQCOMx3u1LBwbyejRydeCbS8kHDzjJmyRFVa9fcJKDKy9wIhAMq43s7p5H7iYGpxAZlz71xGRMw2p%2F4JGcFtv5ew7WRXKv8DCAMQABoMNjM3NDIzMTgzODA1IgwBI%2BWNAHtLhTy%2FmrQq3ANUrd3ahs1kzgT0a5h%2FeyaFQxhwfTQUoak0t6R4xNZtrx%2BxtRipaZ7RkP4LKS5FE10g7zainerIV8NaDtiBrvXM0hLWu2mFTpYVNnJNjHLdLZDtih877RnDpKKRd1X%2Bj4AHFO4HTqWwHO6LNcN0knwt2VtDHFLFHjfn3%2FrrlQWV7rCNtgXNBo0wyJoE3z2z3%2Bi978cfirCxgBrEwI4FalpM7mRChoDdMvqQUe5sjALJiZX%2Fi1E6VZk3l6GmggsLQ1gKbSALSEsHqnbbGBE5kYL9L1q0RxsYYAcx03tIuVeMN3vINpGEJpbT3zpCirf%2FLdCmgQNBItMXVlLP8fWL4XIn4zycZtbyTKKyooBQ1cssxHKLBxKyiPwAl%2FGskyihzcwV2H9dz4JIXxyTIEDFfrgZLs58BYEu3v%2F91axYPYXBbIqliECAX3zdK4JQGZrvhYe0DrNBgltQJKtTWRk5qyBT6EKjXVxQz%2FxVOD%2F2DWj1EXarAdVGl82CE0XuN%2F%2F3cDsAd5lfKdlLlnV8D4AWseyhmb3m4Gp%2Bt7iskILA0HiMKKZA%2Ft50XLR7YgXrT%2Fszjei6%2FrbCeOdkLGMvhzXOzGQPhQDPjAxk4BZc%2BZgiDcAVFZU%2FedpBQlJBt4HiTDCw5u3UBjqkAZ2XP9uSxtjoaQBrAcGmmTPk9CnHSc0YiNJWXtFhenbInSwOHdKnel4wuG77ujE9%2B3Ix1TSBD0Pz3GC1EWEzzoOdm9EoK8vod3W3Vv22qAHD6I9oJRS8mx5%2Fp%2BNEklIGm%2BnUoUTX2oVO5HDP4tpbcpxP2RCSJ4l3Y6iivONuPuFmbrSw8zJZtpDOksci9oRG89VitNOyF7stYmX%2FcckrHNxy4y1d&X-Amz-Signature=e4603c5afdab73998a92de4850c9013f8f6e338bf23d11be22b6bf13e1df2e3d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PCOSKDI%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015804Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJIMEYCIQCOMx3u1LBwbyejRydeCbS8kHDzjJmyRFVa9fcJKDKy9wIhAMq43s7p5H7iYGpxAZlz71xGRMw2p%2F4JGcFtv5ew7WRXKv8DCAMQABoMNjM3NDIzMTgzODA1IgwBI%2BWNAHtLhTy%2FmrQq3ANUrd3ahs1kzgT0a5h%2FeyaFQxhwfTQUoak0t6R4xNZtrx%2BxtRipaZ7RkP4LKS5FE10g7zainerIV8NaDtiBrvXM0hLWu2mFTpYVNnJNjHLdLZDtih877RnDpKKRd1X%2Bj4AHFO4HTqWwHO6LNcN0knwt2VtDHFLFHjfn3%2FrrlQWV7rCNtgXNBo0wyJoE3z2z3%2Bi978cfirCxgBrEwI4FalpM7mRChoDdMvqQUe5sjALJiZX%2Fi1E6VZk3l6GmggsLQ1gKbSALSEsHqnbbGBE5kYL9L1q0RxsYYAcx03tIuVeMN3vINpGEJpbT3zpCirf%2FLdCmgQNBItMXVlLP8fWL4XIn4zycZtbyTKKyooBQ1cssxHKLBxKyiPwAl%2FGskyihzcwV2H9dz4JIXxyTIEDFfrgZLs58BYEu3v%2F91axYPYXBbIqliECAX3zdK4JQGZrvhYe0DrNBgltQJKtTWRk5qyBT6EKjXVxQz%2FxVOD%2F2DWj1EXarAdVGl82CE0XuN%2F%2F3cDsAd5lfKdlLlnV8D4AWseyhmb3m4Gp%2Bt7iskILA0HiMKKZA%2Ft50XLR7YgXrT%2Fszjei6%2FrbCeOdkLGMvhzXOzGQPhQDPjAxk4BZc%2BZgiDcAVFZU%2FedpBQlJBt4HiTDCw5u3UBjqkAZ2XP9uSxtjoaQBrAcGmmTPk9CnHSc0YiNJWXtFhenbInSwOHdKnel4wuG77ujE9%2B3Ix1TSBD0Pz3GC1EWEzzoOdm9EoK8vod3W3Vv22qAHD6I9oJRS8mx5%2Fp%2BNEklIGm%2BnUoUTX2oVO5HDP4tpbcpxP2RCSJ4l3Y6iivONuPuFmbrSw8zJZtpDOksci9oRG89VitNOyF7stYmX%2FcckrHNxy4y1d&X-Amz-Signature=2028b519e4ba55388a3c26203d45522a1eb2f1746d9cfb6fa0bdb5de5eba318a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PCOSKDI%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015804Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJIMEYCIQCOMx3u1LBwbyejRydeCbS8kHDzjJmyRFVa9fcJKDKy9wIhAMq43s7p5H7iYGpxAZlz71xGRMw2p%2F4JGcFtv5ew7WRXKv8DCAMQABoMNjM3NDIzMTgzODA1IgwBI%2BWNAHtLhTy%2FmrQq3ANUrd3ahs1kzgT0a5h%2FeyaFQxhwfTQUoak0t6R4xNZtrx%2BxtRipaZ7RkP4LKS5FE10g7zainerIV8NaDtiBrvXM0hLWu2mFTpYVNnJNjHLdLZDtih877RnDpKKRd1X%2Bj4AHFO4HTqWwHO6LNcN0knwt2VtDHFLFHjfn3%2FrrlQWV7rCNtgXNBo0wyJoE3z2z3%2Bi978cfirCxgBrEwI4FalpM7mRChoDdMvqQUe5sjALJiZX%2Fi1E6VZk3l6GmggsLQ1gKbSALSEsHqnbbGBE5kYL9L1q0RxsYYAcx03tIuVeMN3vINpGEJpbT3zpCirf%2FLdCmgQNBItMXVlLP8fWL4XIn4zycZtbyTKKyooBQ1cssxHKLBxKyiPwAl%2FGskyihzcwV2H9dz4JIXxyTIEDFfrgZLs58BYEu3v%2F91axYPYXBbIqliECAX3zdK4JQGZrvhYe0DrNBgltQJKtTWRk5qyBT6EKjXVxQz%2FxVOD%2F2DWj1EXarAdVGl82CE0XuN%2F%2F3cDsAd5lfKdlLlnV8D4AWseyhmb3m4Gp%2Bt7iskILA0HiMKKZA%2Ft50XLR7YgXrT%2Fszjei6%2FrbCeOdkLGMvhzXOzGQPhQDPjAxk4BZc%2BZgiDcAVFZU%2FedpBQlJBt4HiTDCw5u3UBjqkAZ2XP9uSxtjoaQBrAcGmmTPk9CnHSc0YiNJWXtFhenbInSwOHdKnel4wuG77ujE9%2B3Ix1TSBD0Pz3GC1EWEzzoOdm9EoK8vod3W3Vv22qAHD6I9oJRS8mx5%2Fp%2BNEklIGm%2BnUoUTX2oVO5HDP4tpbcpxP2RCSJ4l3Y6iivONuPuFmbrSw8zJZtpDOksci9oRG89VitNOyF7stYmX%2FcckrHNxy4y1d&X-Amz-Signature=8549a50d96295cc766f5ff287d20c6960119cb5296f603f166b62598789df20a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PCOSKDI%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015804Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJIMEYCIQCOMx3u1LBwbyejRydeCbS8kHDzjJmyRFVa9fcJKDKy9wIhAMq43s7p5H7iYGpxAZlz71xGRMw2p%2F4JGcFtv5ew7WRXKv8DCAMQABoMNjM3NDIzMTgzODA1IgwBI%2BWNAHtLhTy%2FmrQq3ANUrd3ahs1kzgT0a5h%2FeyaFQxhwfTQUoak0t6R4xNZtrx%2BxtRipaZ7RkP4LKS5FE10g7zainerIV8NaDtiBrvXM0hLWu2mFTpYVNnJNjHLdLZDtih877RnDpKKRd1X%2Bj4AHFO4HTqWwHO6LNcN0knwt2VtDHFLFHjfn3%2FrrlQWV7rCNtgXNBo0wyJoE3z2z3%2Bi978cfirCxgBrEwI4FalpM7mRChoDdMvqQUe5sjALJiZX%2Fi1E6VZk3l6GmggsLQ1gKbSALSEsHqnbbGBE5kYL9L1q0RxsYYAcx03tIuVeMN3vINpGEJpbT3zpCirf%2FLdCmgQNBItMXVlLP8fWL4XIn4zycZtbyTKKyooBQ1cssxHKLBxKyiPwAl%2FGskyihzcwV2H9dz4JIXxyTIEDFfrgZLs58BYEu3v%2F91axYPYXBbIqliECAX3zdK4JQGZrvhYe0DrNBgltQJKtTWRk5qyBT6EKjXVxQz%2FxVOD%2F2DWj1EXarAdVGl82CE0XuN%2F%2F3cDsAd5lfKdlLlnV8D4AWseyhmb3m4Gp%2Bt7iskILA0HiMKKZA%2Ft50XLR7YgXrT%2Fszjei6%2FrbCeOdkLGMvhzXOzGQPhQDPjAxk4BZc%2BZgiDcAVFZU%2FedpBQlJBt4HiTDCw5u3UBjqkAZ2XP9uSxtjoaQBrAcGmmTPk9CnHSc0YiNJWXtFhenbInSwOHdKnel4wuG77ujE9%2B3Ix1TSBD0Pz3GC1EWEzzoOdm9EoK8vod3W3Vv22qAHD6I9oJRS8mx5%2Fp%2BNEklIGm%2BnUoUTX2oVO5HDP4tpbcpxP2RCSJ4l3Y6iivONuPuFmbrSw8zJZtpDOksci9oRG89VitNOyF7stYmX%2FcckrHNxy4y1d&X-Amz-Signature=15b3296aece47254a5ae4d2e7abf98fb659de8970d727dcf43cc04ebd5e9b232&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PCOSKDI%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015804Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJIMEYCIQCOMx3u1LBwbyejRydeCbS8kHDzjJmyRFVa9fcJKDKy9wIhAMq43s7p5H7iYGpxAZlz71xGRMw2p%2F4JGcFtv5ew7WRXKv8DCAMQABoMNjM3NDIzMTgzODA1IgwBI%2BWNAHtLhTy%2FmrQq3ANUrd3ahs1kzgT0a5h%2FeyaFQxhwfTQUoak0t6R4xNZtrx%2BxtRipaZ7RkP4LKS5FE10g7zainerIV8NaDtiBrvXM0hLWu2mFTpYVNnJNjHLdLZDtih877RnDpKKRd1X%2Bj4AHFO4HTqWwHO6LNcN0knwt2VtDHFLFHjfn3%2FrrlQWV7rCNtgXNBo0wyJoE3z2z3%2Bi978cfirCxgBrEwI4FalpM7mRChoDdMvqQUe5sjALJiZX%2Fi1E6VZk3l6GmggsLQ1gKbSALSEsHqnbbGBE5kYL9L1q0RxsYYAcx03tIuVeMN3vINpGEJpbT3zpCirf%2FLdCmgQNBItMXVlLP8fWL4XIn4zycZtbyTKKyooBQ1cssxHKLBxKyiPwAl%2FGskyihzcwV2H9dz4JIXxyTIEDFfrgZLs58BYEu3v%2F91axYPYXBbIqliECAX3zdK4JQGZrvhYe0DrNBgltQJKtTWRk5qyBT6EKjXVxQz%2FxVOD%2F2DWj1EXarAdVGl82CE0XuN%2F%2F3cDsAd5lfKdlLlnV8D4AWseyhmb3m4Gp%2Bt7iskILA0HiMKKZA%2Ft50XLR7YgXrT%2Fszjei6%2FrbCeOdkLGMvhzXOzGQPhQDPjAxk4BZc%2BZgiDcAVFZU%2FedpBQlJBt4HiTDCw5u3UBjqkAZ2XP9uSxtjoaQBrAcGmmTPk9CnHSc0YiNJWXtFhenbInSwOHdKnel4wuG77ujE9%2B3Ix1TSBD0Pz3GC1EWEzzoOdm9EoK8vod3W3Vv22qAHD6I9oJRS8mx5%2Fp%2BNEklIGm%2BnUoUTX2oVO5HDP4tpbcpxP2RCSJ4l3Y6iivONuPuFmbrSw8zJZtpDOksci9oRG89VitNOyF7stYmX%2FcckrHNxy4y1d&X-Amz-Signature=c8121cff5a295ca0336fb54691419532f9d9add714eabaecf7c843bce510aef3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PCOSKDI%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015804Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJIMEYCIQCOMx3u1LBwbyejRydeCbS8kHDzjJmyRFVa9fcJKDKy9wIhAMq43s7p5H7iYGpxAZlz71xGRMw2p%2F4JGcFtv5ew7WRXKv8DCAMQABoMNjM3NDIzMTgzODA1IgwBI%2BWNAHtLhTy%2FmrQq3ANUrd3ahs1kzgT0a5h%2FeyaFQxhwfTQUoak0t6R4xNZtrx%2BxtRipaZ7RkP4LKS5FE10g7zainerIV8NaDtiBrvXM0hLWu2mFTpYVNnJNjHLdLZDtih877RnDpKKRd1X%2Bj4AHFO4HTqWwHO6LNcN0knwt2VtDHFLFHjfn3%2FrrlQWV7rCNtgXNBo0wyJoE3z2z3%2Bi978cfirCxgBrEwI4FalpM7mRChoDdMvqQUe5sjALJiZX%2Fi1E6VZk3l6GmggsLQ1gKbSALSEsHqnbbGBE5kYL9L1q0RxsYYAcx03tIuVeMN3vINpGEJpbT3zpCirf%2FLdCmgQNBItMXVlLP8fWL4XIn4zycZtbyTKKyooBQ1cssxHKLBxKyiPwAl%2FGskyihzcwV2H9dz4JIXxyTIEDFfrgZLs58BYEu3v%2F91axYPYXBbIqliECAX3zdK4JQGZrvhYe0DrNBgltQJKtTWRk5qyBT6EKjXVxQz%2FxVOD%2F2DWj1EXarAdVGl82CE0XuN%2F%2F3cDsAd5lfKdlLlnV8D4AWseyhmb3m4Gp%2Bt7iskILA0HiMKKZA%2Ft50XLR7YgXrT%2Fszjei6%2FrbCeOdkLGMvhzXOzGQPhQDPjAxk4BZc%2BZgiDcAVFZU%2FedpBQlJBt4HiTDCw5u3UBjqkAZ2XP9uSxtjoaQBrAcGmmTPk9CnHSc0YiNJWXtFhenbInSwOHdKnel4wuG77ujE9%2B3Ix1TSBD0Pz3GC1EWEzzoOdm9EoK8vod3W3Vv22qAHD6I9oJRS8mx5%2Fp%2BNEklIGm%2BnUoUTX2oVO5HDP4tpbcpxP2RCSJ4l3Y6iivONuPuFmbrSw8zJZtpDOksci9oRG89VitNOyF7stYmX%2FcckrHNxy4y1d&X-Amz-Signature=1c5f58c323cbccaca4767ba495a1556bd287c3a4e16610247f974885793a5728&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
