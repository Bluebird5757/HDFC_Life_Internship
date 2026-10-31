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
fetched_at: '2026-10-31T03:14:01.149Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRC33PHP%2F20261031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261031T031356Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQDisAf%2BICJ5VS803IQLaxkPHlq7j%2Bbz1ckBZ1lCHn1W4gIhAIRaqIaaPAO7Fv0MH7YyjUQWny%2FmX6YTpYW%2B32FckhC3Kv8DCEMQABoMNjM3NDIzMTgzODA1IgyI4pILzz5b4qtcnYUq3ANexNoT6MkLW%2FsCK5tx6vbLvbuKvvx16UEsfKP2fpytGUqUgLLgpGqzztB0LqrtqL2xgnf3mkxzgGr2mYQh1zE9n%2BUDra5eVz9ZRRVAvcbPPzJnJ%2FMIBj%2B2RcceauG3LX%2B%2Bz6NQ54osdf1rj2cQDNrtJIp4qzHzZQZ8SSpN3E7ER2m%2BXW3OvVTbgAx6%2BdfUz8VVjx2bLGhNV%2Bgmaef9cX31KdjsdLAsJ9x5wn%2FYVaAyzWZHggICaNA5GJ2o7%2F%2FfCHvRw2PaZ17aQ695bfhv0zKLW4N%2F2KQu2tgAUk8id%2BCETFpTSHPMluZ7avEvTF9h6HvhNHJJaOKyuszh5b96z22JttA26ELgjG6a1fpLDoO6V8Tl2xkNNyg9QJ9Wx%2Fjy6gaA01IzfiEHEGIloPKlRdCFn6iu9NkjezGcVQL7YZn4Elakzov2yHNGYJERxpzaPXdtiwhsvyDHKcHdJ2HcVFAfdRLHYNkHf2myz0FIilO09cuO4MO7VGngcpEL5UQwG6o2%2FdyzUSHbL6oTgZlv68HrGrrtSnCItYsET1JGIa0qx2NirvP26W2sryQje4r8xQICyIuUNIAb5usm4GGuLQwEvUYgTEEsRRgr4vmPT3VDkKVfz6fTPZkpeyieIjCnlJXXBjqkAcUeODkZWbMFIthbHDA%2BLGuNsRUpRUH4%2FtrcP55y82Wq%2Bd8yV5IiuhJegQFzMqRaMJp4b4%2BnphJrotcDWcMLmFF19E9gGXJDnt3i0KJxVxG2UKNrIAXM8kCKrk6YNCOyOFXt4JCgw3xivSwIR7moXpMIOIvDWeKnq2RSFY4n2WpWb3IA%2FvZA6BknfyuhKcpycP4FtjIbCMFRSA%2FBXOwj0nnSGd2R&X-Amz-Signature=0b1b161ba51928b3ae23ed3a9215c2d1f210bad65202e661031abb973ef9b5cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRC33PHP%2F20261031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261031T031356Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQDisAf%2BICJ5VS803IQLaxkPHlq7j%2Bbz1ckBZ1lCHn1W4gIhAIRaqIaaPAO7Fv0MH7YyjUQWny%2FmX6YTpYW%2B32FckhC3Kv8DCEMQABoMNjM3NDIzMTgzODA1IgyI4pILzz5b4qtcnYUq3ANexNoT6MkLW%2FsCK5tx6vbLvbuKvvx16UEsfKP2fpytGUqUgLLgpGqzztB0LqrtqL2xgnf3mkxzgGr2mYQh1zE9n%2BUDra5eVz9ZRRVAvcbPPzJnJ%2FMIBj%2B2RcceauG3LX%2B%2Bz6NQ54osdf1rj2cQDNrtJIp4qzHzZQZ8SSpN3E7ER2m%2BXW3OvVTbgAx6%2BdfUz8VVjx2bLGhNV%2Bgmaef9cX31KdjsdLAsJ9x5wn%2FYVaAyzWZHggICaNA5GJ2o7%2F%2FfCHvRw2PaZ17aQ695bfhv0zKLW4N%2F2KQu2tgAUk8id%2BCETFpTSHPMluZ7avEvTF9h6HvhNHJJaOKyuszh5b96z22JttA26ELgjG6a1fpLDoO6V8Tl2xkNNyg9QJ9Wx%2Fjy6gaA01IzfiEHEGIloPKlRdCFn6iu9NkjezGcVQL7YZn4Elakzov2yHNGYJERxpzaPXdtiwhsvyDHKcHdJ2HcVFAfdRLHYNkHf2myz0FIilO09cuO4MO7VGngcpEL5UQwG6o2%2FdyzUSHbL6oTgZlv68HrGrrtSnCItYsET1JGIa0qx2NirvP26W2sryQje4r8xQICyIuUNIAb5usm4GGuLQwEvUYgTEEsRRgr4vmPT3VDkKVfz6fTPZkpeyieIjCnlJXXBjqkAcUeODkZWbMFIthbHDA%2BLGuNsRUpRUH4%2FtrcP55y82Wq%2Bd8yV5IiuhJegQFzMqRaMJp4b4%2BnphJrotcDWcMLmFF19E9gGXJDnt3i0KJxVxG2UKNrIAXM8kCKrk6YNCOyOFXt4JCgw3xivSwIR7moXpMIOIvDWeKnq2RSFY4n2WpWb3IA%2FvZA6BknfyuhKcpycP4FtjIbCMFRSA%2FBXOwj0nnSGd2R&X-Amz-Signature=e2b05980f8b5eff2a2accc7075f96b60799d089e0d01786034eda8c4e18b65c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRC33PHP%2F20261031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261031T031356Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQDisAf%2BICJ5VS803IQLaxkPHlq7j%2Bbz1ckBZ1lCHn1W4gIhAIRaqIaaPAO7Fv0MH7YyjUQWny%2FmX6YTpYW%2B32FckhC3Kv8DCEMQABoMNjM3NDIzMTgzODA1IgyI4pILzz5b4qtcnYUq3ANexNoT6MkLW%2FsCK5tx6vbLvbuKvvx16UEsfKP2fpytGUqUgLLgpGqzztB0LqrtqL2xgnf3mkxzgGr2mYQh1zE9n%2BUDra5eVz9ZRRVAvcbPPzJnJ%2FMIBj%2B2RcceauG3LX%2B%2Bz6NQ54osdf1rj2cQDNrtJIp4qzHzZQZ8SSpN3E7ER2m%2BXW3OvVTbgAx6%2BdfUz8VVjx2bLGhNV%2Bgmaef9cX31KdjsdLAsJ9x5wn%2FYVaAyzWZHggICaNA5GJ2o7%2F%2FfCHvRw2PaZ17aQ695bfhv0zKLW4N%2F2KQu2tgAUk8id%2BCETFpTSHPMluZ7avEvTF9h6HvhNHJJaOKyuszh5b96z22JttA26ELgjG6a1fpLDoO6V8Tl2xkNNyg9QJ9Wx%2Fjy6gaA01IzfiEHEGIloPKlRdCFn6iu9NkjezGcVQL7YZn4Elakzov2yHNGYJERxpzaPXdtiwhsvyDHKcHdJ2HcVFAfdRLHYNkHf2myz0FIilO09cuO4MO7VGngcpEL5UQwG6o2%2FdyzUSHbL6oTgZlv68HrGrrtSnCItYsET1JGIa0qx2NirvP26W2sryQje4r8xQICyIuUNIAb5usm4GGuLQwEvUYgTEEsRRgr4vmPT3VDkKVfz6fTPZkpeyieIjCnlJXXBjqkAcUeODkZWbMFIthbHDA%2BLGuNsRUpRUH4%2FtrcP55y82Wq%2Bd8yV5IiuhJegQFzMqRaMJp4b4%2BnphJrotcDWcMLmFF19E9gGXJDnt3i0KJxVxG2UKNrIAXM8kCKrk6YNCOyOFXt4JCgw3xivSwIR7moXpMIOIvDWeKnq2RSFY4n2WpWb3IA%2FvZA6BknfyuhKcpycP4FtjIbCMFRSA%2FBXOwj0nnSGd2R&X-Amz-Signature=522bfa0b66433537e0295f05954d53fe633361a0d75a8b8bc66dc599f0feaeb8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRC33PHP%2F20261031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261031T031356Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQDisAf%2BICJ5VS803IQLaxkPHlq7j%2Bbz1ckBZ1lCHn1W4gIhAIRaqIaaPAO7Fv0MH7YyjUQWny%2FmX6YTpYW%2B32FckhC3Kv8DCEMQABoMNjM3NDIzMTgzODA1IgyI4pILzz5b4qtcnYUq3ANexNoT6MkLW%2FsCK5tx6vbLvbuKvvx16UEsfKP2fpytGUqUgLLgpGqzztB0LqrtqL2xgnf3mkxzgGr2mYQh1zE9n%2BUDra5eVz9ZRRVAvcbPPzJnJ%2FMIBj%2B2RcceauG3LX%2B%2Bz6NQ54osdf1rj2cQDNrtJIp4qzHzZQZ8SSpN3E7ER2m%2BXW3OvVTbgAx6%2BdfUz8VVjx2bLGhNV%2Bgmaef9cX31KdjsdLAsJ9x5wn%2FYVaAyzWZHggICaNA5GJ2o7%2F%2FfCHvRw2PaZ17aQ695bfhv0zKLW4N%2F2KQu2tgAUk8id%2BCETFpTSHPMluZ7avEvTF9h6HvhNHJJaOKyuszh5b96z22JttA26ELgjG6a1fpLDoO6V8Tl2xkNNyg9QJ9Wx%2Fjy6gaA01IzfiEHEGIloPKlRdCFn6iu9NkjezGcVQL7YZn4Elakzov2yHNGYJERxpzaPXdtiwhsvyDHKcHdJ2HcVFAfdRLHYNkHf2myz0FIilO09cuO4MO7VGngcpEL5UQwG6o2%2FdyzUSHbL6oTgZlv68HrGrrtSnCItYsET1JGIa0qx2NirvP26W2sryQje4r8xQICyIuUNIAb5usm4GGuLQwEvUYgTEEsRRgr4vmPT3VDkKVfz6fTPZkpeyieIjCnlJXXBjqkAcUeODkZWbMFIthbHDA%2BLGuNsRUpRUH4%2FtrcP55y82Wq%2Bd8yV5IiuhJegQFzMqRaMJp4b4%2BnphJrotcDWcMLmFF19E9gGXJDnt3i0KJxVxG2UKNrIAXM8kCKrk6YNCOyOFXt4JCgw3xivSwIR7moXpMIOIvDWeKnq2RSFY4n2WpWb3IA%2FvZA6BknfyuhKcpycP4FtjIbCMFRSA%2FBXOwj0nnSGd2R&X-Amz-Signature=a1537c64d28c779b040e4efa638ff042e36d711bc44cc7b33900894bae5437a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRC33PHP%2F20261031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261031T031356Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQDisAf%2BICJ5VS803IQLaxkPHlq7j%2Bbz1ckBZ1lCHn1W4gIhAIRaqIaaPAO7Fv0MH7YyjUQWny%2FmX6YTpYW%2B32FckhC3Kv8DCEMQABoMNjM3NDIzMTgzODA1IgyI4pILzz5b4qtcnYUq3ANexNoT6MkLW%2FsCK5tx6vbLvbuKvvx16UEsfKP2fpytGUqUgLLgpGqzztB0LqrtqL2xgnf3mkxzgGr2mYQh1zE9n%2BUDra5eVz9ZRRVAvcbPPzJnJ%2FMIBj%2B2RcceauG3LX%2B%2Bz6NQ54osdf1rj2cQDNrtJIp4qzHzZQZ8SSpN3E7ER2m%2BXW3OvVTbgAx6%2BdfUz8VVjx2bLGhNV%2Bgmaef9cX31KdjsdLAsJ9x5wn%2FYVaAyzWZHggICaNA5GJ2o7%2F%2FfCHvRw2PaZ17aQ695bfhv0zKLW4N%2F2KQu2tgAUk8id%2BCETFpTSHPMluZ7avEvTF9h6HvhNHJJaOKyuszh5b96z22JttA26ELgjG6a1fpLDoO6V8Tl2xkNNyg9QJ9Wx%2Fjy6gaA01IzfiEHEGIloPKlRdCFn6iu9NkjezGcVQL7YZn4Elakzov2yHNGYJERxpzaPXdtiwhsvyDHKcHdJ2HcVFAfdRLHYNkHf2myz0FIilO09cuO4MO7VGngcpEL5UQwG6o2%2FdyzUSHbL6oTgZlv68HrGrrtSnCItYsET1JGIa0qx2NirvP26W2sryQje4r8xQICyIuUNIAb5usm4GGuLQwEvUYgTEEsRRgr4vmPT3VDkKVfz6fTPZkpeyieIjCnlJXXBjqkAcUeODkZWbMFIthbHDA%2BLGuNsRUpRUH4%2FtrcP55y82Wq%2Bd8yV5IiuhJegQFzMqRaMJp4b4%2BnphJrotcDWcMLmFF19E9gGXJDnt3i0KJxVxG2UKNrIAXM8kCKrk6YNCOyOFXt4JCgw3xivSwIR7moXpMIOIvDWeKnq2RSFY4n2WpWb3IA%2FvZA6BknfyuhKcpycP4FtjIbCMFRSA%2FBXOwj0nnSGd2R&X-Amz-Signature=935576694dc7e36f493b4202f68ebe722891ca72c6f8acdebd3fde4b2a06356f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRC33PHP%2F20261031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261031T031356Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQDisAf%2BICJ5VS803IQLaxkPHlq7j%2Bbz1ckBZ1lCHn1W4gIhAIRaqIaaPAO7Fv0MH7YyjUQWny%2FmX6YTpYW%2B32FckhC3Kv8DCEMQABoMNjM3NDIzMTgzODA1IgyI4pILzz5b4qtcnYUq3ANexNoT6MkLW%2FsCK5tx6vbLvbuKvvx16UEsfKP2fpytGUqUgLLgpGqzztB0LqrtqL2xgnf3mkxzgGr2mYQh1zE9n%2BUDra5eVz9ZRRVAvcbPPzJnJ%2FMIBj%2B2RcceauG3LX%2B%2Bz6NQ54osdf1rj2cQDNrtJIp4qzHzZQZ8SSpN3E7ER2m%2BXW3OvVTbgAx6%2BdfUz8VVjx2bLGhNV%2Bgmaef9cX31KdjsdLAsJ9x5wn%2FYVaAyzWZHggICaNA5GJ2o7%2F%2FfCHvRw2PaZ17aQ695bfhv0zKLW4N%2F2KQu2tgAUk8id%2BCETFpTSHPMluZ7avEvTF9h6HvhNHJJaOKyuszh5b96z22JttA26ELgjG6a1fpLDoO6V8Tl2xkNNyg9QJ9Wx%2Fjy6gaA01IzfiEHEGIloPKlRdCFn6iu9NkjezGcVQL7YZn4Elakzov2yHNGYJERxpzaPXdtiwhsvyDHKcHdJ2HcVFAfdRLHYNkHf2myz0FIilO09cuO4MO7VGngcpEL5UQwG6o2%2FdyzUSHbL6oTgZlv68HrGrrtSnCItYsET1JGIa0qx2NirvP26W2sryQje4r8xQICyIuUNIAb5usm4GGuLQwEvUYgTEEsRRgr4vmPT3VDkKVfz6fTPZkpeyieIjCnlJXXBjqkAcUeODkZWbMFIthbHDA%2BLGuNsRUpRUH4%2FtrcP55y82Wq%2Bd8yV5IiuhJegQFzMqRaMJp4b4%2BnphJrotcDWcMLmFF19E9gGXJDnt3i0KJxVxG2UKNrIAXM8kCKrk6YNCOyOFXt4JCgw3xivSwIR7moXpMIOIvDWeKnq2RSFY4n2WpWb3IA%2FvZA6BknfyuhKcpycP4FtjIbCMFRSA%2FBXOwj0nnSGd2R&X-Amz-Signature=f97d3bbdbdb2fe11eaed56d283fc8f1b6b0c7eb0d74ce99a8c5106b8e42d649d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRC33PHP%2F20261031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261031T031356Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQDisAf%2BICJ5VS803IQLaxkPHlq7j%2Bbz1ckBZ1lCHn1W4gIhAIRaqIaaPAO7Fv0MH7YyjUQWny%2FmX6YTpYW%2B32FckhC3Kv8DCEMQABoMNjM3NDIzMTgzODA1IgyI4pILzz5b4qtcnYUq3ANexNoT6MkLW%2FsCK5tx6vbLvbuKvvx16UEsfKP2fpytGUqUgLLgpGqzztB0LqrtqL2xgnf3mkxzgGr2mYQh1zE9n%2BUDra5eVz9ZRRVAvcbPPzJnJ%2FMIBj%2B2RcceauG3LX%2B%2Bz6NQ54osdf1rj2cQDNrtJIp4qzHzZQZ8SSpN3E7ER2m%2BXW3OvVTbgAx6%2BdfUz8VVjx2bLGhNV%2Bgmaef9cX31KdjsdLAsJ9x5wn%2FYVaAyzWZHggICaNA5GJ2o7%2F%2FfCHvRw2PaZ17aQ695bfhv0zKLW4N%2F2KQu2tgAUk8id%2BCETFpTSHPMluZ7avEvTF9h6HvhNHJJaOKyuszh5b96z22JttA26ELgjG6a1fpLDoO6V8Tl2xkNNyg9QJ9Wx%2Fjy6gaA01IzfiEHEGIloPKlRdCFn6iu9NkjezGcVQL7YZn4Elakzov2yHNGYJERxpzaPXdtiwhsvyDHKcHdJ2HcVFAfdRLHYNkHf2myz0FIilO09cuO4MO7VGngcpEL5UQwG6o2%2FdyzUSHbL6oTgZlv68HrGrrtSnCItYsET1JGIa0qx2NirvP26W2sryQje4r8xQICyIuUNIAb5usm4GGuLQwEvUYgTEEsRRgr4vmPT3VDkKVfz6fTPZkpeyieIjCnlJXXBjqkAcUeODkZWbMFIthbHDA%2BLGuNsRUpRUH4%2FtrcP55y82Wq%2Bd8yV5IiuhJegQFzMqRaMJp4b4%2BnphJrotcDWcMLmFF19E9gGXJDnt3i0KJxVxG2UKNrIAXM8kCKrk6YNCOyOFXt4JCgw3xivSwIR7moXpMIOIvDWeKnq2RSFY4n2WpWb3IA%2FvZA6BknfyuhKcpycP4FtjIbCMFRSA%2FBXOwj0nnSGd2R&X-Amz-Signature=0cfe8089d2b9488259e647997121d0e1a652c830f84919bbb0fdfcdaf4ebb7d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRC33PHP%2F20261031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261031T031356Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQDisAf%2BICJ5VS803IQLaxkPHlq7j%2Bbz1ckBZ1lCHn1W4gIhAIRaqIaaPAO7Fv0MH7YyjUQWny%2FmX6YTpYW%2B32FckhC3Kv8DCEMQABoMNjM3NDIzMTgzODA1IgyI4pILzz5b4qtcnYUq3ANexNoT6MkLW%2FsCK5tx6vbLvbuKvvx16UEsfKP2fpytGUqUgLLgpGqzztB0LqrtqL2xgnf3mkxzgGr2mYQh1zE9n%2BUDra5eVz9ZRRVAvcbPPzJnJ%2FMIBj%2B2RcceauG3LX%2B%2Bz6NQ54osdf1rj2cQDNrtJIp4qzHzZQZ8SSpN3E7ER2m%2BXW3OvVTbgAx6%2BdfUz8VVjx2bLGhNV%2Bgmaef9cX31KdjsdLAsJ9x5wn%2FYVaAyzWZHggICaNA5GJ2o7%2F%2FfCHvRw2PaZ17aQ695bfhv0zKLW4N%2F2KQu2tgAUk8id%2BCETFpTSHPMluZ7avEvTF9h6HvhNHJJaOKyuszh5b96z22JttA26ELgjG6a1fpLDoO6V8Tl2xkNNyg9QJ9Wx%2Fjy6gaA01IzfiEHEGIloPKlRdCFn6iu9NkjezGcVQL7YZn4Elakzov2yHNGYJERxpzaPXdtiwhsvyDHKcHdJ2HcVFAfdRLHYNkHf2myz0FIilO09cuO4MO7VGngcpEL5UQwG6o2%2FdyzUSHbL6oTgZlv68HrGrrtSnCItYsET1JGIa0qx2NirvP26W2sryQje4r8xQICyIuUNIAb5usm4GGuLQwEvUYgTEEsRRgr4vmPT3VDkKVfz6fTPZkpeyieIjCnlJXXBjqkAcUeODkZWbMFIthbHDA%2BLGuNsRUpRUH4%2FtrcP55y82Wq%2Bd8yV5IiuhJegQFzMqRaMJp4b4%2BnphJrotcDWcMLmFF19E9gGXJDnt3i0KJxVxG2UKNrIAXM8kCKrk6YNCOyOFXt4JCgw3xivSwIR7moXpMIOIvDWeKnq2RSFY4n2WpWb3IA%2FvZA6BknfyuhKcpycP4FtjIbCMFRSA%2FBXOwj0nnSGd2R&X-Amz-Signature=94db2169cef2aa50d53b305e52078ae727ea86a7cf736922d9ec63d1f2217194&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRC33PHP%2F20261031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261031T031356Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQDisAf%2BICJ5VS803IQLaxkPHlq7j%2Bbz1ckBZ1lCHn1W4gIhAIRaqIaaPAO7Fv0MH7YyjUQWny%2FmX6YTpYW%2B32FckhC3Kv8DCEMQABoMNjM3NDIzMTgzODA1IgyI4pILzz5b4qtcnYUq3ANexNoT6MkLW%2FsCK5tx6vbLvbuKvvx16UEsfKP2fpytGUqUgLLgpGqzztB0LqrtqL2xgnf3mkxzgGr2mYQh1zE9n%2BUDra5eVz9ZRRVAvcbPPzJnJ%2FMIBj%2B2RcceauG3LX%2B%2Bz6NQ54osdf1rj2cQDNrtJIp4qzHzZQZ8SSpN3E7ER2m%2BXW3OvVTbgAx6%2BdfUz8VVjx2bLGhNV%2Bgmaef9cX31KdjsdLAsJ9x5wn%2FYVaAyzWZHggICaNA5GJ2o7%2F%2FfCHvRw2PaZ17aQ695bfhv0zKLW4N%2F2KQu2tgAUk8id%2BCETFpTSHPMluZ7avEvTF9h6HvhNHJJaOKyuszh5b96z22JttA26ELgjG6a1fpLDoO6V8Tl2xkNNyg9QJ9Wx%2Fjy6gaA01IzfiEHEGIloPKlRdCFn6iu9NkjezGcVQL7YZn4Elakzov2yHNGYJERxpzaPXdtiwhsvyDHKcHdJ2HcVFAfdRLHYNkHf2myz0FIilO09cuO4MO7VGngcpEL5UQwG6o2%2FdyzUSHbL6oTgZlv68HrGrrtSnCItYsET1JGIa0qx2NirvP26W2sryQje4r8xQICyIuUNIAb5usm4GGuLQwEvUYgTEEsRRgr4vmPT3VDkKVfz6fTPZkpeyieIjCnlJXXBjqkAcUeODkZWbMFIthbHDA%2BLGuNsRUpRUH4%2FtrcP55y82Wq%2Bd8yV5IiuhJegQFzMqRaMJp4b4%2BnphJrotcDWcMLmFF19E9gGXJDnt3i0KJxVxG2UKNrIAXM8kCKrk6YNCOyOFXt4JCgw3xivSwIR7moXpMIOIvDWeKnq2RSFY4n2WpWb3IA%2FvZA6BknfyuhKcpycP4FtjIbCMFRSA%2FBXOwj0nnSGd2R&X-Amz-Signature=d234421ab34415ff54b446a3dcf2abe143570769277e45b35f6f34ab962433a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
