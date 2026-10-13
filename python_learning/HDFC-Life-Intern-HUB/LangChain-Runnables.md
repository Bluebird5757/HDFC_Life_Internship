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
fetched_at: '2026-10-13T04:30:11.226Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XAVCRCCE%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043005Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDUC9kvquGXqMgfI51uy5yYEySgxDRfED64XjFc0FDhBQIhAK8ELbDHwx2SSp1l8uo5c0Xq26cdJcD1dXuth1uRTP6BKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igysq4dBFlibS1QP0Lsq3ANUDhZt7%2BA4aEi7wFwiq8PP6ezrkJP8MOg%2Fwp4fTQPMiAJx7Og6P29I9y367teR%2Fjg4Gev%2BU4Y3boAvCSb03Hk6UQoHetBtZAhu%2FhfEbU6hv77UEmLVSiPOfV42MOq%2F%2BjoDoQA2N%2F%2FJp1xD9M6ac9jjJmJiHJRgH7km4Kcy726FbM7JwmfyL716Zv4GcWQAR%2F7zdqzNNwf4FTepirjMT7UL89ApF5nwkmHEoFJIUv9Ayn4LRB1qLvQQods9%2BRBzy%2F63xyMjekIbMRXFadE5QT%2Fo8bsaPcTERTv1X9Ph3seOh1kVItzXrEQuTC2hnuKCApUTd6ZxWPwMw4b2cNIRprBt9Rj2uRrZgyd5h8DBh01QbH28hXaJyFDavyTobgvpMQXX%2BBh3t8UmCQrGV%2BjT896EsRafDxs9aqrqgaUwZ9Zkvcm7kyWJ5lIX3D1fTUXY9v0u0ZkvNd4Khqg%2FcagOiNZn0BcyTOm65udEkpvL6oOhe8%2BIz%2B9Tu4Zbtzh5Rjjjere1cZaMBQDMduAy0FdBotbR15m1%2FvMT0d%2Fcjl72iIx0DlBLnQFPmVlNpG%2BTMzQ04%2FTExnBeEUVb7Sfmb%2FQSG5TK2z1HCc0OX9SPgZnRRa143CL6OX%2FvQ9RksPvZhTDH37bWBjqkAd3799O%2Fzs%2BkjKl32kmubfktR0bGBRexEYGvcWJ0yG3BqADi5fGilCqaPDSW%2BaAif3ZvOp6mqXo%2B2sdf%2BxOsbqA1c1QYMRtLuSDxvLd9Z77Jcz5jnA681YzfUUa8cyomBML9aDnjqJGdDZNBkFUYOSpvq4r16G1xlJ94MkO89YFI%2BvK7q2Jxm7g9C4zvDE7SACN5d4hFsuvLWC3CeI%2B8flCCbI3G&X-Amz-Signature=617aadc4104d5727a67de5dd2de102449b288cc6c641cd537f5921ab056dd3fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XAVCRCCE%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043005Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDUC9kvquGXqMgfI51uy5yYEySgxDRfED64XjFc0FDhBQIhAK8ELbDHwx2SSp1l8uo5c0Xq26cdJcD1dXuth1uRTP6BKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igysq4dBFlibS1QP0Lsq3ANUDhZt7%2BA4aEi7wFwiq8PP6ezrkJP8MOg%2Fwp4fTQPMiAJx7Og6P29I9y367teR%2Fjg4Gev%2BU4Y3boAvCSb03Hk6UQoHetBtZAhu%2FhfEbU6hv77UEmLVSiPOfV42MOq%2F%2BjoDoQA2N%2F%2FJp1xD9M6ac9jjJmJiHJRgH7km4Kcy726FbM7JwmfyL716Zv4GcWQAR%2F7zdqzNNwf4FTepirjMT7UL89ApF5nwkmHEoFJIUv9Ayn4LRB1qLvQQods9%2BRBzy%2F63xyMjekIbMRXFadE5QT%2Fo8bsaPcTERTv1X9Ph3seOh1kVItzXrEQuTC2hnuKCApUTd6ZxWPwMw4b2cNIRprBt9Rj2uRrZgyd5h8DBh01QbH28hXaJyFDavyTobgvpMQXX%2BBh3t8UmCQrGV%2BjT896EsRafDxs9aqrqgaUwZ9Zkvcm7kyWJ5lIX3D1fTUXY9v0u0ZkvNd4Khqg%2FcagOiNZn0BcyTOm65udEkpvL6oOhe8%2BIz%2B9Tu4Zbtzh5Rjjjere1cZaMBQDMduAy0FdBotbR15m1%2FvMT0d%2Fcjl72iIx0DlBLnQFPmVlNpG%2BTMzQ04%2FTExnBeEUVb7Sfmb%2FQSG5TK2z1HCc0OX9SPgZnRRa143CL6OX%2FvQ9RksPvZhTDH37bWBjqkAd3799O%2Fzs%2BkjKl32kmubfktR0bGBRexEYGvcWJ0yG3BqADi5fGilCqaPDSW%2BaAif3ZvOp6mqXo%2B2sdf%2BxOsbqA1c1QYMRtLuSDxvLd9Z77Jcz5jnA681YzfUUa8cyomBML9aDnjqJGdDZNBkFUYOSpvq4r16G1xlJ94MkO89YFI%2BvK7q2Jxm7g9C4zvDE7SACN5d4hFsuvLWC3CeI%2B8flCCbI3G&X-Amz-Signature=3b9bc217a7c01112b1c63e49b4fa359acf3f2e6536f45edfc7744fc20a6db156&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XAVCRCCE%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043005Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDUC9kvquGXqMgfI51uy5yYEySgxDRfED64XjFc0FDhBQIhAK8ELbDHwx2SSp1l8uo5c0Xq26cdJcD1dXuth1uRTP6BKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igysq4dBFlibS1QP0Lsq3ANUDhZt7%2BA4aEi7wFwiq8PP6ezrkJP8MOg%2Fwp4fTQPMiAJx7Og6P29I9y367teR%2Fjg4Gev%2BU4Y3boAvCSb03Hk6UQoHetBtZAhu%2FhfEbU6hv77UEmLVSiPOfV42MOq%2F%2BjoDoQA2N%2F%2FJp1xD9M6ac9jjJmJiHJRgH7km4Kcy726FbM7JwmfyL716Zv4GcWQAR%2F7zdqzNNwf4FTepirjMT7UL89ApF5nwkmHEoFJIUv9Ayn4LRB1qLvQQods9%2BRBzy%2F63xyMjekIbMRXFadE5QT%2Fo8bsaPcTERTv1X9Ph3seOh1kVItzXrEQuTC2hnuKCApUTd6ZxWPwMw4b2cNIRprBt9Rj2uRrZgyd5h8DBh01QbH28hXaJyFDavyTobgvpMQXX%2BBh3t8UmCQrGV%2BjT896EsRafDxs9aqrqgaUwZ9Zkvcm7kyWJ5lIX3D1fTUXY9v0u0ZkvNd4Khqg%2FcagOiNZn0BcyTOm65udEkpvL6oOhe8%2BIz%2B9Tu4Zbtzh5Rjjjere1cZaMBQDMduAy0FdBotbR15m1%2FvMT0d%2Fcjl72iIx0DlBLnQFPmVlNpG%2BTMzQ04%2FTExnBeEUVb7Sfmb%2FQSG5TK2z1HCc0OX9SPgZnRRa143CL6OX%2FvQ9RksPvZhTDH37bWBjqkAd3799O%2Fzs%2BkjKl32kmubfktR0bGBRexEYGvcWJ0yG3BqADi5fGilCqaPDSW%2BaAif3ZvOp6mqXo%2B2sdf%2BxOsbqA1c1QYMRtLuSDxvLd9Z77Jcz5jnA681YzfUUa8cyomBML9aDnjqJGdDZNBkFUYOSpvq4r16G1xlJ94MkO89YFI%2BvK7q2Jxm7g9C4zvDE7SACN5d4hFsuvLWC3CeI%2B8flCCbI3G&X-Amz-Signature=ae8fe95e34c713158dc34972de1afa62ae729282bfdf192d5cf96635cabcd9ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XAVCRCCE%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043005Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDUC9kvquGXqMgfI51uy5yYEySgxDRfED64XjFc0FDhBQIhAK8ELbDHwx2SSp1l8uo5c0Xq26cdJcD1dXuth1uRTP6BKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igysq4dBFlibS1QP0Lsq3ANUDhZt7%2BA4aEi7wFwiq8PP6ezrkJP8MOg%2Fwp4fTQPMiAJx7Og6P29I9y367teR%2Fjg4Gev%2BU4Y3boAvCSb03Hk6UQoHetBtZAhu%2FhfEbU6hv77UEmLVSiPOfV42MOq%2F%2BjoDoQA2N%2F%2FJp1xD9M6ac9jjJmJiHJRgH7km4Kcy726FbM7JwmfyL716Zv4GcWQAR%2F7zdqzNNwf4FTepirjMT7UL89ApF5nwkmHEoFJIUv9Ayn4LRB1qLvQQods9%2BRBzy%2F63xyMjekIbMRXFadE5QT%2Fo8bsaPcTERTv1X9Ph3seOh1kVItzXrEQuTC2hnuKCApUTd6ZxWPwMw4b2cNIRprBt9Rj2uRrZgyd5h8DBh01QbH28hXaJyFDavyTobgvpMQXX%2BBh3t8UmCQrGV%2BjT896EsRafDxs9aqrqgaUwZ9Zkvcm7kyWJ5lIX3D1fTUXY9v0u0ZkvNd4Khqg%2FcagOiNZn0BcyTOm65udEkpvL6oOhe8%2BIz%2B9Tu4Zbtzh5Rjjjere1cZaMBQDMduAy0FdBotbR15m1%2FvMT0d%2Fcjl72iIx0DlBLnQFPmVlNpG%2BTMzQ04%2FTExnBeEUVb7Sfmb%2FQSG5TK2z1HCc0OX9SPgZnRRa143CL6OX%2FvQ9RksPvZhTDH37bWBjqkAd3799O%2Fzs%2BkjKl32kmubfktR0bGBRexEYGvcWJ0yG3BqADi5fGilCqaPDSW%2BaAif3ZvOp6mqXo%2B2sdf%2BxOsbqA1c1QYMRtLuSDxvLd9Z77Jcz5jnA681YzfUUa8cyomBML9aDnjqJGdDZNBkFUYOSpvq4r16G1xlJ94MkO89YFI%2BvK7q2Jxm7g9C4zvDE7SACN5d4hFsuvLWC3CeI%2B8flCCbI3G&X-Amz-Signature=bd3d7b226d38738957a8e5acbcab56ee5d80e6faf829e93ffa161d6260bd6464&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XAVCRCCE%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043005Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDUC9kvquGXqMgfI51uy5yYEySgxDRfED64XjFc0FDhBQIhAK8ELbDHwx2SSp1l8uo5c0Xq26cdJcD1dXuth1uRTP6BKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igysq4dBFlibS1QP0Lsq3ANUDhZt7%2BA4aEi7wFwiq8PP6ezrkJP8MOg%2Fwp4fTQPMiAJx7Og6P29I9y367teR%2Fjg4Gev%2BU4Y3boAvCSb03Hk6UQoHetBtZAhu%2FhfEbU6hv77UEmLVSiPOfV42MOq%2F%2BjoDoQA2N%2F%2FJp1xD9M6ac9jjJmJiHJRgH7km4Kcy726FbM7JwmfyL716Zv4GcWQAR%2F7zdqzNNwf4FTepirjMT7UL89ApF5nwkmHEoFJIUv9Ayn4LRB1qLvQQods9%2BRBzy%2F63xyMjekIbMRXFadE5QT%2Fo8bsaPcTERTv1X9Ph3seOh1kVItzXrEQuTC2hnuKCApUTd6ZxWPwMw4b2cNIRprBt9Rj2uRrZgyd5h8DBh01QbH28hXaJyFDavyTobgvpMQXX%2BBh3t8UmCQrGV%2BjT896EsRafDxs9aqrqgaUwZ9Zkvcm7kyWJ5lIX3D1fTUXY9v0u0ZkvNd4Khqg%2FcagOiNZn0BcyTOm65udEkpvL6oOhe8%2BIz%2B9Tu4Zbtzh5Rjjjere1cZaMBQDMduAy0FdBotbR15m1%2FvMT0d%2Fcjl72iIx0DlBLnQFPmVlNpG%2BTMzQ04%2FTExnBeEUVb7Sfmb%2FQSG5TK2z1HCc0OX9SPgZnRRa143CL6OX%2FvQ9RksPvZhTDH37bWBjqkAd3799O%2Fzs%2BkjKl32kmubfktR0bGBRexEYGvcWJ0yG3BqADi5fGilCqaPDSW%2BaAif3ZvOp6mqXo%2B2sdf%2BxOsbqA1c1QYMRtLuSDxvLd9Z77Jcz5jnA681YzfUUa8cyomBML9aDnjqJGdDZNBkFUYOSpvq4r16G1xlJ94MkO89YFI%2BvK7q2Jxm7g9C4zvDE7SACN5d4hFsuvLWC3CeI%2B8flCCbI3G&X-Amz-Signature=27ef2e546bd40853e80b3ff8c8b5b5be6d30ad7433808bf0e57c3d5faeaed4ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XAVCRCCE%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043005Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDUC9kvquGXqMgfI51uy5yYEySgxDRfED64XjFc0FDhBQIhAK8ELbDHwx2SSp1l8uo5c0Xq26cdJcD1dXuth1uRTP6BKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igysq4dBFlibS1QP0Lsq3ANUDhZt7%2BA4aEi7wFwiq8PP6ezrkJP8MOg%2Fwp4fTQPMiAJx7Og6P29I9y367teR%2Fjg4Gev%2BU4Y3boAvCSb03Hk6UQoHetBtZAhu%2FhfEbU6hv77UEmLVSiPOfV42MOq%2F%2BjoDoQA2N%2F%2FJp1xD9M6ac9jjJmJiHJRgH7km4Kcy726FbM7JwmfyL716Zv4GcWQAR%2F7zdqzNNwf4FTepirjMT7UL89ApF5nwkmHEoFJIUv9Ayn4LRB1qLvQQods9%2BRBzy%2F63xyMjekIbMRXFadE5QT%2Fo8bsaPcTERTv1X9Ph3seOh1kVItzXrEQuTC2hnuKCApUTd6ZxWPwMw4b2cNIRprBt9Rj2uRrZgyd5h8DBh01QbH28hXaJyFDavyTobgvpMQXX%2BBh3t8UmCQrGV%2BjT896EsRafDxs9aqrqgaUwZ9Zkvcm7kyWJ5lIX3D1fTUXY9v0u0ZkvNd4Khqg%2FcagOiNZn0BcyTOm65udEkpvL6oOhe8%2BIz%2B9Tu4Zbtzh5Rjjjere1cZaMBQDMduAy0FdBotbR15m1%2FvMT0d%2Fcjl72iIx0DlBLnQFPmVlNpG%2BTMzQ04%2FTExnBeEUVb7Sfmb%2FQSG5TK2z1HCc0OX9SPgZnRRa143CL6OX%2FvQ9RksPvZhTDH37bWBjqkAd3799O%2Fzs%2BkjKl32kmubfktR0bGBRexEYGvcWJ0yG3BqADi5fGilCqaPDSW%2BaAif3ZvOp6mqXo%2B2sdf%2BxOsbqA1c1QYMRtLuSDxvLd9Z77Jcz5jnA681YzfUUa8cyomBML9aDnjqJGdDZNBkFUYOSpvq4r16G1xlJ94MkO89YFI%2BvK7q2Jxm7g9C4zvDE7SACN5d4hFsuvLWC3CeI%2B8flCCbI3G&X-Amz-Signature=facd9e98807f1aab8c9178d527f077e31fee5cc0aea3083c08cdc38ac3cefee8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XAVCRCCE%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043005Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDUC9kvquGXqMgfI51uy5yYEySgxDRfED64XjFc0FDhBQIhAK8ELbDHwx2SSp1l8uo5c0Xq26cdJcD1dXuth1uRTP6BKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igysq4dBFlibS1QP0Lsq3ANUDhZt7%2BA4aEi7wFwiq8PP6ezrkJP8MOg%2Fwp4fTQPMiAJx7Og6P29I9y367teR%2Fjg4Gev%2BU4Y3boAvCSb03Hk6UQoHetBtZAhu%2FhfEbU6hv77UEmLVSiPOfV42MOq%2F%2BjoDoQA2N%2F%2FJp1xD9M6ac9jjJmJiHJRgH7km4Kcy726FbM7JwmfyL716Zv4GcWQAR%2F7zdqzNNwf4FTepirjMT7UL89ApF5nwkmHEoFJIUv9Ayn4LRB1qLvQQods9%2BRBzy%2F63xyMjekIbMRXFadE5QT%2Fo8bsaPcTERTv1X9Ph3seOh1kVItzXrEQuTC2hnuKCApUTd6ZxWPwMw4b2cNIRprBt9Rj2uRrZgyd5h8DBh01QbH28hXaJyFDavyTobgvpMQXX%2BBh3t8UmCQrGV%2BjT896EsRafDxs9aqrqgaUwZ9Zkvcm7kyWJ5lIX3D1fTUXY9v0u0ZkvNd4Khqg%2FcagOiNZn0BcyTOm65udEkpvL6oOhe8%2BIz%2B9Tu4Zbtzh5Rjjjere1cZaMBQDMduAy0FdBotbR15m1%2FvMT0d%2Fcjl72iIx0DlBLnQFPmVlNpG%2BTMzQ04%2FTExnBeEUVb7Sfmb%2FQSG5TK2z1HCc0OX9SPgZnRRa143CL6OX%2FvQ9RksPvZhTDH37bWBjqkAd3799O%2Fzs%2BkjKl32kmubfktR0bGBRexEYGvcWJ0yG3BqADi5fGilCqaPDSW%2BaAif3ZvOp6mqXo%2B2sdf%2BxOsbqA1c1QYMRtLuSDxvLd9Z77Jcz5jnA681YzfUUa8cyomBML9aDnjqJGdDZNBkFUYOSpvq4r16G1xlJ94MkO89YFI%2BvK7q2Jxm7g9C4zvDE7SACN5d4hFsuvLWC3CeI%2B8flCCbI3G&X-Amz-Signature=df2c5df21b62fab8819af545ccc112b44e44ce228208b6401a49ca8df138b5eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XAVCRCCE%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043005Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDUC9kvquGXqMgfI51uy5yYEySgxDRfED64XjFc0FDhBQIhAK8ELbDHwx2SSp1l8uo5c0Xq26cdJcD1dXuth1uRTP6BKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igysq4dBFlibS1QP0Lsq3ANUDhZt7%2BA4aEi7wFwiq8PP6ezrkJP8MOg%2Fwp4fTQPMiAJx7Og6P29I9y367teR%2Fjg4Gev%2BU4Y3boAvCSb03Hk6UQoHetBtZAhu%2FhfEbU6hv77UEmLVSiPOfV42MOq%2F%2BjoDoQA2N%2F%2FJp1xD9M6ac9jjJmJiHJRgH7km4Kcy726FbM7JwmfyL716Zv4GcWQAR%2F7zdqzNNwf4FTepirjMT7UL89ApF5nwkmHEoFJIUv9Ayn4LRB1qLvQQods9%2BRBzy%2F63xyMjekIbMRXFadE5QT%2Fo8bsaPcTERTv1X9Ph3seOh1kVItzXrEQuTC2hnuKCApUTd6ZxWPwMw4b2cNIRprBt9Rj2uRrZgyd5h8DBh01QbH28hXaJyFDavyTobgvpMQXX%2BBh3t8UmCQrGV%2BjT896EsRafDxs9aqrqgaUwZ9Zkvcm7kyWJ5lIX3D1fTUXY9v0u0ZkvNd4Khqg%2FcagOiNZn0BcyTOm65udEkpvL6oOhe8%2BIz%2B9Tu4Zbtzh5Rjjjere1cZaMBQDMduAy0FdBotbR15m1%2FvMT0d%2Fcjl72iIx0DlBLnQFPmVlNpG%2BTMzQ04%2FTExnBeEUVb7Sfmb%2FQSG5TK2z1HCc0OX9SPgZnRRa143CL6OX%2FvQ9RksPvZhTDH37bWBjqkAd3799O%2Fzs%2BkjKl32kmubfktR0bGBRexEYGvcWJ0yG3BqADi5fGilCqaPDSW%2BaAif3ZvOp6mqXo%2B2sdf%2BxOsbqA1c1QYMRtLuSDxvLd9Z77Jcz5jnA681YzfUUa8cyomBML9aDnjqJGdDZNBkFUYOSpvq4r16G1xlJ94MkO89YFI%2BvK7q2Jxm7g9C4zvDE7SACN5d4hFsuvLWC3CeI%2B8flCCbI3G&X-Amz-Signature=4fa2afbcad6ab979db9f194030a9cf10b1b3865faa1abf9e02d2cfe1f82de6ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XAVCRCCE%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043005Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDUC9kvquGXqMgfI51uy5yYEySgxDRfED64XjFc0FDhBQIhAK8ELbDHwx2SSp1l8uo5c0Xq26cdJcD1dXuth1uRTP6BKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igysq4dBFlibS1QP0Lsq3ANUDhZt7%2BA4aEi7wFwiq8PP6ezrkJP8MOg%2Fwp4fTQPMiAJx7Og6P29I9y367teR%2Fjg4Gev%2BU4Y3boAvCSb03Hk6UQoHetBtZAhu%2FhfEbU6hv77UEmLVSiPOfV42MOq%2F%2BjoDoQA2N%2F%2FJp1xD9M6ac9jjJmJiHJRgH7km4Kcy726FbM7JwmfyL716Zv4GcWQAR%2F7zdqzNNwf4FTepirjMT7UL89ApF5nwkmHEoFJIUv9Ayn4LRB1qLvQQods9%2BRBzy%2F63xyMjekIbMRXFadE5QT%2Fo8bsaPcTERTv1X9Ph3seOh1kVItzXrEQuTC2hnuKCApUTd6ZxWPwMw4b2cNIRprBt9Rj2uRrZgyd5h8DBh01QbH28hXaJyFDavyTobgvpMQXX%2BBh3t8UmCQrGV%2BjT896EsRafDxs9aqrqgaUwZ9Zkvcm7kyWJ5lIX3D1fTUXY9v0u0ZkvNd4Khqg%2FcagOiNZn0BcyTOm65udEkpvL6oOhe8%2BIz%2B9Tu4Zbtzh5Rjjjere1cZaMBQDMduAy0FdBotbR15m1%2FvMT0d%2Fcjl72iIx0DlBLnQFPmVlNpG%2BTMzQ04%2FTExnBeEUVb7Sfmb%2FQSG5TK2z1HCc0OX9SPgZnRRa143CL6OX%2FvQ9RksPvZhTDH37bWBjqkAd3799O%2Fzs%2BkjKl32kmubfktR0bGBRexEYGvcWJ0yG3BqADi5fGilCqaPDSW%2BaAif3ZvOp6mqXo%2B2sdf%2BxOsbqA1c1QYMRtLuSDxvLd9Z77Jcz5jnA681YzfUUa8cyomBML9aDnjqJGdDZNBkFUYOSpvq4r16G1xlJ94MkO89YFI%2BvK7q2Jxm7g9C4zvDE7SACN5d4hFsuvLWC3CeI%2B8flCCbI3G&X-Amz-Signature=77a44629485a8e22a3aebc32f3562b609298236965c0037dcf1f5f2d1bfeb1e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
