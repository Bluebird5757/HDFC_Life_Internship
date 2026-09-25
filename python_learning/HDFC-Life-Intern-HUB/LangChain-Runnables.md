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
fetched_at: '2026-09-25T02:28:07.208Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS2KTOKJ%2F20260925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260925T022801Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJGMEQCIAdYl0O9jVffyOjdMdS7pOwdt27XLdV3eYwRdRf4GhDBAiB7yMiER9PojJH4cuGD7jG1GTKV9EzdBXK00mcjU4JlTyqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxTRqrjvlgMnnU%2BfZKtwDbt2b6%2FFTfWIglSIsUnqrBuwDfVihnPVow2yRLMIqu3DuYKgwEJDWi3SSqVvQT3Z4Bu%2BCGAiy0KDmHlpWoYAEzozjIbs9e1d3xwFGMAW2qaAme4zo6KmsWIuIacfFYvObU9RXR56Y7qo9joca2OITrvL6CsvfrE779HVI9ODotqWdPVE%2BJkxD%2FlVL14dKkOBvZuhpMuJeg5oQkQSgZZEUpWoep8eVi2OSfylHhyxEsiQqnH3Ghnso6XaQ0gJz1shQ1hVy8NAuiWIz1iXjGOu36pQnen2t99c3Pn4fqpErsP5lkngs4uD4qqeQigBgOPb1A51WN27xQKBX70o1avUG840G%2Fjr991mVcLcIFC5DHC4%2FZv3aysPFDu4b3ZtnNWvsn4M5fYVA2QN6Yq24YilVo3uOtmFlVv62M%2BWfZ0zgQCB3mXwoTgY4RiCQvIomtQEf3We7azHaBoZRrgY7HOMMQubcIZU8%2BmUUrXTGutg0W%2BQ%2BapFtW05RouwXxmhTZKqRnpNErOBx1Y%2B9ZUUvqxgs4Hl7FALRdINLZ2VIUu3EsGyk5kd3hENt2eZ%2Fg4bS0N4tcSpCiax1yXgS3iz2AnV2r2coxgwcz7ztgYNn%2F1ZIVsMRYPHdS0dOSnjRIIkwnfnW1QY6pgFE%2BDPf0FE4oL%2B%2FxilL2Jmp57QJEeLGzsoyDX9Uf5BWqQ6rI6SArjF4QwrJsqwH874QBBNcL4zby44auK6kHrD5Nl4yL22%2FZcImr3UlcrUP3R133CjlSHF0xWz8IpiLop5qzeVqJcirVJbsWn1mSA2Frv2t%2BWP7Fe168l3Mb5GOnvQRUjeKKdc1OeudyRafpv7Rqg6Z7R9HCQrJYkFR9aMsb9MxsR6E&X-Amz-Signature=6aa96559f5d916dc4f465663f6976d41375d8c2fe17f82c80cf355d3d7265fbc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS2KTOKJ%2F20260925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260925T022801Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJGMEQCIAdYl0O9jVffyOjdMdS7pOwdt27XLdV3eYwRdRf4GhDBAiB7yMiER9PojJH4cuGD7jG1GTKV9EzdBXK00mcjU4JlTyqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxTRqrjvlgMnnU%2BfZKtwDbt2b6%2FFTfWIglSIsUnqrBuwDfVihnPVow2yRLMIqu3DuYKgwEJDWi3SSqVvQT3Z4Bu%2BCGAiy0KDmHlpWoYAEzozjIbs9e1d3xwFGMAW2qaAme4zo6KmsWIuIacfFYvObU9RXR56Y7qo9joca2OITrvL6CsvfrE779HVI9ODotqWdPVE%2BJkxD%2FlVL14dKkOBvZuhpMuJeg5oQkQSgZZEUpWoep8eVi2OSfylHhyxEsiQqnH3Ghnso6XaQ0gJz1shQ1hVy8NAuiWIz1iXjGOu36pQnen2t99c3Pn4fqpErsP5lkngs4uD4qqeQigBgOPb1A51WN27xQKBX70o1avUG840G%2Fjr991mVcLcIFC5DHC4%2FZv3aysPFDu4b3ZtnNWvsn4M5fYVA2QN6Yq24YilVo3uOtmFlVv62M%2BWfZ0zgQCB3mXwoTgY4RiCQvIomtQEf3We7azHaBoZRrgY7HOMMQubcIZU8%2BmUUrXTGutg0W%2BQ%2BapFtW05RouwXxmhTZKqRnpNErOBx1Y%2B9ZUUvqxgs4Hl7FALRdINLZ2VIUu3EsGyk5kd3hENt2eZ%2Fg4bS0N4tcSpCiax1yXgS3iz2AnV2r2coxgwcz7ztgYNn%2F1ZIVsMRYPHdS0dOSnjRIIkwnfnW1QY6pgFE%2BDPf0FE4oL%2B%2FxilL2Jmp57QJEeLGzsoyDX9Uf5BWqQ6rI6SArjF4QwrJsqwH874QBBNcL4zby44auK6kHrD5Nl4yL22%2FZcImr3UlcrUP3R133CjlSHF0xWz8IpiLop5qzeVqJcirVJbsWn1mSA2Frv2t%2BWP7Fe168l3Mb5GOnvQRUjeKKdc1OeudyRafpv7Rqg6Z7R9HCQrJYkFR9aMsb9MxsR6E&X-Amz-Signature=f5da09f5247ad996e09bc90ab6e0e3a495097d47c95b96c88764390bc16969cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS2KTOKJ%2F20260925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260925T022801Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJGMEQCIAdYl0O9jVffyOjdMdS7pOwdt27XLdV3eYwRdRf4GhDBAiB7yMiER9PojJH4cuGD7jG1GTKV9EzdBXK00mcjU4JlTyqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxTRqrjvlgMnnU%2BfZKtwDbt2b6%2FFTfWIglSIsUnqrBuwDfVihnPVow2yRLMIqu3DuYKgwEJDWi3SSqVvQT3Z4Bu%2BCGAiy0KDmHlpWoYAEzozjIbs9e1d3xwFGMAW2qaAme4zo6KmsWIuIacfFYvObU9RXR56Y7qo9joca2OITrvL6CsvfrE779HVI9ODotqWdPVE%2BJkxD%2FlVL14dKkOBvZuhpMuJeg5oQkQSgZZEUpWoep8eVi2OSfylHhyxEsiQqnH3Ghnso6XaQ0gJz1shQ1hVy8NAuiWIz1iXjGOu36pQnen2t99c3Pn4fqpErsP5lkngs4uD4qqeQigBgOPb1A51WN27xQKBX70o1avUG840G%2Fjr991mVcLcIFC5DHC4%2FZv3aysPFDu4b3ZtnNWvsn4M5fYVA2QN6Yq24YilVo3uOtmFlVv62M%2BWfZ0zgQCB3mXwoTgY4RiCQvIomtQEf3We7azHaBoZRrgY7HOMMQubcIZU8%2BmUUrXTGutg0W%2BQ%2BapFtW05RouwXxmhTZKqRnpNErOBx1Y%2B9ZUUvqxgs4Hl7FALRdINLZ2VIUu3EsGyk5kd3hENt2eZ%2Fg4bS0N4tcSpCiax1yXgS3iz2AnV2r2coxgwcz7ztgYNn%2F1ZIVsMRYPHdS0dOSnjRIIkwnfnW1QY6pgFE%2BDPf0FE4oL%2B%2FxilL2Jmp57QJEeLGzsoyDX9Uf5BWqQ6rI6SArjF4QwrJsqwH874QBBNcL4zby44auK6kHrD5Nl4yL22%2FZcImr3UlcrUP3R133CjlSHF0xWz8IpiLop5qzeVqJcirVJbsWn1mSA2Frv2t%2BWP7Fe168l3Mb5GOnvQRUjeKKdc1OeudyRafpv7Rqg6Z7R9HCQrJYkFR9aMsb9MxsR6E&X-Amz-Signature=6f31fabc5763207e1ab3a020899e98f55b8f92cd5f9147df1daafd45a5568711&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS2KTOKJ%2F20260925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260925T022801Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJGMEQCIAdYl0O9jVffyOjdMdS7pOwdt27XLdV3eYwRdRf4GhDBAiB7yMiER9PojJH4cuGD7jG1GTKV9EzdBXK00mcjU4JlTyqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxTRqrjvlgMnnU%2BfZKtwDbt2b6%2FFTfWIglSIsUnqrBuwDfVihnPVow2yRLMIqu3DuYKgwEJDWi3SSqVvQT3Z4Bu%2BCGAiy0KDmHlpWoYAEzozjIbs9e1d3xwFGMAW2qaAme4zo6KmsWIuIacfFYvObU9RXR56Y7qo9joca2OITrvL6CsvfrE779HVI9ODotqWdPVE%2BJkxD%2FlVL14dKkOBvZuhpMuJeg5oQkQSgZZEUpWoep8eVi2OSfylHhyxEsiQqnH3Ghnso6XaQ0gJz1shQ1hVy8NAuiWIz1iXjGOu36pQnen2t99c3Pn4fqpErsP5lkngs4uD4qqeQigBgOPb1A51WN27xQKBX70o1avUG840G%2Fjr991mVcLcIFC5DHC4%2FZv3aysPFDu4b3ZtnNWvsn4M5fYVA2QN6Yq24YilVo3uOtmFlVv62M%2BWfZ0zgQCB3mXwoTgY4RiCQvIomtQEf3We7azHaBoZRrgY7HOMMQubcIZU8%2BmUUrXTGutg0W%2BQ%2BapFtW05RouwXxmhTZKqRnpNErOBx1Y%2B9ZUUvqxgs4Hl7FALRdINLZ2VIUu3EsGyk5kd3hENt2eZ%2Fg4bS0N4tcSpCiax1yXgS3iz2AnV2r2coxgwcz7ztgYNn%2F1ZIVsMRYPHdS0dOSnjRIIkwnfnW1QY6pgFE%2BDPf0FE4oL%2B%2FxilL2Jmp57QJEeLGzsoyDX9Uf5BWqQ6rI6SArjF4QwrJsqwH874QBBNcL4zby44auK6kHrD5Nl4yL22%2FZcImr3UlcrUP3R133CjlSHF0xWz8IpiLop5qzeVqJcirVJbsWn1mSA2Frv2t%2BWP7Fe168l3Mb5GOnvQRUjeKKdc1OeudyRafpv7Rqg6Z7R9HCQrJYkFR9aMsb9MxsR6E&X-Amz-Signature=df22e7d770b04ec76bd86c1067362737e71b3a4218f2d0cc7174ccf7586095f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS2KTOKJ%2F20260925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260925T022801Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJGMEQCIAdYl0O9jVffyOjdMdS7pOwdt27XLdV3eYwRdRf4GhDBAiB7yMiER9PojJH4cuGD7jG1GTKV9EzdBXK00mcjU4JlTyqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxTRqrjvlgMnnU%2BfZKtwDbt2b6%2FFTfWIglSIsUnqrBuwDfVihnPVow2yRLMIqu3DuYKgwEJDWi3SSqVvQT3Z4Bu%2BCGAiy0KDmHlpWoYAEzozjIbs9e1d3xwFGMAW2qaAme4zo6KmsWIuIacfFYvObU9RXR56Y7qo9joca2OITrvL6CsvfrE779HVI9ODotqWdPVE%2BJkxD%2FlVL14dKkOBvZuhpMuJeg5oQkQSgZZEUpWoep8eVi2OSfylHhyxEsiQqnH3Ghnso6XaQ0gJz1shQ1hVy8NAuiWIz1iXjGOu36pQnen2t99c3Pn4fqpErsP5lkngs4uD4qqeQigBgOPb1A51WN27xQKBX70o1avUG840G%2Fjr991mVcLcIFC5DHC4%2FZv3aysPFDu4b3ZtnNWvsn4M5fYVA2QN6Yq24YilVo3uOtmFlVv62M%2BWfZ0zgQCB3mXwoTgY4RiCQvIomtQEf3We7azHaBoZRrgY7HOMMQubcIZU8%2BmUUrXTGutg0W%2BQ%2BapFtW05RouwXxmhTZKqRnpNErOBx1Y%2B9ZUUvqxgs4Hl7FALRdINLZ2VIUu3EsGyk5kd3hENt2eZ%2Fg4bS0N4tcSpCiax1yXgS3iz2AnV2r2coxgwcz7ztgYNn%2F1ZIVsMRYPHdS0dOSnjRIIkwnfnW1QY6pgFE%2BDPf0FE4oL%2B%2FxilL2Jmp57QJEeLGzsoyDX9Uf5BWqQ6rI6SArjF4QwrJsqwH874QBBNcL4zby44auK6kHrD5Nl4yL22%2FZcImr3UlcrUP3R133CjlSHF0xWz8IpiLop5qzeVqJcirVJbsWn1mSA2Frv2t%2BWP7Fe168l3Mb5GOnvQRUjeKKdc1OeudyRafpv7Rqg6Z7R9HCQrJYkFR9aMsb9MxsR6E&X-Amz-Signature=876b7750557b0c2b568f488971d07c8254a09c1df05ee059207c0aa1b09eef9a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS2KTOKJ%2F20260925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260925T022801Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJGMEQCIAdYl0O9jVffyOjdMdS7pOwdt27XLdV3eYwRdRf4GhDBAiB7yMiER9PojJH4cuGD7jG1GTKV9EzdBXK00mcjU4JlTyqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxTRqrjvlgMnnU%2BfZKtwDbt2b6%2FFTfWIglSIsUnqrBuwDfVihnPVow2yRLMIqu3DuYKgwEJDWi3SSqVvQT3Z4Bu%2BCGAiy0KDmHlpWoYAEzozjIbs9e1d3xwFGMAW2qaAme4zo6KmsWIuIacfFYvObU9RXR56Y7qo9joca2OITrvL6CsvfrE779HVI9ODotqWdPVE%2BJkxD%2FlVL14dKkOBvZuhpMuJeg5oQkQSgZZEUpWoep8eVi2OSfylHhyxEsiQqnH3Ghnso6XaQ0gJz1shQ1hVy8NAuiWIz1iXjGOu36pQnen2t99c3Pn4fqpErsP5lkngs4uD4qqeQigBgOPb1A51WN27xQKBX70o1avUG840G%2Fjr991mVcLcIFC5DHC4%2FZv3aysPFDu4b3ZtnNWvsn4M5fYVA2QN6Yq24YilVo3uOtmFlVv62M%2BWfZ0zgQCB3mXwoTgY4RiCQvIomtQEf3We7azHaBoZRrgY7HOMMQubcIZU8%2BmUUrXTGutg0W%2BQ%2BapFtW05RouwXxmhTZKqRnpNErOBx1Y%2B9ZUUvqxgs4Hl7FALRdINLZ2VIUu3EsGyk5kd3hENt2eZ%2Fg4bS0N4tcSpCiax1yXgS3iz2AnV2r2coxgwcz7ztgYNn%2F1ZIVsMRYPHdS0dOSnjRIIkwnfnW1QY6pgFE%2BDPf0FE4oL%2B%2FxilL2Jmp57QJEeLGzsoyDX9Uf5BWqQ6rI6SArjF4QwrJsqwH874QBBNcL4zby44auK6kHrD5Nl4yL22%2FZcImr3UlcrUP3R133CjlSHF0xWz8IpiLop5qzeVqJcirVJbsWn1mSA2Frv2t%2BWP7Fe168l3Mb5GOnvQRUjeKKdc1OeudyRafpv7Rqg6Z7R9HCQrJYkFR9aMsb9MxsR6E&X-Amz-Signature=bf41bb68ade9abdf2842367d90fc8c401e95c474e0e12828327d1fe3e4034b70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS2KTOKJ%2F20260925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260925T022801Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJGMEQCIAdYl0O9jVffyOjdMdS7pOwdt27XLdV3eYwRdRf4GhDBAiB7yMiER9PojJH4cuGD7jG1GTKV9EzdBXK00mcjU4JlTyqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxTRqrjvlgMnnU%2BfZKtwDbt2b6%2FFTfWIglSIsUnqrBuwDfVihnPVow2yRLMIqu3DuYKgwEJDWi3SSqVvQT3Z4Bu%2BCGAiy0KDmHlpWoYAEzozjIbs9e1d3xwFGMAW2qaAme4zo6KmsWIuIacfFYvObU9RXR56Y7qo9joca2OITrvL6CsvfrE779HVI9ODotqWdPVE%2BJkxD%2FlVL14dKkOBvZuhpMuJeg5oQkQSgZZEUpWoep8eVi2OSfylHhyxEsiQqnH3Ghnso6XaQ0gJz1shQ1hVy8NAuiWIz1iXjGOu36pQnen2t99c3Pn4fqpErsP5lkngs4uD4qqeQigBgOPb1A51WN27xQKBX70o1avUG840G%2Fjr991mVcLcIFC5DHC4%2FZv3aysPFDu4b3ZtnNWvsn4M5fYVA2QN6Yq24YilVo3uOtmFlVv62M%2BWfZ0zgQCB3mXwoTgY4RiCQvIomtQEf3We7azHaBoZRrgY7HOMMQubcIZU8%2BmUUrXTGutg0W%2BQ%2BapFtW05RouwXxmhTZKqRnpNErOBx1Y%2B9ZUUvqxgs4Hl7FALRdINLZ2VIUu3EsGyk5kd3hENt2eZ%2Fg4bS0N4tcSpCiax1yXgS3iz2AnV2r2coxgwcz7ztgYNn%2F1ZIVsMRYPHdS0dOSnjRIIkwnfnW1QY6pgFE%2BDPf0FE4oL%2B%2FxilL2Jmp57QJEeLGzsoyDX9Uf5BWqQ6rI6SArjF4QwrJsqwH874QBBNcL4zby44auK6kHrD5Nl4yL22%2FZcImr3UlcrUP3R133CjlSHF0xWz8IpiLop5qzeVqJcirVJbsWn1mSA2Frv2t%2BWP7Fe168l3Mb5GOnvQRUjeKKdc1OeudyRafpv7Rqg6Z7R9HCQrJYkFR9aMsb9MxsR6E&X-Amz-Signature=7bedbf644c090b5144f6d2676c3f9d1b7b4e624d8b247de2c82ee567e3e42b7c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS2KTOKJ%2F20260925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260925T022801Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJGMEQCIAdYl0O9jVffyOjdMdS7pOwdt27XLdV3eYwRdRf4GhDBAiB7yMiER9PojJH4cuGD7jG1GTKV9EzdBXK00mcjU4JlTyqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxTRqrjvlgMnnU%2BfZKtwDbt2b6%2FFTfWIglSIsUnqrBuwDfVihnPVow2yRLMIqu3DuYKgwEJDWi3SSqVvQT3Z4Bu%2BCGAiy0KDmHlpWoYAEzozjIbs9e1d3xwFGMAW2qaAme4zo6KmsWIuIacfFYvObU9RXR56Y7qo9joca2OITrvL6CsvfrE779HVI9ODotqWdPVE%2BJkxD%2FlVL14dKkOBvZuhpMuJeg5oQkQSgZZEUpWoep8eVi2OSfylHhyxEsiQqnH3Ghnso6XaQ0gJz1shQ1hVy8NAuiWIz1iXjGOu36pQnen2t99c3Pn4fqpErsP5lkngs4uD4qqeQigBgOPb1A51WN27xQKBX70o1avUG840G%2Fjr991mVcLcIFC5DHC4%2FZv3aysPFDu4b3ZtnNWvsn4M5fYVA2QN6Yq24YilVo3uOtmFlVv62M%2BWfZ0zgQCB3mXwoTgY4RiCQvIomtQEf3We7azHaBoZRrgY7HOMMQubcIZU8%2BmUUrXTGutg0W%2BQ%2BapFtW05RouwXxmhTZKqRnpNErOBx1Y%2B9ZUUvqxgs4Hl7FALRdINLZ2VIUu3EsGyk5kd3hENt2eZ%2Fg4bS0N4tcSpCiax1yXgS3iz2AnV2r2coxgwcz7ztgYNn%2F1ZIVsMRYPHdS0dOSnjRIIkwnfnW1QY6pgFE%2BDPf0FE4oL%2B%2FxilL2Jmp57QJEeLGzsoyDX9Uf5BWqQ6rI6SArjF4QwrJsqwH874QBBNcL4zby44auK6kHrD5Nl4yL22%2FZcImr3UlcrUP3R133CjlSHF0xWz8IpiLop5qzeVqJcirVJbsWn1mSA2Frv2t%2BWP7Fe168l3Mb5GOnvQRUjeKKdc1OeudyRafpv7Rqg6Z7R9HCQrJYkFR9aMsb9MxsR6E&X-Amz-Signature=495896efc44a099e2bb11a6137252cf269a944f4921223d3da8332e2f4069058&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS2KTOKJ%2F20260925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260925T022802Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJGMEQCIAdYl0O9jVffyOjdMdS7pOwdt27XLdV3eYwRdRf4GhDBAiB7yMiER9PojJH4cuGD7jG1GTKV9EzdBXK00mcjU4JlTyqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxTRqrjvlgMnnU%2BfZKtwDbt2b6%2FFTfWIglSIsUnqrBuwDfVihnPVow2yRLMIqu3DuYKgwEJDWi3SSqVvQT3Z4Bu%2BCGAiy0KDmHlpWoYAEzozjIbs9e1d3xwFGMAW2qaAme4zo6KmsWIuIacfFYvObU9RXR56Y7qo9joca2OITrvL6CsvfrE779HVI9ODotqWdPVE%2BJkxD%2FlVL14dKkOBvZuhpMuJeg5oQkQSgZZEUpWoep8eVi2OSfylHhyxEsiQqnH3Ghnso6XaQ0gJz1shQ1hVy8NAuiWIz1iXjGOu36pQnen2t99c3Pn4fqpErsP5lkngs4uD4qqeQigBgOPb1A51WN27xQKBX70o1avUG840G%2Fjr991mVcLcIFC5DHC4%2FZv3aysPFDu4b3ZtnNWvsn4M5fYVA2QN6Yq24YilVo3uOtmFlVv62M%2BWfZ0zgQCB3mXwoTgY4RiCQvIomtQEf3We7azHaBoZRrgY7HOMMQubcIZU8%2BmUUrXTGutg0W%2BQ%2BapFtW05RouwXxmhTZKqRnpNErOBx1Y%2B9ZUUvqxgs4Hl7FALRdINLZ2VIUu3EsGyk5kd3hENt2eZ%2Fg4bS0N4tcSpCiax1yXgS3iz2AnV2r2coxgwcz7ztgYNn%2F1ZIVsMRYPHdS0dOSnjRIIkwnfnW1QY6pgFE%2BDPf0FE4oL%2B%2FxilL2Jmp57QJEeLGzsoyDX9Uf5BWqQ6rI6SArjF4QwrJsqwH874QBBNcL4zby44auK6kHrD5Nl4yL22%2FZcImr3UlcrUP3R133CjlSHF0xWz8IpiLop5qzeVqJcirVJbsWn1mSA2Frv2t%2BWP7Fe168l3Mb5GOnvQRUjeKKdc1OeudyRafpv7Rqg6Z7R9HCQrJYkFR9aMsb9MxsR6E&X-Amz-Signature=afd63ecf123058df9e0fd7fed0026b63ca79f38e666594a60e7000b6a0ea6a77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
