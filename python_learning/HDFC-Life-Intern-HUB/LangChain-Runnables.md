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
fetched_at: '2026-09-08T02:01:37.371Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDVTK6RO%2F20260908%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260908T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID8il7ys2GBfb2OSxoXAsIXNzyMFSNlTDmji5KH%2FkQfNAiEAhqVRaUzeyyC4Jr88kBW7wI8YTkjGrb201GUrx7GA9koq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDE%2Ba9nYDv3pxym2aPSrcA%2BpCG4137b4yUFl3zaDWWuVBBP80weZwUg%2F7n0lwiswe616ghbfKPc8LeH8odKkGLJJYqqBserkAcjrRNHFz01ciVooOUMjFCR8GYXnQ%2FOSbp7I7RNmODu3eGsRgnS37EBy7UTnGiDJvSaaFFr1t%2BM8oiKTTE2a96a4HvbNBL8toGoE%2FiKVmh39S9YMb7Lc2fnX%2BxUAHOUkmDg9kU%2BRkXvy%2F1MqbzGv9E5TBKOMjbB1oMgtUIa9hKeDlihkZMLViIU88q06XdKbM1aGFt5Ve56ono5xx5vcGoj0mAEbYjBpUlKB4Fg67s5v0knxeUmZjDCcn0KkpQ8Kf4uLcYx4Y3U1cq8DPU7RitO%2BPi02lRRU%2F2br9DWK32hV4H3L64MkzitlwetU2rALV56J%2FMKkqHhmBZW6KzdvsqB9z9B8C0C%2FEEJd5iucUxUqbNHBgXyMJ5AMQMX1MZMfkoY4SdC9iwW500c6OFaduMIe0lS21jQ5RB9TVWsZqljcoJsf%2Bah%2F2c5MeraNUwplEd1WHzGQAz8tqGAxd%2Fko9vGUBcFlPbd0rHScIf41lmZRPxJlB9JlTV2XTXtPutT7dh1lrCNy4FU26aVpcK08xxok8lhsKOE5x1Xl%2FWM%2Fqdh0wVlWrML%2FF%2FdQGOqUBqkV2oCED98BgDxnwgMbvKfTIG7jldDwGLZZNfm5riTKowgjPDcp7JTQFP45sBCovlUeR0%2BzwrI01Y5ZwAw5gdloRryalxRzJQiKWRuy4blKj80%2F0B4WDeNf6K%2F3tYvrLe3kkhC%2BSOTTv7ve1uusLnelwtHMdOSi1dTkcorWnwhay0UZAM9yqdFEVFPwrdQxUFYtmkxfk3za%2FQKuMj3BBYjf6x5wk&X-Amz-Signature=8a90cd6851bbe7979a17261f589c99d6ee647a174fba90589a1ee7046351732f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDVTK6RO%2F20260908%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260908T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID8il7ys2GBfb2OSxoXAsIXNzyMFSNlTDmji5KH%2FkQfNAiEAhqVRaUzeyyC4Jr88kBW7wI8YTkjGrb201GUrx7GA9koq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDE%2Ba9nYDv3pxym2aPSrcA%2BpCG4137b4yUFl3zaDWWuVBBP80weZwUg%2F7n0lwiswe616ghbfKPc8LeH8odKkGLJJYqqBserkAcjrRNHFz01ciVooOUMjFCR8GYXnQ%2FOSbp7I7RNmODu3eGsRgnS37EBy7UTnGiDJvSaaFFr1t%2BM8oiKTTE2a96a4HvbNBL8toGoE%2FiKVmh39S9YMb7Lc2fnX%2BxUAHOUkmDg9kU%2BRkXvy%2F1MqbzGv9E5TBKOMjbB1oMgtUIa9hKeDlihkZMLViIU88q06XdKbM1aGFt5Ve56ono5xx5vcGoj0mAEbYjBpUlKB4Fg67s5v0knxeUmZjDCcn0KkpQ8Kf4uLcYx4Y3U1cq8DPU7RitO%2BPi02lRRU%2F2br9DWK32hV4H3L64MkzitlwetU2rALV56J%2FMKkqHhmBZW6KzdvsqB9z9B8C0C%2FEEJd5iucUxUqbNHBgXyMJ5AMQMX1MZMfkoY4SdC9iwW500c6OFaduMIe0lS21jQ5RB9TVWsZqljcoJsf%2Bah%2F2c5MeraNUwplEd1WHzGQAz8tqGAxd%2Fko9vGUBcFlPbd0rHScIf41lmZRPxJlB9JlTV2XTXtPutT7dh1lrCNy4FU26aVpcK08xxok8lhsKOE5x1Xl%2FWM%2Fqdh0wVlWrML%2FF%2FdQGOqUBqkV2oCED98BgDxnwgMbvKfTIG7jldDwGLZZNfm5riTKowgjPDcp7JTQFP45sBCovlUeR0%2BzwrI01Y5ZwAw5gdloRryalxRzJQiKWRuy4blKj80%2F0B4WDeNf6K%2F3tYvrLe3kkhC%2BSOTTv7ve1uusLnelwtHMdOSi1dTkcorWnwhay0UZAM9yqdFEVFPwrdQxUFYtmkxfk3za%2FQKuMj3BBYjf6x5wk&X-Amz-Signature=c1832462e7483fc9f0400030ad34c43acf50b4f496bc115faee232f8eeae7c79&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDVTK6RO%2F20260908%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260908T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID8il7ys2GBfb2OSxoXAsIXNzyMFSNlTDmji5KH%2FkQfNAiEAhqVRaUzeyyC4Jr88kBW7wI8YTkjGrb201GUrx7GA9koq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDE%2Ba9nYDv3pxym2aPSrcA%2BpCG4137b4yUFl3zaDWWuVBBP80weZwUg%2F7n0lwiswe616ghbfKPc8LeH8odKkGLJJYqqBserkAcjrRNHFz01ciVooOUMjFCR8GYXnQ%2FOSbp7I7RNmODu3eGsRgnS37EBy7UTnGiDJvSaaFFr1t%2BM8oiKTTE2a96a4HvbNBL8toGoE%2FiKVmh39S9YMb7Lc2fnX%2BxUAHOUkmDg9kU%2BRkXvy%2F1MqbzGv9E5TBKOMjbB1oMgtUIa9hKeDlihkZMLViIU88q06XdKbM1aGFt5Ve56ono5xx5vcGoj0mAEbYjBpUlKB4Fg67s5v0knxeUmZjDCcn0KkpQ8Kf4uLcYx4Y3U1cq8DPU7RitO%2BPi02lRRU%2F2br9DWK32hV4H3L64MkzitlwetU2rALV56J%2FMKkqHhmBZW6KzdvsqB9z9B8C0C%2FEEJd5iucUxUqbNHBgXyMJ5AMQMX1MZMfkoY4SdC9iwW500c6OFaduMIe0lS21jQ5RB9TVWsZqljcoJsf%2Bah%2F2c5MeraNUwplEd1WHzGQAz8tqGAxd%2Fko9vGUBcFlPbd0rHScIf41lmZRPxJlB9JlTV2XTXtPutT7dh1lrCNy4FU26aVpcK08xxok8lhsKOE5x1Xl%2FWM%2Fqdh0wVlWrML%2FF%2FdQGOqUBqkV2oCED98BgDxnwgMbvKfTIG7jldDwGLZZNfm5riTKowgjPDcp7JTQFP45sBCovlUeR0%2BzwrI01Y5ZwAw5gdloRryalxRzJQiKWRuy4blKj80%2F0B4WDeNf6K%2F3tYvrLe3kkhC%2BSOTTv7ve1uusLnelwtHMdOSi1dTkcorWnwhay0UZAM9yqdFEVFPwrdQxUFYtmkxfk3za%2FQKuMj3BBYjf6x5wk&X-Amz-Signature=11962b108f387f42fafcbb312335f2f16a052abae30d1d9aa50f60429f4dfd94&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDVTK6RO%2F20260908%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260908T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID8il7ys2GBfb2OSxoXAsIXNzyMFSNlTDmji5KH%2FkQfNAiEAhqVRaUzeyyC4Jr88kBW7wI8YTkjGrb201GUrx7GA9koq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDE%2Ba9nYDv3pxym2aPSrcA%2BpCG4137b4yUFl3zaDWWuVBBP80weZwUg%2F7n0lwiswe616ghbfKPc8LeH8odKkGLJJYqqBserkAcjrRNHFz01ciVooOUMjFCR8GYXnQ%2FOSbp7I7RNmODu3eGsRgnS37EBy7UTnGiDJvSaaFFr1t%2BM8oiKTTE2a96a4HvbNBL8toGoE%2FiKVmh39S9YMb7Lc2fnX%2BxUAHOUkmDg9kU%2BRkXvy%2F1MqbzGv9E5TBKOMjbB1oMgtUIa9hKeDlihkZMLViIU88q06XdKbM1aGFt5Ve56ono5xx5vcGoj0mAEbYjBpUlKB4Fg67s5v0knxeUmZjDCcn0KkpQ8Kf4uLcYx4Y3U1cq8DPU7RitO%2BPi02lRRU%2F2br9DWK32hV4H3L64MkzitlwetU2rALV56J%2FMKkqHhmBZW6KzdvsqB9z9B8C0C%2FEEJd5iucUxUqbNHBgXyMJ5AMQMX1MZMfkoY4SdC9iwW500c6OFaduMIe0lS21jQ5RB9TVWsZqljcoJsf%2Bah%2F2c5MeraNUwplEd1WHzGQAz8tqGAxd%2Fko9vGUBcFlPbd0rHScIf41lmZRPxJlB9JlTV2XTXtPutT7dh1lrCNy4FU26aVpcK08xxok8lhsKOE5x1Xl%2FWM%2Fqdh0wVlWrML%2FF%2FdQGOqUBqkV2oCED98BgDxnwgMbvKfTIG7jldDwGLZZNfm5riTKowgjPDcp7JTQFP45sBCovlUeR0%2BzwrI01Y5ZwAw5gdloRryalxRzJQiKWRuy4blKj80%2F0B4WDeNf6K%2F3tYvrLe3kkhC%2BSOTTv7ve1uusLnelwtHMdOSi1dTkcorWnwhay0UZAM9yqdFEVFPwrdQxUFYtmkxfk3za%2FQKuMj3BBYjf6x5wk&X-Amz-Signature=da321d894791e6e75f04a26ae7ca163aab2bcd13145813944a3469c9e5ef9fb7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDVTK6RO%2F20260908%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260908T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID8il7ys2GBfb2OSxoXAsIXNzyMFSNlTDmji5KH%2FkQfNAiEAhqVRaUzeyyC4Jr88kBW7wI8YTkjGrb201GUrx7GA9koq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDE%2Ba9nYDv3pxym2aPSrcA%2BpCG4137b4yUFl3zaDWWuVBBP80weZwUg%2F7n0lwiswe616ghbfKPc8LeH8odKkGLJJYqqBserkAcjrRNHFz01ciVooOUMjFCR8GYXnQ%2FOSbp7I7RNmODu3eGsRgnS37EBy7UTnGiDJvSaaFFr1t%2BM8oiKTTE2a96a4HvbNBL8toGoE%2FiKVmh39S9YMb7Lc2fnX%2BxUAHOUkmDg9kU%2BRkXvy%2F1MqbzGv9E5TBKOMjbB1oMgtUIa9hKeDlihkZMLViIU88q06XdKbM1aGFt5Ve56ono5xx5vcGoj0mAEbYjBpUlKB4Fg67s5v0knxeUmZjDCcn0KkpQ8Kf4uLcYx4Y3U1cq8DPU7RitO%2BPi02lRRU%2F2br9DWK32hV4H3L64MkzitlwetU2rALV56J%2FMKkqHhmBZW6KzdvsqB9z9B8C0C%2FEEJd5iucUxUqbNHBgXyMJ5AMQMX1MZMfkoY4SdC9iwW500c6OFaduMIe0lS21jQ5RB9TVWsZqljcoJsf%2Bah%2F2c5MeraNUwplEd1WHzGQAz8tqGAxd%2Fko9vGUBcFlPbd0rHScIf41lmZRPxJlB9JlTV2XTXtPutT7dh1lrCNy4FU26aVpcK08xxok8lhsKOE5x1Xl%2FWM%2Fqdh0wVlWrML%2FF%2FdQGOqUBqkV2oCED98BgDxnwgMbvKfTIG7jldDwGLZZNfm5riTKowgjPDcp7JTQFP45sBCovlUeR0%2BzwrI01Y5ZwAw5gdloRryalxRzJQiKWRuy4blKj80%2F0B4WDeNf6K%2F3tYvrLe3kkhC%2BSOTTv7ve1uusLnelwtHMdOSi1dTkcorWnwhay0UZAM9yqdFEVFPwrdQxUFYtmkxfk3za%2FQKuMj3BBYjf6x5wk&X-Amz-Signature=4168e6adbeeaba67773125544535e5787be9b59996cfb2d61876babf1eea56a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDVTK6RO%2F20260908%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260908T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID8il7ys2GBfb2OSxoXAsIXNzyMFSNlTDmji5KH%2FkQfNAiEAhqVRaUzeyyC4Jr88kBW7wI8YTkjGrb201GUrx7GA9koq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDE%2Ba9nYDv3pxym2aPSrcA%2BpCG4137b4yUFl3zaDWWuVBBP80weZwUg%2F7n0lwiswe616ghbfKPc8LeH8odKkGLJJYqqBserkAcjrRNHFz01ciVooOUMjFCR8GYXnQ%2FOSbp7I7RNmODu3eGsRgnS37EBy7UTnGiDJvSaaFFr1t%2BM8oiKTTE2a96a4HvbNBL8toGoE%2FiKVmh39S9YMb7Lc2fnX%2BxUAHOUkmDg9kU%2BRkXvy%2F1MqbzGv9E5TBKOMjbB1oMgtUIa9hKeDlihkZMLViIU88q06XdKbM1aGFt5Ve56ono5xx5vcGoj0mAEbYjBpUlKB4Fg67s5v0knxeUmZjDCcn0KkpQ8Kf4uLcYx4Y3U1cq8DPU7RitO%2BPi02lRRU%2F2br9DWK32hV4H3L64MkzitlwetU2rALV56J%2FMKkqHhmBZW6KzdvsqB9z9B8C0C%2FEEJd5iucUxUqbNHBgXyMJ5AMQMX1MZMfkoY4SdC9iwW500c6OFaduMIe0lS21jQ5RB9TVWsZqljcoJsf%2Bah%2F2c5MeraNUwplEd1WHzGQAz8tqGAxd%2Fko9vGUBcFlPbd0rHScIf41lmZRPxJlB9JlTV2XTXtPutT7dh1lrCNy4FU26aVpcK08xxok8lhsKOE5x1Xl%2FWM%2Fqdh0wVlWrML%2FF%2FdQGOqUBqkV2oCED98BgDxnwgMbvKfTIG7jldDwGLZZNfm5riTKowgjPDcp7JTQFP45sBCovlUeR0%2BzwrI01Y5ZwAw5gdloRryalxRzJQiKWRuy4blKj80%2F0B4WDeNf6K%2F3tYvrLe3kkhC%2BSOTTv7ve1uusLnelwtHMdOSi1dTkcorWnwhay0UZAM9yqdFEVFPwrdQxUFYtmkxfk3za%2FQKuMj3BBYjf6x5wk&X-Amz-Signature=8ef53cecc17b263ebcd5fdb6ccb418cb55ed58e319eb4f8cc6ac0cd72c04425b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDVTK6RO%2F20260908%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260908T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID8il7ys2GBfb2OSxoXAsIXNzyMFSNlTDmji5KH%2FkQfNAiEAhqVRaUzeyyC4Jr88kBW7wI8YTkjGrb201GUrx7GA9koq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDE%2Ba9nYDv3pxym2aPSrcA%2BpCG4137b4yUFl3zaDWWuVBBP80weZwUg%2F7n0lwiswe616ghbfKPc8LeH8odKkGLJJYqqBserkAcjrRNHFz01ciVooOUMjFCR8GYXnQ%2FOSbp7I7RNmODu3eGsRgnS37EBy7UTnGiDJvSaaFFr1t%2BM8oiKTTE2a96a4HvbNBL8toGoE%2FiKVmh39S9YMb7Lc2fnX%2BxUAHOUkmDg9kU%2BRkXvy%2F1MqbzGv9E5TBKOMjbB1oMgtUIa9hKeDlihkZMLViIU88q06XdKbM1aGFt5Ve56ono5xx5vcGoj0mAEbYjBpUlKB4Fg67s5v0knxeUmZjDCcn0KkpQ8Kf4uLcYx4Y3U1cq8DPU7RitO%2BPi02lRRU%2F2br9DWK32hV4H3L64MkzitlwetU2rALV56J%2FMKkqHhmBZW6KzdvsqB9z9B8C0C%2FEEJd5iucUxUqbNHBgXyMJ5AMQMX1MZMfkoY4SdC9iwW500c6OFaduMIe0lS21jQ5RB9TVWsZqljcoJsf%2Bah%2F2c5MeraNUwplEd1WHzGQAz8tqGAxd%2Fko9vGUBcFlPbd0rHScIf41lmZRPxJlB9JlTV2XTXtPutT7dh1lrCNy4FU26aVpcK08xxok8lhsKOE5x1Xl%2FWM%2Fqdh0wVlWrML%2FF%2FdQGOqUBqkV2oCED98BgDxnwgMbvKfTIG7jldDwGLZZNfm5riTKowgjPDcp7JTQFP45sBCovlUeR0%2BzwrI01Y5ZwAw5gdloRryalxRzJQiKWRuy4blKj80%2F0B4WDeNf6K%2F3tYvrLe3kkhC%2BSOTTv7ve1uusLnelwtHMdOSi1dTkcorWnwhay0UZAM9yqdFEVFPwrdQxUFYtmkxfk3za%2FQKuMj3BBYjf6x5wk&X-Amz-Signature=329ce87e1128f32fc5c599b1db801a5d5f4b413d4ac3d0d5c1a99807be61dab6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDVTK6RO%2F20260908%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260908T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID8il7ys2GBfb2OSxoXAsIXNzyMFSNlTDmji5KH%2FkQfNAiEAhqVRaUzeyyC4Jr88kBW7wI8YTkjGrb201GUrx7GA9koq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDE%2Ba9nYDv3pxym2aPSrcA%2BpCG4137b4yUFl3zaDWWuVBBP80weZwUg%2F7n0lwiswe616ghbfKPc8LeH8odKkGLJJYqqBserkAcjrRNHFz01ciVooOUMjFCR8GYXnQ%2FOSbp7I7RNmODu3eGsRgnS37EBy7UTnGiDJvSaaFFr1t%2BM8oiKTTE2a96a4HvbNBL8toGoE%2FiKVmh39S9YMb7Lc2fnX%2BxUAHOUkmDg9kU%2BRkXvy%2F1MqbzGv9E5TBKOMjbB1oMgtUIa9hKeDlihkZMLViIU88q06XdKbM1aGFt5Ve56ono5xx5vcGoj0mAEbYjBpUlKB4Fg67s5v0knxeUmZjDCcn0KkpQ8Kf4uLcYx4Y3U1cq8DPU7RitO%2BPi02lRRU%2F2br9DWK32hV4H3L64MkzitlwetU2rALV56J%2FMKkqHhmBZW6KzdvsqB9z9B8C0C%2FEEJd5iucUxUqbNHBgXyMJ5AMQMX1MZMfkoY4SdC9iwW500c6OFaduMIe0lS21jQ5RB9TVWsZqljcoJsf%2Bah%2F2c5MeraNUwplEd1WHzGQAz8tqGAxd%2Fko9vGUBcFlPbd0rHScIf41lmZRPxJlB9JlTV2XTXtPutT7dh1lrCNy4FU26aVpcK08xxok8lhsKOE5x1Xl%2FWM%2Fqdh0wVlWrML%2FF%2FdQGOqUBqkV2oCED98BgDxnwgMbvKfTIG7jldDwGLZZNfm5riTKowgjPDcp7JTQFP45sBCovlUeR0%2BzwrI01Y5ZwAw5gdloRryalxRzJQiKWRuy4blKj80%2F0B4WDeNf6K%2F3tYvrLe3kkhC%2BSOTTv7ve1uusLnelwtHMdOSi1dTkcorWnwhay0UZAM9yqdFEVFPwrdQxUFYtmkxfk3za%2FQKuMj3BBYjf6x5wk&X-Amz-Signature=f893b1924194975604da3ba091a2ffa5819440c2a7884f739cbdc3ea7debbf5e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDVTK6RO%2F20260908%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260908T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID8il7ys2GBfb2OSxoXAsIXNzyMFSNlTDmji5KH%2FkQfNAiEAhqVRaUzeyyC4Jr88kBW7wI8YTkjGrb201GUrx7GA9koq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDE%2Ba9nYDv3pxym2aPSrcA%2BpCG4137b4yUFl3zaDWWuVBBP80weZwUg%2F7n0lwiswe616ghbfKPc8LeH8odKkGLJJYqqBserkAcjrRNHFz01ciVooOUMjFCR8GYXnQ%2FOSbp7I7RNmODu3eGsRgnS37EBy7UTnGiDJvSaaFFr1t%2BM8oiKTTE2a96a4HvbNBL8toGoE%2FiKVmh39S9YMb7Lc2fnX%2BxUAHOUkmDg9kU%2BRkXvy%2F1MqbzGv9E5TBKOMjbB1oMgtUIa9hKeDlihkZMLViIU88q06XdKbM1aGFt5Ve56ono5xx5vcGoj0mAEbYjBpUlKB4Fg67s5v0knxeUmZjDCcn0KkpQ8Kf4uLcYx4Y3U1cq8DPU7RitO%2BPi02lRRU%2F2br9DWK32hV4H3L64MkzitlwetU2rALV56J%2FMKkqHhmBZW6KzdvsqB9z9B8C0C%2FEEJd5iucUxUqbNHBgXyMJ5AMQMX1MZMfkoY4SdC9iwW500c6OFaduMIe0lS21jQ5RB9TVWsZqljcoJsf%2Bah%2F2c5MeraNUwplEd1WHzGQAz8tqGAxd%2Fko9vGUBcFlPbd0rHScIf41lmZRPxJlB9JlTV2XTXtPutT7dh1lrCNy4FU26aVpcK08xxok8lhsKOE5x1Xl%2FWM%2Fqdh0wVlWrML%2FF%2FdQGOqUBqkV2oCED98BgDxnwgMbvKfTIG7jldDwGLZZNfm5riTKowgjPDcp7JTQFP45sBCovlUeR0%2BzwrI01Y5ZwAw5gdloRryalxRzJQiKWRuy4blKj80%2F0B4WDeNf6K%2F3tYvrLe3kkhC%2BSOTTv7ve1uusLnelwtHMdOSi1dTkcorWnwhay0UZAM9yqdFEVFPwrdQxUFYtmkxfk3za%2FQKuMj3BBYjf6x5wk&X-Amz-Signature=dd8970a086d5ca1b0cd14113c0126af9237ce2ed53cc12952f8664f9a97a1292&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
