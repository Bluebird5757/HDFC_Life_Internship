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
fetched_at: '2026-10-11T02:51:42.360Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMVGRFEZ%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDk2%2BnZzlid6Lu51oJ62quhUUeRZtfKlaK57jyf8yUXtgIhAIGxWSZB2qzj7s7d4Pp2SzDYErYhu1KJjY7vfvgnvlSSKv8DCGMQABoMNjM3NDIzMTgzODA1IgwZVAFGfyeXypHREBEq3APhtvSlruD468gkXlUS713w6ogXHVOM0tYqGCMHz7CJDNCHSlujKoXyPqWqxr%2BVq1WsxSIJjWWnBgLuAAMml2OksjYplYgiWr%2FvRa9pbckLfyY4E%2F759tR1NVkyKfsE4j81YYzJG%2Fp2D4PsH%2BeBhk2n9Iv5Pd5oWFsGsbWi8ItSza7PM3nGObAgtjM6qBJ6GUbmM7gie7yOAFIWdhRGNS8RxT9WTMyT8F%2F8XEBC%2Fh2ecap29EaQrN0uPnCHuyI0bDtupeo490lWU1f%2ByUqTADpCoYjRoAXMuHtYYr%2BM9GlWFwtda%2BehvG137ZijcgMZv0n3UUTUY5GMlyTt3rSAg7V%2F2rpqH1PTqu%2BoZBONHMMrERbKLRG1VsreXZm62CCvDmcfEE89KFOrndgfHSCEdQQSG3ttciONua0kJTHhpHanqQkwLm0pqJFu%2B86druszXyAXgs%2FIMKoaqqqAdsC0rT1y%2B%2FMIkuY7kjbHTz79HuVFX8LdOnDSQL5K7YcGsPho2rhWEsmRlMYmQmEd8JXPBsz3kB5ntPirSxbjXihjK%2BVdicrdkN4dA%2FbZQARVnRF%2FTe%2BhAM%2B25gtlgT6VD0PmRRi8ZzI2dLsVtlMDuOG%2FdGEJDSoDiS%2FwgPOjqz%2FuhzDA4avWBjqkAVlXusAv%2BL5%2FALSYWtW3jPgQAqm0zEKV19x0zGtuRZXlzNKtUTG%2F2NxQutRlj3syARERSGobiGpMhzNJeIV%2FEQiQKFBeGpnfkipJgfr2ue%2FeUR75kMc9nxspCUTdis9J2BFTdQ%2Bucj9fJOelzBUdCtpAAn2H1v7ZJyYNkQspVBNV1GO76dLDAzok5%2B%2Bz5%2BieuYRY148i%2BjMWcugWG5oaNvxjF0nH&X-Amz-Signature=ea2e670d5974e004f949306d9496c318dd060c1a5bf6755a10850060dc0491e5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMVGRFEZ%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDk2%2BnZzlid6Lu51oJ62quhUUeRZtfKlaK57jyf8yUXtgIhAIGxWSZB2qzj7s7d4Pp2SzDYErYhu1KJjY7vfvgnvlSSKv8DCGMQABoMNjM3NDIzMTgzODA1IgwZVAFGfyeXypHREBEq3APhtvSlruD468gkXlUS713w6ogXHVOM0tYqGCMHz7CJDNCHSlujKoXyPqWqxr%2BVq1WsxSIJjWWnBgLuAAMml2OksjYplYgiWr%2FvRa9pbckLfyY4E%2F759tR1NVkyKfsE4j81YYzJG%2Fp2D4PsH%2BeBhk2n9Iv5Pd5oWFsGsbWi8ItSza7PM3nGObAgtjM6qBJ6GUbmM7gie7yOAFIWdhRGNS8RxT9WTMyT8F%2F8XEBC%2Fh2ecap29EaQrN0uPnCHuyI0bDtupeo490lWU1f%2ByUqTADpCoYjRoAXMuHtYYr%2BM9GlWFwtda%2BehvG137ZijcgMZv0n3UUTUY5GMlyTt3rSAg7V%2F2rpqH1PTqu%2BoZBONHMMrERbKLRG1VsreXZm62CCvDmcfEE89KFOrndgfHSCEdQQSG3ttciONua0kJTHhpHanqQkwLm0pqJFu%2B86druszXyAXgs%2FIMKoaqqqAdsC0rT1y%2B%2FMIkuY7kjbHTz79HuVFX8LdOnDSQL5K7YcGsPho2rhWEsmRlMYmQmEd8JXPBsz3kB5ntPirSxbjXihjK%2BVdicrdkN4dA%2FbZQARVnRF%2FTe%2BhAM%2B25gtlgT6VD0PmRRi8ZzI2dLsVtlMDuOG%2FdGEJDSoDiS%2FwgPOjqz%2FuhzDA4avWBjqkAVlXusAv%2BL5%2FALSYWtW3jPgQAqm0zEKV19x0zGtuRZXlzNKtUTG%2F2NxQutRlj3syARERSGobiGpMhzNJeIV%2FEQiQKFBeGpnfkipJgfr2ue%2FeUR75kMc9nxspCUTdis9J2BFTdQ%2Bucj9fJOelzBUdCtpAAn2H1v7ZJyYNkQspVBNV1GO76dLDAzok5%2B%2Bz5%2BieuYRY148i%2BjMWcugWG5oaNvxjF0nH&X-Amz-Signature=a545f27f257cd4d624a06e49a575ee3a70bd42581d65365a1f71a2c08454511d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMVGRFEZ%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDk2%2BnZzlid6Lu51oJ62quhUUeRZtfKlaK57jyf8yUXtgIhAIGxWSZB2qzj7s7d4Pp2SzDYErYhu1KJjY7vfvgnvlSSKv8DCGMQABoMNjM3NDIzMTgzODA1IgwZVAFGfyeXypHREBEq3APhtvSlruD468gkXlUS713w6ogXHVOM0tYqGCMHz7CJDNCHSlujKoXyPqWqxr%2BVq1WsxSIJjWWnBgLuAAMml2OksjYplYgiWr%2FvRa9pbckLfyY4E%2F759tR1NVkyKfsE4j81YYzJG%2Fp2D4PsH%2BeBhk2n9Iv5Pd5oWFsGsbWi8ItSza7PM3nGObAgtjM6qBJ6GUbmM7gie7yOAFIWdhRGNS8RxT9WTMyT8F%2F8XEBC%2Fh2ecap29EaQrN0uPnCHuyI0bDtupeo490lWU1f%2ByUqTADpCoYjRoAXMuHtYYr%2BM9GlWFwtda%2BehvG137ZijcgMZv0n3UUTUY5GMlyTt3rSAg7V%2F2rpqH1PTqu%2BoZBONHMMrERbKLRG1VsreXZm62CCvDmcfEE89KFOrndgfHSCEdQQSG3ttciONua0kJTHhpHanqQkwLm0pqJFu%2B86druszXyAXgs%2FIMKoaqqqAdsC0rT1y%2B%2FMIkuY7kjbHTz79HuVFX8LdOnDSQL5K7YcGsPho2rhWEsmRlMYmQmEd8JXPBsz3kB5ntPirSxbjXihjK%2BVdicrdkN4dA%2FbZQARVnRF%2FTe%2BhAM%2B25gtlgT6VD0PmRRi8ZzI2dLsVtlMDuOG%2FdGEJDSoDiS%2FwgPOjqz%2FuhzDA4avWBjqkAVlXusAv%2BL5%2FALSYWtW3jPgQAqm0zEKV19x0zGtuRZXlzNKtUTG%2F2NxQutRlj3syARERSGobiGpMhzNJeIV%2FEQiQKFBeGpnfkipJgfr2ue%2FeUR75kMc9nxspCUTdis9J2BFTdQ%2Bucj9fJOelzBUdCtpAAn2H1v7ZJyYNkQspVBNV1GO76dLDAzok5%2B%2Bz5%2BieuYRY148i%2BjMWcugWG5oaNvxjF0nH&X-Amz-Signature=119a8435462616ee4d947581519dd0b226d083cdc7ce81b667ee7098c06c941d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMVGRFEZ%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDk2%2BnZzlid6Lu51oJ62quhUUeRZtfKlaK57jyf8yUXtgIhAIGxWSZB2qzj7s7d4Pp2SzDYErYhu1KJjY7vfvgnvlSSKv8DCGMQABoMNjM3NDIzMTgzODA1IgwZVAFGfyeXypHREBEq3APhtvSlruD468gkXlUS713w6ogXHVOM0tYqGCMHz7CJDNCHSlujKoXyPqWqxr%2BVq1WsxSIJjWWnBgLuAAMml2OksjYplYgiWr%2FvRa9pbckLfyY4E%2F759tR1NVkyKfsE4j81YYzJG%2Fp2D4PsH%2BeBhk2n9Iv5Pd5oWFsGsbWi8ItSza7PM3nGObAgtjM6qBJ6GUbmM7gie7yOAFIWdhRGNS8RxT9WTMyT8F%2F8XEBC%2Fh2ecap29EaQrN0uPnCHuyI0bDtupeo490lWU1f%2ByUqTADpCoYjRoAXMuHtYYr%2BM9GlWFwtda%2BehvG137ZijcgMZv0n3UUTUY5GMlyTt3rSAg7V%2F2rpqH1PTqu%2BoZBONHMMrERbKLRG1VsreXZm62CCvDmcfEE89KFOrndgfHSCEdQQSG3ttciONua0kJTHhpHanqQkwLm0pqJFu%2B86druszXyAXgs%2FIMKoaqqqAdsC0rT1y%2B%2FMIkuY7kjbHTz79HuVFX8LdOnDSQL5K7YcGsPho2rhWEsmRlMYmQmEd8JXPBsz3kB5ntPirSxbjXihjK%2BVdicrdkN4dA%2FbZQARVnRF%2FTe%2BhAM%2B25gtlgT6VD0PmRRi8ZzI2dLsVtlMDuOG%2FdGEJDSoDiS%2FwgPOjqz%2FuhzDA4avWBjqkAVlXusAv%2BL5%2FALSYWtW3jPgQAqm0zEKV19x0zGtuRZXlzNKtUTG%2F2NxQutRlj3syARERSGobiGpMhzNJeIV%2FEQiQKFBeGpnfkipJgfr2ue%2FeUR75kMc9nxspCUTdis9J2BFTdQ%2Bucj9fJOelzBUdCtpAAn2H1v7ZJyYNkQspVBNV1GO76dLDAzok5%2B%2Bz5%2BieuYRY148i%2BjMWcugWG5oaNvxjF0nH&X-Amz-Signature=8a86fcafa874491e37e5b418135026bda4b025eb343445ed8aff0e39486e1628&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMVGRFEZ%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDk2%2BnZzlid6Lu51oJ62quhUUeRZtfKlaK57jyf8yUXtgIhAIGxWSZB2qzj7s7d4Pp2SzDYErYhu1KJjY7vfvgnvlSSKv8DCGMQABoMNjM3NDIzMTgzODA1IgwZVAFGfyeXypHREBEq3APhtvSlruD468gkXlUS713w6ogXHVOM0tYqGCMHz7CJDNCHSlujKoXyPqWqxr%2BVq1WsxSIJjWWnBgLuAAMml2OksjYplYgiWr%2FvRa9pbckLfyY4E%2F759tR1NVkyKfsE4j81YYzJG%2Fp2D4PsH%2BeBhk2n9Iv5Pd5oWFsGsbWi8ItSza7PM3nGObAgtjM6qBJ6GUbmM7gie7yOAFIWdhRGNS8RxT9WTMyT8F%2F8XEBC%2Fh2ecap29EaQrN0uPnCHuyI0bDtupeo490lWU1f%2ByUqTADpCoYjRoAXMuHtYYr%2BM9GlWFwtda%2BehvG137ZijcgMZv0n3UUTUY5GMlyTt3rSAg7V%2F2rpqH1PTqu%2BoZBONHMMrERbKLRG1VsreXZm62CCvDmcfEE89KFOrndgfHSCEdQQSG3ttciONua0kJTHhpHanqQkwLm0pqJFu%2B86druszXyAXgs%2FIMKoaqqqAdsC0rT1y%2B%2FMIkuY7kjbHTz79HuVFX8LdOnDSQL5K7YcGsPho2rhWEsmRlMYmQmEd8JXPBsz3kB5ntPirSxbjXihjK%2BVdicrdkN4dA%2FbZQARVnRF%2FTe%2BhAM%2B25gtlgT6VD0PmRRi8ZzI2dLsVtlMDuOG%2FdGEJDSoDiS%2FwgPOjqz%2FuhzDA4avWBjqkAVlXusAv%2BL5%2FALSYWtW3jPgQAqm0zEKV19x0zGtuRZXlzNKtUTG%2F2NxQutRlj3syARERSGobiGpMhzNJeIV%2FEQiQKFBeGpnfkipJgfr2ue%2FeUR75kMc9nxspCUTdis9J2BFTdQ%2Bucj9fJOelzBUdCtpAAn2H1v7ZJyYNkQspVBNV1GO76dLDAzok5%2B%2Bz5%2BieuYRY148i%2BjMWcugWG5oaNvxjF0nH&X-Amz-Signature=8e98b28489f3a7d9ab3edb22e3868ccee25d25b3a649ebbea2b783ee69b00337&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMVGRFEZ%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDk2%2BnZzlid6Lu51oJ62quhUUeRZtfKlaK57jyf8yUXtgIhAIGxWSZB2qzj7s7d4Pp2SzDYErYhu1KJjY7vfvgnvlSSKv8DCGMQABoMNjM3NDIzMTgzODA1IgwZVAFGfyeXypHREBEq3APhtvSlruD468gkXlUS713w6ogXHVOM0tYqGCMHz7CJDNCHSlujKoXyPqWqxr%2BVq1WsxSIJjWWnBgLuAAMml2OksjYplYgiWr%2FvRa9pbckLfyY4E%2F759tR1NVkyKfsE4j81YYzJG%2Fp2D4PsH%2BeBhk2n9Iv5Pd5oWFsGsbWi8ItSza7PM3nGObAgtjM6qBJ6GUbmM7gie7yOAFIWdhRGNS8RxT9WTMyT8F%2F8XEBC%2Fh2ecap29EaQrN0uPnCHuyI0bDtupeo490lWU1f%2ByUqTADpCoYjRoAXMuHtYYr%2BM9GlWFwtda%2BehvG137ZijcgMZv0n3UUTUY5GMlyTt3rSAg7V%2F2rpqH1PTqu%2BoZBONHMMrERbKLRG1VsreXZm62CCvDmcfEE89KFOrndgfHSCEdQQSG3ttciONua0kJTHhpHanqQkwLm0pqJFu%2B86druszXyAXgs%2FIMKoaqqqAdsC0rT1y%2B%2FMIkuY7kjbHTz79HuVFX8LdOnDSQL5K7YcGsPho2rhWEsmRlMYmQmEd8JXPBsz3kB5ntPirSxbjXihjK%2BVdicrdkN4dA%2FbZQARVnRF%2FTe%2BhAM%2B25gtlgT6VD0PmRRi8ZzI2dLsVtlMDuOG%2FdGEJDSoDiS%2FwgPOjqz%2FuhzDA4avWBjqkAVlXusAv%2BL5%2FALSYWtW3jPgQAqm0zEKV19x0zGtuRZXlzNKtUTG%2F2NxQutRlj3syARERSGobiGpMhzNJeIV%2FEQiQKFBeGpnfkipJgfr2ue%2FeUR75kMc9nxspCUTdis9J2BFTdQ%2Bucj9fJOelzBUdCtpAAn2H1v7ZJyYNkQspVBNV1GO76dLDAzok5%2B%2Bz5%2BieuYRY148i%2BjMWcugWG5oaNvxjF0nH&X-Amz-Signature=eb976019d741e953b1d9498b91ea91c7dbf3186eb1845f04a53c659e90f97473&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMVGRFEZ%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDk2%2BnZzlid6Lu51oJ62quhUUeRZtfKlaK57jyf8yUXtgIhAIGxWSZB2qzj7s7d4Pp2SzDYErYhu1KJjY7vfvgnvlSSKv8DCGMQABoMNjM3NDIzMTgzODA1IgwZVAFGfyeXypHREBEq3APhtvSlruD468gkXlUS713w6ogXHVOM0tYqGCMHz7CJDNCHSlujKoXyPqWqxr%2BVq1WsxSIJjWWnBgLuAAMml2OksjYplYgiWr%2FvRa9pbckLfyY4E%2F759tR1NVkyKfsE4j81YYzJG%2Fp2D4PsH%2BeBhk2n9Iv5Pd5oWFsGsbWi8ItSza7PM3nGObAgtjM6qBJ6GUbmM7gie7yOAFIWdhRGNS8RxT9WTMyT8F%2F8XEBC%2Fh2ecap29EaQrN0uPnCHuyI0bDtupeo490lWU1f%2ByUqTADpCoYjRoAXMuHtYYr%2BM9GlWFwtda%2BehvG137ZijcgMZv0n3UUTUY5GMlyTt3rSAg7V%2F2rpqH1PTqu%2BoZBONHMMrERbKLRG1VsreXZm62CCvDmcfEE89KFOrndgfHSCEdQQSG3ttciONua0kJTHhpHanqQkwLm0pqJFu%2B86druszXyAXgs%2FIMKoaqqqAdsC0rT1y%2B%2FMIkuY7kjbHTz79HuVFX8LdOnDSQL5K7YcGsPho2rhWEsmRlMYmQmEd8JXPBsz3kB5ntPirSxbjXihjK%2BVdicrdkN4dA%2FbZQARVnRF%2FTe%2BhAM%2B25gtlgT6VD0PmRRi8ZzI2dLsVtlMDuOG%2FdGEJDSoDiS%2FwgPOjqz%2FuhzDA4avWBjqkAVlXusAv%2BL5%2FALSYWtW3jPgQAqm0zEKV19x0zGtuRZXlzNKtUTG%2F2NxQutRlj3syARERSGobiGpMhzNJeIV%2FEQiQKFBeGpnfkipJgfr2ue%2FeUR75kMc9nxspCUTdis9J2BFTdQ%2Bucj9fJOelzBUdCtpAAn2H1v7ZJyYNkQspVBNV1GO76dLDAzok5%2B%2Bz5%2BieuYRY148i%2BjMWcugWG5oaNvxjF0nH&X-Amz-Signature=fbca771180f467d26c03539ca887b941f276de06bcd17a98541cc021e0ffe322&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMVGRFEZ%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDk2%2BnZzlid6Lu51oJ62quhUUeRZtfKlaK57jyf8yUXtgIhAIGxWSZB2qzj7s7d4Pp2SzDYErYhu1KJjY7vfvgnvlSSKv8DCGMQABoMNjM3NDIzMTgzODA1IgwZVAFGfyeXypHREBEq3APhtvSlruD468gkXlUS713w6ogXHVOM0tYqGCMHz7CJDNCHSlujKoXyPqWqxr%2BVq1WsxSIJjWWnBgLuAAMml2OksjYplYgiWr%2FvRa9pbckLfyY4E%2F759tR1NVkyKfsE4j81YYzJG%2Fp2D4PsH%2BeBhk2n9Iv5Pd5oWFsGsbWi8ItSza7PM3nGObAgtjM6qBJ6GUbmM7gie7yOAFIWdhRGNS8RxT9WTMyT8F%2F8XEBC%2Fh2ecap29EaQrN0uPnCHuyI0bDtupeo490lWU1f%2ByUqTADpCoYjRoAXMuHtYYr%2BM9GlWFwtda%2BehvG137ZijcgMZv0n3UUTUY5GMlyTt3rSAg7V%2F2rpqH1PTqu%2BoZBONHMMrERbKLRG1VsreXZm62CCvDmcfEE89KFOrndgfHSCEdQQSG3ttciONua0kJTHhpHanqQkwLm0pqJFu%2B86druszXyAXgs%2FIMKoaqqqAdsC0rT1y%2B%2FMIkuY7kjbHTz79HuVFX8LdOnDSQL5K7YcGsPho2rhWEsmRlMYmQmEd8JXPBsz3kB5ntPirSxbjXihjK%2BVdicrdkN4dA%2FbZQARVnRF%2FTe%2BhAM%2B25gtlgT6VD0PmRRi8ZzI2dLsVtlMDuOG%2FdGEJDSoDiS%2FwgPOjqz%2FuhzDA4avWBjqkAVlXusAv%2BL5%2FALSYWtW3jPgQAqm0zEKV19x0zGtuRZXlzNKtUTG%2F2NxQutRlj3syARERSGobiGpMhzNJeIV%2FEQiQKFBeGpnfkipJgfr2ue%2FeUR75kMc9nxspCUTdis9J2BFTdQ%2Bucj9fJOelzBUdCtpAAn2H1v7ZJyYNkQspVBNV1GO76dLDAzok5%2B%2Bz5%2BieuYRY148i%2BjMWcugWG5oaNvxjF0nH&X-Amz-Signature=075645335d66fc37f06e4a7c09a5ea37bea14fecc131cb8f75bc39188081e05c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMVGRFEZ%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDk2%2BnZzlid6Lu51oJ62quhUUeRZtfKlaK57jyf8yUXtgIhAIGxWSZB2qzj7s7d4Pp2SzDYErYhu1KJjY7vfvgnvlSSKv8DCGMQABoMNjM3NDIzMTgzODA1IgwZVAFGfyeXypHREBEq3APhtvSlruD468gkXlUS713w6ogXHVOM0tYqGCMHz7CJDNCHSlujKoXyPqWqxr%2BVq1WsxSIJjWWnBgLuAAMml2OksjYplYgiWr%2FvRa9pbckLfyY4E%2F759tR1NVkyKfsE4j81YYzJG%2Fp2D4PsH%2BeBhk2n9Iv5Pd5oWFsGsbWi8ItSza7PM3nGObAgtjM6qBJ6GUbmM7gie7yOAFIWdhRGNS8RxT9WTMyT8F%2F8XEBC%2Fh2ecap29EaQrN0uPnCHuyI0bDtupeo490lWU1f%2ByUqTADpCoYjRoAXMuHtYYr%2BM9GlWFwtda%2BehvG137ZijcgMZv0n3UUTUY5GMlyTt3rSAg7V%2F2rpqH1PTqu%2BoZBONHMMrERbKLRG1VsreXZm62CCvDmcfEE89KFOrndgfHSCEdQQSG3ttciONua0kJTHhpHanqQkwLm0pqJFu%2B86druszXyAXgs%2FIMKoaqqqAdsC0rT1y%2B%2FMIkuY7kjbHTz79HuVFX8LdOnDSQL5K7YcGsPho2rhWEsmRlMYmQmEd8JXPBsz3kB5ntPirSxbjXihjK%2BVdicrdkN4dA%2FbZQARVnRF%2FTe%2BhAM%2B25gtlgT6VD0PmRRi8ZzI2dLsVtlMDuOG%2FdGEJDSoDiS%2FwgPOjqz%2FuhzDA4avWBjqkAVlXusAv%2BL5%2FALSYWtW3jPgQAqm0zEKV19x0zGtuRZXlzNKtUTG%2F2NxQutRlj3syARERSGobiGpMhzNJeIV%2FEQiQKFBeGpnfkipJgfr2ue%2FeUR75kMc9nxspCUTdis9J2BFTdQ%2Bucj9fJOelzBUdCtpAAn2H1v7ZJyYNkQspVBNV1GO76dLDAzok5%2B%2Bz5%2BieuYRY148i%2BjMWcugWG5oaNvxjF0nH&X-Amz-Signature=ef84e3c48d2f495845b355688c98033a36cd5df31bac131843689eb6516c044b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
