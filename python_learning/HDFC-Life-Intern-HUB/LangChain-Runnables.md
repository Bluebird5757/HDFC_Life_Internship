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
fetched_at: '2026-11-02T03:21:09.659Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI3GTNS3%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkPn6CCtWah%2FnpblUs6V4MWKsXrazEPntmjk6v4XyADAIhAJ8puzZvCYVjOZYYteNHxHEHP%2F90ppWKVWgBkeoT9YLkKv8DCHQQABoMNjM3NDIzMTgzODA1IgxvpVnpaOTQGbinWMwq3APJ18AditRhVkeL8jpvQGJ15Q4xsqdMVik63LB0P%2F26DUwqKhL2FXFKlTS6shEMh%2F4iDU7vRlVFjHo16nTXKQHLCFDeoLegeYwsj%2B67CM6mjWVW7QKEJaLXsn9xRCZ%2BIzMYw4rcESsGQxCxQmmZTv6jzF8hk6IIDy%2FaWi6L6FzEFOGNtZOECKmTO7tKiX4d9TkhAcEhpx8IzxzFuVHpvv0YoJwSD001j089y%2BF%2BW9DqNMvFBcPeJWjYIrM6ypYJh%2FGpeJbe%2F%2Bfru3qVajJ%2FivOte7q7eWlIQu14pXXHfqXvsIDkqXWwTsA7R2L6hWou2If1SN%2BOOVWoFP%2FoAPUCvABE0dunL2YyP7gcwSEUfdP9j%2BXHjOehCTN0nGs29PA5rPQRd8To8lKlAMb78xhoaysbrCwdDVd%2BWp7dgbQUg7i5eKeVzpK2%2FU0l3rdr7FmzY1xthHUAh4ktmN33dimN4pepL4HszCFZ8t%2FUH2hpf9f395hDY%2BvjC%2FSKFwzsyZ%2B%2F4IiRbshztQspVr8d65h662S1YW5KBeToq4y6%2F84AU10zalqfXuR3zSfghUKhmcFwTrV%2BgFekvODiJmbHbMsMMSMYxv9P3OaeHAC91boC22DlpzeRoRw84Frj13V5QjDG%2BJ%2FXBjqkAct9yU%2FUy6J7n0qO%2B4nA7tfv8ZrRHBmEPQX5CdMhEwB90DVYk%2FVY3Ur1zeyq6kkuLddR4%2FTk87EuTz8OfEB2tqO%2B%2Fr7OBYXoJ%2B9uEZB0Nz6rxKQFkuybci8zTugWF%2FYFvoKCneNRyS5hhj%2F6fz0BvMnsJouoxcnUsvlpqvjgYUya76%2FsDIVgX4kWppQldgkEHoO8tDcCXew11hzOYx9r6e%2BBOjZ3&X-Amz-Signature=0a5298054a16337b8ea5dbaab48ba4fc6487a7bdcef8d90b64fbe1d91caf4184&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI3GTNS3%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkPn6CCtWah%2FnpblUs6V4MWKsXrazEPntmjk6v4XyADAIhAJ8puzZvCYVjOZYYteNHxHEHP%2F90ppWKVWgBkeoT9YLkKv8DCHQQABoMNjM3NDIzMTgzODA1IgxvpVnpaOTQGbinWMwq3APJ18AditRhVkeL8jpvQGJ15Q4xsqdMVik63LB0P%2F26DUwqKhL2FXFKlTS6shEMh%2F4iDU7vRlVFjHo16nTXKQHLCFDeoLegeYwsj%2B67CM6mjWVW7QKEJaLXsn9xRCZ%2BIzMYw4rcESsGQxCxQmmZTv6jzF8hk6IIDy%2FaWi6L6FzEFOGNtZOECKmTO7tKiX4d9TkhAcEhpx8IzxzFuVHpvv0YoJwSD001j089y%2BF%2BW9DqNMvFBcPeJWjYIrM6ypYJh%2FGpeJbe%2F%2Bfru3qVajJ%2FivOte7q7eWlIQu14pXXHfqXvsIDkqXWwTsA7R2L6hWou2If1SN%2BOOVWoFP%2FoAPUCvABE0dunL2YyP7gcwSEUfdP9j%2BXHjOehCTN0nGs29PA5rPQRd8To8lKlAMb78xhoaysbrCwdDVd%2BWp7dgbQUg7i5eKeVzpK2%2FU0l3rdr7FmzY1xthHUAh4ktmN33dimN4pepL4HszCFZ8t%2FUH2hpf9f395hDY%2BvjC%2FSKFwzsyZ%2B%2F4IiRbshztQspVr8d65h662S1YW5KBeToq4y6%2F84AU10zalqfXuR3zSfghUKhmcFwTrV%2BgFekvODiJmbHbMsMMSMYxv9P3OaeHAC91boC22DlpzeRoRw84Frj13V5QjDG%2BJ%2FXBjqkAct9yU%2FUy6J7n0qO%2B4nA7tfv8ZrRHBmEPQX5CdMhEwB90DVYk%2FVY3Ur1zeyq6kkuLddR4%2FTk87EuTz8OfEB2tqO%2B%2Fr7OBYXoJ%2B9uEZB0Nz6rxKQFkuybci8zTugWF%2FYFvoKCneNRyS5hhj%2F6fz0BvMnsJouoxcnUsvlpqvjgYUya76%2FsDIVgX4kWppQldgkEHoO8tDcCXew11hzOYx9r6e%2BBOjZ3&X-Amz-Signature=c582714f13b31e10b22d997985d416187369ddd637040d8631f94b3dd1bb8ce1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI3GTNS3%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkPn6CCtWah%2FnpblUs6V4MWKsXrazEPntmjk6v4XyADAIhAJ8puzZvCYVjOZYYteNHxHEHP%2F90ppWKVWgBkeoT9YLkKv8DCHQQABoMNjM3NDIzMTgzODA1IgxvpVnpaOTQGbinWMwq3APJ18AditRhVkeL8jpvQGJ15Q4xsqdMVik63LB0P%2F26DUwqKhL2FXFKlTS6shEMh%2F4iDU7vRlVFjHo16nTXKQHLCFDeoLegeYwsj%2B67CM6mjWVW7QKEJaLXsn9xRCZ%2BIzMYw4rcESsGQxCxQmmZTv6jzF8hk6IIDy%2FaWi6L6FzEFOGNtZOECKmTO7tKiX4d9TkhAcEhpx8IzxzFuVHpvv0YoJwSD001j089y%2BF%2BW9DqNMvFBcPeJWjYIrM6ypYJh%2FGpeJbe%2F%2Bfru3qVajJ%2FivOte7q7eWlIQu14pXXHfqXvsIDkqXWwTsA7R2L6hWou2If1SN%2BOOVWoFP%2FoAPUCvABE0dunL2YyP7gcwSEUfdP9j%2BXHjOehCTN0nGs29PA5rPQRd8To8lKlAMb78xhoaysbrCwdDVd%2BWp7dgbQUg7i5eKeVzpK2%2FU0l3rdr7FmzY1xthHUAh4ktmN33dimN4pepL4HszCFZ8t%2FUH2hpf9f395hDY%2BvjC%2FSKFwzsyZ%2B%2F4IiRbshztQspVr8d65h662S1YW5KBeToq4y6%2F84AU10zalqfXuR3zSfghUKhmcFwTrV%2BgFekvODiJmbHbMsMMSMYxv9P3OaeHAC91boC22DlpzeRoRw84Frj13V5QjDG%2BJ%2FXBjqkAct9yU%2FUy6J7n0qO%2B4nA7tfv8ZrRHBmEPQX5CdMhEwB90DVYk%2FVY3Ur1zeyq6kkuLddR4%2FTk87EuTz8OfEB2tqO%2B%2Fr7OBYXoJ%2B9uEZB0Nz6rxKQFkuybci8zTugWF%2FYFvoKCneNRyS5hhj%2F6fz0BvMnsJouoxcnUsvlpqvjgYUya76%2FsDIVgX4kWppQldgkEHoO8tDcCXew11hzOYx9r6e%2BBOjZ3&X-Amz-Signature=72f854636de102bfe1007a3a431873f0a351c1be3f37e5528b4ac9d0c2c2cf55&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI3GTNS3%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkPn6CCtWah%2FnpblUs6V4MWKsXrazEPntmjk6v4XyADAIhAJ8puzZvCYVjOZYYteNHxHEHP%2F90ppWKVWgBkeoT9YLkKv8DCHQQABoMNjM3NDIzMTgzODA1IgxvpVnpaOTQGbinWMwq3APJ18AditRhVkeL8jpvQGJ15Q4xsqdMVik63LB0P%2F26DUwqKhL2FXFKlTS6shEMh%2F4iDU7vRlVFjHo16nTXKQHLCFDeoLegeYwsj%2B67CM6mjWVW7QKEJaLXsn9xRCZ%2BIzMYw4rcESsGQxCxQmmZTv6jzF8hk6IIDy%2FaWi6L6FzEFOGNtZOECKmTO7tKiX4d9TkhAcEhpx8IzxzFuVHpvv0YoJwSD001j089y%2BF%2BW9DqNMvFBcPeJWjYIrM6ypYJh%2FGpeJbe%2F%2Bfru3qVajJ%2FivOte7q7eWlIQu14pXXHfqXvsIDkqXWwTsA7R2L6hWou2If1SN%2BOOVWoFP%2FoAPUCvABE0dunL2YyP7gcwSEUfdP9j%2BXHjOehCTN0nGs29PA5rPQRd8To8lKlAMb78xhoaysbrCwdDVd%2BWp7dgbQUg7i5eKeVzpK2%2FU0l3rdr7FmzY1xthHUAh4ktmN33dimN4pepL4HszCFZ8t%2FUH2hpf9f395hDY%2BvjC%2FSKFwzsyZ%2B%2F4IiRbshztQspVr8d65h662S1YW5KBeToq4y6%2F84AU10zalqfXuR3zSfghUKhmcFwTrV%2BgFekvODiJmbHbMsMMSMYxv9P3OaeHAC91boC22DlpzeRoRw84Frj13V5QjDG%2BJ%2FXBjqkAct9yU%2FUy6J7n0qO%2B4nA7tfv8ZrRHBmEPQX5CdMhEwB90DVYk%2FVY3Ur1zeyq6kkuLddR4%2FTk87EuTz8OfEB2tqO%2B%2Fr7OBYXoJ%2B9uEZB0Nz6rxKQFkuybci8zTugWF%2FYFvoKCneNRyS5hhj%2F6fz0BvMnsJouoxcnUsvlpqvjgYUya76%2FsDIVgX4kWppQldgkEHoO8tDcCXew11hzOYx9r6e%2BBOjZ3&X-Amz-Signature=5c55581d0655e13ebdc360e0381ee41c5e0f0d3d5baaec071d93127b07a3d0e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI3GTNS3%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkPn6CCtWah%2FnpblUs6V4MWKsXrazEPntmjk6v4XyADAIhAJ8puzZvCYVjOZYYteNHxHEHP%2F90ppWKVWgBkeoT9YLkKv8DCHQQABoMNjM3NDIzMTgzODA1IgxvpVnpaOTQGbinWMwq3APJ18AditRhVkeL8jpvQGJ15Q4xsqdMVik63LB0P%2F26DUwqKhL2FXFKlTS6shEMh%2F4iDU7vRlVFjHo16nTXKQHLCFDeoLegeYwsj%2B67CM6mjWVW7QKEJaLXsn9xRCZ%2BIzMYw4rcESsGQxCxQmmZTv6jzF8hk6IIDy%2FaWi6L6FzEFOGNtZOECKmTO7tKiX4d9TkhAcEhpx8IzxzFuVHpvv0YoJwSD001j089y%2BF%2BW9DqNMvFBcPeJWjYIrM6ypYJh%2FGpeJbe%2F%2Bfru3qVajJ%2FivOte7q7eWlIQu14pXXHfqXvsIDkqXWwTsA7R2L6hWou2If1SN%2BOOVWoFP%2FoAPUCvABE0dunL2YyP7gcwSEUfdP9j%2BXHjOehCTN0nGs29PA5rPQRd8To8lKlAMb78xhoaysbrCwdDVd%2BWp7dgbQUg7i5eKeVzpK2%2FU0l3rdr7FmzY1xthHUAh4ktmN33dimN4pepL4HszCFZ8t%2FUH2hpf9f395hDY%2BvjC%2FSKFwzsyZ%2B%2F4IiRbshztQspVr8d65h662S1YW5KBeToq4y6%2F84AU10zalqfXuR3zSfghUKhmcFwTrV%2BgFekvODiJmbHbMsMMSMYxv9P3OaeHAC91boC22DlpzeRoRw84Frj13V5QjDG%2BJ%2FXBjqkAct9yU%2FUy6J7n0qO%2B4nA7tfv8ZrRHBmEPQX5CdMhEwB90DVYk%2FVY3Ur1zeyq6kkuLddR4%2FTk87EuTz8OfEB2tqO%2B%2Fr7OBYXoJ%2B9uEZB0Nz6rxKQFkuybci8zTugWF%2FYFvoKCneNRyS5hhj%2F6fz0BvMnsJouoxcnUsvlpqvjgYUya76%2FsDIVgX4kWppQldgkEHoO8tDcCXew11hzOYx9r6e%2BBOjZ3&X-Amz-Signature=d95708963a010df7c623f6ccf4bd4a481ef23d065fffaad129d4874008d6ea37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI3GTNS3%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkPn6CCtWah%2FnpblUs6V4MWKsXrazEPntmjk6v4XyADAIhAJ8puzZvCYVjOZYYteNHxHEHP%2F90ppWKVWgBkeoT9YLkKv8DCHQQABoMNjM3NDIzMTgzODA1IgxvpVnpaOTQGbinWMwq3APJ18AditRhVkeL8jpvQGJ15Q4xsqdMVik63LB0P%2F26DUwqKhL2FXFKlTS6shEMh%2F4iDU7vRlVFjHo16nTXKQHLCFDeoLegeYwsj%2B67CM6mjWVW7QKEJaLXsn9xRCZ%2BIzMYw4rcESsGQxCxQmmZTv6jzF8hk6IIDy%2FaWi6L6FzEFOGNtZOECKmTO7tKiX4d9TkhAcEhpx8IzxzFuVHpvv0YoJwSD001j089y%2BF%2BW9DqNMvFBcPeJWjYIrM6ypYJh%2FGpeJbe%2F%2Bfru3qVajJ%2FivOte7q7eWlIQu14pXXHfqXvsIDkqXWwTsA7R2L6hWou2If1SN%2BOOVWoFP%2FoAPUCvABE0dunL2YyP7gcwSEUfdP9j%2BXHjOehCTN0nGs29PA5rPQRd8To8lKlAMb78xhoaysbrCwdDVd%2BWp7dgbQUg7i5eKeVzpK2%2FU0l3rdr7FmzY1xthHUAh4ktmN33dimN4pepL4HszCFZ8t%2FUH2hpf9f395hDY%2BvjC%2FSKFwzsyZ%2B%2F4IiRbshztQspVr8d65h662S1YW5KBeToq4y6%2F84AU10zalqfXuR3zSfghUKhmcFwTrV%2BgFekvODiJmbHbMsMMSMYxv9P3OaeHAC91boC22DlpzeRoRw84Frj13V5QjDG%2BJ%2FXBjqkAct9yU%2FUy6J7n0qO%2B4nA7tfv8ZrRHBmEPQX5CdMhEwB90DVYk%2FVY3Ur1zeyq6kkuLddR4%2FTk87EuTz8OfEB2tqO%2B%2Fr7OBYXoJ%2B9uEZB0Nz6rxKQFkuybci8zTugWF%2FYFvoKCneNRyS5hhj%2F6fz0BvMnsJouoxcnUsvlpqvjgYUya76%2FsDIVgX4kWppQldgkEHoO8tDcCXew11hzOYx9r6e%2BBOjZ3&X-Amz-Signature=bce05a353c551df8cf577766797daad21179cce852b9289a493acd31adc645ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI3GTNS3%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkPn6CCtWah%2FnpblUs6V4MWKsXrazEPntmjk6v4XyADAIhAJ8puzZvCYVjOZYYteNHxHEHP%2F90ppWKVWgBkeoT9YLkKv8DCHQQABoMNjM3NDIzMTgzODA1IgxvpVnpaOTQGbinWMwq3APJ18AditRhVkeL8jpvQGJ15Q4xsqdMVik63LB0P%2F26DUwqKhL2FXFKlTS6shEMh%2F4iDU7vRlVFjHo16nTXKQHLCFDeoLegeYwsj%2B67CM6mjWVW7QKEJaLXsn9xRCZ%2BIzMYw4rcESsGQxCxQmmZTv6jzF8hk6IIDy%2FaWi6L6FzEFOGNtZOECKmTO7tKiX4d9TkhAcEhpx8IzxzFuVHpvv0YoJwSD001j089y%2BF%2BW9DqNMvFBcPeJWjYIrM6ypYJh%2FGpeJbe%2F%2Bfru3qVajJ%2FivOte7q7eWlIQu14pXXHfqXvsIDkqXWwTsA7R2L6hWou2If1SN%2BOOVWoFP%2FoAPUCvABE0dunL2YyP7gcwSEUfdP9j%2BXHjOehCTN0nGs29PA5rPQRd8To8lKlAMb78xhoaysbrCwdDVd%2BWp7dgbQUg7i5eKeVzpK2%2FU0l3rdr7FmzY1xthHUAh4ktmN33dimN4pepL4HszCFZ8t%2FUH2hpf9f395hDY%2BvjC%2FSKFwzsyZ%2B%2F4IiRbshztQspVr8d65h662S1YW5KBeToq4y6%2F84AU10zalqfXuR3zSfghUKhmcFwTrV%2BgFekvODiJmbHbMsMMSMYxv9P3OaeHAC91boC22DlpzeRoRw84Frj13V5QjDG%2BJ%2FXBjqkAct9yU%2FUy6J7n0qO%2B4nA7tfv8ZrRHBmEPQX5CdMhEwB90DVYk%2FVY3Ur1zeyq6kkuLddR4%2FTk87EuTz8OfEB2tqO%2B%2Fr7OBYXoJ%2B9uEZB0Nz6rxKQFkuybci8zTugWF%2FYFvoKCneNRyS5hhj%2F6fz0BvMnsJouoxcnUsvlpqvjgYUya76%2FsDIVgX4kWppQldgkEHoO8tDcCXew11hzOYx9r6e%2BBOjZ3&X-Amz-Signature=ea88538b1f575fd2c03a7360e20bcae29a2311d81e29b745005f391844ee1ac9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI3GTNS3%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkPn6CCtWah%2FnpblUs6V4MWKsXrazEPntmjk6v4XyADAIhAJ8puzZvCYVjOZYYteNHxHEHP%2F90ppWKVWgBkeoT9YLkKv8DCHQQABoMNjM3NDIzMTgzODA1IgxvpVnpaOTQGbinWMwq3APJ18AditRhVkeL8jpvQGJ15Q4xsqdMVik63LB0P%2F26DUwqKhL2FXFKlTS6shEMh%2F4iDU7vRlVFjHo16nTXKQHLCFDeoLegeYwsj%2B67CM6mjWVW7QKEJaLXsn9xRCZ%2BIzMYw4rcESsGQxCxQmmZTv6jzF8hk6IIDy%2FaWi6L6FzEFOGNtZOECKmTO7tKiX4d9TkhAcEhpx8IzxzFuVHpvv0YoJwSD001j089y%2BF%2BW9DqNMvFBcPeJWjYIrM6ypYJh%2FGpeJbe%2F%2Bfru3qVajJ%2FivOte7q7eWlIQu14pXXHfqXvsIDkqXWwTsA7R2L6hWou2If1SN%2BOOVWoFP%2FoAPUCvABE0dunL2YyP7gcwSEUfdP9j%2BXHjOehCTN0nGs29PA5rPQRd8To8lKlAMb78xhoaysbrCwdDVd%2BWp7dgbQUg7i5eKeVzpK2%2FU0l3rdr7FmzY1xthHUAh4ktmN33dimN4pepL4HszCFZ8t%2FUH2hpf9f395hDY%2BvjC%2FSKFwzsyZ%2B%2F4IiRbshztQspVr8d65h662S1YW5KBeToq4y6%2F84AU10zalqfXuR3zSfghUKhmcFwTrV%2BgFekvODiJmbHbMsMMSMYxv9P3OaeHAC91boC22DlpzeRoRw84Frj13V5QjDG%2BJ%2FXBjqkAct9yU%2FUy6J7n0qO%2B4nA7tfv8ZrRHBmEPQX5CdMhEwB90DVYk%2FVY3Ur1zeyq6kkuLddR4%2FTk87EuTz8OfEB2tqO%2B%2Fr7OBYXoJ%2B9uEZB0Nz6rxKQFkuybci8zTugWF%2FYFvoKCneNRyS5hhj%2F6fz0BvMnsJouoxcnUsvlpqvjgYUya76%2FsDIVgX4kWppQldgkEHoO8tDcCXew11hzOYx9r6e%2BBOjZ3&X-Amz-Signature=0a85fb25b8e78d6d71ed1fba917d580e5742522b4246b46b2b0115d42ca6be67&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI3GTNS3%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkPn6CCtWah%2FnpblUs6V4MWKsXrazEPntmjk6v4XyADAIhAJ8puzZvCYVjOZYYteNHxHEHP%2F90ppWKVWgBkeoT9YLkKv8DCHQQABoMNjM3NDIzMTgzODA1IgxvpVnpaOTQGbinWMwq3APJ18AditRhVkeL8jpvQGJ15Q4xsqdMVik63LB0P%2F26DUwqKhL2FXFKlTS6shEMh%2F4iDU7vRlVFjHo16nTXKQHLCFDeoLegeYwsj%2B67CM6mjWVW7QKEJaLXsn9xRCZ%2BIzMYw4rcESsGQxCxQmmZTv6jzF8hk6IIDy%2FaWi6L6FzEFOGNtZOECKmTO7tKiX4d9TkhAcEhpx8IzxzFuVHpvv0YoJwSD001j089y%2BF%2BW9DqNMvFBcPeJWjYIrM6ypYJh%2FGpeJbe%2F%2Bfru3qVajJ%2FivOte7q7eWlIQu14pXXHfqXvsIDkqXWwTsA7R2L6hWou2If1SN%2BOOVWoFP%2FoAPUCvABE0dunL2YyP7gcwSEUfdP9j%2BXHjOehCTN0nGs29PA5rPQRd8To8lKlAMb78xhoaysbrCwdDVd%2BWp7dgbQUg7i5eKeVzpK2%2FU0l3rdr7FmzY1xthHUAh4ktmN33dimN4pepL4HszCFZ8t%2FUH2hpf9f395hDY%2BvjC%2FSKFwzsyZ%2B%2F4IiRbshztQspVr8d65h662S1YW5KBeToq4y6%2F84AU10zalqfXuR3zSfghUKhmcFwTrV%2BgFekvODiJmbHbMsMMSMYxv9P3OaeHAC91boC22DlpzeRoRw84Frj13V5QjDG%2BJ%2FXBjqkAct9yU%2FUy6J7n0qO%2B4nA7tfv8ZrRHBmEPQX5CdMhEwB90DVYk%2FVY3Ur1zeyq6kkuLddR4%2FTk87EuTz8OfEB2tqO%2B%2Fr7OBYXoJ%2B9uEZB0Nz6rxKQFkuybci8zTugWF%2FYFvoKCneNRyS5hhj%2F6fz0BvMnsJouoxcnUsvlpqvjgYUya76%2FsDIVgX4kWppQldgkEHoO8tDcCXew11hzOYx9r6e%2BBOjZ3&X-Amz-Signature=d707512ee96d425cec4d0674bc3aa209e9f86437b76f98080184e608a4d6c408&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
