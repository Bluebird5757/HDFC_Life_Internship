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
fetched_at: '2026-11-06T04:09:06.251Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QANZYDMP%2F20261106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261106T040903Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJGMEQCIAczHjZplEYnWDmhwwzQvNFqdlMwmKncSEUnjRevCD0PAiA6u4MJN0c%2FrnUWkFnWD9nq5yHsMLhqiDH4YcGTxj5%2FFCqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHOg06bSAy91kTkScKtwDEz%2BDt6tbh6wMPwzZilap6JofVhqG%2FJFw02ErLUN852CYC%2BtoppeIrqDE2rX2KP9ZCG%2F5AuYuObCXyGpPqjmV%2Fj%2FpVyW4DrRYAHvkhoras38zU2ilqvSRPqSnK27HUXPb53nby9z0G2dP3UCzohocfUWGz3SD3fiWJCTrr%2FBPWzoEuP7ImWQNkLjaazvMp4cgM4ed5LyPNAK9N%2BLz0YZEVTZtoa7RRsC4na%2B%2BKBMUiXnHVPbEhxeYpTjZPuQjI6cCEPKrNa7AHYm6Mtv%2BIizpdu2%2FT4RcvwojfSfhT3UBzxDx%2FI%2FD21a3BFQ70MhdtfS7q5mhDpNuKh%2BYYTbSwwmlWs7DIMsPyKx9LoZSgzsbd3TfQ2TfgVzmWU9i%2BCt7pOZjcHOTQdk3l%2F1AeGoPA6YWs0F0NT7N7r104cZv%2F%2BnypXaZ0XIeG1nhk%2FNPNxzK86Az7VRFAixE8ieNmJhokeTke9iUpnwKGIYPwteKTVTO0khYsYBOw9Mw1q%2BQumzYHlbn9cWAYSnpcYzgSvhcG1FsmkuIl9INzjNqs4CKSGmoNx6NbpOexTGS6okTd87DNP0973PRppgGDudnONV5qTOwGjddCZ8ZOArs60z9JjwuMlP4SwO7QC7ICdJjTVQwpYi11wY6pgEkW2yUW0X69HqMBE5Qn5bhpRHbJkSQ1iM0vzr25fpy2uR4a6%2Fdn2mgPxJUav7I5rnRkSXVmBYecCx2yH8IbX6wTzWgi81yCyjfyYz6WNbCZyGfEkLer%2FEcz5jinT3tz9XXlyNlTVVVIv2s0G3G9rn6EMlRNYkAmoN7TIqg3UeGLFWbwaB6%2F6iOZMTHXSjjx7S5KfQkm3OePrQ6U4O6E55dQTUQR%2Fa%2B&X-Amz-Signature=6be0517c6e80269db82cec4e2995588089f3ef93644088a5ad1c4b68887df957&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QANZYDMP%2F20261106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261106T040903Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJGMEQCIAczHjZplEYnWDmhwwzQvNFqdlMwmKncSEUnjRevCD0PAiA6u4MJN0c%2FrnUWkFnWD9nq5yHsMLhqiDH4YcGTxj5%2FFCqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHOg06bSAy91kTkScKtwDEz%2BDt6tbh6wMPwzZilap6JofVhqG%2FJFw02ErLUN852CYC%2BtoppeIrqDE2rX2KP9ZCG%2F5AuYuObCXyGpPqjmV%2Fj%2FpVyW4DrRYAHvkhoras38zU2ilqvSRPqSnK27HUXPb53nby9z0G2dP3UCzohocfUWGz3SD3fiWJCTrr%2FBPWzoEuP7ImWQNkLjaazvMp4cgM4ed5LyPNAK9N%2BLz0YZEVTZtoa7RRsC4na%2B%2BKBMUiXnHVPbEhxeYpTjZPuQjI6cCEPKrNa7AHYm6Mtv%2BIizpdu2%2FT4RcvwojfSfhT3UBzxDx%2FI%2FD21a3BFQ70MhdtfS7q5mhDpNuKh%2BYYTbSwwmlWs7DIMsPyKx9LoZSgzsbd3TfQ2TfgVzmWU9i%2BCt7pOZjcHOTQdk3l%2F1AeGoPA6YWs0F0NT7N7r104cZv%2F%2BnypXaZ0XIeG1nhk%2FNPNxzK86Az7VRFAixE8ieNmJhokeTke9iUpnwKGIYPwteKTVTO0khYsYBOw9Mw1q%2BQumzYHlbn9cWAYSnpcYzgSvhcG1FsmkuIl9INzjNqs4CKSGmoNx6NbpOexTGS6okTd87DNP0973PRppgGDudnONV5qTOwGjddCZ8ZOArs60z9JjwuMlP4SwO7QC7ICdJjTVQwpYi11wY6pgEkW2yUW0X69HqMBE5Qn5bhpRHbJkSQ1iM0vzr25fpy2uR4a6%2Fdn2mgPxJUav7I5rnRkSXVmBYecCx2yH8IbX6wTzWgi81yCyjfyYz6WNbCZyGfEkLer%2FEcz5jinT3tz9XXlyNlTVVVIv2s0G3G9rn6EMlRNYkAmoN7TIqg3UeGLFWbwaB6%2F6iOZMTHXSjjx7S5KfQkm3OePrQ6U4O6E55dQTUQR%2Fa%2B&X-Amz-Signature=180b680b2d2fff467f088e9be66cfc7c54a5f77b5de684544314ed1877fe3880&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QANZYDMP%2F20261106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261106T040903Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJGMEQCIAczHjZplEYnWDmhwwzQvNFqdlMwmKncSEUnjRevCD0PAiA6u4MJN0c%2FrnUWkFnWD9nq5yHsMLhqiDH4YcGTxj5%2FFCqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHOg06bSAy91kTkScKtwDEz%2BDt6tbh6wMPwzZilap6JofVhqG%2FJFw02ErLUN852CYC%2BtoppeIrqDE2rX2KP9ZCG%2F5AuYuObCXyGpPqjmV%2Fj%2FpVyW4DrRYAHvkhoras38zU2ilqvSRPqSnK27HUXPb53nby9z0G2dP3UCzohocfUWGz3SD3fiWJCTrr%2FBPWzoEuP7ImWQNkLjaazvMp4cgM4ed5LyPNAK9N%2BLz0YZEVTZtoa7RRsC4na%2B%2BKBMUiXnHVPbEhxeYpTjZPuQjI6cCEPKrNa7AHYm6Mtv%2BIizpdu2%2FT4RcvwojfSfhT3UBzxDx%2FI%2FD21a3BFQ70MhdtfS7q5mhDpNuKh%2BYYTbSwwmlWs7DIMsPyKx9LoZSgzsbd3TfQ2TfgVzmWU9i%2BCt7pOZjcHOTQdk3l%2F1AeGoPA6YWs0F0NT7N7r104cZv%2F%2BnypXaZ0XIeG1nhk%2FNPNxzK86Az7VRFAixE8ieNmJhokeTke9iUpnwKGIYPwteKTVTO0khYsYBOw9Mw1q%2BQumzYHlbn9cWAYSnpcYzgSvhcG1FsmkuIl9INzjNqs4CKSGmoNx6NbpOexTGS6okTd87DNP0973PRppgGDudnONV5qTOwGjddCZ8ZOArs60z9JjwuMlP4SwO7QC7ICdJjTVQwpYi11wY6pgEkW2yUW0X69HqMBE5Qn5bhpRHbJkSQ1iM0vzr25fpy2uR4a6%2Fdn2mgPxJUav7I5rnRkSXVmBYecCx2yH8IbX6wTzWgi81yCyjfyYz6WNbCZyGfEkLer%2FEcz5jinT3tz9XXlyNlTVVVIv2s0G3G9rn6EMlRNYkAmoN7TIqg3UeGLFWbwaB6%2F6iOZMTHXSjjx7S5KfQkm3OePrQ6U4O6E55dQTUQR%2Fa%2B&X-Amz-Signature=c8d3e8e81572ce3794bf5d287923370301cc1a85035c9f3e6202ff1df2672794&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QANZYDMP%2F20261106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261106T040903Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJGMEQCIAczHjZplEYnWDmhwwzQvNFqdlMwmKncSEUnjRevCD0PAiA6u4MJN0c%2FrnUWkFnWD9nq5yHsMLhqiDH4YcGTxj5%2FFCqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHOg06bSAy91kTkScKtwDEz%2BDt6tbh6wMPwzZilap6JofVhqG%2FJFw02ErLUN852CYC%2BtoppeIrqDE2rX2KP9ZCG%2F5AuYuObCXyGpPqjmV%2Fj%2FpVyW4DrRYAHvkhoras38zU2ilqvSRPqSnK27HUXPb53nby9z0G2dP3UCzohocfUWGz3SD3fiWJCTrr%2FBPWzoEuP7ImWQNkLjaazvMp4cgM4ed5LyPNAK9N%2BLz0YZEVTZtoa7RRsC4na%2B%2BKBMUiXnHVPbEhxeYpTjZPuQjI6cCEPKrNa7AHYm6Mtv%2BIizpdu2%2FT4RcvwojfSfhT3UBzxDx%2FI%2FD21a3BFQ70MhdtfS7q5mhDpNuKh%2BYYTbSwwmlWs7DIMsPyKx9LoZSgzsbd3TfQ2TfgVzmWU9i%2BCt7pOZjcHOTQdk3l%2F1AeGoPA6YWs0F0NT7N7r104cZv%2F%2BnypXaZ0XIeG1nhk%2FNPNxzK86Az7VRFAixE8ieNmJhokeTke9iUpnwKGIYPwteKTVTO0khYsYBOw9Mw1q%2BQumzYHlbn9cWAYSnpcYzgSvhcG1FsmkuIl9INzjNqs4CKSGmoNx6NbpOexTGS6okTd87DNP0973PRppgGDudnONV5qTOwGjddCZ8ZOArs60z9JjwuMlP4SwO7QC7ICdJjTVQwpYi11wY6pgEkW2yUW0X69HqMBE5Qn5bhpRHbJkSQ1iM0vzr25fpy2uR4a6%2Fdn2mgPxJUav7I5rnRkSXVmBYecCx2yH8IbX6wTzWgi81yCyjfyYz6WNbCZyGfEkLer%2FEcz5jinT3tz9XXlyNlTVVVIv2s0G3G9rn6EMlRNYkAmoN7TIqg3UeGLFWbwaB6%2F6iOZMTHXSjjx7S5KfQkm3OePrQ6U4O6E55dQTUQR%2Fa%2B&X-Amz-Signature=61c468ddec1640401d06be21abe3d30ac00d7cd62c45f96447b6d4e22ce09e10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QANZYDMP%2F20261106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261106T040903Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJGMEQCIAczHjZplEYnWDmhwwzQvNFqdlMwmKncSEUnjRevCD0PAiA6u4MJN0c%2FrnUWkFnWD9nq5yHsMLhqiDH4YcGTxj5%2FFCqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHOg06bSAy91kTkScKtwDEz%2BDt6tbh6wMPwzZilap6JofVhqG%2FJFw02ErLUN852CYC%2BtoppeIrqDE2rX2KP9ZCG%2F5AuYuObCXyGpPqjmV%2Fj%2FpVyW4DrRYAHvkhoras38zU2ilqvSRPqSnK27HUXPb53nby9z0G2dP3UCzohocfUWGz3SD3fiWJCTrr%2FBPWzoEuP7ImWQNkLjaazvMp4cgM4ed5LyPNAK9N%2BLz0YZEVTZtoa7RRsC4na%2B%2BKBMUiXnHVPbEhxeYpTjZPuQjI6cCEPKrNa7AHYm6Mtv%2BIizpdu2%2FT4RcvwojfSfhT3UBzxDx%2FI%2FD21a3BFQ70MhdtfS7q5mhDpNuKh%2BYYTbSwwmlWs7DIMsPyKx9LoZSgzsbd3TfQ2TfgVzmWU9i%2BCt7pOZjcHOTQdk3l%2F1AeGoPA6YWs0F0NT7N7r104cZv%2F%2BnypXaZ0XIeG1nhk%2FNPNxzK86Az7VRFAixE8ieNmJhokeTke9iUpnwKGIYPwteKTVTO0khYsYBOw9Mw1q%2BQumzYHlbn9cWAYSnpcYzgSvhcG1FsmkuIl9INzjNqs4CKSGmoNx6NbpOexTGS6okTd87DNP0973PRppgGDudnONV5qTOwGjddCZ8ZOArs60z9JjwuMlP4SwO7QC7ICdJjTVQwpYi11wY6pgEkW2yUW0X69HqMBE5Qn5bhpRHbJkSQ1iM0vzr25fpy2uR4a6%2Fdn2mgPxJUav7I5rnRkSXVmBYecCx2yH8IbX6wTzWgi81yCyjfyYz6WNbCZyGfEkLer%2FEcz5jinT3tz9XXlyNlTVVVIv2s0G3G9rn6EMlRNYkAmoN7TIqg3UeGLFWbwaB6%2F6iOZMTHXSjjx7S5KfQkm3OePrQ6U4O6E55dQTUQR%2Fa%2B&X-Amz-Signature=43f1e7b68cff801617fec1501ec34287be121b42a8a70617a9d931812cb16156&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QANZYDMP%2F20261106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261106T040903Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJGMEQCIAczHjZplEYnWDmhwwzQvNFqdlMwmKncSEUnjRevCD0PAiA6u4MJN0c%2FrnUWkFnWD9nq5yHsMLhqiDH4YcGTxj5%2FFCqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHOg06bSAy91kTkScKtwDEz%2BDt6tbh6wMPwzZilap6JofVhqG%2FJFw02ErLUN852CYC%2BtoppeIrqDE2rX2KP9ZCG%2F5AuYuObCXyGpPqjmV%2Fj%2FpVyW4DrRYAHvkhoras38zU2ilqvSRPqSnK27HUXPb53nby9z0G2dP3UCzohocfUWGz3SD3fiWJCTrr%2FBPWzoEuP7ImWQNkLjaazvMp4cgM4ed5LyPNAK9N%2BLz0YZEVTZtoa7RRsC4na%2B%2BKBMUiXnHVPbEhxeYpTjZPuQjI6cCEPKrNa7AHYm6Mtv%2BIizpdu2%2FT4RcvwojfSfhT3UBzxDx%2FI%2FD21a3BFQ70MhdtfS7q5mhDpNuKh%2BYYTbSwwmlWs7DIMsPyKx9LoZSgzsbd3TfQ2TfgVzmWU9i%2BCt7pOZjcHOTQdk3l%2F1AeGoPA6YWs0F0NT7N7r104cZv%2F%2BnypXaZ0XIeG1nhk%2FNPNxzK86Az7VRFAixE8ieNmJhokeTke9iUpnwKGIYPwteKTVTO0khYsYBOw9Mw1q%2BQumzYHlbn9cWAYSnpcYzgSvhcG1FsmkuIl9INzjNqs4CKSGmoNx6NbpOexTGS6okTd87DNP0973PRppgGDudnONV5qTOwGjddCZ8ZOArs60z9JjwuMlP4SwO7QC7ICdJjTVQwpYi11wY6pgEkW2yUW0X69HqMBE5Qn5bhpRHbJkSQ1iM0vzr25fpy2uR4a6%2Fdn2mgPxJUav7I5rnRkSXVmBYecCx2yH8IbX6wTzWgi81yCyjfyYz6WNbCZyGfEkLer%2FEcz5jinT3tz9XXlyNlTVVVIv2s0G3G9rn6EMlRNYkAmoN7TIqg3UeGLFWbwaB6%2F6iOZMTHXSjjx7S5KfQkm3OePrQ6U4O6E55dQTUQR%2Fa%2B&X-Amz-Signature=8cc241b48e4e2ad708d0691ed5f7be4116ef1d64879a8a0b9be9f77bf6e07362&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QANZYDMP%2F20261106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261106T040903Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJGMEQCIAczHjZplEYnWDmhwwzQvNFqdlMwmKncSEUnjRevCD0PAiA6u4MJN0c%2FrnUWkFnWD9nq5yHsMLhqiDH4YcGTxj5%2FFCqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHOg06bSAy91kTkScKtwDEz%2BDt6tbh6wMPwzZilap6JofVhqG%2FJFw02ErLUN852CYC%2BtoppeIrqDE2rX2KP9ZCG%2F5AuYuObCXyGpPqjmV%2Fj%2FpVyW4DrRYAHvkhoras38zU2ilqvSRPqSnK27HUXPb53nby9z0G2dP3UCzohocfUWGz3SD3fiWJCTrr%2FBPWzoEuP7ImWQNkLjaazvMp4cgM4ed5LyPNAK9N%2BLz0YZEVTZtoa7RRsC4na%2B%2BKBMUiXnHVPbEhxeYpTjZPuQjI6cCEPKrNa7AHYm6Mtv%2BIizpdu2%2FT4RcvwojfSfhT3UBzxDx%2FI%2FD21a3BFQ70MhdtfS7q5mhDpNuKh%2BYYTbSwwmlWs7DIMsPyKx9LoZSgzsbd3TfQ2TfgVzmWU9i%2BCt7pOZjcHOTQdk3l%2F1AeGoPA6YWs0F0NT7N7r104cZv%2F%2BnypXaZ0XIeG1nhk%2FNPNxzK86Az7VRFAixE8ieNmJhokeTke9iUpnwKGIYPwteKTVTO0khYsYBOw9Mw1q%2BQumzYHlbn9cWAYSnpcYzgSvhcG1FsmkuIl9INzjNqs4CKSGmoNx6NbpOexTGS6okTd87DNP0973PRppgGDudnONV5qTOwGjddCZ8ZOArs60z9JjwuMlP4SwO7QC7ICdJjTVQwpYi11wY6pgEkW2yUW0X69HqMBE5Qn5bhpRHbJkSQ1iM0vzr25fpy2uR4a6%2Fdn2mgPxJUav7I5rnRkSXVmBYecCx2yH8IbX6wTzWgi81yCyjfyYz6WNbCZyGfEkLer%2FEcz5jinT3tz9XXlyNlTVVVIv2s0G3G9rn6EMlRNYkAmoN7TIqg3UeGLFWbwaB6%2F6iOZMTHXSjjx7S5KfQkm3OePrQ6U4O6E55dQTUQR%2Fa%2B&X-Amz-Signature=b4576005d30cf76774c33bb773b01c728929c6e88f018b57d77d0dce3f273f0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QANZYDMP%2F20261106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261106T040903Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJGMEQCIAczHjZplEYnWDmhwwzQvNFqdlMwmKncSEUnjRevCD0PAiA6u4MJN0c%2FrnUWkFnWD9nq5yHsMLhqiDH4YcGTxj5%2FFCqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHOg06bSAy91kTkScKtwDEz%2BDt6tbh6wMPwzZilap6JofVhqG%2FJFw02ErLUN852CYC%2BtoppeIrqDE2rX2KP9ZCG%2F5AuYuObCXyGpPqjmV%2Fj%2FpVyW4DrRYAHvkhoras38zU2ilqvSRPqSnK27HUXPb53nby9z0G2dP3UCzohocfUWGz3SD3fiWJCTrr%2FBPWzoEuP7ImWQNkLjaazvMp4cgM4ed5LyPNAK9N%2BLz0YZEVTZtoa7RRsC4na%2B%2BKBMUiXnHVPbEhxeYpTjZPuQjI6cCEPKrNa7AHYm6Mtv%2BIizpdu2%2FT4RcvwojfSfhT3UBzxDx%2FI%2FD21a3BFQ70MhdtfS7q5mhDpNuKh%2BYYTbSwwmlWs7DIMsPyKx9LoZSgzsbd3TfQ2TfgVzmWU9i%2BCt7pOZjcHOTQdk3l%2F1AeGoPA6YWs0F0NT7N7r104cZv%2F%2BnypXaZ0XIeG1nhk%2FNPNxzK86Az7VRFAixE8ieNmJhokeTke9iUpnwKGIYPwteKTVTO0khYsYBOw9Mw1q%2BQumzYHlbn9cWAYSnpcYzgSvhcG1FsmkuIl9INzjNqs4CKSGmoNx6NbpOexTGS6okTd87DNP0973PRppgGDudnONV5qTOwGjddCZ8ZOArs60z9JjwuMlP4SwO7QC7ICdJjTVQwpYi11wY6pgEkW2yUW0X69HqMBE5Qn5bhpRHbJkSQ1iM0vzr25fpy2uR4a6%2Fdn2mgPxJUav7I5rnRkSXVmBYecCx2yH8IbX6wTzWgi81yCyjfyYz6WNbCZyGfEkLer%2FEcz5jinT3tz9XXlyNlTVVVIv2s0G3G9rn6EMlRNYkAmoN7TIqg3UeGLFWbwaB6%2F6iOZMTHXSjjx7S5KfQkm3OePrQ6U4O6E55dQTUQR%2Fa%2B&X-Amz-Signature=18fd4a8c19091407803cb74bc3e8bc14b2e0afd4bfa97c6400fe11f6d931bffa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QANZYDMP%2F20261106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261106T040903Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJGMEQCIAczHjZplEYnWDmhwwzQvNFqdlMwmKncSEUnjRevCD0PAiA6u4MJN0c%2FrnUWkFnWD9nq5yHsMLhqiDH4YcGTxj5%2FFCqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHOg06bSAy91kTkScKtwDEz%2BDt6tbh6wMPwzZilap6JofVhqG%2FJFw02ErLUN852CYC%2BtoppeIrqDE2rX2KP9ZCG%2F5AuYuObCXyGpPqjmV%2Fj%2FpVyW4DrRYAHvkhoras38zU2ilqvSRPqSnK27HUXPb53nby9z0G2dP3UCzohocfUWGz3SD3fiWJCTrr%2FBPWzoEuP7ImWQNkLjaazvMp4cgM4ed5LyPNAK9N%2BLz0YZEVTZtoa7RRsC4na%2B%2BKBMUiXnHVPbEhxeYpTjZPuQjI6cCEPKrNa7AHYm6Mtv%2BIizpdu2%2FT4RcvwojfSfhT3UBzxDx%2FI%2FD21a3BFQ70MhdtfS7q5mhDpNuKh%2BYYTbSwwmlWs7DIMsPyKx9LoZSgzsbd3TfQ2TfgVzmWU9i%2BCt7pOZjcHOTQdk3l%2F1AeGoPA6YWs0F0NT7N7r104cZv%2F%2BnypXaZ0XIeG1nhk%2FNPNxzK86Az7VRFAixE8ieNmJhokeTke9iUpnwKGIYPwteKTVTO0khYsYBOw9Mw1q%2BQumzYHlbn9cWAYSnpcYzgSvhcG1FsmkuIl9INzjNqs4CKSGmoNx6NbpOexTGS6okTd87DNP0973PRppgGDudnONV5qTOwGjddCZ8ZOArs60z9JjwuMlP4SwO7QC7ICdJjTVQwpYi11wY6pgEkW2yUW0X69HqMBE5Qn5bhpRHbJkSQ1iM0vzr25fpy2uR4a6%2Fdn2mgPxJUav7I5rnRkSXVmBYecCx2yH8IbX6wTzWgi81yCyjfyYz6WNbCZyGfEkLer%2FEcz5jinT3tz9XXlyNlTVVVIv2s0G3G9rn6EMlRNYkAmoN7TIqg3UeGLFWbwaB6%2F6iOZMTHXSjjx7S5KfQkm3OePrQ6U4O6E55dQTUQR%2Fa%2B&X-Amz-Signature=20ee5c51cf9989f01a01eacf4be78249936b22978e51342f57e53b87285063e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
