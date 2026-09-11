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
fetched_at: '2026-09-11T02:01:23.798Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNMT5EQF%2F20260911%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260911T020117Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCAGYpVQPiKqwYEL2en1O2QilgtmUe7i655tLOUUiD8DgIgLJQJTGP%2F0EYar9ySWaDrU%2FOJlkcyGUpRUviGpcQWZFsqiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKjIyBNfJeMlPnAJmCrcA7%2BUn59Su6hLG28o7zpr4R0ZbZnGusNLUSvdaPO9JqZa%2FWF2GWdoe0r4oaAVWBiM1SuIq0sI9x2Ml9RDsIDhC57bP%2FOy9mV4%2BGgfwrg6oWQPO4JopaI4qYpW836dCR22IRJBpvzUdhX%2BF4jnqBV%2Fwb2LDy1X2lhhgSe02JRWQLSMLhcSglrzD4ZkfVL4Qp6t32Tx6Ag0ko6LfR7CtQp1zsoT3rBO6qIlX9ok70%2F8Uhrx0bu1LkVwJmrcgd8zsogaH8Gfbb1XzU03zm5pDN7y%2F0k0hfIw3yr2AmGyZnsHGer3voMV59ZRGJbRol6p7N18NSAx0TnTKoc0BEEiZ2fXXl8ZXVjZJYXb%2Frt%2F95sjkqnONqUDq9ioRjiwYraxH8CQwqjKYaMLISa0uDGL30X4yU4nK0kYNNtVqEzc5H7RJT%2BN74t3a5CG7S4rXvTE7oW%2BzFwmKrBZG8IU6ff%2B6ipxb1Jvtl0eWAdTw6WEfTFhg4Mwd1pDN6DC7%2FkbxufbJofCj8BpCSvM9FWTKq%2B%2BWO53wysqsn5f7%2F7bvcmMPTuq4IsAEqu%2BeBTAFrrGShgGOdvxoLMW9KgQbmKSN95Y6ZJqQKSLoBNtOjWbs9XojgosLt3DNuFf845ArhBcIq3PMN6sjdUGOqUBkpeafUhu8%2Fj5fdjRMcNCzlYdJtCLFTzn1xdQ39nTv%2BiqqETSZ526fEpHbk3fxE9ye1yESxVyI49GsQ%2F4AWmfx0LULgVJT9gmH%2F%2BaJGyjMsKmQmEZPwmS6l1zJrhXbYgWw8FHJdha9VYRJqBEWndXV0tiMEMp2tF%2BlHBk9%2BgSvSenyKNjp1J4cMCzw84jC8GoVbBhu%2FkLRfu5pSUOLs6H4RYi%2Fg89&X-Amz-Signature=ee50de3a825aa69e6b977a23396a1cee8724847d466c081181ae9d5736caee60&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNMT5EQF%2F20260911%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260911T020117Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCAGYpVQPiKqwYEL2en1O2QilgtmUe7i655tLOUUiD8DgIgLJQJTGP%2F0EYar9ySWaDrU%2FOJlkcyGUpRUviGpcQWZFsqiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKjIyBNfJeMlPnAJmCrcA7%2BUn59Su6hLG28o7zpr4R0ZbZnGusNLUSvdaPO9JqZa%2FWF2GWdoe0r4oaAVWBiM1SuIq0sI9x2Ml9RDsIDhC57bP%2FOy9mV4%2BGgfwrg6oWQPO4JopaI4qYpW836dCR22IRJBpvzUdhX%2BF4jnqBV%2Fwb2LDy1X2lhhgSe02JRWQLSMLhcSglrzD4ZkfVL4Qp6t32Tx6Ag0ko6LfR7CtQp1zsoT3rBO6qIlX9ok70%2F8Uhrx0bu1LkVwJmrcgd8zsogaH8Gfbb1XzU03zm5pDN7y%2F0k0hfIw3yr2AmGyZnsHGer3voMV59ZRGJbRol6p7N18NSAx0TnTKoc0BEEiZ2fXXl8ZXVjZJYXb%2Frt%2F95sjkqnONqUDq9ioRjiwYraxH8CQwqjKYaMLISa0uDGL30X4yU4nK0kYNNtVqEzc5H7RJT%2BN74t3a5CG7S4rXvTE7oW%2BzFwmKrBZG8IU6ff%2B6ipxb1Jvtl0eWAdTw6WEfTFhg4Mwd1pDN6DC7%2FkbxufbJofCj8BpCSvM9FWTKq%2B%2BWO53wysqsn5f7%2F7bvcmMPTuq4IsAEqu%2BeBTAFrrGShgGOdvxoLMW9KgQbmKSN95Y6ZJqQKSLoBNtOjWbs9XojgosLt3DNuFf845ArhBcIq3PMN6sjdUGOqUBkpeafUhu8%2Fj5fdjRMcNCzlYdJtCLFTzn1xdQ39nTv%2BiqqETSZ526fEpHbk3fxE9ye1yESxVyI49GsQ%2F4AWmfx0LULgVJT9gmH%2F%2BaJGyjMsKmQmEZPwmS6l1zJrhXbYgWw8FHJdha9VYRJqBEWndXV0tiMEMp2tF%2BlHBk9%2BgSvSenyKNjp1J4cMCzw84jC8GoVbBhu%2FkLRfu5pSUOLs6H4RYi%2Fg89&X-Amz-Signature=39475ae671fc592bc07e0559d26a61313a37c15be3d320bdfd90c1ec66048987&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNMT5EQF%2F20260911%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260911T020117Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCAGYpVQPiKqwYEL2en1O2QilgtmUe7i655tLOUUiD8DgIgLJQJTGP%2F0EYar9ySWaDrU%2FOJlkcyGUpRUviGpcQWZFsqiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKjIyBNfJeMlPnAJmCrcA7%2BUn59Su6hLG28o7zpr4R0ZbZnGusNLUSvdaPO9JqZa%2FWF2GWdoe0r4oaAVWBiM1SuIq0sI9x2Ml9RDsIDhC57bP%2FOy9mV4%2BGgfwrg6oWQPO4JopaI4qYpW836dCR22IRJBpvzUdhX%2BF4jnqBV%2Fwb2LDy1X2lhhgSe02JRWQLSMLhcSglrzD4ZkfVL4Qp6t32Tx6Ag0ko6LfR7CtQp1zsoT3rBO6qIlX9ok70%2F8Uhrx0bu1LkVwJmrcgd8zsogaH8Gfbb1XzU03zm5pDN7y%2F0k0hfIw3yr2AmGyZnsHGer3voMV59ZRGJbRol6p7N18NSAx0TnTKoc0BEEiZ2fXXl8ZXVjZJYXb%2Frt%2F95sjkqnONqUDq9ioRjiwYraxH8CQwqjKYaMLISa0uDGL30X4yU4nK0kYNNtVqEzc5H7RJT%2BN74t3a5CG7S4rXvTE7oW%2BzFwmKrBZG8IU6ff%2B6ipxb1Jvtl0eWAdTw6WEfTFhg4Mwd1pDN6DC7%2FkbxufbJofCj8BpCSvM9FWTKq%2B%2BWO53wysqsn5f7%2F7bvcmMPTuq4IsAEqu%2BeBTAFrrGShgGOdvxoLMW9KgQbmKSN95Y6ZJqQKSLoBNtOjWbs9XojgosLt3DNuFf845ArhBcIq3PMN6sjdUGOqUBkpeafUhu8%2Fj5fdjRMcNCzlYdJtCLFTzn1xdQ39nTv%2BiqqETSZ526fEpHbk3fxE9ye1yESxVyI49GsQ%2F4AWmfx0LULgVJT9gmH%2F%2BaJGyjMsKmQmEZPwmS6l1zJrhXbYgWw8FHJdha9VYRJqBEWndXV0tiMEMp2tF%2BlHBk9%2BgSvSenyKNjp1J4cMCzw84jC8GoVbBhu%2FkLRfu5pSUOLs6H4RYi%2Fg89&X-Amz-Signature=fcadcb63b2adb2023fb34ae891b23b7cee88631eafb4ae917397e415c0e20ba1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNMT5EQF%2F20260911%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260911T020117Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCAGYpVQPiKqwYEL2en1O2QilgtmUe7i655tLOUUiD8DgIgLJQJTGP%2F0EYar9ySWaDrU%2FOJlkcyGUpRUviGpcQWZFsqiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKjIyBNfJeMlPnAJmCrcA7%2BUn59Su6hLG28o7zpr4R0ZbZnGusNLUSvdaPO9JqZa%2FWF2GWdoe0r4oaAVWBiM1SuIq0sI9x2Ml9RDsIDhC57bP%2FOy9mV4%2BGgfwrg6oWQPO4JopaI4qYpW836dCR22IRJBpvzUdhX%2BF4jnqBV%2Fwb2LDy1X2lhhgSe02JRWQLSMLhcSglrzD4ZkfVL4Qp6t32Tx6Ag0ko6LfR7CtQp1zsoT3rBO6qIlX9ok70%2F8Uhrx0bu1LkVwJmrcgd8zsogaH8Gfbb1XzU03zm5pDN7y%2F0k0hfIw3yr2AmGyZnsHGer3voMV59ZRGJbRol6p7N18NSAx0TnTKoc0BEEiZ2fXXl8ZXVjZJYXb%2Frt%2F95sjkqnONqUDq9ioRjiwYraxH8CQwqjKYaMLISa0uDGL30X4yU4nK0kYNNtVqEzc5H7RJT%2BN74t3a5CG7S4rXvTE7oW%2BzFwmKrBZG8IU6ff%2B6ipxb1Jvtl0eWAdTw6WEfTFhg4Mwd1pDN6DC7%2FkbxufbJofCj8BpCSvM9FWTKq%2B%2BWO53wysqsn5f7%2F7bvcmMPTuq4IsAEqu%2BeBTAFrrGShgGOdvxoLMW9KgQbmKSN95Y6ZJqQKSLoBNtOjWbs9XojgosLt3DNuFf845ArhBcIq3PMN6sjdUGOqUBkpeafUhu8%2Fj5fdjRMcNCzlYdJtCLFTzn1xdQ39nTv%2BiqqETSZ526fEpHbk3fxE9ye1yESxVyI49GsQ%2F4AWmfx0LULgVJT9gmH%2F%2BaJGyjMsKmQmEZPwmS6l1zJrhXbYgWw8FHJdha9VYRJqBEWndXV0tiMEMp2tF%2BlHBk9%2BgSvSenyKNjp1J4cMCzw84jC8GoVbBhu%2FkLRfu5pSUOLs6H4RYi%2Fg89&X-Amz-Signature=45a5e517270b9becf8cf85697006d66a3bf308f54876de474256c51cc4d1a7e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNMT5EQF%2F20260911%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260911T020117Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCAGYpVQPiKqwYEL2en1O2QilgtmUe7i655tLOUUiD8DgIgLJQJTGP%2F0EYar9ySWaDrU%2FOJlkcyGUpRUviGpcQWZFsqiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKjIyBNfJeMlPnAJmCrcA7%2BUn59Su6hLG28o7zpr4R0ZbZnGusNLUSvdaPO9JqZa%2FWF2GWdoe0r4oaAVWBiM1SuIq0sI9x2Ml9RDsIDhC57bP%2FOy9mV4%2BGgfwrg6oWQPO4JopaI4qYpW836dCR22IRJBpvzUdhX%2BF4jnqBV%2Fwb2LDy1X2lhhgSe02JRWQLSMLhcSglrzD4ZkfVL4Qp6t32Tx6Ag0ko6LfR7CtQp1zsoT3rBO6qIlX9ok70%2F8Uhrx0bu1LkVwJmrcgd8zsogaH8Gfbb1XzU03zm5pDN7y%2F0k0hfIw3yr2AmGyZnsHGer3voMV59ZRGJbRol6p7N18NSAx0TnTKoc0BEEiZ2fXXl8ZXVjZJYXb%2Frt%2F95sjkqnONqUDq9ioRjiwYraxH8CQwqjKYaMLISa0uDGL30X4yU4nK0kYNNtVqEzc5H7RJT%2BN74t3a5CG7S4rXvTE7oW%2BzFwmKrBZG8IU6ff%2B6ipxb1Jvtl0eWAdTw6WEfTFhg4Mwd1pDN6DC7%2FkbxufbJofCj8BpCSvM9FWTKq%2B%2BWO53wysqsn5f7%2F7bvcmMPTuq4IsAEqu%2BeBTAFrrGShgGOdvxoLMW9KgQbmKSN95Y6ZJqQKSLoBNtOjWbs9XojgosLt3DNuFf845ArhBcIq3PMN6sjdUGOqUBkpeafUhu8%2Fj5fdjRMcNCzlYdJtCLFTzn1xdQ39nTv%2BiqqETSZ526fEpHbk3fxE9ye1yESxVyI49GsQ%2F4AWmfx0LULgVJT9gmH%2F%2BaJGyjMsKmQmEZPwmS6l1zJrhXbYgWw8FHJdha9VYRJqBEWndXV0tiMEMp2tF%2BlHBk9%2BgSvSenyKNjp1J4cMCzw84jC8GoVbBhu%2FkLRfu5pSUOLs6H4RYi%2Fg89&X-Amz-Signature=1747d48b3c622386ea0324c57a7b8da393a9c136c8d42e5e8100afd55b6457ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNMT5EQF%2F20260911%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260911T020117Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCAGYpVQPiKqwYEL2en1O2QilgtmUe7i655tLOUUiD8DgIgLJQJTGP%2F0EYar9ySWaDrU%2FOJlkcyGUpRUviGpcQWZFsqiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKjIyBNfJeMlPnAJmCrcA7%2BUn59Su6hLG28o7zpr4R0ZbZnGusNLUSvdaPO9JqZa%2FWF2GWdoe0r4oaAVWBiM1SuIq0sI9x2Ml9RDsIDhC57bP%2FOy9mV4%2BGgfwrg6oWQPO4JopaI4qYpW836dCR22IRJBpvzUdhX%2BF4jnqBV%2Fwb2LDy1X2lhhgSe02JRWQLSMLhcSglrzD4ZkfVL4Qp6t32Tx6Ag0ko6LfR7CtQp1zsoT3rBO6qIlX9ok70%2F8Uhrx0bu1LkVwJmrcgd8zsogaH8Gfbb1XzU03zm5pDN7y%2F0k0hfIw3yr2AmGyZnsHGer3voMV59ZRGJbRol6p7N18NSAx0TnTKoc0BEEiZ2fXXl8ZXVjZJYXb%2Frt%2F95sjkqnONqUDq9ioRjiwYraxH8CQwqjKYaMLISa0uDGL30X4yU4nK0kYNNtVqEzc5H7RJT%2BN74t3a5CG7S4rXvTE7oW%2BzFwmKrBZG8IU6ff%2B6ipxb1Jvtl0eWAdTw6WEfTFhg4Mwd1pDN6DC7%2FkbxufbJofCj8BpCSvM9FWTKq%2B%2BWO53wysqsn5f7%2F7bvcmMPTuq4IsAEqu%2BeBTAFrrGShgGOdvxoLMW9KgQbmKSN95Y6ZJqQKSLoBNtOjWbs9XojgosLt3DNuFf845ArhBcIq3PMN6sjdUGOqUBkpeafUhu8%2Fj5fdjRMcNCzlYdJtCLFTzn1xdQ39nTv%2BiqqETSZ526fEpHbk3fxE9ye1yESxVyI49GsQ%2F4AWmfx0LULgVJT9gmH%2F%2BaJGyjMsKmQmEZPwmS6l1zJrhXbYgWw8FHJdha9VYRJqBEWndXV0tiMEMp2tF%2BlHBk9%2BgSvSenyKNjp1J4cMCzw84jC8GoVbBhu%2FkLRfu5pSUOLs6H4RYi%2Fg89&X-Amz-Signature=ddc6fd366570a7b5e46f15ad5ca6cb4cf55617b4ab21461238e0686af71e2e4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNMT5EQF%2F20260911%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260911T020117Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCAGYpVQPiKqwYEL2en1O2QilgtmUe7i655tLOUUiD8DgIgLJQJTGP%2F0EYar9ySWaDrU%2FOJlkcyGUpRUviGpcQWZFsqiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKjIyBNfJeMlPnAJmCrcA7%2BUn59Su6hLG28o7zpr4R0ZbZnGusNLUSvdaPO9JqZa%2FWF2GWdoe0r4oaAVWBiM1SuIq0sI9x2Ml9RDsIDhC57bP%2FOy9mV4%2BGgfwrg6oWQPO4JopaI4qYpW836dCR22IRJBpvzUdhX%2BF4jnqBV%2Fwb2LDy1X2lhhgSe02JRWQLSMLhcSglrzD4ZkfVL4Qp6t32Tx6Ag0ko6LfR7CtQp1zsoT3rBO6qIlX9ok70%2F8Uhrx0bu1LkVwJmrcgd8zsogaH8Gfbb1XzU03zm5pDN7y%2F0k0hfIw3yr2AmGyZnsHGer3voMV59ZRGJbRol6p7N18NSAx0TnTKoc0BEEiZ2fXXl8ZXVjZJYXb%2Frt%2F95sjkqnONqUDq9ioRjiwYraxH8CQwqjKYaMLISa0uDGL30X4yU4nK0kYNNtVqEzc5H7RJT%2BN74t3a5CG7S4rXvTE7oW%2BzFwmKrBZG8IU6ff%2B6ipxb1Jvtl0eWAdTw6WEfTFhg4Mwd1pDN6DC7%2FkbxufbJofCj8BpCSvM9FWTKq%2B%2BWO53wysqsn5f7%2F7bvcmMPTuq4IsAEqu%2BeBTAFrrGShgGOdvxoLMW9KgQbmKSN95Y6ZJqQKSLoBNtOjWbs9XojgosLt3DNuFf845ArhBcIq3PMN6sjdUGOqUBkpeafUhu8%2Fj5fdjRMcNCzlYdJtCLFTzn1xdQ39nTv%2BiqqETSZ526fEpHbk3fxE9ye1yESxVyI49GsQ%2F4AWmfx0LULgVJT9gmH%2F%2BaJGyjMsKmQmEZPwmS6l1zJrhXbYgWw8FHJdha9VYRJqBEWndXV0tiMEMp2tF%2BlHBk9%2BgSvSenyKNjp1J4cMCzw84jC8GoVbBhu%2FkLRfu5pSUOLs6H4RYi%2Fg89&X-Amz-Signature=e1cd2184f7c09a8259d7b7ae2b794cb0785dfaa0e701b39d63f7d68d7a443e2b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNMT5EQF%2F20260911%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260911T020117Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCAGYpVQPiKqwYEL2en1O2QilgtmUe7i655tLOUUiD8DgIgLJQJTGP%2F0EYar9ySWaDrU%2FOJlkcyGUpRUviGpcQWZFsqiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKjIyBNfJeMlPnAJmCrcA7%2BUn59Su6hLG28o7zpr4R0ZbZnGusNLUSvdaPO9JqZa%2FWF2GWdoe0r4oaAVWBiM1SuIq0sI9x2Ml9RDsIDhC57bP%2FOy9mV4%2BGgfwrg6oWQPO4JopaI4qYpW836dCR22IRJBpvzUdhX%2BF4jnqBV%2Fwb2LDy1X2lhhgSe02JRWQLSMLhcSglrzD4ZkfVL4Qp6t32Tx6Ag0ko6LfR7CtQp1zsoT3rBO6qIlX9ok70%2F8Uhrx0bu1LkVwJmrcgd8zsogaH8Gfbb1XzU03zm5pDN7y%2F0k0hfIw3yr2AmGyZnsHGer3voMV59ZRGJbRol6p7N18NSAx0TnTKoc0BEEiZ2fXXl8ZXVjZJYXb%2Frt%2F95sjkqnONqUDq9ioRjiwYraxH8CQwqjKYaMLISa0uDGL30X4yU4nK0kYNNtVqEzc5H7RJT%2BN74t3a5CG7S4rXvTE7oW%2BzFwmKrBZG8IU6ff%2B6ipxb1Jvtl0eWAdTw6WEfTFhg4Mwd1pDN6DC7%2FkbxufbJofCj8BpCSvM9FWTKq%2B%2BWO53wysqsn5f7%2F7bvcmMPTuq4IsAEqu%2BeBTAFrrGShgGOdvxoLMW9KgQbmKSN95Y6ZJqQKSLoBNtOjWbs9XojgosLt3DNuFf845ArhBcIq3PMN6sjdUGOqUBkpeafUhu8%2Fj5fdjRMcNCzlYdJtCLFTzn1xdQ39nTv%2BiqqETSZ526fEpHbk3fxE9ye1yESxVyI49GsQ%2F4AWmfx0LULgVJT9gmH%2F%2BaJGyjMsKmQmEZPwmS6l1zJrhXbYgWw8FHJdha9VYRJqBEWndXV0tiMEMp2tF%2BlHBk9%2BgSvSenyKNjp1J4cMCzw84jC8GoVbBhu%2FkLRfu5pSUOLs6H4RYi%2Fg89&X-Amz-Signature=6499394179a18fd0e659a112cc38c3f902e7ddd871ec12d3d709212aaa089de0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNMT5EQF%2F20260911%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260911T020117Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCAGYpVQPiKqwYEL2en1O2QilgtmUe7i655tLOUUiD8DgIgLJQJTGP%2F0EYar9ySWaDrU%2FOJlkcyGUpRUviGpcQWZFsqiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKjIyBNfJeMlPnAJmCrcA7%2BUn59Su6hLG28o7zpr4R0ZbZnGusNLUSvdaPO9JqZa%2FWF2GWdoe0r4oaAVWBiM1SuIq0sI9x2Ml9RDsIDhC57bP%2FOy9mV4%2BGgfwrg6oWQPO4JopaI4qYpW836dCR22IRJBpvzUdhX%2BF4jnqBV%2Fwb2LDy1X2lhhgSe02JRWQLSMLhcSglrzD4ZkfVL4Qp6t32Tx6Ag0ko6LfR7CtQp1zsoT3rBO6qIlX9ok70%2F8Uhrx0bu1LkVwJmrcgd8zsogaH8Gfbb1XzU03zm5pDN7y%2F0k0hfIw3yr2AmGyZnsHGer3voMV59ZRGJbRol6p7N18NSAx0TnTKoc0BEEiZ2fXXl8ZXVjZJYXb%2Frt%2F95sjkqnONqUDq9ioRjiwYraxH8CQwqjKYaMLISa0uDGL30X4yU4nK0kYNNtVqEzc5H7RJT%2BN74t3a5CG7S4rXvTE7oW%2BzFwmKrBZG8IU6ff%2B6ipxb1Jvtl0eWAdTw6WEfTFhg4Mwd1pDN6DC7%2FkbxufbJofCj8BpCSvM9FWTKq%2B%2BWO53wysqsn5f7%2F7bvcmMPTuq4IsAEqu%2BeBTAFrrGShgGOdvxoLMW9KgQbmKSN95Y6ZJqQKSLoBNtOjWbs9XojgosLt3DNuFf845ArhBcIq3PMN6sjdUGOqUBkpeafUhu8%2Fj5fdjRMcNCzlYdJtCLFTzn1xdQ39nTv%2BiqqETSZ526fEpHbk3fxE9ye1yESxVyI49GsQ%2F4AWmfx0LULgVJT9gmH%2F%2BaJGyjMsKmQmEZPwmS6l1zJrhXbYgWw8FHJdha9VYRJqBEWndXV0tiMEMp2tF%2BlHBk9%2BgSvSenyKNjp1J4cMCzw84jC8GoVbBhu%2FkLRfu5pSUOLs6H4RYi%2Fg89&X-Amz-Signature=033a8b2f2a89f9cde3915c2f062f26f2c6a4b25f8cc0e6a4ad11a47f424ca16d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
