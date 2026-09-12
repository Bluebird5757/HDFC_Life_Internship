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
fetched_at: '2026-09-12T02:06:38.652Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRY2QZOG%2F20260912%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260912T020629Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBiMgGkqsomWB4pCGe9KCOV3Mijye5WBwbBR618yrpCAAiA1AXyHZALJGWJFxwiii5nQIiABcMs2jA0cqR1GaesjEiqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdYNPKPO%2FxcBMlLVYKtwDihAbVYfK77lVr8ldr8i4H1crIron0YAu3B1RsPZPJ1BbaIu3PgDomfzaqswYkKA1QSjFEwhPPzkTCE0b8XH5Tuqt1qHBg7C86GHUsTE5dEA%2BcQlHAJduQvczbBMzf4K7vHngm94euycU1b3m%2FsoMh%2Bo3dQAyuyFHW4vgfR0eiYkNHFR75iBAkWDUWslWP2mYQ0jWu%2FW1pLiixasNzaDsgJYEVHv8%2BztGXJkXhflMj2p2WVcQmzOpJdG4LS3xVtRgZcogTJZhzMmMFJFnmohiqguzeSyX3tw6vN5AF1gviqyCqKNlmxLOsouK%2FJz%2BAHxttPqplJy5cI78MB4NLgbzBpiw6ogpK%2BMvV%2FVMWBj3fwRquU4OW2b1B%2B%2BucW3cDq%2B0shi86xhtZArvTrk%2FSCJB8bXcq%2FlDECljv6U3x%2Fj0J145DKQ%2BLxa0Dr454AgvYq2%2BBykoLPQjFsRV1CtH%2FAY2OJ%2Fo8l2ljsuWlWukI6a1MCH3lXldMcquBCIQKx0ZuholUzxLBPBi1EXuYmZjNZFFqVWA5j5YDtydcbpMN4M%2B1gOX2tcnDjg3AZpNUP2ypC7nknw9%2FgDVtbt2nMLZW9g%2B%2FOd7j90V9QGJJFtAYQV2vK7oIJR6J4UXg7%2B4uLYwhdKS1QY6pgGlgQruaOWl2kzQrj%2BSSjFVG65MhOYM8Iy5lHOyG6rkn6JsZkAX2SwYNkdt%2FIvdIPMQgdqal10wiprqNzJIahSVDjKczuDKPDrvkuTWAwXoXnQW6C0W%2FoVPL9UWTNFlnc%2FD7xy7tXBK9CCQJuDeLJcG56Ek1XyrEBFI%2B5xfPlatr6T%2BI1pBYjJnnoF%2Fq4P4OGsBHJGbAsdSrVJ7wW%2FdVMF4yRvjFaBW&X-Amz-Signature=9c8664470e4744199838c88035d2d52f3ce804a442fc8756d19af5867c183beb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRY2QZOG%2F20260912%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260912T020629Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBiMgGkqsomWB4pCGe9KCOV3Mijye5WBwbBR618yrpCAAiA1AXyHZALJGWJFxwiii5nQIiABcMs2jA0cqR1GaesjEiqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdYNPKPO%2FxcBMlLVYKtwDihAbVYfK77lVr8ldr8i4H1crIron0YAu3B1RsPZPJ1BbaIu3PgDomfzaqswYkKA1QSjFEwhPPzkTCE0b8XH5Tuqt1qHBg7C86GHUsTE5dEA%2BcQlHAJduQvczbBMzf4K7vHngm94euycU1b3m%2FsoMh%2Bo3dQAyuyFHW4vgfR0eiYkNHFR75iBAkWDUWslWP2mYQ0jWu%2FW1pLiixasNzaDsgJYEVHv8%2BztGXJkXhflMj2p2WVcQmzOpJdG4LS3xVtRgZcogTJZhzMmMFJFnmohiqguzeSyX3tw6vN5AF1gviqyCqKNlmxLOsouK%2FJz%2BAHxttPqplJy5cI78MB4NLgbzBpiw6ogpK%2BMvV%2FVMWBj3fwRquU4OW2b1B%2B%2BucW3cDq%2B0shi86xhtZArvTrk%2FSCJB8bXcq%2FlDECljv6U3x%2Fj0J145DKQ%2BLxa0Dr454AgvYq2%2BBykoLPQjFsRV1CtH%2FAY2OJ%2Fo8l2ljsuWlWukI6a1MCH3lXldMcquBCIQKx0ZuholUzxLBPBi1EXuYmZjNZFFqVWA5j5YDtydcbpMN4M%2B1gOX2tcnDjg3AZpNUP2ypC7nknw9%2FgDVtbt2nMLZW9g%2B%2FOd7j90V9QGJJFtAYQV2vK7oIJR6J4UXg7%2B4uLYwhdKS1QY6pgGlgQruaOWl2kzQrj%2BSSjFVG65MhOYM8Iy5lHOyG6rkn6JsZkAX2SwYNkdt%2FIvdIPMQgdqal10wiprqNzJIahSVDjKczuDKPDrvkuTWAwXoXnQW6C0W%2FoVPL9UWTNFlnc%2FD7xy7tXBK9CCQJuDeLJcG56Ek1XyrEBFI%2B5xfPlatr6T%2BI1pBYjJnnoF%2Fq4P4OGsBHJGbAsdSrVJ7wW%2FdVMF4yRvjFaBW&X-Amz-Signature=a4f79dff831f25754a956cceac81a623a3c497f01fb326ce4058479e1f3e2d61&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRY2QZOG%2F20260912%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260912T020629Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBiMgGkqsomWB4pCGe9KCOV3Mijye5WBwbBR618yrpCAAiA1AXyHZALJGWJFxwiii5nQIiABcMs2jA0cqR1GaesjEiqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdYNPKPO%2FxcBMlLVYKtwDihAbVYfK77lVr8ldr8i4H1crIron0YAu3B1RsPZPJ1BbaIu3PgDomfzaqswYkKA1QSjFEwhPPzkTCE0b8XH5Tuqt1qHBg7C86GHUsTE5dEA%2BcQlHAJduQvczbBMzf4K7vHngm94euycU1b3m%2FsoMh%2Bo3dQAyuyFHW4vgfR0eiYkNHFR75iBAkWDUWslWP2mYQ0jWu%2FW1pLiixasNzaDsgJYEVHv8%2BztGXJkXhflMj2p2WVcQmzOpJdG4LS3xVtRgZcogTJZhzMmMFJFnmohiqguzeSyX3tw6vN5AF1gviqyCqKNlmxLOsouK%2FJz%2BAHxttPqplJy5cI78MB4NLgbzBpiw6ogpK%2BMvV%2FVMWBj3fwRquU4OW2b1B%2B%2BucW3cDq%2B0shi86xhtZArvTrk%2FSCJB8bXcq%2FlDECljv6U3x%2Fj0J145DKQ%2BLxa0Dr454AgvYq2%2BBykoLPQjFsRV1CtH%2FAY2OJ%2Fo8l2ljsuWlWukI6a1MCH3lXldMcquBCIQKx0ZuholUzxLBPBi1EXuYmZjNZFFqVWA5j5YDtydcbpMN4M%2B1gOX2tcnDjg3AZpNUP2ypC7nknw9%2FgDVtbt2nMLZW9g%2B%2FOd7j90V9QGJJFtAYQV2vK7oIJR6J4UXg7%2B4uLYwhdKS1QY6pgGlgQruaOWl2kzQrj%2BSSjFVG65MhOYM8Iy5lHOyG6rkn6JsZkAX2SwYNkdt%2FIvdIPMQgdqal10wiprqNzJIahSVDjKczuDKPDrvkuTWAwXoXnQW6C0W%2FoVPL9UWTNFlnc%2FD7xy7tXBK9CCQJuDeLJcG56Ek1XyrEBFI%2B5xfPlatr6T%2BI1pBYjJnnoF%2Fq4P4OGsBHJGbAsdSrVJ7wW%2FdVMF4yRvjFaBW&X-Amz-Signature=1189257cee6f8aba7f1739521b7cd00f555c3478d2c68b266334ea2c05bf968a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRY2QZOG%2F20260912%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260912T020629Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBiMgGkqsomWB4pCGe9KCOV3Mijye5WBwbBR618yrpCAAiA1AXyHZALJGWJFxwiii5nQIiABcMs2jA0cqR1GaesjEiqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdYNPKPO%2FxcBMlLVYKtwDihAbVYfK77lVr8ldr8i4H1crIron0YAu3B1RsPZPJ1BbaIu3PgDomfzaqswYkKA1QSjFEwhPPzkTCE0b8XH5Tuqt1qHBg7C86GHUsTE5dEA%2BcQlHAJduQvczbBMzf4K7vHngm94euycU1b3m%2FsoMh%2Bo3dQAyuyFHW4vgfR0eiYkNHFR75iBAkWDUWslWP2mYQ0jWu%2FW1pLiixasNzaDsgJYEVHv8%2BztGXJkXhflMj2p2WVcQmzOpJdG4LS3xVtRgZcogTJZhzMmMFJFnmohiqguzeSyX3tw6vN5AF1gviqyCqKNlmxLOsouK%2FJz%2BAHxttPqplJy5cI78MB4NLgbzBpiw6ogpK%2BMvV%2FVMWBj3fwRquU4OW2b1B%2B%2BucW3cDq%2B0shi86xhtZArvTrk%2FSCJB8bXcq%2FlDECljv6U3x%2Fj0J145DKQ%2BLxa0Dr454AgvYq2%2BBykoLPQjFsRV1CtH%2FAY2OJ%2Fo8l2ljsuWlWukI6a1MCH3lXldMcquBCIQKx0ZuholUzxLBPBi1EXuYmZjNZFFqVWA5j5YDtydcbpMN4M%2B1gOX2tcnDjg3AZpNUP2ypC7nknw9%2FgDVtbt2nMLZW9g%2B%2FOd7j90V9QGJJFtAYQV2vK7oIJR6J4UXg7%2B4uLYwhdKS1QY6pgGlgQruaOWl2kzQrj%2BSSjFVG65MhOYM8Iy5lHOyG6rkn6JsZkAX2SwYNkdt%2FIvdIPMQgdqal10wiprqNzJIahSVDjKczuDKPDrvkuTWAwXoXnQW6C0W%2FoVPL9UWTNFlnc%2FD7xy7tXBK9CCQJuDeLJcG56Ek1XyrEBFI%2B5xfPlatr6T%2BI1pBYjJnnoF%2Fq4P4OGsBHJGbAsdSrVJ7wW%2FdVMF4yRvjFaBW&X-Amz-Signature=70df11f2550670274bd141bc43ee322dc154c839edcfdbfdcc8a45f7d56a9d8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRY2QZOG%2F20260912%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260912T020629Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBiMgGkqsomWB4pCGe9KCOV3Mijye5WBwbBR618yrpCAAiA1AXyHZALJGWJFxwiii5nQIiABcMs2jA0cqR1GaesjEiqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdYNPKPO%2FxcBMlLVYKtwDihAbVYfK77lVr8ldr8i4H1crIron0YAu3B1RsPZPJ1BbaIu3PgDomfzaqswYkKA1QSjFEwhPPzkTCE0b8XH5Tuqt1qHBg7C86GHUsTE5dEA%2BcQlHAJduQvczbBMzf4K7vHngm94euycU1b3m%2FsoMh%2Bo3dQAyuyFHW4vgfR0eiYkNHFR75iBAkWDUWslWP2mYQ0jWu%2FW1pLiixasNzaDsgJYEVHv8%2BztGXJkXhflMj2p2WVcQmzOpJdG4LS3xVtRgZcogTJZhzMmMFJFnmohiqguzeSyX3tw6vN5AF1gviqyCqKNlmxLOsouK%2FJz%2BAHxttPqplJy5cI78MB4NLgbzBpiw6ogpK%2BMvV%2FVMWBj3fwRquU4OW2b1B%2B%2BucW3cDq%2B0shi86xhtZArvTrk%2FSCJB8bXcq%2FlDECljv6U3x%2Fj0J145DKQ%2BLxa0Dr454AgvYq2%2BBykoLPQjFsRV1CtH%2FAY2OJ%2Fo8l2ljsuWlWukI6a1MCH3lXldMcquBCIQKx0ZuholUzxLBPBi1EXuYmZjNZFFqVWA5j5YDtydcbpMN4M%2B1gOX2tcnDjg3AZpNUP2ypC7nknw9%2FgDVtbt2nMLZW9g%2B%2FOd7j90V9QGJJFtAYQV2vK7oIJR6J4UXg7%2B4uLYwhdKS1QY6pgGlgQruaOWl2kzQrj%2BSSjFVG65MhOYM8Iy5lHOyG6rkn6JsZkAX2SwYNkdt%2FIvdIPMQgdqal10wiprqNzJIahSVDjKczuDKPDrvkuTWAwXoXnQW6C0W%2FoVPL9UWTNFlnc%2FD7xy7tXBK9CCQJuDeLJcG56Ek1XyrEBFI%2B5xfPlatr6T%2BI1pBYjJnnoF%2Fq4P4OGsBHJGbAsdSrVJ7wW%2FdVMF4yRvjFaBW&X-Amz-Signature=3a71b258966e4840ed0b6cccd5f2dc1549681538b031bcd952e85e715a95a24b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRY2QZOG%2F20260912%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260912T020629Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBiMgGkqsomWB4pCGe9KCOV3Mijye5WBwbBR618yrpCAAiA1AXyHZALJGWJFxwiii5nQIiABcMs2jA0cqR1GaesjEiqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdYNPKPO%2FxcBMlLVYKtwDihAbVYfK77lVr8ldr8i4H1crIron0YAu3B1RsPZPJ1BbaIu3PgDomfzaqswYkKA1QSjFEwhPPzkTCE0b8XH5Tuqt1qHBg7C86GHUsTE5dEA%2BcQlHAJduQvczbBMzf4K7vHngm94euycU1b3m%2FsoMh%2Bo3dQAyuyFHW4vgfR0eiYkNHFR75iBAkWDUWslWP2mYQ0jWu%2FW1pLiixasNzaDsgJYEVHv8%2BztGXJkXhflMj2p2WVcQmzOpJdG4LS3xVtRgZcogTJZhzMmMFJFnmohiqguzeSyX3tw6vN5AF1gviqyCqKNlmxLOsouK%2FJz%2BAHxttPqplJy5cI78MB4NLgbzBpiw6ogpK%2BMvV%2FVMWBj3fwRquU4OW2b1B%2B%2BucW3cDq%2B0shi86xhtZArvTrk%2FSCJB8bXcq%2FlDECljv6U3x%2Fj0J145DKQ%2BLxa0Dr454AgvYq2%2BBykoLPQjFsRV1CtH%2FAY2OJ%2Fo8l2ljsuWlWukI6a1MCH3lXldMcquBCIQKx0ZuholUzxLBPBi1EXuYmZjNZFFqVWA5j5YDtydcbpMN4M%2B1gOX2tcnDjg3AZpNUP2ypC7nknw9%2FgDVtbt2nMLZW9g%2B%2FOd7j90V9QGJJFtAYQV2vK7oIJR6J4UXg7%2B4uLYwhdKS1QY6pgGlgQruaOWl2kzQrj%2BSSjFVG65MhOYM8Iy5lHOyG6rkn6JsZkAX2SwYNkdt%2FIvdIPMQgdqal10wiprqNzJIahSVDjKczuDKPDrvkuTWAwXoXnQW6C0W%2FoVPL9UWTNFlnc%2FD7xy7tXBK9CCQJuDeLJcG56Ek1XyrEBFI%2B5xfPlatr6T%2BI1pBYjJnnoF%2Fq4P4OGsBHJGbAsdSrVJ7wW%2FdVMF4yRvjFaBW&X-Amz-Signature=63f94f204abcaad9bc05653bcb58c51258f96ff89c50c8e52eea45a6ab026558&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRY2QZOG%2F20260912%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260912T020629Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBiMgGkqsomWB4pCGe9KCOV3Mijye5WBwbBR618yrpCAAiA1AXyHZALJGWJFxwiii5nQIiABcMs2jA0cqR1GaesjEiqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdYNPKPO%2FxcBMlLVYKtwDihAbVYfK77lVr8ldr8i4H1crIron0YAu3B1RsPZPJ1BbaIu3PgDomfzaqswYkKA1QSjFEwhPPzkTCE0b8XH5Tuqt1qHBg7C86GHUsTE5dEA%2BcQlHAJduQvczbBMzf4K7vHngm94euycU1b3m%2FsoMh%2Bo3dQAyuyFHW4vgfR0eiYkNHFR75iBAkWDUWslWP2mYQ0jWu%2FW1pLiixasNzaDsgJYEVHv8%2BztGXJkXhflMj2p2WVcQmzOpJdG4LS3xVtRgZcogTJZhzMmMFJFnmohiqguzeSyX3tw6vN5AF1gviqyCqKNlmxLOsouK%2FJz%2BAHxttPqplJy5cI78MB4NLgbzBpiw6ogpK%2BMvV%2FVMWBj3fwRquU4OW2b1B%2B%2BucW3cDq%2B0shi86xhtZArvTrk%2FSCJB8bXcq%2FlDECljv6U3x%2Fj0J145DKQ%2BLxa0Dr454AgvYq2%2BBykoLPQjFsRV1CtH%2FAY2OJ%2Fo8l2ljsuWlWukI6a1MCH3lXldMcquBCIQKx0ZuholUzxLBPBi1EXuYmZjNZFFqVWA5j5YDtydcbpMN4M%2B1gOX2tcnDjg3AZpNUP2ypC7nknw9%2FgDVtbt2nMLZW9g%2B%2FOd7j90V9QGJJFtAYQV2vK7oIJR6J4UXg7%2B4uLYwhdKS1QY6pgGlgQruaOWl2kzQrj%2BSSjFVG65MhOYM8Iy5lHOyG6rkn6JsZkAX2SwYNkdt%2FIvdIPMQgdqal10wiprqNzJIahSVDjKczuDKPDrvkuTWAwXoXnQW6C0W%2FoVPL9UWTNFlnc%2FD7xy7tXBK9CCQJuDeLJcG56Ek1XyrEBFI%2B5xfPlatr6T%2BI1pBYjJnnoF%2Fq4P4OGsBHJGbAsdSrVJ7wW%2FdVMF4yRvjFaBW&X-Amz-Signature=fe16846ce65edeb0c6086199464084787fe83363b69a7682128c58890d845862&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRY2QZOG%2F20260912%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260912T020629Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBiMgGkqsomWB4pCGe9KCOV3Mijye5WBwbBR618yrpCAAiA1AXyHZALJGWJFxwiii5nQIiABcMs2jA0cqR1GaesjEiqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdYNPKPO%2FxcBMlLVYKtwDihAbVYfK77lVr8ldr8i4H1crIron0YAu3B1RsPZPJ1BbaIu3PgDomfzaqswYkKA1QSjFEwhPPzkTCE0b8XH5Tuqt1qHBg7C86GHUsTE5dEA%2BcQlHAJduQvczbBMzf4K7vHngm94euycU1b3m%2FsoMh%2Bo3dQAyuyFHW4vgfR0eiYkNHFR75iBAkWDUWslWP2mYQ0jWu%2FW1pLiixasNzaDsgJYEVHv8%2BztGXJkXhflMj2p2WVcQmzOpJdG4LS3xVtRgZcogTJZhzMmMFJFnmohiqguzeSyX3tw6vN5AF1gviqyCqKNlmxLOsouK%2FJz%2BAHxttPqplJy5cI78MB4NLgbzBpiw6ogpK%2BMvV%2FVMWBj3fwRquU4OW2b1B%2B%2BucW3cDq%2B0shi86xhtZArvTrk%2FSCJB8bXcq%2FlDECljv6U3x%2Fj0J145DKQ%2BLxa0Dr454AgvYq2%2BBykoLPQjFsRV1CtH%2FAY2OJ%2Fo8l2ljsuWlWukI6a1MCH3lXldMcquBCIQKx0ZuholUzxLBPBi1EXuYmZjNZFFqVWA5j5YDtydcbpMN4M%2B1gOX2tcnDjg3AZpNUP2ypC7nknw9%2FgDVtbt2nMLZW9g%2B%2FOd7j90V9QGJJFtAYQV2vK7oIJR6J4UXg7%2B4uLYwhdKS1QY6pgGlgQruaOWl2kzQrj%2BSSjFVG65MhOYM8Iy5lHOyG6rkn6JsZkAX2SwYNkdt%2FIvdIPMQgdqal10wiprqNzJIahSVDjKczuDKPDrvkuTWAwXoXnQW6C0W%2FoVPL9UWTNFlnc%2FD7xy7tXBK9CCQJuDeLJcG56Ek1XyrEBFI%2B5xfPlatr6T%2BI1pBYjJnnoF%2Fq4P4OGsBHJGbAsdSrVJ7wW%2FdVMF4yRvjFaBW&X-Amz-Signature=ff14a3db0604735349b67ae275ff0b5d7a016388bad96aa7915a5284ed39049a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRY2QZOG%2F20260912%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260912T020629Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBiMgGkqsomWB4pCGe9KCOV3Mijye5WBwbBR618yrpCAAiA1AXyHZALJGWJFxwiii5nQIiABcMs2jA0cqR1GaesjEiqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdYNPKPO%2FxcBMlLVYKtwDihAbVYfK77lVr8ldr8i4H1crIron0YAu3B1RsPZPJ1BbaIu3PgDomfzaqswYkKA1QSjFEwhPPzkTCE0b8XH5Tuqt1qHBg7C86GHUsTE5dEA%2BcQlHAJduQvczbBMzf4K7vHngm94euycU1b3m%2FsoMh%2Bo3dQAyuyFHW4vgfR0eiYkNHFR75iBAkWDUWslWP2mYQ0jWu%2FW1pLiixasNzaDsgJYEVHv8%2BztGXJkXhflMj2p2WVcQmzOpJdG4LS3xVtRgZcogTJZhzMmMFJFnmohiqguzeSyX3tw6vN5AF1gviqyCqKNlmxLOsouK%2FJz%2BAHxttPqplJy5cI78MB4NLgbzBpiw6ogpK%2BMvV%2FVMWBj3fwRquU4OW2b1B%2B%2BucW3cDq%2B0shi86xhtZArvTrk%2FSCJB8bXcq%2FlDECljv6U3x%2Fj0J145DKQ%2BLxa0Dr454AgvYq2%2BBykoLPQjFsRV1CtH%2FAY2OJ%2Fo8l2ljsuWlWukI6a1MCH3lXldMcquBCIQKx0ZuholUzxLBPBi1EXuYmZjNZFFqVWA5j5YDtydcbpMN4M%2B1gOX2tcnDjg3AZpNUP2ypC7nknw9%2FgDVtbt2nMLZW9g%2B%2FOd7j90V9QGJJFtAYQV2vK7oIJR6J4UXg7%2B4uLYwhdKS1QY6pgGlgQruaOWl2kzQrj%2BSSjFVG65MhOYM8Iy5lHOyG6rkn6JsZkAX2SwYNkdt%2FIvdIPMQgdqal10wiprqNzJIahSVDjKczuDKPDrvkuTWAwXoXnQW6C0W%2FoVPL9UWTNFlnc%2FD7xy7tXBK9CCQJuDeLJcG56Ek1XyrEBFI%2B5xfPlatr6T%2BI1pBYjJnnoF%2Fq4P4OGsBHJGbAsdSrVJ7wW%2FdVMF4yRvjFaBW&X-Amz-Signature=305b6afeacac4a97d284da567a89705175c6fa337ce1aeb420d58f663fba4306&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
