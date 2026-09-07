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
fetched_at: '2026-09-07T01:50:19.801Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FMCCQBR%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015014Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIAthXLvCHt9nwssINsUQh%2BY7NZv8Ttl0Rw63Gd3phxurAiEAlVNlQ6zKdR%2BOBHRnsK2QNv3d9zb7aa8WMOCWMeHHpvgq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDKeNfv8%2Bx%2BeXGOznhircAz0tBsa%2FXvlm8WTpZgOLgKtUHZehFv1GIFzsGpvD1XYfSNUC1DEP7ZVi3eByZkg%2B8bp3qMcLm4Q1Q4Mr5YjM7FvntOfAHSvvHXFG9MJIXVgufv%2Bb1P5vNNX7QTPgCYlLvoL4XkM2xacwXX5YmtY%2Fpwo9iCOJuyg%2BvpdSgKPNt108FNqcWxC5cpc2UmVwUxs45Utm5cdzvpcySYaPIBGt%2FxyPBTAKgFvgQzdjS%2FXYdmYKIr70udMxljd%2B%2Bb%2BI4YPba%2FOkkh9wA%2Ft3ySDxuJpVfazuaHZL9sJ9acxM8ma%2FT0XZJoDdF3GBnENE1jlJ%2Bxw8hqIjkDT3grosH8h2kxDO7s070ksNqB6B9WvnJJ0G4iv2D7dRD7O9HOASd%2F9Q%2F7m%2BNoiSFPmQzPygdDtoorOebaUPVoIPIc%2FhWNZS8E7ld3hCDDqvaINIjsPflz7dTjq%2BavG9pxSaqxzqN4saOmvdFwp3UWHrqhv8BUo0iyJQlOVccjMqENSJcDPnNA8PO7X3hJ310iQJo8bb6OMRYqkL5qYW0jYdL9qSVVDQDAQNL03faMknt2qb%2B9UAJF6z6Rnpk1jEkqcwjk7GMTOrkvCst3syATIFkHpFlhXe0qvwyZGfVMyoidSqCycrZHbuMK2s%2BNQGOqUBbWYcLu2tptjKLEL7fjSZA9lZIZ0GbXjVCY5zqQfXWX96h74jie5NKJT7c7kB%2BICyMin2xuE%2BD%2BO5zuGh8exP6xmwvEnUNH1URi7odcniF9efHS8hzb2Vr9eybkmntOY0cO7fTQKsE874hrT6FVP4v0m6nIbEuzdtgH9LTYZw%2BIHtyWU2UonV9c4jHyKMJScRtrbnXAUmgqSYbINBHFgvdWPiMglE&X-Amz-Signature=e1094a23b3715167059d4ea873d12fe2f62057199253520e40ba7dc0680b28ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FMCCQBR%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015014Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIAthXLvCHt9nwssINsUQh%2BY7NZv8Ttl0Rw63Gd3phxurAiEAlVNlQ6zKdR%2BOBHRnsK2QNv3d9zb7aa8WMOCWMeHHpvgq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDKeNfv8%2Bx%2BeXGOznhircAz0tBsa%2FXvlm8WTpZgOLgKtUHZehFv1GIFzsGpvD1XYfSNUC1DEP7ZVi3eByZkg%2B8bp3qMcLm4Q1Q4Mr5YjM7FvntOfAHSvvHXFG9MJIXVgufv%2Bb1P5vNNX7QTPgCYlLvoL4XkM2xacwXX5YmtY%2Fpwo9iCOJuyg%2BvpdSgKPNt108FNqcWxC5cpc2UmVwUxs45Utm5cdzvpcySYaPIBGt%2FxyPBTAKgFvgQzdjS%2FXYdmYKIr70udMxljd%2B%2Bb%2BI4YPba%2FOkkh9wA%2Ft3ySDxuJpVfazuaHZL9sJ9acxM8ma%2FT0XZJoDdF3GBnENE1jlJ%2Bxw8hqIjkDT3grosH8h2kxDO7s070ksNqB6B9WvnJJ0G4iv2D7dRD7O9HOASd%2F9Q%2F7m%2BNoiSFPmQzPygdDtoorOebaUPVoIPIc%2FhWNZS8E7ld3hCDDqvaINIjsPflz7dTjq%2BavG9pxSaqxzqN4saOmvdFwp3UWHrqhv8BUo0iyJQlOVccjMqENSJcDPnNA8PO7X3hJ310iQJo8bb6OMRYqkL5qYW0jYdL9qSVVDQDAQNL03faMknt2qb%2B9UAJF6z6Rnpk1jEkqcwjk7GMTOrkvCst3syATIFkHpFlhXe0qvwyZGfVMyoidSqCycrZHbuMK2s%2BNQGOqUBbWYcLu2tptjKLEL7fjSZA9lZIZ0GbXjVCY5zqQfXWX96h74jie5NKJT7c7kB%2BICyMin2xuE%2BD%2BO5zuGh8exP6xmwvEnUNH1URi7odcniF9efHS8hzb2Vr9eybkmntOY0cO7fTQKsE874hrT6FVP4v0m6nIbEuzdtgH9LTYZw%2BIHtyWU2UonV9c4jHyKMJScRtrbnXAUmgqSYbINBHFgvdWPiMglE&X-Amz-Signature=d813d6b0ef151fd2047b320da8b7457780874fb25c92050601612ee06cdef872&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FMCCQBR%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015014Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIAthXLvCHt9nwssINsUQh%2BY7NZv8Ttl0Rw63Gd3phxurAiEAlVNlQ6zKdR%2BOBHRnsK2QNv3d9zb7aa8WMOCWMeHHpvgq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDKeNfv8%2Bx%2BeXGOznhircAz0tBsa%2FXvlm8WTpZgOLgKtUHZehFv1GIFzsGpvD1XYfSNUC1DEP7ZVi3eByZkg%2B8bp3qMcLm4Q1Q4Mr5YjM7FvntOfAHSvvHXFG9MJIXVgufv%2Bb1P5vNNX7QTPgCYlLvoL4XkM2xacwXX5YmtY%2Fpwo9iCOJuyg%2BvpdSgKPNt108FNqcWxC5cpc2UmVwUxs45Utm5cdzvpcySYaPIBGt%2FxyPBTAKgFvgQzdjS%2FXYdmYKIr70udMxljd%2B%2Bb%2BI4YPba%2FOkkh9wA%2Ft3ySDxuJpVfazuaHZL9sJ9acxM8ma%2FT0XZJoDdF3GBnENE1jlJ%2Bxw8hqIjkDT3grosH8h2kxDO7s070ksNqB6B9WvnJJ0G4iv2D7dRD7O9HOASd%2F9Q%2F7m%2BNoiSFPmQzPygdDtoorOebaUPVoIPIc%2FhWNZS8E7ld3hCDDqvaINIjsPflz7dTjq%2BavG9pxSaqxzqN4saOmvdFwp3UWHrqhv8BUo0iyJQlOVccjMqENSJcDPnNA8PO7X3hJ310iQJo8bb6OMRYqkL5qYW0jYdL9qSVVDQDAQNL03faMknt2qb%2B9UAJF6z6Rnpk1jEkqcwjk7GMTOrkvCst3syATIFkHpFlhXe0qvwyZGfVMyoidSqCycrZHbuMK2s%2BNQGOqUBbWYcLu2tptjKLEL7fjSZA9lZIZ0GbXjVCY5zqQfXWX96h74jie5NKJT7c7kB%2BICyMin2xuE%2BD%2BO5zuGh8exP6xmwvEnUNH1URi7odcniF9efHS8hzb2Vr9eybkmntOY0cO7fTQKsE874hrT6FVP4v0m6nIbEuzdtgH9LTYZw%2BIHtyWU2UonV9c4jHyKMJScRtrbnXAUmgqSYbINBHFgvdWPiMglE&X-Amz-Signature=a8a5e03f259a2246727f98b31b255d0127f1c427200631be69baf13c2495b16b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FMCCQBR%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015014Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIAthXLvCHt9nwssINsUQh%2BY7NZv8Ttl0Rw63Gd3phxurAiEAlVNlQ6zKdR%2BOBHRnsK2QNv3d9zb7aa8WMOCWMeHHpvgq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDKeNfv8%2Bx%2BeXGOznhircAz0tBsa%2FXvlm8WTpZgOLgKtUHZehFv1GIFzsGpvD1XYfSNUC1DEP7ZVi3eByZkg%2B8bp3qMcLm4Q1Q4Mr5YjM7FvntOfAHSvvHXFG9MJIXVgufv%2Bb1P5vNNX7QTPgCYlLvoL4XkM2xacwXX5YmtY%2Fpwo9iCOJuyg%2BvpdSgKPNt108FNqcWxC5cpc2UmVwUxs45Utm5cdzvpcySYaPIBGt%2FxyPBTAKgFvgQzdjS%2FXYdmYKIr70udMxljd%2B%2Bb%2BI4YPba%2FOkkh9wA%2Ft3ySDxuJpVfazuaHZL9sJ9acxM8ma%2FT0XZJoDdF3GBnENE1jlJ%2Bxw8hqIjkDT3grosH8h2kxDO7s070ksNqB6B9WvnJJ0G4iv2D7dRD7O9HOASd%2F9Q%2F7m%2BNoiSFPmQzPygdDtoorOebaUPVoIPIc%2FhWNZS8E7ld3hCDDqvaINIjsPflz7dTjq%2BavG9pxSaqxzqN4saOmvdFwp3UWHrqhv8BUo0iyJQlOVccjMqENSJcDPnNA8PO7X3hJ310iQJo8bb6OMRYqkL5qYW0jYdL9qSVVDQDAQNL03faMknt2qb%2B9UAJF6z6Rnpk1jEkqcwjk7GMTOrkvCst3syATIFkHpFlhXe0qvwyZGfVMyoidSqCycrZHbuMK2s%2BNQGOqUBbWYcLu2tptjKLEL7fjSZA9lZIZ0GbXjVCY5zqQfXWX96h74jie5NKJT7c7kB%2BICyMin2xuE%2BD%2BO5zuGh8exP6xmwvEnUNH1URi7odcniF9efHS8hzb2Vr9eybkmntOY0cO7fTQKsE874hrT6FVP4v0m6nIbEuzdtgH9LTYZw%2BIHtyWU2UonV9c4jHyKMJScRtrbnXAUmgqSYbINBHFgvdWPiMglE&X-Amz-Signature=0350fe43de36f50d168b7c3ba5fd1db614b4111b9425a2fc7dc85a800548384c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FMCCQBR%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015014Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIAthXLvCHt9nwssINsUQh%2BY7NZv8Ttl0Rw63Gd3phxurAiEAlVNlQ6zKdR%2BOBHRnsK2QNv3d9zb7aa8WMOCWMeHHpvgq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDKeNfv8%2Bx%2BeXGOznhircAz0tBsa%2FXvlm8WTpZgOLgKtUHZehFv1GIFzsGpvD1XYfSNUC1DEP7ZVi3eByZkg%2B8bp3qMcLm4Q1Q4Mr5YjM7FvntOfAHSvvHXFG9MJIXVgufv%2Bb1P5vNNX7QTPgCYlLvoL4XkM2xacwXX5YmtY%2Fpwo9iCOJuyg%2BvpdSgKPNt108FNqcWxC5cpc2UmVwUxs45Utm5cdzvpcySYaPIBGt%2FxyPBTAKgFvgQzdjS%2FXYdmYKIr70udMxljd%2B%2Bb%2BI4YPba%2FOkkh9wA%2Ft3ySDxuJpVfazuaHZL9sJ9acxM8ma%2FT0XZJoDdF3GBnENE1jlJ%2Bxw8hqIjkDT3grosH8h2kxDO7s070ksNqB6B9WvnJJ0G4iv2D7dRD7O9HOASd%2F9Q%2F7m%2BNoiSFPmQzPygdDtoorOebaUPVoIPIc%2FhWNZS8E7ld3hCDDqvaINIjsPflz7dTjq%2BavG9pxSaqxzqN4saOmvdFwp3UWHrqhv8BUo0iyJQlOVccjMqENSJcDPnNA8PO7X3hJ310iQJo8bb6OMRYqkL5qYW0jYdL9qSVVDQDAQNL03faMknt2qb%2B9UAJF6z6Rnpk1jEkqcwjk7GMTOrkvCst3syATIFkHpFlhXe0qvwyZGfVMyoidSqCycrZHbuMK2s%2BNQGOqUBbWYcLu2tptjKLEL7fjSZA9lZIZ0GbXjVCY5zqQfXWX96h74jie5NKJT7c7kB%2BICyMin2xuE%2BD%2BO5zuGh8exP6xmwvEnUNH1URi7odcniF9efHS8hzb2Vr9eybkmntOY0cO7fTQKsE874hrT6FVP4v0m6nIbEuzdtgH9LTYZw%2BIHtyWU2UonV9c4jHyKMJScRtrbnXAUmgqSYbINBHFgvdWPiMglE&X-Amz-Signature=856f63380856ce2cb04442dff95682e1153db0684c75334bd0e4a8c306c7bb57&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FMCCQBR%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015014Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIAthXLvCHt9nwssINsUQh%2BY7NZv8Ttl0Rw63Gd3phxurAiEAlVNlQ6zKdR%2BOBHRnsK2QNv3d9zb7aa8WMOCWMeHHpvgq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDKeNfv8%2Bx%2BeXGOznhircAz0tBsa%2FXvlm8WTpZgOLgKtUHZehFv1GIFzsGpvD1XYfSNUC1DEP7ZVi3eByZkg%2B8bp3qMcLm4Q1Q4Mr5YjM7FvntOfAHSvvHXFG9MJIXVgufv%2Bb1P5vNNX7QTPgCYlLvoL4XkM2xacwXX5YmtY%2Fpwo9iCOJuyg%2BvpdSgKPNt108FNqcWxC5cpc2UmVwUxs45Utm5cdzvpcySYaPIBGt%2FxyPBTAKgFvgQzdjS%2FXYdmYKIr70udMxljd%2B%2Bb%2BI4YPba%2FOkkh9wA%2Ft3ySDxuJpVfazuaHZL9sJ9acxM8ma%2FT0XZJoDdF3GBnENE1jlJ%2Bxw8hqIjkDT3grosH8h2kxDO7s070ksNqB6B9WvnJJ0G4iv2D7dRD7O9HOASd%2F9Q%2F7m%2BNoiSFPmQzPygdDtoorOebaUPVoIPIc%2FhWNZS8E7ld3hCDDqvaINIjsPflz7dTjq%2BavG9pxSaqxzqN4saOmvdFwp3UWHrqhv8BUo0iyJQlOVccjMqENSJcDPnNA8PO7X3hJ310iQJo8bb6OMRYqkL5qYW0jYdL9qSVVDQDAQNL03faMknt2qb%2B9UAJF6z6Rnpk1jEkqcwjk7GMTOrkvCst3syATIFkHpFlhXe0qvwyZGfVMyoidSqCycrZHbuMK2s%2BNQGOqUBbWYcLu2tptjKLEL7fjSZA9lZIZ0GbXjVCY5zqQfXWX96h74jie5NKJT7c7kB%2BICyMin2xuE%2BD%2BO5zuGh8exP6xmwvEnUNH1URi7odcniF9efHS8hzb2Vr9eybkmntOY0cO7fTQKsE874hrT6FVP4v0m6nIbEuzdtgH9LTYZw%2BIHtyWU2UonV9c4jHyKMJScRtrbnXAUmgqSYbINBHFgvdWPiMglE&X-Amz-Signature=96ef526a17ed910f10e6c51535fbcc5b7691ceb3bcc06bbd70ead1865b15a39d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FMCCQBR%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015014Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIAthXLvCHt9nwssINsUQh%2BY7NZv8Ttl0Rw63Gd3phxurAiEAlVNlQ6zKdR%2BOBHRnsK2QNv3d9zb7aa8WMOCWMeHHpvgq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDKeNfv8%2Bx%2BeXGOznhircAz0tBsa%2FXvlm8WTpZgOLgKtUHZehFv1GIFzsGpvD1XYfSNUC1DEP7ZVi3eByZkg%2B8bp3qMcLm4Q1Q4Mr5YjM7FvntOfAHSvvHXFG9MJIXVgufv%2Bb1P5vNNX7QTPgCYlLvoL4XkM2xacwXX5YmtY%2Fpwo9iCOJuyg%2BvpdSgKPNt108FNqcWxC5cpc2UmVwUxs45Utm5cdzvpcySYaPIBGt%2FxyPBTAKgFvgQzdjS%2FXYdmYKIr70udMxljd%2B%2Bb%2BI4YPba%2FOkkh9wA%2Ft3ySDxuJpVfazuaHZL9sJ9acxM8ma%2FT0XZJoDdF3GBnENE1jlJ%2Bxw8hqIjkDT3grosH8h2kxDO7s070ksNqB6B9WvnJJ0G4iv2D7dRD7O9HOASd%2F9Q%2F7m%2BNoiSFPmQzPygdDtoorOebaUPVoIPIc%2FhWNZS8E7ld3hCDDqvaINIjsPflz7dTjq%2BavG9pxSaqxzqN4saOmvdFwp3UWHrqhv8BUo0iyJQlOVccjMqENSJcDPnNA8PO7X3hJ310iQJo8bb6OMRYqkL5qYW0jYdL9qSVVDQDAQNL03faMknt2qb%2B9UAJF6z6Rnpk1jEkqcwjk7GMTOrkvCst3syATIFkHpFlhXe0qvwyZGfVMyoidSqCycrZHbuMK2s%2BNQGOqUBbWYcLu2tptjKLEL7fjSZA9lZIZ0GbXjVCY5zqQfXWX96h74jie5NKJT7c7kB%2BICyMin2xuE%2BD%2BO5zuGh8exP6xmwvEnUNH1URi7odcniF9efHS8hzb2Vr9eybkmntOY0cO7fTQKsE874hrT6FVP4v0m6nIbEuzdtgH9LTYZw%2BIHtyWU2UonV9c4jHyKMJScRtrbnXAUmgqSYbINBHFgvdWPiMglE&X-Amz-Signature=59a807a8fa659a36db8a23a9570ceece74e9f3ae562f415fef793a92238d4377&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FMCCQBR%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015014Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIAthXLvCHt9nwssINsUQh%2BY7NZv8Ttl0Rw63Gd3phxurAiEAlVNlQ6zKdR%2BOBHRnsK2QNv3d9zb7aa8WMOCWMeHHpvgq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDKeNfv8%2Bx%2BeXGOznhircAz0tBsa%2FXvlm8WTpZgOLgKtUHZehFv1GIFzsGpvD1XYfSNUC1DEP7ZVi3eByZkg%2B8bp3qMcLm4Q1Q4Mr5YjM7FvntOfAHSvvHXFG9MJIXVgufv%2Bb1P5vNNX7QTPgCYlLvoL4XkM2xacwXX5YmtY%2Fpwo9iCOJuyg%2BvpdSgKPNt108FNqcWxC5cpc2UmVwUxs45Utm5cdzvpcySYaPIBGt%2FxyPBTAKgFvgQzdjS%2FXYdmYKIr70udMxljd%2B%2Bb%2BI4YPba%2FOkkh9wA%2Ft3ySDxuJpVfazuaHZL9sJ9acxM8ma%2FT0XZJoDdF3GBnENE1jlJ%2Bxw8hqIjkDT3grosH8h2kxDO7s070ksNqB6B9WvnJJ0G4iv2D7dRD7O9HOASd%2F9Q%2F7m%2BNoiSFPmQzPygdDtoorOebaUPVoIPIc%2FhWNZS8E7ld3hCDDqvaINIjsPflz7dTjq%2BavG9pxSaqxzqN4saOmvdFwp3UWHrqhv8BUo0iyJQlOVccjMqENSJcDPnNA8PO7X3hJ310iQJo8bb6OMRYqkL5qYW0jYdL9qSVVDQDAQNL03faMknt2qb%2B9UAJF6z6Rnpk1jEkqcwjk7GMTOrkvCst3syATIFkHpFlhXe0qvwyZGfVMyoidSqCycrZHbuMK2s%2BNQGOqUBbWYcLu2tptjKLEL7fjSZA9lZIZ0GbXjVCY5zqQfXWX96h74jie5NKJT7c7kB%2BICyMin2xuE%2BD%2BO5zuGh8exP6xmwvEnUNH1URi7odcniF9efHS8hzb2Vr9eybkmntOY0cO7fTQKsE874hrT6FVP4v0m6nIbEuzdtgH9LTYZw%2BIHtyWU2UonV9c4jHyKMJScRtrbnXAUmgqSYbINBHFgvdWPiMglE&X-Amz-Signature=3569c8ca352b31f57763f2c773261ff9057a7c9a7c5f30a0757c4a2ed9538489&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FMCCQBR%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015014Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIAthXLvCHt9nwssINsUQh%2BY7NZv8Ttl0Rw63Gd3phxurAiEAlVNlQ6zKdR%2BOBHRnsK2QNv3d9zb7aa8WMOCWMeHHpvgq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDKeNfv8%2Bx%2BeXGOznhircAz0tBsa%2FXvlm8WTpZgOLgKtUHZehFv1GIFzsGpvD1XYfSNUC1DEP7ZVi3eByZkg%2B8bp3qMcLm4Q1Q4Mr5YjM7FvntOfAHSvvHXFG9MJIXVgufv%2Bb1P5vNNX7QTPgCYlLvoL4XkM2xacwXX5YmtY%2Fpwo9iCOJuyg%2BvpdSgKPNt108FNqcWxC5cpc2UmVwUxs45Utm5cdzvpcySYaPIBGt%2FxyPBTAKgFvgQzdjS%2FXYdmYKIr70udMxljd%2B%2Bb%2BI4YPba%2FOkkh9wA%2Ft3ySDxuJpVfazuaHZL9sJ9acxM8ma%2FT0XZJoDdF3GBnENE1jlJ%2Bxw8hqIjkDT3grosH8h2kxDO7s070ksNqB6B9WvnJJ0G4iv2D7dRD7O9HOASd%2F9Q%2F7m%2BNoiSFPmQzPygdDtoorOebaUPVoIPIc%2FhWNZS8E7ld3hCDDqvaINIjsPflz7dTjq%2BavG9pxSaqxzqN4saOmvdFwp3UWHrqhv8BUo0iyJQlOVccjMqENSJcDPnNA8PO7X3hJ310iQJo8bb6OMRYqkL5qYW0jYdL9qSVVDQDAQNL03faMknt2qb%2B9UAJF6z6Rnpk1jEkqcwjk7GMTOrkvCst3syATIFkHpFlhXe0qvwyZGfVMyoidSqCycrZHbuMK2s%2BNQGOqUBbWYcLu2tptjKLEL7fjSZA9lZIZ0GbXjVCY5zqQfXWX96h74jie5NKJT7c7kB%2BICyMin2xuE%2BD%2BO5zuGh8exP6xmwvEnUNH1URi7odcniF9efHS8hzb2Vr9eybkmntOY0cO7fTQKsE874hrT6FVP4v0m6nIbEuzdtgH9LTYZw%2BIHtyWU2UonV9c4jHyKMJScRtrbnXAUmgqSYbINBHFgvdWPiMglE&X-Amz-Signature=5586da09dc786e5129ec5ebe3e06a3fe9d2f668d55a49867379126ee1f440988&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
