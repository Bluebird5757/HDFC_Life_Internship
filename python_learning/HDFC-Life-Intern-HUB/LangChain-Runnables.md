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
fetched_at: '2026-08-29T04:45:31.943Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYIUAJEN%2F20260829%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260829T044528Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDDhx7Q5hR695C8WKMuOVZ6bGvnGzx8%2FhsJslEZC4guHAIgeqPcps5aQaj%2FUPdYamHxMHxWBEMGW%2FlwfjytQhUtFZQq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDLDi4lQNjhn9jSix3ircAy3CTjjND0zP5qXKwoJnp86zd8sfGQ2mI2rinaDGAywzftstR3V%2Bv1XcWjsqkwV9HizQC1km47silGCDRMdf%2Ff%2BRdtntJ9zkXeJQOjQZgfWQLtp2ZIpDsGrLaHhjWyoomKF1PeX4xeYpyLtQBf5m16DRuogQ84WcohoSKC5kscjvQc1uoyn7iIpluAeHW4BHFMI7ieJHTQecXyg2pUPDH9JS5hlX6ee8s%2FmoI79Z3OxRgtpd9ygfJCQFvbZTIg4fa%2B%2B9EubTbtWYMQBn08FAHzP%2B5vBi4BlqM9kgys%2FH9kTd8VLzxBdz8dNXrydsw2gphT%2F6rK9gx%2BlUWMn%2FPJQO2r7HP%2FqR7o76wy3wfky2LdU3Jltt07S1jo0xwvDZFxUHcmFjpm%2BsyEvTakJPS6VIqkKH5wNDJoyMWQ%2BMiWxm2gqs51dkFW1cv%2F6l2%2B62wsTLuwP%2B1ZMVgLikkIwtMblaaMhAXRN2kWs0TcISl%2BpRL%2BQLSo5zbknm1r3j5cfwWt%2B%2FlCMTCFY4mgrkXDvxN2YnzBodGlp92Y61bNc6Y%2FcdU00q2M%2FC4kinYw5CRn1rTxMamJNqreWLi5Qt3i91Lc5LT36Wsw6hukoBXFIfRLHZl7r%2F2i5RwHvFlWBSjEntMMK6ydQGOqUBfzVHKcHm5fZ6zU9oXB1fYO0fK0BkVjAzMBzyWu9B9eysj7kPNp4g59Hg8vRGJRnXKz5M%2F6Soa6LFWxsT1aN2aQjDNAaEcPOMYJXrGwNeICj8J9PJEByA19rzzbh1sq%2BwR7Q11BfJlMgThWCbNTcgis%2Br8%2BtLZSlmPTHiAXEE%2Bq3yrMuU82a%2FTi8%2F5h967UIHtmze91Lo1f5g7GizI48CpfpbAIlp&X-Amz-Signature=3f8594d9ad79647bf2b6a07e9d202f56fd45552e7dd79b6ef77e070477a9f6ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYIUAJEN%2F20260829%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260829T044528Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDDhx7Q5hR695C8WKMuOVZ6bGvnGzx8%2FhsJslEZC4guHAIgeqPcps5aQaj%2FUPdYamHxMHxWBEMGW%2FlwfjytQhUtFZQq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDLDi4lQNjhn9jSix3ircAy3CTjjND0zP5qXKwoJnp86zd8sfGQ2mI2rinaDGAywzftstR3V%2Bv1XcWjsqkwV9HizQC1km47silGCDRMdf%2Ff%2BRdtntJ9zkXeJQOjQZgfWQLtp2ZIpDsGrLaHhjWyoomKF1PeX4xeYpyLtQBf5m16DRuogQ84WcohoSKC5kscjvQc1uoyn7iIpluAeHW4BHFMI7ieJHTQecXyg2pUPDH9JS5hlX6ee8s%2FmoI79Z3OxRgtpd9ygfJCQFvbZTIg4fa%2B%2B9EubTbtWYMQBn08FAHzP%2B5vBi4BlqM9kgys%2FH9kTd8VLzxBdz8dNXrydsw2gphT%2F6rK9gx%2BlUWMn%2FPJQO2r7HP%2FqR7o76wy3wfky2LdU3Jltt07S1jo0xwvDZFxUHcmFjpm%2BsyEvTakJPS6VIqkKH5wNDJoyMWQ%2BMiWxm2gqs51dkFW1cv%2F6l2%2B62wsTLuwP%2B1ZMVgLikkIwtMblaaMhAXRN2kWs0TcISl%2BpRL%2BQLSo5zbknm1r3j5cfwWt%2B%2FlCMTCFY4mgrkXDvxN2YnzBodGlp92Y61bNc6Y%2FcdU00q2M%2FC4kinYw5CRn1rTxMamJNqreWLi5Qt3i91Lc5LT36Wsw6hukoBXFIfRLHZl7r%2F2i5RwHvFlWBSjEntMMK6ydQGOqUBfzVHKcHm5fZ6zU9oXB1fYO0fK0BkVjAzMBzyWu9B9eysj7kPNp4g59Hg8vRGJRnXKz5M%2F6Soa6LFWxsT1aN2aQjDNAaEcPOMYJXrGwNeICj8J9PJEByA19rzzbh1sq%2BwR7Q11BfJlMgThWCbNTcgis%2Br8%2BtLZSlmPTHiAXEE%2Bq3yrMuU82a%2FTi8%2F5h967UIHtmze91Lo1f5g7GizI48CpfpbAIlp&X-Amz-Signature=ce32eb1e70630df65c4ef390ec3dc3fede5ce8d69b010d700698db83fdd6ec57&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYIUAJEN%2F20260829%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260829T044528Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDDhx7Q5hR695C8WKMuOVZ6bGvnGzx8%2FhsJslEZC4guHAIgeqPcps5aQaj%2FUPdYamHxMHxWBEMGW%2FlwfjytQhUtFZQq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDLDi4lQNjhn9jSix3ircAy3CTjjND0zP5qXKwoJnp86zd8sfGQ2mI2rinaDGAywzftstR3V%2Bv1XcWjsqkwV9HizQC1km47silGCDRMdf%2Ff%2BRdtntJ9zkXeJQOjQZgfWQLtp2ZIpDsGrLaHhjWyoomKF1PeX4xeYpyLtQBf5m16DRuogQ84WcohoSKC5kscjvQc1uoyn7iIpluAeHW4BHFMI7ieJHTQecXyg2pUPDH9JS5hlX6ee8s%2FmoI79Z3OxRgtpd9ygfJCQFvbZTIg4fa%2B%2B9EubTbtWYMQBn08FAHzP%2B5vBi4BlqM9kgys%2FH9kTd8VLzxBdz8dNXrydsw2gphT%2F6rK9gx%2BlUWMn%2FPJQO2r7HP%2FqR7o76wy3wfky2LdU3Jltt07S1jo0xwvDZFxUHcmFjpm%2BsyEvTakJPS6VIqkKH5wNDJoyMWQ%2BMiWxm2gqs51dkFW1cv%2F6l2%2B62wsTLuwP%2B1ZMVgLikkIwtMblaaMhAXRN2kWs0TcISl%2BpRL%2BQLSo5zbknm1r3j5cfwWt%2B%2FlCMTCFY4mgrkXDvxN2YnzBodGlp92Y61bNc6Y%2FcdU00q2M%2FC4kinYw5CRn1rTxMamJNqreWLi5Qt3i91Lc5LT36Wsw6hukoBXFIfRLHZl7r%2F2i5RwHvFlWBSjEntMMK6ydQGOqUBfzVHKcHm5fZ6zU9oXB1fYO0fK0BkVjAzMBzyWu9B9eysj7kPNp4g59Hg8vRGJRnXKz5M%2F6Soa6LFWxsT1aN2aQjDNAaEcPOMYJXrGwNeICj8J9PJEByA19rzzbh1sq%2BwR7Q11BfJlMgThWCbNTcgis%2Br8%2BtLZSlmPTHiAXEE%2Bq3yrMuU82a%2FTi8%2F5h967UIHtmze91Lo1f5g7GizI48CpfpbAIlp&X-Amz-Signature=54cf33ab6bfb56d69ae991305bdb678953bb98cb61df000f2873230429273957&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYIUAJEN%2F20260829%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260829T044528Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDDhx7Q5hR695C8WKMuOVZ6bGvnGzx8%2FhsJslEZC4guHAIgeqPcps5aQaj%2FUPdYamHxMHxWBEMGW%2FlwfjytQhUtFZQq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDLDi4lQNjhn9jSix3ircAy3CTjjND0zP5qXKwoJnp86zd8sfGQ2mI2rinaDGAywzftstR3V%2Bv1XcWjsqkwV9HizQC1km47silGCDRMdf%2Ff%2BRdtntJ9zkXeJQOjQZgfWQLtp2ZIpDsGrLaHhjWyoomKF1PeX4xeYpyLtQBf5m16DRuogQ84WcohoSKC5kscjvQc1uoyn7iIpluAeHW4BHFMI7ieJHTQecXyg2pUPDH9JS5hlX6ee8s%2FmoI79Z3OxRgtpd9ygfJCQFvbZTIg4fa%2B%2B9EubTbtWYMQBn08FAHzP%2B5vBi4BlqM9kgys%2FH9kTd8VLzxBdz8dNXrydsw2gphT%2F6rK9gx%2BlUWMn%2FPJQO2r7HP%2FqR7o76wy3wfky2LdU3Jltt07S1jo0xwvDZFxUHcmFjpm%2BsyEvTakJPS6VIqkKH5wNDJoyMWQ%2BMiWxm2gqs51dkFW1cv%2F6l2%2B62wsTLuwP%2B1ZMVgLikkIwtMblaaMhAXRN2kWs0TcISl%2BpRL%2BQLSo5zbknm1r3j5cfwWt%2B%2FlCMTCFY4mgrkXDvxN2YnzBodGlp92Y61bNc6Y%2FcdU00q2M%2FC4kinYw5CRn1rTxMamJNqreWLi5Qt3i91Lc5LT36Wsw6hukoBXFIfRLHZl7r%2F2i5RwHvFlWBSjEntMMK6ydQGOqUBfzVHKcHm5fZ6zU9oXB1fYO0fK0BkVjAzMBzyWu9B9eysj7kPNp4g59Hg8vRGJRnXKz5M%2F6Soa6LFWxsT1aN2aQjDNAaEcPOMYJXrGwNeICj8J9PJEByA19rzzbh1sq%2BwR7Q11BfJlMgThWCbNTcgis%2Br8%2BtLZSlmPTHiAXEE%2Bq3yrMuU82a%2FTi8%2F5h967UIHtmze91Lo1f5g7GizI48CpfpbAIlp&X-Amz-Signature=91f9983b4192bbd9aca91712817746a3f00d052bd05efee2440217044c9f3c58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYIUAJEN%2F20260829%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260829T044528Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDDhx7Q5hR695C8WKMuOVZ6bGvnGzx8%2FhsJslEZC4guHAIgeqPcps5aQaj%2FUPdYamHxMHxWBEMGW%2FlwfjytQhUtFZQq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDLDi4lQNjhn9jSix3ircAy3CTjjND0zP5qXKwoJnp86zd8sfGQ2mI2rinaDGAywzftstR3V%2Bv1XcWjsqkwV9HizQC1km47silGCDRMdf%2Ff%2BRdtntJ9zkXeJQOjQZgfWQLtp2ZIpDsGrLaHhjWyoomKF1PeX4xeYpyLtQBf5m16DRuogQ84WcohoSKC5kscjvQc1uoyn7iIpluAeHW4BHFMI7ieJHTQecXyg2pUPDH9JS5hlX6ee8s%2FmoI79Z3OxRgtpd9ygfJCQFvbZTIg4fa%2B%2B9EubTbtWYMQBn08FAHzP%2B5vBi4BlqM9kgys%2FH9kTd8VLzxBdz8dNXrydsw2gphT%2F6rK9gx%2BlUWMn%2FPJQO2r7HP%2FqR7o76wy3wfky2LdU3Jltt07S1jo0xwvDZFxUHcmFjpm%2BsyEvTakJPS6VIqkKH5wNDJoyMWQ%2BMiWxm2gqs51dkFW1cv%2F6l2%2B62wsTLuwP%2B1ZMVgLikkIwtMblaaMhAXRN2kWs0TcISl%2BpRL%2BQLSo5zbknm1r3j5cfwWt%2B%2FlCMTCFY4mgrkXDvxN2YnzBodGlp92Y61bNc6Y%2FcdU00q2M%2FC4kinYw5CRn1rTxMamJNqreWLi5Qt3i91Lc5LT36Wsw6hukoBXFIfRLHZl7r%2F2i5RwHvFlWBSjEntMMK6ydQGOqUBfzVHKcHm5fZ6zU9oXB1fYO0fK0BkVjAzMBzyWu9B9eysj7kPNp4g59Hg8vRGJRnXKz5M%2F6Soa6LFWxsT1aN2aQjDNAaEcPOMYJXrGwNeICj8J9PJEByA19rzzbh1sq%2BwR7Q11BfJlMgThWCbNTcgis%2Br8%2BtLZSlmPTHiAXEE%2Bq3yrMuU82a%2FTi8%2F5h967UIHtmze91Lo1f5g7GizI48CpfpbAIlp&X-Amz-Signature=dc5ab9581ce30d94f1324d85a720c96125e081e9fa6ff3b7e2eda19e5c00c30c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYIUAJEN%2F20260829%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260829T044528Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDDhx7Q5hR695C8WKMuOVZ6bGvnGzx8%2FhsJslEZC4guHAIgeqPcps5aQaj%2FUPdYamHxMHxWBEMGW%2FlwfjytQhUtFZQq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDLDi4lQNjhn9jSix3ircAy3CTjjND0zP5qXKwoJnp86zd8sfGQ2mI2rinaDGAywzftstR3V%2Bv1XcWjsqkwV9HizQC1km47silGCDRMdf%2Ff%2BRdtntJ9zkXeJQOjQZgfWQLtp2ZIpDsGrLaHhjWyoomKF1PeX4xeYpyLtQBf5m16DRuogQ84WcohoSKC5kscjvQc1uoyn7iIpluAeHW4BHFMI7ieJHTQecXyg2pUPDH9JS5hlX6ee8s%2FmoI79Z3OxRgtpd9ygfJCQFvbZTIg4fa%2B%2B9EubTbtWYMQBn08FAHzP%2B5vBi4BlqM9kgys%2FH9kTd8VLzxBdz8dNXrydsw2gphT%2F6rK9gx%2BlUWMn%2FPJQO2r7HP%2FqR7o76wy3wfky2LdU3Jltt07S1jo0xwvDZFxUHcmFjpm%2BsyEvTakJPS6VIqkKH5wNDJoyMWQ%2BMiWxm2gqs51dkFW1cv%2F6l2%2B62wsTLuwP%2B1ZMVgLikkIwtMblaaMhAXRN2kWs0TcISl%2BpRL%2BQLSo5zbknm1r3j5cfwWt%2B%2FlCMTCFY4mgrkXDvxN2YnzBodGlp92Y61bNc6Y%2FcdU00q2M%2FC4kinYw5CRn1rTxMamJNqreWLi5Qt3i91Lc5LT36Wsw6hukoBXFIfRLHZl7r%2F2i5RwHvFlWBSjEntMMK6ydQGOqUBfzVHKcHm5fZ6zU9oXB1fYO0fK0BkVjAzMBzyWu9B9eysj7kPNp4g59Hg8vRGJRnXKz5M%2F6Soa6LFWxsT1aN2aQjDNAaEcPOMYJXrGwNeICj8J9PJEByA19rzzbh1sq%2BwR7Q11BfJlMgThWCbNTcgis%2Br8%2BtLZSlmPTHiAXEE%2Bq3yrMuU82a%2FTi8%2F5h967UIHtmze91Lo1f5g7GizI48CpfpbAIlp&X-Amz-Signature=4155815f8380ca9778fe4ad15d257db7787e4896e09808101282626cefabd72f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYIUAJEN%2F20260829%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260829T044528Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDDhx7Q5hR695C8WKMuOVZ6bGvnGzx8%2FhsJslEZC4guHAIgeqPcps5aQaj%2FUPdYamHxMHxWBEMGW%2FlwfjytQhUtFZQq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDLDi4lQNjhn9jSix3ircAy3CTjjND0zP5qXKwoJnp86zd8sfGQ2mI2rinaDGAywzftstR3V%2Bv1XcWjsqkwV9HizQC1km47silGCDRMdf%2Ff%2BRdtntJ9zkXeJQOjQZgfWQLtp2ZIpDsGrLaHhjWyoomKF1PeX4xeYpyLtQBf5m16DRuogQ84WcohoSKC5kscjvQc1uoyn7iIpluAeHW4BHFMI7ieJHTQecXyg2pUPDH9JS5hlX6ee8s%2FmoI79Z3OxRgtpd9ygfJCQFvbZTIg4fa%2B%2B9EubTbtWYMQBn08FAHzP%2B5vBi4BlqM9kgys%2FH9kTd8VLzxBdz8dNXrydsw2gphT%2F6rK9gx%2BlUWMn%2FPJQO2r7HP%2FqR7o76wy3wfky2LdU3Jltt07S1jo0xwvDZFxUHcmFjpm%2BsyEvTakJPS6VIqkKH5wNDJoyMWQ%2BMiWxm2gqs51dkFW1cv%2F6l2%2B62wsTLuwP%2B1ZMVgLikkIwtMblaaMhAXRN2kWs0TcISl%2BpRL%2BQLSo5zbknm1r3j5cfwWt%2B%2FlCMTCFY4mgrkXDvxN2YnzBodGlp92Y61bNc6Y%2FcdU00q2M%2FC4kinYw5CRn1rTxMamJNqreWLi5Qt3i91Lc5LT36Wsw6hukoBXFIfRLHZl7r%2F2i5RwHvFlWBSjEntMMK6ydQGOqUBfzVHKcHm5fZ6zU9oXB1fYO0fK0BkVjAzMBzyWu9B9eysj7kPNp4g59Hg8vRGJRnXKz5M%2F6Soa6LFWxsT1aN2aQjDNAaEcPOMYJXrGwNeICj8J9PJEByA19rzzbh1sq%2BwR7Q11BfJlMgThWCbNTcgis%2Br8%2BtLZSlmPTHiAXEE%2Bq3yrMuU82a%2FTi8%2F5h967UIHtmze91Lo1f5g7GizI48CpfpbAIlp&X-Amz-Signature=2f8d610f7361decd6573795263737988050a70f6db5ea16571d82752db72920d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYIUAJEN%2F20260829%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260829T044528Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDDhx7Q5hR695C8WKMuOVZ6bGvnGzx8%2FhsJslEZC4guHAIgeqPcps5aQaj%2FUPdYamHxMHxWBEMGW%2FlwfjytQhUtFZQq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDLDi4lQNjhn9jSix3ircAy3CTjjND0zP5qXKwoJnp86zd8sfGQ2mI2rinaDGAywzftstR3V%2Bv1XcWjsqkwV9HizQC1km47silGCDRMdf%2Ff%2BRdtntJ9zkXeJQOjQZgfWQLtp2ZIpDsGrLaHhjWyoomKF1PeX4xeYpyLtQBf5m16DRuogQ84WcohoSKC5kscjvQc1uoyn7iIpluAeHW4BHFMI7ieJHTQecXyg2pUPDH9JS5hlX6ee8s%2FmoI79Z3OxRgtpd9ygfJCQFvbZTIg4fa%2B%2B9EubTbtWYMQBn08FAHzP%2B5vBi4BlqM9kgys%2FH9kTd8VLzxBdz8dNXrydsw2gphT%2F6rK9gx%2BlUWMn%2FPJQO2r7HP%2FqR7o76wy3wfky2LdU3Jltt07S1jo0xwvDZFxUHcmFjpm%2BsyEvTakJPS6VIqkKH5wNDJoyMWQ%2BMiWxm2gqs51dkFW1cv%2F6l2%2B62wsTLuwP%2B1ZMVgLikkIwtMblaaMhAXRN2kWs0TcISl%2BpRL%2BQLSo5zbknm1r3j5cfwWt%2B%2FlCMTCFY4mgrkXDvxN2YnzBodGlp92Y61bNc6Y%2FcdU00q2M%2FC4kinYw5CRn1rTxMamJNqreWLi5Qt3i91Lc5LT36Wsw6hukoBXFIfRLHZl7r%2F2i5RwHvFlWBSjEntMMK6ydQGOqUBfzVHKcHm5fZ6zU9oXB1fYO0fK0BkVjAzMBzyWu9B9eysj7kPNp4g59Hg8vRGJRnXKz5M%2F6Soa6LFWxsT1aN2aQjDNAaEcPOMYJXrGwNeICj8J9PJEByA19rzzbh1sq%2BwR7Q11BfJlMgThWCbNTcgis%2Br8%2BtLZSlmPTHiAXEE%2Bq3yrMuU82a%2FTi8%2F5h967UIHtmze91Lo1f5g7GizI48CpfpbAIlp&X-Amz-Signature=5595c0bc313fea93ade8ea287a213bcd625fc45f30d3597bb982c9d19919d691&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYIUAJEN%2F20260829%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260829T044528Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDDhx7Q5hR695C8WKMuOVZ6bGvnGzx8%2FhsJslEZC4guHAIgeqPcps5aQaj%2FUPdYamHxMHxWBEMGW%2FlwfjytQhUtFZQq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDLDi4lQNjhn9jSix3ircAy3CTjjND0zP5qXKwoJnp86zd8sfGQ2mI2rinaDGAywzftstR3V%2Bv1XcWjsqkwV9HizQC1km47silGCDRMdf%2Ff%2BRdtntJ9zkXeJQOjQZgfWQLtp2ZIpDsGrLaHhjWyoomKF1PeX4xeYpyLtQBf5m16DRuogQ84WcohoSKC5kscjvQc1uoyn7iIpluAeHW4BHFMI7ieJHTQecXyg2pUPDH9JS5hlX6ee8s%2FmoI79Z3OxRgtpd9ygfJCQFvbZTIg4fa%2B%2B9EubTbtWYMQBn08FAHzP%2B5vBi4BlqM9kgys%2FH9kTd8VLzxBdz8dNXrydsw2gphT%2F6rK9gx%2BlUWMn%2FPJQO2r7HP%2FqR7o76wy3wfky2LdU3Jltt07S1jo0xwvDZFxUHcmFjpm%2BsyEvTakJPS6VIqkKH5wNDJoyMWQ%2BMiWxm2gqs51dkFW1cv%2F6l2%2B62wsTLuwP%2B1ZMVgLikkIwtMblaaMhAXRN2kWs0TcISl%2BpRL%2BQLSo5zbknm1r3j5cfwWt%2B%2FlCMTCFY4mgrkXDvxN2YnzBodGlp92Y61bNc6Y%2FcdU00q2M%2FC4kinYw5CRn1rTxMamJNqreWLi5Qt3i91Lc5LT36Wsw6hukoBXFIfRLHZl7r%2F2i5RwHvFlWBSjEntMMK6ydQGOqUBfzVHKcHm5fZ6zU9oXB1fYO0fK0BkVjAzMBzyWu9B9eysj7kPNp4g59Hg8vRGJRnXKz5M%2F6Soa6LFWxsT1aN2aQjDNAaEcPOMYJXrGwNeICj8J9PJEByA19rzzbh1sq%2BwR7Q11BfJlMgThWCbNTcgis%2Br8%2BtLZSlmPTHiAXEE%2Bq3yrMuU82a%2FTi8%2F5h967UIHtmze91Lo1f5g7GizI48CpfpbAIlp&X-Amz-Signature=58d0a814ea17394d846250858c9e4804e570f68a0bcd3d6e64c327d7504f9bcb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
