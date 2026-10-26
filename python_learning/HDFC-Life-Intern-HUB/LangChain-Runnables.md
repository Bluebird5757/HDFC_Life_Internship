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
fetched_at: '2026-10-26T02:16:19.954Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7X5TSMN%2F20261026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261026T021615Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDVK%2Bkcn9KMwzN627tHE0WrN5fOH1v7ewv%2B9iuAMGLN6QIhAJdSVBH7hOlihpAldBuHTNVPvPdu43qxGYN6GAHPnkBeKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzwalwEenDBtGU5ivMq3ANRqVBUck5UhtmQURfLjGZfNCF38GmquvTyIm0W7ufzpr62Ew%2Bi1zFBvbxLPoi7u0jVbGzHA3gbNcfLW5p%2FlxoveqMSwvr8tT%2Bppd2K3ahwBGC0dy%2BBk7xm0tGtAamI%2BwLqfoCGVcJkkKaKETev51es5eN%2FKru77nv6qo0vYJqPqcjDEbrjgZzBQyYEa7fW%2B4yoKjL8XX4YhyTrnfEvA0j5s3AA6kW1T4AZE3Yi%2FVv6LLGP50cxE85bRoA65WbUxNEZU7vZs9CQ%2F5sRwCtu0zf%2Fr9rsFNMmOfimubBYycvPrmme7GcSX7ZUA%2FOXchLY6Mh%2BO8y%2BvyxNt2TsoZLA3qfTXhxl29CbA7ek0NfPmXEYmG6MeldvRaFMsP3TE8pPXuZc9eDrwEXpR05A8DIbFj%2Bu%2FpBZDKQtvS3a5Beol%2Faaij0dz0H2HKHP237EDLfkIwLkP%2F6%2BqGzJTB2mo2RnVFPXIaLzDKeg7tCxJwQlYfI%2FNJ5NGfkCsatX0fF5e57jHfrPBuREAj2Xfiktqq%2BrCDZmQGoMhnCO8iilllNwJ2tZTIOoB%2FJ5dcePDdVOy8pTIcXRvqc1eUfiDumzfr5xA8CZDEsrWCQBP5zk7rhxWgBZY3MtHopiuehIEYXRhjCfxPrWBjqkAQ1v5%2FA2xp2QzKTuMDi%2BnUlxhjSQiOlZrCunBbo0dyE%2B%2BDGUWX9QcV3sE8u6giKbyV2HC0TNrkCgq%2BKfYwT%2F%2BlpYkHke3FmoodwmUtLM%2Fuw0Mkj6G1tTFCMqhxNTZsrHOgd4q%2FQGZj%2BNHrFkT3568s5Mi%2B7ASVGv5%2BdePwX9e48O0zbc6fCi7mjZyZlC5WA6YSJ8VuU8yAthPvWFmqJ%2Bg%2BHvAYiz&X-Amz-Signature=81ae3ffc34ab3fb1b070621b3ca49a387da4fbc18a92fd740c1d31e70e4db99d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7X5TSMN%2F20261026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261026T021615Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDVK%2Bkcn9KMwzN627tHE0WrN5fOH1v7ewv%2B9iuAMGLN6QIhAJdSVBH7hOlihpAldBuHTNVPvPdu43qxGYN6GAHPnkBeKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzwalwEenDBtGU5ivMq3ANRqVBUck5UhtmQURfLjGZfNCF38GmquvTyIm0W7ufzpr62Ew%2Bi1zFBvbxLPoi7u0jVbGzHA3gbNcfLW5p%2FlxoveqMSwvr8tT%2Bppd2K3ahwBGC0dy%2BBk7xm0tGtAamI%2BwLqfoCGVcJkkKaKETev51es5eN%2FKru77nv6qo0vYJqPqcjDEbrjgZzBQyYEa7fW%2B4yoKjL8XX4YhyTrnfEvA0j5s3AA6kW1T4AZE3Yi%2FVv6LLGP50cxE85bRoA65WbUxNEZU7vZs9CQ%2F5sRwCtu0zf%2Fr9rsFNMmOfimubBYycvPrmme7GcSX7ZUA%2FOXchLY6Mh%2BO8y%2BvyxNt2TsoZLA3qfTXhxl29CbA7ek0NfPmXEYmG6MeldvRaFMsP3TE8pPXuZc9eDrwEXpR05A8DIbFj%2Bu%2FpBZDKQtvS3a5Beol%2Faaij0dz0H2HKHP237EDLfkIwLkP%2F6%2BqGzJTB2mo2RnVFPXIaLzDKeg7tCxJwQlYfI%2FNJ5NGfkCsatX0fF5e57jHfrPBuREAj2Xfiktqq%2BrCDZmQGoMhnCO8iilllNwJ2tZTIOoB%2FJ5dcePDdVOy8pTIcXRvqc1eUfiDumzfr5xA8CZDEsrWCQBP5zk7rhxWgBZY3MtHopiuehIEYXRhjCfxPrWBjqkAQ1v5%2FA2xp2QzKTuMDi%2BnUlxhjSQiOlZrCunBbo0dyE%2B%2BDGUWX9QcV3sE8u6giKbyV2HC0TNrkCgq%2BKfYwT%2F%2BlpYkHke3FmoodwmUtLM%2Fuw0Mkj6G1tTFCMqhxNTZsrHOgd4q%2FQGZj%2BNHrFkT3568s5Mi%2B7ASVGv5%2BdePwX9e48O0zbc6fCi7mjZyZlC5WA6YSJ8VuU8yAthPvWFmqJ%2Bg%2BHvAYiz&X-Amz-Signature=2e6ef7726cac757a807d686a3724038a54b1ea913b01ce71ea253cd3b8ab3c49&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7X5TSMN%2F20261026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261026T021615Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDVK%2Bkcn9KMwzN627tHE0WrN5fOH1v7ewv%2B9iuAMGLN6QIhAJdSVBH7hOlihpAldBuHTNVPvPdu43qxGYN6GAHPnkBeKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzwalwEenDBtGU5ivMq3ANRqVBUck5UhtmQURfLjGZfNCF38GmquvTyIm0W7ufzpr62Ew%2Bi1zFBvbxLPoi7u0jVbGzHA3gbNcfLW5p%2FlxoveqMSwvr8tT%2Bppd2K3ahwBGC0dy%2BBk7xm0tGtAamI%2BwLqfoCGVcJkkKaKETev51es5eN%2FKru77nv6qo0vYJqPqcjDEbrjgZzBQyYEa7fW%2B4yoKjL8XX4YhyTrnfEvA0j5s3AA6kW1T4AZE3Yi%2FVv6LLGP50cxE85bRoA65WbUxNEZU7vZs9CQ%2F5sRwCtu0zf%2Fr9rsFNMmOfimubBYycvPrmme7GcSX7ZUA%2FOXchLY6Mh%2BO8y%2BvyxNt2TsoZLA3qfTXhxl29CbA7ek0NfPmXEYmG6MeldvRaFMsP3TE8pPXuZc9eDrwEXpR05A8DIbFj%2Bu%2FpBZDKQtvS3a5Beol%2Faaij0dz0H2HKHP237EDLfkIwLkP%2F6%2BqGzJTB2mo2RnVFPXIaLzDKeg7tCxJwQlYfI%2FNJ5NGfkCsatX0fF5e57jHfrPBuREAj2Xfiktqq%2BrCDZmQGoMhnCO8iilllNwJ2tZTIOoB%2FJ5dcePDdVOy8pTIcXRvqc1eUfiDumzfr5xA8CZDEsrWCQBP5zk7rhxWgBZY3MtHopiuehIEYXRhjCfxPrWBjqkAQ1v5%2FA2xp2QzKTuMDi%2BnUlxhjSQiOlZrCunBbo0dyE%2B%2BDGUWX9QcV3sE8u6giKbyV2HC0TNrkCgq%2BKfYwT%2F%2BlpYkHke3FmoodwmUtLM%2Fuw0Mkj6G1tTFCMqhxNTZsrHOgd4q%2FQGZj%2BNHrFkT3568s5Mi%2B7ASVGv5%2BdePwX9e48O0zbc6fCi7mjZyZlC5WA6YSJ8VuU8yAthPvWFmqJ%2Bg%2BHvAYiz&X-Amz-Signature=e7520b41522f0c61fe10ad3db6a411bb3036cf3822d44ceb084ba497b1bb94ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7X5TSMN%2F20261026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261026T021615Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDVK%2Bkcn9KMwzN627tHE0WrN5fOH1v7ewv%2B9iuAMGLN6QIhAJdSVBH7hOlihpAldBuHTNVPvPdu43qxGYN6GAHPnkBeKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzwalwEenDBtGU5ivMq3ANRqVBUck5UhtmQURfLjGZfNCF38GmquvTyIm0W7ufzpr62Ew%2Bi1zFBvbxLPoi7u0jVbGzHA3gbNcfLW5p%2FlxoveqMSwvr8tT%2Bppd2K3ahwBGC0dy%2BBk7xm0tGtAamI%2BwLqfoCGVcJkkKaKETev51es5eN%2FKru77nv6qo0vYJqPqcjDEbrjgZzBQyYEa7fW%2B4yoKjL8XX4YhyTrnfEvA0j5s3AA6kW1T4AZE3Yi%2FVv6LLGP50cxE85bRoA65WbUxNEZU7vZs9CQ%2F5sRwCtu0zf%2Fr9rsFNMmOfimubBYycvPrmme7GcSX7ZUA%2FOXchLY6Mh%2BO8y%2BvyxNt2TsoZLA3qfTXhxl29CbA7ek0NfPmXEYmG6MeldvRaFMsP3TE8pPXuZc9eDrwEXpR05A8DIbFj%2Bu%2FpBZDKQtvS3a5Beol%2Faaij0dz0H2HKHP237EDLfkIwLkP%2F6%2BqGzJTB2mo2RnVFPXIaLzDKeg7tCxJwQlYfI%2FNJ5NGfkCsatX0fF5e57jHfrPBuREAj2Xfiktqq%2BrCDZmQGoMhnCO8iilllNwJ2tZTIOoB%2FJ5dcePDdVOy8pTIcXRvqc1eUfiDumzfr5xA8CZDEsrWCQBP5zk7rhxWgBZY3MtHopiuehIEYXRhjCfxPrWBjqkAQ1v5%2FA2xp2QzKTuMDi%2BnUlxhjSQiOlZrCunBbo0dyE%2B%2BDGUWX9QcV3sE8u6giKbyV2HC0TNrkCgq%2BKfYwT%2F%2BlpYkHke3FmoodwmUtLM%2Fuw0Mkj6G1tTFCMqhxNTZsrHOgd4q%2FQGZj%2BNHrFkT3568s5Mi%2B7ASVGv5%2BdePwX9e48O0zbc6fCi7mjZyZlC5WA6YSJ8VuU8yAthPvWFmqJ%2Bg%2BHvAYiz&X-Amz-Signature=9ea10f0bafff1fc2fc06c6fa48fe9c906d17ce0a37d8620d73b45544b01066c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7X5TSMN%2F20261026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261026T021615Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDVK%2Bkcn9KMwzN627tHE0WrN5fOH1v7ewv%2B9iuAMGLN6QIhAJdSVBH7hOlihpAldBuHTNVPvPdu43qxGYN6GAHPnkBeKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzwalwEenDBtGU5ivMq3ANRqVBUck5UhtmQURfLjGZfNCF38GmquvTyIm0W7ufzpr62Ew%2Bi1zFBvbxLPoi7u0jVbGzHA3gbNcfLW5p%2FlxoveqMSwvr8tT%2Bppd2K3ahwBGC0dy%2BBk7xm0tGtAamI%2BwLqfoCGVcJkkKaKETev51es5eN%2FKru77nv6qo0vYJqPqcjDEbrjgZzBQyYEa7fW%2B4yoKjL8XX4YhyTrnfEvA0j5s3AA6kW1T4AZE3Yi%2FVv6LLGP50cxE85bRoA65WbUxNEZU7vZs9CQ%2F5sRwCtu0zf%2Fr9rsFNMmOfimubBYycvPrmme7GcSX7ZUA%2FOXchLY6Mh%2BO8y%2BvyxNt2TsoZLA3qfTXhxl29CbA7ek0NfPmXEYmG6MeldvRaFMsP3TE8pPXuZc9eDrwEXpR05A8DIbFj%2Bu%2FpBZDKQtvS3a5Beol%2Faaij0dz0H2HKHP237EDLfkIwLkP%2F6%2BqGzJTB2mo2RnVFPXIaLzDKeg7tCxJwQlYfI%2FNJ5NGfkCsatX0fF5e57jHfrPBuREAj2Xfiktqq%2BrCDZmQGoMhnCO8iilllNwJ2tZTIOoB%2FJ5dcePDdVOy8pTIcXRvqc1eUfiDumzfr5xA8CZDEsrWCQBP5zk7rhxWgBZY3MtHopiuehIEYXRhjCfxPrWBjqkAQ1v5%2FA2xp2QzKTuMDi%2BnUlxhjSQiOlZrCunBbo0dyE%2B%2BDGUWX9QcV3sE8u6giKbyV2HC0TNrkCgq%2BKfYwT%2F%2BlpYkHke3FmoodwmUtLM%2Fuw0Mkj6G1tTFCMqhxNTZsrHOgd4q%2FQGZj%2BNHrFkT3568s5Mi%2B7ASVGv5%2BdePwX9e48O0zbc6fCi7mjZyZlC5WA6YSJ8VuU8yAthPvWFmqJ%2Bg%2BHvAYiz&X-Amz-Signature=79507c2372890a5fe35734c625954863f044b08bf5a714eebf9f53b9055d4695&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7X5TSMN%2F20261026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261026T021615Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDVK%2Bkcn9KMwzN627tHE0WrN5fOH1v7ewv%2B9iuAMGLN6QIhAJdSVBH7hOlihpAldBuHTNVPvPdu43qxGYN6GAHPnkBeKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzwalwEenDBtGU5ivMq3ANRqVBUck5UhtmQURfLjGZfNCF38GmquvTyIm0W7ufzpr62Ew%2Bi1zFBvbxLPoi7u0jVbGzHA3gbNcfLW5p%2FlxoveqMSwvr8tT%2Bppd2K3ahwBGC0dy%2BBk7xm0tGtAamI%2BwLqfoCGVcJkkKaKETev51es5eN%2FKru77nv6qo0vYJqPqcjDEbrjgZzBQyYEa7fW%2B4yoKjL8XX4YhyTrnfEvA0j5s3AA6kW1T4AZE3Yi%2FVv6LLGP50cxE85bRoA65WbUxNEZU7vZs9CQ%2F5sRwCtu0zf%2Fr9rsFNMmOfimubBYycvPrmme7GcSX7ZUA%2FOXchLY6Mh%2BO8y%2BvyxNt2TsoZLA3qfTXhxl29CbA7ek0NfPmXEYmG6MeldvRaFMsP3TE8pPXuZc9eDrwEXpR05A8DIbFj%2Bu%2FpBZDKQtvS3a5Beol%2Faaij0dz0H2HKHP237EDLfkIwLkP%2F6%2BqGzJTB2mo2RnVFPXIaLzDKeg7tCxJwQlYfI%2FNJ5NGfkCsatX0fF5e57jHfrPBuREAj2Xfiktqq%2BrCDZmQGoMhnCO8iilllNwJ2tZTIOoB%2FJ5dcePDdVOy8pTIcXRvqc1eUfiDumzfr5xA8CZDEsrWCQBP5zk7rhxWgBZY3MtHopiuehIEYXRhjCfxPrWBjqkAQ1v5%2FA2xp2QzKTuMDi%2BnUlxhjSQiOlZrCunBbo0dyE%2B%2BDGUWX9QcV3sE8u6giKbyV2HC0TNrkCgq%2BKfYwT%2F%2BlpYkHke3FmoodwmUtLM%2Fuw0Mkj6G1tTFCMqhxNTZsrHOgd4q%2FQGZj%2BNHrFkT3568s5Mi%2B7ASVGv5%2BdePwX9e48O0zbc6fCi7mjZyZlC5WA6YSJ8VuU8yAthPvWFmqJ%2Bg%2BHvAYiz&X-Amz-Signature=de59b6d477eae1939ef79c3db30210fc0c85501c768787bfbc728634887b8a34&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7X5TSMN%2F20261026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261026T021615Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDVK%2Bkcn9KMwzN627tHE0WrN5fOH1v7ewv%2B9iuAMGLN6QIhAJdSVBH7hOlihpAldBuHTNVPvPdu43qxGYN6GAHPnkBeKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzwalwEenDBtGU5ivMq3ANRqVBUck5UhtmQURfLjGZfNCF38GmquvTyIm0W7ufzpr62Ew%2Bi1zFBvbxLPoi7u0jVbGzHA3gbNcfLW5p%2FlxoveqMSwvr8tT%2Bppd2K3ahwBGC0dy%2BBk7xm0tGtAamI%2BwLqfoCGVcJkkKaKETev51es5eN%2FKru77nv6qo0vYJqPqcjDEbrjgZzBQyYEa7fW%2B4yoKjL8XX4YhyTrnfEvA0j5s3AA6kW1T4AZE3Yi%2FVv6LLGP50cxE85bRoA65WbUxNEZU7vZs9CQ%2F5sRwCtu0zf%2Fr9rsFNMmOfimubBYycvPrmme7GcSX7ZUA%2FOXchLY6Mh%2BO8y%2BvyxNt2TsoZLA3qfTXhxl29CbA7ek0NfPmXEYmG6MeldvRaFMsP3TE8pPXuZc9eDrwEXpR05A8DIbFj%2Bu%2FpBZDKQtvS3a5Beol%2Faaij0dz0H2HKHP237EDLfkIwLkP%2F6%2BqGzJTB2mo2RnVFPXIaLzDKeg7tCxJwQlYfI%2FNJ5NGfkCsatX0fF5e57jHfrPBuREAj2Xfiktqq%2BrCDZmQGoMhnCO8iilllNwJ2tZTIOoB%2FJ5dcePDdVOy8pTIcXRvqc1eUfiDumzfr5xA8CZDEsrWCQBP5zk7rhxWgBZY3MtHopiuehIEYXRhjCfxPrWBjqkAQ1v5%2FA2xp2QzKTuMDi%2BnUlxhjSQiOlZrCunBbo0dyE%2B%2BDGUWX9QcV3sE8u6giKbyV2HC0TNrkCgq%2BKfYwT%2F%2BlpYkHke3FmoodwmUtLM%2Fuw0Mkj6G1tTFCMqhxNTZsrHOgd4q%2FQGZj%2BNHrFkT3568s5Mi%2B7ASVGv5%2BdePwX9e48O0zbc6fCi7mjZyZlC5WA6YSJ8VuU8yAthPvWFmqJ%2Bg%2BHvAYiz&X-Amz-Signature=1ef5c87b812a4f1eb544dff015b693dd88c55a380945244594ef9b306669aa01&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7X5TSMN%2F20261026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261026T021615Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDVK%2Bkcn9KMwzN627tHE0WrN5fOH1v7ewv%2B9iuAMGLN6QIhAJdSVBH7hOlihpAldBuHTNVPvPdu43qxGYN6GAHPnkBeKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzwalwEenDBtGU5ivMq3ANRqVBUck5UhtmQURfLjGZfNCF38GmquvTyIm0W7ufzpr62Ew%2Bi1zFBvbxLPoi7u0jVbGzHA3gbNcfLW5p%2FlxoveqMSwvr8tT%2Bppd2K3ahwBGC0dy%2BBk7xm0tGtAamI%2BwLqfoCGVcJkkKaKETev51es5eN%2FKru77nv6qo0vYJqPqcjDEbrjgZzBQyYEa7fW%2B4yoKjL8XX4YhyTrnfEvA0j5s3AA6kW1T4AZE3Yi%2FVv6LLGP50cxE85bRoA65WbUxNEZU7vZs9CQ%2F5sRwCtu0zf%2Fr9rsFNMmOfimubBYycvPrmme7GcSX7ZUA%2FOXchLY6Mh%2BO8y%2BvyxNt2TsoZLA3qfTXhxl29CbA7ek0NfPmXEYmG6MeldvRaFMsP3TE8pPXuZc9eDrwEXpR05A8DIbFj%2Bu%2FpBZDKQtvS3a5Beol%2Faaij0dz0H2HKHP237EDLfkIwLkP%2F6%2BqGzJTB2mo2RnVFPXIaLzDKeg7tCxJwQlYfI%2FNJ5NGfkCsatX0fF5e57jHfrPBuREAj2Xfiktqq%2BrCDZmQGoMhnCO8iilllNwJ2tZTIOoB%2FJ5dcePDdVOy8pTIcXRvqc1eUfiDumzfr5xA8CZDEsrWCQBP5zk7rhxWgBZY3MtHopiuehIEYXRhjCfxPrWBjqkAQ1v5%2FA2xp2QzKTuMDi%2BnUlxhjSQiOlZrCunBbo0dyE%2B%2BDGUWX9QcV3sE8u6giKbyV2HC0TNrkCgq%2BKfYwT%2F%2BlpYkHke3FmoodwmUtLM%2Fuw0Mkj6G1tTFCMqhxNTZsrHOgd4q%2FQGZj%2BNHrFkT3568s5Mi%2B7ASVGv5%2BdePwX9e48O0zbc6fCi7mjZyZlC5WA6YSJ8VuU8yAthPvWFmqJ%2Bg%2BHvAYiz&X-Amz-Signature=bf6aa4371908f18ce84e7f03833547307a486e0b194fad05a39e96a747acd6e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7X5TSMN%2F20261026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261026T021615Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDVK%2Bkcn9KMwzN627tHE0WrN5fOH1v7ewv%2B9iuAMGLN6QIhAJdSVBH7hOlihpAldBuHTNVPvPdu43qxGYN6GAHPnkBeKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzwalwEenDBtGU5ivMq3ANRqVBUck5UhtmQURfLjGZfNCF38GmquvTyIm0W7ufzpr62Ew%2Bi1zFBvbxLPoi7u0jVbGzHA3gbNcfLW5p%2FlxoveqMSwvr8tT%2Bppd2K3ahwBGC0dy%2BBk7xm0tGtAamI%2BwLqfoCGVcJkkKaKETev51es5eN%2FKru77nv6qo0vYJqPqcjDEbrjgZzBQyYEa7fW%2B4yoKjL8XX4YhyTrnfEvA0j5s3AA6kW1T4AZE3Yi%2FVv6LLGP50cxE85bRoA65WbUxNEZU7vZs9CQ%2F5sRwCtu0zf%2Fr9rsFNMmOfimubBYycvPrmme7GcSX7ZUA%2FOXchLY6Mh%2BO8y%2BvyxNt2TsoZLA3qfTXhxl29CbA7ek0NfPmXEYmG6MeldvRaFMsP3TE8pPXuZc9eDrwEXpR05A8DIbFj%2Bu%2FpBZDKQtvS3a5Beol%2Faaij0dz0H2HKHP237EDLfkIwLkP%2F6%2BqGzJTB2mo2RnVFPXIaLzDKeg7tCxJwQlYfI%2FNJ5NGfkCsatX0fF5e57jHfrPBuREAj2Xfiktqq%2BrCDZmQGoMhnCO8iilllNwJ2tZTIOoB%2FJ5dcePDdVOy8pTIcXRvqc1eUfiDumzfr5xA8CZDEsrWCQBP5zk7rhxWgBZY3MtHopiuehIEYXRhjCfxPrWBjqkAQ1v5%2FA2xp2QzKTuMDi%2BnUlxhjSQiOlZrCunBbo0dyE%2B%2BDGUWX9QcV3sE8u6giKbyV2HC0TNrkCgq%2BKfYwT%2F%2BlpYkHke3FmoodwmUtLM%2Fuw0Mkj6G1tTFCMqhxNTZsrHOgd4q%2FQGZj%2BNHrFkT3568s5Mi%2B7ASVGv5%2BdePwX9e48O0zbc6fCi7mjZyZlC5WA6YSJ8VuU8yAthPvWFmqJ%2Bg%2BHvAYiz&X-Amz-Signature=c36a678df11724279d06662beb7507c3da3dd60d34ddfcdef0b7e38cd9826344&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
