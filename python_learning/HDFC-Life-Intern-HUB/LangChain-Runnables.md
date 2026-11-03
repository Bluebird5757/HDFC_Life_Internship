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
fetched_at: '2026-11-03T04:32:57.824Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNHFNPE5%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQCvxY4zRI%2FOPxXYP%2FTk9TkvUN3%2B6T9wilBx5sLXkdigIgQyZUx4iLWbMls%2B4kcqPBwTWibvzUIMn%2FLoR4lhMLP5cqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJVwbF7NwitEhCWzCrcA%2FkvFoSiRxEDw1mD%2BvrdkkBX5xbDageeE499ivgaL34DX4JLMGBnx8ZrRgAubYk4f4kHeYgl1W9WAK8o48YkkXca5EOTd696Fz%2FuER2Ml6SsqFxrcps3DCOq036ASnwsrtPEvakmuNRdknQW7s8KYTts%2FJdDiB%2FJTQfDj09RiXpDacFJWnl14LaWpOYbdLW8bgGXfP5W0lUYAxAPT0atNmrww5NWLf8weZUNGbckj3J%2BSNG5hf99u4ZCkvKLQMCWOgtg2oyqiq%2BdYHJSym7qqpLZTTjgEVu14tAMZ4xpkIrGzGMXMMQP5738IkD8TUem9mkqxtwZ1%2FLgYxN%2FmHs7EN%2BCeybebGHx%2F%2BdA9M9fKRDFInqHuXhX39GVkmIB3fQ3b2Prhd4MSFEE8H%2FpcQgsqgOq%2FLogukFl0rLmXk5twYBgS5S3kjGUWe1fDbleqGxg6HDbn1U%2BiHksUY69KOX5LexP1%2B5q9NdYd5lFR4GhKfOflKr9fxHfJBPBcGO6Lj8jCfgfwN6tmacwyo7mDeI7nBPFmHtMj%2BhYndO9mYDKTONjkqjK4WwVLpt3TDFw71goQPqky%2B3pOqL8x4qJShhuygcZY%2FLAwtQC%2FK8h6gVBTMIqPuUKrVXQP7QRGuHJMPGVpdcGOqUBcnilFMS6pmBK1KTGZ0weCyrph1MfRy3y5CL8czDRAzc7IubHPEN4c3muQUrVz9JfExvpauKaTeIWY1Z2XQlNTSHjsbVeotxJkyzC1ogrUg67oCa3BUGyaDmAwpFXW2RsLBvbkpuXSbON7B92uElcSXuW5MxjmP784XkZrgx8AMcGUhBabXt9uPXwAYNOOZBHNM1Av69X6PiN7Bh%2BVvjTKNzKamPA&X-Amz-Signature=ff5b58feeed1616552b0ee101cd161b8c1f77a05ba15b0976ff06678e62d6cd0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNHFNPE5%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQCvxY4zRI%2FOPxXYP%2FTk9TkvUN3%2B6T9wilBx5sLXkdigIgQyZUx4iLWbMls%2B4kcqPBwTWibvzUIMn%2FLoR4lhMLP5cqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJVwbF7NwitEhCWzCrcA%2FkvFoSiRxEDw1mD%2BvrdkkBX5xbDageeE499ivgaL34DX4JLMGBnx8ZrRgAubYk4f4kHeYgl1W9WAK8o48YkkXca5EOTd696Fz%2FuER2Ml6SsqFxrcps3DCOq036ASnwsrtPEvakmuNRdknQW7s8KYTts%2FJdDiB%2FJTQfDj09RiXpDacFJWnl14LaWpOYbdLW8bgGXfP5W0lUYAxAPT0atNmrww5NWLf8weZUNGbckj3J%2BSNG5hf99u4ZCkvKLQMCWOgtg2oyqiq%2BdYHJSym7qqpLZTTjgEVu14tAMZ4xpkIrGzGMXMMQP5738IkD8TUem9mkqxtwZ1%2FLgYxN%2FmHs7EN%2BCeybebGHx%2F%2BdA9M9fKRDFInqHuXhX39GVkmIB3fQ3b2Prhd4MSFEE8H%2FpcQgsqgOq%2FLogukFl0rLmXk5twYBgS5S3kjGUWe1fDbleqGxg6HDbn1U%2BiHksUY69KOX5LexP1%2B5q9NdYd5lFR4GhKfOflKr9fxHfJBPBcGO6Lj8jCfgfwN6tmacwyo7mDeI7nBPFmHtMj%2BhYndO9mYDKTONjkqjK4WwVLpt3TDFw71goQPqky%2B3pOqL8x4qJShhuygcZY%2FLAwtQC%2FK8h6gVBTMIqPuUKrVXQP7QRGuHJMPGVpdcGOqUBcnilFMS6pmBK1KTGZ0weCyrph1MfRy3y5CL8czDRAzc7IubHPEN4c3muQUrVz9JfExvpauKaTeIWY1Z2XQlNTSHjsbVeotxJkyzC1ogrUg67oCa3BUGyaDmAwpFXW2RsLBvbkpuXSbON7B92uElcSXuW5MxjmP784XkZrgx8AMcGUhBabXt9uPXwAYNOOZBHNM1Av69X6PiN7Bh%2BVvjTKNzKamPA&X-Amz-Signature=c0ed743954c90820d25b5cabd2f006eb685af4c9353f3a4cd0b39866796adc8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNHFNPE5%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQCvxY4zRI%2FOPxXYP%2FTk9TkvUN3%2B6T9wilBx5sLXkdigIgQyZUx4iLWbMls%2B4kcqPBwTWibvzUIMn%2FLoR4lhMLP5cqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJVwbF7NwitEhCWzCrcA%2FkvFoSiRxEDw1mD%2BvrdkkBX5xbDageeE499ivgaL34DX4JLMGBnx8ZrRgAubYk4f4kHeYgl1W9WAK8o48YkkXca5EOTd696Fz%2FuER2Ml6SsqFxrcps3DCOq036ASnwsrtPEvakmuNRdknQW7s8KYTts%2FJdDiB%2FJTQfDj09RiXpDacFJWnl14LaWpOYbdLW8bgGXfP5W0lUYAxAPT0atNmrww5NWLf8weZUNGbckj3J%2BSNG5hf99u4ZCkvKLQMCWOgtg2oyqiq%2BdYHJSym7qqpLZTTjgEVu14tAMZ4xpkIrGzGMXMMQP5738IkD8TUem9mkqxtwZ1%2FLgYxN%2FmHs7EN%2BCeybebGHx%2F%2BdA9M9fKRDFInqHuXhX39GVkmIB3fQ3b2Prhd4MSFEE8H%2FpcQgsqgOq%2FLogukFl0rLmXk5twYBgS5S3kjGUWe1fDbleqGxg6HDbn1U%2BiHksUY69KOX5LexP1%2B5q9NdYd5lFR4GhKfOflKr9fxHfJBPBcGO6Lj8jCfgfwN6tmacwyo7mDeI7nBPFmHtMj%2BhYndO9mYDKTONjkqjK4WwVLpt3TDFw71goQPqky%2B3pOqL8x4qJShhuygcZY%2FLAwtQC%2FK8h6gVBTMIqPuUKrVXQP7QRGuHJMPGVpdcGOqUBcnilFMS6pmBK1KTGZ0weCyrph1MfRy3y5CL8czDRAzc7IubHPEN4c3muQUrVz9JfExvpauKaTeIWY1Z2XQlNTSHjsbVeotxJkyzC1ogrUg67oCa3BUGyaDmAwpFXW2RsLBvbkpuXSbON7B92uElcSXuW5MxjmP784XkZrgx8AMcGUhBabXt9uPXwAYNOOZBHNM1Av69X6PiN7Bh%2BVvjTKNzKamPA&X-Amz-Signature=e8b1ab34ebc9d03d0ef8225f255b40b8adb9238575188e8ed00d7a93c8fef77c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNHFNPE5%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQCvxY4zRI%2FOPxXYP%2FTk9TkvUN3%2B6T9wilBx5sLXkdigIgQyZUx4iLWbMls%2B4kcqPBwTWibvzUIMn%2FLoR4lhMLP5cqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJVwbF7NwitEhCWzCrcA%2FkvFoSiRxEDw1mD%2BvrdkkBX5xbDageeE499ivgaL34DX4JLMGBnx8ZrRgAubYk4f4kHeYgl1W9WAK8o48YkkXca5EOTd696Fz%2FuER2Ml6SsqFxrcps3DCOq036ASnwsrtPEvakmuNRdknQW7s8KYTts%2FJdDiB%2FJTQfDj09RiXpDacFJWnl14LaWpOYbdLW8bgGXfP5W0lUYAxAPT0atNmrww5NWLf8weZUNGbckj3J%2BSNG5hf99u4ZCkvKLQMCWOgtg2oyqiq%2BdYHJSym7qqpLZTTjgEVu14tAMZ4xpkIrGzGMXMMQP5738IkD8TUem9mkqxtwZ1%2FLgYxN%2FmHs7EN%2BCeybebGHx%2F%2BdA9M9fKRDFInqHuXhX39GVkmIB3fQ3b2Prhd4MSFEE8H%2FpcQgsqgOq%2FLogukFl0rLmXk5twYBgS5S3kjGUWe1fDbleqGxg6HDbn1U%2BiHksUY69KOX5LexP1%2B5q9NdYd5lFR4GhKfOflKr9fxHfJBPBcGO6Lj8jCfgfwN6tmacwyo7mDeI7nBPFmHtMj%2BhYndO9mYDKTONjkqjK4WwVLpt3TDFw71goQPqky%2B3pOqL8x4qJShhuygcZY%2FLAwtQC%2FK8h6gVBTMIqPuUKrVXQP7QRGuHJMPGVpdcGOqUBcnilFMS6pmBK1KTGZ0weCyrph1MfRy3y5CL8czDRAzc7IubHPEN4c3muQUrVz9JfExvpauKaTeIWY1Z2XQlNTSHjsbVeotxJkyzC1ogrUg67oCa3BUGyaDmAwpFXW2RsLBvbkpuXSbON7B92uElcSXuW5MxjmP784XkZrgx8AMcGUhBabXt9uPXwAYNOOZBHNM1Av69X6PiN7Bh%2BVvjTKNzKamPA&X-Amz-Signature=8a9ca1e1f1ec2ac74dbed1be408d702eaff8796131549a8bed06061557f1c1c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNHFNPE5%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQCvxY4zRI%2FOPxXYP%2FTk9TkvUN3%2B6T9wilBx5sLXkdigIgQyZUx4iLWbMls%2B4kcqPBwTWibvzUIMn%2FLoR4lhMLP5cqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJVwbF7NwitEhCWzCrcA%2FkvFoSiRxEDw1mD%2BvrdkkBX5xbDageeE499ivgaL34DX4JLMGBnx8ZrRgAubYk4f4kHeYgl1W9WAK8o48YkkXca5EOTd696Fz%2FuER2Ml6SsqFxrcps3DCOq036ASnwsrtPEvakmuNRdknQW7s8KYTts%2FJdDiB%2FJTQfDj09RiXpDacFJWnl14LaWpOYbdLW8bgGXfP5W0lUYAxAPT0atNmrww5NWLf8weZUNGbckj3J%2BSNG5hf99u4ZCkvKLQMCWOgtg2oyqiq%2BdYHJSym7qqpLZTTjgEVu14tAMZ4xpkIrGzGMXMMQP5738IkD8TUem9mkqxtwZ1%2FLgYxN%2FmHs7EN%2BCeybebGHx%2F%2BdA9M9fKRDFInqHuXhX39GVkmIB3fQ3b2Prhd4MSFEE8H%2FpcQgsqgOq%2FLogukFl0rLmXk5twYBgS5S3kjGUWe1fDbleqGxg6HDbn1U%2BiHksUY69KOX5LexP1%2B5q9NdYd5lFR4GhKfOflKr9fxHfJBPBcGO6Lj8jCfgfwN6tmacwyo7mDeI7nBPFmHtMj%2BhYndO9mYDKTONjkqjK4WwVLpt3TDFw71goQPqky%2B3pOqL8x4qJShhuygcZY%2FLAwtQC%2FK8h6gVBTMIqPuUKrVXQP7QRGuHJMPGVpdcGOqUBcnilFMS6pmBK1KTGZ0weCyrph1MfRy3y5CL8czDRAzc7IubHPEN4c3muQUrVz9JfExvpauKaTeIWY1Z2XQlNTSHjsbVeotxJkyzC1ogrUg67oCa3BUGyaDmAwpFXW2RsLBvbkpuXSbON7B92uElcSXuW5MxjmP784XkZrgx8AMcGUhBabXt9uPXwAYNOOZBHNM1Av69X6PiN7Bh%2BVvjTKNzKamPA&X-Amz-Signature=57c4fc00ec496f3e892dd4b0febcaa25c5c0d07e9f5eceffcad5eee7990c5925&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNHFNPE5%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQCvxY4zRI%2FOPxXYP%2FTk9TkvUN3%2B6T9wilBx5sLXkdigIgQyZUx4iLWbMls%2B4kcqPBwTWibvzUIMn%2FLoR4lhMLP5cqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJVwbF7NwitEhCWzCrcA%2FkvFoSiRxEDw1mD%2BvrdkkBX5xbDageeE499ivgaL34DX4JLMGBnx8ZrRgAubYk4f4kHeYgl1W9WAK8o48YkkXca5EOTd696Fz%2FuER2Ml6SsqFxrcps3DCOq036ASnwsrtPEvakmuNRdknQW7s8KYTts%2FJdDiB%2FJTQfDj09RiXpDacFJWnl14LaWpOYbdLW8bgGXfP5W0lUYAxAPT0atNmrww5NWLf8weZUNGbckj3J%2BSNG5hf99u4ZCkvKLQMCWOgtg2oyqiq%2BdYHJSym7qqpLZTTjgEVu14tAMZ4xpkIrGzGMXMMQP5738IkD8TUem9mkqxtwZ1%2FLgYxN%2FmHs7EN%2BCeybebGHx%2F%2BdA9M9fKRDFInqHuXhX39GVkmIB3fQ3b2Prhd4MSFEE8H%2FpcQgsqgOq%2FLogukFl0rLmXk5twYBgS5S3kjGUWe1fDbleqGxg6HDbn1U%2BiHksUY69KOX5LexP1%2B5q9NdYd5lFR4GhKfOflKr9fxHfJBPBcGO6Lj8jCfgfwN6tmacwyo7mDeI7nBPFmHtMj%2BhYndO9mYDKTONjkqjK4WwVLpt3TDFw71goQPqky%2B3pOqL8x4qJShhuygcZY%2FLAwtQC%2FK8h6gVBTMIqPuUKrVXQP7QRGuHJMPGVpdcGOqUBcnilFMS6pmBK1KTGZ0weCyrph1MfRy3y5CL8czDRAzc7IubHPEN4c3muQUrVz9JfExvpauKaTeIWY1Z2XQlNTSHjsbVeotxJkyzC1ogrUg67oCa3BUGyaDmAwpFXW2RsLBvbkpuXSbON7B92uElcSXuW5MxjmP784XkZrgx8AMcGUhBabXt9uPXwAYNOOZBHNM1Av69X6PiN7Bh%2BVvjTKNzKamPA&X-Amz-Signature=17db555c69667a8870a9dd63f5bb111e909923b6e7827e65511309822cb83eb8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNHFNPE5%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQCvxY4zRI%2FOPxXYP%2FTk9TkvUN3%2B6T9wilBx5sLXkdigIgQyZUx4iLWbMls%2B4kcqPBwTWibvzUIMn%2FLoR4lhMLP5cqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJVwbF7NwitEhCWzCrcA%2FkvFoSiRxEDw1mD%2BvrdkkBX5xbDageeE499ivgaL34DX4JLMGBnx8ZrRgAubYk4f4kHeYgl1W9WAK8o48YkkXca5EOTd696Fz%2FuER2Ml6SsqFxrcps3DCOq036ASnwsrtPEvakmuNRdknQW7s8KYTts%2FJdDiB%2FJTQfDj09RiXpDacFJWnl14LaWpOYbdLW8bgGXfP5W0lUYAxAPT0atNmrww5NWLf8weZUNGbckj3J%2BSNG5hf99u4ZCkvKLQMCWOgtg2oyqiq%2BdYHJSym7qqpLZTTjgEVu14tAMZ4xpkIrGzGMXMMQP5738IkD8TUem9mkqxtwZ1%2FLgYxN%2FmHs7EN%2BCeybebGHx%2F%2BdA9M9fKRDFInqHuXhX39GVkmIB3fQ3b2Prhd4MSFEE8H%2FpcQgsqgOq%2FLogukFl0rLmXk5twYBgS5S3kjGUWe1fDbleqGxg6HDbn1U%2BiHksUY69KOX5LexP1%2B5q9NdYd5lFR4GhKfOflKr9fxHfJBPBcGO6Lj8jCfgfwN6tmacwyo7mDeI7nBPFmHtMj%2BhYndO9mYDKTONjkqjK4WwVLpt3TDFw71goQPqky%2B3pOqL8x4qJShhuygcZY%2FLAwtQC%2FK8h6gVBTMIqPuUKrVXQP7QRGuHJMPGVpdcGOqUBcnilFMS6pmBK1KTGZ0weCyrph1MfRy3y5CL8czDRAzc7IubHPEN4c3muQUrVz9JfExvpauKaTeIWY1Z2XQlNTSHjsbVeotxJkyzC1ogrUg67oCa3BUGyaDmAwpFXW2RsLBvbkpuXSbON7B92uElcSXuW5MxjmP784XkZrgx8AMcGUhBabXt9uPXwAYNOOZBHNM1Av69X6PiN7Bh%2BVvjTKNzKamPA&X-Amz-Signature=15478b032b86b0b01da52980b3895722c99eefbe945f454f3d9ebbe9ca1c0a72&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNHFNPE5%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQCvxY4zRI%2FOPxXYP%2FTk9TkvUN3%2B6T9wilBx5sLXkdigIgQyZUx4iLWbMls%2B4kcqPBwTWibvzUIMn%2FLoR4lhMLP5cqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJVwbF7NwitEhCWzCrcA%2FkvFoSiRxEDw1mD%2BvrdkkBX5xbDageeE499ivgaL34DX4JLMGBnx8ZrRgAubYk4f4kHeYgl1W9WAK8o48YkkXca5EOTd696Fz%2FuER2Ml6SsqFxrcps3DCOq036ASnwsrtPEvakmuNRdknQW7s8KYTts%2FJdDiB%2FJTQfDj09RiXpDacFJWnl14LaWpOYbdLW8bgGXfP5W0lUYAxAPT0atNmrww5NWLf8weZUNGbckj3J%2BSNG5hf99u4ZCkvKLQMCWOgtg2oyqiq%2BdYHJSym7qqpLZTTjgEVu14tAMZ4xpkIrGzGMXMMQP5738IkD8TUem9mkqxtwZ1%2FLgYxN%2FmHs7EN%2BCeybebGHx%2F%2BdA9M9fKRDFInqHuXhX39GVkmIB3fQ3b2Prhd4MSFEE8H%2FpcQgsqgOq%2FLogukFl0rLmXk5twYBgS5S3kjGUWe1fDbleqGxg6HDbn1U%2BiHksUY69KOX5LexP1%2B5q9NdYd5lFR4GhKfOflKr9fxHfJBPBcGO6Lj8jCfgfwN6tmacwyo7mDeI7nBPFmHtMj%2BhYndO9mYDKTONjkqjK4WwVLpt3TDFw71goQPqky%2B3pOqL8x4qJShhuygcZY%2FLAwtQC%2FK8h6gVBTMIqPuUKrVXQP7QRGuHJMPGVpdcGOqUBcnilFMS6pmBK1KTGZ0weCyrph1MfRy3y5CL8czDRAzc7IubHPEN4c3muQUrVz9JfExvpauKaTeIWY1Z2XQlNTSHjsbVeotxJkyzC1ogrUg67oCa3BUGyaDmAwpFXW2RsLBvbkpuXSbON7B92uElcSXuW5MxjmP784XkZrgx8AMcGUhBabXt9uPXwAYNOOZBHNM1Av69X6PiN7Bh%2BVvjTKNzKamPA&X-Amz-Signature=09fd088d8d77354907058438d85323e8ab4492e408851c4862d5fdefbbd6f878&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNHFNPE5%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQCvxY4zRI%2FOPxXYP%2FTk9TkvUN3%2B6T9wilBx5sLXkdigIgQyZUx4iLWbMls%2B4kcqPBwTWibvzUIMn%2FLoR4lhMLP5cqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIJVwbF7NwitEhCWzCrcA%2FkvFoSiRxEDw1mD%2BvrdkkBX5xbDageeE499ivgaL34DX4JLMGBnx8ZrRgAubYk4f4kHeYgl1W9WAK8o48YkkXca5EOTd696Fz%2FuER2Ml6SsqFxrcps3DCOq036ASnwsrtPEvakmuNRdknQW7s8KYTts%2FJdDiB%2FJTQfDj09RiXpDacFJWnl14LaWpOYbdLW8bgGXfP5W0lUYAxAPT0atNmrww5NWLf8weZUNGbckj3J%2BSNG5hf99u4ZCkvKLQMCWOgtg2oyqiq%2BdYHJSym7qqpLZTTjgEVu14tAMZ4xpkIrGzGMXMMQP5738IkD8TUem9mkqxtwZ1%2FLgYxN%2FmHs7EN%2BCeybebGHx%2F%2BdA9M9fKRDFInqHuXhX39GVkmIB3fQ3b2Prhd4MSFEE8H%2FpcQgsqgOq%2FLogukFl0rLmXk5twYBgS5S3kjGUWe1fDbleqGxg6HDbn1U%2BiHksUY69KOX5LexP1%2B5q9NdYd5lFR4GhKfOflKr9fxHfJBPBcGO6Lj8jCfgfwN6tmacwyo7mDeI7nBPFmHtMj%2BhYndO9mYDKTONjkqjK4WwVLpt3TDFw71goQPqky%2B3pOqL8x4qJShhuygcZY%2FLAwtQC%2FK8h6gVBTMIqPuUKrVXQP7QRGuHJMPGVpdcGOqUBcnilFMS6pmBK1KTGZ0weCyrph1MfRy3y5CL8czDRAzc7IubHPEN4c3muQUrVz9JfExvpauKaTeIWY1Z2XQlNTSHjsbVeotxJkyzC1ogrUg67oCa3BUGyaDmAwpFXW2RsLBvbkpuXSbON7B92uElcSXuW5MxjmP784XkZrgx8AMcGUhBabXt9uPXwAYNOOZBHNM1Av69X6PiN7Bh%2BVvjTKNzKamPA&X-Amz-Signature=28aea6306606bcefb00ada09bf42cb9f6961b8586a4e9719396a54de52d060ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
