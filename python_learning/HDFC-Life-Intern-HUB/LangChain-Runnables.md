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
fetched_at: '2026-09-01T02:35:27.304Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZEKB4JO%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023520Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvByke482WxiOlRTkMe1dERCYxpVfnnkGXY7SX%2Fl51kwIgIjBReGpMa73awVG9jB0BMaCHbqEyfEw1B5SU6Qj%2FslIqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDZP%2B5f%2FNsQkoFh2eSrcA7i1g9kJq%2FSsALWDMSy7hBWmy92iG1mrRZsAARNGZUWSDa7PcmxdqXrfgmQhrJwwk8dwAay8ieQIdofTaJ1ThpJIWLH0S%2B4xqptF34EEctOyocHZ664%2F7N7MiyHMKA7%2B40EONSF0BIVEX8rpCledXVidRt2qsAdfAibSVZ%2BuSpyauj7YB8tklYqsx1wx1QPVLODKqUxhbzqzYb3KKm8X5lA9pug16DFzHsynuXzWEsxcVQndHJOpnjPQF2aAztmvJ%2FlI05RVqHZinFfAESwtp%2Ff%2BcHDUbHR%2BUemPpaLuLko6nKRYsltfgBEI1ghkYVqjD%2BV5%2F5nZ8eYc3cwkZizL5Pa%2FczkwEwjDXFaI1G9w3sf25bdWMYyt8PSgdxLRMER2TFLIC6TcjzoZybJDg%2BFywiFLNXthbji2hdymg18fumzO3KLRBXjAWK5GT5%2Fe3wzzg0bT6pCO7baB%2FPgqeEwrcsvLP%2FndiEZOnXvkPjmLGJ4G50PYXaqo17bu0kzDkz30l75BTacJEX586qheDqr2tkdi2veBvWCqaZ%2FJXIYCaTNi%2BJkg9pTf96bBr%2FoYN5lB1BUQU09y8cDDj6v66w6bBIy%2BP2NFIjWApD4de4bWTF%2BCKTxem%2BiS%2FGlsOadXMLLc2NQGOqUBRUbeKWA83YuLnt30dpZHcvgChc6kYJG5Cm3Wh5uRaDtekEmA8aBuY3sMqsr4IuM5mVmWSABTPS2WK6rIhF842dgYgTRvJ1c%2B8uFMezBq%2B7rcoVd3zjrc3raLszog3Nb33BzKUueb4CawG8BfZsKBGP%2BJBmj%2FL%2FE1PjfZaEHCxdke725cnX3aDjRS%2FGDgC1uA5f3nvmvPIme1ZmHblYTCfOFnByXP&X-Amz-Signature=b3dd049195b6cce518f5a7f6fa471f444ab114488d64f55bd253791aa70b4183&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZEKB4JO%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023520Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvByke482WxiOlRTkMe1dERCYxpVfnnkGXY7SX%2Fl51kwIgIjBReGpMa73awVG9jB0BMaCHbqEyfEw1B5SU6Qj%2FslIqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDZP%2B5f%2FNsQkoFh2eSrcA7i1g9kJq%2FSsALWDMSy7hBWmy92iG1mrRZsAARNGZUWSDa7PcmxdqXrfgmQhrJwwk8dwAay8ieQIdofTaJ1ThpJIWLH0S%2B4xqptF34EEctOyocHZ664%2F7N7MiyHMKA7%2B40EONSF0BIVEX8rpCledXVidRt2qsAdfAibSVZ%2BuSpyauj7YB8tklYqsx1wx1QPVLODKqUxhbzqzYb3KKm8X5lA9pug16DFzHsynuXzWEsxcVQndHJOpnjPQF2aAztmvJ%2FlI05RVqHZinFfAESwtp%2Ff%2BcHDUbHR%2BUemPpaLuLko6nKRYsltfgBEI1ghkYVqjD%2BV5%2F5nZ8eYc3cwkZizL5Pa%2FczkwEwjDXFaI1G9w3sf25bdWMYyt8PSgdxLRMER2TFLIC6TcjzoZybJDg%2BFywiFLNXthbji2hdymg18fumzO3KLRBXjAWK5GT5%2Fe3wzzg0bT6pCO7baB%2FPgqeEwrcsvLP%2FndiEZOnXvkPjmLGJ4G50PYXaqo17bu0kzDkz30l75BTacJEX586qheDqr2tkdi2veBvWCqaZ%2FJXIYCaTNi%2BJkg9pTf96bBr%2FoYN5lB1BUQU09y8cDDj6v66w6bBIy%2BP2NFIjWApD4de4bWTF%2BCKTxem%2BiS%2FGlsOadXMLLc2NQGOqUBRUbeKWA83YuLnt30dpZHcvgChc6kYJG5Cm3Wh5uRaDtekEmA8aBuY3sMqsr4IuM5mVmWSABTPS2WK6rIhF842dgYgTRvJ1c%2B8uFMezBq%2B7rcoVd3zjrc3raLszog3Nb33BzKUueb4CawG8BfZsKBGP%2BJBmj%2FL%2FE1PjfZaEHCxdke725cnX3aDjRS%2FGDgC1uA5f3nvmvPIme1ZmHblYTCfOFnByXP&X-Amz-Signature=2a1edf0a867c3ced7c5822db00bf41550eb07a7a63dd60fed9b004bdac33aca1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZEKB4JO%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023520Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvByke482WxiOlRTkMe1dERCYxpVfnnkGXY7SX%2Fl51kwIgIjBReGpMa73awVG9jB0BMaCHbqEyfEw1B5SU6Qj%2FslIqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDZP%2B5f%2FNsQkoFh2eSrcA7i1g9kJq%2FSsALWDMSy7hBWmy92iG1mrRZsAARNGZUWSDa7PcmxdqXrfgmQhrJwwk8dwAay8ieQIdofTaJ1ThpJIWLH0S%2B4xqptF34EEctOyocHZ664%2F7N7MiyHMKA7%2B40EONSF0BIVEX8rpCledXVidRt2qsAdfAibSVZ%2BuSpyauj7YB8tklYqsx1wx1QPVLODKqUxhbzqzYb3KKm8X5lA9pug16DFzHsynuXzWEsxcVQndHJOpnjPQF2aAztmvJ%2FlI05RVqHZinFfAESwtp%2Ff%2BcHDUbHR%2BUemPpaLuLko6nKRYsltfgBEI1ghkYVqjD%2BV5%2F5nZ8eYc3cwkZizL5Pa%2FczkwEwjDXFaI1G9w3sf25bdWMYyt8PSgdxLRMER2TFLIC6TcjzoZybJDg%2BFywiFLNXthbji2hdymg18fumzO3KLRBXjAWK5GT5%2Fe3wzzg0bT6pCO7baB%2FPgqeEwrcsvLP%2FndiEZOnXvkPjmLGJ4G50PYXaqo17bu0kzDkz30l75BTacJEX586qheDqr2tkdi2veBvWCqaZ%2FJXIYCaTNi%2BJkg9pTf96bBr%2FoYN5lB1BUQU09y8cDDj6v66w6bBIy%2BP2NFIjWApD4de4bWTF%2BCKTxem%2BiS%2FGlsOadXMLLc2NQGOqUBRUbeKWA83YuLnt30dpZHcvgChc6kYJG5Cm3Wh5uRaDtekEmA8aBuY3sMqsr4IuM5mVmWSABTPS2WK6rIhF842dgYgTRvJ1c%2B8uFMezBq%2B7rcoVd3zjrc3raLszog3Nb33BzKUueb4CawG8BfZsKBGP%2BJBmj%2FL%2FE1PjfZaEHCxdke725cnX3aDjRS%2FGDgC1uA5f3nvmvPIme1ZmHblYTCfOFnByXP&X-Amz-Signature=e9743f05bf44c42a0a3bb8bf8b88219bcd1e7f9c92f735a5e58495576068f4ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZEKB4JO%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023520Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvByke482WxiOlRTkMe1dERCYxpVfnnkGXY7SX%2Fl51kwIgIjBReGpMa73awVG9jB0BMaCHbqEyfEw1B5SU6Qj%2FslIqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDZP%2B5f%2FNsQkoFh2eSrcA7i1g9kJq%2FSsALWDMSy7hBWmy92iG1mrRZsAARNGZUWSDa7PcmxdqXrfgmQhrJwwk8dwAay8ieQIdofTaJ1ThpJIWLH0S%2B4xqptF34EEctOyocHZ664%2F7N7MiyHMKA7%2B40EONSF0BIVEX8rpCledXVidRt2qsAdfAibSVZ%2BuSpyauj7YB8tklYqsx1wx1QPVLODKqUxhbzqzYb3KKm8X5lA9pug16DFzHsynuXzWEsxcVQndHJOpnjPQF2aAztmvJ%2FlI05RVqHZinFfAESwtp%2Ff%2BcHDUbHR%2BUemPpaLuLko6nKRYsltfgBEI1ghkYVqjD%2BV5%2F5nZ8eYc3cwkZizL5Pa%2FczkwEwjDXFaI1G9w3sf25bdWMYyt8PSgdxLRMER2TFLIC6TcjzoZybJDg%2BFywiFLNXthbji2hdymg18fumzO3KLRBXjAWK5GT5%2Fe3wzzg0bT6pCO7baB%2FPgqeEwrcsvLP%2FndiEZOnXvkPjmLGJ4G50PYXaqo17bu0kzDkz30l75BTacJEX586qheDqr2tkdi2veBvWCqaZ%2FJXIYCaTNi%2BJkg9pTf96bBr%2FoYN5lB1BUQU09y8cDDj6v66w6bBIy%2BP2NFIjWApD4de4bWTF%2BCKTxem%2BiS%2FGlsOadXMLLc2NQGOqUBRUbeKWA83YuLnt30dpZHcvgChc6kYJG5Cm3Wh5uRaDtekEmA8aBuY3sMqsr4IuM5mVmWSABTPS2WK6rIhF842dgYgTRvJ1c%2B8uFMezBq%2B7rcoVd3zjrc3raLszog3Nb33BzKUueb4CawG8BfZsKBGP%2BJBmj%2FL%2FE1PjfZaEHCxdke725cnX3aDjRS%2FGDgC1uA5f3nvmvPIme1ZmHblYTCfOFnByXP&X-Amz-Signature=2ba4e804237dfb07b0e827c970ad44dd51152fd0abd6bbf0f23e507a0a551d66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZEKB4JO%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023520Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvByke482WxiOlRTkMe1dERCYxpVfnnkGXY7SX%2Fl51kwIgIjBReGpMa73awVG9jB0BMaCHbqEyfEw1B5SU6Qj%2FslIqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDZP%2B5f%2FNsQkoFh2eSrcA7i1g9kJq%2FSsALWDMSy7hBWmy92iG1mrRZsAARNGZUWSDa7PcmxdqXrfgmQhrJwwk8dwAay8ieQIdofTaJ1ThpJIWLH0S%2B4xqptF34EEctOyocHZ664%2F7N7MiyHMKA7%2B40EONSF0BIVEX8rpCledXVidRt2qsAdfAibSVZ%2BuSpyauj7YB8tklYqsx1wx1QPVLODKqUxhbzqzYb3KKm8X5lA9pug16DFzHsynuXzWEsxcVQndHJOpnjPQF2aAztmvJ%2FlI05RVqHZinFfAESwtp%2Ff%2BcHDUbHR%2BUemPpaLuLko6nKRYsltfgBEI1ghkYVqjD%2BV5%2F5nZ8eYc3cwkZizL5Pa%2FczkwEwjDXFaI1G9w3sf25bdWMYyt8PSgdxLRMER2TFLIC6TcjzoZybJDg%2BFywiFLNXthbji2hdymg18fumzO3KLRBXjAWK5GT5%2Fe3wzzg0bT6pCO7baB%2FPgqeEwrcsvLP%2FndiEZOnXvkPjmLGJ4G50PYXaqo17bu0kzDkz30l75BTacJEX586qheDqr2tkdi2veBvWCqaZ%2FJXIYCaTNi%2BJkg9pTf96bBr%2FoYN5lB1BUQU09y8cDDj6v66w6bBIy%2BP2NFIjWApD4de4bWTF%2BCKTxem%2BiS%2FGlsOadXMLLc2NQGOqUBRUbeKWA83YuLnt30dpZHcvgChc6kYJG5Cm3Wh5uRaDtekEmA8aBuY3sMqsr4IuM5mVmWSABTPS2WK6rIhF842dgYgTRvJ1c%2B8uFMezBq%2B7rcoVd3zjrc3raLszog3Nb33BzKUueb4CawG8BfZsKBGP%2BJBmj%2FL%2FE1PjfZaEHCxdke725cnX3aDjRS%2FGDgC1uA5f3nvmvPIme1ZmHblYTCfOFnByXP&X-Amz-Signature=1bf2992afa7dd345aca41dcece7eb513a8d6d661469e620a34a6f01457760400&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZEKB4JO%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023520Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvByke482WxiOlRTkMe1dERCYxpVfnnkGXY7SX%2Fl51kwIgIjBReGpMa73awVG9jB0BMaCHbqEyfEw1B5SU6Qj%2FslIqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDZP%2B5f%2FNsQkoFh2eSrcA7i1g9kJq%2FSsALWDMSy7hBWmy92iG1mrRZsAARNGZUWSDa7PcmxdqXrfgmQhrJwwk8dwAay8ieQIdofTaJ1ThpJIWLH0S%2B4xqptF34EEctOyocHZ664%2F7N7MiyHMKA7%2B40EONSF0BIVEX8rpCledXVidRt2qsAdfAibSVZ%2BuSpyauj7YB8tklYqsx1wx1QPVLODKqUxhbzqzYb3KKm8X5lA9pug16DFzHsynuXzWEsxcVQndHJOpnjPQF2aAztmvJ%2FlI05RVqHZinFfAESwtp%2Ff%2BcHDUbHR%2BUemPpaLuLko6nKRYsltfgBEI1ghkYVqjD%2BV5%2F5nZ8eYc3cwkZizL5Pa%2FczkwEwjDXFaI1G9w3sf25bdWMYyt8PSgdxLRMER2TFLIC6TcjzoZybJDg%2BFywiFLNXthbji2hdymg18fumzO3KLRBXjAWK5GT5%2Fe3wzzg0bT6pCO7baB%2FPgqeEwrcsvLP%2FndiEZOnXvkPjmLGJ4G50PYXaqo17bu0kzDkz30l75BTacJEX586qheDqr2tkdi2veBvWCqaZ%2FJXIYCaTNi%2BJkg9pTf96bBr%2FoYN5lB1BUQU09y8cDDj6v66w6bBIy%2BP2NFIjWApD4de4bWTF%2BCKTxem%2BiS%2FGlsOadXMLLc2NQGOqUBRUbeKWA83YuLnt30dpZHcvgChc6kYJG5Cm3Wh5uRaDtekEmA8aBuY3sMqsr4IuM5mVmWSABTPS2WK6rIhF842dgYgTRvJ1c%2B8uFMezBq%2B7rcoVd3zjrc3raLszog3Nb33BzKUueb4CawG8BfZsKBGP%2BJBmj%2FL%2FE1PjfZaEHCxdke725cnX3aDjRS%2FGDgC1uA5f3nvmvPIme1ZmHblYTCfOFnByXP&X-Amz-Signature=8fb8ccc9880676b4238e3910ddcfed6396c01ae79ee21c054b96e9b3ddab1514&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZEKB4JO%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023520Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvByke482WxiOlRTkMe1dERCYxpVfnnkGXY7SX%2Fl51kwIgIjBReGpMa73awVG9jB0BMaCHbqEyfEw1B5SU6Qj%2FslIqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDZP%2B5f%2FNsQkoFh2eSrcA7i1g9kJq%2FSsALWDMSy7hBWmy92iG1mrRZsAARNGZUWSDa7PcmxdqXrfgmQhrJwwk8dwAay8ieQIdofTaJ1ThpJIWLH0S%2B4xqptF34EEctOyocHZ664%2F7N7MiyHMKA7%2B40EONSF0BIVEX8rpCledXVidRt2qsAdfAibSVZ%2BuSpyauj7YB8tklYqsx1wx1QPVLODKqUxhbzqzYb3KKm8X5lA9pug16DFzHsynuXzWEsxcVQndHJOpnjPQF2aAztmvJ%2FlI05RVqHZinFfAESwtp%2Ff%2BcHDUbHR%2BUemPpaLuLko6nKRYsltfgBEI1ghkYVqjD%2BV5%2F5nZ8eYc3cwkZizL5Pa%2FczkwEwjDXFaI1G9w3sf25bdWMYyt8PSgdxLRMER2TFLIC6TcjzoZybJDg%2BFywiFLNXthbji2hdymg18fumzO3KLRBXjAWK5GT5%2Fe3wzzg0bT6pCO7baB%2FPgqeEwrcsvLP%2FndiEZOnXvkPjmLGJ4G50PYXaqo17bu0kzDkz30l75BTacJEX586qheDqr2tkdi2veBvWCqaZ%2FJXIYCaTNi%2BJkg9pTf96bBr%2FoYN5lB1BUQU09y8cDDj6v66w6bBIy%2BP2NFIjWApD4de4bWTF%2BCKTxem%2BiS%2FGlsOadXMLLc2NQGOqUBRUbeKWA83YuLnt30dpZHcvgChc6kYJG5Cm3Wh5uRaDtekEmA8aBuY3sMqsr4IuM5mVmWSABTPS2WK6rIhF842dgYgTRvJ1c%2B8uFMezBq%2B7rcoVd3zjrc3raLszog3Nb33BzKUueb4CawG8BfZsKBGP%2BJBmj%2FL%2FE1PjfZaEHCxdke725cnX3aDjRS%2FGDgC1uA5f3nvmvPIme1ZmHblYTCfOFnByXP&X-Amz-Signature=8def07c6666fed2a0ae3fea3c8d695f74cbde760f3eeddcaa128fe0ebb505b4c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZEKB4JO%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023520Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvByke482WxiOlRTkMe1dERCYxpVfnnkGXY7SX%2Fl51kwIgIjBReGpMa73awVG9jB0BMaCHbqEyfEw1B5SU6Qj%2FslIqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDZP%2B5f%2FNsQkoFh2eSrcA7i1g9kJq%2FSsALWDMSy7hBWmy92iG1mrRZsAARNGZUWSDa7PcmxdqXrfgmQhrJwwk8dwAay8ieQIdofTaJ1ThpJIWLH0S%2B4xqptF34EEctOyocHZ664%2F7N7MiyHMKA7%2B40EONSF0BIVEX8rpCledXVidRt2qsAdfAibSVZ%2BuSpyauj7YB8tklYqsx1wx1QPVLODKqUxhbzqzYb3KKm8X5lA9pug16DFzHsynuXzWEsxcVQndHJOpnjPQF2aAztmvJ%2FlI05RVqHZinFfAESwtp%2Ff%2BcHDUbHR%2BUemPpaLuLko6nKRYsltfgBEI1ghkYVqjD%2BV5%2F5nZ8eYc3cwkZizL5Pa%2FczkwEwjDXFaI1G9w3sf25bdWMYyt8PSgdxLRMER2TFLIC6TcjzoZybJDg%2BFywiFLNXthbji2hdymg18fumzO3KLRBXjAWK5GT5%2Fe3wzzg0bT6pCO7baB%2FPgqeEwrcsvLP%2FndiEZOnXvkPjmLGJ4G50PYXaqo17bu0kzDkz30l75BTacJEX586qheDqr2tkdi2veBvWCqaZ%2FJXIYCaTNi%2BJkg9pTf96bBr%2FoYN5lB1BUQU09y8cDDj6v66w6bBIy%2BP2NFIjWApD4de4bWTF%2BCKTxem%2BiS%2FGlsOadXMLLc2NQGOqUBRUbeKWA83YuLnt30dpZHcvgChc6kYJG5Cm3Wh5uRaDtekEmA8aBuY3sMqsr4IuM5mVmWSABTPS2WK6rIhF842dgYgTRvJ1c%2B8uFMezBq%2B7rcoVd3zjrc3raLszog3Nb33BzKUueb4CawG8BfZsKBGP%2BJBmj%2FL%2FE1PjfZaEHCxdke725cnX3aDjRS%2FGDgC1uA5f3nvmvPIme1ZmHblYTCfOFnByXP&X-Amz-Signature=ed240587e792c69e59c989e8ba747b4341a955f0ae302aac8e6a3fdbd26986c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZEKB4JO%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023520Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvByke482WxiOlRTkMe1dERCYxpVfnnkGXY7SX%2Fl51kwIgIjBReGpMa73awVG9jB0BMaCHbqEyfEw1B5SU6Qj%2FslIqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDZP%2B5f%2FNsQkoFh2eSrcA7i1g9kJq%2FSsALWDMSy7hBWmy92iG1mrRZsAARNGZUWSDa7PcmxdqXrfgmQhrJwwk8dwAay8ieQIdofTaJ1ThpJIWLH0S%2B4xqptF34EEctOyocHZ664%2F7N7MiyHMKA7%2B40EONSF0BIVEX8rpCledXVidRt2qsAdfAibSVZ%2BuSpyauj7YB8tklYqsx1wx1QPVLODKqUxhbzqzYb3KKm8X5lA9pug16DFzHsynuXzWEsxcVQndHJOpnjPQF2aAztmvJ%2FlI05RVqHZinFfAESwtp%2Ff%2BcHDUbHR%2BUemPpaLuLko6nKRYsltfgBEI1ghkYVqjD%2BV5%2F5nZ8eYc3cwkZizL5Pa%2FczkwEwjDXFaI1G9w3sf25bdWMYyt8PSgdxLRMER2TFLIC6TcjzoZybJDg%2BFywiFLNXthbji2hdymg18fumzO3KLRBXjAWK5GT5%2Fe3wzzg0bT6pCO7baB%2FPgqeEwrcsvLP%2FndiEZOnXvkPjmLGJ4G50PYXaqo17bu0kzDkz30l75BTacJEX586qheDqr2tkdi2veBvWCqaZ%2FJXIYCaTNi%2BJkg9pTf96bBr%2FoYN5lB1BUQU09y8cDDj6v66w6bBIy%2BP2NFIjWApD4de4bWTF%2BCKTxem%2BiS%2FGlsOadXMLLc2NQGOqUBRUbeKWA83YuLnt30dpZHcvgChc6kYJG5Cm3Wh5uRaDtekEmA8aBuY3sMqsr4IuM5mVmWSABTPS2WK6rIhF842dgYgTRvJ1c%2B8uFMezBq%2B7rcoVd3zjrc3raLszog3Nb33BzKUueb4CawG8BfZsKBGP%2BJBmj%2FL%2FE1PjfZaEHCxdke725cnX3aDjRS%2FGDgC1uA5f3nvmvPIme1ZmHblYTCfOFnByXP&X-Amz-Signature=8256f0bd21eab459b9e513a30264a8a9bc0cdc5148f37006338dd586211e2813&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
