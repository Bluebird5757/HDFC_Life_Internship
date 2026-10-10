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
fetched_at: '2026-10-10T03:18:42.801Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HZLQSU6%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031838Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFU6SYWfTy%2FXAo9ZvIJnfzp%2B7AKLz42sEYF1xGfFdPP2AiAbkr%2FZrUUea2U0myUZqCvR9oGsHVo6%2BQrSVCz838KofCr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIM4xCrJUAvREePRqeWKtwDGla5HwGnfIuB%2BIHN2DxlZ1vUNSUf%2FI3ABG2hDT9XY3pKYBtBMY37uVUZzEseu4cLiMn%2FtU2WVUpeg3el8OTv0PBkeApdw60wVNUqAGm%2FCdipWQGATaGOvGzS2SDLKmQkLe1iuuv%2FpA8tgAeeQlwxIifbYgsPBNtA1pRNM509%2FiISSqIyvynFrfU3zhl%2BHu00IwQDdECP95IfwALy6tVEHP2JXbniLB7JxFGSR%2B4Az9kvV5GC%2B99JcC7%2FZmgNWSC0EM9QW4HaGl%2FJwDpONGURqrxL7mRcMlgQGZmQE4z8QrtoCJOGpmBSmmEimeIEmqz%2BJTc2eKydzlx5UVgMp8wpm%2BMWtydtDk4IPfJYSVVCY9nFShsUwkKUaavQmrDkKiYYMG3l1fKUtRo7ExWVavbgP3R%2BR78MRRAH%2FU0DAZnX%2FbK9tWPTodzLy3Y1TjeKydpP5Vg0v5htu4a2%2BN92jkLAaZY%2BdC9ri5DQ7znG7qZ1hNvf6mY60DM3PTzWIOV9b3KsyCN7jgP3a0HtpX4MGRNOeTNR0%2BuHiwR7B7Sh4shi8pdb34WxVMS6M9XTcSxpE3NXX7bgLAP0YXYz8ZJlGKQf2XgTzWFOedgS0e%2F0YO1F0cGIzmnGRcVpD%2BYlmI4w%2F8am1gY6pgH%2BujpdNEXCdZmB2ROUk2WOjXPUCxE7bkutmmMrQ%2FAU%2BKaRtvZQyPvf36IJlkZz%2F9coXMNVfQSB9XWi7vyzdToFNcvQWDolvqIwaAlmFQhDYbxDxFTerkeivcfcXjMO2YDo1DKNLRO%2BuL6qlpfcbmikVIWguvkYj%2BFVJQBknor9bYcNTam%2BKmvIdNTA5h1i9ceAiRk5NkvtxVEG1AXiy6mgcwB7apy4&X-Amz-Signature=010584325d5b4f00db4915c019ff5a43c37ea06e7ed0791ce0172580408926fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HZLQSU6%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031838Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFU6SYWfTy%2FXAo9ZvIJnfzp%2B7AKLz42sEYF1xGfFdPP2AiAbkr%2FZrUUea2U0myUZqCvR9oGsHVo6%2BQrSVCz838KofCr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIM4xCrJUAvREePRqeWKtwDGla5HwGnfIuB%2BIHN2DxlZ1vUNSUf%2FI3ABG2hDT9XY3pKYBtBMY37uVUZzEseu4cLiMn%2FtU2WVUpeg3el8OTv0PBkeApdw60wVNUqAGm%2FCdipWQGATaGOvGzS2SDLKmQkLe1iuuv%2FpA8tgAeeQlwxIifbYgsPBNtA1pRNM509%2FiISSqIyvynFrfU3zhl%2BHu00IwQDdECP95IfwALy6tVEHP2JXbniLB7JxFGSR%2B4Az9kvV5GC%2B99JcC7%2FZmgNWSC0EM9QW4HaGl%2FJwDpONGURqrxL7mRcMlgQGZmQE4z8QrtoCJOGpmBSmmEimeIEmqz%2BJTc2eKydzlx5UVgMp8wpm%2BMWtydtDk4IPfJYSVVCY9nFShsUwkKUaavQmrDkKiYYMG3l1fKUtRo7ExWVavbgP3R%2BR78MRRAH%2FU0DAZnX%2FbK9tWPTodzLy3Y1TjeKydpP5Vg0v5htu4a2%2BN92jkLAaZY%2BdC9ri5DQ7znG7qZ1hNvf6mY60DM3PTzWIOV9b3KsyCN7jgP3a0HtpX4MGRNOeTNR0%2BuHiwR7B7Sh4shi8pdb34WxVMS6M9XTcSxpE3NXX7bgLAP0YXYz8ZJlGKQf2XgTzWFOedgS0e%2F0YO1F0cGIzmnGRcVpD%2BYlmI4w%2F8am1gY6pgH%2BujpdNEXCdZmB2ROUk2WOjXPUCxE7bkutmmMrQ%2FAU%2BKaRtvZQyPvf36IJlkZz%2F9coXMNVfQSB9XWi7vyzdToFNcvQWDolvqIwaAlmFQhDYbxDxFTerkeivcfcXjMO2YDo1DKNLRO%2BuL6qlpfcbmikVIWguvkYj%2BFVJQBknor9bYcNTam%2BKmvIdNTA5h1i9ceAiRk5NkvtxVEG1AXiy6mgcwB7apy4&X-Amz-Signature=2f040621b2e0bc61444a4ec2eefbe8f724136d54c146f755d08bc78ea41a1504&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HZLQSU6%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031838Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFU6SYWfTy%2FXAo9ZvIJnfzp%2B7AKLz42sEYF1xGfFdPP2AiAbkr%2FZrUUea2U0myUZqCvR9oGsHVo6%2BQrSVCz838KofCr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIM4xCrJUAvREePRqeWKtwDGla5HwGnfIuB%2BIHN2DxlZ1vUNSUf%2FI3ABG2hDT9XY3pKYBtBMY37uVUZzEseu4cLiMn%2FtU2WVUpeg3el8OTv0PBkeApdw60wVNUqAGm%2FCdipWQGATaGOvGzS2SDLKmQkLe1iuuv%2FpA8tgAeeQlwxIifbYgsPBNtA1pRNM509%2FiISSqIyvynFrfU3zhl%2BHu00IwQDdECP95IfwALy6tVEHP2JXbniLB7JxFGSR%2B4Az9kvV5GC%2B99JcC7%2FZmgNWSC0EM9QW4HaGl%2FJwDpONGURqrxL7mRcMlgQGZmQE4z8QrtoCJOGpmBSmmEimeIEmqz%2BJTc2eKydzlx5UVgMp8wpm%2BMWtydtDk4IPfJYSVVCY9nFShsUwkKUaavQmrDkKiYYMG3l1fKUtRo7ExWVavbgP3R%2BR78MRRAH%2FU0DAZnX%2FbK9tWPTodzLy3Y1TjeKydpP5Vg0v5htu4a2%2BN92jkLAaZY%2BdC9ri5DQ7znG7qZ1hNvf6mY60DM3PTzWIOV9b3KsyCN7jgP3a0HtpX4MGRNOeTNR0%2BuHiwR7B7Sh4shi8pdb34WxVMS6M9XTcSxpE3NXX7bgLAP0YXYz8ZJlGKQf2XgTzWFOedgS0e%2F0YO1F0cGIzmnGRcVpD%2BYlmI4w%2F8am1gY6pgH%2BujpdNEXCdZmB2ROUk2WOjXPUCxE7bkutmmMrQ%2FAU%2BKaRtvZQyPvf36IJlkZz%2F9coXMNVfQSB9XWi7vyzdToFNcvQWDolvqIwaAlmFQhDYbxDxFTerkeivcfcXjMO2YDo1DKNLRO%2BuL6qlpfcbmikVIWguvkYj%2BFVJQBknor9bYcNTam%2BKmvIdNTA5h1i9ceAiRk5NkvtxVEG1AXiy6mgcwB7apy4&X-Amz-Signature=a3555ab372b8cfb23be929c9321828544d8dd0b4641447c54c8885d559f2ac45&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HZLQSU6%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031838Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFU6SYWfTy%2FXAo9ZvIJnfzp%2B7AKLz42sEYF1xGfFdPP2AiAbkr%2FZrUUea2U0myUZqCvR9oGsHVo6%2BQrSVCz838KofCr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIM4xCrJUAvREePRqeWKtwDGla5HwGnfIuB%2BIHN2DxlZ1vUNSUf%2FI3ABG2hDT9XY3pKYBtBMY37uVUZzEseu4cLiMn%2FtU2WVUpeg3el8OTv0PBkeApdw60wVNUqAGm%2FCdipWQGATaGOvGzS2SDLKmQkLe1iuuv%2FpA8tgAeeQlwxIifbYgsPBNtA1pRNM509%2FiISSqIyvynFrfU3zhl%2BHu00IwQDdECP95IfwALy6tVEHP2JXbniLB7JxFGSR%2B4Az9kvV5GC%2B99JcC7%2FZmgNWSC0EM9QW4HaGl%2FJwDpONGURqrxL7mRcMlgQGZmQE4z8QrtoCJOGpmBSmmEimeIEmqz%2BJTc2eKydzlx5UVgMp8wpm%2BMWtydtDk4IPfJYSVVCY9nFShsUwkKUaavQmrDkKiYYMG3l1fKUtRo7ExWVavbgP3R%2BR78MRRAH%2FU0DAZnX%2FbK9tWPTodzLy3Y1TjeKydpP5Vg0v5htu4a2%2BN92jkLAaZY%2BdC9ri5DQ7znG7qZ1hNvf6mY60DM3PTzWIOV9b3KsyCN7jgP3a0HtpX4MGRNOeTNR0%2BuHiwR7B7Sh4shi8pdb34WxVMS6M9XTcSxpE3NXX7bgLAP0YXYz8ZJlGKQf2XgTzWFOedgS0e%2F0YO1F0cGIzmnGRcVpD%2BYlmI4w%2F8am1gY6pgH%2BujpdNEXCdZmB2ROUk2WOjXPUCxE7bkutmmMrQ%2FAU%2BKaRtvZQyPvf36IJlkZz%2F9coXMNVfQSB9XWi7vyzdToFNcvQWDolvqIwaAlmFQhDYbxDxFTerkeivcfcXjMO2YDo1DKNLRO%2BuL6qlpfcbmikVIWguvkYj%2BFVJQBknor9bYcNTam%2BKmvIdNTA5h1i9ceAiRk5NkvtxVEG1AXiy6mgcwB7apy4&X-Amz-Signature=9a3c8a9eae1c5bee4c70854fc97981a5836256722b28354dc805438edce838e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HZLQSU6%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031838Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFU6SYWfTy%2FXAo9ZvIJnfzp%2B7AKLz42sEYF1xGfFdPP2AiAbkr%2FZrUUea2U0myUZqCvR9oGsHVo6%2BQrSVCz838KofCr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIM4xCrJUAvREePRqeWKtwDGla5HwGnfIuB%2BIHN2DxlZ1vUNSUf%2FI3ABG2hDT9XY3pKYBtBMY37uVUZzEseu4cLiMn%2FtU2WVUpeg3el8OTv0PBkeApdw60wVNUqAGm%2FCdipWQGATaGOvGzS2SDLKmQkLe1iuuv%2FpA8tgAeeQlwxIifbYgsPBNtA1pRNM509%2FiISSqIyvynFrfU3zhl%2BHu00IwQDdECP95IfwALy6tVEHP2JXbniLB7JxFGSR%2B4Az9kvV5GC%2B99JcC7%2FZmgNWSC0EM9QW4HaGl%2FJwDpONGURqrxL7mRcMlgQGZmQE4z8QrtoCJOGpmBSmmEimeIEmqz%2BJTc2eKydzlx5UVgMp8wpm%2BMWtydtDk4IPfJYSVVCY9nFShsUwkKUaavQmrDkKiYYMG3l1fKUtRo7ExWVavbgP3R%2BR78MRRAH%2FU0DAZnX%2FbK9tWPTodzLy3Y1TjeKydpP5Vg0v5htu4a2%2BN92jkLAaZY%2BdC9ri5DQ7znG7qZ1hNvf6mY60DM3PTzWIOV9b3KsyCN7jgP3a0HtpX4MGRNOeTNR0%2BuHiwR7B7Sh4shi8pdb34WxVMS6M9XTcSxpE3NXX7bgLAP0YXYz8ZJlGKQf2XgTzWFOedgS0e%2F0YO1F0cGIzmnGRcVpD%2BYlmI4w%2F8am1gY6pgH%2BujpdNEXCdZmB2ROUk2WOjXPUCxE7bkutmmMrQ%2FAU%2BKaRtvZQyPvf36IJlkZz%2F9coXMNVfQSB9XWi7vyzdToFNcvQWDolvqIwaAlmFQhDYbxDxFTerkeivcfcXjMO2YDo1DKNLRO%2BuL6qlpfcbmikVIWguvkYj%2BFVJQBknor9bYcNTam%2BKmvIdNTA5h1i9ceAiRk5NkvtxVEG1AXiy6mgcwB7apy4&X-Amz-Signature=88eadfbb126940d45d899e226176c79acaeaf99e871bb34adce79c1a6d8ae32e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HZLQSU6%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031838Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFU6SYWfTy%2FXAo9ZvIJnfzp%2B7AKLz42sEYF1xGfFdPP2AiAbkr%2FZrUUea2U0myUZqCvR9oGsHVo6%2BQrSVCz838KofCr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIM4xCrJUAvREePRqeWKtwDGla5HwGnfIuB%2BIHN2DxlZ1vUNSUf%2FI3ABG2hDT9XY3pKYBtBMY37uVUZzEseu4cLiMn%2FtU2WVUpeg3el8OTv0PBkeApdw60wVNUqAGm%2FCdipWQGATaGOvGzS2SDLKmQkLe1iuuv%2FpA8tgAeeQlwxIifbYgsPBNtA1pRNM509%2FiISSqIyvynFrfU3zhl%2BHu00IwQDdECP95IfwALy6tVEHP2JXbniLB7JxFGSR%2B4Az9kvV5GC%2B99JcC7%2FZmgNWSC0EM9QW4HaGl%2FJwDpONGURqrxL7mRcMlgQGZmQE4z8QrtoCJOGpmBSmmEimeIEmqz%2BJTc2eKydzlx5UVgMp8wpm%2BMWtydtDk4IPfJYSVVCY9nFShsUwkKUaavQmrDkKiYYMG3l1fKUtRo7ExWVavbgP3R%2BR78MRRAH%2FU0DAZnX%2FbK9tWPTodzLy3Y1TjeKydpP5Vg0v5htu4a2%2BN92jkLAaZY%2BdC9ri5DQ7znG7qZ1hNvf6mY60DM3PTzWIOV9b3KsyCN7jgP3a0HtpX4MGRNOeTNR0%2BuHiwR7B7Sh4shi8pdb34WxVMS6M9XTcSxpE3NXX7bgLAP0YXYz8ZJlGKQf2XgTzWFOedgS0e%2F0YO1F0cGIzmnGRcVpD%2BYlmI4w%2F8am1gY6pgH%2BujpdNEXCdZmB2ROUk2WOjXPUCxE7bkutmmMrQ%2FAU%2BKaRtvZQyPvf36IJlkZz%2F9coXMNVfQSB9XWi7vyzdToFNcvQWDolvqIwaAlmFQhDYbxDxFTerkeivcfcXjMO2YDo1DKNLRO%2BuL6qlpfcbmikVIWguvkYj%2BFVJQBknor9bYcNTam%2BKmvIdNTA5h1i9ceAiRk5NkvtxVEG1AXiy6mgcwB7apy4&X-Amz-Signature=c2bbdac4f4fd296c609d566ac6c51e596c6e37b990d9acc075cde16b67107c8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HZLQSU6%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031838Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFU6SYWfTy%2FXAo9ZvIJnfzp%2B7AKLz42sEYF1xGfFdPP2AiAbkr%2FZrUUea2U0myUZqCvR9oGsHVo6%2BQrSVCz838KofCr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIM4xCrJUAvREePRqeWKtwDGla5HwGnfIuB%2BIHN2DxlZ1vUNSUf%2FI3ABG2hDT9XY3pKYBtBMY37uVUZzEseu4cLiMn%2FtU2WVUpeg3el8OTv0PBkeApdw60wVNUqAGm%2FCdipWQGATaGOvGzS2SDLKmQkLe1iuuv%2FpA8tgAeeQlwxIifbYgsPBNtA1pRNM509%2FiISSqIyvynFrfU3zhl%2BHu00IwQDdECP95IfwALy6tVEHP2JXbniLB7JxFGSR%2B4Az9kvV5GC%2B99JcC7%2FZmgNWSC0EM9QW4HaGl%2FJwDpONGURqrxL7mRcMlgQGZmQE4z8QrtoCJOGpmBSmmEimeIEmqz%2BJTc2eKydzlx5UVgMp8wpm%2BMWtydtDk4IPfJYSVVCY9nFShsUwkKUaavQmrDkKiYYMG3l1fKUtRo7ExWVavbgP3R%2BR78MRRAH%2FU0DAZnX%2FbK9tWPTodzLy3Y1TjeKydpP5Vg0v5htu4a2%2BN92jkLAaZY%2BdC9ri5DQ7znG7qZ1hNvf6mY60DM3PTzWIOV9b3KsyCN7jgP3a0HtpX4MGRNOeTNR0%2BuHiwR7B7Sh4shi8pdb34WxVMS6M9XTcSxpE3NXX7bgLAP0YXYz8ZJlGKQf2XgTzWFOedgS0e%2F0YO1F0cGIzmnGRcVpD%2BYlmI4w%2F8am1gY6pgH%2BujpdNEXCdZmB2ROUk2WOjXPUCxE7bkutmmMrQ%2FAU%2BKaRtvZQyPvf36IJlkZz%2F9coXMNVfQSB9XWi7vyzdToFNcvQWDolvqIwaAlmFQhDYbxDxFTerkeivcfcXjMO2YDo1DKNLRO%2BuL6qlpfcbmikVIWguvkYj%2BFVJQBknor9bYcNTam%2BKmvIdNTA5h1i9ceAiRk5NkvtxVEG1AXiy6mgcwB7apy4&X-Amz-Signature=265d250be7daf41f80567f0ab8a683ca44c15350a614bee56dba4db861e53d29&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HZLQSU6%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031838Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFU6SYWfTy%2FXAo9ZvIJnfzp%2B7AKLz42sEYF1xGfFdPP2AiAbkr%2FZrUUea2U0myUZqCvR9oGsHVo6%2BQrSVCz838KofCr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIM4xCrJUAvREePRqeWKtwDGla5HwGnfIuB%2BIHN2DxlZ1vUNSUf%2FI3ABG2hDT9XY3pKYBtBMY37uVUZzEseu4cLiMn%2FtU2WVUpeg3el8OTv0PBkeApdw60wVNUqAGm%2FCdipWQGATaGOvGzS2SDLKmQkLe1iuuv%2FpA8tgAeeQlwxIifbYgsPBNtA1pRNM509%2FiISSqIyvynFrfU3zhl%2BHu00IwQDdECP95IfwALy6tVEHP2JXbniLB7JxFGSR%2B4Az9kvV5GC%2B99JcC7%2FZmgNWSC0EM9QW4HaGl%2FJwDpONGURqrxL7mRcMlgQGZmQE4z8QrtoCJOGpmBSmmEimeIEmqz%2BJTc2eKydzlx5UVgMp8wpm%2BMWtydtDk4IPfJYSVVCY9nFShsUwkKUaavQmrDkKiYYMG3l1fKUtRo7ExWVavbgP3R%2BR78MRRAH%2FU0DAZnX%2FbK9tWPTodzLy3Y1TjeKydpP5Vg0v5htu4a2%2BN92jkLAaZY%2BdC9ri5DQ7znG7qZ1hNvf6mY60DM3PTzWIOV9b3KsyCN7jgP3a0HtpX4MGRNOeTNR0%2BuHiwR7B7Sh4shi8pdb34WxVMS6M9XTcSxpE3NXX7bgLAP0YXYz8ZJlGKQf2XgTzWFOedgS0e%2F0YO1F0cGIzmnGRcVpD%2BYlmI4w%2F8am1gY6pgH%2BujpdNEXCdZmB2ROUk2WOjXPUCxE7bkutmmMrQ%2FAU%2BKaRtvZQyPvf36IJlkZz%2F9coXMNVfQSB9XWi7vyzdToFNcvQWDolvqIwaAlmFQhDYbxDxFTerkeivcfcXjMO2YDo1DKNLRO%2BuL6qlpfcbmikVIWguvkYj%2BFVJQBknor9bYcNTam%2BKmvIdNTA5h1i9ceAiRk5NkvtxVEG1AXiy6mgcwB7apy4&X-Amz-Signature=a26c83c543ab3e00ca431ccbb639f5f0ecd109cb63bb4c5dbdf7ffc17be19b6b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HZLQSU6%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031838Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFU6SYWfTy%2FXAo9ZvIJnfzp%2B7AKLz42sEYF1xGfFdPP2AiAbkr%2FZrUUea2U0myUZqCvR9oGsHVo6%2BQrSVCz838KofCr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIM4xCrJUAvREePRqeWKtwDGla5HwGnfIuB%2BIHN2DxlZ1vUNSUf%2FI3ABG2hDT9XY3pKYBtBMY37uVUZzEseu4cLiMn%2FtU2WVUpeg3el8OTv0PBkeApdw60wVNUqAGm%2FCdipWQGATaGOvGzS2SDLKmQkLe1iuuv%2FpA8tgAeeQlwxIifbYgsPBNtA1pRNM509%2FiISSqIyvynFrfU3zhl%2BHu00IwQDdECP95IfwALy6tVEHP2JXbniLB7JxFGSR%2B4Az9kvV5GC%2B99JcC7%2FZmgNWSC0EM9QW4HaGl%2FJwDpONGURqrxL7mRcMlgQGZmQE4z8QrtoCJOGpmBSmmEimeIEmqz%2BJTc2eKydzlx5UVgMp8wpm%2BMWtydtDk4IPfJYSVVCY9nFShsUwkKUaavQmrDkKiYYMG3l1fKUtRo7ExWVavbgP3R%2BR78MRRAH%2FU0DAZnX%2FbK9tWPTodzLy3Y1TjeKydpP5Vg0v5htu4a2%2BN92jkLAaZY%2BdC9ri5DQ7znG7qZ1hNvf6mY60DM3PTzWIOV9b3KsyCN7jgP3a0HtpX4MGRNOeTNR0%2BuHiwR7B7Sh4shi8pdb34WxVMS6M9XTcSxpE3NXX7bgLAP0YXYz8ZJlGKQf2XgTzWFOedgS0e%2F0YO1F0cGIzmnGRcVpD%2BYlmI4w%2F8am1gY6pgH%2BujpdNEXCdZmB2ROUk2WOjXPUCxE7bkutmmMrQ%2FAU%2BKaRtvZQyPvf36IJlkZz%2F9coXMNVfQSB9XWi7vyzdToFNcvQWDolvqIwaAlmFQhDYbxDxFTerkeivcfcXjMO2YDo1DKNLRO%2BuL6qlpfcbmikVIWguvkYj%2BFVJQBknor9bYcNTam%2BKmvIdNTA5h1i9ceAiRk5NkvtxVEG1AXiy6mgcwB7apy4&X-Amz-Signature=c38cca46c5f709d060bd52b802eea1b8b543c432607729898999a3eb637b2c70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
