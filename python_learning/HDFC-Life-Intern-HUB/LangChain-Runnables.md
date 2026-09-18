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
fetched_at: '2026-09-18T02:08:30.404Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664U5JHXTV%2F20260918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260918T020826Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGK0VFQCcyLhe9WyITpTMbXiMiOJcBxeel3mS%2B9Ctw80AiEAvnZjDooGe9zAnPtODFz5jTvMixHUd9e5sejHCg0O2j8q%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDFy9DtgIF9UgOjV%2BhCrcA3lIguQ0cuOkUAH1uD0DJjlpCPpBjCmWdCcgs%2FOP79gEfBRWHmGUXG%2FJnj2ubTwpgvQCfB5sv7r5tp419WjMZA1pSAEoHjg9rVZZ0XCGeKXPFVUYDaNZrgk09b3zt9Ced3%2Fq53Zc8YSzJ2%2BMCkm1KP%2F%2BsMTgNCEQJtmc6Mw665vZAlaWfPUwMOKAuv6p6z9vbMvZ03KztfiS7b1YI8Y1UxFgRrR%2FyL7XpPneeoePCJFRStAjlQ4XtN8mEtY9fLSX%2FJBooyyeObwfy1I6mXWu2IJs%2FHXF4A3yX9%2FNyZ5W%2BVgGmlPP18i7M%2B1Om2PLwNRdqW8PqMLxzWbICom4YWDm9j6glIWs1S0yG6p13y9lXPWadZRx7tWsbCNkJZwlyQu0bq0%2F%2FQjI5XxjhTHqOuE01blmpU88bHL3nk3myhFp%2BJkCGTCdWqyQe%2BWJSQdotHBceQ8pU6NAGNJyKMGzhhVT%2FcIdwOtC%2FrjCSGAqc55rS8MowPZOA4%2BjMdFn%2FL5NwRrg%2BXuC%2F0j9kvaoeBDoT%2FnkgvCpzh7CXHTqvK2%2FiF3P9n4oCeCSzRiYIrTgs%2Fup9gKewPakt9ZKEEVqlTDxdCrpHcevV5516orA7qKauCG35%2FOPwOwE76JxytSEpPkFMMKVstUGOqUBf%2FjNo%2BH7AuROBf20hX5NZSgP1JgfxLahLvVSiX3VeU7yvsB2KdKzQU4CTHv2Fy7kab9xUme9S0Q5HxgBzg7SEJwjgVFXhDbfeNlhUefoglQ6yCkU4w4KKZOvYdwXjGZQTC3dHmOcywDant45W5EsIMjcP5ZuHSxvhhdmRZyZ6UDXgfvuY8sq0NDY6EkGf2SGgbROWrGHxELnPpORjuqYPxIF5mSR&X-Amz-Signature=71d55e8384538f8246ee6181792492c39c843606340321d186632849a1d32f72&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664U5JHXTV%2F20260918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260918T020826Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGK0VFQCcyLhe9WyITpTMbXiMiOJcBxeel3mS%2B9Ctw80AiEAvnZjDooGe9zAnPtODFz5jTvMixHUd9e5sejHCg0O2j8q%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDFy9DtgIF9UgOjV%2BhCrcA3lIguQ0cuOkUAH1uD0DJjlpCPpBjCmWdCcgs%2FOP79gEfBRWHmGUXG%2FJnj2ubTwpgvQCfB5sv7r5tp419WjMZA1pSAEoHjg9rVZZ0XCGeKXPFVUYDaNZrgk09b3zt9Ced3%2Fq53Zc8YSzJ2%2BMCkm1KP%2F%2BsMTgNCEQJtmc6Mw665vZAlaWfPUwMOKAuv6p6z9vbMvZ03KztfiS7b1YI8Y1UxFgRrR%2FyL7XpPneeoePCJFRStAjlQ4XtN8mEtY9fLSX%2FJBooyyeObwfy1I6mXWu2IJs%2FHXF4A3yX9%2FNyZ5W%2BVgGmlPP18i7M%2B1Om2PLwNRdqW8PqMLxzWbICom4YWDm9j6glIWs1S0yG6p13y9lXPWadZRx7tWsbCNkJZwlyQu0bq0%2F%2FQjI5XxjhTHqOuE01blmpU88bHL3nk3myhFp%2BJkCGTCdWqyQe%2BWJSQdotHBceQ8pU6NAGNJyKMGzhhVT%2FcIdwOtC%2FrjCSGAqc55rS8MowPZOA4%2BjMdFn%2FL5NwRrg%2BXuC%2F0j9kvaoeBDoT%2FnkgvCpzh7CXHTqvK2%2FiF3P9n4oCeCSzRiYIrTgs%2Fup9gKewPakt9ZKEEVqlTDxdCrpHcevV5516orA7qKauCG35%2FOPwOwE76JxytSEpPkFMMKVstUGOqUBf%2FjNo%2BH7AuROBf20hX5NZSgP1JgfxLahLvVSiX3VeU7yvsB2KdKzQU4CTHv2Fy7kab9xUme9S0Q5HxgBzg7SEJwjgVFXhDbfeNlhUefoglQ6yCkU4w4KKZOvYdwXjGZQTC3dHmOcywDant45W5EsIMjcP5ZuHSxvhhdmRZyZ6UDXgfvuY8sq0NDY6EkGf2SGgbROWrGHxELnPpORjuqYPxIF5mSR&X-Amz-Signature=3f7f538d4baa6f561253cbdc43ba59d52959f7078be29fbabd812e619dd27952&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664U5JHXTV%2F20260918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260918T020825Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGK0VFQCcyLhe9WyITpTMbXiMiOJcBxeel3mS%2B9Ctw80AiEAvnZjDooGe9zAnPtODFz5jTvMixHUd9e5sejHCg0O2j8q%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDFy9DtgIF9UgOjV%2BhCrcA3lIguQ0cuOkUAH1uD0DJjlpCPpBjCmWdCcgs%2FOP79gEfBRWHmGUXG%2FJnj2ubTwpgvQCfB5sv7r5tp419WjMZA1pSAEoHjg9rVZZ0XCGeKXPFVUYDaNZrgk09b3zt9Ced3%2Fq53Zc8YSzJ2%2BMCkm1KP%2F%2BsMTgNCEQJtmc6Mw665vZAlaWfPUwMOKAuv6p6z9vbMvZ03KztfiS7b1YI8Y1UxFgRrR%2FyL7XpPneeoePCJFRStAjlQ4XtN8mEtY9fLSX%2FJBooyyeObwfy1I6mXWu2IJs%2FHXF4A3yX9%2FNyZ5W%2BVgGmlPP18i7M%2B1Om2PLwNRdqW8PqMLxzWbICom4YWDm9j6glIWs1S0yG6p13y9lXPWadZRx7tWsbCNkJZwlyQu0bq0%2F%2FQjI5XxjhTHqOuE01blmpU88bHL3nk3myhFp%2BJkCGTCdWqyQe%2BWJSQdotHBceQ8pU6NAGNJyKMGzhhVT%2FcIdwOtC%2FrjCSGAqc55rS8MowPZOA4%2BjMdFn%2FL5NwRrg%2BXuC%2F0j9kvaoeBDoT%2FnkgvCpzh7CXHTqvK2%2FiF3P9n4oCeCSzRiYIrTgs%2Fup9gKewPakt9ZKEEVqlTDxdCrpHcevV5516orA7qKauCG35%2FOPwOwE76JxytSEpPkFMMKVstUGOqUBf%2FjNo%2BH7AuROBf20hX5NZSgP1JgfxLahLvVSiX3VeU7yvsB2KdKzQU4CTHv2Fy7kab9xUme9S0Q5HxgBzg7SEJwjgVFXhDbfeNlhUefoglQ6yCkU4w4KKZOvYdwXjGZQTC3dHmOcywDant45W5EsIMjcP5ZuHSxvhhdmRZyZ6UDXgfvuY8sq0NDY6EkGf2SGgbROWrGHxELnPpORjuqYPxIF5mSR&X-Amz-Signature=c26f821b1603d3a1e3736b26432d0cdfed5e3da7230adc810b7252dd5357e515&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664U5JHXTV%2F20260918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260918T020825Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGK0VFQCcyLhe9WyITpTMbXiMiOJcBxeel3mS%2B9Ctw80AiEAvnZjDooGe9zAnPtODFz5jTvMixHUd9e5sejHCg0O2j8q%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDFy9DtgIF9UgOjV%2BhCrcA3lIguQ0cuOkUAH1uD0DJjlpCPpBjCmWdCcgs%2FOP79gEfBRWHmGUXG%2FJnj2ubTwpgvQCfB5sv7r5tp419WjMZA1pSAEoHjg9rVZZ0XCGeKXPFVUYDaNZrgk09b3zt9Ced3%2Fq53Zc8YSzJ2%2BMCkm1KP%2F%2BsMTgNCEQJtmc6Mw665vZAlaWfPUwMOKAuv6p6z9vbMvZ03KztfiS7b1YI8Y1UxFgRrR%2FyL7XpPneeoePCJFRStAjlQ4XtN8mEtY9fLSX%2FJBooyyeObwfy1I6mXWu2IJs%2FHXF4A3yX9%2FNyZ5W%2BVgGmlPP18i7M%2B1Om2PLwNRdqW8PqMLxzWbICom4YWDm9j6glIWs1S0yG6p13y9lXPWadZRx7tWsbCNkJZwlyQu0bq0%2F%2FQjI5XxjhTHqOuE01blmpU88bHL3nk3myhFp%2BJkCGTCdWqyQe%2BWJSQdotHBceQ8pU6NAGNJyKMGzhhVT%2FcIdwOtC%2FrjCSGAqc55rS8MowPZOA4%2BjMdFn%2FL5NwRrg%2BXuC%2F0j9kvaoeBDoT%2FnkgvCpzh7CXHTqvK2%2FiF3P9n4oCeCSzRiYIrTgs%2Fup9gKewPakt9ZKEEVqlTDxdCrpHcevV5516orA7qKauCG35%2FOPwOwE76JxytSEpPkFMMKVstUGOqUBf%2FjNo%2BH7AuROBf20hX5NZSgP1JgfxLahLvVSiX3VeU7yvsB2KdKzQU4CTHv2Fy7kab9xUme9S0Q5HxgBzg7SEJwjgVFXhDbfeNlhUefoglQ6yCkU4w4KKZOvYdwXjGZQTC3dHmOcywDant45W5EsIMjcP5ZuHSxvhhdmRZyZ6UDXgfvuY8sq0NDY6EkGf2SGgbROWrGHxELnPpORjuqYPxIF5mSR&X-Amz-Signature=ef33157047b381a5b4ea5e2deef464b5f60356100dd4d92cfbda38680dffe272&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664U5JHXTV%2F20260918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260918T020825Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGK0VFQCcyLhe9WyITpTMbXiMiOJcBxeel3mS%2B9Ctw80AiEAvnZjDooGe9zAnPtODFz5jTvMixHUd9e5sejHCg0O2j8q%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDFy9DtgIF9UgOjV%2BhCrcA3lIguQ0cuOkUAH1uD0DJjlpCPpBjCmWdCcgs%2FOP79gEfBRWHmGUXG%2FJnj2ubTwpgvQCfB5sv7r5tp419WjMZA1pSAEoHjg9rVZZ0XCGeKXPFVUYDaNZrgk09b3zt9Ced3%2Fq53Zc8YSzJ2%2BMCkm1KP%2F%2BsMTgNCEQJtmc6Mw665vZAlaWfPUwMOKAuv6p6z9vbMvZ03KztfiS7b1YI8Y1UxFgRrR%2FyL7XpPneeoePCJFRStAjlQ4XtN8mEtY9fLSX%2FJBooyyeObwfy1I6mXWu2IJs%2FHXF4A3yX9%2FNyZ5W%2BVgGmlPP18i7M%2B1Om2PLwNRdqW8PqMLxzWbICom4YWDm9j6glIWs1S0yG6p13y9lXPWadZRx7tWsbCNkJZwlyQu0bq0%2F%2FQjI5XxjhTHqOuE01blmpU88bHL3nk3myhFp%2BJkCGTCdWqyQe%2BWJSQdotHBceQ8pU6NAGNJyKMGzhhVT%2FcIdwOtC%2FrjCSGAqc55rS8MowPZOA4%2BjMdFn%2FL5NwRrg%2BXuC%2F0j9kvaoeBDoT%2FnkgvCpzh7CXHTqvK2%2FiF3P9n4oCeCSzRiYIrTgs%2Fup9gKewPakt9ZKEEVqlTDxdCrpHcevV5516orA7qKauCG35%2FOPwOwE76JxytSEpPkFMMKVstUGOqUBf%2FjNo%2BH7AuROBf20hX5NZSgP1JgfxLahLvVSiX3VeU7yvsB2KdKzQU4CTHv2Fy7kab9xUme9S0Q5HxgBzg7SEJwjgVFXhDbfeNlhUefoglQ6yCkU4w4KKZOvYdwXjGZQTC3dHmOcywDant45W5EsIMjcP5ZuHSxvhhdmRZyZ6UDXgfvuY8sq0NDY6EkGf2SGgbROWrGHxELnPpORjuqYPxIF5mSR&X-Amz-Signature=49b001c34b4afb419e998344d83f255a04e059c1f5459deab30939f0f9c83523&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664U5JHXTV%2F20260918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260918T020825Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGK0VFQCcyLhe9WyITpTMbXiMiOJcBxeel3mS%2B9Ctw80AiEAvnZjDooGe9zAnPtODFz5jTvMixHUd9e5sejHCg0O2j8q%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDFy9DtgIF9UgOjV%2BhCrcA3lIguQ0cuOkUAH1uD0DJjlpCPpBjCmWdCcgs%2FOP79gEfBRWHmGUXG%2FJnj2ubTwpgvQCfB5sv7r5tp419WjMZA1pSAEoHjg9rVZZ0XCGeKXPFVUYDaNZrgk09b3zt9Ced3%2Fq53Zc8YSzJ2%2BMCkm1KP%2F%2BsMTgNCEQJtmc6Mw665vZAlaWfPUwMOKAuv6p6z9vbMvZ03KztfiS7b1YI8Y1UxFgRrR%2FyL7XpPneeoePCJFRStAjlQ4XtN8mEtY9fLSX%2FJBooyyeObwfy1I6mXWu2IJs%2FHXF4A3yX9%2FNyZ5W%2BVgGmlPP18i7M%2B1Om2PLwNRdqW8PqMLxzWbICom4YWDm9j6glIWs1S0yG6p13y9lXPWadZRx7tWsbCNkJZwlyQu0bq0%2F%2FQjI5XxjhTHqOuE01blmpU88bHL3nk3myhFp%2BJkCGTCdWqyQe%2BWJSQdotHBceQ8pU6NAGNJyKMGzhhVT%2FcIdwOtC%2FrjCSGAqc55rS8MowPZOA4%2BjMdFn%2FL5NwRrg%2BXuC%2F0j9kvaoeBDoT%2FnkgvCpzh7CXHTqvK2%2FiF3P9n4oCeCSzRiYIrTgs%2Fup9gKewPakt9ZKEEVqlTDxdCrpHcevV5516orA7qKauCG35%2FOPwOwE76JxytSEpPkFMMKVstUGOqUBf%2FjNo%2BH7AuROBf20hX5NZSgP1JgfxLahLvVSiX3VeU7yvsB2KdKzQU4CTHv2Fy7kab9xUme9S0Q5HxgBzg7SEJwjgVFXhDbfeNlhUefoglQ6yCkU4w4KKZOvYdwXjGZQTC3dHmOcywDant45W5EsIMjcP5ZuHSxvhhdmRZyZ6UDXgfvuY8sq0NDY6EkGf2SGgbROWrGHxELnPpORjuqYPxIF5mSR&X-Amz-Signature=fadee38f01f56319c5e1ea7da4ed4b7acd08b8a5ee4ecd0c28f4970fd8fbe0c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664U5JHXTV%2F20260918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260918T020825Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGK0VFQCcyLhe9WyITpTMbXiMiOJcBxeel3mS%2B9Ctw80AiEAvnZjDooGe9zAnPtODFz5jTvMixHUd9e5sejHCg0O2j8q%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDFy9DtgIF9UgOjV%2BhCrcA3lIguQ0cuOkUAH1uD0DJjlpCPpBjCmWdCcgs%2FOP79gEfBRWHmGUXG%2FJnj2ubTwpgvQCfB5sv7r5tp419WjMZA1pSAEoHjg9rVZZ0XCGeKXPFVUYDaNZrgk09b3zt9Ced3%2Fq53Zc8YSzJ2%2BMCkm1KP%2F%2BsMTgNCEQJtmc6Mw665vZAlaWfPUwMOKAuv6p6z9vbMvZ03KztfiS7b1YI8Y1UxFgRrR%2FyL7XpPneeoePCJFRStAjlQ4XtN8mEtY9fLSX%2FJBooyyeObwfy1I6mXWu2IJs%2FHXF4A3yX9%2FNyZ5W%2BVgGmlPP18i7M%2B1Om2PLwNRdqW8PqMLxzWbICom4YWDm9j6glIWs1S0yG6p13y9lXPWadZRx7tWsbCNkJZwlyQu0bq0%2F%2FQjI5XxjhTHqOuE01blmpU88bHL3nk3myhFp%2BJkCGTCdWqyQe%2BWJSQdotHBceQ8pU6NAGNJyKMGzhhVT%2FcIdwOtC%2FrjCSGAqc55rS8MowPZOA4%2BjMdFn%2FL5NwRrg%2BXuC%2F0j9kvaoeBDoT%2FnkgvCpzh7CXHTqvK2%2FiF3P9n4oCeCSzRiYIrTgs%2Fup9gKewPakt9ZKEEVqlTDxdCrpHcevV5516orA7qKauCG35%2FOPwOwE76JxytSEpPkFMMKVstUGOqUBf%2FjNo%2BH7AuROBf20hX5NZSgP1JgfxLahLvVSiX3VeU7yvsB2KdKzQU4CTHv2Fy7kab9xUme9S0Q5HxgBzg7SEJwjgVFXhDbfeNlhUefoglQ6yCkU4w4KKZOvYdwXjGZQTC3dHmOcywDant45W5EsIMjcP5ZuHSxvhhdmRZyZ6UDXgfvuY8sq0NDY6EkGf2SGgbROWrGHxELnPpORjuqYPxIF5mSR&X-Amz-Signature=d29fbfef1294e010d82643d0ebcf917e3d3607b53bc70cfc2a5c7dc36b6ebefc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664U5JHXTV%2F20260918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260918T020825Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGK0VFQCcyLhe9WyITpTMbXiMiOJcBxeel3mS%2B9Ctw80AiEAvnZjDooGe9zAnPtODFz5jTvMixHUd9e5sejHCg0O2j8q%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDFy9DtgIF9UgOjV%2BhCrcA3lIguQ0cuOkUAH1uD0DJjlpCPpBjCmWdCcgs%2FOP79gEfBRWHmGUXG%2FJnj2ubTwpgvQCfB5sv7r5tp419WjMZA1pSAEoHjg9rVZZ0XCGeKXPFVUYDaNZrgk09b3zt9Ced3%2Fq53Zc8YSzJ2%2BMCkm1KP%2F%2BsMTgNCEQJtmc6Mw665vZAlaWfPUwMOKAuv6p6z9vbMvZ03KztfiS7b1YI8Y1UxFgRrR%2FyL7XpPneeoePCJFRStAjlQ4XtN8mEtY9fLSX%2FJBooyyeObwfy1I6mXWu2IJs%2FHXF4A3yX9%2FNyZ5W%2BVgGmlPP18i7M%2B1Om2PLwNRdqW8PqMLxzWbICom4YWDm9j6glIWs1S0yG6p13y9lXPWadZRx7tWsbCNkJZwlyQu0bq0%2F%2FQjI5XxjhTHqOuE01blmpU88bHL3nk3myhFp%2BJkCGTCdWqyQe%2BWJSQdotHBceQ8pU6NAGNJyKMGzhhVT%2FcIdwOtC%2FrjCSGAqc55rS8MowPZOA4%2BjMdFn%2FL5NwRrg%2BXuC%2F0j9kvaoeBDoT%2FnkgvCpzh7CXHTqvK2%2FiF3P9n4oCeCSzRiYIrTgs%2Fup9gKewPakt9ZKEEVqlTDxdCrpHcevV5516orA7qKauCG35%2FOPwOwE76JxytSEpPkFMMKVstUGOqUBf%2FjNo%2BH7AuROBf20hX5NZSgP1JgfxLahLvVSiX3VeU7yvsB2KdKzQU4CTHv2Fy7kab9xUme9S0Q5HxgBzg7SEJwjgVFXhDbfeNlhUefoglQ6yCkU4w4KKZOvYdwXjGZQTC3dHmOcywDant45W5EsIMjcP5ZuHSxvhhdmRZyZ6UDXgfvuY8sq0NDY6EkGf2SGgbROWrGHxELnPpORjuqYPxIF5mSR&X-Amz-Signature=48c0e979800a99b65eafcdae223c58b97adcad6a3f02dffc4cf471897af17495&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664U5JHXTV%2F20260918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260918T020826Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGK0VFQCcyLhe9WyITpTMbXiMiOJcBxeel3mS%2B9Ctw80AiEAvnZjDooGe9zAnPtODFz5jTvMixHUd9e5sejHCg0O2j8q%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDFy9DtgIF9UgOjV%2BhCrcA3lIguQ0cuOkUAH1uD0DJjlpCPpBjCmWdCcgs%2FOP79gEfBRWHmGUXG%2FJnj2ubTwpgvQCfB5sv7r5tp419WjMZA1pSAEoHjg9rVZZ0XCGeKXPFVUYDaNZrgk09b3zt9Ced3%2Fq53Zc8YSzJ2%2BMCkm1KP%2F%2BsMTgNCEQJtmc6Mw665vZAlaWfPUwMOKAuv6p6z9vbMvZ03KztfiS7b1YI8Y1UxFgRrR%2FyL7XpPneeoePCJFRStAjlQ4XtN8mEtY9fLSX%2FJBooyyeObwfy1I6mXWu2IJs%2FHXF4A3yX9%2FNyZ5W%2BVgGmlPP18i7M%2B1Om2PLwNRdqW8PqMLxzWbICom4YWDm9j6glIWs1S0yG6p13y9lXPWadZRx7tWsbCNkJZwlyQu0bq0%2F%2FQjI5XxjhTHqOuE01blmpU88bHL3nk3myhFp%2BJkCGTCdWqyQe%2BWJSQdotHBceQ8pU6NAGNJyKMGzhhVT%2FcIdwOtC%2FrjCSGAqc55rS8MowPZOA4%2BjMdFn%2FL5NwRrg%2BXuC%2F0j9kvaoeBDoT%2FnkgvCpzh7CXHTqvK2%2FiF3P9n4oCeCSzRiYIrTgs%2Fup9gKewPakt9ZKEEVqlTDxdCrpHcevV5516orA7qKauCG35%2FOPwOwE76JxytSEpPkFMMKVstUGOqUBf%2FjNo%2BH7AuROBf20hX5NZSgP1JgfxLahLvVSiX3VeU7yvsB2KdKzQU4CTHv2Fy7kab9xUme9S0Q5HxgBzg7SEJwjgVFXhDbfeNlhUefoglQ6yCkU4w4KKZOvYdwXjGZQTC3dHmOcywDant45W5EsIMjcP5ZuHSxvhhdmRZyZ6UDXgfvuY8sq0NDY6EkGf2SGgbROWrGHxELnPpORjuqYPxIF5mSR&X-Amz-Signature=18f86c441993d7ba9d42785fa6dd41012fdc9880f8e3cda829d33025e6e69638&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
