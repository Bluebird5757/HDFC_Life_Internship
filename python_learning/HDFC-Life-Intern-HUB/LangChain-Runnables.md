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
fetched_at: '2026-09-20T02:19:39.310Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJOMTEDB%2F20260920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260920T021934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHrw49njasL%2BNYIy3vJkeaaphJU6gz6SEjakvaJsi9iQIgSTh911MBXsVVa%2BrNq1RVq07p7P6bKQ8GKhgLWniyp%2Boq%2FwMIZhAAGgw2Mzc0MjMxODM4MDUiDJYZKVI5oh9P9hIsqSrcA1T9lQP%2Fxfh6c8sECRsGTnDLlGzh6YthjWyzMdll6UyS%2FBmUvRIX5WG45AKZr2j5E9V82Q%2FoPr51D0Yncuy5k9FjRsmM41QQ8vD0qJqM%2BvxNpslmNxSd%2FCuC2XcX0BHjdqtPVnAfpy%2BvlrLj3DE4vzQP9L7xnaD47pDK%2B8wj6XuDYLv5UGXEwCVrY13QmSrEAbc4Z4YsK%2ByTl2jyewqFa5iyYIx3qMd8sGHBbUfvYynR%2BmuU%2B%2Br%2FZdRMOgCa78IgM2%2BW8uJpfK6uUisyA2Q7u3nLtu9Sz5Rb7S00bRLb0T0eyYy7Wb6ciNuZuBh98TuN6m37ZDnxbBqc2jFirVrbv4TBbQJXstTqc0WyaqWEvy%2Fhf5Vz2UMeQ0lYGDwXAqyL51nz%2F5FlP1Gk3J%2BVJEV8uFPj0I6QYnP1mKglsNaiX0Qa%2FkLvzLD7V7Y9otK5nhkXZXqskWZquLfGW%2BKaq8gypnTNp%2BvnXTIHsBaKGCPuUD34th91fAPKt4GjVGd6AItrwPgZeITMQOJXrpPzD%2BND9H1nwC4QFl9FNycS79JZsFH0hbh3kqAv67NTGYIOzqJONAjKJNaaFLKRNU4uminlxSZCSOlYGfXA5cPRQ31vOx%2Fi4KAN6zCMQUIMY19kMNDvu9UGOqUBdJ2e1Ra8j1aKWdSaaj%2BbAjPGX3sDsgmzK5PMZGountbbtrJ6wLf6%2BlWXhpf9XQEnZQ4lR2gbf9zYYx%2FDD5X5xajBVeIDiY14HShPWsyRk161lGOwmIAnNHf5Yho1%2B4PvlUckZMWPUIGovZbq7ctrVMQXAej4QXXGbTja%2BOp8orNh5W%2FdJxLANSnTQHG2Aig85W%2F9p6sACDeA9pNSZCGhwoCcWi%2BW&X-Amz-Signature=d1e1b845840869a8fc3e830c02bffb6e45bddc0509192551c8cc0026e52c215a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJOMTEDB%2F20260920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260920T021934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHrw49njasL%2BNYIy3vJkeaaphJU6gz6SEjakvaJsi9iQIgSTh911MBXsVVa%2BrNq1RVq07p7P6bKQ8GKhgLWniyp%2Boq%2FwMIZhAAGgw2Mzc0MjMxODM4MDUiDJYZKVI5oh9P9hIsqSrcA1T9lQP%2Fxfh6c8sECRsGTnDLlGzh6YthjWyzMdll6UyS%2FBmUvRIX5WG45AKZr2j5E9V82Q%2FoPr51D0Yncuy5k9FjRsmM41QQ8vD0qJqM%2BvxNpslmNxSd%2FCuC2XcX0BHjdqtPVnAfpy%2BvlrLj3DE4vzQP9L7xnaD47pDK%2B8wj6XuDYLv5UGXEwCVrY13QmSrEAbc4Z4YsK%2ByTl2jyewqFa5iyYIx3qMd8sGHBbUfvYynR%2BmuU%2B%2Br%2FZdRMOgCa78IgM2%2BW8uJpfK6uUisyA2Q7u3nLtu9Sz5Rb7S00bRLb0T0eyYy7Wb6ciNuZuBh98TuN6m37ZDnxbBqc2jFirVrbv4TBbQJXstTqc0WyaqWEvy%2Fhf5Vz2UMeQ0lYGDwXAqyL51nz%2F5FlP1Gk3J%2BVJEV8uFPj0I6QYnP1mKglsNaiX0Qa%2FkLvzLD7V7Y9otK5nhkXZXqskWZquLfGW%2BKaq8gypnTNp%2BvnXTIHsBaKGCPuUD34th91fAPKt4GjVGd6AItrwPgZeITMQOJXrpPzD%2BND9H1nwC4QFl9FNycS79JZsFH0hbh3kqAv67NTGYIOzqJONAjKJNaaFLKRNU4uminlxSZCSOlYGfXA5cPRQ31vOx%2Fi4KAN6zCMQUIMY19kMNDvu9UGOqUBdJ2e1Ra8j1aKWdSaaj%2BbAjPGX3sDsgmzK5PMZGountbbtrJ6wLf6%2BlWXhpf9XQEnZQ4lR2gbf9zYYx%2FDD5X5xajBVeIDiY14HShPWsyRk161lGOwmIAnNHf5Yho1%2B4PvlUckZMWPUIGovZbq7ctrVMQXAej4QXXGbTja%2BOp8orNh5W%2FdJxLANSnTQHG2Aig85W%2F9p6sACDeA9pNSZCGhwoCcWi%2BW&X-Amz-Signature=3ea403baac1e7209141bed83aa29ce967807f59af9311f118bc3583898eb69f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJOMTEDB%2F20260920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260920T021934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHrw49njasL%2BNYIy3vJkeaaphJU6gz6SEjakvaJsi9iQIgSTh911MBXsVVa%2BrNq1RVq07p7P6bKQ8GKhgLWniyp%2Boq%2FwMIZhAAGgw2Mzc0MjMxODM4MDUiDJYZKVI5oh9P9hIsqSrcA1T9lQP%2Fxfh6c8sECRsGTnDLlGzh6YthjWyzMdll6UyS%2FBmUvRIX5WG45AKZr2j5E9V82Q%2FoPr51D0Yncuy5k9FjRsmM41QQ8vD0qJqM%2BvxNpslmNxSd%2FCuC2XcX0BHjdqtPVnAfpy%2BvlrLj3DE4vzQP9L7xnaD47pDK%2B8wj6XuDYLv5UGXEwCVrY13QmSrEAbc4Z4YsK%2ByTl2jyewqFa5iyYIx3qMd8sGHBbUfvYynR%2BmuU%2B%2Br%2FZdRMOgCa78IgM2%2BW8uJpfK6uUisyA2Q7u3nLtu9Sz5Rb7S00bRLb0T0eyYy7Wb6ciNuZuBh98TuN6m37ZDnxbBqc2jFirVrbv4TBbQJXstTqc0WyaqWEvy%2Fhf5Vz2UMeQ0lYGDwXAqyL51nz%2F5FlP1Gk3J%2BVJEV8uFPj0I6QYnP1mKglsNaiX0Qa%2FkLvzLD7V7Y9otK5nhkXZXqskWZquLfGW%2BKaq8gypnTNp%2BvnXTIHsBaKGCPuUD34th91fAPKt4GjVGd6AItrwPgZeITMQOJXrpPzD%2BND9H1nwC4QFl9FNycS79JZsFH0hbh3kqAv67NTGYIOzqJONAjKJNaaFLKRNU4uminlxSZCSOlYGfXA5cPRQ31vOx%2Fi4KAN6zCMQUIMY19kMNDvu9UGOqUBdJ2e1Ra8j1aKWdSaaj%2BbAjPGX3sDsgmzK5PMZGountbbtrJ6wLf6%2BlWXhpf9XQEnZQ4lR2gbf9zYYx%2FDD5X5xajBVeIDiY14HShPWsyRk161lGOwmIAnNHf5Yho1%2B4PvlUckZMWPUIGovZbq7ctrVMQXAej4QXXGbTja%2BOp8orNh5W%2FdJxLANSnTQHG2Aig85W%2F9p6sACDeA9pNSZCGhwoCcWi%2BW&X-Amz-Signature=7fd5ae9eb2dd506edc349fdf97bc5c4f64b74545f0261d26227dfa084c9aa263&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJOMTEDB%2F20260920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260920T021934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHrw49njasL%2BNYIy3vJkeaaphJU6gz6SEjakvaJsi9iQIgSTh911MBXsVVa%2BrNq1RVq07p7P6bKQ8GKhgLWniyp%2Boq%2FwMIZhAAGgw2Mzc0MjMxODM4MDUiDJYZKVI5oh9P9hIsqSrcA1T9lQP%2Fxfh6c8sECRsGTnDLlGzh6YthjWyzMdll6UyS%2FBmUvRIX5WG45AKZr2j5E9V82Q%2FoPr51D0Yncuy5k9FjRsmM41QQ8vD0qJqM%2BvxNpslmNxSd%2FCuC2XcX0BHjdqtPVnAfpy%2BvlrLj3DE4vzQP9L7xnaD47pDK%2B8wj6XuDYLv5UGXEwCVrY13QmSrEAbc4Z4YsK%2ByTl2jyewqFa5iyYIx3qMd8sGHBbUfvYynR%2BmuU%2B%2Br%2FZdRMOgCa78IgM2%2BW8uJpfK6uUisyA2Q7u3nLtu9Sz5Rb7S00bRLb0T0eyYy7Wb6ciNuZuBh98TuN6m37ZDnxbBqc2jFirVrbv4TBbQJXstTqc0WyaqWEvy%2Fhf5Vz2UMeQ0lYGDwXAqyL51nz%2F5FlP1Gk3J%2BVJEV8uFPj0I6QYnP1mKglsNaiX0Qa%2FkLvzLD7V7Y9otK5nhkXZXqskWZquLfGW%2BKaq8gypnTNp%2BvnXTIHsBaKGCPuUD34th91fAPKt4GjVGd6AItrwPgZeITMQOJXrpPzD%2BND9H1nwC4QFl9FNycS79JZsFH0hbh3kqAv67NTGYIOzqJONAjKJNaaFLKRNU4uminlxSZCSOlYGfXA5cPRQ31vOx%2Fi4KAN6zCMQUIMY19kMNDvu9UGOqUBdJ2e1Ra8j1aKWdSaaj%2BbAjPGX3sDsgmzK5PMZGountbbtrJ6wLf6%2BlWXhpf9XQEnZQ4lR2gbf9zYYx%2FDD5X5xajBVeIDiY14HShPWsyRk161lGOwmIAnNHf5Yho1%2B4PvlUckZMWPUIGovZbq7ctrVMQXAej4QXXGbTja%2BOp8orNh5W%2FdJxLANSnTQHG2Aig85W%2F9p6sACDeA9pNSZCGhwoCcWi%2BW&X-Amz-Signature=6b0bdd515c3a2e66382322219164d1f57cba674719263af3b49a76d92bdcfc45&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJOMTEDB%2F20260920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260920T021934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHrw49njasL%2BNYIy3vJkeaaphJU6gz6SEjakvaJsi9iQIgSTh911MBXsVVa%2BrNq1RVq07p7P6bKQ8GKhgLWniyp%2Boq%2FwMIZhAAGgw2Mzc0MjMxODM4MDUiDJYZKVI5oh9P9hIsqSrcA1T9lQP%2Fxfh6c8sECRsGTnDLlGzh6YthjWyzMdll6UyS%2FBmUvRIX5WG45AKZr2j5E9V82Q%2FoPr51D0Yncuy5k9FjRsmM41QQ8vD0qJqM%2BvxNpslmNxSd%2FCuC2XcX0BHjdqtPVnAfpy%2BvlrLj3DE4vzQP9L7xnaD47pDK%2B8wj6XuDYLv5UGXEwCVrY13QmSrEAbc4Z4YsK%2ByTl2jyewqFa5iyYIx3qMd8sGHBbUfvYynR%2BmuU%2B%2Br%2FZdRMOgCa78IgM2%2BW8uJpfK6uUisyA2Q7u3nLtu9Sz5Rb7S00bRLb0T0eyYy7Wb6ciNuZuBh98TuN6m37ZDnxbBqc2jFirVrbv4TBbQJXstTqc0WyaqWEvy%2Fhf5Vz2UMeQ0lYGDwXAqyL51nz%2F5FlP1Gk3J%2BVJEV8uFPj0I6QYnP1mKglsNaiX0Qa%2FkLvzLD7V7Y9otK5nhkXZXqskWZquLfGW%2BKaq8gypnTNp%2BvnXTIHsBaKGCPuUD34th91fAPKt4GjVGd6AItrwPgZeITMQOJXrpPzD%2BND9H1nwC4QFl9FNycS79JZsFH0hbh3kqAv67NTGYIOzqJONAjKJNaaFLKRNU4uminlxSZCSOlYGfXA5cPRQ31vOx%2Fi4KAN6zCMQUIMY19kMNDvu9UGOqUBdJ2e1Ra8j1aKWdSaaj%2BbAjPGX3sDsgmzK5PMZGountbbtrJ6wLf6%2BlWXhpf9XQEnZQ4lR2gbf9zYYx%2FDD5X5xajBVeIDiY14HShPWsyRk161lGOwmIAnNHf5Yho1%2B4PvlUckZMWPUIGovZbq7ctrVMQXAej4QXXGbTja%2BOp8orNh5W%2FdJxLANSnTQHG2Aig85W%2F9p6sACDeA9pNSZCGhwoCcWi%2BW&X-Amz-Signature=b23530b18c1a449efd17557aef2b1006c2cb82b9a1789e69b8449c41effe87c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJOMTEDB%2F20260920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260920T021934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHrw49njasL%2BNYIy3vJkeaaphJU6gz6SEjakvaJsi9iQIgSTh911MBXsVVa%2BrNq1RVq07p7P6bKQ8GKhgLWniyp%2Boq%2FwMIZhAAGgw2Mzc0MjMxODM4MDUiDJYZKVI5oh9P9hIsqSrcA1T9lQP%2Fxfh6c8sECRsGTnDLlGzh6YthjWyzMdll6UyS%2FBmUvRIX5WG45AKZr2j5E9V82Q%2FoPr51D0Yncuy5k9FjRsmM41QQ8vD0qJqM%2BvxNpslmNxSd%2FCuC2XcX0BHjdqtPVnAfpy%2BvlrLj3DE4vzQP9L7xnaD47pDK%2B8wj6XuDYLv5UGXEwCVrY13QmSrEAbc4Z4YsK%2ByTl2jyewqFa5iyYIx3qMd8sGHBbUfvYynR%2BmuU%2B%2Br%2FZdRMOgCa78IgM2%2BW8uJpfK6uUisyA2Q7u3nLtu9Sz5Rb7S00bRLb0T0eyYy7Wb6ciNuZuBh98TuN6m37ZDnxbBqc2jFirVrbv4TBbQJXstTqc0WyaqWEvy%2Fhf5Vz2UMeQ0lYGDwXAqyL51nz%2F5FlP1Gk3J%2BVJEV8uFPj0I6QYnP1mKglsNaiX0Qa%2FkLvzLD7V7Y9otK5nhkXZXqskWZquLfGW%2BKaq8gypnTNp%2BvnXTIHsBaKGCPuUD34th91fAPKt4GjVGd6AItrwPgZeITMQOJXrpPzD%2BND9H1nwC4QFl9FNycS79JZsFH0hbh3kqAv67NTGYIOzqJONAjKJNaaFLKRNU4uminlxSZCSOlYGfXA5cPRQ31vOx%2Fi4KAN6zCMQUIMY19kMNDvu9UGOqUBdJ2e1Ra8j1aKWdSaaj%2BbAjPGX3sDsgmzK5PMZGountbbtrJ6wLf6%2BlWXhpf9XQEnZQ4lR2gbf9zYYx%2FDD5X5xajBVeIDiY14HShPWsyRk161lGOwmIAnNHf5Yho1%2B4PvlUckZMWPUIGovZbq7ctrVMQXAej4QXXGbTja%2BOp8orNh5W%2FdJxLANSnTQHG2Aig85W%2F9p6sACDeA9pNSZCGhwoCcWi%2BW&X-Amz-Signature=8601c93b4171e529a90727887a36d5a3fe1d46f5476f6426983c9b3b2606277e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJOMTEDB%2F20260920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260920T021934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHrw49njasL%2BNYIy3vJkeaaphJU6gz6SEjakvaJsi9iQIgSTh911MBXsVVa%2BrNq1RVq07p7P6bKQ8GKhgLWniyp%2Boq%2FwMIZhAAGgw2Mzc0MjMxODM4MDUiDJYZKVI5oh9P9hIsqSrcA1T9lQP%2Fxfh6c8sECRsGTnDLlGzh6YthjWyzMdll6UyS%2FBmUvRIX5WG45AKZr2j5E9V82Q%2FoPr51D0Yncuy5k9FjRsmM41QQ8vD0qJqM%2BvxNpslmNxSd%2FCuC2XcX0BHjdqtPVnAfpy%2BvlrLj3DE4vzQP9L7xnaD47pDK%2B8wj6XuDYLv5UGXEwCVrY13QmSrEAbc4Z4YsK%2ByTl2jyewqFa5iyYIx3qMd8sGHBbUfvYynR%2BmuU%2B%2Br%2FZdRMOgCa78IgM2%2BW8uJpfK6uUisyA2Q7u3nLtu9Sz5Rb7S00bRLb0T0eyYy7Wb6ciNuZuBh98TuN6m37ZDnxbBqc2jFirVrbv4TBbQJXstTqc0WyaqWEvy%2Fhf5Vz2UMeQ0lYGDwXAqyL51nz%2F5FlP1Gk3J%2BVJEV8uFPj0I6QYnP1mKglsNaiX0Qa%2FkLvzLD7V7Y9otK5nhkXZXqskWZquLfGW%2BKaq8gypnTNp%2BvnXTIHsBaKGCPuUD34th91fAPKt4GjVGd6AItrwPgZeITMQOJXrpPzD%2BND9H1nwC4QFl9FNycS79JZsFH0hbh3kqAv67NTGYIOzqJONAjKJNaaFLKRNU4uminlxSZCSOlYGfXA5cPRQ31vOx%2Fi4KAN6zCMQUIMY19kMNDvu9UGOqUBdJ2e1Ra8j1aKWdSaaj%2BbAjPGX3sDsgmzK5PMZGountbbtrJ6wLf6%2BlWXhpf9XQEnZQ4lR2gbf9zYYx%2FDD5X5xajBVeIDiY14HShPWsyRk161lGOwmIAnNHf5Yho1%2B4PvlUckZMWPUIGovZbq7ctrVMQXAej4QXXGbTja%2BOp8orNh5W%2FdJxLANSnTQHG2Aig85W%2F9p6sACDeA9pNSZCGhwoCcWi%2BW&X-Amz-Signature=e7fda20e1c516e70307ac61168f15b79e1a87311065ddfde3afc886f7b87f844&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJOMTEDB%2F20260920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260920T021934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHrw49njasL%2BNYIy3vJkeaaphJU6gz6SEjakvaJsi9iQIgSTh911MBXsVVa%2BrNq1RVq07p7P6bKQ8GKhgLWniyp%2Boq%2FwMIZhAAGgw2Mzc0MjMxODM4MDUiDJYZKVI5oh9P9hIsqSrcA1T9lQP%2Fxfh6c8sECRsGTnDLlGzh6YthjWyzMdll6UyS%2FBmUvRIX5WG45AKZr2j5E9V82Q%2FoPr51D0Yncuy5k9FjRsmM41QQ8vD0qJqM%2BvxNpslmNxSd%2FCuC2XcX0BHjdqtPVnAfpy%2BvlrLj3DE4vzQP9L7xnaD47pDK%2B8wj6XuDYLv5UGXEwCVrY13QmSrEAbc4Z4YsK%2ByTl2jyewqFa5iyYIx3qMd8sGHBbUfvYynR%2BmuU%2B%2Br%2FZdRMOgCa78IgM2%2BW8uJpfK6uUisyA2Q7u3nLtu9Sz5Rb7S00bRLb0T0eyYy7Wb6ciNuZuBh98TuN6m37ZDnxbBqc2jFirVrbv4TBbQJXstTqc0WyaqWEvy%2Fhf5Vz2UMeQ0lYGDwXAqyL51nz%2F5FlP1Gk3J%2BVJEV8uFPj0I6QYnP1mKglsNaiX0Qa%2FkLvzLD7V7Y9otK5nhkXZXqskWZquLfGW%2BKaq8gypnTNp%2BvnXTIHsBaKGCPuUD34th91fAPKt4GjVGd6AItrwPgZeITMQOJXrpPzD%2BND9H1nwC4QFl9FNycS79JZsFH0hbh3kqAv67NTGYIOzqJONAjKJNaaFLKRNU4uminlxSZCSOlYGfXA5cPRQ31vOx%2Fi4KAN6zCMQUIMY19kMNDvu9UGOqUBdJ2e1Ra8j1aKWdSaaj%2BbAjPGX3sDsgmzK5PMZGountbbtrJ6wLf6%2BlWXhpf9XQEnZQ4lR2gbf9zYYx%2FDD5X5xajBVeIDiY14HShPWsyRk161lGOwmIAnNHf5Yho1%2B4PvlUckZMWPUIGovZbq7ctrVMQXAej4QXXGbTja%2BOp8orNh5W%2FdJxLANSnTQHG2Aig85W%2F9p6sACDeA9pNSZCGhwoCcWi%2BW&X-Amz-Signature=994beb65ba2687ab0bdd27bd07d2feabdbcbb3c629b18e6e4e9026620c13a2c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJOMTEDB%2F20260920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260920T021934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHrw49njasL%2BNYIy3vJkeaaphJU6gz6SEjakvaJsi9iQIgSTh911MBXsVVa%2BrNq1RVq07p7P6bKQ8GKhgLWniyp%2Boq%2FwMIZhAAGgw2Mzc0MjMxODM4MDUiDJYZKVI5oh9P9hIsqSrcA1T9lQP%2Fxfh6c8sECRsGTnDLlGzh6YthjWyzMdll6UyS%2FBmUvRIX5WG45AKZr2j5E9V82Q%2FoPr51D0Yncuy5k9FjRsmM41QQ8vD0qJqM%2BvxNpslmNxSd%2FCuC2XcX0BHjdqtPVnAfpy%2BvlrLj3DE4vzQP9L7xnaD47pDK%2B8wj6XuDYLv5UGXEwCVrY13QmSrEAbc4Z4YsK%2ByTl2jyewqFa5iyYIx3qMd8sGHBbUfvYynR%2BmuU%2B%2Br%2FZdRMOgCa78IgM2%2BW8uJpfK6uUisyA2Q7u3nLtu9Sz5Rb7S00bRLb0T0eyYy7Wb6ciNuZuBh98TuN6m37ZDnxbBqc2jFirVrbv4TBbQJXstTqc0WyaqWEvy%2Fhf5Vz2UMeQ0lYGDwXAqyL51nz%2F5FlP1Gk3J%2BVJEV8uFPj0I6QYnP1mKglsNaiX0Qa%2FkLvzLD7V7Y9otK5nhkXZXqskWZquLfGW%2BKaq8gypnTNp%2BvnXTIHsBaKGCPuUD34th91fAPKt4GjVGd6AItrwPgZeITMQOJXrpPzD%2BND9H1nwC4QFl9FNycS79JZsFH0hbh3kqAv67NTGYIOzqJONAjKJNaaFLKRNU4uminlxSZCSOlYGfXA5cPRQ31vOx%2Fi4KAN6zCMQUIMY19kMNDvu9UGOqUBdJ2e1Ra8j1aKWdSaaj%2BbAjPGX3sDsgmzK5PMZGountbbtrJ6wLf6%2BlWXhpf9XQEnZQ4lR2gbf9zYYx%2FDD5X5xajBVeIDiY14HShPWsyRk161lGOwmIAnNHf5Yho1%2B4PvlUckZMWPUIGovZbq7ctrVMQXAej4QXXGbTja%2BOp8orNh5W%2FdJxLANSnTQHG2Aig85W%2F9p6sACDeA9pNSZCGhwoCcWi%2BW&X-Amz-Signature=4113730b5345c9b63022e17cc0cc98e7c462b89638ba23afeda89119c6816bdd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
