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
fetched_at: '2026-09-30T02:57:37.321Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRGVVM6Y%2F20260930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260930T025733Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDa7KwjQBS5JPLWIIKqdFuToUasNXfqe%2FhXsDJIjyVRVAiEA%2F%2F16jxbV4d1TpS8W5V5LhiErE90vE24FcD05kcafCgAq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDCmZnIqsZKE1%2FFIb2SrcAyQlGCEQdQ197O353T7iocKWdfMsj0%2FIJTjCbDHm7fYzWkQ3ud8OMdSY4MBE7L%2Bw1Xacdj%2B7iSAZmOYdkp68lmruhSKFnzAC8%2BE0a4KXnIo9L47xBab4THVpjZynv4X49axLFpUvGmqJSqB9kTM267Jvpz%2FjwbAJ54XKSrXG8RtzDzGTqIXG%2BmTKs54hnrstwdb8RRlr0yiteA5Sbg7aXFCyXkU1wRXsr7cWtb%2FGK7RexQoWSS7CAbCRYuMLvTavB2TbEC%2BBwPS2CsznuKKOerwvYiwcHt0ui8rGnCOkdCf6J5bDZvrujjs0O1H16GcyY1htsSKzmJCiJv6fa%2BCStequ96ClFeZXnOTflqb%2B4EhHXrr93WgvWzjGFDi80iyXd2ikIkhR3gwviIbws5me5RQATw%2F81dm9BDxMoYj0mzrni62cOnAmejD8tbuZSVEfjXN6g%2FxrndfUy%2BtAgSRKRxR3jh%2Bbxmi0C3kH7UiL9BgGbajIWyieCTNiCKESJXctJeImS6oKtbT7llB3PIDJNc61d1dIsMuGWNFhbhlhFQAYUSY%2BhOc94qUokTF1GIOnfaCxVmRwaM4F4uPy68xD5kIWDx4JthzeM6kyg4WUvHB8Hxi12yBVm7pTcIbkMMPS8dUGOqUB91cfQRK1IsgsJU0xa2eo6MYNSlhvZXV%2Fi9kUZ7z%2FriNQk3fn0wB6nE4IkgR%2BZAlM6AWzcYXyMz%2BTAZ7mnoc1vz3g%2BsMl3wDArBixvZW2BL5MF5YvnDSAaC9OaQNSRJnmWau8Ya7T6HAr0k05FrmZtMF3k5mfVkqNezwtV0KlfAZOAKD8%2B9YEJ4VrVbJSEY1vqt3v7VquCswjyGTbXKHIS%2F%2BFtMNs&X-Amz-Signature=c13dd72abfd208b9d21eaffc1c6963fad0dbc16fc35d3f117fc75727f1390f7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRGVVM6Y%2F20260930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260930T025733Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDa7KwjQBS5JPLWIIKqdFuToUasNXfqe%2FhXsDJIjyVRVAiEA%2F%2F16jxbV4d1TpS8W5V5LhiErE90vE24FcD05kcafCgAq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDCmZnIqsZKE1%2FFIb2SrcAyQlGCEQdQ197O353T7iocKWdfMsj0%2FIJTjCbDHm7fYzWkQ3ud8OMdSY4MBE7L%2Bw1Xacdj%2B7iSAZmOYdkp68lmruhSKFnzAC8%2BE0a4KXnIo9L47xBab4THVpjZynv4X49axLFpUvGmqJSqB9kTM267Jvpz%2FjwbAJ54XKSrXG8RtzDzGTqIXG%2BmTKs54hnrstwdb8RRlr0yiteA5Sbg7aXFCyXkU1wRXsr7cWtb%2FGK7RexQoWSS7CAbCRYuMLvTavB2TbEC%2BBwPS2CsznuKKOerwvYiwcHt0ui8rGnCOkdCf6J5bDZvrujjs0O1H16GcyY1htsSKzmJCiJv6fa%2BCStequ96ClFeZXnOTflqb%2B4EhHXrr93WgvWzjGFDi80iyXd2ikIkhR3gwviIbws5me5RQATw%2F81dm9BDxMoYj0mzrni62cOnAmejD8tbuZSVEfjXN6g%2FxrndfUy%2BtAgSRKRxR3jh%2Bbxmi0C3kH7UiL9BgGbajIWyieCTNiCKESJXctJeImS6oKtbT7llB3PIDJNc61d1dIsMuGWNFhbhlhFQAYUSY%2BhOc94qUokTF1GIOnfaCxVmRwaM4F4uPy68xD5kIWDx4JthzeM6kyg4WUvHB8Hxi12yBVm7pTcIbkMMPS8dUGOqUB91cfQRK1IsgsJU0xa2eo6MYNSlhvZXV%2Fi9kUZ7z%2FriNQk3fn0wB6nE4IkgR%2BZAlM6AWzcYXyMz%2BTAZ7mnoc1vz3g%2BsMl3wDArBixvZW2BL5MF5YvnDSAaC9OaQNSRJnmWau8Ya7T6HAr0k05FrmZtMF3k5mfVkqNezwtV0KlfAZOAKD8%2B9YEJ4VrVbJSEY1vqt3v7VquCswjyGTbXKHIS%2F%2BFtMNs&X-Amz-Signature=57d8a9366d8d470dd410fe29319ba730c7d5aff8806b19100462f8740afaf987&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRGVVM6Y%2F20260930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260930T025733Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDa7KwjQBS5JPLWIIKqdFuToUasNXfqe%2FhXsDJIjyVRVAiEA%2F%2F16jxbV4d1TpS8W5V5LhiErE90vE24FcD05kcafCgAq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDCmZnIqsZKE1%2FFIb2SrcAyQlGCEQdQ197O353T7iocKWdfMsj0%2FIJTjCbDHm7fYzWkQ3ud8OMdSY4MBE7L%2Bw1Xacdj%2B7iSAZmOYdkp68lmruhSKFnzAC8%2BE0a4KXnIo9L47xBab4THVpjZynv4X49axLFpUvGmqJSqB9kTM267Jvpz%2FjwbAJ54XKSrXG8RtzDzGTqIXG%2BmTKs54hnrstwdb8RRlr0yiteA5Sbg7aXFCyXkU1wRXsr7cWtb%2FGK7RexQoWSS7CAbCRYuMLvTavB2TbEC%2BBwPS2CsznuKKOerwvYiwcHt0ui8rGnCOkdCf6J5bDZvrujjs0O1H16GcyY1htsSKzmJCiJv6fa%2BCStequ96ClFeZXnOTflqb%2B4EhHXrr93WgvWzjGFDi80iyXd2ikIkhR3gwviIbws5me5RQATw%2F81dm9BDxMoYj0mzrni62cOnAmejD8tbuZSVEfjXN6g%2FxrndfUy%2BtAgSRKRxR3jh%2Bbxmi0C3kH7UiL9BgGbajIWyieCTNiCKESJXctJeImS6oKtbT7llB3PIDJNc61d1dIsMuGWNFhbhlhFQAYUSY%2BhOc94qUokTF1GIOnfaCxVmRwaM4F4uPy68xD5kIWDx4JthzeM6kyg4WUvHB8Hxi12yBVm7pTcIbkMMPS8dUGOqUB91cfQRK1IsgsJU0xa2eo6MYNSlhvZXV%2Fi9kUZ7z%2FriNQk3fn0wB6nE4IkgR%2BZAlM6AWzcYXyMz%2BTAZ7mnoc1vz3g%2BsMl3wDArBixvZW2BL5MF5YvnDSAaC9OaQNSRJnmWau8Ya7T6HAr0k05FrmZtMF3k5mfVkqNezwtV0KlfAZOAKD8%2B9YEJ4VrVbJSEY1vqt3v7VquCswjyGTbXKHIS%2F%2BFtMNs&X-Amz-Signature=1f5e8d5b1ee1c2d9a460fcf52549d042a318da696c1c2ccf31967d2147a5e6d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRGVVM6Y%2F20260930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260930T025733Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDa7KwjQBS5JPLWIIKqdFuToUasNXfqe%2FhXsDJIjyVRVAiEA%2F%2F16jxbV4d1TpS8W5V5LhiErE90vE24FcD05kcafCgAq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDCmZnIqsZKE1%2FFIb2SrcAyQlGCEQdQ197O353T7iocKWdfMsj0%2FIJTjCbDHm7fYzWkQ3ud8OMdSY4MBE7L%2Bw1Xacdj%2B7iSAZmOYdkp68lmruhSKFnzAC8%2BE0a4KXnIo9L47xBab4THVpjZynv4X49axLFpUvGmqJSqB9kTM267Jvpz%2FjwbAJ54XKSrXG8RtzDzGTqIXG%2BmTKs54hnrstwdb8RRlr0yiteA5Sbg7aXFCyXkU1wRXsr7cWtb%2FGK7RexQoWSS7CAbCRYuMLvTavB2TbEC%2BBwPS2CsznuKKOerwvYiwcHt0ui8rGnCOkdCf6J5bDZvrujjs0O1H16GcyY1htsSKzmJCiJv6fa%2BCStequ96ClFeZXnOTflqb%2B4EhHXrr93WgvWzjGFDi80iyXd2ikIkhR3gwviIbws5me5RQATw%2F81dm9BDxMoYj0mzrni62cOnAmejD8tbuZSVEfjXN6g%2FxrndfUy%2BtAgSRKRxR3jh%2Bbxmi0C3kH7UiL9BgGbajIWyieCTNiCKESJXctJeImS6oKtbT7llB3PIDJNc61d1dIsMuGWNFhbhlhFQAYUSY%2BhOc94qUokTF1GIOnfaCxVmRwaM4F4uPy68xD5kIWDx4JthzeM6kyg4WUvHB8Hxi12yBVm7pTcIbkMMPS8dUGOqUB91cfQRK1IsgsJU0xa2eo6MYNSlhvZXV%2Fi9kUZ7z%2FriNQk3fn0wB6nE4IkgR%2BZAlM6AWzcYXyMz%2BTAZ7mnoc1vz3g%2BsMl3wDArBixvZW2BL5MF5YvnDSAaC9OaQNSRJnmWau8Ya7T6HAr0k05FrmZtMF3k5mfVkqNezwtV0KlfAZOAKD8%2B9YEJ4VrVbJSEY1vqt3v7VquCswjyGTbXKHIS%2F%2BFtMNs&X-Amz-Signature=666a7941eccb54019f71ee43a9bdc66b9278fa9f8f4e27908b428bbf1eebc180&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRGVVM6Y%2F20260930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260930T025733Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDa7KwjQBS5JPLWIIKqdFuToUasNXfqe%2FhXsDJIjyVRVAiEA%2F%2F16jxbV4d1TpS8W5V5LhiErE90vE24FcD05kcafCgAq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDCmZnIqsZKE1%2FFIb2SrcAyQlGCEQdQ197O353T7iocKWdfMsj0%2FIJTjCbDHm7fYzWkQ3ud8OMdSY4MBE7L%2Bw1Xacdj%2B7iSAZmOYdkp68lmruhSKFnzAC8%2BE0a4KXnIo9L47xBab4THVpjZynv4X49axLFpUvGmqJSqB9kTM267Jvpz%2FjwbAJ54XKSrXG8RtzDzGTqIXG%2BmTKs54hnrstwdb8RRlr0yiteA5Sbg7aXFCyXkU1wRXsr7cWtb%2FGK7RexQoWSS7CAbCRYuMLvTavB2TbEC%2BBwPS2CsznuKKOerwvYiwcHt0ui8rGnCOkdCf6J5bDZvrujjs0O1H16GcyY1htsSKzmJCiJv6fa%2BCStequ96ClFeZXnOTflqb%2B4EhHXrr93WgvWzjGFDi80iyXd2ikIkhR3gwviIbws5me5RQATw%2F81dm9BDxMoYj0mzrni62cOnAmejD8tbuZSVEfjXN6g%2FxrndfUy%2BtAgSRKRxR3jh%2Bbxmi0C3kH7UiL9BgGbajIWyieCTNiCKESJXctJeImS6oKtbT7llB3PIDJNc61d1dIsMuGWNFhbhlhFQAYUSY%2BhOc94qUokTF1GIOnfaCxVmRwaM4F4uPy68xD5kIWDx4JthzeM6kyg4WUvHB8Hxi12yBVm7pTcIbkMMPS8dUGOqUB91cfQRK1IsgsJU0xa2eo6MYNSlhvZXV%2Fi9kUZ7z%2FriNQk3fn0wB6nE4IkgR%2BZAlM6AWzcYXyMz%2BTAZ7mnoc1vz3g%2BsMl3wDArBixvZW2BL5MF5YvnDSAaC9OaQNSRJnmWau8Ya7T6HAr0k05FrmZtMF3k5mfVkqNezwtV0KlfAZOAKD8%2B9YEJ4VrVbJSEY1vqt3v7VquCswjyGTbXKHIS%2F%2BFtMNs&X-Amz-Signature=5406655ac62743ecc81035f1279cf5ca9819f6c721063feaf6b5ba137075c75a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRGVVM6Y%2F20260930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260930T025733Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDa7KwjQBS5JPLWIIKqdFuToUasNXfqe%2FhXsDJIjyVRVAiEA%2F%2F16jxbV4d1TpS8W5V5LhiErE90vE24FcD05kcafCgAq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDCmZnIqsZKE1%2FFIb2SrcAyQlGCEQdQ197O353T7iocKWdfMsj0%2FIJTjCbDHm7fYzWkQ3ud8OMdSY4MBE7L%2Bw1Xacdj%2B7iSAZmOYdkp68lmruhSKFnzAC8%2BE0a4KXnIo9L47xBab4THVpjZynv4X49axLFpUvGmqJSqB9kTM267Jvpz%2FjwbAJ54XKSrXG8RtzDzGTqIXG%2BmTKs54hnrstwdb8RRlr0yiteA5Sbg7aXFCyXkU1wRXsr7cWtb%2FGK7RexQoWSS7CAbCRYuMLvTavB2TbEC%2BBwPS2CsznuKKOerwvYiwcHt0ui8rGnCOkdCf6J5bDZvrujjs0O1H16GcyY1htsSKzmJCiJv6fa%2BCStequ96ClFeZXnOTflqb%2B4EhHXrr93WgvWzjGFDi80iyXd2ikIkhR3gwviIbws5me5RQATw%2F81dm9BDxMoYj0mzrni62cOnAmejD8tbuZSVEfjXN6g%2FxrndfUy%2BtAgSRKRxR3jh%2Bbxmi0C3kH7UiL9BgGbajIWyieCTNiCKESJXctJeImS6oKtbT7llB3PIDJNc61d1dIsMuGWNFhbhlhFQAYUSY%2BhOc94qUokTF1GIOnfaCxVmRwaM4F4uPy68xD5kIWDx4JthzeM6kyg4WUvHB8Hxi12yBVm7pTcIbkMMPS8dUGOqUB91cfQRK1IsgsJU0xa2eo6MYNSlhvZXV%2Fi9kUZ7z%2FriNQk3fn0wB6nE4IkgR%2BZAlM6AWzcYXyMz%2BTAZ7mnoc1vz3g%2BsMl3wDArBixvZW2BL5MF5YvnDSAaC9OaQNSRJnmWau8Ya7T6HAr0k05FrmZtMF3k5mfVkqNezwtV0KlfAZOAKD8%2B9YEJ4VrVbJSEY1vqt3v7VquCswjyGTbXKHIS%2F%2BFtMNs&X-Amz-Signature=c741592ad318c6647fe79da685c2d85327097f6d37c826e01d2d5c054df4baa1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRGVVM6Y%2F20260930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260930T025733Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDa7KwjQBS5JPLWIIKqdFuToUasNXfqe%2FhXsDJIjyVRVAiEA%2F%2F16jxbV4d1TpS8W5V5LhiErE90vE24FcD05kcafCgAq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDCmZnIqsZKE1%2FFIb2SrcAyQlGCEQdQ197O353T7iocKWdfMsj0%2FIJTjCbDHm7fYzWkQ3ud8OMdSY4MBE7L%2Bw1Xacdj%2B7iSAZmOYdkp68lmruhSKFnzAC8%2BE0a4KXnIo9L47xBab4THVpjZynv4X49axLFpUvGmqJSqB9kTM267Jvpz%2FjwbAJ54XKSrXG8RtzDzGTqIXG%2BmTKs54hnrstwdb8RRlr0yiteA5Sbg7aXFCyXkU1wRXsr7cWtb%2FGK7RexQoWSS7CAbCRYuMLvTavB2TbEC%2BBwPS2CsznuKKOerwvYiwcHt0ui8rGnCOkdCf6J5bDZvrujjs0O1H16GcyY1htsSKzmJCiJv6fa%2BCStequ96ClFeZXnOTflqb%2B4EhHXrr93WgvWzjGFDi80iyXd2ikIkhR3gwviIbws5me5RQATw%2F81dm9BDxMoYj0mzrni62cOnAmejD8tbuZSVEfjXN6g%2FxrndfUy%2BtAgSRKRxR3jh%2Bbxmi0C3kH7UiL9BgGbajIWyieCTNiCKESJXctJeImS6oKtbT7llB3PIDJNc61d1dIsMuGWNFhbhlhFQAYUSY%2BhOc94qUokTF1GIOnfaCxVmRwaM4F4uPy68xD5kIWDx4JthzeM6kyg4WUvHB8Hxi12yBVm7pTcIbkMMPS8dUGOqUB91cfQRK1IsgsJU0xa2eo6MYNSlhvZXV%2Fi9kUZ7z%2FriNQk3fn0wB6nE4IkgR%2BZAlM6AWzcYXyMz%2BTAZ7mnoc1vz3g%2BsMl3wDArBixvZW2BL5MF5YvnDSAaC9OaQNSRJnmWau8Ya7T6HAr0k05FrmZtMF3k5mfVkqNezwtV0KlfAZOAKD8%2B9YEJ4VrVbJSEY1vqt3v7VquCswjyGTbXKHIS%2F%2BFtMNs&X-Amz-Signature=76998f8d515826e85a7bd32ccd6a674f7937f8226fe5372b08d10424a22a1950&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRGVVM6Y%2F20260930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260930T025733Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDa7KwjQBS5JPLWIIKqdFuToUasNXfqe%2FhXsDJIjyVRVAiEA%2F%2F16jxbV4d1TpS8W5V5LhiErE90vE24FcD05kcafCgAq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDCmZnIqsZKE1%2FFIb2SrcAyQlGCEQdQ197O353T7iocKWdfMsj0%2FIJTjCbDHm7fYzWkQ3ud8OMdSY4MBE7L%2Bw1Xacdj%2B7iSAZmOYdkp68lmruhSKFnzAC8%2BE0a4KXnIo9L47xBab4THVpjZynv4X49axLFpUvGmqJSqB9kTM267Jvpz%2FjwbAJ54XKSrXG8RtzDzGTqIXG%2BmTKs54hnrstwdb8RRlr0yiteA5Sbg7aXFCyXkU1wRXsr7cWtb%2FGK7RexQoWSS7CAbCRYuMLvTavB2TbEC%2BBwPS2CsznuKKOerwvYiwcHt0ui8rGnCOkdCf6J5bDZvrujjs0O1H16GcyY1htsSKzmJCiJv6fa%2BCStequ96ClFeZXnOTflqb%2B4EhHXrr93WgvWzjGFDi80iyXd2ikIkhR3gwviIbws5me5RQATw%2F81dm9BDxMoYj0mzrni62cOnAmejD8tbuZSVEfjXN6g%2FxrndfUy%2BtAgSRKRxR3jh%2Bbxmi0C3kH7UiL9BgGbajIWyieCTNiCKESJXctJeImS6oKtbT7llB3PIDJNc61d1dIsMuGWNFhbhlhFQAYUSY%2BhOc94qUokTF1GIOnfaCxVmRwaM4F4uPy68xD5kIWDx4JthzeM6kyg4WUvHB8Hxi12yBVm7pTcIbkMMPS8dUGOqUB91cfQRK1IsgsJU0xa2eo6MYNSlhvZXV%2Fi9kUZ7z%2FriNQk3fn0wB6nE4IkgR%2BZAlM6AWzcYXyMz%2BTAZ7mnoc1vz3g%2BsMl3wDArBixvZW2BL5MF5YvnDSAaC9OaQNSRJnmWau8Ya7T6HAr0k05FrmZtMF3k5mfVkqNezwtV0KlfAZOAKD8%2B9YEJ4VrVbJSEY1vqt3v7VquCswjyGTbXKHIS%2F%2BFtMNs&X-Amz-Signature=a2607ef891af1d7fcafb3808b779b5a5061a30572efcdd9cae94ed9115448a03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRGVVM6Y%2F20260930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260930T025733Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDa7KwjQBS5JPLWIIKqdFuToUasNXfqe%2FhXsDJIjyVRVAiEA%2F%2F16jxbV4d1TpS8W5V5LhiErE90vE24FcD05kcafCgAq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDCmZnIqsZKE1%2FFIb2SrcAyQlGCEQdQ197O353T7iocKWdfMsj0%2FIJTjCbDHm7fYzWkQ3ud8OMdSY4MBE7L%2Bw1Xacdj%2B7iSAZmOYdkp68lmruhSKFnzAC8%2BE0a4KXnIo9L47xBab4THVpjZynv4X49axLFpUvGmqJSqB9kTM267Jvpz%2FjwbAJ54XKSrXG8RtzDzGTqIXG%2BmTKs54hnrstwdb8RRlr0yiteA5Sbg7aXFCyXkU1wRXsr7cWtb%2FGK7RexQoWSS7CAbCRYuMLvTavB2TbEC%2BBwPS2CsznuKKOerwvYiwcHt0ui8rGnCOkdCf6J5bDZvrujjs0O1H16GcyY1htsSKzmJCiJv6fa%2BCStequ96ClFeZXnOTflqb%2B4EhHXrr93WgvWzjGFDi80iyXd2ikIkhR3gwviIbws5me5RQATw%2F81dm9BDxMoYj0mzrni62cOnAmejD8tbuZSVEfjXN6g%2FxrndfUy%2BtAgSRKRxR3jh%2Bbxmi0C3kH7UiL9BgGbajIWyieCTNiCKESJXctJeImS6oKtbT7llB3PIDJNc61d1dIsMuGWNFhbhlhFQAYUSY%2BhOc94qUokTF1GIOnfaCxVmRwaM4F4uPy68xD5kIWDx4JthzeM6kyg4WUvHB8Hxi12yBVm7pTcIbkMMPS8dUGOqUB91cfQRK1IsgsJU0xa2eo6MYNSlhvZXV%2Fi9kUZ7z%2FriNQk3fn0wB6nE4IkgR%2BZAlM6AWzcYXyMz%2BTAZ7mnoc1vz3g%2BsMl3wDArBixvZW2BL5MF5YvnDSAaC9OaQNSRJnmWau8Ya7T6HAr0k05FrmZtMF3k5mfVkqNezwtV0KlfAZOAKD8%2B9YEJ4VrVbJSEY1vqt3v7VquCswjyGTbXKHIS%2F%2BFtMNs&X-Amz-Signature=9affabdc3142ec90c5febbf4623cfad13865fcfdd71edbf3662e6da877ed54eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
