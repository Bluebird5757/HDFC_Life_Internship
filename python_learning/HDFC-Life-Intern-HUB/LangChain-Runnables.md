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
fetched_at: '2026-10-19T02:22:00.160Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662YDXWLRC%2F20261019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261019T022155Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIQDWlSX26JR5XhSLe9qFBF0zzU1qAmEQCUHwnUM3puO41QIgX1a%2FqIlaOXbrtNFn3Mea8xsWiabfEwKtxvQOD3uMFaQq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDOmQaWkgX1XQSGmBtCrcA5GashUe2uMVUl0gPfd2%2FAZsae3PHhJk2MXNOuIkb2%2Foc%2FhpGpo2XoohQ6%2FJHDbYZ8AyX3G2c%2BIB4rhAFaVwDMBAe3dpgMBwO5p3N65CoLQi8H8sn4H6X%2FB9VVaNYAMa9LwZ87PkiZ8OrkdMd1zvHt2AYNUCY7w1JJvdg2gKyn8XqboUM4ch%2BHb0jyb0zbF9zYWqFmipMeE4f%2FTAOQYuBBiqSG%2Fd7S%2Bd59yzuyVT6NAq0S4YJdXQ3mC7D%2F%2BbOlEVWWTzGrFsnzRO4SogbQsSFv2Wj5UJyQAcSNYPAYA15vX%2F2aV%2BV%2FWj%2BgM1k8ndIGfNYM7rqxiTPwOLIXxO5L8FXO%2BZ3WxWEGBLvYWHWgbvnAFSil%2Fv0ljCgk5Pm8IWC1gFnkQs9biP%2BOWZUHiwMvS%2BP9fuvJ8HaqbY8yNjWSOVQXS4RA41S4FrAI%2F7liEbPpql7hgiU6m%2B9DruGEZtSEPej%2FfWgPn1jXcgaeoQlfHvunLh2%2BaML53Y1Wt1V5jK7LjemYicGpZp4yhXAQJR3QvlbpDK%2Bl3CuY08TtSmlKR4hP9JIbNGsv0WnNf3MDSwTjYRGx8O7sqymWEy%2FnNsq0uM85TZCUnCnyYA1Wca3WCpWOcIKKUm1a8HVkIZW50EMKXu1dYGOqUBr%2Bc5ExQuDOS0ESrDnMwzD3mlryTYNuzV%2F%2FdQAiw55VRSGZKF2bRVUStIDEudL%2FyUePHQsqik0caX6n7HjkoU427JubxAuhAi428wMBzCNLt1VV4bL9tvl4NPqrD5F492iud4UrgPGfpq23hO7QKx4Q2QQ%2FC8FHHRxrKqbsHtkXKgsNS8n3bp1l71cPa%2BguNhs2CJUpO3fcurwjc4W4pNBFLeHc%2BK&X-Amz-Signature=3dc60d59c8ef849dd731a0fe109b55633d39ce47a59d5951b1761e041a73d3f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662YDXWLRC%2F20261019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261019T022155Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIQDWlSX26JR5XhSLe9qFBF0zzU1qAmEQCUHwnUM3puO41QIgX1a%2FqIlaOXbrtNFn3Mea8xsWiabfEwKtxvQOD3uMFaQq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDOmQaWkgX1XQSGmBtCrcA5GashUe2uMVUl0gPfd2%2FAZsae3PHhJk2MXNOuIkb2%2Foc%2FhpGpo2XoohQ6%2FJHDbYZ8AyX3G2c%2BIB4rhAFaVwDMBAe3dpgMBwO5p3N65CoLQi8H8sn4H6X%2FB9VVaNYAMa9LwZ87PkiZ8OrkdMd1zvHt2AYNUCY7w1JJvdg2gKyn8XqboUM4ch%2BHb0jyb0zbF9zYWqFmipMeE4f%2FTAOQYuBBiqSG%2Fd7S%2Bd59yzuyVT6NAq0S4YJdXQ3mC7D%2F%2BbOlEVWWTzGrFsnzRO4SogbQsSFv2Wj5UJyQAcSNYPAYA15vX%2F2aV%2BV%2FWj%2BgM1k8ndIGfNYM7rqxiTPwOLIXxO5L8FXO%2BZ3WxWEGBLvYWHWgbvnAFSil%2Fv0ljCgk5Pm8IWC1gFnkQs9biP%2BOWZUHiwMvS%2BP9fuvJ8HaqbY8yNjWSOVQXS4RA41S4FrAI%2F7liEbPpql7hgiU6m%2B9DruGEZtSEPej%2FfWgPn1jXcgaeoQlfHvunLh2%2BaML53Y1Wt1V5jK7LjemYicGpZp4yhXAQJR3QvlbpDK%2Bl3CuY08TtSmlKR4hP9JIbNGsv0WnNf3MDSwTjYRGx8O7sqymWEy%2FnNsq0uM85TZCUnCnyYA1Wca3WCpWOcIKKUm1a8HVkIZW50EMKXu1dYGOqUBr%2Bc5ExQuDOS0ESrDnMwzD3mlryTYNuzV%2F%2FdQAiw55VRSGZKF2bRVUStIDEudL%2FyUePHQsqik0caX6n7HjkoU427JubxAuhAi428wMBzCNLt1VV4bL9tvl4NPqrD5F492iud4UrgPGfpq23hO7QKx4Q2QQ%2FC8FHHRxrKqbsHtkXKgsNS8n3bp1l71cPa%2BguNhs2CJUpO3fcurwjc4W4pNBFLeHc%2BK&X-Amz-Signature=9549577000cf5c288e24a2bce867f27d9c4885369962dc7857eb8a6e7db6b636&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662YDXWLRC%2F20261019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261019T022155Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIQDWlSX26JR5XhSLe9qFBF0zzU1qAmEQCUHwnUM3puO41QIgX1a%2FqIlaOXbrtNFn3Mea8xsWiabfEwKtxvQOD3uMFaQq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDOmQaWkgX1XQSGmBtCrcA5GashUe2uMVUl0gPfd2%2FAZsae3PHhJk2MXNOuIkb2%2Foc%2FhpGpo2XoohQ6%2FJHDbYZ8AyX3G2c%2BIB4rhAFaVwDMBAe3dpgMBwO5p3N65CoLQi8H8sn4H6X%2FB9VVaNYAMa9LwZ87PkiZ8OrkdMd1zvHt2AYNUCY7w1JJvdg2gKyn8XqboUM4ch%2BHb0jyb0zbF9zYWqFmipMeE4f%2FTAOQYuBBiqSG%2Fd7S%2Bd59yzuyVT6NAq0S4YJdXQ3mC7D%2F%2BbOlEVWWTzGrFsnzRO4SogbQsSFv2Wj5UJyQAcSNYPAYA15vX%2F2aV%2BV%2FWj%2BgM1k8ndIGfNYM7rqxiTPwOLIXxO5L8FXO%2BZ3WxWEGBLvYWHWgbvnAFSil%2Fv0ljCgk5Pm8IWC1gFnkQs9biP%2BOWZUHiwMvS%2BP9fuvJ8HaqbY8yNjWSOVQXS4RA41S4FrAI%2F7liEbPpql7hgiU6m%2B9DruGEZtSEPej%2FfWgPn1jXcgaeoQlfHvunLh2%2BaML53Y1Wt1V5jK7LjemYicGpZp4yhXAQJR3QvlbpDK%2Bl3CuY08TtSmlKR4hP9JIbNGsv0WnNf3MDSwTjYRGx8O7sqymWEy%2FnNsq0uM85TZCUnCnyYA1Wca3WCpWOcIKKUm1a8HVkIZW50EMKXu1dYGOqUBr%2Bc5ExQuDOS0ESrDnMwzD3mlryTYNuzV%2F%2FdQAiw55VRSGZKF2bRVUStIDEudL%2FyUePHQsqik0caX6n7HjkoU427JubxAuhAi428wMBzCNLt1VV4bL9tvl4NPqrD5F492iud4UrgPGfpq23hO7QKx4Q2QQ%2FC8FHHRxrKqbsHtkXKgsNS8n3bp1l71cPa%2BguNhs2CJUpO3fcurwjc4W4pNBFLeHc%2BK&X-Amz-Signature=58457554d5230d259b39a14cded6b8c850ef1a6c25e803a32e178f4b30e45858&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662YDXWLRC%2F20261019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261019T022155Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIQDWlSX26JR5XhSLe9qFBF0zzU1qAmEQCUHwnUM3puO41QIgX1a%2FqIlaOXbrtNFn3Mea8xsWiabfEwKtxvQOD3uMFaQq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDOmQaWkgX1XQSGmBtCrcA5GashUe2uMVUl0gPfd2%2FAZsae3PHhJk2MXNOuIkb2%2Foc%2FhpGpo2XoohQ6%2FJHDbYZ8AyX3G2c%2BIB4rhAFaVwDMBAe3dpgMBwO5p3N65CoLQi8H8sn4H6X%2FB9VVaNYAMa9LwZ87PkiZ8OrkdMd1zvHt2AYNUCY7w1JJvdg2gKyn8XqboUM4ch%2BHb0jyb0zbF9zYWqFmipMeE4f%2FTAOQYuBBiqSG%2Fd7S%2Bd59yzuyVT6NAq0S4YJdXQ3mC7D%2F%2BbOlEVWWTzGrFsnzRO4SogbQsSFv2Wj5UJyQAcSNYPAYA15vX%2F2aV%2BV%2FWj%2BgM1k8ndIGfNYM7rqxiTPwOLIXxO5L8FXO%2BZ3WxWEGBLvYWHWgbvnAFSil%2Fv0ljCgk5Pm8IWC1gFnkQs9biP%2BOWZUHiwMvS%2BP9fuvJ8HaqbY8yNjWSOVQXS4RA41S4FrAI%2F7liEbPpql7hgiU6m%2B9DruGEZtSEPej%2FfWgPn1jXcgaeoQlfHvunLh2%2BaML53Y1Wt1V5jK7LjemYicGpZp4yhXAQJR3QvlbpDK%2Bl3CuY08TtSmlKR4hP9JIbNGsv0WnNf3MDSwTjYRGx8O7sqymWEy%2FnNsq0uM85TZCUnCnyYA1Wca3WCpWOcIKKUm1a8HVkIZW50EMKXu1dYGOqUBr%2Bc5ExQuDOS0ESrDnMwzD3mlryTYNuzV%2F%2FdQAiw55VRSGZKF2bRVUStIDEudL%2FyUePHQsqik0caX6n7HjkoU427JubxAuhAi428wMBzCNLt1VV4bL9tvl4NPqrD5F492iud4UrgPGfpq23hO7QKx4Q2QQ%2FC8FHHRxrKqbsHtkXKgsNS8n3bp1l71cPa%2BguNhs2CJUpO3fcurwjc4W4pNBFLeHc%2BK&X-Amz-Signature=06598786bfef763b9b7dfe453a6249c4168e69e66a300f39a4a8d478e7b082b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662YDXWLRC%2F20261019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261019T022155Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIQDWlSX26JR5XhSLe9qFBF0zzU1qAmEQCUHwnUM3puO41QIgX1a%2FqIlaOXbrtNFn3Mea8xsWiabfEwKtxvQOD3uMFaQq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDOmQaWkgX1XQSGmBtCrcA5GashUe2uMVUl0gPfd2%2FAZsae3PHhJk2MXNOuIkb2%2Foc%2FhpGpo2XoohQ6%2FJHDbYZ8AyX3G2c%2BIB4rhAFaVwDMBAe3dpgMBwO5p3N65CoLQi8H8sn4H6X%2FB9VVaNYAMa9LwZ87PkiZ8OrkdMd1zvHt2AYNUCY7w1JJvdg2gKyn8XqboUM4ch%2BHb0jyb0zbF9zYWqFmipMeE4f%2FTAOQYuBBiqSG%2Fd7S%2Bd59yzuyVT6NAq0S4YJdXQ3mC7D%2F%2BbOlEVWWTzGrFsnzRO4SogbQsSFv2Wj5UJyQAcSNYPAYA15vX%2F2aV%2BV%2FWj%2BgM1k8ndIGfNYM7rqxiTPwOLIXxO5L8FXO%2BZ3WxWEGBLvYWHWgbvnAFSil%2Fv0ljCgk5Pm8IWC1gFnkQs9biP%2BOWZUHiwMvS%2BP9fuvJ8HaqbY8yNjWSOVQXS4RA41S4FrAI%2F7liEbPpql7hgiU6m%2B9DruGEZtSEPej%2FfWgPn1jXcgaeoQlfHvunLh2%2BaML53Y1Wt1V5jK7LjemYicGpZp4yhXAQJR3QvlbpDK%2Bl3CuY08TtSmlKR4hP9JIbNGsv0WnNf3MDSwTjYRGx8O7sqymWEy%2FnNsq0uM85TZCUnCnyYA1Wca3WCpWOcIKKUm1a8HVkIZW50EMKXu1dYGOqUBr%2Bc5ExQuDOS0ESrDnMwzD3mlryTYNuzV%2F%2FdQAiw55VRSGZKF2bRVUStIDEudL%2FyUePHQsqik0caX6n7HjkoU427JubxAuhAi428wMBzCNLt1VV4bL9tvl4NPqrD5F492iud4UrgPGfpq23hO7QKx4Q2QQ%2FC8FHHRxrKqbsHtkXKgsNS8n3bp1l71cPa%2BguNhs2CJUpO3fcurwjc4W4pNBFLeHc%2BK&X-Amz-Signature=67ed054af4bbba11c8ae19dea06fd75eb5a21a35e16fb7d1c9f6ab8a7135dbd7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662YDXWLRC%2F20261019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261019T022155Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIQDWlSX26JR5XhSLe9qFBF0zzU1qAmEQCUHwnUM3puO41QIgX1a%2FqIlaOXbrtNFn3Mea8xsWiabfEwKtxvQOD3uMFaQq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDOmQaWkgX1XQSGmBtCrcA5GashUe2uMVUl0gPfd2%2FAZsae3PHhJk2MXNOuIkb2%2Foc%2FhpGpo2XoohQ6%2FJHDbYZ8AyX3G2c%2BIB4rhAFaVwDMBAe3dpgMBwO5p3N65CoLQi8H8sn4H6X%2FB9VVaNYAMa9LwZ87PkiZ8OrkdMd1zvHt2AYNUCY7w1JJvdg2gKyn8XqboUM4ch%2BHb0jyb0zbF9zYWqFmipMeE4f%2FTAOQYuBBiqSG%2Fd7S%2Bd59yzuyVT6NAq0S4YJdXQ3mC7D%2F%2BbOlEVWWTzGrFsnzRO4SogbQsSFv2Wj5UJyQAcSNYPAYA15vX%2F2aV%2BV%2FWj%2BgM1k8ndIGfNYM7rqxiTPwOLIXxO5L8FXO%2BZ3WxWEGBLvYWHWgbvnAFSil%2Fv0ljCgk5Pm8IWC1gFnkQs9biP%2BOWZUHiwMvS%2BP9fuvJ8HaqbY8yNjWSOVQXS4RA41S4FrAI%2F7liEbPpql7hgiU6m%2B9DruGEZtSEPej%2FfWgPn1jXcgaeoQlfHvunLh2%2BaML53Y1Wt1V5jK7LjemYicGpZp4yhXAQJR3QvlbpDK%2Bl3CuY08TtSmlKR4hP9JIbNGsv0WnNf3MDSwTjYRGx8O7sqymWEy%2FnNsq0uM85TZCUnCnyYA1Wca3WCpWOcIKKUm1a8HVkIZW50EMKXu1dYGOqUBr%2Bc5ExQuDOS0ESrDnMwzD3mlryTYNuzV%2F%2FdQAiw55VRSGZKF2bRVUStIDEudL%2FyUePHQsqik0caX6n7HjkoU427JubxAuhAi428wMBzCNLt1VV4bL9tvl4NPqrD5F492iud4UrgPGfpq23hO7QKx4Q2QQ%2FC8FHHRxrKqbsHtkXKgsNS8n3bp1l71cPa%2BguNhs2CJUpO3fcurwjc4W4pNBFLeHc%2BK&X-Amz-Signature=597225891b5f4e07332e3e6cceabd626c2b7cbb40f5b4f647d9d72fadd38da4d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662YDXWLRC%2F20261019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261019T022155Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIQDWlSX26JR5XhSLe9qFBF0zzU1qAmEQCUHwnUM3puO41QIgX1a%2FqIlaOXbrtNFn3Mea8xsWiabfEwKtxvQOD3uMFaQq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDOmQaWkgX1XQSGmBtCrcA5GashUe2uMVUl0gPfd2%2FAZsae3PHhJk2MXNOuIkb2%2Foc%2FhpGpo2XoohQ6%2FJHDbYZ8AyX3G2c%2BIB4rhAFaVwDMBAe3dpgMBwO5p3N65CoLQi8H8sn4H6X%2FB9VVaNYAMa9LwZ87PkiZ8OrkdMd1zvHt2AYNUCY7w1JJvdg2gKyn8XqboUM4ch%2BHb0jyb0zbF9zYWqFmipMeE4f%2FTAOQYuBBiqSG%2Fd7S%2Bd59yzuyVT6NAq0S4YJdXQ3mC7D%2F%2BbOlEVWWTzGrFsnzRO4SogbQsSFv2Wj5UJyQAcSNYPAYA15vX%2F2aV%2BV%2FWj%2BgM1k8ndIGfNYM7rqxiTPwOLIXxO5L8FXO%2BZ3WxWEGBLvYWHWgbvnAFSil%2Fv0ljCgk5Pm8IWC1gFnkQs9biP%2BOWZUHiwMvS%2BP9fuvJ8HaqbY8yNjWSOVQXS4RA41S4FrAI%2F7liEbPpql7hgiU6m%2B9DruGEZtSEPej%2FfWgPn1jXcgaeoQlfHvunLh2%2BaML53Y1Wt1V5jK7LjemYicGpZp4yhXAQJR3QvlbpDK%2Bl3CuY08TtSmlKR4hP9JIbNGsv0WnNf3MDSwTjYRGx8O7sqymWEy%2FnNsq0uM85TZCUnCnyYA1Wca3WCpWOcIKKUm1a8HVkIZW50EMKXu1dYGOqUBr%2Bc5ExQuDOS0ESrDnMwzD3mlryTYNuzV%2F%2FdQAiw55VRSGZKF2bRVUStIDEudL%2FyUePHQsqik0caX6n7HjkoU427JubxAuhAi428wMBzCNLt1VV4bL9tvl4NPqrD5F492iud4UrgPGfpq23hO7QKx4Q2QQ%2FC8FHHRxrKqbsHtkXKgsNS8n3bp1l71cPa%2BguNhs2CJUpO3fcurwjc4W4pNBFLeHc%2BK&X-Amz-Signature=6030d35d483ef69834fd9e32fc13aad5991a52ff078ca8e3af5e3c62c813dade&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662YDXWLRC%2F20261019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261019T022155Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIQDWlSX26JR5XhSLe9qFBF0zzU1qAmEQCUHwnUM3puO41QIgX1a%2FqIlaOXbrtNFn3Mea8xsWiabfEwKtxvQOD3uMFaQq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDOmQaWkgX1XQSGmBtCrcA5GashUe2uMVUl0gPfd2%2FAZsae3PHhJk2MXNOuIkb2%2Foc%2FhpGpo2XoohQ6%2FJHDbYZ8AyX3G2c%2BIB4rhAFaVwDMBAe3dpgMBwO5p3N65CoLQi8H8sn4H6X%2FB9VVaNYAMa9LwZ87PkiZ8OrkdMd1zvHt2AYNUCY7w1JJvdg2gKyn8XqboUM4ch%2BHb0jyb0zbF9zYWqFmipMeE4f%2FTAOQYuBBiqSG%2Fd7S%2Bd59yzuyVT6NAq0S4YJdXQ3mC7D%2F%2BbOlEVWWTzGrFsnzRO4SogbQsSFv2Wj5UJyQAcSNYPAYA15vX%2F2aV%2BV%2FWj%2BgM1k8ndIGfNYM7rqxiTPwOLIXxO5L8FXO%2BZ3WxWEGBLvYWHWgbvnAFSil%2Fv0ljCgk5Pm8IWC1gFnkQs9biP%2BOWZUHiwMvS%2BP9fuvJ8HaqbY8yNjWSOVQXS4RA41S4FrAI%2F7liEbPpql7hgiU6m%2B9DruGEZtSEPej%2FfWgPn1jXcgaeoQlfHvunLh2%2BaML53Y1Wt1V5jK7LjemYicGpZp4yhXAQJR3QvlbpDK%2Bl3CuY08TtSmlKR4hP9JIbNGsv0WnNf3MDSwTjYRGx8O7sqymWEy%2FnNsq0uM85TZCUnCnyYA1Wca3WCpWOcIKKUm1a8HVkIZW50EMKXu1dYGOqUBr%2Bc5ExQuDOS0ESrDnMwzD3mlryTYNuzV%2F%2FdQAiw55VRSGZKF2bRVUStIDEudL%2FyUePHQsqik0caX6n7HjkoU427JubxAuhAi428wMBzCNLt1VV4bL9tvl4NPqrD5F492iud4UrgPGfpq23hO7QKx4Q2QQ%2FC8FHHRxrKqbsHtkXKgsNS8n3bp1l71cPa%2BguNhs2CJUpO3fcurwjc4W4pNBFLeHc%2BK&X-Amz-Signature=283641eae604e29f1576e8102e9a76d7437376a58c076dd576f702fd2e473638&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662YDXWLRC%2F20261019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261019T022155Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIQDWlSX26JR5XhSLe9qFBF0zzU1qAmEQCUHwnUM3puO41QIgX1a%2FqIlaOXbrtNFn3Mea8xsWiabfEwKtxvQOD3uMFaQq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDOmQaWkgX1XQSGmBtCrcA5GashUe2uMVUl0gPfd2%2FAZsae3PHhJk2MXNOuIkb2%2Foc%2FhpGpo2XoohQ6%2FJHDbYZ8AyX3G2c%2BIB4rhAFaVwDMBAe3dpgMBwO5p3N65CoLQi8H8sn4H6X%2FB9VVaNYAMa9LwZ87PkiZ8OrkdMd1zvHt2AYNUCY7w1JJvdg2gKyn8XqboUM4ch%2BHb0jyb0zbF9zYWqFmipMeE4f%2FTAOQYuBBiqSG%2Fd7S%2Bd59yzuyVT6NAq0S4YJdXQ3mC7D%2F%2BbOlEVWWTzGrFsnzRO4SogbQsSFv2Wj5UJyQAcSNYPAYA15vX%2F2aV%2BV%2FWj%2BgM1k8ndIGfNYM7rqxiTPwOLIXxO5L8FXO%2BZ3WxWEGBLvYWHWgbvnAFSil%2Fv0ljCgk5Pm8IWC1gFnkQs9biP%2BOWZUHiwMvS%2BP9fuvJ8HaqbY8yNjWSOVQXS4RA41S4FrAI%2F7liEbPpql7hgiU6m%2B9DruGEZtSEPej%2FfWgPn1jXcgaeoQlfHvunLh2%2BaML53Y1Wt1V5jK7LjemYicGpZp4yhXAQJR3QvlbpDK%2Bl3CuY08TtSmlKR4hP9JIbNGsv0WnNf3MDSwTjYRGx8O7sqymWEy%2FnNsq0uM85TZCUnCnyYA1Wca3WCpWOcIKKUm1a8HVkIZW50EMKXu1dYGOqUBr%2Bc5ExQuDOS0ESrDnMwzD3mlryTYNuzV%2F%2FdQAiw55VRSGZKF2bRVUStIDEudL%2FyUePHQsqik0caX6n7HjkoU427JubxAuhAi428wMBzCNLt1VV4bL9tvl4NPqrD5F492iud4UrgPGfpq23hO7QKx4Q2QQ%2FC8FHHRxrKqbsHtkXKgsNS8n3bp1l71cPa%2BguNhs2CJUpO3fcurwjc4W4pNBFLeHc%2BK&X-Amz-Signature=625544ab14da9fcd49e2facc47c95ad01acee3fef5c4b7e86e11717a17e1ef62&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
