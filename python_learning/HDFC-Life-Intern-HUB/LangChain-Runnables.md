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
fetched_at: '2026-11-16T03:47:12.218Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Y7GFJWQ%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034707Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5TOh%2FwsCArIRCa0pl62noEqggW9EoyoyzE1gXOpjXugIgAoe7Nv0Jm1P9KP5x55LXmNNc68n109qGKbyBzm9P6WcqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEd%2BNk0Mvd%2F3oPH6JircAxwMwFPjZRUpV%2BOlRFYlP30vCbVCNmQ%2Fgeha0ZJShGqG%2F2BrtROJR1yp3%2BbISMNZaxKfE3xHR6LRtTFvYYZF8zyEmZn3AtVf6R%2BpinEFG2HyvVXtqqFboAT%2FW3yG4kk2xtGGD7o8HdsYedt%2FwCr4BaW6kjPlSU%2BHzgNI15inogvtAiN0hquqQgFtUHaEvp0E376sC07tOtGSZdTrcD5R7LU%2F5nIs5DQ%2Bg5Ic0LU48ZVJGvd35y7QIAxPTMg%2FwaPEZBq%2BOj5taoK3Oj7P4F6q0TV3DfB0DZx2m4ZH%2BhGXU0ef5ui23fCNmVpjFnK4UT0Y3INu82pV4J01y3YCQYEeBK9qkImiak5b4Z5bbs8Umfvnx4QZSyyCNYDFBvB4riO6pJC4qnercfG9mJT6RyTUwT976GHXU78OWRx8qq43Pi1fCPdn0O6v8HIvSbECXHEilEFMSyfZ%2BqLVIlUJ%2Fu4faR5HEWKY9%2BcjOucPJgLJLljU5brk7zlkenJrIBAg1YvmepiEBIdCoQKIAmwyHz1%2FLv7ALqbzgb2%2FBkkyonKy3qWDqIlKtEIJjP2Y5u50EE2h1My7Aa8nmn2wzHhrPuf3dNvkqUMK%2FqxQ%2BX%2FbwXEh0o9aFmzcvrEThDSLczjyMOzJ6dcGOqUBytC6tvt8zLBaneSgXucXoRMrxbbo7MgNkUdKZ2bVl5ax4emoOS4uYElEuGmLvnLqNAvjGkGpoaP9202kePM41TdHA1EysEhfkvCrPitULYEffs5W0RXEfTSPxRsqruR3kdnFkgRN8vKP85AiIVPCED5qCo8qK%2FxfPuoqe3BOlWc9B3F0uGwWmlUeM8286DhHBAfG4xTITOYG%2BKAyIdZ9Tj37Ep4l&X-Amz-Signature=790d49e4fb0bdd1c0b40db4755989482fcf7060e1e1a89ae7332e6b9ac8ce4af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Y7GFJWQ%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034707Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5TOh%2FwsCArIRCa0pl62noEqggW9EoyoyzE1gXOpjXugIgAoe7Nv0Jm1P9KP5x55LXmNNc68n109qGKbyBzm9P6WcqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEd%2BNk0Mvd%2F3oPH6JircAxwMwFPjZRUpV%2BOlRFYlP30vCbVCNmQ%2Fgeha0ZJShGqG%2F2BrtROJR1yp3%2BbISMNZaxKfE3xHR6LRtTFvYYZF8zyEmZn3AtVf6R%2BpinEFG2HyvVXtqqFboAT%2FW3yG4kk2xtGGD7o8HdsYedt%2FwCr4BaW6kjPlSU%2BHzgNI15inogvtAiN0hquqQgFtUHaEvp0E376sC07tOtGSZdTrcD5R7LU%2F5nIs5DQ%2Bg5Ic0LU48ZVJGvd35y7QIAxPTMg%2FwaPEZBq%2BOj5taoK3Oj7P4F6q0TV3DfB0DZx2m4ZH%2BhGXU0ef5ui23fCNmVpjFnK4UT0Y3INu82pV4J01y3YCQYEeBK9qkImiak5b4Z5bbs8Umfvnx4QZSyyCNYDFBvB4riO6pJC4qnercfG9mJT6RyTUwT976GHXU78OWRx8qq43Pi1fCPdn0O6v8HIvSbECXHEilEFMSyfZ%2BqLVIlUJ%2Fu4faR5HEWKY9%2BcjOucPJgLJLljU5brk7zlkenJrIBAg1YvmepiEBIdCoQKIAmwyHz1%2FLv7ALqbzgb2%2FBkkyonKy3qWDqIlKtEIJjP2Y5u50EE2h1My7Aa8nmn2wzHhrPuf3dNvkqUMK%2FqxQ%2BX%2FbwXEh0o9aFmzcvrEThDSLczjyMOzJ6dcGOqUBytC6tvt8zLBaneSgXucXoRMrxbbo7MgNkUdKZ2bVl5ax4emoOS4uYElEuGmLvnLqNAvjGkGpoaP9202kePM41TdHA1EysEhfkvCrPitULYEffs5W0RXEfTSPxRsqruR3kdnFkgRN8vKP85AiIVPCED5qCo8qK%2FxfPuoqe3BOlWc9B3F0uGwWmlUeM8286DhHBAfG4xTITOYG%2BKAyIdZ9Tj37Ep4l&X-Amz-Signature=d258c257b6fc617afdf754ac7c4714a5cef7301c3c61a4adf4f9a081183400c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Y7GFJWQ%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034707Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5TOh%2FwsCArIRCa0pl62noEqggW9EoyoyzE1gXOpjXugIgAoe7Nv0Jm1P9KP5x55LXmNNc68n109qGKbyBzm9P6WcqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEd%2BNk0Mvd%2F3oPH6JircAxwMwFPjZRUpV%2BOlRFYlP30vCbVCNmQ%2Fgeha0ZJShGqG%2F2BrtROJR1yp3%2BbISMNZaxKfE3xHR6LRtTFvYYZF8zyEmZn3AtVf6R%2BpinEFG2HyvVXtqqFboAT%2FW3yG4kk2xtGGD7o8HdsYedt%2FwCr4BaW6kjPlSU%2BHzgNI15inogvtAiN0hquqQgFtUHaEvp0E376sC07tOtGSZdTrcD5R7LU%2F5nIs5DQ%2Bg5Ic0LU48ZVJGvd35y7QIAxPTMg%2FwaPEZBq%2BOj5taoK3Oj7P4F6q0TV3DfB0DZx2m4ZH%2BhGXU0ef5ui23fCNmVpjFnK4UT0Y3INu82pV4J01y3YCQYEeBK9qkImiak5b4Z5bbs8Umfvnx4QZSyyCNYDFBvB4riO6pJC4qnercfG9mJT6RyTUwT976GHXU78OWRx8qq43Pi1fCPdn0O6v8HIvSbECXHEilEFMSyfZ%2BqLVIlUJ%2Fu4faR5HEWKY9%2BcjOucPJgLJLljU5brk7zlkenJrIBAg1YvmepiEBIdCoQKIAmwyHz1%2FLv7ALqbzgb2%2FBkkyonKy3qWDqIlKtEIJjP2Y5u50EE2h1My7Aa8nmn2wzHhrPuf3dNvkqUMK%2FqxQ%2BX%2FbwXEh0o9aFmzcvrEThDSLczjyMOzJ6dcGOqUBytC6tvt8zLBaneSgXucXoRMrxbbo7MgNkUdKZ2bVl5ax4emoOS4uYElEuGmLvnLqNAvjGkGpoaP9202kePM41TdHA1EysEhfkvCrPitULYEffs5W0RXEfTSPxRsqruR3kdnFkgRN8vKP85AiIVPCED5qCo8qK%2FxfPuoqe3BOlWc9B3F0uGwWmlUeM8286DhHBAfG4xTITOYG%2BKAyIdZ9Tj37Ep4l&X-Amz-Signature=571e2ee4a74490a2dcd68348885aee26104f907e3e8978da8251e727ff2b6e3d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Y7GFJWQ%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034707Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5TOh%2FwsCArIRCa0pl62noEqggW9EoyoyzE1gXOpjXugIgAoe7Nv0Jm1P9KP5x55LXmNNc68n109qGKbyBzm9P6WcqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEd%2BNk0Mvd%2F3oPH6JircAxwMwFPjZRUpV%2BOlRFYlP30vCbVCNmQ%2Fgeha0ZJShGqG%2F2BrtROJR1yp3%2BbISMNZaxKfE3xHR6LRtTFvYYZF8zyEmZn3AtVf6R%2BpinEFG2HyvVXtqqFboAT%2FW3yG4kk2xtGGD7o8HdsYedt%2FwCr4BaW6kjPlSU%2BHzgNI15inogvtAiN0hquqQgFtUHaEvp0E376sC07tOtGSZdTrcD5R7LU%2F5nIs5DQ%2Bg5Ic0LU48ZVJGvd35y7QIAxPTMg%2FwaPEZBq%2BOj5taoK3Oj7P4F6q0TV3DfB0DZx2m4ZH%2BhGXU0ef5ui23fCNmVpjFnK4UT0Y3INu82pV4J01y3YCQYEeBK9qkImiak5b4Z5bbs8Umfvnx4QZSyyCNYDFBvB4riO6pJC4qnercfG9mJT6RyTUwT976GHXU78OWRx8qq43Pi1fCPdn0O6v8HIvSbECXHEilEFMSyfZ%2BqLVIlUJ%2Fu4faR5HEWKY9%2BcjOucPJgLJLljU5brk7zlkenJrIBAg1YvmepiEBIdCoQKIAmwyHz1%2FLv7ALqbzgb2%2FBkkyonKy3qWDqIlKtEIJjP2Y5u50EE2h1My7Aa8nmn2wzHhrPuf3dNvkqUMK%2FqxQ%2BX%2FbwXEh0o9aFmzcvrEThDSLczjyMOzJ6dcGOqUBytC6tvt8zLBaneSgXucXoRMrxbbo7MgNkUdKZ2bVl5ax4emoOS4uYElEuGmLvnLqNAvjGkGpoaP9202kePM41TdHA1EysEhfkvCrPitULYEffs5W0RXEfTSPxRsqruR3kdnFkgRN8vKP85AiIVPCED5qCo8qK%2FxfPuoqe3BOlWc9B3F0uGwWmlUeM8286DhHBAfG4xTITOYG%2BKAyIdZ9Tj37Ep4l&X-Amz-Signature=9fb8642f3a6612bfc242fca9dcd3ff767362af8d72bf5d0550f4c50a315ceb8d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Y7GFJWQ%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034707Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5TOh%2FwsCArIRCa0pl62noEqggW9EoyoyzE1gXOpjXugIgAoe7Nv0Jm1P9KP5x55LXmNNc68n109qGKbyBzm9P6WcqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEd%2BNk0Mvd%2F3oPH6JircAxwMwFPjZRUpV%2BOlRFYlP30vCbVCNmQ%2Fgeha0ZJShGqG%2F2BrtROJR1yp3%2BbISMNZaxKfE3xHR6LRtTFvYYZF8zyEmZn3AtVf6R%2BpinEFG2HyvVXtqqFboAT%2FW3yG4kk2xtGGD7o8HdsYedt%2FwCr4BaW6kjPlSU%2BHzgNI15inogvtAiN0hquqQgFtUHaEvp0E376sC07tOtGSZdTrcD5R7LU%2F5nIs5DQ%2Bg5Ic0LU48ZVJGvd35y7QIAxPTMg%2FwaPEZBq%2BOj5taoK3Oj7P4F6q0TV3DfB0DZx2m4ZH%2BhGXU0ef5ui23fCNmVpjFnK4UT0Y3INu82pV4J01y3YCQYEeBK9qkImiak5b4Z5bbs8Umfvnx4QZSyyCNYDFBvB4riO6pJC4qnercfG9mJT6RyTUwT976GHXU78OWRx8qq43Pi1fCPdn0O6v8HIvSbECXHEilEFMSyfZ%2BqLVIlUJ%2Fu4faR5HEWKY9%2BcjOucPJgLJLljU5brk7zlkenJrIBAg1YvmepiEBIdCoQKIAmwyHz1%2FLv7ALqbzgb2%2FBkkyonKy3qWDqIlKtEIJjP2Y5u50EE2h1My7Aa8nmn2wzHhrPuf3dNvkqUMK%2FqxQ%2BX%2FbwXEh0o9aFmzcvrEThDSLczjyMOzJ6dcGOqUBytC6tvt8zLBaneSgXucXoRMrxbbo7MgNkUdKZ2bVl5ax4emoOS4uYElEuGmLvnLqNAvjGkGpoaP9202kePM41TdHA1EysEhfkvCrPitULYEffs5W0RXEfTSPxRsqruR3kdnFkgRN8vKP85AiIVPCED5qCo8qK%2FxfPuoqe3BOlWc9B3F0uGwWmlUeM8286DhHBAfG4xTITOYG%2BKAyIdZ9Tj37Ep4l&X-Amz-Signature=613723f719461b998157cb21bf145fc127c9fad1f686251213cec992c01d9884&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Y7GFJWQ%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034707Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5TOh%2FwsCArIRCa0pl62noEqggW9EoyoyzE1gXOpjXugIgAoe7Nv0Jm1P9KP5x55LXmNNc68n109qGKbyBzm9P6WcqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEd%2BNk0Mvd%2F3oPH6JircAxwMwFPjZRUpV%2BOlRFYlP30vCbVCNmQ%2Fgeha0ZJShGqG%2F2BrtROJR1yp3%2BbISMNZaxKfE3xHR6LRtTFvYYZF8zyEmZn3AtVf6R%2BpinEFG2HyvVXtqqFboAT%2FW3yG4kk2xtGGD7o8HdsYedt%2FwCr4BaW6kjPlSU%2BHzgNI15inogvtAiN0hquqQgFtUHaEvp0E376sC07tOtGSZdTrcD5R7LU%2F5nIs5DQ%2Bg5Ic0LU48ZVJGvd35y7QIAxPTMg%2FwaPEZBq%2BOj5taoK3Oj7P4F6q0TV3DfB0DZx2m4ZH%2BhGXU0ef5ui23fCNmVpjFnK4UT0Y3INu82pV4J01y3YCQYEeBK9qkImiak5b4Z5bbs8Umfvnx4QZSyyCNYDFBvB4riO6pJC4qnercfG9mJT6RyTUwT976GHXU78OWRx8qq43Pi1fCPdn0O6v8HIvSbECXHEilEFMSyfZ%2BqLVIlUJ%2Fu4faR5HEWKY9%2BcjOucPJgLJLljU5brk7zlkenJrIBAg1YvmepiEBIdCoQKIAmwyHz1%2FLv7ALqbzgb2%2FBkkyonKy3qWDqIlKtEIJjP2Y5u50EE2h1My7Aa8nmn2wzHhrPuf3dNvkqUMK%2FqxQ%2BX%2FbwXEh0o9aFmzcvrEThDSLczjyMOzJ6dcGOqUBytC6tvt8zLBaneSgXucXoRMrxbbo7MgNkUdKZ2bVl5ax4emoOS4uYElEuGmLvnLqNAvjGkGpoaP9202kePM41TdHA1EysEhfkvCrPitULYEffs5W0RXEfTSPxRsqruR3kdnFkgRN8vKP85AiIVPCED5qCo8qK%2FxfPuoqe3BOlWc9B3F0uGwWmlUeM8286DhHBAfG4xTITOYG%2BKAyIdZ9Tj37Ep4l&X-Amz-Signature=859bd1ddd3095d0247eaab5e1adc43e0b3f45787d970c768f51b042fa858290f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Y7GFJWQ%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034707Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5TOh%2FwsCArIRCa0pl62noEqggW9EoyoyzE1gXOpjXugIgAoe7Nv0Jm1P9KP5x55LXmNNc68n109qGKbyBzm9P6WcqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEd%2BNk0Mvd%2F3oPH6JircAxwMwFPjZRUpV%2BOlRFYlP30vCbVCNmQ%2Fgeha0ZJShGqG%2F2BrtROJR1yp3%2BbISMNZaxKfE3xHR6LRtTFvYYZF8zyEmZn3AtVf6R%2BpinEFG2HyvVXtqqFboAT%2FW3yG4kk2xtGGD7o8HdsYedt%2FwCr4BaW6kjPlSU%2BHzgNI15inogvtAiN0hquqQgFtUHaEvp0E376sC07tOtGSZdTrcD5R7LU%2F5nIs5DQ%2Bg5Ic0LU48ZVJGvd35y7QIAxPTMg%2FwaPEZBq%2BOj5taoK3Oj7P4F6q0TV3DfB0DZx2m4ZH%2BhGXU0ef5ui23fCNmVpjFnK4UT0Y3INu82pV4J01y3YCQYEeBK9qkImiak5b4Z5bbs8Umfvnx4QZSyyCNYDFBvB4riO6pJC4qnercfG9mJT6RyTUwT976GHXU78OWRx8qq43Pi1fCPdn0O6v8HIvSbECXHEilEFMSyfZ%2BqLVIlUJ%2Fu4faR5HEWKY9%2BcjOucPJgLJLljU5brk7zlkenJrIBAg1YvmepiEBIdCoQKIAmwyHz1%2FLv7ALqbzgb2%2FBkkyonKy3qWDqIlKtEIJjP2Y5u50EE2h1My7Aa8nmn2wzHhrPuf3dNvkqUMK%2FqxQ%2BX%2FbwXEh0o9aFmzcvrEThDSLczjyMOzJ6dcGOqUBytC6tvt8zLBaneSgXucXoRMrxbbo7MgNkUdKZ2bVl5ax4emoOS4uYElEuGmLvnLqNAvjGkGpoaP9202kePM41TdHA1EysEhfkvCrPitULYEffs5W0RXEfTSPxRsqruR3kdnFkgRN8vKP85AiIVPCED5qCo8qK%2FxfPuoqe3BOlWc9B3F0uGwWmlUeM8286DhHBAfG4xTITOYG%2BKAyIdZ9Tj37Ep4l&X-Amz-Signature=1a72a3c1ed9309450b45c33f11abfc988380af4c1f0a9157f02f68d97e3b3128&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Y7GFJWQ%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034707Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5TOh%2FwsCArIRCa0pl62noEqggW9EoyoyzE1gXOpjXugIgAoe7Nv0Jm1P9KP5x55LXmNNc68n109qGKbyBzm9P6WcqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEd%2BNk0Mvd%2F3oPH6JircAxwMwFPjZRUpV%2BOlRFYlP30vCbVCNmQ%2Fgeha0ZJShGqG%2F2BrtROJR1yp3%2BbISMNZaxKfE3xHR6LRtTFvYYZF8zyEmZn3AtVf6R%2BpinEFG2HyvVXtqqFboAT%2FW3yG4kk2xtGGD7o8HdsYedt%2FwCr4BaW6kjPlSU%2BHzgNI15inogvtAiN0hquqQgFtUHaEvp0E376sC07tOtGSZdTrcD5R7LU%2F5nIs5DQ%2Bg5Ic0LU48ZVJGvd35y7QIAxPTMg%2FwaPEZBq%2BOj5taoK3Oj7P4F6q0TV3DfB0DZx2m4ZH%2BhGXU0ef5ui23fCNmVpjFnK4UT0Y3INu82pV4J01y3YCQYEeBK9qkImiak5b4Z5bbs8Umfvnx4QZSyyCNYDFBvB4riO6pJC4qnercfG9mJT6RyTUwT976GHXU78OWRx8qq43Pi1fCPdn0O6v8HIvSbECXHEilEFMSyfZ%2BqLVIlUJ%2Fu4faR5HEWKY9%2BcjOucPJgLJLljU5brk7zlkenJrIBAg1YvmepiEBIdCoQKIAmwyHz1%2FLv7ALqbzgb2%2FBkkyonKy3qWDqIlKtEIJjP2Y5u50EE2h1My7Aa8nmn2wzHhrPuf3dNvkqUMK%2FqxQ%2BX%2FbwXEh0o9aFmzcvrEThDSLczjyMOzJ6dcGOqUBytC6tvt8zLBaneSgXucXoRMrxbbo7MgNkUdKZ2bVl5ax4emoOS4uYElEuGmLvnLqNAvjGkGpoaP9202kePM41TdHA1EysEhfkvCrPitULYEffs5W0RXEfTSPxRsqruR3kdnFkgRN8vKP85AiIVPCED5qCo8qK%2FxfPuoqe3BOlWc9B3F0uGwWmlUeM8286DhHBAfG4xTITOYG%2BKAyIdZ9Tj37Ep4l&X-Amz-Signature=6d02050a2e447ba159f98e86bfda62c5bbf28c1fd98c452d53a7adaf781f9a2d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Y7GFJWQ%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034707Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5TOh%2FwsCArIRCa0pl62noEqggW9EoyoyzE1gXOpjXugIgAoe7Nv0Jm1P9KP5x55LXmNNc68n109qGKbyBzm9P6WcqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEd%2BNk0Mvd%2F3oPH6JircAxwMwFPjZRUpV%2BOlRFYlP30vCbVCNmQ%2Fgeha0ZJShGqG%2F2BrtROJR1yp3%2BbISMNZaxKfE3xHR6LRtTFvYYZF8zyEmZn3AtVf6R%2BpinEFG2HyvVXtqqFboAT%2FW3yG4kk2xtGGD7o8HdsYedt%2FwCr4BaW6kjPlSU%2BHzgNI15inogvtAiN0hquqQgFtUHaEvp0E376sC07tOtGSZdTrcD5R7LU%2F5nIs5DQ%2Bg5Ic0LU48ZVJGvd35y7QIAxPTMg%2FwaPEZBq%2BOj5taoK3Oj7P4F6q0TV3DfB0DZx2m4ZH%2BhGXU0ef5ui23fCNmVpjFnK4UT0Y3INu82pV4J01y3YCQYEeBK9qkImiak5b4Z5bbs8Umfvnx4QZSyyCNYDFBvB4riO6pJC4qnercfG9mJT6RyTUwT976GHXU78OWRx8qq43Pi1fCPdn0O6v8HIvSbECXHEilEFMSyfZ%2BqLVIlUJ%2Fu4faR5HEWKY9%2BcjOucPJgLJLljU5brk7zlkenJrIBAg1YvmepiEBIdCoQKIAmwyHz1%2FLv7ALqbzgb2%2FBkkyonKy3qWDqIlKtEIJjP2Y5u50EE2h1My7Aa8nmn2wzHhrPuf3dNvkqUMK%2FqxQ%2BX%2FbwXEh0o9aFmzcvrEThDSLczjyMOzJ6dcGOqUBytC6tvt8zLBaneSgXucXoRMrxbbo7MgNkUdKZ2bVl5ax4emoOS4uYElEuGmLvnLqNAvjGkGpoaP9202kePM41TdHA1EysEhfkvCrPitULYEffs5W0RXEfTSPxRsqruR3kdnFkgRN8vKP85AiIVPCED5qCo8qK%2FxfPuoqe3BOlWc9B3F0uGwWmlUeM8286DhHBAfG4xTITOYG%2BKAyIdZ9Tj37Ep4l&X-Amz-Signature=3d7e08fe11b4fb8120b0cc4c3f3153090d608dd4574d34c1f0b26e7a91391daf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
