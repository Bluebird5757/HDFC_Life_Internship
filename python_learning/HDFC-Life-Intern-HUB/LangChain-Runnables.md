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
fetched_at: '2026-10-17T02:05:45.607Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPRVOTM7%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIH%2FvsbGmxmarVTkavP0B6MpZ30Ab%2Fw69ECSAx4IJ%2FosTAiEA1%2F1NGVklYsPzGWaTMoSzlnrV5ohpWYwXDaIyC2rJWLAqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFappTc4sqHYDKs9fyrcA89GPSbG9i9wbM%2F%2FEUDAhdN35TA4WRrza7Dd8PLaRyoSsdbZtV%2FGbWSsnqR389mcjIHwMHOjpLi6deh8KmVfYnOWxovPQBMqrRng9RDEsWTjLoKrW%2FkOXqU56RN%2Bq%2Fz7h0uwuPZPaeNCxloC49kRE0P5jv7uKjUoTXdJ%2BGzG%2BQdbzs%2B5AW6RiG4WRs6cBz2OsAhEurbPcZA%2FOZpV1vcaaZ6c5G0%2FZbPCTiC1hbPIK5%2BavaPzQ3HOsdK8EgzWvQnmebWDLNiSKxqMDbN8TmMXDrYHi0KuOUcwh9duX5NmZubFRK9oeDIDcSen74d1J1HEJA3YnFj0eOAy9nu4%2FWwUoW9di3kXKYH2lqub90SPI0VsqTk2XAxD3Qmn5usPK1K7oIb6tsclIUrZre1D141K%2F9bFHFKzd2Vnznuy7FwmqenaVThngygI846PnSiJzNXPHfevQXnlciGSpCsL9sZwFsdyfaN%2B7L33s7CiGMv4IwrdS%2F8eTVLPb9nh6%2FH8Q3%2BxCXLvRWy0ax19riUFCiDU%2FJzabxxDLMzO5KDdfwljbM6cgo%2BvMFvUMg9cQRHhgsIDAkUcVn%2FKu0hLc1eM6dSzJKCCT8j4%2FMtDQONUcVZI9LgqzvE21elPMDfojoYAMLPgytYGOqUBxjaFoyOVC2RSDO65MjU8elw8DW6hjbIdeFwsJx47YIaWPffbpJXVba8jXhqdIDX8wTJI0C8KnWCnfO5sF9eypygyanv%2BMygT8iDYEtHNoUgYzPQd3AnbNT%2B4KQimYT8nkcU4u%2FGW82G4F0xfbCk2Va%2F4WgynJAC3zN5UP%2BGIiImtTijyXcRYsTsR785%2FYpy4f6Bdzaq4FxWInN2iQwsBEX8EywN1&X-Amz-Signature=ba62f31872922734c8aeaa47d159523e21d7f68e2a6d7593aff5d4df2147fa5f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPRVOTM7%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIH%2FvsbGmxmarVTkavP0B6MpZ30Ab%2Fw69ECSAx4IJ%2FosTAiEA1%2F1NGVklYsPzGWaTMoSzlnrV5ohpWYwXDaIyC2rJWLAqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFappTc4sqHYDKs9fyrcA89GPSbG9i9wbM%2F%2FEUDAhdN35TA4WRrza7Dd8PLaRyoSsdbZtV%2FGbWSsnqR389mcjIHwMHOjpLi6deh8KmVfYnOWxovPQBMqrRng9RDEsWTjLoKrW%2FkOXqU56RN%2Bq%2Fz7h0uwuPZPaeNCxloC49kRE0P5jv7uKjUoTXdJ%2BGzG%2BQdbzs%2B5AW6RiG4WRs6cBz2OsAhEurbPcZA%2FOZpV1vcaaZ6c5G0%2FZbPCTiC1hbPIK5%2BavaPzQ3HOsdK8EgzWvQnmebWDLNiSKxqMDbN8TmMXDrYHi0KuOUcwh9duX5NmZubFRK9oeDIDcSen74d1J1HEJA3YnFj0eOAy9nu4%2FWwUoW9di3kXKYH2lqub90SPI0VsqTk2XAxD3Qmn5usPK1K7oIb6tsclIUrZre1D141K%2F9bFHFKzd2Vnznuy7FwmqenaVThngygI846PnSiJzNXPHfevQXnlciGSpCsL9sZwFsdyfaN%2B7L33s7CiGMv4IwrdS%2F8eTVLPb9nh6%2FH8Q3%2BxCXLvRWy0ax19riUFCiDU%2FJzabxxDLMzO5KDdfwljbM6cgo%2BvMFvUMg9cQRHhgsIDAkUcVn%2FKu0hLc1eM6dSzJKCCT8j4%2FMtDQONUcVZI9LgqzvE21elPMDfojoYAMLPgytYGOqUBxjaFoyOVC2RSDO65MjU8elw8DW6hjbIdeFwsJx47YIaWPffbpJXVba8jXhqdIDX8wTJI0C8KnWCnfO5sF9eypygyanv%2BMygT8iDYEtHNoUgYzPQd3AnbNT%2B4KQimYT8nkcU4u%2FGW82G4F0xfbCk2Va%2F4WgynJAC3zN5UP%2BGIiImtTijyXcRYsTsR785%2FYpy4f6Bdzaq4FxWInN2iQwsBEX8EywN1&X-Amz-Signature=232281ff623798112927f21ddb7303fc83baf6b22e0c36ea151cf11cda1b1886&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPRVOTM7%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIH%2FvsbGmxmarVTkavP0B6MpZ30Ab%2Fw69ECSAx4IJ%2FosTAiEA1%2F1NGVklYsPzGWaTMoSzlnrV5ohpWYwXDaIyC2rJWLAqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFappTc4sqHYDKs9fyrcA89GPSbG9i9wbM%2F%2FEUDAhdN35TA4WRrza7Dd8PLaRyoSsdbZtV%2FGbWSsnqR389mcjIHwMHOjpLi6deh8KmVfYnOWxovPQBMqrRng9RDEsWTjLoKrW%2FkOXqU56RN%2Bq%2Fz7h0uwuPZPaeNCxloC49kRE0P5jv7uKjUoTXdJ%2BGzG%2BQdbzs%2B5AW6RiG4WRs6cBz2OsAhEurbPcZA%2FOZpV1vcaaZ6c5G0%2FZbPCTiC1hbPIK5%2BavaPzQ3HOsdK8EgzWvQnmebWDLNiSKxqMDbN8TmMXDrYHi0KuOUcwh9duX5NmZubFRK9oeDIDcSen74d1J1HEJA3YnFj0eOAy9nu4%2FWwUoW9di3kXKYH2lqub90SPI0VsqTk2XAxD3Qmn5usPK1K7oIb6tsclIUrZre1D141K%2F9bFHFKzd2Vnznuy7FwmqenaVThngygI846PnSiJzNXPHfevQXnlciGSpCsL9sZwFsdyfaN%2B7L33s7CiGMv4IwrdS%2F8eTVLPb9nh6%2FH8Q3%2BxCXLvRWy0ax19riUFCiDU%2FJzabxxDLMzO5KDdfwljbM6cgo%2BvMFvUMg9cQRHhgsIDAkUcVn%2FKu0hLc1eM6dSzJKCCT8j4%2FMtDQONUcVZI9LgqzvE21elPMDfojoYAMLPgytYGOqUBxjaFoyOVC2RSDO65MjU8elw8DW6hjbIdeFwsJx47YIaWPffbpJXVba8jXhqdIDX8wTJI0C8KnWCnfO5sF9eypygyanv%2BMygT8iDYEtHNoUgYzPQd3AnbNT%2B4KQimYT8nkcU4u%2FGW82G4F0xfbCk2Va%2F4WgynJAC3zN5UP%2BGIiImtTijyXcRYsTsR785%2FYpy4f6Bdzaq4FxWInN2iQwsBEX8EywN1&X-Amz-Signature=62943b6a2873147cf1ef9924311c9c7ef38e8cbe976775d1f75e918902785387&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPRVOTM7%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIH%2FvsbGmxmarVTkavP0B6MpZ30Ab%2Fw69ECSAx4IJ%2FosTAiEA1%2F1NGVklYsPzGWaTMoSzlnrV5ohpWYwXDaIyC2rJWLAqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFappTc4sqHYDKs9fyrcA89GPSbG9i9wbM%2F%2FEUDAhdN35TA4WRrza7Dd8PLaRyoSsdbZtV%2FGbWSsnqR389mcjIHwMHOjpLi6deh8KmVfYnOWxovPQBMqrRng9RDEsWTjLoKrW%2FkOXqU56RN%2Bq%2Fz7h0uwuPZPaeNCxloC49kRE0P5jv7uKjUoTXdJ%2BGzG%2BQdbzs%2B5AW6RiG4WRs6cBz2OsAhEurbPcZA%2FOZpV1vcaaZ6c5G0%2FZbPCTiC1hbPIK5%2BavaPzQ3HOsdK8EgzWvQnmebWDLNiSKxqMDbN8TmMXDrYHi0KuOUcwh9duX5NmZubFRK9oeDIDcSen74d1J1HEJA3YnFj0eOAy9nu4%2FWwUoW9di3kXKYH2lqub90SPI0VsqTk2XAxD3Qmn5usPK1K7oIb6tsclIUrZre1D141K%2F9bFHFKzd2Vnznuy7FwmqenaVThngygI846PnSiJzNXPHfevQXnlciGSpCsL9sZwFsdyfaN%2B7L33s7CiGMv4IwrdS%2F8eTVLPb9nh6%2FH8Q3%2BxCXLvRWy0ax19riUFCiDU%2FJzabxxDLMzO5KDdfwljbM6cgo%2BvMFvUMg9cQRHhgsIDAkUcVn%2FKu0hLc1eM6dSzJKCCT8j4%2FMtDQONUcVZI9LgqzvE21elPMDfojoYAMLPgytYGOqUBxjaFoyOVC2RSDO65MjU8elw8DW6hjbIdeFwsJx47YIaWPffbpJXVba8jXhqdIDX8wTJI0C8KnWCnfO5sF9eypygyanv%2BMygT8iDYEtHNoUgYzPQd3AnbNT%2B4KQimYT8nkcU4u%2FGW82G4F0xfbCk2Va%2F4WgynJAC3zN5UP%2BGIiImtTijyXcRYsTsR785%2FYpy4f6Bdzaq4FxWInN2iQwsBEX8EywN1&X-Amz-Signature=c3cee60d083da1a577fdf99d75ac815f66b743f84f8f62454fa1f54b8a63080d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPRVOTM7%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIH%2FvsbGmxmarVTkavP0B6MpZ30Ab%2Fw69ECSAx4IJ%2FosTAiEA1%2F1NGVklYsPzGWaTMoSzlnrV5ohpWYwXDaIyC2rJWLAqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFappTc4sqHYDKs9fyrcA89GPSbG9i9wbM%2F%2FEUDAhdN35TA4WRrza7Dd8PLaRyoSsdbZtV%2FGbWSsnqR389mcjIHwMHOjpLi6deh8KmVfYnOWxovPQBMqrRng9RDEsWTjLoKrW%2FkOXqU56RN%2Bq%2Fz7h0uwuPZPaeNCxloC49kRE0P5jv7uKjUoTXdJ%2BGzG%2BQdbzs%2B5AW6RiG4WRs6cBz2OsAhEurbPcZA%2FOZpV1vcaaZ6c5G0%2FZbPCTiC1hbPIK5%2BavaPzQ3HOsdK8EgzWvQnmebWDLNiSKxqMDbN8TmMXDrYHi0KuOUcwh9duX5NmZubFRK9oeDIDcSen74d1J1HEJA3YnFj0eOAy9nu4%2FWwUoW9di3kXKYH2lqub90SPI0VsqTk2XAxD3Qmn5usPK1K7oIb6tsclIUrZre1D141K%2F9bFHFKzd2Vnznuy7FwmqenaVThngygI846PnSiJzNXPHfevQXnlciGSpCsL9sZwFsdyfaN%2B7L33s7CiGMv4IwrdS%2F8eTVLPb9nh6%2FH8Q3%2BxCXLvRWy0ax19riUFCiDU%2FJzabxxDLMzO5KDdfwljbM6cgo%2BvMFvUMg9cQRHhgsIDAkUcVn%2FKu0hLc1eM6dSzJKCCT8j4%2FMtDQONUcVZI9LgqzvE21elPMDfojoYAMLPgytYGOqUBxjaFoyOVC2RSDO65MjU8elw8DW6hjbIdeFwsJx47YIaWPffbpJXVba8jXhqdIDX8wTJI0C8KnWCnfO5sF9eypygyanv%2BMygT8iDYEtHNoUgYzPQd3AnbNT%2B4KQimYT8nkcU4u%2FGW82G4F0xfbCk2Va%2F4WgynJAC3zN5UP%2BGIiImtTijyXcRYsTsR785%2FYpy4f6Bdzaq4FxWInN2iQwsBEX8EywN1&X-Amz-Signature=c5289a89f1153cf3960620c30de03100dceee9dae172d3220fecc1f89780a890&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPRVOTM7%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIH%2FvsbGmxmarVTkavP0B6MpZ30Ab%2Fw69ECSAx4IJ%2FosTAiEA1%2F1NGVklYsPzGWaTMoSzlnrV5ohpWYwXDaIyC2rJWLAqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFappTc4sqHYDKs9fyrcA89GPSbG9i9wbM%2F%2FEUDAhdN35TA4WRrza7Dd8PLaRyoSsdbZtV%2FGbWSsnqR389mcjIHwMHOjpLi6deh8KmVfYnOWxovPQBMqrRng9RDEsWTjLoKrW%2FkOXqU56RN%2Bq%2Fz7h0uwuPZPaeNCxloC49kRE0P5jv7uKjUoTXdJ%2BGzG%2BQdbzs%2B5AW6RiG4WRs6cBz2OsAhEurbPcZA%2FOZpV1vcaaZ6c5G0%2FZbPCTiC1hbPIK5%2BavaPzQ3HOsdK8EgzWvQnmebWDLNiSKxqMDbN8TmMXDrYHi0KuOUcwh9duX5NmZubFRK9oeDIDcSen74d1J1HEJA3YnFj0eOAy9nu4%2FWwUoW9di3kXKYH2lqub90SPI0VsqTk2XAxD3Qmn5usPK1K7oIb6tsclIUrZre1D141K%2F9bFHFKzd2Vnznuy7FwmqenaVThngygI846PnSiJzNXPHfevQXnlciGSpCsL9sZwFsdyfaN%2B7L33s7CiGMv4IwrdS%2F8eTVLPb9nh6%2FH8Q3%2BxCXLvRWy0ax19riUFCiDU%2FJzabxxDLMzO5KDdfwljbM6cgo%2BvMFvUMg9cQRHhgsIDAkUcVn%2FKu0hLc1eM6dSzJKCCT8j4%2FMtDQONUcVZI9LgqzvE21elPMDfojoYAMLPgytYGOqUBxjaFoyOVC2RSDO65MjU8elw8DW6hjbIdeFwsJx47YIaWPffbpJXVba8jXhqdIDX8wTJI0C8KnWCnfO5sF9eypygyanv%2BMygT8iDYEtHNoUgYzPQd3AnbNT%2B4KQimYT8nkcU4u%2FGW82G4F0xfbCk2Va%2F4WgynJAC3zN5UP%2BGIiImtTijyXcRYsTsR785%2FYpy4f6Bdzaq4FxWInN2iQwsBEX8EywN1&X-Amz-Signature=d507e093823fe1dd30b41682a4e984b72d924705c0dc73c57cb017ca92237bd8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPRVOTM7%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIH%2FvsbGmxmarVTkavP0B6MpZ30Ab%2Fw69ECSAx4IJ%2FosTAiEA1%2F1NGVklYsPzGWaTMoSzlnrV5ohpWYwXDaIyC2rJWLAqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFappTc4sqHYDKs9fyrcA89GPSbG9i9wbM%2F%2FEUDAhdN35TA4WRrza7Dd8PLaRyoSsdbZtV%2FGbWSsnqR389mcjIHwMHOjpLi6deh8KmVfYnOWxovPQBMqrRng9RDEsWTjLoKrW%2FkOXqU56RN%2Bq%2Fz7h0uwuPZPaeNCxloC49kRE0P5jv7uKjUoTXdJ%2BGzG%2BQdbzs%2B5AW6RiG4WRs6cBz2OsAhEurbPcZA%2FOZpV1vcaaZ6c5G0%2FZbPCTiC1hbPIK5%2BavaPzQ3HOsdK8EgzWvQnmebWDLNiSKxqMDbN8TmMXDrYHi0KuOUcwh9duX5NmZubFRK9oeDIDcSen74d1J1HEJA3YnFj0eOAy9nu4%2FWwUoW9di3kXKYH2lqub90SPI0VsqTk2XAxD3Qmn5usPK1K7oIb6tsclIUrZre1D141K%2F9bFHFKzd2Vnznuy7FwmqenaVThngygI846PnSiJzNXPHfevQXnlciGSpCsL9sZwFsdyfaN%2B7L33s7CiGMv4IwrdS%2F8eTVLPb9nh6%2FH8Q3%2BxCXLvRWy0ax19riUFCiDU%2FJzabxxDLMzO5KDdfwljbM6cgo%2BvMFvUMg9cQRHhgsIDAkUcVn%2FKu0hLc1eM6dSzJKCCT8j4%2FMtDQONUcVZI9LgqzvE21elPMDfojoYAMLPgytYGOqUBxjaFoyOVC2RSDO65MjU8elw8DW6hjbIdeFwsJx47YIaWPffbpJXVba8jXhqdIDX8wTJI0C8KnWCnfO5sF9eypygyanv%2BMygT8iDYEtHNoUgYzPQd3AnbNT%2B4KQimYT8nkcU4u%2FGW82G4F0xfbCk2Va%2F4WgynJAC3zN5UP%2BGIiImtTijyXcRYsTsR785%2FYpy4f6Bdzaq4FxWInN2iQwsBEX8EywN1&X-Amz-Signature=a88a268761841d3fb1df76f533c8e4cb29cbcfc0655f46135c2f5a02e321d7d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPRVOTM7%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIH%2FvsbGmxmarVTkavP0B6MpZ30Ab%2Fw69ECSAx4IJ%2FosTAiEA1%2F1NGVklYsPzGWaTMoSzlnrV5ohpWYwXDaIyC2rJWLAqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFappTc4sqHYDKs9fyrcA89GPSbG9i9wbM%2F%2FEUDAhdN35TA4WRrza7Dd8PLaRyoSsdbZtV%2FGbWSsnqR389mcjIHwMHOjpLi6deh8KmVfYnOWxovPQBMqrRng9RDEsWTjLoKrW%2FkOXqU56RN%2Bq%2Fz7h0uwuPZPaeNCxloC49kRE0P5jv7uKjUoTXdJ%2BGzG%2BQdbzs%2B5AW6RiG4WRs6cBz2OsAhEurbPcZA%2FOZpV1vcaaZ6c5G0%2FZbPCTiC1hbPIK5%2BavaPzQ3HOsdK8EgzWvQnmebWDLNiSKxqMDbN8TmMXDrYHi0KuOUcwh9duX5NmZubFRK9oeDIDcSen74d1J1HEJA3YnFj0eOAy9nu4%2FWwUoW9di3kXKYH2lqub90SPI0VsqTk2XAxD3Qmn5usPK1K7oIb6tsclIUrZre1D141K%2F9bFHFKzd2Vnznuy7FwmqenaVThngygI846PnSiJzNXPHfevQXnlciGSpCsL9sZwFsdyfaN%2B7L33s7CiGMv4IwrdS%2F8eTVLPb9nh6%2FH8Q3%2BxCXLvRWy0ax19riUFCiDU%2FJzabxxDLMzO5KDdfwljbM6cgo%2BvMFvUMg9cQRHhgsIDAkUcVn%2FKu0hLc1eM6dSzJKCCT8j4%2FMtDQONUcVZI9LgqzvE21elPMDfojoYAMLPgytYGOqUBxjaFoyOVC2RSDO65MjU8elw8DW6hjbIdeFwsJx47YIaWPffbpJXVba8jXhqdIDX8wTJI0C8KnWCnfO5sF9eypygyanv%2BMygT8iDYEtHNoUgYzPQd3AnbNT%2B4KQimYT8nkcU4u%2FGW82G4F0xfbCk2Va%2F4WgynJAC3zN5UP%2BGIiImtTijyXcRYsTsR785%2FYpy4f6Bdzaq4FxWInN2iQwsBEX8EywN1&X-Amz-Signature=950b2869b4706c419de1cc3111290c5c0d43a37765ce4d680ed2491627dea815&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPRVOTM7%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIH%2FvsbGmxmarVTkavP0B6MpZ30Ab%2Fw69ECSAx4IJ%2FosTAiEA1%2F1NGVklYsPzGWaTMoSzlnrV5ohpWYwXDaIyC2rJWLAqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFappTc4sqHYDKs9fyrcA89GPSbG9i9wbM%2F%2FEUDAhdN35TA4WRrza7Dd8PLaRyoSsdbZtV%2FGbWSsnqR389mcjIHwMHOjpLi6deh8KmVfYnOWxovPQBMqrRng9RDEsWTjLoKrW%2FkOXqU56RN%2Bq%2Fz7h0uwuPZPaeNCxloC49kRE0P5jv7uKjUoTXdJ%2BGzG%2BQdbzs%2B5AW6RiG4WRs6cBz2OsAhEurbPcZA%2FOZpV1vcaaZ6c5G0%2FZbPCTiC1hbPIK5%2BavaPzQ3HOsdK8EgzWvQnmebWDLNiSKxqMDbN8TmMXDrYHi0KuOUcwh9duX5NmZubFRK9oeDIDcSen74d1J1HEJA3YnFj0eOAy9nu4%2FWwUoW9di3kXKYH2lqub90SPI0VsqTk2XAxD3Qmn5usPK1K7oIb6tsclIUrZre1D141K%2F9bFHFKzd2Vnznuy7FwmqenaVThngygI846PnSiJzNXPHfevQXnlciGSpCsL9sZwFsdyfaN%2B7L33s7CiGMv4IwrdS%2F8eTVLPb9nh6%2FH8Q3%2BxCXLvRWy0ax19riUFCiDU%2FJzabxxDLMzO5KDdfwljbM6cgo%2BvMFvUMg9cQRHhgsIDAkUcVn%2FKu0hLc1eM6dSzJKCCT8j4%2FMtDQONUcVZI9LgqzvE21elPMDfojoYAMLPgytYGOqUBxjaFoyOVC2RSDO65MjU8elw8DW6hjbIdeFwsJx47YIaWPffbpJXVba8jXhqdIDX8wTJI0C8KnWCnfO5sF9eypygyanv%2BMygT8iDYEtHNoUgYzPQd3AnbNT%2B4KQimYT8nkcU4u%2FGW82G4F0xfbCk2Va%2F4WgynJAC3zN5UP%2BGIiImtTijyXcRYsTsR785%2FYpy4f6Bdzaq4FxWInN2iQwsBEX8EywN1&X-Amz-Signature=b8b614fcb66875d05934f503cb09abd01085ee42ab493cb56a8a9073379e2dfb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
