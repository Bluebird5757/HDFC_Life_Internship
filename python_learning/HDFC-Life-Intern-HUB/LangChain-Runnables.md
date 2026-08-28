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
fetched_at: '2026-08-28T07:51:43.959Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5W2KKHT%2F20260828%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260828T075139Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQC%2FqisyeP7t3z72yrgEA6H3sPh4tr42yyzRyEFXXkihxgIgOwZ9khhiFR3UpoF%2B11%2FVxWgkqLsa74D8vh7zLW%2BvY6Uq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDLrAMAn0%2BKmoE0RLbircAwFwsxqKgjL8H6kkFGIi%2FJqTx6lwIbrx7NngUljNu5pVgzpzDgzy2EUamunSLCbXALqcJT3KGIEaMQgcSe3V6RAiMHS58%2FmGvagVeWqFDYnNI%2FnL0idoiAkAe2OZvodTzOKBqa%2BgPqoleNQ8rYuMn77euf7rH0gOhtkEX1IeYHfMgo%2FI%2FUmR%2FKSCS8lcco6Xdjyv8RpAoFnxZy3lhseh1Hn15IPDIZ40VGRd%2BWzE9v4XyzpQ%2Fbj3ojUY6YP7rtWEmRC3AMeXbjRUEZl5PlL8Iz5qGX%2Fpml6SDm6PmcpPRPPKeUlD8AnmXlu%2F14m7gD2DbaSrQPipgHvU2S9OElRz2PsZZoBrX9fAI82FAbw4VpqxZ3XbvWVdCFKC7cNEIqXk8UtxX%2BciZC3HafUbUJNjJsKuiDbekhFT6jDXYGZVI7XUoThT16sjl1iKPJRjux02y2qXHXN%2FJFhJooculjwmjkqphoYoaf3wYxIFOwxE2r3OPg%2FdZbpK4WSqDhleZi%2FxCEo8irJrOr%2Fwbxd5FA7%2F4UM%2FWGp4oXnILg0dWo6DdJ3FGv30tSXKTx%2FihzCgNTYC7aiG1l80FIAfzxP%2BYT7ZoDifKJkfHud6bYmHU3TFBS1cj%2FW4kAZeEH8gIzbpMMfmxNQGOqUB64L00xog0NrpVvbz%2FjAoZo6i1YqywQqoF40kZt2GzGY9c4qhHrI0%2Fnao59ygtJSqGrYeUA2FGCC1an3UQSO5Ol3%2FvyHimmm3HuMn6seu4vi9Cg1XU5BwnWzdaQTh4mUNsx6muhHYB0was5fZ6PaDi7lNzeMnHB34aGfJTSnC%2BGYKKGH6lafqfVXmIKEWy17j%2BPXhk8n0JwyH31%2B%2F7uZQJsCuId2d&X-Amz-Signature=8e1f965eba18366e9f9d2a333196330883d0aa08fa670d9b3d976d5d63e5f962&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5W2KKHT%2F20260828%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260828T075139Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQC%2FqisyeP7t3z72yrgEA6H3sPh4tr42yyzRyEFXXkihxgIgOwZ9khhiFR3UpoF%2B11%2FVxWgkqLsa74D8vh7zLW%2BvY6Uq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDLrAMAn0%2BKmoE0RLbircAwFwsxqKgjL8H6kkFGIi%2FJqTx6lwIbrx7NngUljNu5pVgzpzDgzy2EUamunSLCbXALqcJT3KGIEaMQgcSe3V6RAiMHS58%2FmGvagVeWqFDYnNI%2FnL0idoiAkAe2OZvodTzOKBqa%2BgPqoleNQ8rYuMn77euf7rH0gOhtkEX1IeYHfMgo%2FI%2FUmR%2FKSCS8lcco6Xdjyv8RpAoFnxZy3lhseh1Hn15IPDIZ40VGRd%2BWzE9v4XyzpQ%2Fbj3ojUY6YP7rtWEmRC3AMeXbjRUEZl5PlL8Iz5qGX%2Fpml6SDm6PmcpPRPPKeUlD8AnmXlu%2F14m7gD2DbaSrQPipgHvU2S9OElRz2PsZZoBrX9fAI82FAbw4VpqxZ3XbvWVdCFKC7cNEIqXk8UtxX%2BciZC3HafUbUJNjJsKuiDbekhFT6jDXYGZVI7XUoThT16sjl1iKPJRjux02y2qXHXN%2FJFhJooculjwmjkqphoYoaf3wYxIFOwxE2r3OPg%2FdZbpK4WSqDhleZi%2FxCEo8irJrOr%2Fwbxd5FA7%2F4UM%2FWGp4oXnILg0dWo6DdJ3FGv30tSXKTx%2FihzCgNTYC7aiG1l80FIAfzxP%2BYT7ZoDifKJkfHud6bYmHU3TFBS1cj%2FW4kAZeEH8gIzbpMMfmxNQGOqUB64L00xog0NrpVvbz%2FjAoZo6i1YqywQqoF40kZt2GzGY9c4qhHrI0%2Fnao59ygtJSqGrYeUA2FGCC1an3UQSO5Ol3%2FvyHimmm3HuMn6seu4vi9Cg1XU5BwnWzdaQTh4mUNsx6muhHYB0was5fZ6PaDi7lNzeMnHB34aGfJTSnC%2BGYKKGH6lafqfVXmIKEWy17j%2BPXhk8n0JwyH31%2B%2F7uZQJsCuId2d&X-Amz-Signature=38c8d46eede5847ffd128a54a7f794e1031a5d04ff7f6bbb7949c0f07ffc4b19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5W2KKHT%2F20260828%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260828T075139Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQC%2FqisyeP7t3z72yrgEA6H3sPh4tr42yyzRyEFXXkihxgIgOwZ9khhiFR3UpoF%2B11%2FVxWgkqLsa74D8vh7zLW%2BvY6Uq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDLrAMAn0%2BKmoE0RLbircAwFwsxqKgjL8H6kkFGIi%2FJqTx6lwIbrx7NngUljNu5pVgzpzDgzy2EUamunSLCbXALqcJT3KGIEaMQgcSe3V6RAiMHS58%2FmGvagVeWqFDYnNI%2FnL0idoiAkAe2OZvodTzOKBqa%2BgPqoleNQ8rYuMn77euf7rH0gOhtkEX1IeYHfMgo%2FI%2FUmR%2FKSCS8lcco6Xdjyv8RpAoFnxZy3lhseh1Hn15IPDIZ40VGRd%2BWzE9v4XyzpQ%2Fbj3ojUY6YP7rtWEmRC3AMeXbjRUEZl5PlL8Iz5qGX%2Fpml6SDm6PmcpPRPPKeUlD8AnmXlu%2F14m7gD2DbaSrQPipgHvU2S9OElRz2PsZZoBrX9fAI82FAbw4VpqxZ3XbvWVdCFKC7cNEIqXk8UtxX%2BciZC3HafUbUJNjJsKuiDbekhFT6jDXYGZVI7XUoThT16sjl1iKPJRjux02y2qXHXN%2FJFhJooculjwmjkqphoYoaf3wYxIFOwxE2r3OPg%2FdZbpK4WSqDhleZi%2FxCEo8irJrOr%2Fwbxd5FA7%2F4UM%2FWGp4oXnILg0dWo6DdJ3FGv30tSXKTx%2FihzCgNTYC7aiG1l80FIAfzxP%2BYT7ZoDifKJkfHud6bYmHU3TFBS1cj%2FW4kAZeEH8gIzbpMMfmxNQGOqUB64L00xog0NrpVvbz%2FjAoZo6i1YqywQqoF40kZt2GzGY9c4qhHrI0%2Fnao59ygtJSqGrYeUA2FGCC1an3UQSO5Ol3%2FvyHimmm3HuMn6seu4vi9Cg1XU5BwnWzdaQTh4mUNsx6muhHYB0was5fZ6PaDi7lNzeMnHB34aGfJTSnC%2BGYKKGH6lafqfVXmIKEWy17j%2BPXhk8n0JwyH31%2B%2F7uZQJsCuId2d&X-Amz-Signature=495bed218faebc2a8d0c1e43921ab903a82a4bc2c45c6090a2f29cc0c9cabd8b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5W2KKHT%2F20260828%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260828T075139Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQC%2FqisyeP7t3z72yrgEA6H3sPh4tr42yyzRyEFXXkihxgIgOwZ9khhiFR3UpoF%2B11%2FVxWgkqLsa74D8vh7zLW%2BvY6Uq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDLrAMAn0%2BKmoE0RLbircAwFwsxqKgjL8H6kkFGIi%2FJqTx6lwIbrx7NngUljNu5pVgzpzDgzy2EUamunSLCbXALqcJT3KGIEaMQgcSe3V6RAiMHS58%2FmGvagVeWqFDYnNI%2FnL0idoiAkAe2OZvodTzOKBqa%2BgPqoleNQ8rYuMn77euf7rH0gOhtkEX1IeYHfMgo%2FI%2FUmR%2FKSCS8lcco6Xdjyv8RpAoFnxZy3lhseh1Hn15IPDIZ40VGRd%2BWzE9v4XyzpQ%2Fbj3ojUY6YP7rtWEmRC3AMeXbjRUEZl5PlL8Iz5qGX%2Fpml6SDm6PmcpPRPPKeUlD8AnmXlu%2F14m7gD2DbaSrQPipgHvU2S9OElRz2PsZZoBrX9fAI82FAbw4VpqxZ3XbvWVdCFKC7cNEIqXk8UtxX%2BciZC3HafUbUJNjJsKuiDbekhFT6jDXYGZVI7XUoThT16sjl1iKPJRjux02y2qXHXN%2FJFhJooculjwmjkqphoYoaf3wYxIFOwxE2r3OPg%2FdZbpK4WSqDhleZi%2FxCEo8irJrOr%2Fwbxd5FA7%2F4UM%2FWGp4oXnILg0dWo6DdJ3FGv30tSXKTx%2FihzCgNTYC7aiG1l80FIAfzxP%2BYT7ZoDifKJkfHud6bYmHU3TFBS1cj%2FW4kAZeEH8gIzbpMMfmxNQGOqUB64L00xog0NrpVvbz%2FjAoZo6i1YqywQqoF40kZt2GzGY9c4qhHrI0%2Fnao59ygtJSqGrYeUA2FGCC1an3UQSO5Ol3%2FvyHimmm3HuMn6seu4vi9Cg1XU5BwnWzdaQTh4mUNsx6muhHYB0was5fZ6PaDi7lNzeMnHB34aGfJTSnC%2BGYKKGH6lafqfVXmIKEWy17j%2BPXhk8n0JwyH31%2B%2F7uZQJsCuId2d&X-Amz-Signature=d0e62fd7c730c1b498d93a8eeb88225d811bd7a1e5f6a58be6fb7f842b93baf7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5W2KKHT%2F20260828%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260828T075139Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQC%2FqisyeP7t3z72yrgEA6H3sPh4tr42yyzRyEFXXkihxgIgOwZ9khhiFR3UpoF%2B11%2FVxWgkqLsa74D8vh7zLW%2BvY6Uq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDLrAMAn0%2BKmoE0RLbircAwFwsxqKgjL8H6kkFGIi%2FJqTx6lwIbrx7NngUljNu5pVgzpzDgzy2EUamunSLCbXALqcJT3KGIEaMQgcSe3V6RAiMHS58%2FmGvagVeWqFDYnNI%2FnL0idoiAkAe2OZvodTzOKBqa%2BgPqoleNQ8rYuMn77euf7rH0gOhtkEX1IeYHfMgo%2FI%2FUmR%2FKSCS8lcco6Xdjyv8RpAoFnxZy3lhseh1Hn15IPDIZ40VGRd%2BWzE9v4XyzpQ%2Fbj3ojUY6YP7rtWEmRC3AMeXbjRUEZl5PlL8Iz5qGX%2Fpml6SDm6PmcpPRPPKeUlD8AnmXlu%2F14m7gD2DbaSrQPipgHvU2S9OElRz2PsZZoBrX9fAI82FAbw4VpqxZ3XbvWVdCFKC7cNEIqXk8UtxX%2BciZC3HafUbUJNjJsKuiDbekhFT6jDXYGZVI7XUoThT16sjl1iKPJRjux02y2qXHXN%2FJFhJooculjwmjkqphoYoaf3wYxIFOwxE2r3OPg%2FdZbpK4WSqDhleZi%2FxCEo8irJrOr%2Fwbxd5FA7%2F4UM%2FWGp4oXnILg0dWo6DdJ3FGv30tSXKTx%2FihzCgNTYC7aiG1l80FIAfzxP%2BYT7ZoDifKJkfHud6bYmHU3TFBS1cj%2FW4kAZeEH8gIzbpMMfmxNQGOqUB64L00xog0NrpVvbz%2FjAoZo6i1YqywQqoF40kZt2GzGY9c4qhHrI0%2Fnao59ygtJSqGrYeUA2FGCC1an3UQSO5Ol3%2FvyHimmm3HuMn6seu4vi9Cg1XU5BwnWzdaQTh4mUNsx6muhHYB0was5fZ6PaDi7lNzeMnHB34aGfJTSnC%2BGYKKGH6lafqfVXmIKEWy17j%2BPXhk8n0JwyH31%2B%2F7uZQJsCuId2d&X-Amz-Signature=e510e0100d2416645f5df5a1f822acfb7128137e0cbd81961f8a66d84bd36b12&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5W2KKHT%2F20260828%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260828T075139Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQC%2FqisyeP7t3z72yrgEA6H3sPh4tr42yyzRyEFXXkihxgIgOwZ9khhiFR3UpoF%2B11%2FVxWgkqLsa74D8vh7zLW%2BvY6Uq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDLrAMAn0%2BKmoE0RLbircAwFwsxqKgjL8H6kkFGIi%2FJqTx6lwIbrx7NngUljNu5pVgzpzDgzy2EUamunSLCbXALqcJT3KGIEaMQgcSe3V6RAiMHS58%2FmGvagVeWqFDYnNI%2FnL0idoiAkAe2OZvodTzOKBqa%2BgPqoleNQ8rYuMn77euf7rH0gOhtkEX1IeYHfMgo%2FI%2FUmR%2FKSCS8lcco6Xdjyv8RpAoFnxZy3lhseh1Hn15IPDIZ40VGRd%2BWzE9v4XyzpQ%2Fbj3ojUY6YP7rtWEmRC3AMeXbjRUEZl5PlL8Iz5qGX%2Fpml6SDm6PmcpPRPPKeUlD8AnmXlu%2F14m7gD2DbaSrQPipgHvU2S9OElRz2PsZZoBrX9fAI82FAbw4VpqxZ3XbvWVdCFKC7cNEIqXk8UtxX%2BciZC3HafUbUJNjJsKuiDbekhFT6jDXYGZVI7XUoThT16sjl1iKPJRjux02y2qXHXN%2FJFhJooculjwmjkqphoYoaf3wYxIFOwxE2r3OPg%2FdZbpK4WSqDhleZi%2FxCEo8irJrOr%2Fwbxd5FA7%2F4UM%2FWGp4oXnILg0dWo6DdJ3FGv30tSXKTx%2FihzCgNTYC7aiG1l80FIAfzxP%2BYT7ZoDifKJkfHud6bYmHU3TFBS1cj%2FW4kAZeEH8gIzbpMMfmxNQGOqUB64L00xog0NrpVvbz%2FjAoZo6i1YqywQqoF40kZt2GzGY9c4qhHrI0%2Fnao59ygtJSqGrYeUA2FGCC1an3UQSO5Ol3%2FvyHimmm3HuMn6seu4vi9Cg1XU5BwnWzdaQTh4mUNsx6muhHYB0was5fZ6PaDi7lNzeMnHB34aGfJTSnC%2BGYKKGH6lafqfVXmIKEWy17j%2BPXhk8n0JwyH31%2B%2F7uZQJsCuId2d&X-Amz-Signature=fc85e3243f24a111d7dbc4e67a1e0dba1fbbb5d2b7605c4d2302643dbf56ee44&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5W2KKHT%2F20260828%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260828T075139Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQC%2FqisyeP7t3z72yrgEA6H3sPh4tr42yyzRyEFXXkihxgIgOwZ9khhiFR3UpoF%2B11%2FVxWgkqLsa74D8vh7zLW%2BvY6Uq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDLrAMAn0%2BKmoE0RLbircAwFwsxqKgjL8H6kkFGIi%2FJqTx6lwIbrx7NngUljNu5pVgzpzDgzy2EUamunSLCbXALqcJT3KGIEaMQgcSe3V6RAiMHS58%2FmGvagVeWqFDYnNI%2FnL0idoiAkAe2OZvodTzOKBqa%2BgPqoleNQ8rYuMn77euf7rH0gOhtkEX1IeYHfMgo%2FI%2FUmR%2FKSCS8lcco6Xdjyv8RpAoFnxZy3lhseh1Hn15IPDIZ40VGRd%2BWzE9v4XyzpQ%2Fbj3ojUY6YP7rtWEmRC3AMeXbjRUEZl5PlL8Iz5qGX%2Fpml6SDm6PmcpPRPPKeUlD8AnmXlu%2F14m7gD2DbaSrQPipgHvU2S9OElRz2PsZZoBrX9fAI82FAbw4VpqxZ3XbvWVdCFKC7cNEIqXk8UtxX%2BciZC3HafUbUJNjJsKuiDbekhFT6jDXYGZVI7XUoThT16sjl1iKPJRjux02y2qXHXN%2FJFhJooculjwmjkqphoYoaf3wYxIFOwxE2r3OPg%2FdZbpK4WSqDhleZi%2FxCEo8irJrOr%2Fwbxd5FA7%2F4UM%2FWGp4oXnILg0dWo6DdJ3FGv30tSXKTx%2FihzCgNTYC7aiG1l80FIAfzxP%2BYT7ZoDifKJkfHud6bYmHU3TFBS1cj%2FW4kAZeEH8gIzbpMMfmxNQGOqUB64L00xog0NrpVvbz%2FjAoZo6i1YqywQqoF40kZt2GzGY9c4qhHrI0%2Fnao59ygtJSqGrYeUA2FGCC1an3UQSO5Ol3%2FvyHimmm3HuMn6seu4vi9Cg1XU5BwnWzdaQTh4mUNsx6muhHYB0was5fZ6PaDi7lNzeMnHB34aGfJTSnC%2BGYKKGH6lafqfVXmIKEWy17j%2BPXhk8n0JwyH31%2B%2F7uZQJsCuId2d&X-Amz-Signature=4eea8918546512d0a8444c13585e30d1d65d1beca980ec41ad50dddf0cb4a7f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5W2KKHT%2F20260828%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260828T075139Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQC%2FqisyeP7t3z72yrgEA6H3sPh4tr42yyzRyEFXXkihxgIgOwZ9khhiFR3UpoF%2B11%2FVxWgkqLsa74D8vh7zLW%2BvY6Uq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDLrAMAn0%2BKmoE0RLbircAwFwsxqKgjL8H6kkFGIi%2FJqTx6lwIbrx7NngUljNu5pVgzpzDgzy2EUamunSLCbXALqcJT3KGIEaMQgcSe3V6RAiMHS58%2FmGvagVeWqFDYnNI%2FnL0idoiAkAe2OZvodTzOKBqa%2BgPqoleNQ8rYuMn77euf7rH0gOhtkEX1IeYHfMgo%2FI%2FUmR%2FKSCS8lcco6Xdjyv8RpAoFnxZy3lhseh1Hn15IPDIZ40VGRd%2BWzE9v4XyzpQ%2Fbj3ojUY6YP7rtWEmRC3AMeXbjRUEZl5PlL8Iz5qGX%2Fpml6SDm6PmcpPRPPKeUlD8AnmXlu%2F14m7gD2DbaSrQPipgHvU2S9OElRz2PsZZoBrX9fAI82FAbw4VpqxZ3XbvWVdCFKC7cNEIqXk8UtxX%2BciZC3HafUbUJNjJsKuiDbekhFT6jDXYGZVI7XUoThT16sjl1iKPJRjux02y2qXHXN%2FJFhJooculjwmjkqphoYoaf3wYxIFOwxE2r3OPg%2FdZbpK4WSqDhleZi%2FxCEo8irJrOr%2Fwbxd5FA7%2F4UM%2FWGp4oXnILg0dWo6DdJ3FGv30tSXKTx%2FihzCgNTYC7aiG1l80FIAfzxP%2BYT7ZoDifKJkfHud6bYmHU3TFBS1cj%2FW4kAZeEH8gIzbpMMfmxNQGOqUB64L00xog0NrpVvbz%2FjAoZo6i1YqywQqoF40kZt2GzGY9c4qhHrI0%2Fnao59ygtJSqGrYeUA2FGCC1an3UQSO5Ol3%2FvyHimmm3HuMn6seu4vi9Cg1XU5BwnWzdaQTh4mUNsx6muhHYB0was5fZ6PaDi7lNzeMnHB34aGfJTSnC%2BGYKKGH6lafqfVXmIKEWy17j%2BPXhk8n0JwyH31%2B%2F7uZQJsCuId2d&X-Amz-Signature=3f39d9b378647802afbbf0988e7b0181412e11aa2e7c894659f07fa558cb3d74&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5W2KKHT%2F20260828%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260828T075139Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQC%2FqisyeP7t3z72yrgEA6H3sPh4tr42yyzRyEFXXkihxgIgOwZ9khhiFR3UpoF%2B11%2FVxWgkqLsa74D8vh7zLW%2BvY6Uq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDLrAMAn0%2BKmoE0RLbircAwFwsxqKgjL8H6kkFGIi%2FJqTx6lwIbrx7NngUljNu5pVgzpzDgzy2EUamunSLCbXALqcJT3KGIEaMQgcSe3V6RAiMHS58%2FmGvagVeWqFDYnNI%2FnL0idoiAkAe2OZvodTzOKBqa%2BgPqoleNQ8rYuMn77euf7rH0gOhtkEX1IeYHfMgo%2FI%2FUmR%2FKSCS8lcco6Xdjyv8RpAoFnxZy3lhseh1Hn15IPDIZ40VGRd%2BWzE9v4XyzpQ%2Fbj3ojUY6YP7rtWEmRC3AMeXbjRUEZl5PlL8Iz5qGX%2Fpml6SDm6PmcpPRPPKeUlD8AnmXlu%2F14m7gD2DbaSrQPipgHvU2S9OElRz2PsZZoBrX9fAI82FAbw4VpqxZ3XbvWVdCFKC7cNEIqXk8UtxX%2BciZC3HafUbUJNjJsKuiDbekhFT6jDXYGZVI7XUoThT16sjl1iKPJRjux02y2qXHXN%2FJFhJooculjwmjkqphoYoaf3wYxIFOwxE2r3OPg%2FdZbpK4WSqDhleZi%2FxCEo8irJrOr%2Fwbxd5FA7%2F4UM%2FWGp4oXnILg0dWo6DdJ3FGv30tSXKTx%2FihzCgNTYC7aiG1l80FIAfzxP%2BYT7ZoDifKJkfHud6bYmHU3TFBS1cj%2FW4kAZeEH8gIzbpMMfmxNQGOqUB64L00xog0NrpVvbz%2FjAoZo6i1YqywQqoF40kZt2GzGY9c4qhHrI0%2Fnao59ygtJSqGrYeUA2FGCC1an3UQSO5Ol3%2FvyHimmm3HuMn6seu4vi9Cg1XU5BwnWzdaQTh4mUNsx6muhHYB0was5fZ6PaDi7lNzeMnHB34aGfJTSnC%2BGYKKGH6lafqfVXmIKEWy17j%2BPXhk8n0JwyH31%2B%2F7uZQJsCuId2d&X-Amz-Signature=b2fbe89510fb57e17148afd777a178438c3bca97b5e1f54054cad0891c9a4957&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
