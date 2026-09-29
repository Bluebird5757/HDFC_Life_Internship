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
fetched_at: '2026-09-29T03:15:09.501Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2ATBLKV%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031501Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQDOfDIRpPschVVqiXIwPyEce1v%2Bs%2BiROquS05GK9zMaBAIhAOC76vahzx8oLcyoJbUS7s3o7pLNuk7MWPP2NRIf6KBDKv8DCEQQABoMNjM3NDIzMTgzODA1IgwOBe06FyStNnVSDHEq3APjq%2FbCV1GXw6Zg025ZzikyWoFS%2F9BoGnIXu0oqDvWfwjNdTQLvGU%2BDR3ujHyP9q%2BcvM5qo3di%2BBH1DcfXk2OGTROThTdyrIOHWVaB0EY9Zb5OtuaS0Mf8%2Bd1DbY%2BSXovbCbf5XOU%2BE8UBCUzIQru1cf7F18kTEvjxXwJUf%2B0KKe1Qt%2FUI4XjfYCARkOo8Cp4N53Qc%2F59QF9Vnd%2BUhYCa7dtSDesK%2FZizLo%2FBjdVbwPO%2FFBiL6hk%2FWZO0efP7W5urnVfIhkuL3QMCREyNIhfwWhs69qdr6nol%2BDFGXNJ9nXoYx4Q6jSox4HheZGM6xWZFn2HfMcctcdS%2Fpa6u9BAiUIx4302KUNofh2TNFX%2F1kd2gZIAOah%2Bv9tH7eAlGMI90opnLpnTXIslJGRfWaB7IWhsMLgq2kR%2Fe0P1XjLOqu54CX1UUB3C0UfAATrcR0Zc%2BPOWfbA%2BTPH7QagHRZj%2FJxSNwSBG4deKNfnTQuupfNUiGTTqOJFavmntf9JTEIRlqE8nxaYCoSjFyVqBPTqgYHYn4CBQSMRyQURZiPmkYi%2BePrAaSpGWTX0uWDU6peG3Lfe0yi475ynRW9%2FFX1X2aXcI7DD8t8Q9uq9pfFR2NLAwgPat56gtKwckx%2BclzDWzOzVBjqkAUzMOSljcjxmexWwx2YMAbTsN0fkZq0OYV2lAov8CuR8QF6%2B%2BE13ZNP%2FbcdpsUnZ1DdNDSDJATHCcmXLei%2BSU9%2Fi5fddB9RBH9tUyJYy0z509fZBeDfryBC7sLOqcjgBPXLSHrMQ39gUXoN03xYJvkH5RO1wczfi8jCkiUmz1Cl86TSTpKmhoX1HyBUVFqA9Vq2wLT%2BOklrI1O%2Bix%2Br4W5Z%2FvSGE&X-Amz-Signature=61b6cf80d56bf3d23164fa4502072d6b6574ae68992bd44a10a5a0967ed2307d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2ATBLKV%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031501Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQDOfDIRpPschVVqiXIwPyEce1v%2Bs%2BiROquS05GK9zMaBAIhAOC76vahzx8oLcyoJbUS7s3o7pLNuk7MWPP2NRIf6KBDKv8DCEQQABoMNjM3NDIzMTgzODA1IgwOBe06FyStNnVSDHEq3APjq%2FbCV1GXw6Zg025ZzikyWoFS%2F9BoGnIXu0oqDvWfwjNdTQLvGU%2BDR3ujHyP9q%2BcvM5qo3di%2BBH1DcfXk2OGTROThTdyrIOHWVaB0EY9Zb5OtuaS0Mf8%2Bd1DbY%2BSXovbCbf5XOU%2BE8UBCUzIQru1cf7F18kTEvjxXwJUf%2B0KKe1Qt%2FUI4XjfYCARkOo8Cp4N53Qc%2F59QF9Vnd%2BUhYCa7dtSDesK%2FZizLo%2FBjdVbwPO%2FFBiL6hk%2FWZO0efP7W5urnVfIhkuL3QMCREyNIhfwWhs69qdr6nol%2BDFGXNJ9nXoYx4Q6jSox4HheZGM6xWZFn2HfMcctcdS%2Fpa6u9BAiUIx4302KUNofh2TNFX%2F1kd2gZIAOah%2Bv9tH7eAlGMI90opnLpnTXIslJGRfWaB7IWhsMLgq2kR%2Fe0P1XjLOqu54CX1UUB3C0UfAATrcR0Zc%2BPOWfbA%2BTPH7QagHRZj%2FJxSNwSBG4deKNfnTQuupfNUiGTTqOJFavmntf9JTEIRlqE8nxaYCoSjFyVqBPTqgYHYn4CBQSMRyQURZiPmkYi%2BePrAaSpGWTX0uWDU6peG3Lfe0yi475ynRW9%2FFX1X2aXcI7DD8t8Q9uq9pfFR2NLAwgPat56gtKwckx%2BclzDWzOzVBjqkAUzMOSljcjxmexWwx2YMAbTsN0fkZq0OYV2lAov8CuR8QF6%2B%2BE13ZNP%2FbcdpsUnZ1DdNDSDJATHCcmXLei%2BSU9%2Fi5fddB9RBH9tUyJYy0z509fZBeDfryBC7sLOqcjgBPXLSHrMQ39gUXoN03xYJvkH5RO1wczfi8jCkiUmz1Cl86TSTpKmhoX1HyBUVFqA9Vq2wLT%2BOklrI1O%2Bix%2Br4W5Z%2FvSGE&X-Amz-Signature=4dc967b2a6f20062c1c4da6efa2d24b717844a7dd6231f923867c8b56185b76a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2ATBLKV%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031501Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQDOfDIRpPschVVqiXIwPyEce1v%2Bs%2BiROquS05GK9zMaBAIhAOC76vahzx8oLcyoJbUS7s3o7pLNuk7MWPP2NRIf6KBDKv8DCEQQABoMNjM3NDIzMTgzODA1IgwOBe06FyStNnVSDHEq3APjq%2FbCV1GXw6Zg025ZzikyWoFS%2F9BoGnIXu0oqDvWfwjNdTQLvGU%2BDR3ujHyP9q%2BcvM5qo3di%2BBH1DcfXk2OGTROThTdyrIOHWVaB0EY9Zb5OtuaS0Mf8%2Bd1DbY%2BSXovbCbf5XOU%2BE8UBCUzIQru1cf7F18kTEvjxXwJUf%2B0KKe1Qt%2FUI4XjfYCARkOo8Cp4N53Qc%2F59QF9Vnd%2BUhYCa7dtSDesK%2FZizLo%2FBjdVbwPO%2FFBiL6hk%2FWZO0efP7W5urnVfIhkuL3QMCREyNIhfwWhs69qdr6nol%2BDFGXNJ9nXoYx4Q6jSox4HheZGM6xWZFn2HfMcctcdS%2Fpa6u9BAiUIx4302KUNofh2TNFX%2F1kd2gZIAOah%2Bv9tH7eAlGMI90opnLpnTXIslJGRfWaB7IWhsMLgq2kR%2Fe0P1XjLOqu54CX1UUB3C0UfAATrcR0Zc%2BPOWfbA%2BTPH7QagHRZj%2FJxSNwSBG4deKNfnTQuupfNUiGTTqOJFavmntf9JTEIRlqE8nxaYCoSjFyVqBPTqgYHYn4CBQSMRyQURZiPmkYi%2BePrAaSpGWTX0uWDU6peG3Lfe0yi475ynRW9%2FFX1X2aXcI7DD8t8Q9uq9pfFR2NLAwgPat56gtKwckx%2BclzDWzOzVBjqkAUzMOSljcjxmexWwx2YMAbTsN0fkZq0OYV2lAov8CuR8QF6%2B%2BE13ZNP%2FbcdpsUnZ1DdNDSDJATHCcmXLei%2BSU9%2Fi5fddB9RBH9tUyJYy0z509fZBeDfryBC7sLOqcjgBPXLSHrMQ39gUXoN03xYJvkH5RO1wczfi8jCkiUmz1Cl86TSTpKmhoX1HyBUVFqA9Vq2wLT%2BOklrI1O%2Bix%2Br4W5Z%2FvSGE&X-Amz-Signature=e45810fe4eb5c1b335a8dc686d8458c31c097707206ba57a4bb2b770562f52ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2ATBLKV%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031501Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQDOfDIRpPschVVqiXIwPyEce1v%2Bs%2BiROquS05GK9zMaBAIhAOC76vahzx8oLcyoJbUS7s3o7pLNuk7MWPP2NRIf6KBDKv8DCEQQABoMNjM3NDIzMTgzODA1IgwOBe06FyStNnVSDHEq3APjq%2FbCV1GXw6Zg025ZzikyWoFS%2F9BoGnIXu0oqDvWfwjNdTQLvGU%2BDR3ujHyP9q%2BcvM5qo3di%2BBH1DcfXk2OGTROThTdyrIOHWVaB0EY9Zb5OtuaS0Mf8%2Bd1DbY%2BSXovbCbf5XOU%2BE8UBCUzIQru1cf7F18kTEvjxXwJUf%2B0KKe1Qt%2FUI4XjfYCARkOo8Cp4N53Qc%2F59QF9Vnd%2BUhYCa7dtSDesK%2FZizLo%2FBjdVbwPO%2FFBiL6hk%2FWZO0efP7W5urnVfIhkuL3QMCREyNIhfwWhs69qdr6nol%2BDFGXNJ9nXoYx4Q6jSox4HheZGM6xWZFn2HfMcctcdS%2Fpa6u9BAiUIx4302KUNofh2TNFX%2F1kd2gZIAOah%2Bv9tH7eAlGMI90opnLpnTXIslJGRfWaB7IWhsMLgq2kR%2Fe0P1XjLOqu54CX1UUB3C0UfAATrcR0Zc%2BPOWfbA%2BTPH7QagHRZj%2FJxSNwSBG4deKNfnTQuupfNUiGTTqOJFavmntf9JTEIRlqE8nxaYCoSjFyVqBPTqgYHYn4CBQSMRyQURZiPmkYi%2BePrAaSpGWTX0uWDU6peG3Lfe0yi475ynRW9%2FFX1X2aXcI7DD8t8Q9uq9pfFR2NLAwgPat56gtKwckx%2BclzDWzOzVBjqkAUzMOSljcjxmexWwx2YMAbTsN0fkZq0OYV2lAov8CuR8QF6%2B%2BE13ZNP%2FbcdpsUnZ1DdNDSDJATHCcmXLei%2BSU9%2Fi5fddB9RBH9tUyJYy0z509fZBeDfryBC7sLOqcjgBPXLSHrMQ39gUXoN03xYJvkH5RO1wczfi8jCkiUmz1Cl86TSTpKmhoX1HyBUVFqA9Vq2wLT%2BOklrI1O%2Bix%2Br4W5Z%2FvSGE&X-Amz-Signature=21a0d1f7957f516e40adc6ba2cb0707b18626aa178466ec9f2a581365a09a72d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2ATBLKV%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031501Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQDOfDIRpPschVVqiXIwPyEce1v%2Bs%2BiROquS05GK9zMaBAIhAOC76vahzx8oLcyoJbUS7s3o7pLNuk7MWPP2NRIf6KBDKv8DCEQQABoMNjM3NDIzMTgzODA1IgwOBe06FyStNnVSDHEq3APjq%2FbCV1GXw6Zg025ZzikyWoFS%2F9BoGnIXu0oqDvWfwjNdTQLvGU%2BDR3ujHyP9q%2BcvM5qo3di%2BBH1DcfXk2OGTROThTdyrIOHWVaB0EY9Zb5OtuaS0Mf8%2Bd1DbY%2BSXovbCbf5XOU%2BE8UBCUzIQru1cf7F18kTEvjxXwJUf%2B0KKe1Qt%2FUI4XjfYCARkOo8Cp4N53Qc%2F59QF9Vnd%2BUhYCa7dtSDesK%2FZizLo%2FBjdVbwPO%2FFBiL6hk%2FWZO0efP7W5urnVfIhkuL3QMCREyNIhfwWhs69qdr6nol%2BDFGXNJ9nXoYx4Q6jSox4HheZGM6xWZFn2HfMcctcdS%2Fpa6u9BAiUIx4302KUNofh2TNFX%2F1kd2gZIAOah%2Bv9tH7eAlGMI90opnLpnTXIslJGRfWaB7IWhsMLgq2kR%2Fe0P1XjLOqu54CX1UUB3C0UfAATrcR0Zc%2BPOWfbA%2BTPH7QagHRZj%2FJxSNwSBG4deKNfnTQuupfNUiGTTqOJFavmntf9JTEIRlqE8nxaYCoSjFyVqBPTqgYHYn4CBQSMRyQURZiPmkYi%2BePrAaSpGWTX0uWDU6peG3Lfe0yi475ynRW9%2FFX1X2aXcI7DD8t8Q9uq9pfFR2NLAwgPat56gtKwckx%2BclzDWzOzVBjqkAUzMOSljcjxmexWwx2YMAbTsN0fkZq0OYV2lAov8CuR8QF6%2B%2BE13ZNP%2FbcdpsUnZ1DdNDSDJATHCcmXLei%2BSU9%2Fi5fddB9RBH9tUyJYy0z509fZBeDfryBC7sLOqcjgBPXLSHrMQ39gUXoN03xYJvkH5RO1wczfi8jCkiUmz1Cl86TSTpKmhoX1HyBUVFqA9Vq2wLT%2BOklrI1O%2Bix%2Br4W5Z%2FvSGE&X-Amz-Signature=d7765a9ced1dd19ef47662e75a8594e80d67ea999ff512d947c6b6dde382617e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2ATBLKV%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031501Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQDOfDIRpPschVVqiXIwPyEce1v%2Bs%2BiROquS05GK9zMaBAIhAOC76vahzx8oLcyoJbUS7s3o7pLNuk7MWPP2NRIf6KBDKv8DCEQQABoMNjM3NDIzMTgzODA1IgwOBe06FyStNnVSDHEq3APjq%2FbCV1GXw6Zg025ZzikyWoFS%2F9BoGnIXu0oqDvWfwjNdTQLvGU%2BDR3ujHyP9q%2BcvM5qo3di%2BBH1DcfXk2OGTROThTdyrIOHWVaB0EY9Zb5OtuaS0Mf8%2Bd1DbY%2BSXovbCbf5XOU%2BE8UBCUzIQru1cf7F18kTEvjxXwJUf%2B0KKe1Qt%2FUI4XjfYCARkOo8Cp4N53Qc%2F59QF9Vnd%2BUhYCa7dtSDesK%2FZizLo%2FBjdVbwPO%2FFBiL6hk%2FWZO0efP7W5urnVfIhkuL3QMCREyNIhfwWhs69qdr6nol%2BDFGXNJ9nXoYx4Q6jSox4HheZGM6xWZFn2HfMcctcdS%2Fpa6u9BAiUIx4302KUNofh2TNFX%2F1kd2gZIAOah%2Bv9tH7eAlGMI90opnLpnTXIslJGRfWaB7IWhsMLgq2kR%2Fe0P1XjLOqu54CX1UUB3C0UfAATrcR0Zc%2BPOWfbA%2BTPH7QagHRZj%2FJxSNwSBG4deKNfnTQuupfNUiGTTqOJFavmntf9JTEIRlqE8nxaYCoSjFyVqBPTqgYHYn4CBQSMRyQURZiPmkYi%2BePrAaSpGWTX0uWDU6peG3Lfe0yi475ynRW9%2FFX1X2aXcI7DD8t8Q9uq9pfFR2NLAwgPat56gtKwckx%2BclzDWzOzVBjqkAUzMOSljcjxmexWwx2YMAbTsN0fkZq0OYV2lAov8CuR8QF6%2B%2BE13ZNP%2FbcdpsUnZ1DdNDSDJATHCcmXLei%2BSU9%2Fi5fddB9RBH9tUyJYy0z509fZBeDfryBC7sLOqcjgBPXLSHrMQ39gUXoN03xYJvkH5RO1wczfi8jCkiUmz1Cl86TSTpKmhoX1HyBUVFqA9Vq2wLT%2BOklrI1O%2Bix%2Br4W5Z%2FvSGE&X-Amz-Signature=4c2199cfdd2b0f7d889ca6df8d164438d5dcc9913d26192e9c9ab563c4dcc79e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2ATBLKV%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031501Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQDOfDIRpPschVVqiXIwPyEce1v%2Bs%2BiROquS05GK9zMaBAIhAOC76vahzx8oLcyoJbUS7s3o7pLNuk7MWPP2NRIf6KBDKv8DCEQQABoMNjM3NDIzMTgzODA1IgwOBe06FyStNnVSDHEq3APjq%2FbCV1GXw6Zg025ZzikyWoFS%2F9BoGnIXu0oqDvWfwjNdTQLvGU%2BDR3ujHyP9q%2BcvM5qo3di%2BBH1DcfXk2OGTROThTdyrIOHWVaB0EY9Zb5OtuaS0Mf8%2Bd1DbY%2BSXovbCbf5XOU%2BE8UBCUzIQru1cf7F18kTEvjxXwJUf%2B0KKe1Qt%2FUI4XjfYCARkOo8Cp4N53Qc%2F59QF9Vnd%2BUhYCa7dtSDesK%2FZizLo%2FBjdVbwPO%2FFBiL6hk%2FWZO0efP7W5urnVfIhkuL3QMCREyNIhfwWhs69qdr6nol%2BDFGXNJ9nXoYx4Q6jSox4HheZGM6xWZFn2HfMcctcdS%2Fpa6u9BAiUIx4302KUNofh2TNFX%2F1kd2gZIAOah%2Bv9tH7eAlGMI90opnLpnTXIslJGRfWaB7IWhsMLgq2kR%2Fe0P1XjLOqu54CX1UUB3C0UfAATrcR0Zc%2BPOWfbA%2BTPH7QagHRZj%2FJxSNwSBG4deKNfnTQuupfNUiGTTqOJFavmntf9JTEIRlqE8nxaYCoSjFyVqBPTqgYHYn4CBQSMRyQURZiPmkYi%2BePrAaSpGWTX0uWDU6peG3Lfe0yi475ynRW9%2FFX1X2aXcI7DD8t8Q9uq9pfFR2NLAwgPat56gtKwckx%2BclzDWzOzVBjqkAUzMOSljcjxmexWwx2YMAbTsN0fkZq0OYV2lAov8CuR8QF6%2B%2BE13ZNP%2FbcdpsUnZ1DdNDSDJATHCcmXLei%2BSU9%2Fi5fddB9RBH9tUyJYy0z509fZBeDfryBC7sLOqcjgBPXLSHrMQ39gUXoN03xYJvkH5RO1wczfi8jCkiUmz1Cl86TSTpKmhoX1HyBUVFqA9Vq2wLT%2BOklrI1O%2Bix%2Br4W5Z%2FvSGE&X-Amz-Signature=d3385f99e60f0938f2d17212353ee383b54ada2b5ae7fad53e59d218ad630a0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2ATBLKV%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031501Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQDOfDIRpPschVVqiXIwPyEce1v%2Bs%2BiROquS05GK9zMaBAIhAOC76vahzx8oLcyoJbUS7s3o7pLNuk7MWPP2NRIf6KBDKv8DCEQQABoMNjM3NDIzMTgzODA1IgwOBe06FyStNnVSDHEq3APjq%2FbCV1GXw6Zg025ZzikyWoFS%2F9BoGnIXu0oqDvWfwjNdTQLvGU%2BDR3ujHyP9q%2BcvM5qo3di%2BBH1DcfXk2OGTROThTdyrIOHWVaB0EY9Zb5OtuaS0Mf8%2Bd1DbY%2BSXovbCbf5XOU%2BE8UBCUzIQru1cf7F18kTEvjxXwJUf%2B0KKe1Qt%2FUI4XjfYCARkOo8Cp4N53Qc%2F59QF9Vnd%2BUhYCa7dtSDesK%2FZizLo%2FBjdVbwPO%2FFBiL6hk%2FWZO0efP7W5urnVfIhkuL3QMCREyNIhfwWhs69qdr6nol%2BDFGXNJ9nXoYx4Q6jSox4HheZGM6xWZFn2HfMcctcdS%2Fpa6u9BAiUIx4302KUNofh2TNFX%2F1kd2gZIAOah%2Bv9tH7eAlGMI90opnLpnTXIslJGRfWaB7IWhsMLgq2kR%2Fe0P1XjLOqu54CX1UUB3C0UfAATrcR0Zc%2BPOWfbA%2BTPH7QagHRZj%2FJxSNwSBG4deKNfnTQuupfNUiGTTqOJFavmntf9JTEIRlqE8nxaYCoSjFyVqBPTqgYHYn4CBQSMRyQURZiPmkYi%2BePrAaSpGWTX0uWDU6peG3Lfe0yi475ynRW9%2FFX1X2aXcI7DD8t8Q9uq9pfFR2NLAwgPat56gtKwckx%2BclzDWzOzVBjqkAUzMOSljcjxmexWwx2YMAbTsN0fkZq0OYV2lAov8CuR8QF6%2B%2BE13ZNP%2FbcdpsUnZ1DdNDSDJATHCcmXLei%2BSU9%2Fi5fddB9RBH9tUyJYy0z509fZBeDfryBC7sLOqcjgBPXLSHrMQ39gUXoN03xYJvkH5RO1wczfi8jCkiUmz1Cl86TSTpKmhoX1HyBUVFqA9Vq2wLT%2BOklrI1O%2Bix%2Br4W5Z%2FvSGE&X-Amz-Signature=4c3400fd6e7f152a1ca5f59777bf077d7fcdff2801a1c4b3c78d7dd5b3c37b36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2ATBLKV%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031501Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQDOfDIRpPschVVqiXIwPyEce1v%2Bs%2BiROquS05GK9zMaBAIhAOC76vahzx8oLcyoJbUS7s3o7pLNuk7MWPP2NRIf6KBDKv8DCEQQABoMNjM3NDIzMTgzODA1IgwOBe06FyStNnVSDHEq3APjq%2FbCV1GXw6Zg025ZzikyWoFS%2F9BoGnIXu0oqDvWfwjNdTQLvGU%2BDR3ujHyP9q%2BcvM5qo3di%2BBH1DcfXk2OGTROThTdyrIOHWVaB0EY9Zb5OtuaS0Mf8%2Bd1DbY%2BSXovbCbf5XOU%2BE8UBCUzIQru1cf7F18kTEvjxXwJUf%2B0KKe1Qt%2FUI4XjfYCARkOo8Cp4N53Qc%2F59QF9Vnd%2BUhYCa7dtSDesK%2FZizLo%2FBjdVbwPO%2FFBiL6hk%2FWZO0efP7W5urnVfIhkuL3QMCREyNIhfwWhs69qdr6nol%2BDFGXNJ9nXoYx4Q6jSox4HheZGM6xWZFn2HfMcctcdS%2Fpa6u9BAiUIx4302KUNofh2TNFX%2F1kd2gZIAOah%2Bv9tH7eAlGMI90opnLpnTXIslJGRfWaB7IWhsMLgq2kR%2Fe0P1XjLOqu54CX1UUB3C0UfAATrcR0Zc%2BPOWfbA%2BTPH7QagHRZj%2FJxSNwSBG4deKNfnTQuupfNUiGTTqOJFavmntf9JTEIRlqE8nxaYCoSjFyVqBPTqgYHYn4CBQSMRyQURZiPmkYi%2BePrAaSpGWTX0uWDU6peG3Lfe0yi475ynRW9%2FFX1X2aXcI7DD8t8Q9uq9pfFR2NLAwgPat56gtKwckx%2BclzDWzOzVBjqkAUzMOSljcjxmexWwx2YMAbTsN0fkZq0OYV2lAov8CuR8QF6%2B%2BE13ZNP%2FbcdpsUnZ1DdNDSDJATHCcmXLei%2BSU9%2Fi5fddB9RBH9tUyJYy0z509fZBeDfryBC7sLOqcjgBPXLSHrMQ39gUXoN03xYJvkH5RO1wczfi8jCkiUmz1Cl86TSTpKmhoX1HyBUVFqA9Vq2wLT%2BOklrI1O%2Bix%2Br4W5Z%2FvSGE&X-Amz-Signature=618d8530ab56425a49aa8143872fd75a473221389b1e5ec5344544031002932a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
