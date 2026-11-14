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
fetched_at: '2026-11-14T04:00:32.482Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM75M2FD%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040027Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEp4fP2ATIRrGDyyGWis8TSVNfdWkqmv6Vu4ZYTU8MRCAiA0qu%2Bka86mVzRVlRDfP6ie1DVTXMSgWzd6zD58PEMOMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIc1dnSDRmv1z90sPKtwDg1rsOnOK3RWEud664T%2F37duLGocPN%2F2FmgpyzFh4KATAa6kBywBz8MBGWnBPx9DMdX3rmUlgDRkc0wHSmnkM6LSSckQRhX2RXS5waRoPRNlee1LgKbJSJKhx08iTtN1AvOdo5UYlycIU8eFrATGTWR8be9dH2oA0LbTsGC0%2BNC6GZu2c1BbhE5chXTmm8LuA3uxQrD6QK6vEg6Ft%2BsVH0OCbBfUxfQ4ijqUs2CtDjsDgvw9vDMp1iMWzHepcIpEG6PFHqDQUT53fm%2BohhR%2BB3jQLVUIpbJ1YbpURetUGvkeAZjCmmRsCN8Dkq43EGGS0rlbpchYxHAd5LWWF7IdZgHsL2F%2Bm8gYvwrBpM5dy%2BiNRtRZa6%2FyIVvrEe%2FpKIsZbJ3sK8LodGUXrHIQu9xwqP0IM6odTO2KA%2BLLUqm2JSs%2FpSnUkBZNem9Vj%2FQke7NF4x5%2FEgzwKvplI%2Bk1jaIGtRTn0Gp76YH4ZyKcMhRdZGJ3SYXMm5SBRdvgClWj1tKB8fNxu3grNKu1oICSlUyX6k5Eydd0ob5omb3mVNUs0juzF%2FBcqHNFAiRrm%2FndNj%2BqOEJBxtSkzHH93NgWqhpxUr0JnrgpNGUDSMIdnCaHQoVowfzTNEsJJKmVuFiIwwN7e1wY6pgH3AZTjVwkwqCVUOYd5znkgyqCsDHIBIYDzAdRY5d5gZE8PZOdn4eyKDQ59mlxz0gvfySVYcMYYv3NvQNnE00gRfsEhMuzZoa2BMPqQxU8BCjlQigOoyHw8HfLXdzBuuG2g6ufwTahLcyxTExsTsLtnIktWvaLUbI3uLFZ8ad%2BWMCR9XldCxWmWXNYpGrc4luIyeeEtT%2Fa2%2BtAe8mOfES%2Fvdx%2FQ7QSR&X-Amz-Signature=5f9ec5aa71cc356c1d7363ab0918fdfb7a92b6a1a83b516af9cef720be1d0000&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM75M2FD%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040027Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEp4fP2ATIRrGDyyGWis8TSVNfdWkqmv6Vu4ZYTU8MRCAiA0qu%2Bka86mVzRVlRDfP6ie1DVTXMSgWzd6zD58PEMOMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIc1dnSDRmv1z90sPKtwDg1rsOnOK3RWEud664T%2F37duLGocPN%2F2FmgpyzFh4KATAa6kBywBz8MBGWnBPx9DMdX3rmUlgDRkc0wHSmnkM6LSSckQRhX2RXS5waRoPRNlee1LgKbJSJKhx08iTtN1AvOdo5UYlycIU8eFrATGTWR8be9dH2oA0LbTsGC0%2BNC6GZu2c1BbhE5chXTmm8LuA3uxQrD6QK6vEg6Ft%2BsVH0OCbBfUxfQ4ijqUs2CtDjsDgvw9vDMp1iMWzHepcIpEG6PFHqDQUT53fm%2BohhR%2BB3jQLVUIpbJ1YbpURetUGvkeAZjCmmRsCN8Dkq43EGGS0rlbpchYxHAd5LWWF7IdZgHsL2F%2Bm8gYvwrBpM5dy%2BiNRtRZa6%2FyIVvrEe%2FpKIsZbJ3sK8LodGUXrHIQu9xwqP0IM6odTO2KA%2BLLUqm2JSs%2FpSnUkBZNem9Vj%2FQke7NF4x5%2FEgzwKvplI%2Bk1jaIGtRTn0Gp76YH4ZyKcMhRdZGJ3SYXMm5SBRdvgClWj1tKB8fNxu3grNKu1oICSlUyX6k5Eydd0ob5omb3mVNUs0juzF%2FBcqHNFAiRrm%2FndNj%2BqOEJBxtSkzHH93NgWqhpxUr0JnrgpNGUDSMIdnCaHQoVowfzTNEsJJKmVuFiIwwN7e1wY6pgH3AZTjVwkwqCVUOYd5znkgyqCsDHIBIYDzAdRY5d5gZE8PZOdn4eyKDQ59mlxz0gvfySVYcMYYv3NvQNnE00gRfsEhMuzZoa2BMPqQxU8BCjlQigOoyHw8HfLXdzBuuG2g6ufwTahLcyxTExsTsLtnIktWvaLUbI3uLFZ8ad%2BWMCR9XldCxWmWXNYpGrc4luIyeeEtT%2Fa2%2BtAe8mOfES%2Fvdx%2FQ7QSR&X-Amz-Signature=a9abae2328066b99cb4e5f56e6ee00c56b52aa96632958e4776fa5a896d542e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM75M2FD%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040027Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEp4fP2ATIRrGDyyGWis8TSVNfdWkqmv6Vu4ZYTU8MRCAiA0qu%2Bka86mVzRVlRDfP6ie1DVTXMSgWzd6zD58PEMOMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIc1dnSDRmv1z90sPKtwDg1rsOnOK3RWEud664T%2F37duLGocPN%2F2FmgpyzFh4KATAa6kBywBz8MBGWnBPx9DMdX3rmUlgDRkc0wHSmnkM6LSSckQRhX2RXS5waRoPRNlee1LgKbJSJKhx08iTtN1AvOdo5UYlycIU8eFrATGTWR8be9dH2oA0LbTsGC0%2BNC6GZu2c1BbhE5chXTmm8LuA3uxQrD6QK6vEg6Ft%2BsVH0OCbBfUxfQ4ijqUs2CtDjsDgvw9vDMp1iMWzHepcIpEG6PFHqDQUT53fm%2BohhR%2BB3jQLVUIpbJ1YbpURetUGvkeAZjCmmRsCN8Dkq43EGGS0rlbpchYxHAd5LWWF7IdZgHsL2F%2Bm8gYvwrBpM5dy%2BiNRtRZa6%2FyIVvrEe%2FpKIsZbJ3sK8LodGUXrHIQu9xwqP0IM6odTO2KA%2BLLUqm2JSs%2FpSnUkBZNem9Vj%2FQke7NF4x5%2FEgzwKvplI%2Bk1jaIGtRTn0Gp76YH4ZyKcMhRdZGJ3SYXMm5SBRdvgClWj1tKB8fNxu3grNKu1oICSlUyX6k5Eydd0ob5omb3mVNUs0juzF%2FBcqHNFAiRrm%2FndNj%2BqOEJBxtSkzHH93NgWqhpxUr0JnrgpNGUDSMIdnCaHQoVowfzTNEsJJKmVuFiIwwN7e1wY6pgH3AZTjVwkwqCVUOYd5znkgyqCsDHIBIYDzAdRY5d5gZE8PZOdn4eyKDQ59mlxz0gvfySVYcMYYv3NvQNnE00gRfsEhMuzZoa2BMPqQxU8BCjlQigOoyHw8HfLXdzBuuG2g6ufwTahLcyxTExsTsLtnIktWvaLUbI3uLFZ8ad%2BWMCR9XldCxWmWXNYpGrc4luIyeeEtT%2Fa2%2BtAe8mOfES%2Fvdx%2FQ7QSR&X-Amz-Signature=35c8a9834bef542ab8877c4dff9828e5940aab5feefc46460fe75e6855d5ca76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM75M2FD%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040027Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEp4fP2ATIRrGDyyGWis8TSVNfdWkqmv6Vu4ZYTU8MRCAiA0qu%2Bka86mVzRVlRDfP6ie1DVTXMSgWzd6zD58PEMOMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIc1dnSDRmv1z90sPKtwDg1rsOnOK3RWEud664T%2F37duLGocPN%2F2FmgpyzFh4KATAa6kBywBz8MBGWnBPx9DMdX3rmUlgDRkc0wHSmnkM6LSSckQRhX2RXS5waRoPRNlee1LgKbJSJKhx08iTtN1AvOdo5UYlycIU8eFrATGTWR8be9dH2oA0LbTsGC0%2BNC6GZu2c1BbhE5chXTmm8LuA3uxQrD6QK6vEg6Ft%2BsVH0OCbBfUxfQ4ijqUs2CtDjsDgvw9vDMp1iMWzHepcIpEG6PFHqDQUT53fm%2BohhR%2BB3jQLVUIpbJ1YbpURetUGvkeAZjCmmRsCN8Dkq43EGGS0rlbpchYxHAd5LWWF7IdZgHsL2F%2Bm8gYvwrBpM5dy%2BiNRtRZa6%2FyIVvrEe%2FpKIsZbJ3sK8LodGUXrHIQu9xwqP0IM6odTO2KA%2BLLUqm2JSs%2FpSnUkBZNem9Vj%2FQke7NF4x5%2FEgzwKvplI%2Bk1jaIGtRTn0Gp76YH4ZyKcMhRdZGJ3SYXMm5SBRdvgClWj1tKB8fNxu3grNKu1oICSlUyX6k5Eydd0ob5omb3mVNUs0juzF%2FBcqHNFAiRrm%2FndNj%2BqOEJBxtSkzHH93NgWqhpxUr0JnrgpNGUDSMIdnCaHQoVowfzTNEsJJKmVuFiIwwN7e1wY6pgH3AZTjVwkwqCVUOYd5znkgyqCsDHIBIYDzAdRY5d5gZE8PZOdn4eyKDQ59mlxz0gvfySVYcMYYv3NvQNnE00gRfsEhMuzZoa2BMPqQxU8BCjlQigOoyHw8HfLXdzBuuG2g6ufwTahLcyxTExsTsLtnIktWvaLUbI3uLFZ8ad%2BWMCR9XldCxWmWXNYpGrc4luIyeeEtT%2Fa2%2BtAe8mOfES%2Fvdx%2FQ7QSR&X-Amz-Signature=c2b97079d89c516d480ac7f7882147baa16f993d4c4e1d3fe71dcc7e1e4fabd7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM75M2FD%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040027Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEp4fP2ATIRrGDyyGWis8TSVNfdWkqmv6Vu4ZYTU8MRCAiA0qu%2Bka86mVzRVlRDfP6ie1DVTXMSgWzd6zD58PEMOMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIc1dnSDRmv1z90sPKtwDg1rsOnOK3RWEud664T%2F37duLGocPN%2F2FmgpyzFh4KATAa6kBywBz8MBGWnBPx9DMdX3rmUlgDRkc0wHSmnkM6LSSckQRhX2RXS5waRoPRNlee1LgKbJSJKhx08iTtN1AvOdo5UYlycIU8eFrATGTWR8be9dH2oA0LbTsGC0%2BNC6GZu2c1BbhE5chXTmm8LuA3uxQrD6QK6vEg6Ft%2BsVH0OCbBfUxfQ4ijqUs2CtDjsDgvw9vDMp1iMWzHepcIpEG6PFHqDQUT53fm%2BohhR%2BB3jQLVUIpbJ1YbpURetUGvkeAZjCmmRsCN8Dkq43EGGS0rlbpchYxHAd5LWWF7IdZgHsL2F%2Bm8gYvwrBpM5dy%2BiNRtRZa6%2FyIVvrEe%2FpKIsZbJ3sK8LodGUXrHIQu9xwqP0IM6odTO2KA%2BLLUqm2JSs%2FpSnUkBZNem9Vj%2FQke7NF4x5%2FEgzwKvplI%2Bk1jaIGtRTn0Gp76YH4ZyKcMhRdZGJ3SYXMm5SBRdvgClWj1tKB8fNxu3grNKu1oICSlUyX6k5Eydd0ob5omb3mVNUs0juzF%2FBcqHNFAiRrm%2FndNj%2BqOEJBxtSkzHH93NgWqhpxUr0JnrgpNGUDSMIdnCaHQoVowfzTNEsJJKmVuFiIwwN7e1wY6pgH3AZTjVwkwqCVUOYd5znkgyqCsDHIBIYDzAdRY5d5gZE8PZOdn4eyKDQ59mlxz0gvfySVYcMYYv3NvQNnE00gRfsEhMuzZoa2BMPqQxU8BCjlQigOoyHw8HfLXdzBuuG2g6ufwTahLcyxTExsTsLtnIktWvaLUbI3uLFZ8ad%2BWMCR9XldCxWmWXNYpGrc4luIyeeEtT%2Fa2%2BtAe8mOfES%2Fvdx%2FQ7QSR&X-Amz-Signature=85dadb87a5e747b076ee48fd0411e95fcfcb509191419680cb9480c2d7139406&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM75M2FD%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040027Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEp4fP2ATIRrGDyyGWis8TSVNfdWkqmv6Vu4ZYTU8MRCAiA0qu%2Bka86mVzRVlRDfP6ie1DVTXMSgWzd6zD58PEMOMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIc1dnSDRmv1z90sPKtwDg1rsOnOK3RWEud664T%2F37duLGocPN%2F2FmgpyzFh4KATAa6kBywBz8MBGWnBPx9DMdX3rmUlgDRkc0wHSmnkM6LSSckQRhX2RXS5waRoPRNlee1LgKbJSJKhx08iTtN1AvOdo5UYlycIU8eFrATGTWR8be9dH2oA0LbTsGC0%2BNC6GZu2c1BbhE5chXTmm8LuA3uxQrD6QK6vEg6Ft%2BsVH0OCbBfUxfQ4ijqUs2CtDjsDgvw9vDMp1iMWzHepcIpEG6PFHqDQUT53fm%2BohhR%2BB3jQLVUIpbJ1YbpURetUGvkeAZjCmmRsCN8Dkq43EGGS0rlbpchYxHAd5LWWF7IdZgHsL2F%2Bm8gYvwrBpM5dy%2BiNRtRZa6%2FyIVvrEe%2FpKIsZbJ3sK8LodGUXrHIQu9xwqP0IM6odTO2KA%2BLLUqm2JSs%2FpSnUkBZNem9Vj%2FQke7NF4x5%2FEgzwKvplI%2Bk1jaIGtRTn0Gp76YH4ZyKcMhRdZGJ3SYXMm5SBRdvgClWj1tKB8fNxu3grNKu1oICSlUyX6k5Eydd0ob5omb3mVNUs0juzF%2FBcqHNFAiRrm%2FndNj%2BqOEJBxtSkzHH93NgWqhpxUr0JnrgpNGUDSMIdnCaHQoVowfzTNEsJJKmVuFiIwwN7e1wY6pgH3AZTjVwkwqCVUOYd5znkgyqCsDHIBIYDzAdRY5d5gZE8PZOdn4eyKDQ59mlxz0gvfySVYcMYYv3NvQNnE00gRfsEhMuzZoa2BMPqQxU8BCjlQigOoyHw8HfLXdzBuuG2g6ufwTahLcyxTExsTsLtnIktWvaLUbI3uLFZ8ad%2BWMCR9XldCxWmWXNYpGrc4luIyeeEtT%2Fa2%2BtAe8mOfES%2Fvdx%2FQ7QSR&X-Amz-Signature=e821c14d3fadfd57645960b265067409ee9649409cec0abe7f3b3d9c171f5f01&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM75M2FD%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040027Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEp4fP2ATIRrGDyyGWis8TSVNfdWkqmv6Vu4ZYTU8MRCAiA0qu%2Bka86mVzRVlRDfP6ie1DVTXMSgWzd6zD58PEMOMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIc1dnSDRmv1z90sPKtwDg1rsOnOK3RWEud664T%2F37duLGocPN%2F2FmgpyzFh4KATAa6kBywBz8MBGWnBPx9DMdX3rmUlgDRkc0wHSmnkM6LSSckQRhX2RXS5waRoPRNlee1LgKbJSJKhx08iTtN1AvOdo5UYlycIU8eFrATGTWR8be9dH2oA0LbTsGC0%2BNC6GZu2c1BbhE5chXTmm8LuA3uxQrD6QK6vEg6Ft%2BsVH0OCbBfUxfQ4ijqUs2CtDjsDgvw9vDMp1iMWzHepcIpEG6PFHqDQUT53fm%2BohhR%2BB3jQLVUIpbJ1YbpURetUGvkeAZjCmmRsCN8Dkq43EGGS0rlbpchYxHAd5LWWF7IdZgHsL2F%2Bm8gYvwrBpM5dy%2BiNRtRZa6%2FyIVvrEe%2FpKIsZbJ3sK8LodGUXrHIQu9xwqP0IM6odTO2KA%2BLLUqm2JSs%2FpSnUkBZNem9Vj%2FQke7NF4x5%2FEgzwKvplI%2Bk1jaIGtRTn0Gp76YH4ZyKcMhRdZGJ3SYXMm5SBRdvgClWj1tKB8fNxu3grNKu1oICSlUyX6k5Eydd0ob5omb3mVNUs0juzF%2FBcqHNFAiRrm%2FndNj%2BqOEJBxtSkzHH93NgWqhpxUr0JnrgpNGUDSMIdnCaHQoVowfzTNEsJJKmVuFiIwwN7e1wY6pgH3AZTjVwkwqCVUOYd5znkgyqCsDHIBIYDzAdRY5d5gZE8PZOdn4eyKDQ59mlxz0gvfySVYcMYYv3NvQNnE00gRfsEhMuzZoa2BMPqQxU8BCjlQigOoyHw8HfLXdzBuuG2g6ufwTahLcyxTExsTsLtnIktWvaLUbI3uLFZ8ad%2BWMCR9XldCxWmWXNYpGrc4luIyeeEtT%2Fa2%2BtAe8mOfES%2Fvdx%2FQ7QSR&X-Amz-Signature=6ce990310148cb97eb02c99b6258f74d959c2fa77205c44f900e4663164a8af1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM75M2FD%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040027Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEp4fP2ATIRrGDyyGWis8TSVNfdWkqmv6Vu4ZYTU8MRCAiA0qu%2Bka86mVzRVlRDfP6ie1DVTXMSgWzd6zD58PEMOMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIc1dnSDRmv1z90sPKtwDg1rsOnOK3RWEud664T%2F37duLGocPN%2F2FmgpyzFh4KATAa6kBywBz8MBGWnBPx9DMdX3rmUlgDRkc0wHSmnkM6LSSckQRhX2RXS5waRoPRNlee1LgKbJSJKhx08iTtN1AvOdo5UYlycIU8eFrATGTWR8be9dH2oA0LbTsGC0%2BNC6GZu2c1BbhE5chXTmm8LuA3uxQrD6QK6vEg6Ft%2BsVH0OCbBfUxfQ4ijqUs2CtDjsDgvw9vDMp1iMWzHepcIpEG6PFHqDQUT53fm%2BohhR%2BB3jQLVUIpbJ1YbpURetUGvkeAZjCmmRsCN8Dkq43EGGS0rlbpchYxHAd5LWWF7IdZgHsL2F%2Bm8gYvwrBpM5dy%2BiNRtRZa6%2FyIVvrEe%2FpKIsZbJ3sK8LodGUXrHIQu9xwqP0IM6odTO2KA%2BLLUqm2JSs%2FpSnUkBZNem9Vj%2FQke7NF4x5%2FEgzwKvplI%2Bk1jaIGtRTn0Gp76YH4ZyKcMhRdZGJ3SYXMm5SBRdvgClWj1tKB8fNxu3grNKu1oICSlUyX6k5Eydd0ob5omb3mVNUs0juzF%2FBcqHNFAiRrm%2FndNj%2BqOEJBxtSkzHH93NgWqhpxUr0JnrgpNGUDSMIdnCaHQoVowfzTNEsJJKmVuFiIwwN7e1wY6pgH3AZTjVwkwqCVUOYd5znkgyqCsDHIBIYDzAdRY5d5gZE8PZOdn4eyKDQ59mlxz0gvfySVYcMYYv3NvQNnE00gRfsEhMuzZoa2BMPqQxU8BCjlQigOoyHw8HfLXdzBuuG2g6ufwTahLcyxTExsTsLtnIktWvaLUbI3uLFZ8ad%2BWMCR9XldCxWmWXNYpGrc4luIyeeEtT%2Fa2%2BtAe8mOfES%2Fvdx%2FQ7QSR&X-Amz-Signature=4aaae995fdde22b27833dc0a7c5ce1ed36f95aa59d7e9c4efc29b61597b7717a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM75M2FD%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040027Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEp4fP2ATIRrGDyyGWis8TSVNfdWkqmv6Vu4ZYTU8MRCAiA0qu%2Bka86mVzRVlRDfP6ie1DVTXMSgWzd6zD58PEMOMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIc1dnSDRmv1z90sPKtwDg1rsOnOK3RWEud664T%2F37duLGocPN%2F2FmgpyzFh4KATAa6kBywBz8MBGWnBPx9DMdX3rmUlgDRkc0wHSmnkM6LSSckQRhX2RXS5waRoPRNlee1LgKbJSJKhx08iTtN1AvOdo5UYlycIU8eFrATGTWR8be9dH2oA0LbTsGC0%2BNC6GZu2c1BbhE5chXTmm8LuA3uxQrD6QK6vEg6Ft%2BsVH0OCbBfUxfQ4ijqUs2CtDjsDgvw9vDMp1iMWzHepcIpEG6PFHqDQUT53fm%2BohhR%2BB3jQLVUIpbJ1YbpURetUGvkeAZjCmmRsCN8Dkq43EGGS0rlbpchYxHAd5LWWF7IdZgHsL2F%2Bm8gYvwrBpM5dy%2BiNRtRZa6%2FyIVvrEe%2FpKIsZbJ3sK8LodGUXrHIQu9xwqP0IM6odTO2KA%2BLLUqm2JSs%2FpSnUkBZNem9Vj%2FQke7NF4x5%2FEgzwKvplI%2Bk1jaIGtRTn0Gp76YH4ZyKcMhRdZGJ3SYXMm5SBRdvgClWj1tKB8fNxu3grNKu1oICSlUyX6k5Eydd0ob5omb3mVNUs0juzF%2FBcqHNFAiRrm%2FndNj%2BqOEJBxtSkzHH93NgWqhpxUr0JnrgpNGUDSMIdnCaHQoVowfzTNEsJJKmVuFiIwwN7e1wY6pgH3AZTjVwkwqCVUOYd5znkgyqCsDHIBIYDzAdRY5d5gZE8PZOdn4eyKDQ59mlxz0gvfySVYcMYYv3NvQNnE00gRfsEhMuzZoa2BMPqQxU8BCjlQigOoyHw8HfLXdzBuuG2g6ufwTahLcyxTExsTsLtnIktWvaLUbI3uLFZ8ad%2BWMCR9XldCxWmWXNYpGrc4luIyeeEtT%2Fa2%2BtAe8mOfES%2Fvdx%2FQ7QSR&X-Amz-Signature=a9b2763b02133e35bdf9b2cd9a238d3a3204db5e7613e37e6febb973019dd6bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
