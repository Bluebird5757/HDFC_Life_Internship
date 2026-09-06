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
fetched_at: '2026-09-06T01:53:03.618Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VO7YGMYV%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015258Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIC8yw%2BCDoeJxbFOVxv87pq7QyCW7w5IaA8YCXuCyqk%2FlAiEAoyeCLjwRyG2g%2FOwSOLVIhHy6clc8XTRU48GedsakW%2B4q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDEnaqcZspDsa6rFR5SrcA5hnwTuWsLh9ZJ9hsHcaaU%2FF5Mg8DSVozIYQacCZx3kAjzdT4iV8tAG8QICgWouwA8CytFze8n7AxRU9WunW1GBlwsHjj3L4Yo8q3Nii3QxFo31wrP0k6RfHOOrfSle3OHqhv4R822UGP%2BsBinw8tP9NtDvE8prFHY0%2Fq4gW50DYiPwNpSVyZrHAtxIaUwD9XIzpz5gZCwZ0c752qo32ra1JwSc8%2FaOooTPCK2%2FBBRWl4VrB3ttqWZHuMJDMhn2ZK9cJpzaZrGUknzkEoug13fweHoGKvmzBHq5XkDgFRx2N6%2F6MMgDsZLKoJ%2B01D82X5fPu9rqtbXMYQkDXGLJP1peVpSaJlSXhzpuGFOc4B0R8dzRCilGNLNTgQ3b5zBcnOkF%2FXS1TWJ3PFVdvB3%2FutvpvvQQqVT8226i3jmP99Ll%2Baj2l17RBCcOaM2Gzclk29091qwZRN6s5aMmGZsCzjy9XtdKBQ8wWtD9cZtsuZDrSPFekITvBGFEOqLuAZ%2B1J6P6ny3qneU%2BGLFQuHg4qSMzK7L0B91v3ZwV4S0qM5buOW1Ezvz5MQbdoGkL7fJUP3YwEnFoe1iv6%2BmNjWqaiMO0vZLlmS8f1fF7iG3qXdDWj%2FVTyx4tv53tcQ0coMJXr8tQGOqUBiTxNvJ4XhyP3KV887%2F4fLlEnoIphU9NkXlVKjSzYlS4kMMXVKRNK%2BAodefjuuUOGqOJzZKnbT8dPbhUGnJd8ptrE8wXV4s%2FAq%2FA%2B6pvM9Sm3RZBql1FXZbdIAzoKRRwaedULtzMDb8WdHYNArYfr6EtWouxgvScicAze9%2FRWY4gEpBFNzQlxBwXvShTHvOnDVbW7WQWnKTBIh6HV0id7Kvj%2FAEbo&X-Amz-Signature=bb9a4dd943bf7d29ce0c73256533e408f9afbcce8fad71fc00b133ee35c88b72&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VO7YGMYV%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015258Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIC8yw%2BCDoeJxbFOVxv87pq7QyCW7w5IaA8YCXuCyqk%2FlAiEAoyeCLjwRyG2g%2FOwSOLVIhHy6clc8XTRU48GedsakW%2B4q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDEnaqcZspDsa6rFR5SrcA5hnwTuWsLh9ZJ9hsHcaaU%2FF5Mg8DSVozIYQacCZx3kAjzdT4iV8tAG8QICgWouwA8CytFze8n7AxRU9WunW1GBlwsHjj3L4Yo8q3Nii3QxFo31wrP0k6RfHOOrfSle3OHqhv4R822UGP%2BsBinw8tP9NtDvE8prFHY0%2Fq4gW50DYiPwNpSVyZrHAtxIaUwD9XIzpz5gZCwZ0c752qo32ra1JwSc8%2FaOooTPCK2%2FBBRWl4VrB3ttqWZHuMJDMhn2ZK9cJpzaZrGUknzkEoug13fweHoGKvmzBHq5XkDgFRx2N6%2F6MMgDsZLKoJ%2B01D82X5fPu9rqtbXMYQkDXGLJP1peVpSaJlSXhzpuGFOc4B0R8dzRCilGNLNTgQ3b5zBcnOkF%2FXS1TWJ3PFVdvB3%2FutvpvvQQqVT8226i3jmP99Ll%2Baj2l17RBCcOaM2Gzclk29091qwZRN6s5aMmGZsCzjy9XtdKBQ8wWtD9cZtsuZDrSPFekITvBGFEOqLuAZ%2B1J6P6ny3qneU%2BGLFQuHg4qSMzK7L0B91v3ZwV4S0qM5buOW1Ezvz5MQbdoGkL7fJUP3YwEnFoe1iv6%2BmNjWqaiMO0vZLlmS8f1fF7iG3qXdDWj%2FVTyx4tv53tcQ0coMJXr8tQGOqUBiTxNvJ4XhyP3KV887%2F4fLlEnoIphU9NkXlVKjSzYlS4kMMXVKRNK%2BAodefjuuUOGqOJzZKnbT8dPbhUGnJd8ptrE8wXV4s%2FAq%2FA%2B6pvM9Sm3RZBql1FXZbdIAzoKRRwaedULtzMDb8WdHYNArYfr6EtWouxgvScicAze9%2FRWY4gEpBFNzQlxBwXvShTHvOnDVbW7WQWnKTBIh6HV0id7Kvj%2FAEbo&X-Amz-Signature=e6f9fd5bcfd1fff23ac911092db9c2c5666547d6f94731469de68d3d2fbb8b2d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VO7YGMYV%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015258Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIC8yw%2BCDoeJxbFOVxv87pq7QyCW7w5IaA8YCXuCyqk%2FlAiEAoyeCLjwRyG2g%2FOwSOLVIhHy6clc8XTRU48GedsakW%2B4q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDEnaqcZspDsa6rFR5SrcA5hnwTuWsLh9ZJ9hsHcaaU%2FF5Mg8DSVozIYQacCZx3kAjzdT4iV8tAG8QICgWouwA8CytFze8n7AxRU9WunW1GBlwsHjj3L4Yo8q3Nii3QxFo31wrP0k6RfHOOrfSle3OHqhv4R822UGP%2BsBinw8tP9NtDvE8prFHY0%2Fq4gW50DYiPwNpSVyZrHAtxIaUwD9XIzpz5gZCwZ0c752qo32ra1JwSc8%2FaOooTPCK2%2FBBRWl4VrB3ttqWZHuMJDMhn2ZK9cJpzaZrGUknzkEoug13fweHoGKvmzBHq5XkDgFRx2N6%2F6MMgDsZLKoJ%2B01D82X5fPu9rqtbXMYQkDXGLJP1peVpSaJlSXhzpuGFOc4B0R8dzRCilGNLNTgQ3b5zBcnOkF%2FXS1TWJ3PFVdvB3%2FutvpvvQQqVT8226i3jmP99Ll%2Baj2l17RBCcOaM2Gzclk29091qwZRN6s5aMmGZsCzjy9XtdKBQ8wWtD9cZtsuZDrSPFekITvBGFEOqLuAZ%2B1J6P6ny3qneU%2BGLFQuHg4qSMzK7L0B91v3ZwV4S0qM5buOW1Ezvz5MQbdoGkL7fJUP3YwEnFoe1iv6%2BmNjWqaiMO0vZLlmS8f1fF7iG3qXdDWj%2FVTyx4tv53tcQ0coMJXr8tQGOqUBiTxNvJ4XhyP3KV887%2F4fLlEnoIphU9NkXlVKjSzYlS4kMMXVKRNK%2BAodefjuuUOGqOJzZKnbT8dPbhUGnJd8ptrE8wXV4s%2FAq%2FA%2B6pvM9Sm3RZBql1FXZbdIAzoKRRwaedULtzMDb8WdHYNArYfr6EtWouxgvScicAze9%2FRWY4gEpBFNzQlxBwXvShTHvOnDVbW7WQWnKTBIh6HV0id7Kvj%2FAEbo&X-Amz-Signature=5371011bb345d0d123c45abeb6e508a156d12338131ccf4bff6fb829762ec389&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VO7YGMYV%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015258Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIC8yw%2BCDoeJxbFOVxv87pq7QyCW7w5IaA8YCXuCyqk%2FlAiEAoyeCLjwRyG2g%2FOwSOLVIhHy6clc8XTRU48GedsakW%2B4q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDEnaqcZspDsa6rFR5SrcA5hnwTuWsLh9ZJ9hsHcaaU%2FF5Mg8DSVozIYQacCZx3kAjzdT4iV8tAG8QICgWouwA8CytFze8n7AxRU9WunW1GBlwsHjj3L4Yo8q3Nii3QxFo31wrP0k6RfHOOrfSle3OHqhv4R822UGP%2BsBinw8tP9NtDvE8prFHY0%2Fq4gW50DYiPwNpSVyZrHAtxIaUwD9XIzpz5gZCwZ0c752qo32ra1JwSc8%2FaOooTPCK2%2FBBRWl4VrB3ttqWZHuMJDMhn2ZK9cJpzaZrGUknzkEoug13fweHoGKvmzBHq5XkDgFRx2N6%2F6MMgDsZLKoJ%2B01D82X5fPu9rqtbXMYQkDXGLJP1peVpSaJlSXhzpuGFOc4B0R8dzRCilGNLNTgQ3b5zBcnOkF%2FXS1TWJ3PFVdvB3%2FutvpvvQQqVT8226i3jmP99Ll%2Baj2l17RBCcOaM2Gzclk29091qwZRN6s5aMmGZsCzjy9XtdKBQ8wWtD9cZtsuZDrSPFekITvBGFEOqLuAZ%2B1J6P6ny3qneU%2BGLFQuHg4qSMzK7L0B91v3ZwV4S0qM5buOW1Ezvz5MQbdoGkL7fJUP3YwEnFoe1iv6%2BmNjWqaiMO0vZLlmS8f1fF7iG3qXdDWj%2FVTyx4tv53tcQ0coMJXr8tQGOqUBiTxNvJ4XhyP3KV887%2F4fLlEnoIphU9NkXlVKjSzYlS4kMMXVKRNK%2BAodefjuuUOGqOJzZKnbT8dPbhUGnJd8ptrE8wXV4s%2FAq%2FA%2B6pvM9Sm3RZBql1FXZbdIAzoKRRwaedULtzMDb8WdHYNArYfr6EtWouxgvScicAze9%2FRWY4gEpBFNzQlxBwXvShTHvOnDVbW7WQWnKTBIh6HV0id7Kvj%2FAEbo&X-Amz-Signature=bc8cf9d2bc9a0f4a826325526bcb5332b4ddba7ac6cfd28a97fb325685c68ac4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VO7YGMYV%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015258Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIC8yw%2BCDoeJxbFOVxv87pq7QyCW7w5IaA8YCXuCyqk%2FlAiEAoyeCLjwRyG2g%2FOwSOLVIhHy6clc8XTRU48GedsakW%2B4q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDEnaqcZspDsa6rFR5SrcA5hnwTuWsLh9ZJ9hsHcaaU%2FF5Mg8DSVozIYQacCZx3kAjzdT4iV8tAG8QICgWouwA8CytFze8n7AxRU9WunW1GBlwsHjj3L4Yo8q3Nii3QxFo31wrP0k6RfHOOrfSle3OHqhv4R822UGP%2BsBinw8tP9NtDvE8prFHY0%2Fq4gW50DYiPwNpSVyZrHAtxIaUwD9XIzpz5gZCwZ0c752qo32ra1JwSc8%2FaOooTPCK2%2FBBRWl4VrB3ttqWZHuMJDMhn2ZK9cJpzaZrGUknzkEoug13fweHoGKvmzBHq5XkDgFRx2N6%2F6MMgDsZLKoJ%2B01D82X5fPu9rqtbXMYQkDXGLJP1peVpSaJlSXhzpuGFOc4B0R8dzRCilGNLNTgQ3b5zBcnOkF%2FXS1TWJ3PFVdvB3%2FutvpvvQQqVT8226i3jmP99Ll%2Baj2l17RBCcOaM2Gzclk29091qwZRN6s5aMmGZsCzjy9XtdKBQ8wWtD9cZtsuZDrSPFekITvBGFEOqLuAZ%2B1J6P6ny3qneU%2BGLFQuHg4qSMzK7L0B91v3ZwV4S0qM5buOW1Ezvz5MQbdoGkL7fJUP3YwEnFoe1iv6%2BmNjWqaiMO0vZLlmS8f1fF7iG3qXdDWj%2FVTyx4tv53tcQ0coMJXr8tQGOqUBiTxNvJ4XhyP3KV887%2F4fLlEnoIphU9NkXlVKjSzYlS4kMMXVKRNK%2BAodefjuuUOGqOJzZKnbT8dPbhUGnJd8ptrE8wXV4s%2FAq%2FA%2B6pvM9Sm3RZBql1FXZbdIAzoKRRwaedULtzMDb8WdHYNArYfr6EtWouxgvScicAze9%2FRWY4gEpBFNzQlxBwXvShTHvOnDVbW7WQWnKTBIh6HV0id7Kvj%2FAEbo&X-Amz-Signature=2f765646d7ac65d80ce49876587f4ae79fce24334ec74ec97fbd4853e11890b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VO7YGMYV%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015258Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIC8yw%2BCDoeJxbFOVxv87pq7QyCW7w5IaA8YCXuCyqk%2FlAiEAoyeCLjwRyG2g%2FOwSOLVIhHy6clc8XTRU48GedsakW%2B4q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDEnaqcZspDsa6rFR5SrcA5hnwTuWsLh9ZJ9hsHcaaU%2FF5Mg8DSVozIYQacCZx3kAjzdT4iV8tAG8QICgWouwA8CytFze8n7AxRU9WunW1GBlwsHjj3L4Yo8q3Nii3QxFo31wrP0k6RfHOOrfSle3OHqhv4R822UGP%2BsBinw8tP9NtDvE8prFHY0%2Fq4gW50DYiPwNpSVyZrHAtxIaUwD9XIzpz5gZCwZ0c752qo32ra1JwSc8%2FaOooTPCK2%2FBBRWl4VrB3ttqWZHuMJDMhn2ZK9cJpzaZrGUknzkEoug13fweHoGKvmzBHq5XkDgFRx2N6%2F6MMgDsZLKoJ%2B01D82X5fPu9rqtbXMYQkDXGLJP1peVpSaJlSXhzpuGFOc4B0R8dzRCilGNLNTgQ3b5zBcnOkF%2FXS1TWJ3PFVdvB3%2FutvpvvQQqVT8226i3jmP99Ll%2Baj2l17RBCcOaM2Gzclk29091qwZRN6s5aMmGZsCzjy9XtdKBQ8wWtD9cZtsuZDrSPFekITvBGFEOqLuAZ%2B1J6P6ny3qneU%2BGLFQuHg4qSMzK7L0B91v3ZwV4S0qM5buOW1Ezvz5MQbdoGkL7fJUP3YwEnFoe1iv6%2BmNjWqaiMO0vZLlmS8f1fF7iG3qXdDWj%2FVTyx4tv53tcQ0coMJXr8tQGOqUBiTxNvJ4XhyP3KV887%2F4fLlEnoIphU9NkXlVKjSzYlS4kMMXVKRNK%2BAodefjuuUOGqOJzZKnbT8dPbhUGnJd8ptrE8wXV4s%2FAq%2FA%2B6pvM9Sm3RZBql1FXZbdIAzoKRRwaedULtzMDb8WdHYNArYfr6EtWouxgvScicAze9%2FRWY4gEpBFNzQlxBwXvShTHvOnDVbW7WQWnKTBIh6HV0id7Kvj%2FAEbo&X-Amz-Signature=6c30a2f09fff513d00f79069b2619ade0fe99ef2353e6509d546116379f23205&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VO7YGMYV%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015258Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIC8yw%2BCDoeJxbFOVxv87pq7QyCW7w5IaA8YCXuCyqk%2FlAiEAoyeCLjwRyG2g%2FOwSOLVIhHy6clc8XTRU48GedsakW%2B4q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDEnaqcZspDsa6rFR5SrcA5hnwTuWsLh9ZJ9hsHcaaU%2FF5Mg8DSVozIYQacCZx3kAjzdT4iV8tAG8QICgWouwA8CytFze8n7AxRU9WunW1GBlwsHjj3L4Yo8q3Nii3QxFo31wrP0k6RfHOOrfSle3OHqhv4R822UGP%2BsBinw8tP9NtDvE8prFHY0%2Fq4gW50DYiPwNpSVyZrHAtxIaUwD9XIzpz5gZCwZ0c752qo32ra1JwSc8%2FaOooTPCK2%2FBBRWl4VrB3ttqWZHuMJDMhn2ZK9cJpzaZrGUknzkEoug13fweHoGKvmzBHq5XkDgFRx2N6%2F6MMgDsZLKoJ%2B01D82X5fPu9rqtbXMYQkDXGLJP1peVpSaJlSXhzpuGFOc4B0R8dzRCilGNLNTgQ3b5zBcnOkF%2FXS1TWJ3PFVdvB3%2FutvpvvQQqVT8226i3jmP99Ll%2Baj2l17RBCcOaM2Gzclk29091qwZRN6s5aMmGZsCzjy9XtdKBQ8wWtD9cZtsuZDrSPFekITvBGFEOqLuAZ%2B1J6P6ny3qneU%2BGLFQuHg4qSMzK7L0B91v3ZwV4S0qM5buOW1Ezvz5MQbdoGkL7fJUP3YwEnFoe1iv6%2BmNjWqaiMO0vZLlmS8f1fF7iG3qXdDWj%2FVTyx4tv53tcQ0coMJXr8tQGOqUBiTxNvJ4XhyP3KV887%2F4fLlEnoIphU9NkXlVKjSzYlS4kMMXVKRNK%2BAodefjuuUOGqOJzZKnbT8dPbhUGnJd8ptrE8wXV4s%2FAq%2FA%2B6pvM9Sm3RZBql1FXZbdIAzoKRRwaedULtzMDb8WdHYNArYfr6EtWouxgvScicAze9%2FRWY4gEpBFNzQlxBwXvShTHvOnDVbW7WQWnKTBIh6HV0id7Kvj%2FAEbo&X-Amz-Signature=6a14e4c7f34c13b503f496b823dad0e3db0490bba0cbf371e41ac9b4939de9b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VO7YGMYV%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015258Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIC8yw%2BCDoeJxbFOVxv87pq7QyCW7w5IaA8YCXuCyqk%2FlAiEAoyeCLjwRyG2g%2FOwSOLVIhHy6clc8XTRU48GedsakW%2B4q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDEnaqcZspDsa6rFR5SrcA5hnwTuWsLh9ZJ9hsHcaaU%2FF5Mg8DSVozIYQacCZx3kAjzdT4iV8tAG8QICgWouwA8CytFze8n7AxRU9WunW1GBlwsHjj3L4Yo8q3Nii3QxFo31wrP0k6RfHOOrfSle3OHqhv4R822UGP%2BsBinw8tP9NtDvE8prFHY0%2Fq4gW50DYiPwNpSVyZrHAtxIaUwD9XIzpz5gZCwZ0c752qo32ra1JwSc8%2FaOooTPCK2%2FBBRWl4VrB3ttqWZHuMJDMhn2ZK9cJpzaZrGUknzkEoug13fweHoGKvmzBHq5XkDgFRx2N6%2F6MMgDsZLKoJ%2B01D82X5fPu9rqtbXMYQkDXGLJP1peVpSaJlSXhzpuGFOc4B0R8dzRCilGNLNTgQ3b5zBcnOkF%2FXS1TWJ3PFVdvB3%2FutvpvvQQqVT8226i3jmP99Ll%2Baj2l17RBCcOaM2Gzclk29091qwZRN6s5aMmGZsCzjy9XtdKBQ8wWtD9cZtsuZDrSPFekITvBGFEOqLuAZ%2B1J6P6ny3qneU%2BGLFQuHg4qSMzK7L0B91v3ZwV4S0qM5buOW1Ezvz5MQbdoGkL7fJUP3YwEnFoe1iv6%2BmNjWqaiMO0vZLlmS8f1fF7iG3qXdDWj%2FVTyx4tv53tcQ0coMJXr8tQGOqUBiTxNvJ4XhyP3KV887%2F4fLlEnoIphU9NkXlVKjSzYlS4kMMXVKRNK%2BAodefjuuUOGqOJzZKnbT8dPbhUGnJd8ptrE8wXV4s%2FAq%2FA%2B6pvM9Sm3RZBql1FXZbdIAzoKRRwaedULtzMDb8WdHYNArYfr6EtWouxgvScicAze9%2FRWY4gEpBFNzQlxBwXvShTHvOnDVbW7WQWnKTBIh6HV0id7Kvj%2FAEbo&X-Amz-Signature=73cfd1c6e034a5c3cffedc41620706f6dba69205c6e3fce7aa01be94f064124e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VO7YGMYV%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015258Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIC8yw%2BCDoeJxbFOVxv87pq7QyCW7w5IaA8YCXuCyqk%2FlAiEAoyeCLjwRyG2g%2FOwSOLVIhHy6clc8XTRU48GedsakW%2B4q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDEnaqcZspDsa6rFR5SrcA5hnwTuWsLh9ZJ9hsHcaaU%2FF5Mg8DSVozIYQacCZx3kAjzdT4iV8tAG8QICgWouwA8CytFze8n7AxRU9WunW1GBlwsHjj3L4Yo8q3Nii3QxFo31wrP0k6RfHOOrfSle3OHqhv4R822UGP%2BsBinw8tP9NtDvE8prFHY0%2Fq4gW50DYiPwNpSVyZrHAtxIaUwD9XIzpz5gZCwZ0c752qo32ra1JwSc8%2FaOooTPCK2%2FBBRWl4VrB3ttqWZHuMJDMhn2ZK9cJpzaZrGUknzkEoug13fweHoGKvmzBHq5XkDgFRx2N6%2F6MMgDsZLKoJ%2B01D82X5fPu9rqtbXMYQkDXGLJP1peVpSaJlSXhzpuGFOc4B0R8dzRCilGNLNTgQ3b5zBcnOkF%2FXS1TWJ3PFVdvB3%2FutvpvvQQqVT8226i3jmP99Ll%2Baj2l17RBCcOaM2Gzclk29091qwZRN6s5aMmGZsCzjy9XtdKBQ8wWtD9cZtsuZDrSPFekITvBGFEOqLuAZ%2B1J6P6ny3qneU%2BGLFQuHg4qSMzK7L0B91v3ZwV4S0qM5buOW1Ezvz5MQbdoGkL7fJUP3YwEnFoe1iv6%2BmNjWqaiMO0vZLlmS8f1fF7iG3qXdDWj%2FVTyx4tv53tcQ0coMJXr8tQGOqUBiTxNvJ4XhyP3KV887%2F4fLlEnoIphU9NkXlVKjSzYlS4kMMXVKRNK%2BAodefjuuUOGqOJzZKnbT8dPbhUGnJd8ptrE8wXV4s%2FAq%2FA%2B6pvM9Sm3RZBql1FXZbdIAzoKRRwaedULtzMDb8WdHYNArYfr6EtWouxgvScicAze9%2FRWY4gEpBFNzQlxBwXvShTHvOnDVbW7WQWnKTBIh6HV0id7Kvj%2FAEbo&X-Amz-Signature=e1b205387156dcedc6e48b4baa73ef554c304ae99654ccd549d8e67060285654&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
