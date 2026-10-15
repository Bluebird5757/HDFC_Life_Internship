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
fetched_at: '2026-10-15T02:19:51.783Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSZCCQIB%2F20261015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261015T021948Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDSbTwMpqt%2BeZjzPYqGluUzyJDyos3iyzeNO%2FnGGsFqGAiEAyqoJsbo1BqPNnxxJAhSLt7ki1uh%2BwSq%2F6AzyKGsj6VkqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLSMtCjK%2FTgksywyVSrcA1d04wmvUODy%2BnTVNmSVlSTUQ8HbtVyVkktDuYjwd8Smp2KGJuweMBWDrlEGwSPelYjYLnZ9cuAWOeZ4JKYzVKvjnN3pBmWGUbHmPpcxSFzx6RGyQ8LzDuhTs4w5QqbS2YxSxLi4wtLOe5lz1YKvf1UWzsUMntS3ToSpUc3NygQbIyyTvS27CqzP4%2FfohlC6fLslsZJhDSH9kPKLy%2Fbnl2gtFZNFc5WiWj5ZjdTVC8AVGaFT5H5nZunR8UP6ng5TLbzc7uc%2Fjtywsu4QTtofZpbNmbEgn5xquOfXFVdBsCKXBbzDSLvgc1ZsxLjsnaOilHlzuP3Q%2F6fGw3jV6bEdrvK5Q%2FCxR0sgxplOUefNNT%2BXCHv5kAR6hLQ7OUNdyy%2FDIu81UBVfJb6v%2BzOcA800zXLYjPkRLNlScft4sd50cx71pDGb1lOzRuUsrIsMgWXkG%2B9hmEOHP9l13UFy650pdMDGRiJM7%2BrG1y9CW3T2%2Baa1AGuOiwHq4sXxAZSW6rOalYZ%2FkCLrgQmCB8rIHYh%2BzHe6L3QcpX9zTVkcIzFN6yFOAWZ15kXjpG4uUXLBmfdZI4YSswb7RIJK3I0CUPxZ8vavlW54qEs2UiXvhPuu0jY4kxRoN60yHVLVibKlMKy%2BwNYGOqUBOXWBqHJi8zE9z1nykmvETelgeoJSBLDEMfsaZhAPIymI1A5ntGuYNfCYsDNntkuckn1AKwpqqbLyRyYpYxX8tpjuYuRlQsqfXXdwgVP63Lt9mPWDXKWfXZD5FEQV4WKRH%2FsA%2FHPdJXHSVsK0bKLHAbs3JSp2j6KmXlG14z0PhxD9U1gJwZVhSZXJqCDAEf64IjNpTOaVscmOBAU%2Bj6SZXr7JkgIx&X-Amz-Signature=9dcb73e7c3f06baec9f90af78412b3e8c9687fe53b57d595afd92f6fd0db26de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSZCCQIB%2F20261015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261015T021948Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDSbTwMpqt%2BeZjzPYqGluUzyJDyos3iyzeNO%2FnGGsFqGAiEAyqoJsbo1BqPNnxxJAhSLt7ki1uh%2BwSq%2F6AzyKGsj6VkqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLSMtCjK%2FTgksywyVSrcA1d04wmvUODy%2BnTVNmSVlSTUQ8HbtVyVkktDuYjwd8Smp2KGJuweMBWDrlEGwSPelYjYLnZ9cuAWOeZ4JKYzVKvjnN3pBmWGUbHmPpcxSFzx6RGyQ8LzDuhTs4w5QqbS2YxSxLi4wtLOe5lz1YKvf1UWzsUMntS3ToSpUc3NygQbIyyTvS27CqzP4%2FfohlC6fLslsZJhDSH9kPKLy%2Fbnl2gtFZNFc5WiWj5ZjdTVC8AVGaFT5H5nZunR8UP6ng5TLbzc7uc%2Fjtywsu4QTtofZpbNmbEgn5xquOfXFVdBsCKXBbzDSLvgc1ZsxLjsnaOilHlzuP3Q%2F6fGw3jV6bEdrvK5Q%2FCxR0sgxplOUefNNT%2BXCHv5kAR6hLQ7OUNdyy%2FDIu81UBVfJb6v%2BzOcA800zXLYjPkRLNlScft4sd50cx71pDGb1lOzRuUsrIsMgWXkG%2B9hmEOHP9l13UFy650pdMDGRiJM7%2BrG1y9CW3T2%2Baa1AGuOiwHq4sXxAZSW6rOalYZ%2FkCLrgQmCB8rIHYh%2BzHe6L3QcpX9zTVkcIzFN6yFOAWZ15kXjpG4uUXLBmfdZI4YSswb7RIJK3I0CUPxZ8vavlW54qEs2UiXvhPuu0jY4kxRoN60yHVLVibKlMKy%2BwNYGOqUBOXWBqHJi8zE9z1nykmvETelgeoJSBLDEMfsaZhAPIymI1A5ntGuYNfCYsDNntkuckn1AKwpqqbLyRyYpYxX8tpjuYuRlQsqfXXdwgVP63Lt9mPWDXKWfXZD5FEQV4WKRH%2FsA%2FHPdJXHSVsK0bKLHAbs3JSp2j6KmXlG14z0PhxD9U1gJwZVhSZXJqCDAEf64IjNpTOaVscmOBAU%2Bj6SZXr7JkgIx&X-Amz-Signature=20e9d4e978379f9585da593aaf11720a997be3c116f8f99504421807a6accc26&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSZCCQIB%2F20261015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261015T021948Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDSbTwMpqt%2BeZjzPYqGluUzyJDyos3iyzeNO%2FnGGsFqGAiEAyqoJsbo1BqPNnxxJAhSLt7ki1uh%2BwSq%2F6AzyKGsj6VkqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLSMtCjK%2FTgksywyVSrcA1d04wmvUODy%2BnTVNmSVlSTUQ8HbtVyVkktDuYjwd8Smp2KGJuweMBWDrlEGwSPelYjYLnZ9cuAWOeZ4JKYzVKvjnN3pBmWGUbHmPpcxSFzx6RGyQ8LzDuhTs4w5QqbS2YxSxLi4wtLOe5lz1YKvf1UWzsUMntS3ToSpUc3NygQbIyyTvS27CqzP4%2FfohlC6fLslsZJhDSH9kPKLy%2Fbnl2gtFZNFc5WiWj5ZjdTVC8AVGaFT5H5nZunR8UP6ng5TLbzc7uc%2Fjtywsu4QTtofZpbNmbEgn5xquOfXFVdBsCKXBbzDSLvgc1ZsxLjsnaOilHlzuP3Q%2F6fGw3jV6bEdrvK5Q%2FCxR0sgxplOUefNNT%2BXCHv5kAR6hLQ7OUNdyy%2FDIu81UBVfJb6v%2BzOcA800zXLYjPkRLNlScft4sd50cx71pDGb1lOzRuUsrIsMgWXkG%2B9hmEOHP9l13UFy650pdMDGRiJM7%2BrG1y9CW3T2%2Baa1AGuOiwHq4sXxAZSW6rOalYZ%2FkCLrgQmCB8rIHYh%2BzHe6L3QcpX9zTVkcIzFN6yFOAWZ15kXjpG4uUXLBmfdZI4YSswb7RIJK3I0CUPxZ8vavlW54qEs2UiXvhPuu0jY4kxRoN60yHVLVibKlMKy%2BwNYGOqUBOXWBqHJi8zE9z1nykmvETelgeoJSBLDEMfsaZhAPIymI1A5ntGuYNfCYsDNntkuckn1AKwpqqbLyRyYpYxX8tpjuYuRlQsqfXXdwgVP63Lt9mPWDXKWfXZD5FEQV4WKRH%2FsA%2FHPdJXHSVsK0bKLHAbs3JSp2j6KmXlG14z0PhxD9U1gJwZVhSZXJqCDAEf64IjNpTOaVscmOBAU%2Bj6SZXr7JkgIx&X-Amz-Signature=5e1189d40bba7cf4d0b03b00b6f233cd0efa4e53d5344186ad6d20a8eecbfcf0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSZCCQIB%2F20261015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261015T021948Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDSbTwMpqt%2BeZjzPYqGluUzyJDyos3iyzeNO%2FnGGsFqGAiEAyqoJsbo1BqPNnxxJAhSLt7ki1uh%2BwSq%2F6AzyKGsj6VkqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLSMtCjK%2FTgksywyVSrcA1d04wmvUODy%2BnTVNmSVlSTUQ8HbtVyVkktDuYjwd8Smp2KGJuweMBWDrlEGwSPelYjYLnZ9cuAWOeZ4JKYzVKvjnN3pBmWGUbHmPpcxSFzx6RGyQ8LzDuhTs4w5QqbS2YxSxLi4wtLOe5lz1YKvf1UWzsUMntS3ToSpUc3NygQbIyyTvS27CqzP4%2FfohlC6fLslsZJhDSH9kPKLy%2Fbnl2gtFZNFc5WiWj5ZjdTVC8AVGaFT5H5nZunR8UP6ng5TLbzc7uc%2Fjtywsu4QTtofZpbNmbEgn5xquOfXFVdBsCKXBbzDSLvgc1ZsxLjsnaOilHlzuP3Q%2F6fGw3jV6bEdrvK5Q%2FCxR0sgxplOUefNNT%2BXCHv5kAR6hLQ7OUNdyy%2FDIu81UBVfJb6v%2BzOcA800zXLYjPkRLNlScft4sd50cx71pDGb1lOzRuUsrIsMgWXkG%2B9hmEOHP9l13UFy650pdMDGRiJM7%2BrG1y9CW3T2%2Baa1AGuOiwHq4sXxAZSW6rOalYZ%2FkCLrgQmCB8rIHYh%2BzHe6L3QcpX9zTVkcIzFN6yFOAWZ15kXjpG4uUXLBmfdZI4YSswb7RIJK3I0CUPxZ8vavlW54qEs2UiXvhPuu0jY4kxRoN60yHVLVibKlMKy%2BwNYGOqUBOXWBqHJi8zE9z1nykmvETelgeoJSBLDEMfsaZhAPIymI1A5ntGuYNfCYsDNntkuckn1AKwpqqbLyRyYpYxX8tpjuYuRlQsqfXXdwgVP63Lt9mPWDXKWfXZD5FEQV4WKRH%2FsA%2FHPdJXHSVsK0bKLHAbs3JSp2j6KmXlG14z0PhxD9U1gJwZVhSZXJqCDAEf64IjNpTOaVscmOBAU%2Bj6SZXr7JkgIx&X-Amz-Signature=0e32b7d43b8bc70ef06daba3808a091c2be1550049094db284b4e22f1de93f6f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSZCCQIB%2F20261015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261015T021948Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDSbTwMpqt%2BeZjzPYqGluUzyJDyos3iyzeNO%2FnGGsFqGAiEAyqoJsbo1BqPNnxxJAhSLt7ki1uh%2BwSq%2F6AzyKGsj6VkqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLSMtCjK%2FTgksywyVSrcA1d04wmvUODy%2BnTVNmSVlSTUQ8HbtVyVkktDuYjwd8Smp2KGJuweMBWDrlEGwSPelYjYLnZ9cuAWOeZ4JKYzVKvjnN3pBmWGUbHmPpcxSFzx6RGyQ8LzDuhTs4w5QqbS2YxSxLi4wtLOe5lz1YKvf1UWzsUMntS3ToSpUc3NygQbIyyTvS27CqzP4%2FfohlC6fLslsZJhDSH9kPKLy%2Fbnl2gtFZNFc5WiWj5ZjdTVC8AVGaFT5H5nZunR8UP6ng5TLbzc7uc%2Fjtywsu4QTtofZpbNmbEgn5xquOfXFVdBsCKXBbzDSLvgc1ZsxLjsnaOilHlzuP3Q%2F6fGw3jV6bEdrvK5Q%2FCxR0sgxplOUefNNT%2BXCHv5kAR6hLQ7OUNdyy%2FDIu81UBVfJb6v%2BzOcA800zXLYjPkRLNlScft4sd50cx71pDGb1lOzRuUsrIsMgWXkG%2B9hmEOHP9l13UFy650pdMDGRiJM7%2BrG1y9CW3T2%2Baa1AGuOiwHq4sXxAZSW6rOalYZ%2FkCLrgQmCB8rIHYh%2BzHe6L3QcpX9zTVkcIzFN6yFOAWZ15kXjpG4uUXLBmfdZI4YSswb7RIJK3I0CUPxZ8vavlW54qEs2UiXvhPuu0jY4kxRoN60yHVLVibKlMKy%2BwNYGOqUBOXWBqHJi8zE9z1nykmvETelgeoJSBLDEMfsaZhAPIymI1A5ntGuYNfCYsDNntkuckn1AKwpqqbLyRyYpYxX8tpjuYuRlQsqfXXdwgVP63Lt9mPWDXKWfXZD5FEQV4WKRH%2FsA%2FHPdJXHSVsK0bKLHAbs3JSp2j6KmXlG14z0PhxD9U1gJwZVhSZXJqCDAEf64IjNpTOaVscmOBAU%2Bj6SZXr7JkgIx&X-Amz-Signature=6fd7a86c8ce582cd0c737071b123444b3026980fd546b6d3781401dca8f1f53a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSZCCQIB%2F20261015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261015T021948Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDSbTwMpqt%2BeZjzPYqGluUzyJDyos3iyzeNO%2FnGGsFqGAiEAyqoJsbo1BqPNnxxJAhSLt7ki1uh%2BwSq%2F6AzyKGsj6VkqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLSMtCjK%2FTgksywyVSrcA1d04wmvUODy%2BnTVNmSVlSTUQ8HbtVyVkktDuYjwd8Smp2KGJuweMBWDrlEGwSPelYjYLnZ9cuAWOeZ4JKYzVKvjnN3pBmWGUbHmPpcxSFzx6RGyQ8LzDuhTs4w5QqbS2YxSxLi4wtLOe5lz1YKvf1UWzsUMntS3ToSpUc3NygQbIyyTvS27CqzP4%2FfohlC6fLslsZJhDSH9kPKLy%2Fbnl2gtFZNFc5WiWj5ZjdTVC8AVGaFT5H5nZunR8UP6ng5TLbzc7uc%2Fjtywsu4QTtofZpbNmbEgn5xquOfXFVdBsCKXBbzDSLvgc1ZsxLjsnaOilHlzuP3Q%2F6fGw3jV6bEdrvK5Q%2FCxR0sgxplOUefNNT%2BXCHv5kAR6hLQ7OUNdyy%2FDIu81UBVfJb6v%2BzOcA800zXLYjPkRLNlScft4sd50cx71pDGb1lOzRuUsrIsMgWXkG%2B9hmEOHP9l13UFy650pdMDGRiJM7%2BrG1y9CW3T2%2Baa1AGuOiwHq4sXxAZSW6rOalYZ%2FkCLrgQmCB8rIHYh%2BzHe6L3QcpX9zTVkcIzFN6yFOAWZ15kXjpG4uUXLBmfdZI4YSswb7RIJK3I0CUPxZ8vavlW54qEs2UiXvhPuu0jY4kxRoN60yHVLVibKlMKy%2BwNYGOqUBOXWBqHJi8zE9z1nykmvETelgeoJSBLDEMfsaZhAPIymI1A5ntGuYNfCYsDNntkuckn1AKwpqqbLyRyYpYxX8tpjuYuRlQsqfXXdwgVP63Lt9mPWDXKWfXZD5FEQV4WKRH%2FsA%2FHPdJXHSVsK0bKLHAbs3JSp2j6KmXlG14z0PhxD9U1gJwZVhSZXJqCDAEf64IjNpTOaVscmOBAU%2Bj6SZXr7JkgIx&X-Amz-Signature=b3606f706e940dcd3a64eba1d202af6a44b3ed3e152cf9ab165dd44f7e43c497&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSZCCQIB%2F20261015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261015T021948Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDSbTwMpqt%2BeZjzPYqGluUzyJDyos3iyzeNO%2FnGGsFqGAiEAyqoJsbo1BqPNnxxJAhSLt7ki1uh%2BwSq%2F6AzyKGsj6VkqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLSMtCjK%2FTgksywyVSrcA1d04wmvUODy%2BnTVNmSVlSTUQ8HbtVyVkktDuYjwd8Smp2KGJuweMBWDrlEGwSPelYjYLnZ9cuAWOeZ4JKYzVKvjnN3pBmWGUbHmPpcxSFzx6RGyQ8LzDuhTs4w5QqbS2YxSxLi4wtLOe5lz1YKvf1UWzsUMntS3ToSpUc3NygQbIyyTvS27CqzP4%2FfohlC6fLslsZJhDSH9kPKLy%2Fbnl2gtFZNFc5WiWj5ZjdTVC8AVGaFT5H5nZunR8UP6ng5TLbzc7uc%2Fjtywsu4QTtofZpbNmbEgn5xquOfXFVdBsCKXBbzDSLvgc1ZsxLjsnaOilHlzuP3Q%2F6fGw3jV6bEdrvK5Q%2FCxR0sgxplOUefNNT%2BXCHv5kAR6hLQ7OUNdyy%2FDIu81UBVfJb6v%2BzOcA800zXLYjPkRLNlScft4sd50cx71pDGb1lOzRuUsrIsMgWXkG%2B9hmEOHP9l13UFy650pdMDGRiJM7%2BrG1y9CW3T2%2Baa1AGuOiwHq4sXxAZSW6rOalYZ%2FkCLrgQmCB8rIHYh%2BzHe6L3QcpX9zTVkcIzFN6yFOAWZ15kXjpG4uUXLBmfdZI4YSswb7RIJK3I0CUPxZ8vavlW54qEs2UiXvhPuu0jY4kxRoN60yHVLVibKlMKy%2BwNYGOqUBOXWBqHJi8zE9z1nykmvETelgeoJSBLDEMfsaZhAPIymI1A5ntGuYNfCYsDNntkuckn1AKwpqqbLyRyYpYxX8tpjuYuRlQsqfXXdwgVP63Lt9mPWDXKWfXZD5FEQV4WKRH%2FsA%2FHPdJXHSVsK0bKLHAbs3JSp2j6KmXlG14z0PhxD9U1gJwZVhSZXJqCDAEf64IjNpTOaVscmOBAU%2Bj6SZXr7JkgIx&X-Amz-Signature=a3bd62eb4a5c81ceea704d0811c81f613081a4b7d71466bb01cffc4dc3a38947&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSZCCQIB%2F20261015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261015T021948Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDSbTwMpqt%2BeZjzPYqGluUzyJDyos3iyzeNO%2FnGGsFqGAiEAyqoJsbo1BqPNnxxJAhSLt7ki1uh%2BwSq%2F6AzyKGsj6VkqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLSMtCjK%2FTgksywyVSrcA1d04wmvUODy%2BnTVNmSVlSTUQ8HbtVyVkktDuYjwd8Smp2KGJuweMBWDrlEGwSPelYjYLnZ9cuAWOeZ4JKYzVKvjnN3pBmWGUbHmPpcxSFzx6RGyQ8LzDuhTs4w5QqbS2YxSxLi4wtLOe5lz1YKvf1UWzsUMntS3ToSpUc3NygQbIyyTvS27CqzP4%2FfohlC6fLslsZJhDSH9kPKLy%2Fbnl2gtFZNFc5WiWj5ZjdTVC8AVGaFT5H5nZunR8UP6ng5TLbzc7uc%2Fjtywsu4QTtofZpbNmbEgn5xquOfXFVdBsCKXBbzDSLvgc1ZsxLjsnaOilHlzuP3Q%2F6fGw3jV6bEdrvK5Q%2FCxR0sgxplOUefNNT%2BXCHv5kAR6hLQ7OUNdyy%2FDIu81UBVfJb6v%2BzOcA800zXLYjPkRLNlScft4sd50cx71pDGb1lOzRuUsrIsMgWXkG%2B9hmEOHP9l13UFy650pdMDGRiJM7%2BrG1y9CW3T2%2Baa1AGuOiwHq4sXxAZSW6rOalYZ%2FkCLrgQmCB8rIHYh%2BzHe6L3QcpX9zTVkcIzFN6yFOAWZ15kXjpG4uUXLBmfdZI4YSswb7RIJK3I0CUPxZ8vavlW54qEs2UiXvhPuu0jY4kxRoN60yHVLVibKlMKy%2BwNYGOqUBOXWBqHJi8zE9z1nykmvETelgeoJSBLDEMfsaZhAPIymI1A5ntGuYNfCYsDNntkuckn1AKwpqqbLyRyYpYxX8tpjuYuRlQsqfXXdwgVP63Lt9mPWDXKWfXZD5FEQV4WKRH%2FsA%2FHPdJXHSVsK0bKLHAbs3JSp2j6KmXlG14z0PhxD9U1gJwZVhSZXJqCDAEf64IjNpTOaVscmOBAU%2Bj6SZXr7JkgIx&X-Amz-Signature=5e877a446c4879744cac0368bef07890ea95140716cb13b79ecd27fa99da9035&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSZCCQIB%2F20261015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261015T021948Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDSbTwMpqt%2BeZjzPYqGluUzyJDyos3iyzeNO%2FnGGsFqGAiEAyqoJsbo1BqPNnxxJAhSLt7ki1uh%2BwSq%2F6AzyKGsj6VkqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLSMtCjK%2FTgksywyVSrcA1d04wmvUODy%2BnTVNmSVlSTUQ8HbtVyVkktDuYjwd8Smp2KGJuweMBWDrlEGwSPelYjYLnZ9cuAWOeZ4JKYzVKvjnN3pBmWGUbHmPpcxSFzx6RGyQ8LzDuhTs4w5QqbS2YxSxLi4wtLOe5lz1YKvf1UWzsUMntS3ToSpUc3NygQbIyyTvS27CqzP4%2FfohlC6fLslsZJhDSH9kPKLy%2Fbnl2gtFZNFc5WiWj5ZjdTVC8AVGaFT5H5nZunR8UP6ng5TLbzc7uc%2Fjtywsu4QTtofZpbNmbEgn5xquOfXFVdBsCKXBbzDSLvgc1ZsxLjsnaOilHlzuP3Q%2F6fGw3jV6bEdrvK5Q%2FCxR0sgxplOUefNNT%2BXCHv5kAR6hLQ7OUNdyy%2FDIu81UBVfJb6v%2BzOcA800zXLYjPkRLNlScft4sd50cx71pDGb1lOzRuUsrIsMgWXkG%2B9hmEOHP9l13UFy650pdMDGRiJM7%2BrG1y9CW3T2%2Baa1AGuOiwHq4sXxAZSW6rOalYZ%2FkCLrgQmCB8rIHYh%2BzHe6L3QcpX9zTVkcIzFN6yFOAWZ15kXjpG4uUXLBmfdZI4YSswb7RIJK3I0CUPxZ8vavlW54qEs2UiXvhPuu0jY4kxRoN60yHVLVibKlMKy%2BwNYGOqUBOXWBqHJi8zE9z1nykmvETelgeoJSBLDEMfsaZhAPIymI1A5ntGuYNfCYsDNntkuckn1AKwpqqbLyRyYpYxX8tpjuYuRlQsqfXXdwgVP63Lt9mPWDXKWfXZD5FEQV4WKRH%2FsA%2FHPdJXHSVsK0bKLHAbs3JSp2j6KmXlG14z0PhxD9U1gJwZVhSZXJqCDAEf64IjNpTOaVscmOBAU%2Bj6SZXr7JkgIx&X-Amz-Signature=c05c0d2aa47e34a9145815c7d143e599a532794ef6e68d74efe5aba532f609c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
