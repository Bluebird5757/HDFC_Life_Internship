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
fetched_at: '2026-10-04T03:22:38.035Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TXPK6VI%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032233Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDtfyjLZJT0UpDf1oDIeJbXKCW%2BkQ5wLaZY5niJmXOQrAIgPcHjC%2FtHpzcWd09efvNEk0t5fLYYs66pzE0aIEpS1zoqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNjq%2BrNNdy%2FiNBGarSrcA1np0xEccmlBqnGUzAKjEiiKvlRB%2BjIa4rlikZ9OgzMLIcz%2F4A2k%2F%2FX3bKSKTTJT%2BvjJ9DrauXP9M8tIoS7eIXxfeAq%2F63sUICl5RTNBgXU6puhLhmMGKibhPEvneYMQ5C0BFX8WxBwCWxY9CsoeE%2FX7LOq%2BjEsOWa5ZyAQeLzXxU8Q9QT2cB5IttdOJWszJiD4pIo8RKNn06OuqBWIg7IWGhjosYSbJzrtss9LnVUb987RotD7lNqbUX5WSNGZ6Az%2FpsXPUUwGlEzuE24z1Y5wJJ0uqhBzzTIT0a4%2BJjFGZIBlKdj5cDNflzHJBOpxwa%2Bo9u0NBxlg%2F%2FtEdhjVQWkUQ1ARJfZanq9Hx2GwB2O32E1XES5JkCAiW0G%2FKtGstfLuDJ5TcwXSLD60SYGw3JZCgKOBcNuY2cxe24flTEfRlvTS1PwNy4n67xLiGHW0J17MkxT4MR7kEuf9lq3tA5q86%2BvL84JeWMRew41o1VBI2iRgrixmReZ8qZFS%2Fvp0hyxsc5mba1VOOHvAaHocXuCtysOiG2KEMZByhPuAvDhZ5R%2F3KQrH9K0UbJHLax3fw56l82JMiQtcdEUGXV0g5HZuU8%2FP9Cdp2MrWtH%2F0ojQLSI66s%2FjMrUsdjbi8%2BMLOEh9YGOqUBA9kG%2FwOBjB92lKHfGgjGbsvKAuBBMVs%2FkGkGUaTMlPgigAxzzl%2FgJEu02IZAgyetYnJsjuAmXhH3%2BBPFirq%2FHMOW2LiyEqqzeNmQ7bxzUBj0pHymX8zK5p6LeXnQqXuYVthkkXAQndFQGq%2FxIvZOLURH%2FkgYHzVKgANkDcMq6jrj%2Bp2IFTG7Ap6Pa2YugCQECXEv300ZS2pZJqsLiAxuS7Wi9Qtb&X-Amz-Signature=08f85dd92910311b9ad3f0b4d6f4b706787c4b455445f97ad8ab9a8bb31d4d2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TXPK6VI%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032233Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDtfyjLZJT0UpDf1oDIeJbXKCW%2BkQ5wLaZY5niJmXOQrAIgPcHjC%2FtHpzcWd09efvNEk0t5fLYYs66pzE0aIEpS1zoqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNjq%2BrNNdy%2FiNBGarSrcA1np0xEccmlBqnGUzAKjEiiKvlRB%2BjIa4rlikZ9OgzMLIcz%2F4A2k%2F%2FX3bKSKTTJT%2BvjJ9DrauXP9M8tIoS7eIXxfeAq%2F63sUICl5RTNBgXU6puhLhmMGKibhPEvneYMQ5C0BFX8WxBwCWxY9CsoeE%2FX7LOq%2BjEsOWa5ZyAQeLzXxU8Q9QT2cB5IttdOJWszJiD4pIo8RKNn06OuqBWIg7IWGhjosYSbJzrtss9LnVUb987RotD7lNqbUX5WSNGZ6Az%2FpsXPUUwGlEzuE24z1Y5wJJ0uqhBzzTIT0a4%2BJjFGZIBlKdj5cDNflzHJBOpxwa%2Bo9u0NBxlg%2F%2FtEdhjVQWkUQ1ARJfZanq9Hx2GwB2O32E1XES5JkCAiW0G%2FKtGstfLuDJ5TcwXSLD60SYGw3JZCgKOBcNuY2cxe24flTEfRlvTS1PwNy4n67xLiGHW0J17MkxT4MR7kEuf9lq3tA5q86%2BvL84JeWMRew41o1VBI2iRgrixmReZ8qZFS%2Fvp0hyxsc5mba1VOOHvAaHocXuCtysOiG2KEMZByhPuAvDhZ5R%2F3KQrH9K0UbJHLax3fw56l82JMiQtcdEUGXV0g5HZuU8%2FP9Cdp2MrWtH%2F0ojQLSI66s%2FjMrUsdjbi8%2BMLOEh9YGOqUBA9kG%2FwOBjB92lKHfGgjGbsvKAuBBMVs%2FkGkGUaTMlPgigAxzzl%2FgJEu02IZAgyetYnJsjuAmXhH3%2BBPFirq%2FHMOW2LiyEqqzeNmQ7bxzUBj0pHymX8zK5p6LeXnQqXuYVthkkXAQndFQGq%2FxIvZOLURH%2FkgYHzVKgANkDcMq6jrj%2Bp2IFTG7Ap6Pa2YugCQECXEv300ZS2pZJqsLiAxuS7Wi9Qtb&X-Amz-Signature=6e9dacf2bfa3619d05fd9ad514b8cf24e516cbb8555cfce32319a02002ecd963&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TXPK6VI%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032233Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDtfyjLZJT0UpDf1oDIeJbXKCW%2BkQ5wLaZY5niJmXOQrAIgPcHjC%2FtHpzcWd09efvNEk0t5fLYYs66pzE0aIEpS1zoqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNjq%2BrNNdy%2FiNBGarSrcA1np0xEccmlBqnGUzAKjEiiKvlRB%2BjIa4rlikZ9OgzMLIcz%2F4A2k%2F%2FX3bKSKTTJT%2BvjJ9DrauXP9M8tIoS7eIXxfeAq%2F63sUICl5RTNBgXU6puhLhmMGKibhPEvneYMQ5C0BFX8WxBwCWxY9CsoeE%2FX7LOq%2BjEsOWa5ZyAQeLzXxU8Q9QT2cB5IttdOJWszJiD4pIo8RKNn06OuqBWIg7IWGhjosYSbJzrtss9LnVUb987RotD7lNqbUX5WSNGZ6Az%2FpsXPUUwGlEzuE24z1Y5wJJ0uqhBzzTIT0a4%2BJjFGZIBlKdj5cDNflzHJBOpxwa%2Bo9u0NBxlg%2F%2FtEdhjVQWkUQ1ARJfZanq9Hx2GwB2O32E1XES5JkCAiW0G%2FKtGstfLuDJ5TcwXSLD60SYGw3JZCgKOBcNuY2cxe24flTEfRlvTS1PwNy4n67xLiGHW0J17MkxT4MR7kEuf9lq3tA5q86%2BvL84JeWMRew41o1VBI2iRgrixmReZ8qZFS%2Fvp0hyxsc5mba1VOOHvAaHocXuCtysOiG2KEMZByhPuAvDhZ5R%2F3KQrH9K0UbJHLax3fw56l82JMiQtcdEUGXV0g5HZuU8%2FP9Cdp2MrWtH%2F0ojQLSI66s%2FjMrUsdjbi8%2BMLOEh9YGOqUBA9kG%2FwOBjB92lKHfGgjGbsvKAuBBMVs%2FkGkGUaTMlPgigAxzzl%2FgJEu02IZAgyetYnJsjuAmXhH3%2BBPFirq%2FHMOW2LiyEqqzeNmQ7bxzUBj0pHymX8zK5p6LeXnQqXuYVthkkXAQndFQGq%2FxIvZOLURH%2FkgYHzVKgANkDcMq6jrj%2Bp2IFTG7Ap6Pa2YugCQECXEv300ZS2pZJqsLiAxuS7Wi9Qtb&X-Amz-Signature=b98ff1037bd3aaf12791eebbbc979afd7756e1aaa1b0b4fb8db1b19b0b1a0249&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TXPK6VI%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032233Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDtfyjLZJT0UpDf1oDIeJbXKCW%2BkQ5wLaZY5niJmXOQrAIgPcHjC%2FtHpzcWd09efvNEk0t5fLYYs66pzE0aIEpS1zoqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNjq%2BrNNdy%2FiNBGarSrcA1np0xEccmlBqnGUzAKjEiiKvlRB%2BjIa4rlikZ9OgzMLIcz%2F4A2k%2F%2FX3bKSKTTJT%2BvjJ9DrauXP9M8tIoS7eIXxfeAq%2F63sUICl5RTNBgXU6puhLhmMGKibhPEvneYMQ5C0BFX8WxBwCWxY9CsoeE%2FX7LOq%2BjEsOWa5ZyAQeLzXxU8Q9QT2cB5IttdOJWszJiD4pIo8RKNn06OuqBWIg7IWGhjosYSbJzrtss9LnVUb987RotD7lNqbUX5WSNGZ6Az%2FpsXPUUwGlEzuE24z1Y5wJJ0uqhBzzTIT0a4%2BJjFGZIBlKdj5cDNflzHJBOpxwa%2Bo9u0NBxlg%2F%2FtEdhjVQWkUQ1ARJfZanq9Hx2GwB2O32E1XES5JkCAiW0G%2FKtGstfLuDJ5TcwXSLD60SYGw3JZCgKOBcNuY2cxe24flTEfRlvTS1PwNy4n67xLiGHW0J17MkxT4MR7kEuf9lq3tA5q86%2BvL84JeWMRew41o1VBI2iRgrixmReZ8qZFS%2Fvp0hyxsc5mba1VOOHvAaHocXuCtysOiG2KEMZByhPuAvDhZ5R%2F3KQrH9K0UbJHLax3fw56l82JMiQtcdEUGXV0g5HZuU8%2FP9Cdp2MrWtH%2F0ojQLSI66s%2FjMrUsdjbi8%2BMLOEh9YGOqUBA9kG%2FwOBjB92lKHfGgjGbsvKAuBBMVs%2FkGkGUaTMlPgigAxzzl%2FgJEu02IZAgyetYnJsjuAmXhH3%2BBPFirq%2FHMOW2LiyEqqzeNmQ7bxzUBj0pHymX8zK5p6LeXnQqXuYVthkkXAQndFQGq%2FxIvZOLURH%2FkgYHzVKgANkDcMq6jrj%2Bp2IFTG7Ap6Pa2YugCQECXEv300ZS2pZJqsLiAxuS7Wi9Qtb&X-Amz-Signature=32637b307c5b004e43feb4285d4dbc8e9aca929117376379f9527fdfce4d99a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TXPK6VI%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032233Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDtfyjLZJT0UpDf1oDIeJbXKCW%2BkQ5wLaZY5niJmXOQrAIgPcHjC%2FtHpzcWd09efvNEk0t5fLYYs66pzE0aIEpS1zoqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNjq%2BrNNdy%2FiNBGarSrcA1np0xEccmlBqnGUzAKjEiiKvlRB%2BjIa4rlikZ9OgzMLIcz%2F4A2k%2F%2FX3bKSKTTJT%2BvjJ9DrauXP9M8tIoS7eIXxfeAq%2F63sUICl5RTNBgXU6puhLhmMGKibhPEvneYMQ5C0BFX8WxBwCWxY9CsoeE%2FX7LOq%2BjEsOWa5ZyAQeLzXxU8Q9QT2cB5IttdOJWszJiD4pIo8RKNn06OuqBWIg7IWGhjosYSbJzrtss9LnVUb987RotD7lNqbUX5WSNGZ6Az%2FpsXPUUwGlEzuE24z1Y5wJJ0uqhBzzTIT0a4%2BJjFGZIBlKdj5cDNflzHJBOpxwa%2Bo9u0NBxlg%2F%2FtEdhjVQWkUQ1ARJfZanq9Hx2GwB2O32E1XES5JkCAiW0G%2FKtGstfLuDJ5TcwXSLD60SYGw3JZCgKOBcNuY2cxe24flTEfRlvTS1PwNy4n67xLiGHW0J17MkxT4MR7kEuf9lq3tA5q86%2BvL84JeWMRew41o1VBI2iRgrixmReZ8qZFS%2Fvp0hyxsc5mba1VOOHvAaHocXuCtysOiG2KEMZByhPuAvDhZ5R%2F3KQrH9K0UbJHLax3fw56l82JMiQtcdEUGXV0g5HZuU8%2FP9Cdp2MrWtH%2F0ojQLSI66s%2FjMrUsdjbi8%2BMLOEh9YGOqUBA9kG%2FwOBjB92lKHfGgjGbsvKAuBBMVs%2FkGkGUaTMlPgigAxzzl%2FgJEu02IZAgyetYnJsjuAmXhH3%2BBPFirq%2FHMOW2LiyEqqzeNmQ7bxzUBj0pHymX8zK5p6LeXnQqXuYVthkkXAQndFQGq%2FxIvZOLURH%2FkgYHzVKgANkDcMq6jrj%2Bp2IFTG7Ap6Pa2YugCQECXEv300ZS2pZJqsLiAxuS7Wi9Qtb&X-Amz-Signature=d09f1f5ecef6672e6836896b43816720cab9422d425f351cab09584d79563e74&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TXPK6VI%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032233Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDtfyjLZJT0UpDf1oDIeJbXKCW%2BkQ5wLaZY5niJmXOQrAIgPcHjC%2FtHpzcWd09efvNEk0t5fLYYs66pzE0aIEpS1zoqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNjq%2BrNNdy%2FiNBGarSrcA1np0xEccmlBqnGUzAKjEiiKvlRB%2BjIa4rlikZ9OgzMLIcz%2F4A2k%2F%2FX3bKSKTTJT%2BvjJ9DrauXP9M8tIoS7eIXxfeAq%2F63sUICl5RTNBgXU6puhLhmMGKibhPEvneYMQ5C0BFX8WxBwCWxY9CsoeE%2FX7LOq%2BjEsOWa5ZyAQeLzXxU8Q9QT2cB5IttdOJWszJiD4pIo8RKNn06OuqBWIg7IWGhjosYSbJzrtss9LnVUb987RotD7lNqbUX5WSNGZ6Az%2FpsXPUUwGlEzuE24z1Y5wJJ0uqhBzzTIT0a4%2BJjFGZIBlKdj5cDNflzHJBOpxwa%2Bo9u0NBxlg%2F%2FtEdhjVQWkUQ1ARJfZanq9Hx2GwB2O32E1XES5JkCAiW0G%2FKtGstfLuDJ5TcwXSLD60SYGw3JZCgKOBcNuY2cxe24flTEfRlvTS1PwNy4n67xLiGHW0J17MkxT4MR7kEuf9lq3tA5q86%2BvL84JeWMRew41o1VBI2iRgrixmReZ8qZFS%2Fvp0hyxsc5mba1VOOHvAaHocXuCtysOiG2KEMZByhPuAvDhZ5R%2F3KQrH9K0UbJHLax3fw56l82JMiQtcdEUGXV0g5HZuU8%2FP9Cdp2MrWtH%2F0ojQLSI66s%2FjMrUsdjbi8%2BMLOEh9YGOqUBA9kG%2FwOBjB92lKHfGgjGbsvKAuBBMVs%2FkGkGUaTMlPgigAxzzl%2FgJEu02IZAgyetYnJsjuAmXhH3%2BBPFirq%2FHMOW2LiyEqqzeNmQ7bxzUBj0pHymX8zK5p6LeXnQqXuYVthkkXAQndFQGq%2FxIvZOLURH%2FkgYHzVKgANkDcMq6jrj%2Bp2IFTG7Ap6Pa2YugCQECXEv300ZS2pZJqsLiAxuS7Wi9Qtb&X-Amz-Signature=a6284af501b9ef6a91450abb1035a39ea92e1c840ef1f330985e53e8f1399186&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TXPK6VI%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032233Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDtfyjLZJT0UpDf1oDIeJbXKCW%2BkQ5wLaZY5niJmXOQrAIgPcHjC%2FtHpzcWd09efvNEk0t5fLYYs66pzE0aIEpS1zoqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNjq%2BrNNdy%2FiNBGarSrcA1np0xEccmlBqnGUzAKjEiiKvlRB%2BjIa4rlikZ9OgzMLIcz%2F4A2k%2F%2FX3bKSKTTJT%2BvjJ9DrauXP9M8tIoS7eIXxfeAq%2F63sUICl5RTNBgXU6puhLhmMGKibhPEvneYMQ5C0BFX8WxBwCWxY9CsoeE%2FX7LOq%2BjEsOWa5ZyAQeLzXxU8Q9QT2cB5IttdOJWszJiD4pIo8RKNn06OuqBWIg7IWGhjosYSbJzrtss9LnVUb987RotD7lNqbUX5WSNGZ6Az%2FpsXPUUwGlEzuE24z1Y5wJJ0uqhBzzTIT0a4%2BJjFGZIBlKdj5cDNflzHJBOpxwa%2Bo9u0NBxlg%2F%2FtEdhjVQWkUQ1ARJfZanq9Hx2GwB2O32E1XES5JkCAiW0G%2FKtGstfLuDJ5TcwXSLD60SYGw3JZCgKOBcNuY2cxe24flTEfRlvTS1PwNy4n67xLiGHW0J17MkxT4MR7kEuf9lq3tA5q86%2BvL84JeWMRew41o1VBI2iRgrixmReZ8qZFS%2Fvp0hyxsc5mba1VOOHvAaHocXuCtysOiG2KEMZByhPuAvDhZ5R%2F3KQrH9K0UbJHLax3fw56l82JMiQtcdEUGXV0g5HZuU8%2FP9Cdp2MrWtH%2F0ojQLSI66s%2FjMrUsdjbi8%2BMLOEh9YGOqUBA9kG%2FwOBjB92lKHfGgjGbsvKAuBBMVs%2FkGkGUaTMlPgigAxzzl%2FgJEu02IZAgyetYnJsjuAmXhH3%2BBPFirq%2FHMOW2LiyEqqzeNmQ7bxzUBj0pHymX8zK5p6LeXnQqXuYVthkkXAQndFQGq%2FxIvZOLURH%2FkgYHzVKgANkDcMq6jrj%2Bp2IFTG7Ap6Pa2YugCQECXEv300ZS2pZJqsLiAxuS7Wi9Qtb&X-Amz-Signature=820933d2b2eb0b1784a7be5f109e49950bf28a91682bb8e61ee618ed4cf06703&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TXPK6VI%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032233Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDtfyjLZJT0UpDf1oDIeJbXKCW%2BkQ5wLaZY5niJmXOQrAIgPcHjC%2FtHpzcWd09efvNEk0t5fLYYs66pzE0aIEpS1zoqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNjq%2BrNNdy%2FiNBGarSrcA1np0xEccmlBqnGUzAKjEiiKvlRB%2BjIa4rlikZ9OgzMLIcz%2F4A2k%2F%2FX3bKSKTTJT%2BvjJ9DrauXP9M8tIoS7eIXxfeAq%2F63sUICl5RTNBgXU6puhLhmMGKibhPEvneYMQ5C0BFX8WxBwCWxY9CsoeE%2FX7LOq%2BjEsOWa5ZyAQeLzXxU8Q9QT2cB5IttdOJWszJiD4pIo8RKNn06OuqBWIg7IWGhjosYSbJzrtss9LnVUb987RotD7lNqbUX5WSNGZ6Az%2FpsXPUUwGlEzuE24z1Y5wJJ0uqhBzzTIT0a4%2BJjFGZIBlKdj5cDNflzHJBOpxwa%2Bo9u0NBxlg%2F%2FtEdhjVQWkUQ1ARJfZanq9Hx2GwB2O32E1XES5JkCAiW0G%2FKtGstfLuDJ5TcwXSLD60SYGw3JZCgKOBcNuY2cxe24flTEfRlvTS1PwNy4n67xLiGHW0J17MkxT4MR7kEuf9lq3tA5q86%2BvL84JeWMRew41o1VBI2iRgrixmReZ8qZFS%2Fvp0hyxsc5mba1VOOHvAaHocXuCtysOiG2KEMZByhPuAvDhZ5R%2F3KQrH9K0UbJHLax3fw56l82JMiQtcdEUGXV0g5HZuU8%2FP9Cdp2MrWtH%2F0ojQLSI66s%2FjMrUsdjbi8%2BMLOEh9YGOqUBA9kG%2FwOBjB92lKHfGgjGbsvKAuBBMVs%2FkGkGUaTMlPgigAxzzl%2FgJEu02IZAgyetYnJsjuAmXhH3%2BBPFirq%2FHMOW2LiyEqqzeNmQ7bxzUBj0pHymX8zK5p6LeXnQqXuYVthkkXAQndFQGq%2FxIvZOLURH%2FkgYHzVKgANkDcMq6jrj%2Bp2IFTG7Ap6Pa2YugCQECXEv300ZS2pZJqsLiAxuS7Wi9Qtb&X-Amz-Signature=307a38b23ec13ae3d8c33f66d217b72d059ebc2432e746cce2da3a2c4de4c7d4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TXPK6VI%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032233Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDtfyjLZJT0UpDf1oDIeJbXKCW%2BkQ5wLaZY5niJmXOQrAIgPcHjC%2FtHpzcWd09efvNEk0t5fLYYs66pzE0aIEpS1zoqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNjq%2BrNNdy%2FiNBGarSrcA1np0xEccmlBqnGUzAKjEiiKvlRB%2BjIa4rlikZ9OgzMLIcz%2F4A2k%2F%2FX3bKSKTTJT%2BvjJ9DrauXP9M8tIoS7eIXxfeAq%2F63sUICl5RTNBgXU6puhLhmMGKibhPEvneYMQ5C0BFX8WxBwCWxY9CsoeE%2FX7LOq%2BjEsOWa5ZyAQeLzXxU8Q9QT2cB5IttdOJWszJiD4pIo8RKNn06OuqBWIg7IWGhjosYSbJzrtss9LnVUb987RotD7lNqbUX5WSNGZ6Az%2FpsXPUUwGlEzuE24z1Y5wJJ0uqhBzzTIT0a4%2BJjFGZIBlKdj5cDNflzHJBOpxwa%2Bo9u0NBxlg%2F%2FtEdhjVQWkUQ1ARJfZanq9Hx2GwB2O32E1XES5JkCAiW0G%2FKtGstfLuDJ5TcwXSLD60SYGw3JZCgKOBcNuY2cxe24flTEfRlvTS1PwNy4n67xLiGHW0J17MkxT4MR7kEuf9lq3tA5q86%2BvL84JeWMRew41o1VBI2iRgrixmReZ8qZFS%2Fvp0hyxsc5mba1VOOHvAaHocXuCtysOiG2KEMZByhPuAvDhZ5R%2F3KQrH9K0UbJHLax3fw56l82JMiQtcdEUGXV0g5HZuU8%2FP9Cdp2MrWtH%2F0ojQLSI66s%2FjMrUsdjbi8%2BMLOEh9YGOqUBA9kG%2FwOBjB92lKHfGgjGbsvKAuBBMVs%2FkGkGUaTMlPgigAxzzl%2FgJEu02IZAgyetYnJsjuAmXhH3%2BBPFirq%2FHMOW2LiyEqqzeNmQ7bxzUBj0pHymX8zK5p6LeXnQqXuYVthkkXAQndFQGq%2FxIvZOLURH%2FkgYHzVKgANkDcMq6jrj%2Bp2IFTG7Ap6Pa2YugCQECXEv300ZS2pZJqsLiAxuS7Wi9Qtb&X-Amz-Signature=c566895d0800f4c9765f9128e3ca891a89bffe9d8c7ff5cf8e0beca91f2ca7b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
