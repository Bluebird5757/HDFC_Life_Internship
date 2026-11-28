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
fetched_at: '2026-11-28T05:29:17.955Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666FB4XFH%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052912Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIQDFBvdtAXO8dVXLDkSQJhK4207J%2BpyPkS60AWLVImqy2wIgFPKTrdtVExywwBnRTxWca8sDt7nZ7o5shGim5Q0Z4ewqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAM76ECTHQB%2Bm%2Fy2aSrcA%2FHzVFSTe0muFy3eEZPkeYbFoBUyAPG4jfGU1IoPEUizmaX%2FsLpQWfb3RQi7feavJYaxcPV%2BVaozpXJ1%2B2ruiZ8q50q1r8PB%2Fe%2FqJf5%2BuzL7UA7g2YtZH1C6AZ7ix9lrIGMepiFftKHL8DWXz3iVb8wXork2hTklbhl4moYs8rbdq%2BvCpXznKbhFAv7PFr0zqn6JpLadAfJ0F5QTTyANPLrJHoMDdTUdVyE7n3Ulq%2B1%2B0o2vwrkeS6x8mnMMsCS0ituEkTH9iabR%2BH0GS4xH767uZn8zcYnvi9IAXHDomn55HhgtKgDCqWAASWwPAi%2FpCqBZd%2Fc3yR6zzWn9flJtosvQxj1VY6JyCf%2Fla2g%2Fs46%2Bbmvax1nU4tmSGq%2F%2Bz8HctMEWwkwuiml7vjlFJrEWbERtmMsG0YXU1OnZROs7LfzQ6FNYaCF27crVkfULmDZum4%2FDFaPaYEgSfCcnDv4sD8v6kvX8FivO9Dd7iY2s0vxO7Kwfuw7c519j%2F7TR9%2FGZLeTGJ8soNxs6x%2BTFUF5JLGdY7L1apPSgtNAuGxb9GOl%2BJ%2FHGSim6fG8sV%2FavupbuyH6sAR1ABcOkyOBbDfUqqedreEVED31%2B3DvHHz%2FbGORaVnZUn6wyHEVqU2BxMLLMqNgGOqUBf0tbjrdFlCwV7CoatpL20ev7YQLQE1sNGWiX7Agkt7rGaYdTr64aXC1D3mkqWXb1xYqHEzbxCPU41setidybBKnFOO%2FsoJzGn%2FWoAu4lK54HdW1OY1T%2B%2FAXb2dQuAPBp%2F7bs1EraR6m02LmpE%2BUabU9Q09AJKdkod%2FNhr4h3FgCNGgs2R3o0maMXEm2aVewvYhSeQ9oYUAgJxhopR9LkStgv9nWq&X-Amz-Signature=1ed16c65a47ed0c90427bae50ed4c1f76549c2e6f19619c318185a7ef5e5a231&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666FB4XFH%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052912Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIQDFBvdtAXO8dVXLDkSQJhK4207J%2BpyPkS60AWLVImqy2wIgFPKTrdtVExywwBnRTxWca8sDt7nZ7o5shGim5Q0Z4ewqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAM76ECTHQB%2Bm%2Fy2aSrcA%2FHzVFSTe0muFy3eEZPkeYbFoBUyAPG4jfGU1IoPEUizmaX%2FsLpQWfb3RQi7feavJYaxcPV%2BVaozpXJ1%2B2ruiZ8q50q1r8PB%2Fe%2FqJf5%2BuzL7UA7g2YtZH1C6AZ7ix9lrIGMepiFftKHL8DWXz3iVb8wXork2hTklbhl4moYs8rbdq%2BvCpXznKbhFAv7PFr0zqn6JpLadAfJ0F5QTTyANPLrJHoMDdTUdVyE7n3Ulq%2B1%2B0o2vwrkeS6x8mnMMsCS0ituEkTH9iabR%2BH0GS4xH767uZn8zcYnvi9IAXHDomn55HhgtKgDCqWAASWwPAi%2FpCqBZd%2Fc3yR6zzWn9flJtosvQxj1VY6JyCf%2Fla2g%2Fs46%2Bbmvax1nU4tmSGq%2F%2Bz8HctMEWwkwuiml7vjlFJrEWbERtmMsG0YXU1OnZROs7LfzQ6FNYaCF27crVkfULmDZum4%2FDFaPaYEgSfCcnDv4sD8v6kvX8FivO9Dd7iY2s0vxO7Kwfuw7c519j%2F7TR9%2FGZLeTGJ8soNxs6x%2BTFUF5JLGdY7L1apPSgtNAuGxb9GOl%2BJ%2FHGSim6fG8sV%2FavupbuyH6sAR1ABcOkyOBbDfUqqedreEVED31%2B3DvHHz%2FbGORaVnZUn6wyHEVqU2BxMLLMqNgGOqUBf0tbjrdFlCwV7CoatpL20ev7YQLQE1sNGWiX7Agkt7rGaYdTr64aXC1D3mkqWXb1xYqHEzbxCPU41setidybBKnFOO%2FsoJzGn%2FWoAu4lK54HdW1OY1T%2B%2FAXb2dQuAPBp%2F7bs1EraR6m02LmpE%2BUabU9Q09AJKdkod%2FNhr4h3FgCNGgs2R3o0maMXEm2aVewvYhSeQ9oYUAgJxhopR9LkStgv9nWq&X-Amz-Signature=3e2562b7b9bee6a7ca2b584e769201bfe7e555925083f5f57cfca975b63af043&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666FB4XFH%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052912Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIQDFBvdtAXO8dVXLDkSQJhK4207J%2BpyPkS60AWLVImqy2wIgFPKTrdtVExywwBnRTxWca8sDt7nZ7o5shGim5Q0Z4ewqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAM76ECTHQB%2Bm%2Fy2aSrcA%2FHzVFSTe0muFy3eEZPkeYbFoBUyAPG4jfGU1IoPEUizmaX%2FsLpQWfb3RQi7feavJYaxcPV%2BVaozpXJ1%2B2ruiZ8q50q1r8PB%2Fe%2FqJf5%2BuzL7UA7g2YtZH1C6AZ7ix9lrIGMepiFftKHL8DWXz3iVb8wXork2hTklbhl4moYs8rbdq%2BvCpXznKbhFAv7PFr0zqn6JpLadAfJ0F5QTTyANPLrJHoMDdTUdVyE7n3Ulq%2B1%2B0o2vwrkeS6x8mnMMsCS0ituEkTH9iabR%2BH0GS4xH767uZn8zcYnvi9IAXHDomn55HhgtKgDCqWAASWwPAi%2FpCqBZd%2Fc3yR6zzWn9flJtosvQxj1VY6JyCf%2Fla2g%2Fs46%2Bbmvax1nU4tmSGq%2F%2Bz8HctMEWwkwuiml7vjlFJrEWbERtmMsG0YXU1OnZROs7LfzQ6FNYaCF27crVkfULmDZum4%2FDFaPaYEgSfCcnDv4sD8v6kvX8FivO9Dd7iY2s0vxO7Kwfuw7c519j%2F7TR9%2FGZLeTGJ8soNxs6x%2BTFUF5JLGdY7L1apPSgtNAuGxb9GOl%2BJ%2FHGSim6fG8sV%2FavupbuyH6sAR1ABcOkyOBbDfUqqedreEVED31%2B3DvHHz%2FbGORaVnZUn6wyHEVqU2BxMLLMqNgGOqUBf0tbjrdFlCwV7CoatpL20ev7YQLQE1sNGWiX7Agkt7rGaYdTr64aXC1D3mkqWXb1xYqHEzbxCPU41setidybBKnFOO%2FsoJzGn%2FWoAu4lK54HdW1OY1T%2B%2FAXb2dQuAPBp%2F7bs1EraR6m02LmpE%2BUabU9Q09AJKdkod%2FNhr4h3FgCNGgs2R3o0maMXEm2aVewvYhSeQ9oYUAgJxhopR9LkStgv9nWq&X-Amz-Signature=ae700a530a6f061dbb270bedd518e3a6447a87835ab54e66af8d90d7d4a62a85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666FB4XFH%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052912Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIQDFBvdtAXO8dVXLDkSQJhK4207J%2BpyPkS60AWLVImqy2wIgFPKTrdtVExywwBnRTxWca8sDt7nZ7o5shGim5Q0Z4ewqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAM76ECTHQB%2Bm%2Fy2aSrcA%2FHzVFSTe0muFy3eEZPkeYbFoBUyAPG4jfGU1IoPEUizmaX%2FsLpQWfb3RQi7feavJYaxcPV%2BVaozpXJ1%2B2ruiZ8q50q1r8PB%2Fe%2FqJf5%2BuzL7UA7g2YtZH1C6AZ7ix9lrIGMepiFftKHL8DWXz3iVb8wXork2hTklbhl4moYs8rbdq%2BvCpXznKbhFAv7PFr0zqn6JpLadAfJ0F5QTTyANPLrJHoMDdTUdVyE7n3Ulq%2B1%2B0o2vwrkeS6x8mnMMsCS0ituEkTH9iabR%2BH0GS4xH767uZn8zcYnvi9IAXHDomn55HhgtKgDCqWAASWwPAi%2FpCqBZd%2Fc3yR6zzWn9flJtosvQxj1VY6JyCf%2Fla2g%2Fs46%2Bbmvax1nU4tmSGq%2F%2Bz8HctMEWwkwuiml7vjlFJrEWbERtmMsG0YXU1OnZROs7LfzQ6FNYaCF27crVkfULmDZum4%2FDFaPaYEgSfCcnDv4sD8v6kvX8FivO9Dd7iY2s0vxO7Kwfuw7c519j%2F7TR9%2FGZLeTGJ8soNxs6x%2BTFUF5JLGdY7L1apPSgtNAuGxb9GOl%2BJ%2FHGSim6fG8sV%2FavupbuyH6sAR1ABcOkyOBbDfUqqedreEVED31%2B3DvHHz%2FbGORaVnZUn6wyHEVqU2BxMLLMqNgGOqUBf0tbjrdFlCwV7CoatpL20ev7YQLQE1sNGWiX7Agkt7rGaYdTr64aXC1D3mkqWXb1xYqHEzbxCPU41setidybBKnFOO%2FsoJzGn%2FWoAu4lK54HdW1OY1T%2B%2FAXb2dQuAPBp%2F7bs1EraR6m02LmpE%2BUabU9Q09AJKdkod%2FNhr4h3FgCNGgs2R3o0maMXEm2aVewvYhSeQ9oYUAgJxhopR9LkStgv9nWq&X-Amz-Signature=2c3d61cda494ec7803ce952b996ec1c98a2fe02d31b10fd147a5e9e07e57f9d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666FB4XFH%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052912Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIQDFBvdtAXO8dVXLDkSQJhK4207J%2BpyPkS60AWLVImqy2wIgFPKTrdtVExywwBnRTxWca8sDt7nZ7o5shGim5Q0Z4ewqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAM76ECTHQB%2Bm%2Fy2aSrcA%2FHzVFSTe0muFy3eEZPkeYbFoBUyAPG4jfGU1IoPEUizmaX%2FsLpQWfb3RQi7feavJYaxcPV%2BVaozpXJ1%2B2ruiZ8q50q1r8PB%2Fe%2FqJf5%2BuzL7UA7g2YtZH1C6AZ7ix9lrIGMepiFftKHL8DWXz3iVb8wXork2hTklbhl4moYs8rbdq%2BvCpXznKbhFAv7PFr0zqn6JpLadAfJ0F5QTTyANPLrJHoMDdTUdVyE7n3Ulq%2B1%2B0o2vwrkeS6x8mnMMsCS0ituEkTH9iabR%2BH0GS4xH767uZn8zcYnvi9IAXHDomn55HhgtKgDCqWAASWwPAi%2FpCqBZd%2Fc3yR6zzWn9flJtosvQxj1VY6JyCf%2Fla2g%2Fs46%2Bbmvax1nU4tmSGq%2F%2Bz8HctMEWwkwuiml7vjlFJrEWbERtmMsG0YXU1OnZROs7LfzQ6FNYaCF27crVkfULmDZum4%2FDFaPaYEgSfCcnDv4sD8v6kvX8FivO9Dd7iY2s0vxO7Kwfuw7c519j%2F7TR9%2FGZLeTGJ8soNxs6x%2BTFUF5JLGdY7L1apPSgtNAuGxb9GOl%2BJ%2FHGSim6fG8sV%2FavupbuyH6sAR1ABcOkyOBbDfUqqedreEVED31%2B3DvHHz%2FbGORaVnZUn6wyHEVqU2BxMLLMqNgGOqUBf0tbjrdFlCwV7CoatpL20ev7YQLQE1sNGWiX7Agkt7rGaYdTr64aXC1D3mkqWXb1xYqHEzbxCPU41setidybBKnFOO%2FsoJzGn%2FWoAu4lK54HdW1OY1T%2B%2FAXb2dQuAPBp%2F7bs1EraR6m02LmpE%2BUabU9Q09AJKdkod%2FNhr4h3FgCNGgs2R3o0maMXEm2aVewvYhSeQ9oYUAgJxhopR9LkStgv9nWq&X-Amz-Signature=66eef1e71506c4042ac5f9fd054cf3ccdf1dc9ad7abf9b62a6a1ebda5ae13838&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666FB4XFH%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052912Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIQDFBvdtAXO8dVXLDkSQJhK4207J%2BpyPkS60AWLVImqy2wIgFPKTrdtVExywwBnRTxWca8sDt7nZ7o5shGim5Q0Z4ewqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAM76ECTHQB%2Bm%2Fy2aSrcA%2FHzVFSTe0muFy3eEZPkeYbFoBUyAPG4jfGU1IoPEUizmaX%2FsLpQWfb3RQi7feavJYaxcPV%2BVaozpXJ1%2B2ruiZ8q50q1r8PB%2Fe%2FqJf5%2BuzL7UA7g2YtZH1C6AZ7ix9lrIGMepiFftKHL8DWXz3iVb8wXork2hTklbhl4moYs8rbdq%2BvCpXznKbhFAv7PFr0zqn6JpLadAfJ0F5QTTyANPLrJHoMDdTUdVyE7n3Ulq%2B1%2B0o2vwrkeS6x8mnMMsCS0ituEkTH9iabR%2BH0GS4xH767uZn8zcYnvi9IAXHDomn55HhgtKgDCqWAASWwPAi%2FpCqBZd%2Fc3yR6zzWn9flJtosvQxj1VY6JyCf%2Fla2g%2Fs46%2Bbmvax1nU4tmSGq%2F%2Bz8HctMEWwkwuiml7vjlFJrEWbERtmMsG0YXU1OnZROs7LfzQ6FNYaCF27crVkfULmDZum4%2FDFaPaYEgSfCcnDv4sD8v6kvX8FivO9Dd7iY2s0vxO7Kwfuw7c519j%2F7TR9%2FGZLeTGJ8soNxs6x%2BTFUF5JLGdY7L1apPSgtNAuGxb9GOl%2BJ%2FHGSim6fG8sV%2FavupbuyH6sAR1ABcOkyOBbDfUqqedreEVED31%2B3DvHHz%2FbGORaVnZUn6wyHEVqU2BxMLLMqNgGOqUBf0tbjrdFlCwV7CoatpL20ev7YQLQE1sNGWiX7Agkt7rGaYdTr64aXC1D3mkqWXb1xYqHEzbxCPU41setidybBKnFOO%2FsoJzGn%2FWoAu4lK54HdW1OY1T%2B%2FAXb2dQuAPBp%2F7bs1EraR6m02LmpE%2BUabU9Q09AJKdkod%2FNhr4h3FgCNGgs2R3o0maMXEm2aVewvYhSeQ9oYUAgJxhopR9LkStgv9nWq&X-Amz-Signature=cb7b0d37075a0df3ddb747887e7e052631f27c42c2752f11893eaab351188ca1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666FB4XFH%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052912Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIQDFBvdtAXO8dVXLDkSQJhK4207J%2BpyPkS60AWLVImqy2wIgFPKTrdtVExywwBnRTxWca8sDt7nZ7o5shGim5Q0Z4ewqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAM76ECTHQB%2Bm%2Fy2aSrcA%2FHzVFSTe0muFy3eEZPkeYbFoBUyAPG4jfGU1IoPEUizmaX%2FsLpQWfb3RQi7feavJYaxcPV%2BVaozpXJ1%2B2ruiZ8q50q1r8PB%2Fe%2FqJf5%2BuzL7UA7g2YtZH1C6AZ7ix9lrIGMepiFftKHL8DWXz3iVb8wXork2hTklbhl4moYs8rbdq%2BvCpXznKbhFAv7PFr0zqn6JpLadAfJ0F5QTTyANPLrJHoMDdTUdVyE7n3Ulq%2B1%2B0o2vwrkeS6x8mnMMsCS0ituEkTH9iabR%2BH0GS4xH767uZn8zcYnvi9IAXHDomn55HhgtKgDCqWAASWwPAi%2FpCqBZd%2Fc3yR6zzWn9flJtosvQxj1VY6JyCf%2Fla2g%2Fs46%2Bbmvax1nU4tmSGq%2F%2Bz8HctMEWwkwuiml7vjlFJrEWbERtmMsG0YXU1OnZROs7LfzQ6FNYaCF27crVkfULmDZum4%2FDFaPaYEgSfCcnDv4sD8v6kvX8FivO9Dd7iY2s0vxO7Kwfuw7c519j%2F7TR9%2FGZLeTGJ8soNxs6x%2BTFUF5JLGdY7L1apPSgtNAuGxb9GOl%2BJ%2FHGSim6fG8sV%2FavupbuyH6sAR1ABcOkyOBbDfUqqedreEVED31%2B3DvHHz%2FbGORaVnZUn6wyHEVqU2BxMLLMqNgGOqUBf0tbjrdFlCwV7CoatpL20ev7YQLQE1sNGWiX7Agkt7rGaYdTr64aXC1D3mkqWXb1xYqHEzbxCPU41setidybBKnFOO%2FsoJzGn%2FWoAu4lK54HdW1OY1T%2B%2FAXb2dQuAPBp%2F7bs1EraR6m02LmpE%2BUabU9Q09AJKdkod%2FNhr4h3FgCNGgs2R3o0maMXEm2aVewvYhSeQ9oYUAgJxhopR9LkStgv9nWq&X-Amz-Signature=3b973c0d8e1604b5f1ca77666a2680cdeace349cf805c70b2af8746cf6759696&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666FB4XFH%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052912Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIQDFBvdtAXO8dVXLDkSQJhK4207J%2BpyPkS60AWLVImqy2wIgFPKTrdtVExywwBnRTxWca8sDt7nZ7o5shGim5Q0Z4ewqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAM76ECTHQB%2Bm%2Fy2aSrcA%2FHzVFSTe0muFy3eEZPkeYbFoBUyAPG4jfGU1IoPEUizmaX%2FsLpQWfb3RQi7feavJYaxcPV%2BVaozpXJ1%2B2ruiZ8q50q1r8PB%2Fe%2FqJf5%2BuzL7UA7g2YtZH1C6AZ7ix9lrIGMepiFftKHL8DWXz3iVb8wXork2hTklbhl4moYs8rbdq%2BvCpXznKbhFAv7PFr0zqn6JpLadAfJ0F5QTTyANPLrJHoMDdTUdVyE7n3Ulq%2B1%2B0o2vwrkeS6x8mnMMsCS0ituEkTH9iabR%2BH0GS4xH767uZn8zcYnvi9IAXHDomn55HhgtKgDCqWAASWwPAi%2FpCqBZd%2Fc3yR6zzWn9flJtosvQxj1VY6JyCf%2Fla2g%2Fs46%2Bbmvax1nU4tmSGq%2F%2Bz8HctMEWwkwuiml7vjlFJrEWbERtmMsG0YXU1OnZROs7LfzQ6FNYaCF27crVkfULmDZum4%2FDFaPaYEgSfCcnDv4sD8v6kvX8FivO9Dd7iY2s0vxO7Kwfuw7c519j%2F7TR9%2FGZLeTGJ8soNxs6x%2BTFUF5JLGdY7L1apPSgtNAuGxb9GOl%2BJ%2FHGSim6fG8sV%2FavupbuyH6sAR1ABcOkyOBbDfUqqedreEVED31%2B3DvHHz%2FbGORaVnZUn6wyHEVqU2BxMLLMqNgGOqUBf0tbjrdFlCwV7CoatpL20ev7YQLQE1sNGWiX7Agkt7rGaYdTr64aXC1D3mkqWXb1xYqHEzbxCPU41setidybBKnFOO%2FsoJzGn%2FWoAu4lK54HdW1OY1T%2B%2FAXb2dQuAPBp%2F7bs1EraR6m02LmpE%2BUabU9Q09AJKdkod%2FNhr4h3FgCNGgs2R3o0maMXEm2aVewvYhSeQ9oYUAgJxhopR9LkStgv9nWq&X-Amz-Signature=f0812d434e93bcace7fc37ec4681f33fbdf0d790db19c5d32c873cfed39619d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666FB4XFH%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052912Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIQDFBvdtAXO8dVXLDkSQJhK4207J%2BpyPkS60AWLVImqy2wIgFPKTrdtVExywwBnRTxWca8sDt7nZ7o5shGim5Q0Z4ewqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAM76ECTHQB%2Bm%2Fy2aSrcA%2FHzVFSTe0muFy3eEZPkeYbFoBUyAPG4jfGU1IoPEUizmaX%2FsLpQWfb3RQi7feavJYaxcPV%2BVaozpXJ1%2B2ruiZ8q50q1r8PB%2Fe%2FqJf5%2BuzL7UA7g2YtZH1C6AZ7ix9lrIGMepiFftKHL8DWXz3iVb8wXork2hTklbhl4moYs8rbdq%2BvCpXznKbhFAv7PFr0zqn6JpLadAfJ0F5QTTyANPLrJHoMDdTUdVyE7n3Ulq%2B1%2B0o2vwrkeS6x8mnMMsCS0ituEkTH9iabR%2BH0GS4xH767uZn8zcYnvi9IAXHDomn55HhgtKgDCqWAASWwPAi%2FpCqBZd%2Fc3yR6zzWn9flJtosvQxj1VY6JyCf%2Fla2g%2Fs46%2Bbmvax1nU4tmSGq%2F%2Bz8HctMEWwkwuiml7vjlFJrEWbERtmMsG0YXU1OnZROs7LfzQ6FNYaCF27crVkfULmDZum4%2FDFaPaYEgSfCcnDv4sD8v6kvX8FivO9Dd7iY2s0vxO7Kwfuw7c519j%2F7TR9%2FGZLeTGJ8soNxs6x%2BTFUF5JLGdY7L1apPSgtNAuGxb9GOl%2BJ%2FHGSim6fG8sV%2FavupbuyH6sAR1ABcOkyOBbDfUqqedreEVED31%2B3DvHHz%2FbGORaVnZUn6wyHEVqU2BxMLLMqNgGOqUBf0tbjrdFlCwV7CoatpL20ev7YQLQE1sNGWiX7Agkt7rGaYdTr64aXC1D3mkqWXb1xYqHEzbxCPU41setidybBKnFOO%2FsoJzGn%2FWoAu4lK54HdW1OY1T%2B%2FAXb2dQuAPBp%2F7bs1EraR6m02LmpE%2BUabU9Q09AJKdkod%2FNhr4h3FgCNGgs2R3o0maMXEm2aVewvYhSeQ9oYUAgJxhopR9LkStgv9nWq&X-Amz-Signature=a0351a9d3b37afb82731fca0d384f4120ad6361a0f4d73f1cbf62d9bb95c1ac1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
