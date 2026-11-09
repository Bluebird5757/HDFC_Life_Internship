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
fetched_at: '2026-11-09T03:38:48.177Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666MGQ7XP3%2F20261109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261109T033845Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIQC9bv2N%2B%2F72OUPUNKwKgns1fjtAa%2B%2BOo556u3k8%2F8SfzQIgctd3OvUyRJ17wth%2BNChGYFCGcVk3m7HGysIrYKsKVwgq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDE7VNOL6%2FJHAVn4bpSrcA6xQQqiDXW%2FECkXWKLUPKdGjWofxt%2BJC6oMM%2BAujFtqGO438OR8SjNADtf%2FzBI24V7UkeGUYo%2FyqTJ6sImJ2V0tLjM0hMQHXGoCoNVnq%2FaLX9tHMN0kkkQGZkPlfkpuAj5SgZvdF8veVsnGM3KT9V4ZU%2Bl%2B64tGE%2FDSA01UHlQzYxvV7NEWP8l4RvsXqRJRWZ6qmNa8UxYuIpQrGD0mr3O71YQFhMQbx1kby3rGhaeyUr2XER2xNURItHzHo0gZn8QVD4yQzmD68DbnKrBdjrAHIDN2H%2BNXGR3hdwyOLkX4UWyfAd5m%2Fvfbr%2BnN9qv9kokIFHHSR88wpla0wzqgjclowki9%2FC4ETIy9xKPcUVa3fVAZvq1jksqRiO58yIgv9zBiHLztNbdbvcDrJfNL9QMilUoB2%2Bz9GIh3BqZ34Q2tCRYE9DE7jwZ1I5uMP1q0qPgzQIbJV2TYBKN4r%2F9h%2FUpJzSBInwtMCwPVyobTjDbQsNAypLwqkIKV9bnb3Ui%2Bkc%2F2x2kZ5Www8k%2BcTosirWt8baq7g7%2BLPWo8ImAROnntOCtAZE8WQH5o1SEuVM%2FQZFtsAAZ6BTwr7KeaH6Vym9txzNp7cHqWPhGx8RSAy9C0UUJ1oK%2F4KymXPXMshMLv2xNcGOqUB00Bu6HKTE3AadY4i73p7d5IHHVaQ9anwrIsinN8axWCbTWnd1Re7jrvOr0JRKI2wixfOo0pk5%2FMeRFeUCYVv8jPb9mtclZxoC4nsnG8HXKJsNetx2S17KqPr%2Fz7NOCfpKutRgFD1lowv8kwJTwvlJtPsOB3cb8Wk10hkYMr6IlTYETYhQD9bDYZWyDXYiekVWTceYAqWu0%2FycH3EO7natBJRMFwB&X-Amz-Signature=4e0192c1c2fda99a72378fd33157203ffbe725fe56841d9590cac260760634c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666MGQ7XP3%2F20261109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261109T033845Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIQC9bv2N%2B%2F72OUPUNKwKgns1fjtAa%2B%2BOo556u3k8%2F8SfzQIgctd3OvUyRJ17wth%2BNChGYFCGcVk3m7HGysIrYKsKVwgq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDE7VNOL6%2FJHAVn4bpSrcA6xQQqiDXW%2FECkXWKLUPKdGjWofxt%2BJC6oMM%2BAujFtqGO438OR8SjNADtf%2FzBI24V7UkeGUYo%2FyqTJ6sImJ2V0tLjM0hMQHXGoCoNVnq%2FaLX9tHMN0kkkQGZkPlfkpuAj5SgZvdF8veVsnGM3KT9V4ZU%2Bl%2B64tGE%2FDSA01UHlQzYxvV7NEWP8l4RvsXqRJRWZ6qmNa8UxYuIpQrGD0mr3O71YQFhMQbx1kby3rGhaeyUr2XER2xNURItHzHo0gZn8QVD4yQzmD68DbnKrBdjrAHIDN2H%2BNXGR3hdwyOLkX4UWyfAd5m%2Fvfbr%2BnN9qv9kokIFHHSR88wpla0wzqgjclowki9%2FC4ETIy9xKPcUVa3fVAZvq1jksqRiO58yIgv9zBiHLztNbdbvcDrJfNL9QMilUoB2%2Bz9GIh3BqZ34Q2tCRYE9DE7jwZ1I5uMP1q0qPgzQIbJV2TYBKN4r%2F9h%2FUpJzSBInwtMCwPVyobTjDbQsNAypLwqkIKV9bnb3Ui%2Bkc%2F2x2kZ5Www8k%2BcTosirWt8baq7g7%2BLPWo8ImAROnntOCtAZE8WQH5o1SEuVM%2FQZFtsAAZ6BTwr7KeaH6Vym9txzNp7cHqWPhGx8RSAy9C0UUJ1oK%2F4KymXPXMshMLv2xNcGOqUB00Bu6HKTE3AadY4i73p7d5IHHVaQ9anwrIsinN8axWCbTWnd1Re7jrvOr0JRKI2wixfOo0pk5%2FMeRFeUCYVv8jPb9mtclZxoC4nsnG8HXKJsNetx2S17KqPr%2Fz7NOCfpKutRgFD1lowv8kwJTwvlJtPsOB3cb8Wk10hkYMr6IlTYETYhQD9bDYZWyDXYiekVWTceYAqWu0%2FycH3EO7natBJRMFwB&X-Amz-Signature=aa6d5ea41e474f7d7925996d0d43e502c77c0ebd2e4b574c8b2b1327b7e59ba5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666MGQ7XP3%2F20261109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261109T033845Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIQC9bv2N%2B%2F72OUPUNKwKgns1fjtAa%2B%2BOo556u3k8%2F8SfzQIgctd3OvUyRJ17wth%2BNChGYFCGcVk3m7HGysIrYKsKVwgq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDE7VNOL6%2FJHAVn4bpSrcA6xQQqiDXW%2FECkXWKLUPKdGjWofxt%2BJC6oMM%2BAujFtqGO438OR8SjNADtf%2FzBI24V7UkeGUYo%2FyqTJ6sImJ2V0tLjM0hMQHXGoCoNVnq%2FaLX9tHMN0kkkQGZkPlfkpuAj5SgZvdF8veVsnGM3KT9V4ZU%2Bl%2B64tGE%2FDSA01UHlQzYxvV7NEWP8l4RvsXqRJRWZ6qmNa8UxYuIpQrGD0mr3O71YQFhMQbx1kby3rGhaeyUr2XER2xNURItHzHo0gZn8QVD4yQzmD68DbnKrBdjrAHIDN2H%2BNXGR3hdwyOLkX4UWyfAd5m%2Fvfbr%2BnN9qv9kokIFHHSR88wpla0wzqgjclowki9%2FC4ETIy9xKPcUVa3fVAZvq1jksqRiO58yIgv9zBiHLztNbdbvcDrJfNL9QMilUoB2%2Bz9GIh3BqZ34Q2tCRYE9DE7jwZ1I5uMP1q0qPgzQIbJV2TYBKN4r%2F9h%2FUpJzSBInwtMCwPVyobTjDbQsNAypLwqkIKV9bnb3Ui%2Bkc%2F2x2kZ5Www8k%2BcTosirWt8baq7g7%2BLPWo8ImAROnntOCtAZE8WQH5o1SEuVM%2FQZFtsAAZ6BTwr7KeaH6Vym9txzNp7cHqWPhGx8RSAy9C0UUJ1oK%2F4KymXPXMshMLv2xNcGOqUB00Bu6HKTE3AadY4i73p7d5IHHVaQ9anwrIsinN8axWCbTWnd1Re7jrvOr0JRKI2wixfOo0pk5%2FMeRFeUCYVv8jPb9mtclZxoC4nsnG8HXKJsNetx2S17KqPr%2Fz7NOCfpKutRgFD1lowv8kwJTwvlJtPsOB3cb8Wk10hkYMr6IlTYETYhQD9bDYZWyDXYiekVWTceYAqWu0%2FycH3EO7natBJRMFwB&X-Amz-Signature=f290bfd7727e399b7ad045d44191b5a00a89a22785065323c9f86966150d417f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666MGQ7XP3%2F20261109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261109T033845Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIQC9bv2N%2B%2F72OUPUNKwKgns1fjtAa%2B%2BOo556u3k8%2F8SfzQIgctd3OvUyRJ17wth%2BNChGYFCGcVk3m7HGysIrYKsKVwgq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDE7VNOL6%2FJHAVn4bpSrcA6xQQqiDXW%2FECkXWKLUPKdGjWofxt%2BJC6oMM%2BAujFtqGO438OR8SjNADtf%2FzBI24V7UkeGUYo%2FyqTJ6sImJ2V0tLjM0hMQHXGoCoNVnq%2FaLX9tHMN0kkkQGZkPlfkpuAj5SgZvdF8veVsnGM3KT9V4ZU%2Bl%2B64tGE%2FDSA01UHlQzYxvV7NEWP8l4RvsXqRJRWZ6qmNa8UxYuIpQrGD0mr3O71YQFhMQbx1kby3rGhaeyUr2XER2xNURItHzHo0gZn8QVD4yQzmD68DbnKrBdjrAHIDN2H%2BNXGR3hdwyOLkX4UWyfAd5m%2Fvfbr%2BnN9qv9kokIFHHSR88wpla0wzqgjclowki9%2FC4ETIy9xKPcUVa3fVAZvq1jksqRiO58yIgv9zBiHLztNbdbvcDrJfNL9QMilUoB2%2Bz9GIh3BqZ34Q2tCRYE9DE7jwZ1I5uMP1q0qPgzQIbJV2TYBKN4r%2F9h%2FUpJzSBInwtMCwPVyobTjDbQsNAypLwqkIKV9bnb3Ui%2Bkc%2F2x2kZ5Www8k%2BcTosirWt8baq7g7%2BLPWo8ImAROnntOCtAZE8WQH5o1SEuVM%2FQZFtsAAZ6BTwr7KeaH6Vym9txzNp7cHqWPhGx8RSAy9C0UUJ1oK%2F4KymXPXMshMLv2xNcGOqUB00Bu6HKTE3AadY4i73p7d5IHHVaQ9anwrIsinN8axWCbTWnd1Re7jrvOr0JRKI2wixfOo0pk5%2FMeRFeUCYVv8jPb9mtclZxoC4nsnG8HXKJsNetx2S17KqPr%2Fz7NOCfpKutRgFD1lowv8kwJTwvlJtPsOB3cb8Wk10hkYMr6IlTYETYhQD9bDYZWyDXYiekVWTceYAqWu0%2FycH3EO7natBJRMFwB&X-Amz-Signature=91203630da4dca7fd2279ab1f216d0f380a3044658a62a80f9d2cbb3ac8c51c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666MGQ7XP3%2F20261109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261109T033845Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIQC9bv2N%2B%2F72OUPUNKwKgns1fjtAa%2B%2BOo556u3k8%2F8SfzQIgctd3OvUyRJ17wth%2BNChGYFCGcVk3m7HGysIrYKsKVwgq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDE7VNOL6%2FJHAVn4bpSrcA6xQQqiDXW%2FECkXWKLUPKdGjWofxt%2BJC6oMM%2BAujFtqGO438OR8SjNADtf%2FzBI24V7UkeGUYo%2FyqTJ6sImJ2V0tLjM0hMQHXGoCoNVnq%2FaLX9tHMN0kkkQGZkPlfkpuAj5SgZvdF8veVsnGM3KT9V4ZU%2Bl%2B64tGE%2FDSA01UHlQzYxvV7NEWP8l4RvsXqRJRWZ6qmNa8UxYuIpQrGD0mr3O71YQFhMQbx1kby3rGhaeyUr2XER2xNURItHzHo0gZn8QVD4yQzmD68DbnKrBdjrAHIDN2H%2BNXGR3hdwyOLkX4UWyfAd5m%2Fvfbr%2BnN9qv9kokIFHHSR88wpla0wzqgjclowki9%2FC4ETIy9xKPcUVa3fVAZvq1jksqRiO58yIgv9zBiHLztNbdbvcDrJfNL9QMilUoB2%2Bz9GIh3BqZ34Q2tCRYE9DE7jwZ1I5uMP1q0qPgzQIbJV2TYBKN4r%2F9h%2FUpJzSBInwtMCwPVyobTjDbQsNAypLwqkIKV9bnb3Ui%2Bkc%2F2x2kZ5Www8k%2BcTosirWt8baq7g7%2BLPWo8ImAROnntOCtAZE8WQH5o1SEuVM%2FQZFtsAAZ6BTwr7KeaH6Vym9txzNp7cHqWPhGx8RSAy9C0UUJ1oK%2F4KymXPXMshMLv2xNcGOqUB00Bu6HKTE3AadY4i73p7d5IHHVaQ9anwrIsinN8axWCbTWnd1Re7jrvOr0JRKI2wixfOo0pk5%2FMeRFeUCYVv8jPb9mtclZxoC4nsnG8HXKJsNetx2S17KqPr%2Fz7NOCfpKutRgFD1lowv8kwJTwvlJtPsOB3cb8Wk10hkYMr6IlTYETYhQD9bDYZWyDXYiekVWTceYAqWu0%2FycH3EO7natBJRMFwB&X-Amz-Signature=5a5f61d79009b25c447d924e4c52b160c9bb5ed169418e92f2584ae3fe1f549d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666MGQ7XP3%2F20261109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261109T033845Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIQC9bv2N%2B%2F72OUPUNKwKgns1fjtAa%2B%2BOo556u3k8%2F8SfzQIgctd3OvUyRJ17wth%2BNChGYFCGcVk3m7HGysIrYKsKVwgq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDE7VNOL6%2FJHAVn4bpSrcA6xQQqiDXW%2FECkXWKLUPKdGjWofxt%2BJC6oMM%2BAujFtqGO438OR8SjNADtf%2FzBI24V7UkeGUYo%2FyqTJ6sImJ2V0tLjM0hMQHXGoCoNVnq%2FaLX9tHMN0kkkQGZkPlfkpuAj5SgZvdF8veVsnGM3KT9V4ZU%2Bl%2B64tGE%2FDSA01UHlQzYxvV7NEWP8l4RvsXqRJRWZ6qmNa8UxYuIpQrGD0mr3O71YQFhMQbx1kby3rGhaeyUr2XER2xNURItHzHo0gZn8QVD4yQzmD68DbnKrBdjrAHIDN2H%2BNXGR3hdwyOLkX4UWyfAd5m%2Fvfbr%2BnN9qv9kokIFHHSR88wpla0wzqgjclowki9%2FC4ETIy9xKPcUVa3fVAZvq1jksqRiO58yIgv9zBiHLztNbdbvcDrJfNL9QMilUoB2%2Bz9GIh3BqZ34Q2tCRYE9DE7jwZ1I5uMP1q0qPgzQIbJV2TYBKN4r%2F9h%2FUpJzSBInwtMCwPVyobTjDbQsNAypLwqkIKV9bnb3Ui%2Bkc%2F2x2kZ5Www8k%2BcTosirWt8baq7g7%2BLPWo8ImAROnntOCtAZE8WQH5o1SEuVM%2FQZFtsAAZ6BTwr7KeaH6Vym9txzNp7cHqWPhGx8RSAy9C0UUJ1oK%2F4KymXPXMshMLv2xNcGOqUB00Bu6HKTE3AadY4i73p7d5IHHVaQ9anwrIsinN8axWCbTWnd1Re7jrvOr0JRKI2wixfOo0pk5%2FMeRFeUCYVv8jPb9mtclZxoC4nsnG8HXKJsNetx2S17KqPr%2Fz7NOCfpKutRgFD1lowv8kwJTwvlJtPsOB3cb8Wk10hkYMr6IlTYETYhQD9bDYZWyDXYiekVWTceYAqWu0%2FycH3EO7natBJRMFwB&X-Amz-Signature=b4861bc0a6b0fd4d73023e657be8d08b9e050604620771ce1b85177aa1b9e1f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666MGQ7XP3%2F20261109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261109T033845Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIQC9bv2N%2B%2F72OUPUNKwKgns1fjtAa%2B%2BOo556u3k8%2F8SfzQIgctd3OvUyRJ17wth%2BNChGYFCGcVk3m7HGysIrYKsKVwgq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDE7VNOL6%2FJHAVn4bpSrcA6xQQqiDXW%2FECkXWKLUPKdGjWofxt%2BJC6oMM%2BAujFtqGO438OR8SjNADtf%2FzBI24V7UkeGUYo%2FyqTJ6sImJ2V0tLjM0hMQHXGoCoNVnq%2FaLX9tHMN0kkkQGZkPlfkpuAj5SgZvdF8veVsnGM3KT9V4ZU%2Bl%2B64tGE%2FDSA01UHlQzYxvV7NEWP8l4RvsXqRJRWZ6qmNa8UxYuIpQrGD0mr3O71YQFhMQbx1kby3rGhaeyUr2XER2xNURItHzHo0gZn8QVD4yQzmD68DbnKrBdjrAHIDN2H%2BNXGR3hdwyOLkX4UWyfAd5m%2Fvfbr%2BnN9qv9kokIFHHSR88wpla0wzqgjclowki9%2FC4ETIy9xKPcUVa3fVAZvq1jksqRiO58yIgv9zBiHLztNbdbvcDrJfNL9QMilUoB2%2Bz9GIh3BqZ34Q2tCRYE9DE7jwZ1I5uMP1q0qPgzQIbJV2TYBKN4r%2F9h%2FUpJzSBInwtMCwPVyobTjDbQsNAypLwqkIKV9bnb3Ui%2Bkc%2F2x2kZ5Www8k%2BcTosirWt8baq7g7%2BLPWo8ImAROnntOCtAZE8WQH5o1SEuVM%2FQZFtsAAZ6BTwr7KeaH6Vym9txzNp7cHqWPhGx8RSAy9C0UUJ1oK%2F4KymXPXMshMLv2xNcGOqUB00Bu6HKTE3AadY4i73p7d5IHHVaQ9anwrIsinN8axWCbTWnd1Re7jrvOr0JRKI2wixfOo0pk5%2FMeRFeUCYVv8jPb9mtclZxoC4nsnG8HXKJsNetx2S17KqPr%2Fz7NOCfpKutRgFD1lowv8kwJTwvlJtPsOB3cb8Wk10hkYMr6IlTYETYhQD9bDYZWyDXYiekVWTceYAqWu0%2FycH3EO7natBJRMFwB&X-Amz-Signature=f4a9340c55003c6c90ca49df020a4034b7bea1d85d3cd76808f4bc09c80b8ef8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666MGQ7XP3%2F20261109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261109T033845Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIQC9bv2N%2B%2F72OUPUNKwKgns1fjtAa%2B%2BOo556u3k8%2F8SfzQIgctd3OvUyRJ17wth%2BNChGYFCGcVk3m7HGysIrYKsKVwgq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDE7VNOL6%2FJHAVn4bpSrcA6xQQqiDXW%2FECkXWKLUPKdGjWofxt%2BJC6oMM%2BAujFtqGO438OR8SjNADtf%2FzBI24V7UkeGUYo%2FyqTJ6sImJ2V0tLjM0hMQHXGoCoNVnq%2FaLX9tHMN0kkkQGZkPlfkpuAj5SgZvdF8veVsnGM3KT9V4ZU%2Bl%2B64tGE%2FDSA01UHlQzYxvV7NEWP8l4RvsXqRJRWZ6qmNa8UxYuIpQrGD0mr3O71YQFhMQbx1kby3rGhaeyUr2XER2xNURItHzHo0gZn8QVD4yQzmD68DbnKrBdjrAHIDN2H%2BNXGR3hdwyOLkX4UWyfAd5m%2Fvfbr%2BnN9qv9kokIFHHSR88wpla0wzqgjclowki9%2FC4ETIy9xKPcUVa3fVAZvq1jksqRiO58yIgv9zBiHLztNbdbvcDrJfNL9QMilUoB2%2Bz9GIh3BqZ34Q2tCRYE9DE7jwZ1I5uMP1q0qPgzQIbJV2TYBKN4r%2F9h%2FUpJzSBInwtMCwPVyobTjDbQsNAypLwqkIKV9bnb3Ui%2Bkc%2F2x2kZ5Www8k%2BcTosirWt8baq7g7%2BLPWo8ImAROnntOCtAZE8WQH5o1SEuVM%2FQZFtsAAZ6BTwr7KeaH6Vym9txzNp7cHqWPhGx8RSAy9C0UUJ1oK%2F4KymXPXMshMLv2xNcGOqUB00Bu6HKTE3AadY4i73p7d5IHHVaQ9anwrIsinN8axWCbTWnd1Re7jrvOr0JRKI2wixfOo0pk5%2FMeRFeUCYVv8jPb9mtclZxoC4nsnG8HXKJsNetx2S17KqPr%2Fz7NOCfpKutRgFD1lowv8kwJTwvlJtPsOB3cb8Wk10hkYMr6IlTYETYhQD9bDYZWyDXYiekVWTceYAqWu0%2FycH3EO7natBJRMFwB&X-Amz-Signature=4480d493142f1c3e287608040a8353c78ea10674ed660541555bb481497d3649&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666MGQ7XP3%2F20261109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261109T033845Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIQC9bv2N%2B%2F72OUPUNKwKgns1fjtAa%2B%2BOo556u3k8%2F8SfzQIgctd3OvUyRJ17wth%2BNChGYFCGcVk3m7HGysIrYKsKVwgq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDE7VNOL6%2FJHAVn4bpSrcA6xQQqiDXW%2FECkXWKLUPKdGjWofxt%2BJC6oMM%2BAujFtqGO438OR8SjNADtf%2FzBI24V7UkeGUYo%2FyqTJ6sImJ2V0tLjM0hMQHXGoCoNVnq%2FaLX9tHMN0kkkQGZkPlfkpuAj5SgZvdF8veVsnGM3KT9V4ZU%2Bl%2B64tGE%2FDSA01UHlQzYxvV7NEWP8l4RvsXqRJRWZ6qmNa8UxYuIpQrGD0mr3O71YQFhMQbx1kby3rGhaeyUr2XER2xNURItHzHo0gZn8QVD4yQzmD68DbnKrBdjrAHIDN2H%2BNXGR3hdwyOLkX4UWyfAd5m%2Fvfbr%2BnN9qv9kokIFHHSR88wpla0wzqgjclowki9%2FC4ETIy9xKPcUVa3fVAZvq1jksqRiO58yIgv9zBiHLztNbdbvcDrJfNL9QMilUoB2%2Bz9GIh3BqZ34Q2tCRYE9DE7jwZ1I5uMP1q0qPgzQIbJV2TYBKN4r%2F9h%2FUpJzSBInwtMCwPVyobTjDbQsNAypLwqkIKV9bnb3Ui%2Bkc%2F2x2kZ5Www8k%2BcTosirWt8baq7g7%2BLPWo8ImAROnntOCtAZE8WQH5o1SEuVM%2FQZFtsAAZ6BTwr7KeaH6Vym9txzNp7cHqWPhGx8RSAy9C0UUJ1oK%2F4KymXPXMshMLv2xNcGOqUB00Bu6HKTE3AadY4i73p7d5IHHVaQ9anwrIsinN8axWCbTWnd1Re7jrvOr0JRKI2wixfOo0pk5%2FMeRFeUCYVv8jPb9mtclZxoC4nsnG8HXKJsNetx2S17KqPr%2Fz7NOCfpKutRgFD1lowv8kwJTwvlJtPsOB3cb8Wk10hkYMr6IlTYETYhQD9bDYZWyDXYiekVWTceYAqWu0%2FycH3EO7natBJRMFwB&X-Amz-Signature=664cb807f64addd4e2f45910731a9aee306e66edc8fce83f31848712542d5f60&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
