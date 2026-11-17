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
fetched_at: '2026-11-17T05:41:17.965Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667NPI53R%2F20261117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261117T054113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIByjDslMKXafTpQ%2F5n9YNEXURknnJ8JVc8pB%2BRleuVY0AiEA4IlnVKyYEFm0WlSMlriBv1iQwFxDlfZiom9SrijWg8YqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKyVIWepCz9a1xYYBSrcAy%2BeoNbGfP%2BLnWjz%2Bawqm%2FNa55xImDyDkmOGPjTWUkoGA882ws0lAnt0pdS16%2BqhGE%2BDc%2FR93a0WcpTNLkAfvyDFTOujWBk76gCOZ6Dvq5p8roFWBHhfDB%2Fqg2eKYBbDl3nKj1XClZGkXN17HECU9OdWMdvDPsOKBo87RcL4hB%2BJWDBUBpvr4x7OoNn0OjrrIUUElFhK6pB5DI6FwZZBblcwE9HzKJDG1aV6XcScSLX1kYqlJfrQJqY4ts0PCDAvdmKMkG85AhyUWhTrRw6b5LQW0s0tKuIYwKCNLBLvdHScnv2hlGH2BmgGe7jkePefUqghW2GbGIA%2FqBRPp9uEcZUcHqqrBbIOBbxAKJtG0QwUf8u9%2B3OhYI6gtVHRbBjdIDilbbUc4KAABBc2%2F4mP5s0sIowow16mZyM%2Fl97O5aKM9ebzWUWjYpmXY0k8T6qxf%2Ff7YbSCJWfXfR4yW6W0Gz6ONeN0Bj%2B1LLHemJ1kmH0cCQZ9GwVaaJhDk%2BOy5m4nUAFFuO%2Fnn0%2FHSCdYuMp%2BQFknGfL2jxA5lPKMXmOc2bJKKV8yaaHFtw9Uy9egK9%2FnfQYyyGQTpH9S5YIU8Lvc714XeE2fSjZ9damlik4WjVjK7JugEAMs22eD4N9qMO2579cGOqUBdaWS2WKSX1pkmZpezggQcyfQClZjdZ1soolCgTXxycDyGqwMaychtA%2B4sNQxbcZa6BkirxqQhqFY%2B1UM%2BTBCOOgEDCMyjE4yNonyPoJnQzWK6kUfMdwoe2fGlJEUDrAKKDWp%2F0zbGZyrXkgPOW63NYB%2FF13c6x6CPMUWOxUtba2NgMkBsMdA%2BIIHBidlJqlvom0quNKuxaHtZwQ0uOPzCjr7ZTjB&X-Amz-Signature=8f6f5910fd1fce548a3c4bb7db9077b48a8436c2d0145ff5756144fdda00d083&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667NPI53R%2F20261117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261117T054113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIByjDslMKXafTpQ%2F5n9YNEXURknnJ8JVc8pB%2BRleuVY0AiEA4IlnVKyYEFm0WlSMlriBv1iQwFxDlfZiom9SrijWg8YqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKyVIWepCz9a1xYYBSrcAy%2BeoNbGfP%2BLnWjz%2Bawqm%2FNa55xImDyDkmOGPjTWUkoGA882ws0lAnt0pdS16%2BqhGE%2BDc%2FR93a0WcpTNLkAfvyDFTOujWBk76gCOZ6Dvq5p8roFWBHhfDB%2Fqg2eKYBbDl3nKj1XClZGkXN17HECU9OdWMdvDPsOKBo87RcL4hB%2BJWDBUBpvr4x7OoNn0OjrrIUUElFhK6pB5DI6FwZZBblcwE9HzKJDG1aV6XcScSLX1kYqlJfrQJqY4ts0PCDAvdmKMkG85AhyUWhTrRw6b5LQW0s0tKuIYwKCNLBLvdHScnv2hlGH2BmgGe7jkePefUqghW2GbGIA%2FqBRPp9uEcZUcHqqrBbIOBbxAKJtG0QwUf8u9%2B3OhYI6gtVHRbBjdIDilbbUc4KAABBc2%2F4mP5s0sIowow16mZyM%2Fl97O5aKM9ebzWUWjYpmXY0k8T6qxf%2Ff7YbSCJWfXfR4yW6W0Gz6ONeN0Bj%2B1LLHemJ1kmH0cCQZ9GwVaaJhDk%2BOy5m4nUAFFuO%2Fnn0%2FHSCdYuMp%2BQFknGfL2jxA5lPKMXmOc2bJKKV8yaaHFtw9Uy9egK9%2FnfQYyyGQTpH9S5YIU8Lvc714XeE2fSjZ9damlik4WjVjK7JugEAMs22eD4N9qMO2579cGOqUBdaWS2WKSX1pkmZpezggQcyfQClZjdZ1soolCgTXxycDyGqwMaychtA%2B4sNQxbcZa6BkirxqQhqFY%2B1UM%2BTBCOOgEDCMyjE4yNonyPoJnQzWK6kUfMdwoe2fGlJEUDrAKKDWp%2F0zbGZyrXkgPOW63NYB%2FF13c6x6CPMUWOxUtba2NgMkBsMdA%2BIIHBidlJqlvom0quNKuxaHtZwQ0uOPzCjr7ZTjB&X-Amz-Signature=2198c80a4695abb87fdd6c0491c5b688b0b9c972c3fbccb760800b095664e53d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667NPI53R%2F20261117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261117T054113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIByjDslMKXafTpQ%2F5n9YNEXURknnJ8JVc8pB%2BRleuVY0AiEA4IlnVKyYEFm0WlSMlriBv1iQwFxDlfZiom9SrijWg8YqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKyVIWepCz9a1xYYBSrcAy%2BeoNbGfP%2BLnWjz%2Bawqm%2FNa55xImDyDkmOGPjTWUkoGA882ws0lAnt0pdS16%2BqhGE%2BDc%2FR93a0WcpTNLkAfvyDFTOujWBk76gCOZ6Dvq5p8roFWBHhfDB%2Fqg2eKYBbDl3nKj1XClZGkXN17HECU9OdWMdvDPsOKBo87RcL4hB%2BJWDBUBpvr4x7OoNn0OjrrIUUElFhK6pB5DI6FwZZBblcwE9HzKJDG1aV6XcScSLX1kYqlJfrQJqY4ts0PCDAvdmKMkG85AhyUWhTrRw6b5LQW0s0tKuIYwKCNLBLvdHScnv2hlGH2BmgGe7jkePefUqghW2GbGIA%2FqBRPp9uEcZUcHqqrBbIOBbxAKJtG0QwUf8u9%2B3OhYI6gtVHRbBjdIDilbbUc4KAABBc2%2F4mP5s0sIowow16mZyM%2Fl97O5aKM9ebzWUWjYpmXY0k8T6qxf%2Ff7YbSCJWfXfR4yW6W0Gz6ONeN0Bj%2B1LLHemJ1kmH0cCQZ9GwVaaJhDk%2BOy5m4nUAFFuO%2Fnn0%2FHSCdYuMp%2BQFknGfL2jxA5lPKMXmOc2bJKKV8yaaHFtw9Uy9egK9%2FnfQYyyGQTpH9S5YIU8Lvc714XeE2fSjZ9damlik4WjVjK7JugEAMs22eD4N9qMO2579cGOqUBdaWS2WKSX1pkmZpezggQcyfQClZjdZ1soolCgTXxycDyGqwMaychtA%2B4sNQxbcZa6BkirxqQhqFY%2B1UM%2BTBCOOgEDCMyjE4yNonyPoJnQzWK6kUfMdwoe2fGlJEUDrAKKDWp%2F0zbGZyrXkgPOW63NYB%2FF13c6x6CPMUWOxUtba2NgMkBsMdA%2BIIHBidlJqlvom0quNKuxaHtZwQ0uOPzCjr7ZTjB&X-Amz-Signature=72946d434beefb65b4806d96a27348bfaafa46f0e7940867975fd3734170db8f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667NPI53R%2F20261117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261117T054113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIByjDslMKXafTpQ%2F5n9YNEXURknnJ8JVc8pB%2BRleuVY0AiEA4IlnVKyYEFm0WlSMlriBv1iQwFxDlfZiom9SrijWg8YqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKyVIWepCz9a1xYYBSrcAy%2BeoNbGfP%2BLnWjz%2Bawqm%2FNa55xImDyDkmOGPjTWUkoGA882ws0lAnt0pdS16%2BqhGE%2BDc%2FR93a0WcpTNLkAfvyDFTOujWBk76gCOZ6Dvq5p8roFWBHhfDB%2Fqg2eKYBbDl3nKj1XClZGkXN17HECU9OdWMdvDPsOKBo87RcL4hB%2BJWDBUBpvr4x7OoNn0OjrrIUUElFhK6pB5DI6FwZZBblcwE9HzKJDG1aV6XcScSLX1kYqlJfrQJqY4ts0PCDAvdmKMkG85AhyUWhTrRw6b5LQW0s0tKuIYwKCNLBLvdHScnv2hlGH2BmgGe7jkePefUqghW2GbGIA%2FqBRPp9uEcZUcHqqrBbIOBbxAKJtG0QwUf8u9%2B3OhYI6gtVHRbBjdIDilbbUc4KAABBc2%2F4mP5s0sIowow16mZyM%2Fl97O5aKM9ebzWUWjYpmXY0k8T6qxf%2Ff7YbSCJWfXfR4yW6W0Gz6ONeN0Bj%2B1LLHemJ1kmH0cCQZ9GwVaaJhDk%2BOy5m4nUAFFuO%2Fnn0%2FHSCdYuMp%2BQFknGfL2jxA5lPKMXmOc2bJKKV8yaaHFtw9Uy9egK9%2FnfQYyyGQTpH9S5YIU8Lvc714XeE2fSjZ9damlik4WjVjK7JugEAMs22eD4N9qMO2579cGOqUBdaWS2WKSX1pkmZpezggQcyfQClZjdZ1soolCgTXxycDyGqwMaychtA%2B4sNQxbcZa6BkirxqQhqFY%2B1UM%2BTBCOOgEDCMyjE4yNonyPoJnQzWK6kUfMdwoe2fGlJEUDrAKKDWp%2F0zbGZyrXkgPOW63NYB%2FF13c6x6CPMUWOxUtba2NgMkBsMdA%2BIIHBidlJqlvom0quNKuxaHtZwQ0uOPzCjr7ZTjB&X-Amz-Signature=9a69fb7cf56d0fb133446cde93800acb7a369cc680f4ef95b7131d104a513490&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667NPI53R%2F20261117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261117T054113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIByjDslMKXafTpQ%2F5n9YNEXURknnJ8JVc8pB%2BRleuVY0AiEA4IlnVKyYEFm0WlSMlriBv1iQwFxDlfZiom9SrijWg8YqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKyVIWepCz9a1xYYBSrcAy%2BeoNbGfP%2BLnWjz%2Bawqm%2FNa55xImDyDkmOGPjTWUkoGA882ws0lAnt0pdS16%2BqhGE%2BDc%2FR93a0WcpTNLkAfvyDFTOujWBk76gCOZ6Dvq5p8roFWBHhfDB%2Fqg2eKYBbDl3nKj1XClZGkXN17HECU9OdWMdvDPsOKBo87RcL4hB%2BJWDBUBpvr4x7OoNn0OjrrIUUElFhK6pB5DI6FwZZBblcwE9HzKJDG1aV6XcScSLX1kYqlJfrQJqY4ts0PCDAvdmKMkG85AhyUWhTrRw6b5LQW0s0tKuIYwKCNLBLvdHScnv2hlGH2BmgGe7jkePefUqghW2GbGIA%2FqBRPp9uEcZUcHqqrBbIOBbxAKJtG0QwUf8u9%2B3OhYI6gtVHRbBjdIDilbbUc4KAABBc2%2F4mP5s0sIowow16mZyM%2Fl97O5aKM9ebzWUWjYpmXY0k8T6qxf%2Ff7YbSCJWfXfR4yW6W0Gz6ONeN0Bj%2B1LLHemJ1kmH0cCQZ9GwVaaJhDk%2BOy5m4nUAFFuO%2Fnn0%2FHSCdYuMp%2BQFknGfL2jxA5lPKMXmOc2bJKKV8yaaHFtw9Uy9egK9%2FnfQYyyGQTpH9S5YIU8Lvc714XeE2fSjZ9damlik4WjVjK7JugEAMs22eD4N9qMO2579cGOqUBdaWS2WKSX1pkmZpezggQcyfQClZjdZ1soolCgTXxycDyGqwMaychtA%2B4sNQxbcZa6BkirxqQhqFY%2B1UM%2BTBCOOgEDCMyjE4yNonyPoJnQzWK6kUfMdwoe2fGlJEUDrAKKDWp%2F0zbGZyrXkgPOW63NYB%2FF13c6x6CPMUWOxUtba2NgMkBsMdA%2BIIHBidlJqlvom0quNKuxaHtZwQ0uOPzCjr7ZTjB&X-Amz-Signature=2577890867be61c3b8adcbf3cd097c89feb16c18d76eb4a34ffd7adf726e897c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667NPI53R%2F20261117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261117T054113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIByjDslMKXafTpQ%2F5n9YNEXURknnJ8JVc8pB%2BRleuVY0AiEA4IlnVKyYEFm0WlSMlriBv1iQwFxDlfZiom9SrijWg8YqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKyVIWepCz9a1xYYBSrcAy%2BeoNbGfP%2BLnWjz%2Bawqm%2FNa55xImDyDkmOGPjTWUkoGA882ws0lAnt0pdS16%2BqhGE%2BDc%2FR93a0WcpTNLkAfvyDFTOujWBk76gCOZ6Dvq5p8roFWBHhfDB%2Fqg2eKYBbDl3nKj1XClZGkXN17HECU9OdWMdvDPsOKBo87RcL4hB%2BJWDBUBpvr4x7OoNn0OjrrIUUElFhK6pB5DI6FwZZBblcwE9HzKJDG1aV6XcScSLX1kYqlJfrQJqY4ts0PCDAvdmKMkG85AhyUWhTrRw6b5LQW0s0tKuIYwKCNLBLvdHScnv2hlGH2BmgGe7jkePefUqghW2GbGIA%2FqBRPp9uEcZUcHqqrBbIOBbxAKJtG0QwUf8u9%2B3OhYI6gtVHRbBjdIDilbbUc4KAABBc2%2F4mP5s0sIowow16mZyM%2Fl97O5aKM9ebzWUWjYpmXY0k8T6qxf%2Ff7YbSCJWfXfR4yW6W0Gz6ONeN0Bj%2B1LLHemJ1kmH0cCQZ9GwVaaJhDk%2BOy5m4nUAFFuO%2Fnn0%2FHSCdYuMp%2BQFknGfL2jxA5lPKMXmOc2bJKKV8yaaHFtw9Uy9egK9%2FnfQYyyGQTpH9S5YIU8Lvc714XeE2fSjZ9damlik4WjVjK7JugEAMs22eD4N9qMO2579cGOqUBdaWS2WKSX1pkmZpezggQcyfQClZjdZ1soolCgTXxycDyGqwMaychtA%2B4sNQxbcZa6BkirxqQhqFY%2B1UM%2BTBCOOgEDCMyjE4yNonyPoJnQzWK6kUfMdwoe2fGlJEUDrAKKDWp%2F0zbGZyrXkgPOW63NYB%2FF13c6x6CPMUWOxUtba2NgMkBsMdA%2BIIHBidlJqlvom0quNKuxaHtZwQ0uOPzCjr7ZTjB&X-Amz-Signature=f3c102f69c6007a7d682577d651ebe63ca88ebea5f0436b48e16877981eb46e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667NPI53R%2F20261117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261117T054113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIByjDslMKXafTpQ%2F5n9YNEXURknnJ8JVc8pB%2BRleuVY0AiEA4IlnVKyYEFm0WlSMlriBv1iQwFxDlfZiom9SrijWg8YqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKyVIWepCz9a1xYYBSrcAy%2BeoNbGfP%2BLnWjz%2Bawqm%2FNa55xImDyDkmOGPjTWUkoGA882ws0lAnt0pdS16%2BqhGE%2BDc%2FR93a0WcpTNLkAfvyDFTOujWBk76gCOZ6Dvq5p8roFWBHhfDB%2Fqg2eKYBbDl3nKj1XClZGkXN17HECU9OdWMdvDPsOKBo87RcL4hB%2BJWDBUBpvr4x7OoNn0OjrrIUUElFhK6pB5DI6FwZZBblcwE9HzKJDG1aV6XcScSLX1kYqlJfrQJqY4ts0PCDAvdmKMkG85AhyUWhTrRw6b5LQW0s0tKuIYwKCNLBLvdHScnv2hlGH2BmgGe7jkePefUqghW2GbGIA%2FqBRPp9uEcZUcHqqrBbIOBbxAKJtG0QwUf8u9%2B3OhYI6gtVHRbBjdIDilbbUc4KAABBc2%2F4mP5s0sIowow16mZyM%2Fl97O5aKM9ebzWUWjYpmXY0k8T6qxf%2Ff7YbSCJWfXfR4yW6W0Gz6ONeN0Bj%2B1LLHemJ1kmH0cCQZ9GwVaaJhDk%2BOy5m4nUAFFuO%2Fnn0%2FHSCdYuMp%2BQFknGfL2jxA5lPKMXmOc2bJKKV8yaaHFtw9Uy9egK9%2FnfQYyyGQTpH9S5YIU8Lvc714XeE2fSjZ9damlik4WjVjK7JugEAMs22eD4N9qMO2579cGOqUBdaWS2WKSX1pkmZpezggQcyfQClZjdZ1soolCgTXxycDyGqwMaychtA%2B4sNQxbcZa6BkirxqQhqFY%2B1UM%2BTBCOOgEDCMyjE4yNonyPoJnQzWK6kUfMdwoe2fGlJEUDrAKKDWp%2F0zbGZyrXkgPOW63NYB%2FF13c6x6CPMUWOxUtba2NgMkBsMdA%2BIIHBidlJqlvom0quNKuxaHtZwQ0uOPzCjr7ZTjB&X-Amz-Signature=4f38bf3e3d985dbc274627559d2a7a083ae15343e05bc0dae02981dc6e661a1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667NPI53R%2F20261117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261117T054113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIByjDslMKXafTpQ%2F5n9YNEXURknnJ8JVc8pB%2BRleuVY0AiEA4IlnVKyYEFm0WlSMlriBv1iQwFxDlfZiom9SrijWg8YqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKyVIWepCz9a1xYYBSrcAy%2BeoNbGfP%2BLnWjz%2Bawqm%2FNa55xImDyDkmOGPjTWUkoGA882ws0lAnt0pdS16%2BqhGE%2BDc%2FR93a0WcpTNLkAfvyDFTOujWBk76gCOZ6Dvq5p8roFWBHhfDB%2Fqg2eKYBbDl3nKj1XClZGkXN17HECU9OdWMdvDPsOKBo87RcL4hB%2BJWDBUBpvr4x7OoNn0OjrrIUUElFhK6pB5DI6FwZZBblcwE9HzKJDG1aV6XcScSLX1kYqlJfrQJqY4ts0PCDAvdmKMkG85AhyUWhTrRw6b5LQW0s0tKuIYwKCNLBLvdHScnv2hlGH2BmgGe7jkePefUqghW2GbGIA%2FqBRPp9uEcZUcHqqrBbIOBbxAKJtG0QwUf8u9%2B3OhYI6gtVHRbBjdIDilbbUc4KAABBc2%2F4mP5s0sIowow16mZyM%2Fl97O5aKM9ebzWUWjYpmXY0k8T6qxf%2Ff7YbSCJWfXfR4yW6W0Gz6ONeN0Bj%2B1LLHemJ1kmH0cCQZ9GwVaaJhDk%2BOy5m4nUAFFuO%2Fnn0%2FHSCdYuMp%2BQFknGfL2jxA5lPKMXmOc2bJKKV8yaaHFtw9Uy9egK9%2FnfQYyyGQTpH9S5YIU8Lvc714XeE2fSjZ9damlik4WjVjK7JugEAMs22eD4N9qMO2579cGOqUBdaWS2WKSX1pkmZpezggQcyfQClZjdZ1soolCgTXxycDyGqwMaychtA%2B4sNQxbcZa6BkirxqQhqFY%2B1UM%2BTBCOOgEDCMyjE4yNonyPoJnQzWK6kUfMdwoe2fGlJEUDrAKKDWp%2F0zbGZyrXkgPOW63NYB%2FF13c6x6CPMUWOxUtba2NgMkBsMdA%2BIIHBidlJqlvom0quNKuxaHtZwQ0uOPzCjr7ZTjB&X-Amz-Signature=08cec0e303fa48220e22cc78fcdaad750940ecef2fb9d5e5a2a751e48472d263&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667NPI53R%2F20261117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261117T054113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIByjDslMKXafTpQ%2F5n9YNEXURknnJ8JVc8pB%2BRleuVY0AiEA4IlnVKyYEFm0WlSMlriBv1iQwFxDlfZiom9SrijWg8YqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKyVIWepCz9a1xYYBSrcAy%2BeoNbGfP%2BLnWjz%2Bawqm%2FNa55xImDyDkmOGPjTWUkoGA882ws0lAnt0pdS16%2BqhGE%2BDc%2FR93a0WcpTNLkAfvyDFTOujWBk76gCOZ6Dvq5p8roFWBHhfDB%2Fqg2eKYBbDl3nKj1XClZGkXN17HECU9OdWMdvDPsOKBo87RcL4hB%2BJWDBUBpvr4x7OoNn0OjrrIUUElFhK6pB5DI6FwZZBblcwE9HzKJDG1aV6XcScSLX1kYqlJfrQJqY4ts0PCDAvdmKMkG85AhyUWhTrRw6b5LQW0s0tKuIYwKCNLBLvdHScnv2hlGH2BmgGe7jkePefUqghW2GbGIA%2FqBRPp9uEcZUcHqqrBbIOBbxAKJtG0QwUf8u9%2B3OhYI6gtVHRbBjdIDilbbUc4KAABBc2%2F4mP5s0sIowow16mZyM%2Fl97O5aKM9ebzWUWjYpmXY0k8T6qxf%2Ff7YbSCJWfXfR4yW6W0Gz6ONeN0Bj%2B1LLHemJ1kmH0cCQZ9GwVaaJhDk%2BOy5m4nUAFFuO%2Fnn0%2FHSCdYuMp%2BQFknGfL2jxA5lPKMXmOc2bJKKV8yaaHFtw9Uy9egK9%2FnfQYyyGQTpH9S5YIU8Lvc714XeE2fSjZ9damlik4WjVjK7JugEAMs22eD4N9qMO2579cGOqUBdaWS2WKSX1pkmZpezggQcyfQClZjdZ1soolCgTXxycDyGqwMaychtA%2B4sNQxbcZa6BkirxqQhqFY%2B1UM%2BTBCOOgEDCMyjE4yNonyPoJnQzWK6kUfMdwoe2fGlJEUDrAKKDWp%2F0zbGZyrXkgPOW63NYB%2FF13c6x6CPMUWOxUtba2NgMkBsMdA%2BIIHBidlJqlvom0quNKuxaHtZwQ0uOPzCjr7ZTjB&X-Amz-Signature=c549397fa5c0dc968c9a12015fb22ba6ced04ba6ea3a2eec2abd200821cb6971&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
