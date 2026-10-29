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
fetched_at: '2026-10-29T03:27:07.213Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PZCHARP%2F20261029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261029T032703Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJHMEUCICtXXkjyBUAjxedmW%2B%2B8OIC8CCwV%2B33fUeMJeu0I7ssLAiEAxnbB95VEjX%2BadmhJpnxOuSZuDhM%2F9wHAcOeeoGfSlGAq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDALaCHcpAopUpCRzqyrcA1uKIiQYUWQIlbAmHR04j1tQifcG1Bn%2BLQk7rc8SibfpeB4ouLczuRbR69M3sZJFU7uQv2tNiBYN2hZ10ddiDD%2BiSxBMV3I%2FGbaV1yhYO2ulzgAJm3Zdhrb5zO1QbyUbA5CddiKgUFZZ%2BfLDg%2BUte0SGLRK20IyNlKDhNm1WKsV6M3ua%2BNLHuFMQqqS9YQJ%2BbVfZGI2LRkp11QlvurONzZI6HnDiysKSbXkOAca3SACiMyDeSqg9EDxhFngOO%2BXwZiOkx%2FKEPr31Z1BFQkrN1kMB0h9YazcMRrzzV%2FKZK0USh2sekaNxR3jm63EZoz62eKZi5dRf9W3v%2BSiG5oMpzmGLkogZQ98F3ffadqwWII%2BHrS9czR5tFzF8pcdnl55jGuTUQhr68jzz8tWKAOcCGPFn%2FfDEey%2B17sEL7%2FIj1VC66nUVoUADVkonQwUBE8EiCyyoJfWF78SQmDB5%2B4n6cI1nvkr1VewjUHwUXwXlvh1TGHOnLPwgPehtSG4mYq1eSxi6lrROBQZh4NZY7%2FiX5yFnxCgOL8N3rpPP6%2Fk8M8Ja%2FpLKyg408AjOta7ba%2BUwIq2zAXivwq4bWLaiW%2F%2F8S7V%2FDvksjf5oZZ1v3hTaOw8G%2Fdl7pA%2FfRnEdm%2FQJMPToitcGOqUBYuvsviR3H2Sb2TZOTIujgR3W3AV%2FLmXBirZrC7ifjGPl%2BifDw0%2F8cPjcZu%2BpJ1kkO0BZwvkCV8dtat%2F6aCWC0J%2BM8WJq0CgFeywb2IWKKmqp7VScYgz7H28ce%2FyNjUWKEhSag1s6Ko4CLZzPf4aEjqpV7b4sYYvdBjdTLnaYhYtUUISh9nhfa1gopHQU1%2FrEBUOggYs1d3m8d4s%2FXR0N6jrX9P3z&X-Amz-Signature=1b4a4413a98ef00fed5f3d93be26c3aebeb4ad3c8d194963f9f2cb0f5f29a964&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PZCHARP%2F20261029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261029T032703Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJHMEUCICtXXkjyBUAjxedmW%2B%2B8OIC8CCwV%2B33fUeMJeu0I7ssLAiEAxnbB95VEjX%2BadmhJpnxOuSZuDhM%2F9wHAcOeeoGfSlGAq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDALaCHcpAopUpCRzqyrcA1uKIiQYUWQIlbAmHR04j1tQifcG1Bn%2BLQk7rc8SibfpeB4ouLczuRbR69M3sZJFU7uQv2tNiBYN2hZ10ddiDD%2BiSxBMV3I%2FGbaV1yhYO2ulzgAJm3Zdhrb5zO1QbyUbA5CddiKgUFZZ%2BfLDg%2BUte0SGLRK20IyNlKDhNm1WKsV6M3ua%2BNLHuFMQqqS9YQJ%2BbVfZGI2LRkp11QlvurONzZI6HnDiysKSbXkOAca3SACiMyDeSqg9EDxhFngOO%2BXwZiOkx%2FKEPr31Z1BFQkrN1kMB0h9YazcMRrzzV%2FKZK0USh2sekaNxR3jm63EZoz62eKZi5dRf9W3v%2BSiG5oMpzmGLkogZQ98F3ffadqwWII%2BHrS9czR5tFzF8pcdnl55jGuTUQhr68jzz8tWKAOcCGPFn%2FfDEey%2B17sEL7%2FIj1VC66nUVoUADVkonQwUBE8EiCyyoJfWF78SQmDB5%2B4n6cI1nvkr1VewjUHwUXwXlvh1TGHOnLPwgPehtSG4mYq1eSxi6lrROBQZh4NZY7%2FiX5yFnxCgOL8N3rpPP6%2Fk8M8Ja%2FpLKyg408AjOta7ba%2BUwIq2zAXivwq4bWLaiW%2F%2F8S7V%2FDvksjf5oZZ1v3hTaOw8G%2Fdl7pA%2FfRnEdm%2FQJMPToitcGOqUBYuvsviR3H2Sb2TZOTIujgR3W3AV%2FLmXBirZrC7ifjGPl%2BifDw0%2F8cPjcZu%2BpJ1kkO0BZwvkCV8dtat%2F6aCWC0J%2BM8WJq0CgFeywb2IWKKmqp7VScYgz7H28ce%2FyNjUWKEhSag1s6Ko4CLZzPf4aEjqpV7b4sYYvdBjdTLnaYhYtUUISh9nhfa1gopHQU1%2FrEBUOggYs1d3m8d4s%2FXR0N6jrX9P3z&X-Amz-Signature=2a00b47e6c6c015055ad7084b3ec0398197138fbac18c8afcfcb8868209418d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PZCHARP%2F20261029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261029T032703Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJHMEUCICtXXkjyBUAjxedmW%2B%2B8OIC8CCwV%2B33fUeMJeu0I7ssLAiEAxnbB95VEjX%2BadmhJpnxOuSZuDhM%2F9wHAcOeeoGfSlGAq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDALaCHcpAopUpCRzqyrcA1uKIiQYUWQIlbAmHR04j1tQifcG1Bn%2BLQk7rc8SibfpeB4ouLczuRbR69M3sZJFU7uQv2tNiBYN2hZ10ddiDD%2BiSxBMV3I%2FGbaV1yhYO2ulzgAJm3Zdhrb5zO1QbyUbA5CddiKgUFZZ%2BfLDg%2BUte0SGLRK20IyNlKDhNm1WKsV6M3ua%2BNLHuFMQqqS9YQJ%2BbVfZGI2LRkp11QlvurONzZI6HnDiysKSbXkOAca3SACiMyDeSqg9EDxhFngOO%2BXwZiOkx%2FKEPr31Z1BFQkrN1kMB0h9YazcMRrzzV%2FKZK0USh2sekaNxR3jm63EZoz62eKZi5dRf9W3v%2BSiG5oMpzmGLkogZQ98F3ffadqwWII%2BHrS9czR5tFzF8pcdnl55jGuTUQhr68jzz8tWKAOcCGPFn%2FfDEey%2B17sEL7%2FIj1VC66nUVoUADVkonQwUBE8EiCyyoJfWF78SQmDB5%2B4n6cI1nvkr1VewjUHwUXwXlvh1TGHOnLPwgPehtSG4mYq1eSxi6lrROBQZh4NZY7%2FiX5yFnxCgOL8N3rpPP6%2Fk8M8Ja%2FpLKyg408AjOta7ba%2BUwIq2zAXivwq4bWLaiW%2F%2F8S7V%2FDvksjf5oZZ1v3hTaOw8G%2Fdl7pA%2FfRnEdm%2FQJMPToitcGOqUBYuvsviR3H2Sb2TZOTIujgR3W3AV%2FLmXBirZrC7ifjGPl%2BifDw0%2F8cPjcZu%2BpJ1kkO0BZwvkCV8dtat%2F6aCWC0J%2BM8WJq0CgFeywb2IWKKmqp7VScYgz7H28ce%2FyNjUWKEhSag1s6Ko4CLZzPf4aEjqpV7b4sYYvdBjdTLnaYhYtUUISh9nhfa1gopHQU1%2FrEBUOggYs1d3m8d4s%2FXR0N6jrX9P3z&X-Amz-Signature=e156121d0f072da1ae65864fe68610be1959da7a70eeb28dc3b1f242a0bc0417&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PZCHARP%2F20261029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261029T032703Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJHMEUCICtXXkjyBUAjxedmW%2B%2B8OIC8CCwV%2B33fUeMJeu0I7ssLAiEAxnbB95VEjX%2BadmhJpnxOuSZuDhM%2F9wHAcOeeoGfSlGAq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDALaCHcpAopUpCRzqyrcA1uKIiQYUWQIlbAmHR04j1tQifcG1Bn%2BLQk7rc8SibfpeB4ouLczuRbR69M3sZJFU7uQv2tNiBYN2hZ10ddiDD%2BiSxBMV3I%2FGbaV1yhYO2ulzgAJm3Zdhrb5zO1QbyUbA5CddiKgUFZZ%2BfLDg%2BUte0SGLRK20IyNlKDhNm1WKsV6M3ua%2BNLHuFMQqqS9YQJ%2BbVfZGI2LRkp11QlvurONzZI6HnDiysKSbXkOAca3SACiMyDeSqg9EDxhFngOO%2BXwZiOkx%2FKEPr31Z1BFQkrN1kMB0h9YazcMRrzzV%2FKZK0USh2sekaNxR3jm63EZoz62eKZi5dRf9W3v%2BSiG5oMpzmGLkogZQ98F3ffadqwWII%2BHrS9czR5tFzF8pcdnl55jGuTUQhr68jzz8tWKAOcCGPFn%2FfDEey%2B17sEL7%2FIj1VC66nUVoUADVkonQwUBE8EiCyyoJfWF78SQmDB5%2B4n6cI1nvkr1VewjUHwUXwXlvh1TGHOnLPwgPehtSG4mYq1eSxi6lrROBQZh4NZY7%2FiX5yFnxCgOL8N3rpPP6%2Fk8M8Ja%2FpLKyg408AjOta7ba%2BUwIq2zAXivwq4bWLaiW%2F%2F8S7V%2FDvksjf5oZZ1v3hTaOw8G%2Fdl7pA%2FfRnEdm%2FQJMPToitcGOqUBYuvsviR3H2Sb2TZOTIujgR3W3AV%2FLmXBirZrC7ifjGPl%2BifDw0%2F8cPjcZu%2BpJ1kkO0BZwvkCV8dtat%2F6aCWC0J%2BM8WJq0CgFeywb2IWKKmqp7VScYgz7H28ce%2FyNjUWKEhSag1s6Ko4CLZzPf4aEjqpV7b4sYYvdBjdTLnaYhYtUUISh9nhfa1gopHQU1%2FrEBUOggYs1d3m8d4s%2FXR0N6jrX9P3z&X-Amz-Signature=7444ae07c2b7d1184ce1ef1208778298c7df5de9acdc5fad81c71ff5b291c21e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PZCHARP%2F20261029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261029T032703Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJHMEUCICtXXkjyBUAjxedmW%2B%2B8OIC8CCwV%2B33fUeMJeu0I7ssLAiEAxnbB95VEjX%2BadmhJpnxOuSZuDhM%2F9wHAcOeeoGfSlGAq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDALaCHcpAopUpCRzqyrcA1uKIiQYUWQIlbAmHR04j1tQifcG1Bn%2BLQk7rc8SibfpeB4ouLczuRbR69M3sZJFU7uQv2tNiBYN2hZ10ddiDD%2BiSxBMV3I%2FGbaV1yhYO2ulzgAJm3Zdhrb5zO1QbyUbA5CddiKgUFZZ%2BfLDg%2BUte0SGLRK20IyNlKDhNm1WKsV6M3ua%2BNLHuFMQqqS9YQJ%2BbVfZGI2LRkp11QlvurONzZI6HnDiysKSbXkOAca3SACiMyDeSqg9EDxhFngOO%2BXwZiOkx%2FKEPr31Z1BFQkrN1kMB0h9YazcMRrzzV%2FKZK0USh2sekaNxR3jm63EZoz62eKZi5dRf9W3v%2BSiG5oMpzmGLkogZQ98F3ffadqwWII%2BHrS9czR5tFzF8pcdnl55jGuTUQhr68jzz8tWKAOcCGPFn%2FfDEey%2B17sEL7%2FIj1VC66nUVoUADVkonQwUBE8EiCyyoJfWF78SQmDB5%2B4n6cI1nvkr1VewjUHwUXwXlvh1TGHOnLPwgPehtSG4mYq1eSxi6lrROBQZh4NZY7%2FiX5yFnxCgOL8N3rpPP6%2Fk8M8Ja%2FpLKyg408AjOta7ba%2BUwIq2zAXivwq4bWLaiW%2F%2F8S7V%2FDvksjf5oZZ1v3hTaOw8G%2Fdl7pA%2FfRnEdm%2FQJMPToitcGOqUBYuvsviR3H2Sb2TZOTIujgR3W3AV%2FLmXBirZrC7ifjGPl%2BifDw0%2F8cPjcZu%2BpJ1kkO0BZwvkCV8dtat%2F6aCWC0J%2BM8WJq0CgFeywb2IWKKmqp7VScYgz7H28ce%2FyNjUWKEhSag1s6Ko4CLZzPf4aEjqpV7b4sYYvdBjdTLnaYhYtUUISh9nhfa1gopHQU1%2FrEBUOggYs1d3m8d4s%2FXR0N6jrX9P3z&X-Amz-Signature=67fe1e8bd15881e2a60f8faf3a9c512a9f888c7a2c7ed6a2b0be5689cacc98e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PZCHARP%2F20261029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261029T032703Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJHMEUCICtXXkjyBUAjxedmW%2B%2B8OIC8CCwV%2B33fUeMJeu0I7ssLAiEAxnbB95VEjX%2BadmhJpnxOuSZuDhM%2F9wHAcOeeoGfSlGAq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDALaCHcpAopUpCRzqyrcA1uKIiQYUWQIlbAmHR04j1tQifcG1Bn%2BLQk7rc8SibfpeB4ouLczuRbR69M3sZJFU7uQv2tNiBYN2hZ10ddiDD%2BiSxBMV3I%2FGbaV1yhYO2ulzgAJm3Zdhrb5zO1QbyUbA5CddiKgUFZZ%2BfLDg%2BUte0SGLRK20IyNlKDhNm1WKsV6M3ua%2BNLHuFMQqqS9YQJ%2BbVfZGI2LRkp11QlvurONzZI6HnDiysKSbXkOAca3SACiMyDeSqg9EDxhFngOO%2BXwZiOkx%2FKEPr31Z1BFQkrN1kMB0h9YazcMRrzzV%2FKZK0USh2sekaNxR3jm63EZoz62eKZi5dRf9W3v%2BSiG5oMpzmGLkogZQ98F3ffadqwWII%2BHrS9czR5tFzF8pcdnl55jGuTUQhr68jzz8tWKAOcCGPFn%2FfDEey%2B17sEL7%2FIj1VC66nUVoUADVkonQwUBE8EiCyyoJfWF78SQmDB5%2B4n6cI1nvkr1VewjUHwUXwXlvh1TGHOnLPwgPehtSG4mYq1eSxi6lrROBQZh4NZY7%2FiX5yFnxCgOL8N3rpPP6%2Fk8M8Ja%2FpLKyg408AjOta7ba%2BUwIq2zAXivwq4bWLaiW%2F%2F8S7V%2FDvksjf5oZZ1v3hTaOw8G%2Fdl7pA%2FfRnEdm%2FQJMPToitcGOqUBYuvsviR3H2Sb2TZOTIujgR3W3AV%2FLmXBirZrC7ifjGPl%2BifDw0%2F8cPjcZu%2BpJ1kkO0BZwvkCV8dtat%2F6aCWC0J%2BM8WJq0CgFeywb2IWKKmqp7VScYgz7H28ce%2FyNjUWKEhSag1s6Ko4CLZzPf4aEjqpV7b4sYYvdBjdTLnaYhYtUUISh9nhfa1gopHQU1%2FrEBUOggYs1d3m8d4s%2FXR0N6jrX9P3z&X-Amz-Signature=6cea53cdb581b367c85641965ad79f2308cfed21022d1cbcc4fd6179ae34d0d5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PZCHARP%2F20261029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261029T032703Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJHMEUCICtXXkjyBUAjxedmW%2B%2B8OIC8CCwV%2B33fUeMJeu0I7ssLAiEAxnbB95VEjX%2BadmhJpnxOuSZuDhM%2F9wHAcOeeoGfSlGAq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDALaCHcpAopUpCRzqyrcA1uKIiQYUWQIlbAmHR04j1tQifcG1Bn%2BLQk7rc8SibfpeB4ouLczuRbR69M3sZJFU7uQv2tNiBYN2hZ10ddiDD%2BiSxBMV3I%2FGbaV1yhYO2ulzgAJm3Zdhrb5zO1QbyUbA5CddiKgUFZZ%2BfLDg%2BUte0SGLRK20IyNlKDhNm1WKsV6M3ua%2BNLHuFMQqqS9YQJ%2BbVfZGI2LRkp11QlvurONzZI6HnDiysKSbXkOAca3SACiMyDeSqg9EDxhFngOO%2BXwZiOkx%2FKEPr31Z1BFQkrN1kMB0h9YazcMRrzzV%2FKZK0USh2sekaNxR3jm63EZoz62eKZi5dRf9W3v%2BSiG5oMpzmGLkogZQ98F3ffadqwWII%2BHrS9czR5tFzF8pcdnl55jGuTUQhr68jzz8tWKAOcCGPFn%2FfDEey%2B17sEL7%2FIj1VC66nUVoUADVkonQwUBE8EiCyyoJfWF78SQmDB5%2B4n6cI1nvkr1VewjUHwUXwXlvh1TGHOnLPwgPehtSG4mYq1eSxi6lrROBQZh4NZY7%2FiX5yFnxCgOL8N3rpPP6%2Fk8M8Ja%2FpLKyg408AjOta7ba%2BUwIq2zAXivwq4bWLaiW%2F%2F8S7V%2FDvksjf5oZZ1v3hTaOw8G%2Fdl7pA%2FfRnEdm%2FQJMPToitcGOqUBYuvsviR3H2Sb2TZOTIujgR3W3AV%2FLmXBirZrC7ifjGPl%2BifDw0%2F8cPjcZu%2BpJ1kkO0BZwvkCV8dtat%2F6aCWC0J%2BM8WJq0CgFeywb2IWKKmqp7VScYgz7H28ce%2FyNjUWKEhSag1s6Ko4CLZzPf4aEjqpV7b4sYYvdBjdTLnaYhYtUUISh9nhfa1gopHQU1%2FrEBUOggYs1d3m8d4s%2FXR0N6jrX9P3z&X-Amz-Signature=c0f924b0972574d9f8309e77fc0db4eed2f8a9430440d8e22a446b9b3d3ff755&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PZCHARP%2F20261029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261029T032703Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJHMEUCICtXXkjyBUAjxedmW%2B%2B8OIC8CCwV%2B33fUeMJeu0I7ssLAiEAxnbB95VEjX%2BadmhJpnxOuSZuDhM%2F9wHAcOeeoGfSlGAq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDALaCHcpAopUpCRzqyrcA1uKIiQYUWQIlbAmHR04j1tQifcG1Bn%2BLQk7rc8SibfpeB4ouLczuRbR69M3sZJFU7uQv2tNiBYN2hZ10ddiDD%2BiSxBMV3I%2FGbaV1yhYO2ulzgAJm3Zdhrb5zO1QbyUbA5CddiKgUFZZ%2BfLDg%2BUte0SGLRK20IyNlKDhNm1WKsV6M3ua%2BNLHuFMQqqS9YQJ%2BbVfZGI2LRkp11QlvurONzZI6HnDiysKSbXkOAca3SACiMyDeSqg9EDxhFngOO%2BXwZiOkx%2FKEPr31Z1BFQkrN1kMB0h9YazcMRrzzV%2FKZK0USh2sekaNxR3jm63EZoz62eKZi5dRf9W3v%2BSiG5oMpzmGLkogZQ98F3ffadqwWII%2BHrS9czR5tFzF8pcdnl55jGuTUQhr68jzz8tWKAOcCGPFn%2FfDEey%2B17sEL7%2FIj1VC66nUVoUADVkonQwUBE8EiCyyoJfWF78SQmDB5%2B4n6cI1nvkr1VewjUHwUXwXlvh1TGHOnLPwgPehtSG4mYq1eSxi6lrROBQZh4NZY7%2FiX5yFnxCgOL8N3rpPP6%2Fk8M8Ja%2FpLKyg408AjOta7ba%2BUwIq2zAXivwq4bWLaiW%2F%2F8S7V%2FDvksjf5oZZ1v3hTaOw8G%2Fdl7pA%2FfRnEdm%2FQJMPToitcGOqUBYuvsviR3H2Sb2TZOTIujgR3W3AV%2FLmXBirZrC7ifjGPl%2BifDw0%2F8cPjcZu%2BpJ1kkO0BZwvkCV8dtat%2F6aCWC0J%2BM8WJq0CgFeywb2IWKKmqp7VScYgz7H28ce%2FyNjUWKEhSag1s6Ko4CLZzPf4aEjqpV7b4sYYvdBjdTLnaYhYtUUISh9nhfa1gopHQU1%2FrEBUOggYs1d3m8d4s%2FXR0N6jrX9P3z&X-Amz-Signature=5edda1f19597c7027cb0a7c38cba86f2d27f732e4a5702d1c2c703aec778e615&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PZCHARP%2F20261029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261029T032703Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJHMEUCICtXXkjyBUAjxedmW%2B%2B8OIC8CCwV%2B33fUeMJeu0I7ssLAiEAxnbB95VEjX%2BadmhJpnxOuSZuDhM%2F9wHAcOeeoGfSlGAq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDALaCHcpAopUpCRzqyrcA1uKIiQYUWQIlbAmHR04j1tQifcG1Bn%2BLQk7rc8SibfpeB4ouLczuRbR69M3sZJFU7uQv2tNiBYN2hZ10ddiDD%2BiSxBMV3I%2FGbaV1yhYO2ulzgAJm3Zdhrb5zO1QbyUbA5CddiKgUFZZ%2BfLDg%2BUte0SGLRK20IyNlKDhNm1WKsV6M3ua%2BNLHuFMQqqS9YQJ%2BbVfZGI2LRkp11QlvurONzZI6HnDiysKSbXkOAca3SACiMyDeSqg9EDxhFngOO%2BXwZiOkx%2FKEPr31Z1BFQkrN1kMB0h9YazcMRrzzV%2FKZK0USh2sekaNxR3jm63EZoz62eKZi5dRf9W3v%2BSiG5oMpzmGLkogZQ98F3ffadqwWII%2BHrS9czR5tFzF8pcdnl55jGuTUQhr68jzz8tWKAOcCGPFn%2FfDEey%2B17sEL7%2FIj1VC66nUVoUADVkonQwUBE8EiCyyoJfWF78SQmDB5%2B4n6cI1nvkr1VewjUHwUXwXlvh1TGHOnLPwgPehtSG4mYq1eSxi6lrROBQZh4NZY7%2FiX5yFnxCgOL8N3rpPP6%2Fk8M8Ja%2FpLKyg408AjOta7ba%2BUwIq2zAXivwq4bWLaiW%2F%2F8S7V%2FDvksjf5oZZ1v3hTaOw8G%2Fdl7pA%2FfRnEdm%2FQJMPToitcGOqUBYuvsviR3H2Sb2TZOTIujgR3W3AV%2FLmXBirZrC7ifjGPl%2BifDw0%2F8cPjcZu%2BpJ1kkO0BZwvkCV8dtat%2F6aCWC0J%2BM8WJq0CgFeywb2IWKKmqp7VScYgz7H28ce%2FyNjUWKEhSag1s6Ko4CLZzPf4aEjqpV7b4sYYvdBjdTLnaYhYtUUISh9nhfa1gopHQU1%2FrEBUOggYs1d3m8d4s%2FXR0N6jrX9P3z&X-Amz-Signature=1550f44763b23a11abbb36b80b0184215761b6deeeb8710a22a326e2b2aecd63&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
