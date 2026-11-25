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
fetched_at: '2026-11-25T06:02:22.988Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CZL5E47%2F20261125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261125T060217Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCS8QH%2BEnP2vL941vpbRdbO0iU6cAYFqk2Eb3TH79FxagIgaHiCrZcPeZ7JfyoHC3p4LwrZxwmz5y2j87a6t6vskUMqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC2dxrSHh6gDIkw1OCrcA6dfds%2FPK52X5iGY7sIrQeDq9dkFqbEZgJo8UkKaSGyhW2cbHQASgsN76tmtyO6oJZ8ZMcFr14kUqSXe5ijO30sP9pyt5OhMMJo7QAr55gvp9%2Bj8YYm5kYQ2B3r4vGRjk5QvHBIsXEx8RxedWs0FSNdbl%2FTvbpPEnAhLgFvOtgJ7WKR8teMphJxY4XezJMthCkcY54fKtHBSHsK5Z1HIf39e5TE05s70NzvWZd%2F24SyeczDIpZSOmwWIhRnXBi8%2FwxbtIew08tjFrwr43XJaBaf6t9GMAbDE1bYWziaQu8XHm6RNPrPTHGCCuxz4Sh6KqapnUCINykfBX1TJx0EMkEJBRl3RzNOUqG6odVu2By93UdAHzlDHcdJ2y76H4NAqTGz1zdR4WkzDjCUBGP49kLbgw2iW1pVGWqx6yKlOjT7Y341bPNdWnqKQhZY%2FCjYHqo9aOJ2p9hH6AFax9lF4vCnJwCc85vMsd1C%2BPPfZT1SGs1CNQvuymjpRCGYLzbkjbmsJcJZdaonvPHXAjCUMFUSaJ%2BSfogtfKJB3Sz7xt5F6VHpR09SrvZwPqsW5ys2JL1w10SNOm1iVwfKLklPEKKe92%2BPv59WgrAYWskT1Wplof5MiFDIqC%2FCuUpvXMOmlmdgGOqUBB47gkiGJjkM%2BCThYwMX1yoBQBmOUTqsdBXqLWtydrxnSyoS7Mrko1YEtnLgM2HdZymRn%2BhcmBehwFzIjILL5ZN9n4Qom%2FySiHUpRrO3YBUT0mKz4BKlNOAJUgAXSzwHrlLoYK%2FwIA8EZRUVNz32RTmGUVhQAZ1DRec4Wd%2FHESGzXfUJqHwFzOiS%2FdYenjU1bBQo0z5JSiSUErRb8Z0c7kZbhDyIe&X-Amz-Signature=c70bf07656352a5d9cf99f1970e8d5b9c13c236a8fce3ba188f4fdc8cc0e746f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CZL5E47%2F20261125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261125T060217Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCS8QH%2BEnP2vL941vpbRdbO0iU6cAYFqk2Eb3TH79FxagIgaHiCrZcPeZ7JfyoHC3p4LwrZxwmz5y2j87a6t6vskUMqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC2dxrSHh6gDIkw1OCrcA6dfds%2FPK52X5iGY7sIrQeDq9dkFqbEZgJo8UkKaSGyhW2cbHQASgsN76tmtyO6oJZ8ZMcFr14kUqSXe5ijO30sP9pyt5OhMMJo7QAr55gvp9%2Bj8YYm5kYQ2B3r4vGRjk5QvHBIsXEx8RxedWs0FSNdbl%2FTvbpPEnAhLgFvOtgJ7WKR8teMphJxY4XezJMthCkcY54fKtHBSHsK5Z1HIf39e5TE05s70NzvWZd%2F24SyeczDIpZSOmwWIhRnXBi8%2FwxbtIew08tjFrwr43XJaBaf6t9GMAbDE1bYWziaQu8XHm6RNPrPTHGCCuxz4Sh6KqapnUCINykfBX1TJx0EMkEJBRl3RzNOUqG6odVu2By93UdAHzlDHcdJ2y76H4NAqTGz1zdR4WkzDjCUBGP49kLbgw2iW1pVGWqx6yKlOjT7Y341bPNdWnqKQhZY%2FCjYHqo9aOJ2p9hH6AFax9lF4vCnJwCc85vMsd1C%2BPPfZT1SGs1CNQvuymjpRCGYLzbkjbmsJcJZdaonvPHXAjCUMFUSaJ%2BSfogtfKJB3Sz7xt5F6VHpR09SrvZwPqsW5ys2JL1w10SNOm1iVwfKLklPEKKe92%2BPv59WgrAYWskT1Wplof5MiFDIqC%2FCuUpvXMOmlmdgGOqUBB47gkiGJjkM%2BCThYwMX1yoBQBmOUTqsdBXqLWtydrxnSyoS7Mrko1YEtnLgM2HdZymRn%2BhcmBehwFzIjILL5ZN9n4Qom%2FySiHUpRrO3YBUT0mKz4BKlNOAJUgAXSzwHrlLoYK%2FwIA8EZRUVNz32RTmGUVhQAZ1DRec4Wd%2FHESGzXfUJqHwFzOiS%2FdYenjU1bBQo0z5JSiSUErRb8Z0c7kZbhDyIe&X-Amz-Signature=6a0efb15ace73ebe9b8dc6d71c1fd1a776c4956631761d1d4cf30bc542e2fc01&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CZL5E47%2F20261125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261125T060217Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCS8QH%2BEnP2vL941vpbRdbO0iU6cAYFqk2Eb3TH79FxagIgaHiCrZcPeZ7JfyoHC3p4LwrZxwmz5y2j87a6t6vskUMqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC2dxrSHh6gDIkw1OCrcA6dfds%2FPK52X5iGY7sIrQeDq9dkFqbEZgJo8UkKaSGyhW2cbHQASgsN76tmtyO6oJZ8ZMcFr14kUqSXe5ijO30sP9pyt5OhMMJo7QAr55gvp9%2Bj8YYm5kYQ2B3r4vGRjk5QvHBIsXEx8RxedWs0FSNdbl%2FTvbpPEnAhLgFvOtgJ7WKR8teMphJxY4XezJMthCkcY54fKtHBSHsK5Z1HIf39e5TE05s70NzvWZd%2F24SyeczDIpZSOmwWIhRnXBi8%2FwxbtIew08tjFrwr43XJaBaf6t9GMAbDE1bYWziaQu8XHm6RNPrPTHGCCuxz4Sh6KqapnUCINykfBX1TJx0EMkEJBRl3RzNOUqG6odVu2By93UdAHzlDHcdJ2y76H4NAqTGz1zdR4WkzDjCUBGP49kLbgw2iW1pVGWqx6yKlOjT7Y341bPNdWnqKQhZY%2FCjYHqo9aOJ2p9hH6AFax9lF4vCnJwCc85vMsd1C%2BPPfZT1SGs1CNQvuymjpRCGYLzbkjbmsJcJZdaonvPHXAjCUMFUSaJ%2BSfogtfKJB3Sz7xt5F6VHpR09SrvZwPqsW5ys2JL1w10SNOm1iVwfKLklPEKKe92%2BPv59WgrAYWskT1Wplof5MiFDIqC%2FCuUpvXMOmlmdgGOqUBB47gkiGJjkM%2BCThYwMX1yoBQBmOUTqsdBXqLWtydrxnSyoS7Mrko1YEtnLgM2HdZymRn%2BhcmBehwFzIjILL5ZN9n4Qom%2FySiHUpRrO3YBUT0mKz4BKlNOAJUgAXSzwHrlLoYK%2FwIA8EZRUVNz32RTmGUVhQAZ1DRec4Wd%2FHESGzXfUJqHwFzOiS%2FdYenjU1bBQo0z5JSiSUErRb8Z0c7kZbhDyIe&X-Amz-Signature=7ba3f3a0150d0ee18caf015c3860cb5d58d9acff27ef96a826f70bf0ea53aaad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CZL5E47%2F20261125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261125T060217Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCS8QH%2BEnP2vL941vpbRdbO0iU6cAYFqk2Eb3TH79FxagIgaHiCrZcPeZ7JfyoHC3p4LwrZxwmz5y2j87a6t6vskUMqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC2dxrSHh6gDIkw1OCrcA6dfds%2FPK52X5iGY7sIrQeDq9dkFqbEZgJo8UkKaSGyhW2cbHQASgsN76tmtyO6oJZ8ZMcFr14kUqSXe5ijO30sP9pyt5OhMMJo7QAr55gvp9%2Bj8YYm5kYQ2B3r4vGRjk5QvHBIsXEx8RxedWs0FSNdbl%2FTvbpPEnAhLgFvOtgJ7WKR8teMphJxY4XezJMthCkcY54fKtHBSHsK5Z1HIf39e5TE05s70NzvWZd%2F24SyeczDIpZSOmwWIhRnXBi8%2FwxbtIew08tjFrwr43XJaBaf6t9GMAbDE1bYWziaQu8XHm6RNPrPTHGCCuxz4Sh6KqapnUCINykfBX1TJx0EMkEJBRl3RzNOUqG6odVu2By93UdAHzlDHcdJ2y76H4NAqTGz1zdR4WkzDjCUBGP49kLbgw2iW1pVGWqx6yKlOjT7Y341bPNdWnqKQhZY%2FCjYHqo9aOJ2p9hH6AFax9lF4vCnJwCc85vMsd1C%2BPPfZT1SGs1CNQvuymjpRCGYLzbkjbmsJcJZdaonvPHXAjCUMFUSaJ%2BSfogtfKJB3Sz7xt5F6VHpR09SrvZwPqsW5ys2JL1w10SNOm1iVwfKLklPEKKe92%2BPv59WgrAYWskT1Wplof5MiFDIqC%2FCuUpvXMOmlmdgGOqUBB47gkiGJjkM%2BCThYwMX1yoBQBmOUTqsdBXqLWtydrxnSyoS7Mrko1YEtnLgM2HdZymRn%2BhcmBehwFzIjILL5ZN9n4Qom%2FySiHUpRrO3YBUT0mKz4BKlNOAJUgAXSzwHrlLoYK%2FwIA8EZRUVNz32RTmGUVhQAZ1DRec4Wd%2FHESGzXfUJqHwFzOiS%2FdYenjU1bBQo0z5JSiSUErRb8Z0c7kZbhDyIe&X-Amz-Signature=a6aa6e0e6a7f35dd55eb5a2a549201cb362a34242aa9736222e91dc854436e53&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CZL5E47%2F20261125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261125T060217Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCS8QH%2BEnP2vL941vpbRdbO0iU6cAYFqk2Eb3TH79FxagIgaHiCrZcPeZ7JfyoHC3p4LwrZxwmz5y2j87a6t6vskUMqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC2dxrSHh6gDIkw1OCrcA6dfds%2FPK52X5iGY7sIrQeDq9dkFqbEZgJo8UkKaSGyhW2cbHQASgsN76tmtyO6oJZ8ZMcFr14kUqSXe5ijO30sP9pyt5OhMMJo7QAr55gvp9%2Bj8YYm5kYQ2B3r4vGRjk5QvHBIsXEx8RxedWs0FSNdbl%2FTvbpPEnAhLgFvOtgJ7WKR8teMphJxY4XezJMthCkcY54fKtHBSHsK5Z1HIf39e5TE05s70NzvWZd%2F24SyeczDIpZSOmwWIhRnXBi8%2FwxbtIew08tjFrwr43XJaBaf6t9GMAbDE1bYWziaQu8XHm6RNPrPTHGCCuxz4Sh6KqapnUCINykfBX1TJx0EMkEJBRl3RzNOUqG6odVu2By93UdAHzlDHcdJ2y76H4NAqTGz1zdR4WkzDjCUBGP49kLbgw2iW1pVGWqx6yKlOjT7Y341bPNdWnqKQhZY%2FCjYHqo9aOJ2p9hH6AFax9lF4vCnJwCc85vMsd1C%2BPPfZT1SGs1CNQvuymjpRCGYLzbkjbmsJcJZdaonvPHXAjCUMFUSaJ%2BSfogtfKJB3Sz7xt5F6VHpR09SrvZwPqsW5ys2JL1w10SNOm1iVwfKLklPEKKe92%2BPv59WgrAYWskT1Wplof5MiFDIqC%2FCuUpvXMOmlmdgGOqUBB47gkiGJjkM%2BCThYwMX1yoBQBmOUTqsdBXqLWtydrxnSyoS7Mrko1YEtnLgM2HdZymRn%2BhcmBehwFzIjILL5ZN9n4Qom%2FySiHUpRrO3YBUT0mKz4BKlNOAJUgAXSzwHrlLoYK%2FwIA8EZRUVNz32RTmGUVhQAZ1DRec4Wd%2FHESGzXfUJqHwFzOiS%2FdYenjU1bBQo0z5JSiSUErRb8Z0c7kZbhDyIe&X-Amz-Signature=54d91edfe41aef669e89a155443a32ea46cf933d9a8d9c1427c12cb5c434fa21&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CZL5E47%2F20261125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261125T060217Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCS8QH%2BEnP2vL941vpbRdbO0iU6cAYFqk2Eb3TH79FxagIgaHiCrZcPeZ7JfyoHC3p4LwrZxwmz5y2j87a6t6vskUMqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC2dxrSHh6gDIkw1OCrcA6dfds%2FPK52X5iGY7sIrQeDq9dkFqbEZgJo8UkKaSGyhW2cbHQASgsN76tmtyO6oJZ8ZMcFr14kUqSXe5ijO30sP9pyt5OhMMJo7QAr55gvp9%2Bj8YYm5kYQ2B3r4vGRjk5QvHBIsXEx8RxedWs0FSNdbl%2FTvbpPEnAhLgFvOtgJ7WKR8teMphJxY4XezJMthCkcY54fKtHBSHsK5Z1HIf39e5TE05s70NzvWZd%2F24SyeczDIpZSOmwWIhRnXBi8%2FwxbtIew08tjFrwr43XJaBaf6t9GMAbDE1bYWziaQu8XHm6RNPrPTHGCCuxz4Sh6KqapnUCINykfBX1TJx0EMkEJBRl3RzNOUqG6odVu2By93UdAHzlDHcdJ2y76H4NAqTGz1zdR4WkzDjCUBGP49kLbgw2iW1pVGWqx6yKlOjT7Y341bPNdWnqKQhZY%2FCjYHqo9aOJ2p9hH6AFax9lF4vCnJwCc85vMsd1C%2BPPfZT1SGs1CNQvuymjpRCGYLzbkjbmsJcJZdaonvPHXAjCUMFUSaJ%2BSfogtfKJB3Sz7xt5F6VHpR09SrvZwPqsW5ys2JL1w10SNOm1iVwfKLklPEKKe92%2BPv59WgrAYWskT1Wplof5MiFDIqC%2FCuUpvXMOmlmdgGOqUBB47gkiGJjkM%2BCThYwMX1yoBQBmOUTqsdBXqLWtydrxnSyoS7Mrko1YEtnLgM2HdZymRn%2BhcmBehwFzIjILL5ZN9n4Qom%2FySiHUpRrO3YBUT0mKz4BKlNOAJUgAXSzwHrlLoYK%2FwIA8EZRUVNz32RTmGUVhQAZ1DRec4Wd%2FHESGzXfUJqHwFzOiS%2FdYenjU1bBQo0z5JSiSUErRb8Z0c7kZbhDyIe&X-Amz-Signature=06638abba8c02df8c7a752f7dd86fbf1c160957ea066235ef02189fa14541f4a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CZL5E47%2F20261125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261125T060217Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCS8QH%2BEnP2vL941vpbRdbO0iU6cAYFqk2Eb3TH79FxagIgaHiCrZcPeZ7JfyoHC3p4LwrZxwmz5y2j87a6t6vskUMqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC2dxrSHh6gDIkw1OCrcA6dfds%2FPK52X5iGY7sIrQeDq9dkFqbEZgJo8UkKaSGyhW2cbHQASgsN76tmtyO6oJZ8ZMcFr14kUqSXe5ijO30sP9pyt5OhMMJo7QAr55gvp9%2Bj8YYm5kYQ2B3r4vGRjk5QvHBIsXEx8RxedWs0FSNdbl%2FTvbpPEnAhLgFvOtgJ7WKR8teMphJxY4XezJMthCkcY54fKtHBSHsK5Z1HIf39e5TE05s70NzvWZd%2F24SyeczDIpZSOmwWIhRnXBi8%2FwxbtIew08tjFrwr43XJaBaf6t9GMAbDE1bYWziaQu8XHm6RNPrPTHGCCuxz4Sh6KqapnUCINykfBX1TJx0EMkEJBRl3RzNOUqG6odVu2By93UdAHzlDHcdJ2y76H4NAqTGz1zdR4WkzDjCUBGP49kLbgw2iW1pVGWqx6yKlOjT7Y341bPNdWnqKQhZY%2FCjYHqo9aOJ2p9hH6AFax9lF4vCnJwCc85vMsd1C%2BPPfZT1SGs1CNQvuymjpRCGYLzbkjbmsJcJZdaonvPHXAjCUMFUSaJ%2BSfogtfKJB3Sz7xt5F6VHpR09SrvZwPqsW5ys2JL1w10SNOm1iVwfKLklPEKKe92%2BPv59WgrAYWskT1Wplof5MiFDIqC%2FCuUpvXMOmlmdgGOqUBB47gkiGJjkM%2BCThYwMX1yoBQBmOUTqsdBXqLWtydrxnSyoS7Mrko1YEtnLgM2HdZymRn%2BhcmBehwFzIjILL5ZN9n4Qom%2FySiHUpRrO3YBUT0mKz4BKlNOAJUgAXSzwHrlLoYK%2FwIA8EZRUVNz32RTmGUVhQAZ1DRec4Wd%2FHESGzXfUJqHwFzOiS%2FdYenjU1bBQo0z5JSiSUErRb8Z0c7kZbhDyIe&X-Amz-Signature=52e53f1124376eceaa55c0f90fb3bf08e546d4883ab320b76325e537c46a2615&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CZL5E47%2F20261125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261125T060217Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCS8QH%2BEnP2vL941vpbRdbO0iU6cAYFqk2Eb3TH79FxagIgaHiCrZcPeZ7JfyoHC3p4LwrZxwmz5y2j87a6t6vskUMqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC2dxrSHh6gDIkw1OCrcA6dfds%2FPK52X5iGY7sIrQeDq9dkFqbEZgJo8UkKaSGyhW2cbHQASgsN76tmtyO6oJZ8ZMcFr14kUqSXe5ijO30sP9pyt5OhMMJo7QAr55gvp9%2Bj8YYm5kYQ2B3r4vGRjk5QvHBIsXEx8RxedWs0FSNdbl%2FTvbpPEnAhLgFvOtgJ7WKR8teMphJxY4XezJMthCkcY54fKtHBSHsK5Z1HIf39e5TE05s70NzvWZd%2F24SyeczDIpZSOmwWIhRnXBi8%2FwxbtIew08tjFrwr43XJaBaf6t9GMAbDE1bYWziaQu8XHm6RNPrPTHGCCuxz4Sh6KqapnUCINykfBX1TJx0EMkEJBRl3RzNOUqG6odVu2By93UdAHzlDHcdJ2y76H4NAqTGz1zdR4WkzDjCUBGP49kLbgw2iW1pVGWqx6yKlOjT7Y341bPNdWnqKQhZY%2FCjYHqo9aOJ2p9hH6AFax9lF4vCnJwCc85vMsd1C%2BPPfZT1SGs1CNQvuymjpRCGYLzbkjbmsJcJZdaonvPHXAjCUMFUSaJ%2BSfogtfKJB3Sz7xt5F6VHpR09SrvZwPqsW5ys2JL1w10SNOm1iVwfKLklPEKKe92%2BPv59WgrAYWskT1Wplof5MiFDIqC%2FCuUpvXMOmlmdgGOqUBB47gkiGJjkM%2BCThYwMX1yoBQBmOUTqsdBXqLWtydrxnSyoS7Mrko1YEtnLgM2HdZymRn%2BhcmBehwFzIjILL5ZN9n4Qom%2FySiHUpRrO3YBUT0mKz4BKlNOAJUgAXSzwHrlLoYK%2FwIA8EZRUVNz32RTmGUVhQAZ1DRec4Wd%2FHESGzXfUJqHwFzOiS%2FdYenjU1bBQo0z5JSiSUErRb8Z0c7kZbhDyIe&X-Amz-Signature=c8dc87139d1d81c7302a2b2b6eb162919c6a04cfbdfe590b33a4a2fcb453a9ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CZL5E47%2F20261125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261125T060218Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCS8QH%2BEnP2vL941vpbRdbO0iU6cAYFqk2Eb3TH79FxagIgaHiCrZcPeZ7JfyoHC3p4LwrZxwmz5y2j87a6t6vskUMqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC2dxrSHh6gDIkw1OCrcA6dfds%2FPK52X5iGY7sIrQeDq9dkFqbEZgJo8UkKaSGyhW2cbHQASgsN76tmtyO6oJZ8ZMcFr14kUqSXe5ijO30sP9pyt5OhMMJo7QAr55gvp9%2Bj8YYm5kYQ2B3r4vGRjk5QvHBIsXEx8RxedWs0FSNdbl%2FTvbpPEnAhLgFvOtgJ7WKR8teMphJxY4XezJMthCkcY54fKtHBSHsK5Z1HIf39e5TE05s70NzvWZd%2F24SyeczDIpZSOmwWIhRnXBi8%2FwxbtIew08tjFrwr43XJaBaf6t9GMAbDE1bYWziaQu8XHm6RNPrPTHGCCuxz4Sh6KqapnUCINykfBX1TJx0EMkEJBRl3RzNOUqG6odVu2By93UdAHzlDHcdJ2y76H4NAqTGz1zdR4WkzDjCUBGP49kLbgw2iW1pVGWqx6yKlOjT7Y341bPNdWnqKQhZY%2FCjYHqo9aOJ2p9hH6AFax9lF4vCnJwCc85vMsd1C%2BPPfZT1SGs1CNQvuymjpRCGYLzbkjbmsJcJZdaonvPHXAjCUMFUSaJ%2BSfogtfKJB3Sz7xt5F6VHpR09SrvZwPqsW5ys2JL1w10SNOm1iVwfKLklPEKKe92%2BPv59WgrAYWskT1Wplof5MiFDIqC%2FCuUpvXMOmlmdgGOqUBB47gkiGJjkM%2BCThYwMX1yoBQBmOUTqsdBXqLWtydrxnSyoS7Mrko1YEtnLgM2HdZymRn%2BhcmBehwFzIjILL5ZN9n4Qom%2FySiHUpRrO3YBUT0mKz4BKlNOAJUgAXSzwHrlLoYK%2FwIA8EZRUVNz32RTmGUVhQAZ1DRec4Wd%2FHESGzXfUJqHwFzOiS%2FdYenjU1bBQo0z5JSiSUErRb8Z0c7kZbhDyIe&X-Amz-Signature=37588ba4b956e269b9261d7155930a9040be0ab3350316417ab6538b8fe9fc0f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
