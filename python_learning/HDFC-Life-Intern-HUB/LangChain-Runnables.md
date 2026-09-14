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
fetched_at: '2026-09-14T02:19:32.235Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXWBT4CY%2F20260914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260914T021925Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIQCaCQjwKjGXwINBIPrqCliqiqgez%2Fo4cJynX2Fin50XSgIgchWlbLXxv5e1lf4LxwT3rGUeczzwSZTYwA2xvI0S7CMqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMBmieBOxh2kbwEInSrcA70zbZdMc5fjT8q6MAxJN97kR2gmVgiGzfnDmCVU6McGcCyobDRUzPyIy68Nzm9PoFg8BaLasyZmumBvkRPJjGnSeSGzAw5ocPoUahFuJxA%2Bzq%2FitahsTmxzxllQiwBTQaQNundSuQtTkpC2scY9yr4DpG65RzzOJ%2Beck0Lz9%2FTcGZwdsxMwb%2Fw7tFnr%2FPJzg4clEbEuA2jvTVZAZo%2B1XXVIn6In1Hfsg0%2FjiIeQnkWPL0joOIPX2qtTnRj9PLBuTiTy80bNbFoimHQiPU3BzjbZ65OfsRFEuEBmeKDdvT7wMR1p4mn01HVjy9UkxOwirQzfTqe5hJ%2F7i%2BwbkmxNF3WQ0eDq52kvO2HQ2MmnaCNGC7RSlrUz%2BEl30v5eXOj9EWWAA2qdFzTWuYQGRmYnuDiA6fa77AH3DPLCjdTLepv%2FG45rBBzsJ0l47LhPIb5XpmeDL70htcQ63XQlOK4%2FF3Q%2BkdBcxZ0tsq%2Fa71EGfyjE%2FeWsKd2%2FqGCHsCpSstRSLXp3VQCcjMe1MUdpgr2D9ALm%2FZKZfppogpFurEROkrvwX6tcnRYdRKUBCOD5r9y1vw%2B58HsbRA3h%2B2VLRPXJHr5iHN05FZF7%2BS6rLRvDbhHAxrmnem%2FkpH%2FMu%2FshMOSEndUGOqUBv%2FkCxGlTOn3%2FGxZJzzYzBV9A53ozlnWO1LOQoGUyJrMi340OV8Ryn86ysMU5wzvZPteLAHfswfUntPVIFQDJseNVUqqnn%2FqsEh81EXzxg1jLrhoX8pq%2FiwOQDEQqR13dkNlrW8n1kX21slj%2BTV0f%2FxgY54UZDRzW1GBZZLZOtoR9cwaKwMcw1x5bPXEvjzsIuhyEP9jHFdW%2Bz9QJ1FmdL1ls%2BPYY&X-Amz-Signature=12eaa8613c179453efc55b70d05847560af1f074c97dc0f47d6d1a244e456c02&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXWBT4CY%2F20260914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260914T021925Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIQCaCQjwKjGXwINBIPrqCliqiqgez%2Fo4cJynX2Fin50XSgIgchWlbLXxv5e1lf4LxwT3rGUeczzwSZTYwA2xvI0S7CMqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMBmieBOxh2kbwEInSrcA70zbZdMc5fjT8q6MAxJN97kR2gmVgiGzfnDmCVU6McGcCyobDRUzPyIy68Nzm9PoFg8BaLasyZmumBvkRPJjGnSeSGzAw5ocPoUahFuJxA%2Bzq%2FitahsTmxzxllQiwBTQaQNundSuQtTkpC2scY9yr4DpG65RzzOJ%2Beck0Lz9%2FTcGZwdsxMwb%2Fw7tFnr%2FPJzg4clEbEuA2jvTVZAZo%2B1XXVIn6In1Hfsg0%2FjiIeQnkWPL0joOIPX2qtTnRj9PLBuTiTy80bNbFoimHQiPU3BzjbZ65OfsRFEuEBmeKDdvT7wMR1p4mn01HVjy9UkxOwirQzfTqe5hJ%2F7i%2BwbkmxNF3WQ0eDq52kvO2HQ2MmnaCNGC7RSlrUz%2BEl30v5eXOj9EWWAA2qdFzTWuYQGRmYnuDiA6fa77AH3DPLCjdTLepv%2FG45rBBzsJ0l47LhPIb5XpmeDL70htcQ63XQlOK4%2FF3Q%2BkdBcxZ0tsq%2Fa71EGfyjE%2FeWsKd2%2FqGCHsCpSstRSLXp3VQCcjMe1MUdpgr2D9ALm%2FZKZfppogpFurEROkrvwX6tcnRYdRKUBCOD5r9y1vw%2B58HsbRA3h%2B2VLRPXJHr5iHN05FZF7%2BS6rLRvDbhHAxrmnem%2FkpH%2FMu%2FshMOSEndUGOqUBv%2FkCxGlTOn3%2FGxZJzzYzBV9A53ozlnWO1LOQoGUyJrMi340OV8Ryn86ysMU5wzvZPteLAHfswfUntPVIFQDJseNVUqqnn%2FqsEh81EXzxg1jLrhoX8pq%2FiwOQDEQqR13dkNlrW8n1kX21slj%2BTV0f%2FxgY54UZDRzW1GBZZLZOtoR9cwaKwMcw1x5bPXEvjzsIuhyEP9jHFdW%2Bz9QJ1FmdL1ls%2BPYY&X-Amz-Signature=d8d897c6050b939fad9e3ddaf5bc693357ecedc4e78b12414b92c078ce57eea4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXWBT4CY%2F20260914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260914T021926Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIQCaCQjwKjGXwINBIPrqCliqiqgez%2Fo4cJynX2Fin50XSgIgchWlbLXxv5e1lf4LxwT3rGUeczzwSZTYwA2xvI0S7CMqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMBmieBOxh2kbwEInSrcA70zbZdMc5fjT8q6MAxJN97kR2gmVgiGzfnDmCVU6McGcCyobDRUzPyIy68Nzm9PoFg8BaLasyZmumBvkRPJjGnSeSGzAw5ocPoUahFuJxA%2Bzq%2FitahsTmxzxllQiwBTQaQNundSuQtTkpC2scY9yr4DpG65RzzOJ%2Beck0Lz9%2FTcGZwdsxMwb%2Fw7tFnr%2FPJzg4clEbEuA2jvTVZAZo%2B1XXVIn6In1Hfsg0%2FjiIeQnkWPL0joOIPX2qtTnRj9PLBuTiTy80bNbFoimHQiPU3BzjbZ65OfsRFEuEBmeKDdvT7wMR1p4mn01HVjy9UkxOwirQzfTqe5hJ%2F7i%2BwbkmxNF3WQ0eDq52kvO2HQ2MmnaCNGC7RSlrUz%2BEl30v5eXOj9EWWAA2qdFzTWuYQGRmYnuDiA6fa77AH3DPLCjdTLepv%2FG45rBBzsJ0l47LhPIb5XpmeDL70htcQ63XQlOK4%2FF3Q%2BkdBcxZ0tsq%2Fa71EGfyjE%2FeWsKd2%2FqGCHsCpSstRSLXp3VQCcjMe1MUdpgr2D9ALm%2FZKZfppogpFurEROkrvwX6tcnRYdRKUBCOD5r9y1vw%2B58HsbRA3h%2B2VLRPXJHr5iHN05FZF7%2BS6rLRvDbhHAxrmnem%2FkpH%2FMu%2FshMOSEndUGOqUBv%2FkCxGlTOn3%2FGxZJzzYzBV9A53ozlnWO1LOQoGUyJrMi340OV8Ryn86ysMU5wzvZPteLAHfswfUntPVIFQDJseNVUqqnn%2FqsEh81EXzxg1jLrhoX8pq%2FiwOQDEQqR13dkNlrW8n1kX21slj%2BTV0f%2FxgY54UZDRzW1GBZZLZOtoR9cwaKwMcw1x5bPXEvjzsIuhyEP9jHFdW%2Bz9QJ1FmdL1ls%2BPYY&X-Amz-Signature=4320a69765256328abd46f4f45d22da0bcef338c6548ac5bc8e186bd3fdb99f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXWBT4CY%2F20260914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260914T021926Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIQCaCQjwKjGXwINBIPrqCliqiqgez%2Fo4cJynX2Fin50XSgIgchWlbLXxv5e1lf4LxwT3rGUeczzwSZTYwA2xvI0S7CMqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMBmieBOxh2kbwEInSrcA70zbZdMc5fjT8q6MAxJN97kR2gmVgiGzfnDmCVU6McGcCyobDRUzPyIy68Nzm9PoFg8BaLasyZmumBvkRPJjGnSeSGzAw5ocPoUahFuJxA%2Bzq%2FitahsTmxzxllQiwBTQaQNundSuQtTkpC2scY9yr4DpG65RzzOJ%2Beck0Lz9%2FTcGZwdsxMwb%2Fw7tFnr%2FPJzg4clEbEuA2jvTVZAZo%2B1XXVIn6In1Hfsg0%2FjiIeQnkWPL0joOIPX2qtTnRj9PLBuTiTy80bNbFoimHQiPU3BzjbZ65OfsRFEuEBmeKDdvT7wMR1p4mn01HVjy9UkxOwirQzfTqe5hJ%2F7i%2BwbkmxNF3WQ0eDq52kvO2HQ2MmnaCNGC7RSlrUz%2BEl30v5eXOj9EWWAA2qdFzTWuYQGRmYnuDiA6fa77AH3DPLCjdTLepv%2FG45rBBzsJ0l47LhPIb5XpmeDL70htcQ63XQlOK4%2FF3Q%2BkdBcxZ0tsq%2Fa71EGfyjE%2FeWsKd2%2FqGCHsCpSstRSLXp3VQCcjMe1MUdpgr2D9ALm%2FZKZfppogpFurEROkrvwX6tcnRYdRKUBCOD5r9y1vw%2B58HsbRA3h%2B2VLRPXJHr5iHN05FZF7%2BS6rLRvDbhHAxrmnem%2FkpH%2FMu%2FshMOSEndUGOqUBv%2FkCxGlTOn3%2FGxZJzzYzBV9A53ozlnWO1LOQoGUyJrMi340OV8Ryn86ysMU5wzvZPteLAHfswfUntPVIFQDJseNVUqqnn%2FqsEh81EXzxg1jLrhoX8pq%2FiwOQDEQqR13dkNlrW8n1kX21slj%2BTV0f%2FxgY54UZDRzW1GBZZLZOtoR9cwaKwMcw1x5bPXEvjzsIuhyEP9jHFdW%2Bz9QJ1FmdL1ls%2BPYY&X-Amz-Signature=18b2751b008629919669b5e3d3686283bf2e61cbbef52d26e07addf6d9460bc4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXWBT4CY%2F20260914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260914T021925Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIQCaCQjwKjGXwINBIPrqCliqiqgez%2Fo4cJynX2Fin50XSgIgchWlbLXxv5e1lf4LxwT3rGUeczzwSZTYwA2xvI0S7CMqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMBmieBOxh2kbwEInSrcA70zbZdMc5fjT8q6MAxJN97kR2gmVgiGzfnDmCVU6McGcCyobDRUzPyIy68Nzm9PoFg8BaLasyZmumBvkRPJjGnSeSGzAw5ocPoUahFuJxA%2Bzq%2FitahsTmxzxllQiwBTQaQNundSuQtTkpC2scY9yr4DpG65RzzOJ%2Beck0Lz9%2FTcGZwdsxMwb%2Fw7tFnr%2FPJzg4clEbEuA2jvTVZAZo%2B1XXVIn6In1Hfsg0%2FjiIeQnkWPL0joOIPX2qtTnRj9PLBuTiTy80bNbFoimHQiPU3BzjbZ65OfsRFEuEBmeKDdvT7wMR1p4mn01HVjy9UkxOwirQzfTqe5hJ%2F7i%2BwbkmxNF3WQ0eDq52kvO2HQ2MmnaCNGC7RSlrUz%2BEl30v5eXOj9EWWAA2qdFzTWuYQGRmYnuDiA6fa77AH3DPLCjdTLepv%2FG45rBBzsJ0l47LhPIb5XpmeDL70htcQ63XQlOK4%2FF3Q%2BkdBcxZ0tsq%2Fa71EGfyjE%2FeWsKd2%2FqGCHsCpSstRSLXp3VQCcjMe1MUdpgr2D9ALm%2FZKZfppogpFurEROkrvwX6tcnRYdRKUBCOD5r9y1vw%2B58HsbRA3h%2B2VLRPXJHr5iHN05FZF7%2BS6rLRvDbhHAxrmnem%2FkpH%2FMu%2FshMOSEndUGOqUBv%2FkCxGlTOn3%2FGxZJzzYzBV9A53ozlnWO1LOQoGUyJrMi340OV8Ryn86ysMU5wzvZPteLAHfswfUntPVIFQDJseNVUqqnn%2FqsEh81EXzxg1jLrhoX8pq%2FiwOQDEQqR13dkNlrW8n1kX21slj%2BTV0f%2FxgY54UZDRzW1GBZZLZOtoR9cwaKwMcw1x5bPXEvjzsIuhyEP9jHFdW%2Bz9QJ1FmdL1ls%2BPYY&X-Amz-Signature=4b89ead904ee2b22dfb46e11bb290c2db9d9af5f9eeed00dcfdfb71b4f335acd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXWBT4CY%2F20260914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260914T021925Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIQCaCQjwKjGXwINBIPrqCliqiqgez%2Fo4cJynX2Fin50XSgIgchWlbLXxv5e1lf4LxwT3rGUeczzwSZTYwA2xvI0S7CMqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMBmieBOxh2kbwEInSrcA70zbZdMc5fjT8q6MAxJN97kR2gmVgiGzfnDmCVU6McGcCyobDRUzPyIy68Nzm9PoFg8BaLasyZmumBvkRPJjGnSeSGzAw5ocPoUahFuJxA%2Bzq%2FitahsTmxzxllQiwBTQaQNundSuQtTkpC2scY9yr4DpG65RzzOJ%2Beck0Lz9%2FTcGZwdsxMwb%2Fw7tFnr%2FPJzg4clEbEuA2jvTVZAZo%2B1XXVIn6In1Hfsg0%2FjiIeQnkWPL0joOIPX2qtTnRj9PLBuTiTy80bNbFoimHQiPU3BzjbZ65OfsRFEuEBmeKDdvT7wMR1p4mn01HVjy9UkxOwirQzfTqe5hJ%2F7i%2BwbkmxNF3WQ0eDq52kvO2HQ2MmnaCNGC7RSlrUz%2BEl30v5eXOj9EWWAA2qdFzTWuYQGRmYnuDiA6fa77AH3DPLCjdTLepv%2FG45rBBzsJ0l47LhPIb5XpmeDL70htcQ63XQlOK4%2FF3Q%2BkdBcxZ0tsq%2Fa71EGfyjE%2FeWsKd2%2FqGCHsCpSstRSLXp3VQCcjMe1MUdpgr2D9ALm%2FZKZfppogpFurEROkrvwX6tcnRYdRKUBCOD5r9y1vw%2B58HsbRA3h%2B2VLRPXJHr5iHN05FZF7%2BS6rLRvDbhHAxrmnem%2FkpH%2FMu%2FshMOSEndUGOqUBv%2FkCxGlTOn3%2FGxZJzzYzBV9A53ozlnWO1LOQoGUyJrMi340OV8Ryn86ysMU5wzvZPteLAHfswfUntPVIFQDJseNVUqqnn%2FqsEh81EXzxg1jLrhoX8pq%2FiwOQDEQqR13dkNlrW8n1kX21slj%2BTV0f%2FxgY54UZDRzW1GBZZLZOtoR9cwaKwMcw1x5bPXEvjzsIuhyEP9jHFdW%2Bz9QJ1FmdL1ls%2BPYY&X-Amz-Signature=26ef7f9a2348f6bf00f6104d6abf4c142366c17a3523e3c01af05b03b1e37a1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXWBT4CY%2F20260914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260914T021925Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIQCaCQjwKjGXwINBIPrqCliqiqgez%2Fo4cJynX2Fin50XSgIgchWlbLXxv5e1lf4LxwT3rGUeczzwSZTYwA2xvI0S7CMqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMBmieBOxh2kbwEInSrcA70zbZdMc5fjT8q6MAxJN97kR2gmVgiGzfnDmCVU6McGcCyobDRUzPyIy68Nzm9PoFg8BaLasyZmumBvkRPJjGnSeSGzAw5ocPoUahFuJxA%2Bzq%2FitahsTmxzxllQiwBTQaQNundSuQtTkpC2scY9yr4DpG65RzzOJ%2Beck0Lz9%2FTcGZwdsxMwb%2Fw7tFnr%2FPJzg4clEbEuA2jvTVZAZo%2B1XXVIn6In1Hfsg0%2FjiIeQnkWPL0joOIPX2qtTnRj9PLBuTiTy80bNbFoimHQiPU3BzjbZ65OfsRFEuEBmeKDdvT7wMR1p4mn01HVjy9UkxOwirQzfTqe5hJ%2F7i%2BwbkmxNF3WQ0eDq52kvO2HQ2MmnaCNGC7RSlrUz%2BEl30v5eXOj9EWWAA2qdFzTWuYQGRmYnuDiA6fa77AH3DPLCjdTLepv%2FG45rBBzsJ0l47LhPIb5XpmeDL70htcQ63XQlOK4%2FF3Q%2BkdBcxZ0tsq%2Fa71EGfyjE%2FeWsKd2%2FqGCHsCpSstRSLXp3VQCcjMe1MUdpgr2D9ALm%2FZKZfppogpFurEROkrvwX6tcnRYdRKUBCOD5r9y1vw%2B58HsbRA3h%2B2VLRPXJHr5iHN05FZF7%2BS6rLRvDbhHAxrmnem%2FkpH%2FMu%2FshMOSEndUGOqUBv%2FkCxGlTOn3%2FGxZJzzYzBV9A53ozlnWO1LOQoGUyJrMi340OV8Ryn86ysMU5wzvZPteLAHfswfUntPVIFQDJseNVUqqnn%2FqsEh81EXzxg1jLrhoX8pq%2FiwOQDEQqR13dkNlrW8n1kX21slj%2BTV0f%2FxgY54UZDRzW1GBZZLZOtoR9cwaKwMcw1x5bPXEvjzsIuhyEP9jHFdW%2Bz9QJ1FmdL1ls%2BPYY&X-Amz-Signature=e8b41419866a234bafd44531b00397861e35a71e427e4b2c5f2d9f745ae4a61a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXWBT4CY%2F20260914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260914T021925Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIQCaCQjwKjGXwINBIPrqCliqiqgez%2Fo4cJynX2Fin50XSgIgchWlbLXxv5e1lf4LxwT3rGUeczzwSZTYwA2xvI0S7CMqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMBmieBOxh2kbwEInSrcA70zbZdMc5fjT8q6MAxJN97kR2gmVgiGzfnDmCVU6McGcCyobDRUzPyIy68Nzm9PoFg8BaLasyZmumBvkRPJjGnSeSGzAw5ocPoUahFuJxA%2Bzq%2FitahsTmxzxllQiwBTQaQNundSuQtTkpC2scY9yr4DpG65RzzOJ%2Beck0Lz9%2FTcGZwdsxMwb%2Fw7tFnr%2FPJzg4clEbEuA2jvTVZAZo%2B1XXVIn6In1Hfsg0%2FjiIeQnkWPL0joOIPX2qtTnRj9PLBuTiTy80bNbFoimHQiPU3BzjbZ65OfsRFEuEBmeKDdvT7wMR1p4mn01HVjy9UkxOwirQzfTqe5hJ%2F7i%2BwbkmxNF3WQ0eDq52kvO2HQ2MmnaCNGC7RSlrUz%2BEl30v5eXOj9EWWAA2qdFzTWuYQGRmYnuDiA6fa77AH3DPLCjdTLepv%2FG45rBBzsJ0l47LhPIb5XpmeDL70htcQ63XQlOK4%2FF3Q%2BkdBcxZ0tsq%2Fa71EGfyjE%2FeWsKd2%2FqGCHsCpSstRSLXp3VQCcjMe1MUdpgr2D9ALm%2FZKZfppogpFurEROkrvwX6tcnRYdRKUBCOD5r9y1vw%2B58HsbRA3h%2B2VLRPXJHr5iHN05FZF7%2BS6rLRvDbhHAxrmnem%2FkpH%2FMu%2FshMOSEndUGOqUBv%2FkCxGlTOn3%2FGxZJzzYzBV9A53ozlnWO1LOQoGUyJrMi340OV8Ryn86ysMU5wzvZPteLAHfswfUntPVIFQDJseNVUqqnn%2FqsEh81EXzxg1jLrhoX8pq%2FiwOQDEQqR13dkNlrW8n1kX21slj%2BTV0f%2FxgY54UZDRzW1GBZZLZOtoR9cwaKwMcw1x5bPXEvjzsIuhyEP9jHFdW%2Bz9QJ1FmdL1ls%2BPYY&X-Amz-Signature=177ee18704291328cfd51080f11357fa33365b184f3e2b5f5db6a3e33538e71b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXWBT4CY%2F20260914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260914T021926Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIQCaCQjwKjGXwINBIPrqCliqiqgez%2Fo4cJynX2Fin50XSgIgchWlbLXxv5e1lf4LxwT3rGUeczzwSZTYwA2xvI0S7CMqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMBmieBOxh2kbwEInSrcA70zbZdMc5fjT8q6MAxJN97kR2gmVgiGzfnDmCVU6McGcCyobDRUzPyIy68Nzm9PoFg8BaLasyZmumBvkRPJjGnSeSGzAw5ocPoUahFuJxA%2Bzq%2FitahsTmxzxllQiwBTQaQNundSuQtTkpC2scY9yr4DpG65RzzOJ%2Beck0Lz9%2FTcGZwdsxMwb%2Fw7tFnr%2FPJzg4clEbEuA2jvTVZAZo%2B1XXVIn6In1Hfsg0%2FjiIeQnkWPL0joOIPX2qtTnRj9PLBuTiTy80bNbFoimHQiPU3BzjbZ65OfsRFEuEBmeKDdvT7wMR1p4mn01HVjy9UkxOwirQzfTqe5hJ%2F7i%2BwbkmxNF3WQ0eDq52kvO2HQ2MmnaCNGC7RSlrUz%2BEl30v5eXOj9EWWAA2qdFzTWuYQGRmYnuDiA6fa77AH3DPLCjdTLepv%2FG45rBBzsJ0l47LhPIb5XpmeDL70htcQ63XQlOK4%2FF3Q%2BkdBcxZ0tsq%2Fa71EGfyjE%2FeWsKd2%2FqGCHsCpSstRSLXp3VQCcjMe1MUdpgr2D9ALm%2FZKZfppogpFurEROkrvwX6tcnRYdRKUBCOD5r9y1vw%2B58HsbRA3h%2B2VLRPXJHr5iHN05FZF7%2BS6rLRvDbhHAxrmnem%2FkpH%2FMu%2FshMOSEndUGOqUBv%2FkCxGlTOn3%2FGxZJzzYzBV9A53ozlnWO1LOQoGUyJrMi340OV8Ryn86ysMU5wzvZPteLAHfswfUntPVIFQDJseNVUqqnn%2FqsEh81EXzxg1jLrhoX8pq%2FiwOQDEQqR13dkNlrW8n1kX21slj%2BTV0f%2FxgY54UZDRzW1GBZZLZOtoR9cwaKwMcw1x5bPXEvjzsIuhyEP9jHFdW%2Bz9QJ1FmdL1ls%2BPYY&X-Amz-Signature=d795ca538cf6f284094c602cf4e018f96d3ddf351a74622df348b42805eaab5a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
