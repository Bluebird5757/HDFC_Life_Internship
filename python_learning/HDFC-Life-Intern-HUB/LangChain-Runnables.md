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
fetched_at: '2026-11-22T03:59:59.502Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X2R67763%2F20261122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261122T035954Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxVZDvF96GzrM51%2F5RyYPMqf%2FKW%2Fdr1UXiQxJoVAE5wQIhALbDAigSh5Dn1T4kofrCD5rrENgH3q4lS3yda7IpZEItKv8DCFMQABoMNjM3NDIzMTgzODA1IgyJRI6GNc6AZfPUDEwq3AOJdPcNoQmb8pkHv1b6uGiJtwCjm48a2Utw6KadnxlFCVu8afRPxnGYQLyqLeNUw0RFMJjajqHbe7nDwjs07HaJQBz%2B9%2FcEtRKqtQ2lEf3ymF4%2BaR6dIS6Q0IVq1hwYzm7QDU6CVIHvfZouYhpdYblIun8hAoWVqDTPSw3KDo%2F4Ey%2Bqjoz5H86%2FnHjvIPNQRKtPXhEGN0HylrXEKMMR2v7KWQdCs3GKmGUYkWryzOmbCypmpt5QSC32SldyfQqwiZq2fgFpIqdcr1NXhqfY5wsFQvIk1%2BKcJcfLqbFBfNgtcZu5krn3QUNnLsm9%2FsINwVjrqFGjiLHp4%2Fm7pNVTHed3DRyEC%2BNPh7FQsYlWGuD9gLLgukNMUeJ3syNsbej9hYpvvdHqqRIDsDywIejkCXa5%2F7pQMG751v2cfkk9pUywpjGL6hr0bMqZhnL0P5Dzsf7zjZV7ZjIJnG3seldbCNuGQrBAJdxxfYXMshpJn0Tam9hi7fRS3X7BFHlX8NpCedh6Xx%2FHAFnCA2O7KC6Rh5Qb%2Fk%2FnuFrbAPZoibgmn11r%2BsXIV9I%2BjAbtztULX2n1ehksCn4f%2Fk9b%2FKgc6bf46d828Na4fi8ZqTYlq95KuCHQcJzACpPhXjIeHiYtRDChponYBjqkAVSk6lFdzlMexHzSMHaZ7hTOU4bNW5NEeDKej5Q7eY4OL3o8Xv6vLP3iffaV2scEafbD0hL6EfovLMmlDTBKFa607ZH43eNUvSkcBjef7tyu3rsQgbq6R3ABEMRN7cWvpSdhtIa245OQTMZlb7GEDV3CD1NhKADlLcjKFRfSsgHHN9rtUEGcWg6tkK5sVhdx2ANaqBw507hOzBAObiqyDen2sktm&X-Amz-Signature=531aad611de52545f6f221c28a99ffdbcf110c5eaf3c02cb869e18faa9dc48b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X2R67763%2F20261122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261122T035954Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxVZDvF96GzrM51%2F5RyYPMqf%2FKW%2Fdr1UXiQxJoVAE5wQIhALbDAigSh5Dn1T4kofrCD5rrENgH3q4lS3yda7IpZEItKv8DCFMQABoMNjM3NDIzMTgzODA1IgyJRI6GNc6AZfPUDEwq3AOJdPcNoQmb8pkHv1b6uGiJtwCjm48a2Utw6KadnxlFCVu8afRPxnGYQLyqLeNUw0RFMJjajqHbe7nDwjs07HaJQBz%2B9%2FcEtRKqtQ2lEf3ymF4%2BaR6dIS6Q0IVq1hwYzm7QDU6CVIHvfZouYhpdYblIun8hAoWVqDTPSw3KDo%2F4Ey%2Bqjoz5H86%2FnHjvIPNQRKtPXhEGN0HylrXEKMMR2v7KWQdCs3GKmGUYkWryzOmbCypmpt5QSC32SldyfQqwiZq2fgFpIqdcr1NXhqfY5wsFQvIk1%2BKcJcfLqbFBfNgtcZu5krn3QUNnLsm9%2FsINwVjrqFGjiLHp4%2Fm7pNVTHed3DRyEC%2BNPh7FQsYlWGuD9gLLgukNMUeJ3syNsbej9hYpvvdHqqRIDsDywIejkCXa5%2F7pQMG751v2cfkk9pUywpjGL6hr0bMqZhnL0P5Dzsf7zjZV7ZjIJnG3seldbCNuGQrBAJdxxfYXMshpJn0Tam9hi7fRS3X7BFHlX8NpCedh6Xx%2FHAFnCA2O7KC6Rh5Qb%2Fk%2FnuFrbAPZoibgmn11r%2BsXIV9I%2BjAbtztULX2n1ehksCn4f%2Fk9b%2FKgc6bf46d828Na4fi8ZqTYlq95KuCHQcJzACpPhXjIeHiYtRDChponYBjqkAVSk6lFdzlMexHzSMHaZ7hTOU4bNW5NEeDKej5Q7eY4OL3o8Xv6vLP3iffaV2scEafbD0hL6EfovLMmlDTBKFa607ZH43eNUvSkcBjef7tyu3rsQgbq6R3ABEMRN7cWvpSdhtIa245OQTMZlb7GEDV3CD1NhKADlLcjKFRfSsgHHN9rtUEGcWg6tkK5sVhdx2ANaqBw507hOzBAObiqyDen2sktm&X-Amz-Signature=765460c6f8f63e99b3171abf4834538d5c0dd7d28379203abe78573212b4195c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X2R67763%2F20261122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261122T035954Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxVZDvF96GzrM51%2F5RyYPMqf%2FKW%2Fdr1UXiQxJoVAE5wQIhALbDAigSh5Dn1T4kofrCD5rrENgH3q4lS3yda7IpZEItKv8DCFMQABoMNjM3NDIzMTgzODA1IgyJRI6GNc6AZfPUDEwq3AOJdPcNoQmb8pkHv1b6uGiJtwCjm48a2Utw6KadnxlFCVu8afRPxnGYQLyqLeNUw0RFMJjajqHbe7nDwjs07HaJQBz%2B9%2FcEtRKqtQ2lEf3ymF4%2BaR6dIS6Q0IVq1hwYzm7QDU6CVIHvfZouYhpdYblIun8hAoWVqDTPSw3KDo%2F4Ey%2Bqjoz5H86%2FnHjvIPNQRKtPXhEGN0HylrXEKMMR2v7KWQdCs3GKmGUYkWryzOmbCypmpt5QSC32SldyfQqwiZq2fgFpIqdcr1NXhqfY5wsFQvIk1%2BKcJcfLqbFBfNgtcZu5krn3QUNnLsm9%2FsINwVjrqFGjiLHp4%2Fm7pNVTHed3DRyEC%2BNPh7FQsYlWGuD9gLLgukNMUeJ3syNsbej9hYpvvdHqqRIDsDywIejkCXa5%2F7pQMG751v2cfkk9pUywpjGL6hr0bMqZhnL0P5Dzsf7zjZV7ZjIJnG3seldbCNuGQrBAJdxxfYXMshpJn0Tam9hi7fRS3X7BFHlX8NpCedh6Xx%2FHAFnCA2O7KC6Rh5Qb%2Fk%2FnuFrbAPZoibgmn11r%2BsXIV9I%2BjAbtztULX2n1ehksCn4f%2Fk9b%2FKgc6bf46d828Na4fi8ZqTYlq95KuCHQcJzACpPhXjIeHiYtRDChponYBjqkAVSk6lFdzlMexHzSMHaZ7hTOU4bNW5NEeDKej5Q7eY4OL3o8Xv6vLP3iffaV2scEafbD0hL6EfovLMmlDTBKFa607ZH43eNUvSkcBjef7tyu3rsQgbq6R3ABEMRN7cWvpSdhtIa245OQTMZlb7GEDV3CD1NhKADlLcjKFRfSsgHHN9rtUEGcWg6tkK5sVhdx2ANaqBw507hOzBAObiqyDen2sktm&X-Amz-Signature=24e8bfb57bbf43f77fdd8da01772b73711852c342684095dc2e57f3819ce375e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X2R67763%2F20261122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261122T035954Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxVZDvF96GzrM51%2F5RyYPMqf%2FKW%2Fdr1UXiQxJoVAE5wQIhALbDAigSh5Dn1T4kofrCD5rrENgH3q4lS3yda7IpZEItKv8DCFMQABoMNjM3NDIzMTgzODA1IgyJRI6GNc6AZfPUDEwq3AOJdPcNoQmb8pkHv1b6uGiJtwCjm48a2Utw6KadnxlFCVu8afRPxnGYQLyqLeNUw0RFMJjajqHbe7nDwjs07HaJQBz%2B9%2FcEtRKqtQ2lEf3ymF4%2BaR6dIS6Q0IVq1hwYzm7QDU6CVIHvfZouYhpdYblIun8hAoWVqDTPSw3KDo%2F4Ey%2Bqjoz5H86%2FnHjvIPNQRKtPXhEGN0HylrXEKMMR2v7KWQdCs3GKmGUYkWryzOmbCypmpt5QSC32SldyfQqwiZq2fgFpIqdcr1NXhqfY5wsFQvIk1%2BKcJcfLqbFBfNgtcZu5krn3QUNnLsm9%2FsINwVjrqFGjiLHp4%2Fm7pNVTHed3DRyEC%2BNPh7FQsYlWGuD9gLLgukNMUeJ3syNsbej9hYpvvdHqqRIDsDywIejkCXa5%2F7pQMG751v2cfkk9pUywpjGL6hr0bMqZhnL0P5Dzsf7zjZV7ZjIJnG3seldbCNuGQrBAJdxxfYXMshpJn0Tam9hi7fRS3X7BFHlX8NpCedh6Xx%2FHAFnCA2O7KC6Rh5Qb%2Fk%2FnuFrbAPZoibgmn11r%2BsXIV9I%2BjAbtztULX2n1ehksCn4f%2Fk9b%2FKgc6bf46d828Na4fi8ZqTYlq95KuCHQcJzACpPhXjIeHiYtRDChponYBjqkAVSk6lFdzlMexHzSMHaZ7hTOU4bNW5NEeDKej5Q7eY4OL3o8Xv6vLP3iffaV2scEafbD0hL6EfovLMmlDTBKFa607ZH43eNUvSkcBjef7tyu3rsQgbq6R3ABEMRN7cWvpSdhtIa245OQTMZlb7GEDV3CD1NhKADlLcjKFRfSsgHHN9rtUEGcWg6tkK5sVhdx2ANaqBw507hOzBAObiqyDen2sktm&X-Amz-Signature=3e61602ff8168e2e9791751a9fd6d2a95d5fbdfd6eef77767336ab972ae5ba54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X2R67763%2F20261122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261122T035954Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxVZDvF96GzrM51%2F5RyYPMqf%2FKW%2Fdr1UXiQxJoVAE5wQIhALbDAigSh5Dn1T4kofrCD5rrENgH3q4lS3yda7IpZEItKv8DCFMQABoMNjM3NDIzMTgzODA1IgyJRI6GNc6AZfPUDEwq3AOJdPcNoQmb8pkHv1b6uGiJtwCjm48a2Utw6KadnxlFCVu8afRPxnGYQLyqLeNUw0RFMJjajqHbe7nDwjs07HaJQBz%2B9%2FcEtRKqtQ2lEf3ymF4%2BaR6dIS6Q0IVq1hwYzm7QDU6CVIHvfZouYhpdYblIun8hAoWVqDTPSw3KDo%2F4Ey%2Bqjoz5H86%2FnHjvIPNQRKtPXhEGN0HylrXEKMMR2v7KWQdCs3GKmGUYkWryzOmbCypmpt5QSC32SldyfQqwiZq2fgFpIqdcr1NXhqfY5wsFQvIk1%2BKcJcfLqbFBfNgtcZu5krn3QUNnLsm9%2FsINwVjrqFGjiLHp4%2Fm7pNVTHed3DRyEC%2BNPh7FQsYlWGuD9gLLgukNMUeJ3syNsbej9hYpvvdHqqRIDsDywIejkCXa5%2F7pQMG751v2cfkk9pUywpjGL6hr0bMqZhnL0P5Dzsf7zjZV7ZjIJnG3seldbCNuGQrBAJdxxfYXMshpJn0Tam9hi7fRS3X7BFHlX8NpCedh6Xx%2FHAFnCA2O7KC6Rh5Qb%2Fk%2FnuFrbAPZoibgmn11r%2BsXIV9I%2BjAbtztULX2n1ehksCn4f%2Fk9b%2FKgc6bf46d828Na4fi8ZqTYlq95KuCHQcJzACpPhXjIeHiYtRDChponYBjqkAVSk6lFdzlMexHzSMHaZ7hTOU4bNW5NEeDKej5Q7eY4OL3o8Xv6vLP3iffaV2scEafbD0hL6EfovLMmlDTBKFa607ZH43eNUvSkcBjef7tyu3rsQgbq6R3ABEMRN7cWvpSdhtIa245OQTMZlb7GEDV3CD1NhKADlLcjKFRfSsgHHN9rtUEGcWg6tkK5sVhdx2ANaqBw507hOzBAObiqyDen2sktm&X-Amz-Signature=1b92b63e0ab21129a9d8db714e0bfb085ceafc7629fee2f6da25d917316eb13c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X2R67763%2F20261122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261122T035954Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxVZDvF96GzrM51%2F5RyYPMqf%2FKW%2Fdr1UXiQxJoVAE5wQIhALbDAigSh5Dn1T4kofrCD5rrENgH3q4lS3yda7IpZEItKv8DCFMQABoMNjM3NDIzMTgzODA1IgyJRI6GNc6AZfPUDEwq3AOJdPcNoQmb8pkHv1b6uGiJtwCjm48a2Utw6KadnxlFCVu8afRPxnGYQLyqLeNUw0RFMJjajqHbe7nDwjs07HaJQBz%2B9%2FcEtRKqtQ2lEf3ymF4%2BaR6dIS6Q0IVq1hwYzm7QDU6CVIHvfZouYhpdYblIun8hAoWVqDTPSw3KDo%2F4Ey%2Bqjoz5H86%2FnHjvIPNQRKtPXhEGN0HylrXEKMMR2v7KWQdCs3GKmGUYkWryzOmbCypmpt5QSC32SldyfQqwiZq2fgFpIqdcr1NXhqfY5wsFQvIk1%2BKcJcfLqbFBfNgtcZu5krn3QUNnLsm9%2FsINwVjrqFGjiLHp4%2Fm7pNVTHed3DRyEC%2BNPh7FQsYlWGuD9gLLgukNMUeJ3syNsbej9hYpvvdHqqRIDsDywIejkCXa5%2F7pQMG751v2cfkk9pUywpjGL6hr0bMqZhnL0P5Dzsf7zjZV7ZjIJnG3seldbCNuGQrBAJdxxfYXMshpJn0Tam9hi7fRS3X7BFHlX8NpCedh6Xx%2FHAFnCA2O7KC6Rh5Qb%2Fk%2FnuFrbAPZoibgmn11r%2BsXIV9I%2BjAbtztULX2n1ehksCn4f%2Fk9b%2FKgc6bf46d828Na4fi8ZqTYlq95KuCHQcJzACpPhXjIeHiYtRDChponYBjqkAVSk6lFdzlMexHzSMHaZ7hTOU4bNW5NEeDKej5Q7eY4OL3o8Xv6vLP3iffaV2scEafbD0hL6EfovLMmlDTBKFa607ZH43eNUvSkcBjef7tyu3rsQgbq6R3ABEMRN7cWvpSdhtIa245OQTMZlb7GEDV3CD1NhKADlLcjKFRfSsgHHN9rtUEGcWg6tkK5sVhdx2ANaqBw507hOzBAObiqyDen2sktm&X-Amz-Signature=78b43b2bf27867d3e6293248c96e1f97705838fa93651deba9471555b55d8bfb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X2R67763%2F20261122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261122T035954Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxVZDvF96GzrM51%2F5RyYPMqf%2FKW%2Fdr1UXiQxJoVAE5wQIhALbDAigSh5Dn1T4kofrCD5rrENgH3q4lS3yda7IpZEItKv8DCFMQABoMNjM3NDIzMTgzODA1IgyJRI6GNc6AZfPUDEwq3AOJdPcNoQmb8pkHv1b6uGiJtwCjm48a2Utw6KadnxlFCVu8afRPxnGYQLyqLeNUw0RFMJjajqHbe7nDwjs07HaJQBz%2B9%2FcEtRKqtQ2lEf3ymF4%2BaR6dIS6Q0IVq1hwYzm7QDU6CVIHvfZouYhpdYblIun8hAoWVqDTPSw3KDo%2F4Ey%2Bqjoz5H86%2FnHjvIPNQRKtPXhEGN0HylrXEKMMR2v7KWQdCs3GKmGUYkWryzOmbCypmpt5QSC32SldyfQqwiZq2fgFpIqdcr1NXhqfY5wsFQvIk1%2BKcJcfLqbFBfNgtcZu5krn3QUNnLsm9%2FsINwVjrqFGjiLHp4%2Fm7pNVTHed3DRyEC%2BNPh7FQsYlWGuD9gLLgukNMUeJ3syNsbej9hYpvvdHqqRIDsDywIejkCXa5%2F7pQMG751v2cfkk9pUywpjGL6hr0bMqZhnL0P5Dzsf7zjZV7ZjIJnG3seldbCNuGQrBAJdxxfYXMshpJn0Tam9hi7fRS3X7BFHlX8NpCedh6Xx%2FHAFnCA2O7KC6Rh5Qb%2Fk%2FnuFrbAPZoibgmn11r%2BsXIV9I%2BjAbtztULX2n1ehksCn4f%2Fk9b%2FKgc6bf46d828Na4fi8ZqTYlq95KuCHQcJzACpPhXjIeHiYtRDChponYBjqkAVSk6lFdzlMexHzSMHaZ7hTOU4bNW5NEeDKej5Q7eY4OL3o8Xv6vLP3iffaV2scEafbD0hL6EfovLMmlDTBKFa607ZH43eNUvSkcBjef7tyu3rsQgbq6R3ABEMRN7cWvpSdhtIa245OQTMZlb7GEDV3CD1NhKADlLcjKFRfSsgHHN9rtUEGcWg6tkK5sVhdx2ANaqBw507hOzBAObiqyDen2sktm&X-Amz-Signature=a89d2c42cc70f7eea94f157c56b4246d53434884731d46c0118c39d93155d87f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X2R67763%2F20261122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261122T035954Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxVZDvF96GzrM51%2F5RyYPMqf%2FKW%2Fdr1UXiQxJoVAE5wQIhALbDAigSh5Dn1T4kofrCD5rrENgH3q4lS3yda7IpZEItKv8DCFMQABoMNjM3NDIzMTgzODA1IgyJRI6GNc6AZfPUDEwq3AOJdPcNoQmb8pkHv1b6uGiJtwCjm48a2Utw6KadnxlFCVu8afRPxnGYQLyqLeNUw0RFMJjajqHbe7nDwjs07HaJQBz%2B9%2FcEtRKqtQ2lEf3ymF4%2BaR6dIS6Q0IVq1hwYzm7QDU6CVIHvfZouYhpdYblIun8hAoWVqDTPSw3KDo%2F4Ey%2Bqjoz5H86%2FnHjvIPNQRKtPXhEGN0HylrXEKMMR2v7KWQdCs3GKmGUYkWryzOmbCypmpt5QSC32SldyfQqwiZq2fgFpIqdcr1NXhqfY5wsFQvIk1%2BKcJcfLqbFBfNgtcZu5krn3QUNnLsm9%2FsINwVjrqFGjiLHp4%2Fm7pNVTHed3DRyEC%2BNPh7FQsYlWGuD9gLLgukNMUeJ3syNsbej9hYpvvdHqqRIDsDywIejkCXa5%2F7pQMG751v2cfkk9pUywpjGL6hr0bMqZhnL0P5Dzsf7zjZV7ZjIJnG3seldbCNuGQrBAJdxxfYXMshpJn0Tam9hi7fRS3X7BFHlX8NpCedh6Xx%2FHAFnCA2O7KC6Rh5Qb%2Fk%2FnuFrbAPZoibgmn11r%2BsXIV9I%2BjAbtztULX2n1ehksCn4f%2Fk9b%2FKgc6bf46d828Na4fi8ZqTYlq95KuCHQcJzACpPhXjIeHiYtRDChponYBjqkAVSk6lFdzlMexHzSMHaZ7hTOU4bNW5NEeDKej5Q7eY4OL3o8Xv6vLP3iffaV2scEafbD0hL6EfovLMmlDTBKFa607ZH43eNUvSkcBjef7tyu3rsQgbq6R3ABEMRN7cWvpSdhtIa245OQTMZlb7GEDV3CD1NhKADlLcjKFRfSsgHHN9rtUEGcWg6tkK5sVhdx2ANaqBw507hOzBAObiqyDen2sktm&X-Amz-Signature=0c449a40f56c4834bab81a1c1a65461e1a14ee51f5981e79ec9634d40cd08f37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X2R67763%2F20261122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261122T035954Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxVZDvF96GzrM51%2F5RyYPMqf%2FKW%2Fdr1UXiQxJoVAE5wQIhALbDAigSh5Dn1T4kofrCD5rrENgH3q4lS3yda7IpZEItKv8DCFMQABoMNjM3NDIzMTgzODA1IgyJRI6GNc6AZfPUDEwq3AOJdPcNoQmb8pkHv1b6uGiJtwCjm48a2Utw6KadnxlFCVu8afRPxnGYQLyqLeNUw0RFMJjajqHbe7nDwjs07HaJQBz%2B9%2FcEtRKqtQ2lEf3ymF4%2BaR6dIS6Q0IVq1hwYzm7QDU6CVIHvfZouYhpdYblIun8hAoWVqDTPSw3KDo%2F4Ey%2Bqjoz5H86%2FnHjvIPNQRKtPXhEGN0HylrXEKMMR2v7KWQdCs3GKmGUYkWryzOmbCypmpt5QSC32SldyfQqwiZq2fgFpIqdcr1NXhqfY5wsFQvIk1%2BKcJcfLqbFBfNgtcZu5krn3QUNnLsm9%2FsINwVjrqFGjiLHp4%2Fm7pNVTHed3DRyEC%2BNPh7FQsYlWGuD9gLLgukNMUeJ3syNsbej9hYpvvdHqqRIDsDywIejkCXa5%2F7pQMG751v2cfkk9pUywpjGL6hr0bMqZhnL0P5Dzsf7zjZV7ZjIJnG3seldbCNuGQrBAJdxxfYXMshpJn0Tam9hi7fRS3X7BFHlX8NpCedh6Xx%2FHAFnCA2O7KC6Rh5Qb%2Fk%2FnuFrbAPZoibgmn11r%2BsXIV9I%2BjAbtztULX2n1ehksCn4f%2Fk9b%2FKgc6bf46d828Na4fi8ZqTYlq95KuCHQcJzACpPhXjIeHiYtRDChponYBjqkAVSk6lFdzlMexHzSMHaZ7hTOU4bNW5NEeDKej5Q7eY4OL3o8Xv6vLP3iffaV2scEafbD0hL6EfovLMmlDTBKFa607ZH43eNUvSkcBjef7tyu3rsQgbq6R3ABEMRN7cWvpSdhtIa245OQTMZlb7GEDV3CD1NhKADlLcjKFRfSsgHHN9rtUEGcWg6tkK5sVhdx2ANaqBw507hOzBAObiqyDen2sktm&X-Amz-Signature=d0c7b3b4f3990c0cae334230c843e729eb7e7108bc7dbf5a40af7fdc7cd47d21&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
