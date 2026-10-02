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
fetched_at: '2026-10-02T03:06:17.477Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DHL5UML%2F20261002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261002T030612Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEQgkciZJxOvZotWLFlpLzLiWxtbDi14JBuewBF5Et34AiBhOMmWJ7TRjbfEh3iyFcRyUAt4oq6xVhlKdoV1P0qsLCqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2B3GNnuGX4Lk10BGMKtwDpxZUkVBoJqCGVOsXm1tSKlwmwbQKZXjR%2BIQ2gC8CH81jVz9ojc5WQKKKL14lD87HjR44DX0dnW16lzwxZIFpvrddtDDTl1rXuESW5Q5XsL8Fo21SoeP%2BukBH49dO5CI61NXG1PP8s%2FoYaI7qmTuSpfxNs0Ion21gEl358%2B6q727582NTto7T0LauMWeuekHmwz1%2B9UIFFGDEiUrTZkb38A3xNo0ywhNw%2FWW7wp5MCvgsN7ctySu7EIPtLnz%2FNdz3ECo6a%2FrPJtLUVfs4cKzIiBmnADHNlZiGeWfb3O3FdqKVcQqAbmU9WvM0oFoe11rYI8HSkEq0Ze2nG34JAxe2b484tki68Sy5Wam%2Fn5Gaz0QA1%2FErzqRJ5W9k9Xh76CfRTOIJAyISL58zgDiK0jRApIzfibvkL9zWxUP2Qf%2FwPwso7QoirvD05zQrP74rEBXE4sSG0y1jjUwXlWJImeZhoTTKwuXD2zRnHYSPUdFlOneeDUIltsDbwUaBKahN0COkDF9VOpieR2G%2F6DCw3wGLuf7UHoi8UFNxFiMXsPCqxjvASkZaiUEUHtGTBkRxiVhDJGNgijL%2FlhWYtWbO%2FBLLVnxLedgfWhKZ4QnEkorka0EOnBDeySg7iC4OVBAw4Zb81QY6pgGal%2F5FM%2BEHb1OJLr4Djzey6AIMrrXQdfY9MWqm5d3FmR2T9DFHs2%2BTcRjkasC1ntzdKokEmwz0vQWO2zW4E8k4y7EA%2B82MibOhwhrpyfLM4X4K%2FnspvGhPrheIXKE%2FAk9h6P2k9oIu1XRqwhKPG0Jy44tkmSTZSQaP3kUpsNDMaD4xVqFNyGJOK9EDWnMEx4DIZ4FwxIKlNit9egIQb11HddjibJMN&X-Amz-Signature=3c1740d06745cd18b7264ac5e4b71d3e310e54b4709162fbfa821b26fd1d9c10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DHL5UML%2F20261002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261002T030612Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEQgkciZJxOvZotWLFlpLzLiWxtbDi14JBuewBF5Et34AiBhOMmWJ7TRjbfEh3iyFcRyUAt4oq6xVhlKdoV1P0qsLCqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2B3GNnuGX4Lk10BGMKtwDpxZUkVBoJqCGVOsXm1tSKlwmwbQKZXjR%2BIQ2gC8CH81jVz9ojc5WQKKKL14lD87HjR44DX0dnW16lzwxZIFpvrddtDDTl1rXuESW5Q5XsL8Fo21SoeP%2BukBH49dO5CI61NXG1PP8s%2FoYaI7qmTuSpfxNs0Ion21gEl358%2B6q727582NTto7T0LauMWeuekHmwz1%2B9UIFFGDEiUrTZkb38A3xNo0ywhNw%2FWW7wp5MCvgsN7ctySu7EIPtLnz%2FNdz3ECo6a%2FrPJtLUVfs4cKzIiBmnADHNlZiGeWfb3O3FdqKVcQqAbmU9WvM0oFoe11rYI8HSkEq0Ze2nG34JAxe2b484tki68Sy5Wam%2Fn5Gaz0QA1%2FErzqRJ5W9k9Xh76CfRTOIJAyISL58zgDiK0jRApIzfibvkL9zWxUP2Qf%2FwPwso7QoirvD05zQrP74rEBXE4sSG0y1jjUwXlWJImeZhoTTKwuXD2zRnHYSPUdFlOneeDUIltsDbwUaBKahN0COkDF9VOpieR2G%2F6DCw3wGLuf7UHoi8UFNxFiMXsPCqxjvASkZaiUEUHtGTBkRxiVhDJGNgijL%2FlhWYtWbO%2FBLLVnxLedgfWhKZ4QnEkorka0EOnBDeySg7iC4OVBAw4Zb81QY6pgGal%2F5FM%2BEHb1OJLr4Djzey6AIMrrXQdfY9MWqm5d3FmR2T9DFHs2%2BTcRjkasC1ntzdKokEmwz0vQWO2zW4E8k4y7EA%2B82MibOhwhrpyfLM4X4K%2FnspvGhPrheIXKE%2FAk9h6P2k9oIu1XRqwhKPG0Jy44tkmSTZSQaP3kUpsNDMaD4xVqFNyGJOK9EDWnMEx4DIZ4FwxIKlNit9egIQb11HddjibJMN&X-Amz-Signature=28f0ad6f883e4c809ead4490dba54c1a6efde3191cecc4dc8766a6189f7a65a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DHL5UML%2F20261002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261002T030612Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEQgkciZJxOvZotWLFlpLzLiWxtbDi14JBuewBF5Et34AiBhOMmWJ7TRjbfEh3iyFcRyUAt4oq6xVhlKdoV1P0qsLCqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2B3GNnuGX4Lk10BGMKtwDpxZUkVBoJqCGVOsXm1tSKlwmwbQKZXjR%2BIQ2gC8CH81jVz9ojc5WQKKKL14lD87HjR44DX0dnW16lzwxZIFpvrddtDDTl1rXuESW5Q5XsL8Fo21SoeP%2BukBH49dO5CI61NXG1PP8s%2FoYaI7qmTuSpfxNs0Ion21gEl358%2B6q727582NTto7T0LauMWeuekHmwz1%2B9UIFFGDEiUrTZkb38A3xNo0ywhNw%2FWW7wp5MCvgsN7ctySu7EIPtLnz%2FNdz3ECo6a%2FrPJtLUVfs4cKzIiBmnADHNlZiGeWfb3O3FdqKVcQqAbmU9WvM0oFoe11rYI8HSkEq0Ze2nG34JAxe2b484tki68Sy5Wam%2Fn5Gaz0QA1%2FErzqRJ5W9k9Xh76CfRTOIJAyISL58zgDiK0jRApIzfibvkL9zWxUP2Qf%2FwPwso7QoirvD05zQrP74rEBXE4sSG0y1jjUwXlWJImeZhoTTKwuXD2zRnHYSPUdFlOneeDUIltsDbwUaBKahN0COkDF9VOpieR2G%2F6DCw3wGLuf7UHoi8UFNxFiMXsPCqxjvASkZaiUEUHtGTBkRxiVhDJGNgijL%2FlhWYtWbO%2FBLLVnxLedgfWhKZ4QnEkorka0EOnBDeySg7iC4OVBAw4Zb81QY6pgGal%2F5FM%2BEHb1OJLr4Djzey6AIMrrXQdfY9MWqm5d3FmR2T9DFHs2%2BTcRjkasC1ntzdKokEmwz0vQWO2zW4E8k4y7EA%2B82MibOhwhrpyfLM4X4K%2FnspvGhPrheIXKE%2FAk9h6P2k9oIu1XRqwhKPG0Jy44tkmSTZSQaP3kUpsNDMaD4xVqFNyGJOK9EDWnMEx4DIZ4FwxIKlNit9egIQb11HddjibJMN&X-Amz-Signature=8ac87acee46507e693da3146428950ad9f499c962367d6cb8de6bfd490a40fc8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DHL5UML%2F20261002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261002T030612Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEQgkciZJxOvZotWLFlpLzLiWxtbDi14JBuewBF5Et34AiBhOMmWJ7TRjbfEh3iyFcRyUAt4oq6xVhlKdoV1P0qsLCqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2B3GNnuGX4Lk10BGMKtwDpxZUkVBoJqCGVOsXm1tSKlwmwbQKZXjR%2BIQ2gC8CH81jVz9ojc5WQKKKL14lD87HjR44DX0dnW16lzwxZIFpvrddtDDTl1rXuESW5Q5XsL8Fo21SoeP%2BukBH49dO5CI61NXG1PP8s%2FoYaI7qmTuSpfxNs0Ion21gEl358%2B6q727582NTto7T0LauMWeuekHmwz1%2B9UIFFGDEiUrTZkb38A3xNo0ywhNw%2FWW7wp5MCvgsN7ctySu7EIPtLnz%2FNdz3ECo6a%2FrPJtLUVfs4cKzIiBmnADHNlZiGeWfb3O3FdqKVcQqAbmU9WvM0oFoe11rYI8HSkEq0Ze2nG34JAxe2b484tki68Sy5Wam%2Fn5Gaz0QA1%2FErzqRJ5W9k9Xh76CfRTOIJAyISL58zgDiK0jRApIzfibvkL9zWxUP2Qf%2FwPwso7QoirvD05zQrP74rEBXE4sSG0y1jjUwXlWJImeZhoTTKwuXD2zRnHYSPUdFlOneeDUIltsDbwUaBKahN0COkDF9VOpieR2G%2F6DCw3wGLuf7UHoi8UFNxFiMXsPCqxjvASkZaiUEUHtGTBkRxiVhDJGNgijL%2FlhWYtWbO%2FBLLVnxLedgfWhKZ4QnEkorka0EOnBDeySg7iC4OVBAw4Zb81QY6pgGal%2F5FM%2BEHb1OJLr4Djzey6AIMrrXQdfY9MWqm5d3FmR2T9DFHs2%2BTcRjkasC1ntzdKokEmwz0vQWO2zW4E8k4y7EA%2B82MibOhwhrpyfLM4X4K%2FnspvGhPrheIXKE%2FAk9h6P2k9oIu1XRqwhKPG0Jy44tkmSTZSQaP3kUpsNDMaD4xVqFNyGJOK9EDWnMEx4DIZ4FwxIKlNit9egIQb11HddjibJMN&X-Amz-Signature=8bd46f249c7837351abfeb72b1381541f64a0ee3618149892c63089452f787d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DHL5UML%2F20261002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261002T030612Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEQgkciZJxOvZotWLFlpLzLiWxtbDi14JBuewBF5Et34AiBhOMmWJ7TRjbfEh3iyFcRyUAt4oq6xVhlKdoV1P0qsLCqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2B3GNnuGX4Lk10BGMKtwDpxZUkVBoJqCGVOsXm1tSKlwmwbQKZXjR%2BIQ2gC8CH81jVz9ojc5WQKKKL14lD87HjR44DX0dnW16lzwxZIFpvrddtDDTl1rXuESW5Q5XsL8Fo21SoeP%2BukBH49dO5CI61NXG1PP8s%2FoYaI7qmTuSpfxNs0Ion21gEl358%2B6q727582NTto7T0LauMWeuekHmwz1%2B9UIFFGDEiUrTZkb38A3xNo0ywhNw%2FWW7wp5MCvgsN7ctySu7EIPtLnz%2FNdz3ECo6a%2FrPJtLUVfs4cKzIiBmnADHNlZiGeWfb3O3FdqKVcQqAbmU9WvM0oFoe11rYI8HSkEq0Ze2nG34JAxe2b484tki68Sy5Wam%2Fn5Gaz0QA1%2FErzqRJ5W9k9Xh76CfRTOIJAyISL58zgDiK0jRApIzfibvkL9zWxUP2Qf%2FwPwso7QoirvD05zQrP74rEBXE4sSG0y1jjUwXlWJImeZhoTTKwuXD2zRnHYSPUdFlOneeDUIltsDbwUaBKahN0COkDF9VOpieR2G%2F6DCw3wGLuf7UHoi8UFNxFiMXsPCqxjvASkZaiUEUHtGTBkRxiVhDJGNgijL%2FlhWYtWbO%2FBLLVnxLedgfWhKZ4QnEkorka0EOnBDeySg7iC4OVBAw4Zb81QY6pgGal%2F5FM%2BEHb1OJLr4Djzey6AIMrrXQdfY9MWqm5d3FmR2T9DFHs2%2BTcRjkasC1ntzdKokEmwz0vQWO2zW4E8k4y7EA%2B82MibOhwhrpyfLM4X4K%2FnspvGhPrheIXKE%2FAk9h6P2k9oIu1XRqwhKPG0Jy44tkmSTZSQaP3kUpsNDMaD4xVqFNyGJOK9EDWnMEx4DIZ4FwxIKlNit9egIQb11HddjibJMN&X-Amz-Signature=3f7852216f3e62139eeec540b2273d835762cac1e750c6e37eac0492f6bdd1b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DHL5UML%2F20261002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261002T030612Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEQgkciZJxOvZotWLFlpLzLiWxtbDi14JBuewBF5Et34AiBhOMmWJ7TRjbfEh3iyFcRyUAt4oq6xVhlKdoV1P0qsLCqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2B3GNnuGX4Lk10BGMKtwDpxZUkVBoJqCGVOsXm1tSKlwmwbQKZXjR%2BIQ2gC8CH81jVz9ojc5WQKKKL14lD87HjR44DX0dnW16lzwxZIFpvrddtDDTl1rXuESW5Q5XsL8Fo21SoeP%2BukBH49dO5CI61NXG1PP8s%2FoYaI7qmTuSpfxNs0Ion21gEl358%2B6q727582NTto7T0LauMWeuekHmwz1%2B9UIFFGDEiUrTZkb38A3xNo0ywhNw%2FWW7wp5MCvgsN7ctySu7EIPtLnz%2FNdz3ECo6a%2FrPJtLUVfs4cKzIiBmnADHNlZiGeWfb3O3FdqKVcQqAbmU9WvM0oFoe11rYI8HSkEq0Ze2nG34JAxe2b484tki68Sy5Wam%2Fn5Gaz0QA1%2FErzqRJ5W9k9Xh76CfRTOIJAyISL58zgDiK0jRApIzfibvkL9zWxUP2Qf%2FwPwso7QoirvD05zQrP74rEBXE4sSG0y1jjUwXlWJImeZhoTTKwuXD2zRnHYSPUdFlOneeDUIltsDbwUaBKahN0COkDF9VOpieR2G%2F6DCw3wGLuf7UHoi8UFNxFiMXsPCqxjvASkZaiUEUHtGTBkRxiVhDJGNgijL%2FlhWYtWbO%2FBLLVnxLedgfWhKZ4QnEkorka0EOnBDeySg7iC4OVBAw4Zb81QY6pgGal%2F5FM%2BEHb1OJLr4Djzey6AIMrrXQdfY9MWqm5d3FmR2T9DFHs2%2BTcRjkasC1ntzdKokEmwz0vQWO2zW4E8k4y7EA%2B82MibOhwhrpyfLM4X4K%2FnspvGhPrheIXKE%2FAk9h6P2k9oIu1XRqwhKPG0Jy44tkmSTZSQaP3kUpsNDMaD4xVqFNyGJOK9EDWnMEx4DIZ4FwxIKlNit9egIQb11HddjibJMN&X-Amz-Signature=d9be71ce8811b5163519a60eee0e3f41d6ad36b511459c1c99789e6cc5e04567&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DHL5UML%2F20261002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261002T030612Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEQgkciZJxOvZotWLFlpLzLiWxtbDi14JBuewBF5Et34AiBhOMmWJ7TRjbfEh3iyFcRyUAt4oq6xVhlKdoV1P0qsLCqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2B3GNnuGX4Lk10BGMKtwDpxZUkVBoJqCGVOsXm1tSKlwmwbQKZXjR%2BIQ2gC8CH81jVz9ojc5WQKKKL14lD87HjR44DX0dnW16lzwxZIFpvrddtDDTl1rXuESW5Q5XsL8Fo21SoeP%2BukBH49dO5CI61NXG1PP8s%2FoYaI7qmTuSpfxNs0Ion21gEl358%2B6q727582NTto7T0LauMWeuekHmwz1%2B9UIFFGDEiUrTZkb38A3xNo0ywhNw%2FWW7wp5MCvgsN7ctySu7EIPtLnz%2FNdz3ECo6a%2FrPJtLUVfs4cKzIiBmnADHNlZiGeWfb3O3FdqKVcQqAbmU9WvM0oFoe11rYI8HSkEq0Ze2nG34JAxe2b484tki68Sy5Wam%2Fn5Gaz0QA1%2FErzqRJ5W9k9Xh76CfRTOIJAyISL58zgDiK0jRApIzfibvkL9zWxUP2Qf%2FwPwso7QoirvD05zQrP74rEBXE4sSG0y1jjUwXlWJImeZhoTTKwuXD2zRnHYSPUdFlOneeDUIltsDbwUaBKahN0COkDF9VOpieR2G%2F6DCw3wGLuf7UHoi8UFNxFiMXsPCqxjvASkZaiUEUHtGTBkRxiVhDJGNgijL%2FlhWYtWbO%2FBLLVnxLedgfWhKZ4QnEkorka0EOnBDeySg7iC4OVBAw4Zb81QY6pgGal%2F5FM%2BEHb1OJLr4Djzey6AIMrrXQdfY9MWqm5d3FmR2T9DFHs2%2BTcRjkasC1ntzdKokEmwz0vQWO2zW4E8k4y7EA%2B82MibOhwhrpyfLM4X4K%2FnspvGhPrheIXKE%2FAk9h6P2k9oIu1XRqwhKPG0Jy44tkmSTZSQaP3kUpsNDMaD4xVqFNyGJOK9EDWnMEx4DIZ4FwxIKlNit9egIQb11HddjibJMN&X-Amz-Signature=2cc9b178dbe4134283c386ff6add29dbd1b2fefdf7cd93dce440bff72c11a921&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DHL5UML%2F20261002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261002T030612Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEQgkciZJxOvZotWLFlpLzLiWxtbDi14JBuewBF5Et34AiBhOMmWJ7TRjbfEh3iyFcRyUAt4oq6xVhlKdoV1P0qsLCqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2B3GNnuGX4Lk10BGMKtwDpxZUkVBoJqCGVOsXm1tSKlwmwbQKZXjR%2BIQ2gC8CH81jVz9ojc5WQKKKL14lD87HjR44DX0dnW16lzwxZIFpvrddtDDTl1rXuESW5Q5XsL8Fo21SoeP%2BukBH49dO5CI61NXG1PP8s%2FoYaI7qmTuSpfxNs0Ion21gEl358%2B6q727582NTto7T0LauMWeuekHmwz1%2B9UIFFGDEiUrTZkb38A3xNo0ywhNw%2FWW7wp5MCvgsN7ctySu7EIPtLnz%2FNdz3ECo6a%2FrPJtLUVfs4cKzIiBmnADHNlZiGeWfb3O3FdqKVcQqAbmU9WvM0oFoe11rYI8HSkEq0Ze2nG34JAxe2b484tki68Sy5Wam%2Fn5Gaz0QA1%2FErzqRJ5W9k9Xh76CfRTOIJAyISL58zgDiK0jRApIzfibvkL9zWxUP2Qf%2FwPwso7QoirvD05zQrP74rEBXE4sSG0y1jjUwXlWJImeZhoTTKwuXD2zRnHYSPUdFlOneeDUIltsDbwUaBKahN0COkDF9VOpieR2G%2F6DCw3wGLuf7UHoi8UFNxFiMXsPCqxjvASkZaiUEUHtGTBkRxiVhDJGNgijL%2FlhWYtWbO%2FBLLVnxLedgfWhKZ4QnEkorka0EOnBDeySg7iC4OVBAw4Zb81QY6pgGal%2F5FM%2BEHb1OJLr4Djzey6AIMrrXQdfY9MWqm5d3FmR2T9DFHs2%2BTcRjkasC1ntzdKokEmwz0vQWO2zW4E8k4y7EA%2B82MibOhwhrpyfLM4X4K%2FnspvGhPrheIXKE%2FAk9h6P2k9oIu1XRqwhKPG0Jy44tkmSTZSQaP3kUpsNDMaD4xVqFNyGJOK9EDWnMEx4DIZ4FwxIKlNit9egIQb11HddjibJMN&X-Amz-Signature=de353993781839be826a4dc73043d363d1d778f0a6c3f8272f7217effdc09406&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DHL5UML%2F20261002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261002T030612Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEQgkciZJxOvZotWLFlpLzLiWxtbDi14JBuewBF5Et34AiBhOMmWJ7TRjbfEh3iyFcRyUAt4oq6xVhlKdoV1P0qsLCqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2B3GNnuGX4Lk10BGMKtwDpxZUkVBoJqCGVOsXm1tSKlwmwbQKZXjR%2BIQ2gC8CH81jVz9ojc5WQKKKL14lD87HjR44DX0dnW16lzwxZIFpvrddtDDTl1rXuESW5Q5XsL8Fo21SoeP%2BukBH49dO5CI61NXG1PP8s%2FoYaI7qmTuSpfxNs0Ion21gEl358%2B6q727582NTto7T0LauMWeuekHmwz1%2B9UIFFGDEiUrTZkb38A3xNo0ywhNw%2FWW7wp5MCvgsN7ctySu7EIPtLnz%2FNdz3ECo6a%2FrPJtLUVfs4cKzIiBmnADHNlZiGeWfb3O3FdqKVcQqAbmU9WvM0oFoe11rYI8HSkEq0Ze2nG34JAxe2b484tki68Sy5Wam%2Fn5Gaz0QA1%2FErzqRJ5W9k9Xh76CfRTOIJAyISL58zgDiK0jRApIzfibvkL9zWxUP2Qf%2FwPwso7QoirvD05zQrP74rEBXE4sSG0y1jjUwXlWJImeZhoTTKwuXD2zRnHYSPUdFlOneeDUIltsDbwUaBKahN0COkDF9VOpieR2G%2F6DCw3wGLuf7UHoi8UFNxFiMXsPCqxjvASkZaiUEUHtGTBkRxiVhDJGNgijL%2FlhWYtWbO%2FBLLVnxLedgfWhKZ4QnEkorka0EOnBDeySg7iC4OVBAw4Zb81QY6pgGal%2F5FM%2BEHb1OJLr4Djzey6AIMrrXQdfY9MWqm5d3FmR2T9DFHs2%2BTcRjkasC1ntzdKokEmwz0vQWO2zW4E8k4y7EA%2B82MibOhwhrpyfLM4X4K%2FnspvGhPrheIXKE%2FAk9h6P2k9oIu1XRqwhKPG0Jy44tkmSTZSQaP3kUpsNDMaD4xVqFNyGJOK9EDWnMEx4DIZ4FwxIKlNit9egIQb11HddjibJMN&X-Amz-Signature=6a83a5f5f9b98bdb6a62c650dc13c995ebc154add49d58eb8cedba779ac305f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
