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
fetched_at: '2026-09-03T02:01:58.600Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRF2GEEM%2F20260903%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260903T020153Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIBZ8dXhM6lo20DVrqZ2WaGfVLANf77%2B1NOA2GwJo%2BoveAiEAmYDn5cCoxinJEafwegw2irs%2FgECQo3IZU%2B%2Bq8efaHwEqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF8yR8c3ia78F01SeircA89wpIAz8NKCJrvACZTHiP9xIhn%2F%2BSPkrCTPphcepCny1ImEtiY2wJiF41Py7y7l7efOWHh3nUZd5ZwBccwey%2FEoe%2B8xEBz%2B2xo2jr1yg%2BK13l31QcIt1tcrJLtJDIwIp4ir9P3xpkJGc86eOOqbYExvt5B1y0kJPm4CSoCyunV%2Bm1K99O4AOIGZOF7jmDzv1PSlSdKPC%2BSVCOT5ckGN%2FMNh9bXDsIzwopZrw3HI4h3Lw3djrjsGOIslE4fAIBeAAMuqKdQTT6ORrnsdGRAD1GObBRu5jQWegny%2F1rNVLVM9vH8csoC0wfzujVBiEaCLBaSAFFidVCaV7RtSYNd1dkuEhV7Oda0MXii2sAydBmFTjZ9veyaFFKOO5Pg1zKTVpD0z8Ba6NJkx3spVmDM158uXbISTmVBqyrH6D1iCt2JLfKX89%2BVymRtJ83ynONrAtcCgOmdgu2MiAZ%2BdZwEpMNy0g8lnVaqyP27sqlj3H2g0YgarQZLimoC%2Bc7AD4AAPj1CnfBDXvaIWzKy%2BiNRW6jRib5A0FgXOGsoj0E%2FHsYbHsBU0htI28jaamf3TmGKgMrOGomSZxFWlQfg6WoCxaBjV7ZxZq1IeIs2g4v0buqLCjDMuRTkKDz3T0Co3MIeg49QGOqUBLxdv6k7p%2Bi2c9J96UFKXvi4YL2ANn1%2BaeWkMHAefgFv8d%2FrfP%2FmS2a7MRpcnCCNJcwR%2BZ4y2CzsXUi96c%2BzGUMibfZOUbIYNGuRc92eD4uHopVxH72G%2BOGAzbQdSqIaXx%2FSwNI9TsKmHVXtbH3PmO2kJmZHpvpuMxI9eGv4UTs92Iw5Yh7Jyy6DmKaAvJF71ZBPdP7lr9MIbz1%2BHfGkobq%2BZLjO0&X-Amz-Signature=efca11f57530bd47341f7eadd05c50aaa10852d8b6846e065263f74d276bc7e5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRF2GEEM%2F20260903%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260903T020153Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIBZ8dXhM6lo20DVrqZ2WaGfVLANf77%2B1NOA2GwJo%2BoveAiEAmYDn5cCoxinJEafwegw2irs%2FgECQo3IZU%2B%2Bq8efaHwEqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF8yR8c3ia78F01SeircA89wpIAz8NKCJrvACZTHiP9xIhn%2F%2BSPkrCTPphcepCny1ImEtiY2wJiF41Py7y7l7efOWHh3nUZd5ZwBccwey%2FEoe%2B8xEBz%2B2xo2jr1yg%2BK13l31QcIt1tcrJLtJDIwIp4ir9P3xpkJGc86eOOqbYExvt5B1y0kJPm4CSoCyunV%2Bm1K99O4AOIGZOF7jmDzv1PSlSdKPC%2BSVCOT5ckGN%2FMNh9bXDsIzwopZrw3HI4h3Lw3djrjsGOIslE4fAIBeAAMuqKdQTT6ORrnsdGRAD1GObBRu5jQWegny%2F1rNVLVM9vH8csoC0wfzujVBiEaCLBaSAFFidVCaV7RtSYNd1dkuEhV7Oda0MXii2sAydBmFTjZ9veyaFFKOO5Pg1zKTVpD0z8Ba6NJkx3spVmDM158uXbISTmVBqyrH6D1iCt2JLfKX89%2BVymRtJ83ynONrAtcCgOmdgu2MiAZ%2BdZwEpMNy0g8lnVaqyP27sqlj3H2g0YgarQZLimoC%2Bc7AD4AAPj1CnfBDXvaIWzKy%2BiNRW6jRib5A0FgXOGsoj0E%2FHsYbHsBU0htI28jaamf3TmGKgMrOGomSZxFWlQfg6WoCxaBjV7ZxZq1IeIs2g4v0buqLCjDMuRTkKDz3T0Co3MIeg49QGOqUBLxdv6k7p%2Bi2c9J96UFKXvi4YL2ANn1%2BaeWkMHAefgFv8d%2FrfP%2FmS2a7MRpcnCCNJcwR%2BZ4y2CzsXUi96c%2BzGUMibfZOUbIYNGuRc92eD4uHopVxH72G%2BOGAzbQdSqIaXx%2FSwNI9TsKmHVXtbH3PmO2kJmZHpvpuMxI9eGv4UTs92Iw5Yh7Jyy6DmKaAvJF71ZBPdP7lr9MIbz1%2BHfGkobq%2BZLjO0&X-Amz-Signature=434c5bd7a537534a5eca01dc0856ae464b091835a7bc536809e8d09acba18bdb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRF2GEEM%2F20260903%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260903T020153Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIBZ8dXhM6lo20DVrqZ2WaGfVLANf77%2B1NOA2GwJo%2BoveAiEAmYDn5cCoxinJEafwegw2irs%2FgECQo3IZU%2B%2Bq8efaHwEqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF8yR8c3ia78F01SeircA89wpIAz8NKCJrvACZTHiP9xIhn%2F%2BSPkrCTPphcepCny1ImEtiY2wJiF41Py7y7l7efOWHh3nUZd5ZwBccwey%2FEoe%2B8xEBz%2B2xo2jr1yg%2BK13l31QcIt1tcrJLtJDIwIp4ir9P3xpkJGc86eOOqbYExvt5B1y0kJPm4CSoCyunV%2Bm1K99O4AOIGZOF7jmDzv1PSlSdKPC%2BSVCOT5ckGN%2FMNh9bXDsIzwopZrw3HI4h3Lw3djrjsGOIslE4fAIBeAAMuqKdQTT6ORrnsdGRAD1GObBRu5jQWegny%2F1rNVLVM9vH8csoC0wfzujVBiEaCLBaSAFFidVCaV7RtSYNd1dkuEhV7Oda0MXii2sAydBmFTjZ9veyaFFKOO5Pg1zKTVpD0z8Ba6NJkx3spVmDM158uXbISTmVBqyrH6D1iCt2JLfKX89%2BVymRtJ83ynONrAtcCgOmdgu2MiAZ%2BdZwEpMNy0g8lnVaqyP27sqlj3H2g0YgarQZLimoC%2Bc7AD4AAPj1CnfBDXvaIWzKy%2BiNRW6jRib5A0FgXOGsoj0E%2FHsYbHsBU0htI28jaamf3TmGKgMrOGomSZxFWlQfg6WoCxaBjV7ZxZq1IeIs2g4v0buqLCjDMuRTkKDz3T0Co3MIeg49QGOqUBLxdv6k7p%2Bi2c9J96UFKXvi4YL2ANn1%2BaeWkMHAefgFv8d%2FrfP%2FmS2a7MRpcnCCNJcwR%2BZ4y2CzsXUi96c%2BzGUMibfZOUbIYNGuRc92eD4uHopVxH72G%2BOGAzbQdSqIaXx%2FSwNI9TsKmHVXtbH3PmO2kJmZHpvpuMxI9eGv4UTs92Iw5Yh7Jyy6DmKaAvJF71ZBPdP7lr9MIbz1%2BHfGkobq%2BZLjO0&X-Amz-Signature=f7ed856c1034967dbd1fa2fabe6cae42327ace9866167e6a4a2ce6588ea1c685&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRF2GEEM%2F20260903%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260903T020153Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIBZ8dXhM6lo20DVrqZ2WaGfVLANf77%2B1NOA2GwJo%2BoveAiEAmYDn5cCoxinJEafwegw2irs%2FgECQo3IZU%2B%2Bq8efaHwEqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF8yR8c3ia78F01SeircA89wpIAz8NKCJrvACZTHiP9xIhn%2F%2BSPkrCTPphcepCny1ImEtiY2wJiF41Py7y7l7efOWHh3nUZd5ZwBccwey%2FEoe%2B8xEBz%2B2xo2jr1yg%2BK13l31QcIt1tcrJLtJDIwIp4ir9P3xpkJGc86eOOqbYExvt5B1y0kJPm4CSoCyunV%2Bm1K99O4AOIGZOF7jmDzv1PSlSdKPC%2BSVCOT5ckGN%2FMNh9bXDsIzwopZrw3HI4h3Lw3djrjsGOIslE4fAIBeAAMuqKdQTT6ORrnsdGRAD1GObBRu5jQWegny%2F1rNVLVM9vH8csoC0wfzujVBiEaCLBaSAFFidVCaV7RtSYNd1dkuEhV7Oda0MXii2sAydBmFTjZ9veyaFFKOO5Pg1zKTVpD0z8Ba6NJkx3spVmDM158uXbISTmVBqyrH6D1iCt2JLfKX89%2BVymRtJ83ynONrAtcCgOmdgu2MiAZ%2BdZwEpMNy0g8lnVaqyP27sqlj3H2g0YgarQZLimoC%2Bc7AD4AAPj1CnfBDXvaIWzKy%2BiNRW6jRib5A0FgXOGsoj0E%2FHsYbHsBU0htI28jaamf3TmGKgMrOGomSZxFWlQfg6WoCxaBjV7ZxZq1IeIs2g4v0buqLCjDMuRTkKDz3T0Co3MIeg49QGOqUBLxdv6k7p%2Bi2c9J96UFKXvi4YL2ANn1%2BaeWkMHAefgFv8d%2FrfP%2FmS2a7MRpcnCCNJcwR%2BZ4y2CzsXUi96c%2BzGUMibfZOUbIYNGuRc92eD4uHopVxH72G%2BOGAzbQdSqIaXx%2FSwNI9TsKmHVXtbH3PmO2kJmZHpvpuMxI9eGv4UTs92Iw5Yh7Jyy6DmKaAvJF71ZBPdP7lr9MIbz1%2BHfGkobq%2BZLjO0&X-Amz-Signature=12fd30301d3315d84498340570a509f25e43b88fdaad8304887c0be0b5f7e7b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRF2GEEM%2F20260903%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260903T020153Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIBZ8dXhM6lo20DVrqZ2WaGfVLANf77%2B1NOA2GwJo%2BoveAiEAmYDn5cCoxinJEafwegw2irs%2FgECQo3IZU%2B%2Bq8efaHwEqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF8yR8c3ia78F01SeircA89wpIAz8NKCJrvACZTHiP9xIhn%2F%2BSPkrCTPphcepCny1ImEtiY2wJiF41Py7y7l7efOWHh3nUZd5ZwBccwey%2FEoe%2B8xEBz%2B2xo2jr1yg%2BK13l31QcIt1tcrJLtJDIwIp4ir9P3xpkJGc86eOOqbYExvt5B1y0kJPm4CSoCyunV%2Bm1K99O4AOIGZOF7jmDzv1PSlSdKPC%2BSVCOT5ckGN%2FMNh9bXDsIzwopZrw3HI4h3Lw3djrjsGOIslE4fAIBeAAMuqKdQTT6ORrnsdGRAD1GObBRu5jQWegny%2F1rNVLVM9vH8csoC0wfzujVBiEaCLBaSAFFidVCaV7RtSYNd1dkuEhV7Oda0MXii2sAydBmFTjZ9veyaFFKOO5Pg1zKTVpD0z8Ba6NJkx3spVmDM158uXbISTmVBqyrH6D1iCt2JLfKX89%2BVymRtJ83ynONrAtcCgOmdgu2MiAZ%2BdZwEpMNy0g8lnVaqyP27sqlj3H2g0YgarQZLimoC%2Bc7AD4AAPj1CnfBDXvaIWzKy%2BiNRW6jRib5A0FgXOGsoj0E%2FHsYbHsBU0htI28jaamf3TmGKgMrOGomSZxFWlQfg6WoCxaBjV7ZxZq1IeIs2g4v0buqLCjDMuRTkKDz3T0Co3MIeg49QGOqUBLxdv6k7p%2Bi2c9J96UFKXvi4YL2ANn1%2BaeWkMHAefgFv8d%2FrfP%2FmS2a7MRpcnCCNJcwR%2BZ4y2CzsXUi96c%2BzGUMibfZOUbIYNGuRc92eD4uHopVxH72G%2BOGAzbQdSqIaXx%2FSwNI9TsKmHVXtbH3PmO2kJmZHpvpuMxI9eGv4UTs92Iw5Yh7Jyy6DmKaAvJF71ZBPdP7lr9MIbz1%2BHfGkobq%2BZLjO0&X-Amz-Signature=13cf8726d1e8c528fddadd7ed83c86ad263c80f61be0edc9f0a06516ffd2091c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRF2GEEM%2F20260903%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260903T020153Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIBZ8dXhM6lo20DVrqZ2WaGfVLANf77%2B1NOA2GwJo%2BoveAiEAmYDn5cCoxinJEafwegw2irs%2FgECQo3IZU%2B%2Bq8efaHwEqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF8yR8c3ia78F01SeircA89wpIAz8NKCJrvACZTHiP9xIhn%2F%2BSPkrCTPphcepCny1ImEtiY2wJiF41Py7y7l7efOWHh3nUZd5ZwBccwey%2FEoe%2B8xEBz%2B2xo2jr1yg%2BK13l31QcIt1tcrJLtJDIwIp4ir9P3xpkJGc86eOOqbYExvt5B1y0kJPm4CSoCyunV%2Bm1K99O4AOIGZOF7jmDzv1PSlSdKPC%2BSVCOT5ckGN%2FMNh9bXDsIzwopZrw3HI4h3Lw3djrjsGOIslE4fAIBeAAMuqKdQTT6ORrnsdGRAD1GObBRu5jQWegny%2F1rNVLVM9vH8csoC0wfzujVBiEaCLBaSAFFidVCaV7RtSYNd1dkuEhV7Oda0MXii2sAydBmFTjZ9veyaFFKOO5Pg1zKTVpD0z8Ba6NJkx3spVmDM158uXbISTmVBqyrH6D1iCt2JLfKX89%2BVymRtJ83ynONrAtcCgOmdgu2MiAZ%2BdZwEpMNy0g8lnVaqyP27sqlj3H2g0YgarQZLimoC%2Bc7AD4AAPj1CnfBDXvaIWzKy%2BiNRW6jRib5A0FgXOGsoj0E%2FHsYbHsBU0htI28jaamf3TmGKgMrOGomSZxFWlQfg6WoCxaBjV7ZxZq1IeIs2g4v0buqLCjDMuRTkKDz3T0Co3MIeg49QGOqUBLxdv6k7p%2Bi2c9J96UFKXvi4YL2ANn1%2BaeWkMHAefgFv8d%2FrfP%2FmS2a7MRpcnCCNJcwR%2BZ4y2CzsXUi96c%2BzGUMibfZOUbIYNGuRc92eD4uHopVxH72G%2BOGAzbQdSqIaXx%2FSwNI9TsKmHVXtbH3PmO2kJmZHpvpuMxI9eGv4UTs92Iw5Yh7Jyy6DmKaAvJF71ZBPdP7lr9MIbz1%2BHfGkobq%2BZLjO0&X-Amz-Signature=c52a0e255289a97b1c6aae08ea25b6a2b05e4e1cc5ec75c80935f5838baac2be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRF2GEEM%2F20260903%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260903T020153Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIBZ8dXhM6lo20DVrqZ2WaGfVLANf77%2B1NOA2GwJo%2BoveAiEAmYDn5cCoxinJEafwegw2irs%2FgECQo3IZU%2B%2Bq8efaHwEqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF8yR8c3ia78F01SeircA89wpIAz8NKCJrvACZTHiP9xIhn%2F%2BSPkrCTPphcepCny1ImEtiY2wJiF41Py7y7l7efOWHh3nUZd5ZwBccwey%2FEoe%2B8xEBz%2B2xo2jr1yg%2BK13l31QcIt1tcrJLtJDIwIp4ir9P3xpkJGc86eOOqbYExvt5B1y0kJPm4CSoCyunV%2Bm1K99O4AOIGZOF7jmDzv1PSlSdKPC%2BSVCOT5ckGN%2FMNh9bXDsIzwopZrw3HI4h3Lw3djrjsGOIslE4fAIBeAAMuqKdQTT6ORrnsdGRAD1GObBRu5jQWegny%2F1rNVLVM9vH8csoC0wfzujVBiEaCLBaSAFFidVCaV7RtSYNd1dkuEhV7Oda0MXii2sAydBmFTjZ9veyaFFKOO5Pg1zKTVpD0z8Ba6NJkx3spVmDM158uXbISTmVBqyrH6D1iCt2JLfKX89%2BVymRtJ83ynONrAtcCgOmdgu2MiAZ%2BdZwEpMNy0g8lnVaqyP27sqlj3H2g0YgarQZLimoC%2Bc7AD4AAPj1CnfBDXvaIWzKy%2BiNRW6jRib5A0FgXOGsoj0E%2FHsYbHsBU0htI28jaamf3TmGKgMrOGomSZxFWlQfg6WoCxaBjV7ZxZq1IeIs2g4v0buqLCjDMuRTkKDz3T0Co3MIeg49QGOqUBLxdv6k7p%2Bi2c9J96UFKXvi4YL2ANn1%2BaeWkMHAefgFv8d%2FrfP%2FmS2a7MRpcnCCNJcwR%2BZ4y2CzsXUi96c%2BzGUMibfZOUbIYNGuRc92eD4uHopVxH72G%2BOGAzbQdSqIaXx%2FSwNI9TsKmHVXtbH3PmO2kJmZHpvpuMxI9eGv4UTs92Iw5Yh7Jyy6DmKaAvJF71ZBPdP7lr9MIbz1%2BHfGkobq%2BZLjO0&X-Amz-Signature=9fb4a4408382fbfe0c0b23145aa88a20e183058b61d8d5d2f711ebf6a81361e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRF2GEEM%2F20260903%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260903T020153Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIBZ8dXhM6lo20DVrqZ2WaGfVLANf77%2B1NOA2GwJo%2BoveAiEAmYDn5cCoxinJEafwegw2irs%2FgECQo3IZU%2B%2Bq8efaHwEqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF8yR8c3ia78F01SeircA89wpIAz8NKCJrvACZTHiP9xIhn%2F%2BSPkrCTPphcepCny1ImEtiY2wJiF41Py7y7l7efOWHh3nUZd5ZwBccwey%2FEoe%2B8xEBz%2B2xo2jr1yg%2BK13l31QcIt1tcrJLtJDIwIp4ir9P3xpkJGc86eOOqbYExvt5B1y0kJPm4CSoCyunV%2Bm1K99O4AOIGZOF7jmDzv1PSlSdKPC%2BSVCOT5ckGN%2FMNh9bXDsIzwopZrw3HI4h3Lw3djrjsGOIslE4fAIBeAAMuqKdQTT6ORrnsdGRAD1GObBRu5jQWegny%2F1rNVLVM9vH8csoC0wfzujVBiEaCLBaSAFFidVCaV7RtSYNd1dkuEhV7Oda0MXii2sAydBmFTjZ9veyaFFKOO5Pg1zKTVpD0z8Ba6NJkx3spVmDM158uXbISTmVBqyrH6D1iCt2JLfKX89%2BVymRtJ83ynONrAtcCgOmdgu2MiAZ%2BdZwEpMNy0g8lnVaqyP27sqlj3H2g0YgarQZLimoC%2Bc7AD4AAPj1CnfBDXvaIWzKy%2BiNRW6jRib5A0FgXOGsoj0E%2FHsYbHsBU0htI28jaamf3TmGKgMrOGomSZxFWlQfg6WoCxaBjV7ZxZq1IeIs2g4v0buqLCjDMuRTkKDz3T0Co3MIeg49QGOqUBLxdv6k7p%2Bi2c9J96UFKXvi4YL2ANn1%2BaeWkMHAefgFv8d%2FrfP%2FmS2a7MRpcnCCNJcwR%2BZ4y2CzsXUi96c%2BzGUMibfZOUbIYNGuRc92eD4uHopVxH72G%2BOGAzbQdSqIaXx%2FSwNI9TsKmHVXtbH3PmO2kJmZHpvpuMxI9eGv4UTs92Iw5Yh7Jyy6DmKaAvJF71ZBPdP7lr9MIbz1%2BHfGkobq%2BZLjO0&X-Amz-Signature=29b06410b1453ae9718e0b920df19cf4305f5064d0f311da3ba5ebf69ecfe28e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRF2GEEM%2F20260903%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260903T020153Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIBZ8dXhM6lo20DVrqZ2WaGfVLANf77%2B1NOA2GwJo%2BoveAiEAmYDn5cCoxinJEafwegw2irs%2FgECQo3IZU%2B%2Bq8efaHwEqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF8yR8c3ia78F01SeircA89wpIAz8NKCJrvACZTHiP9xIhn%2F%2BSPkrCTPphcepCny1ImEtiY2wJiF41Py7y7l7efOWHh3nUZd5ZwBccwey%2FEoe%2B8xEBz%2B2xo2jr1yg%2BK13l31QcIt1tcrJLtJDIwIp4ir9P3xpkJGc86eOOqbYExvt5B1y0kJPm4CSoCyunV%2Bm1K99O4AOIGZOF7jmDzv1PSlSdKPC%2BSVCOT5ckGN%2FMNh9bXDsIzwopZrw3HI4h3Lw3djrjsGOIslE4fAIBeAAMuqKdQTT6ORrnsdGRAD1GObBRu5jQWegny%2F1rNVLVM9vH8csoC0wfzujVBiEaCLBaSAFFidVCaV7RtSYNd1dkuEhV7Oda0MXii2sAydBmFTjZ9veyaFFKOO5Pg1zKTVpD0z8Ba6NJkx3spVmDM158uXbISTmVBqyrH6D1iCt2JLfKX89%2BVymRtJ83ynONrAtcCgOmdgu2MiAZ%2BdZwEpMNy0g8lnVaqyP27sqlj3H2g0YgarQZLimoC%2Bc7AD4AAPj1CnfBDXvaIWzKy%2BiNRW6jRib5A0FgXOGsoj0E%2FHsYbHsBU0htI28jaamf3TmGKgMrOGomSZxFWlQfg6WoCxaBjV7ZxZq1IeIs2g4v0buqLCjDMuRTkKDz3T0Co3MIeg49QGOqUBLxdv6k7p%2Bi2c9J96UFKXvi4YL2ANn1%2BaeWkMHAefgFv8d%2FrfP%2FmS2a7MRpcnCCNJcwR%2BZ4y2CzsXUi96c%2BzGUMibfZOUbIYNGuRc92eD4uHopVxH72G%2BOGAzbQdSqIaXx%2FSwNI9TsKmHVXtbH3PmO2kJmZHpvpuMxI9eGv4UTs92Iw5Yh7Jyy6DmKaAvJF71ZBPdP7lr9MIbz1%2BHfGkobq%2BZLjO0&X-Amz-Signature=76c4e800d1dafef03e660f855c9240b70816af525a44ad7e389992b34091c2d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
