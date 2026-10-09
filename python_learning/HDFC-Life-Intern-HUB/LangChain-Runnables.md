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
fetched_at: '2026-10-09T03:37:30.307Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OQEKAN3%2F20261009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261009T033726Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQDGGNOx42nmrLnKMT08UV52V0PCE7uBO%2Ba8%2Bhm8amBthAIgfmHmMCu9oMrZDMRLfzqec9wsBCmogGmYL1NxgS050Voq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDG%2FxiikrH55DBQeNkSrcAxjz5rFxCj0Q%2FizgQ7RhLlgTgsb4WuwLvmUZH0MraylF2n8E6Vh0cwiqNzWkAEpwYWD5ZjdE847x1IxlyUfCA2ZJwdXtSuYFoLZTZDDrmySddnKKlrIh6KTBK%2FKTKLP5RyV64bE%2BKxy95bquQ%2FUKyJs2%2Bg0Yj54IfyT6B6OlAyFefwmpcLuU3Hw%2BmzCzk8Z09aj70yWFVsQDzpsMG3Aa5WnmLNTYAvWGsYbX05I9PbBa0svykwszfZjJpL3UdIzemqAgYlGb65RDZ1xUxWSSC1%2BEYIKOykL4DSJTkJIK5CM7L2pIwQn6iwILxFhihkRHuZ1T39i6QYMTABjP1k6UrUXL%2FBmTMhaeVhCkMp%2BeKjMMvDCVgcDnOpTDa9AaqyYqSxEB%2BvsMFZ3TpuFp09e8V8tXixQlAmAzvj35FqgjD0dIgl2krmorZmtcsShkDDIC8I71Z3tJlFUiPX67BRM8HCTWr0Tpt2RLJDc7YTmPNsa3kOmErxYBUxPN5zz1dpF2eo6YFyn62avCl7PNl3%2FqtYg0l8Y7m0Q4LSZDNmX5G6lETLQIYFD%2FLri%2FjbdqwqvVvrFK%2BLFS2xhGy9zD8Dx7I%2Fy7NcZZpOJJbcD7MF4Sgq3V3lQqqdQ1Ut9gy2kWMOq%2BodYGOqUBUKZpRiHyWjroyeQQQFWcxMZIeb8oJiH3Mi4u1g0fiTZXpN%2BnP%2BBx86tk1KbTjZYcIU0b1tF9psvFWEuDAKz1ZILmgY%2FMeYVOTQPLTCa4AiSv5xQivNGvfAvNS9Salp8VxxZX6d6KgwobLogPSmny9U7zj7fnz%2FYnTwuPZ3XH2re4Xx%2F4h1QvI7APKSd18AfVAXq0UeTfWZKH4EsAQSwMAjPCGV4H&X-Amz-Signature=7465516464bdebda74e8af40f8edf580bbdb578c4f483018c780c1aa1bc100be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OQEKAN3%2F20261009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261009T033726Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQDGGNOx42nmrLnKMT08UV52V0PCE7uBO%2Ba8%2Bhm8amBthAIgfmHmMCu9oMrZDMRLfzqec9wsBCmogGmYL1NxgS050Voq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDG%2FxiikrH55DBQeNkSrcAxjz5rFxCj0Q%2FizgQ7RhLlgTgsb4WuwLvmUZH0MraylF2n8E6Vh0cwiqNzWkAEpwYWD5ZjdE847x1IxlyUfCA2ZJwdXtSuYFoLZTZDDrmySddnKKlrIh6KTBK%2FKTKLP5RyV64bE%2BKxy95bquQ%2FUKyJs2%2Bg0Yj54IfyT6B6OlAyFefwmpcLuU3Hw%2BmzCzk8Z09aj70yWFVsQDzpsMG3Aa5WnmLNTYAvWGsYbX05I9PbBa0svykwszfZjJpL3UdIzemqAgYlGb65RDZ1xUxWSSC1%2BEYIKOykL4DSJTkJIK5CM7L2pIwQn6iwILxFhihkRHuZ1T39i6QYMTABjP1k6UrUXL%2FBmTMhaeVhCkMp%2BeKjMMvDCVgcDnOpTDa9AaqyYqSxEB%2BvsMFZ3TpuFp09e8V8tXixQlAmAzvj35FqgjD0dIgl2krmorZmtcsShkDDIC8I71Z3tJlFUiPX67BRM8HCTWr0Tpt2RLJDc7YTmPNsa3kOmErxYBUxPN5zz1dpF2eo6YFyn62avCl7PNl3%2FqtYg0l8Y7m0Q4LSZDNmX5G6lETLQIYFD%2FLri%2FjbdqwqvVvrFK%2BLFS2xhGy9zD8Dx7I%2Fy7NcZZpOJJbcD7MF4Sgq3V3lQqqdQ1Ut9gy2kWMOq%2BodYGOqUBUKZpRiHyWjroyeQQQFWcxMZIeb8oJiH3Mi4u1g0fiTZXpN%2BnP%2BBx86tk1KbTjZYcIU0b1tF9psvFWEuDAKz1ZILmgY%2FMeYVOTQPLTCa4AiSv5xQivNGvfAvNS9Salp8VxxZX6d6KgwobLogPSmny9U7zj7fnz%2FYnTwuPZ3XH2re4Xx%2F4h1QvI7APKSd18AfVAXq0UeTfWZKH4EsAQSwMAjPCGV4H&X-Amz-Signature=bf481d9a585e969498a3c4ba2564b85ec82f97d781b69cdf791a20c4252c1f1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OQEKAN3%2F20261009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261009T033726Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQDGGNOx42nmrLnKMT08UV52V0PCE7uBO%2Ba8%2Bhm8amBthAIgfmHmMCu9oMrZDMRLfzqec9wsBCmogGmYL1NxgS050Voq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDG%2FxiikrH55DBQeNkSrcAxjz5rFxCj0Q%2FizgQ7RhLlgTgsb4WuwLvmUZH0MraylF2n8E6Vh0cwiqNzWkAEpwYWD5ZjdE847x1IxlyUfCA2ZJwdXtSuYFoLZTZDDrmySddnKKlrIh6KTBK%2FKTKLP5RyV64bE%2BKxy95bquQ%2FUKyJs2%2Bg0Yj54IfyT6B6OlAyFefwmpcLuU3Hw%2BmzCzk8Z09aj70yWFVsQDzpsMG3Aa5WnmLNTYAvWGsYbX05I9PbBa0svykwszfZjJpL3UdIzemqAgYlGb65RDZ1xUxWSSC1%2BEYIKOykL4DSJTkJIK5CM7L2pIwQn6iwILxFhihkRHuZ1T39i6QYMTABjP1k6UrUXL%2FBmTMhaeVhCkMp%2BeKjMMvDCVgcDnOpTDa9AaqyYqSxEB%2BvsMFZ3TpuFp09e8V8tXixQlAmAzvj35FqgjD0dIgl2krmorZmtcsShkDDIC8I71Z3tJlFUiPX67BRM8HCTWr0Tpt2RLJDc7YTmPNsa3kOmErxYBUxPN5zz1dpF2eo6YFyn62avCl7PNl3%2FqtYg0l8Y7m0Q4LSZDNmX5G6lETLQIYFD%2FLri%2FjbdqwqvVvrFK%2BLFS2xhGy9zD8Dx7I%2Fy7NcZZpOJJbcD7MF4Sgq3V3lQqqdQ1Ut9gy2kWMOq%2BodYGOqUBUKZpRiHyWjroyeQQQFWcxMZIeb8oJiH3Mi4u1g0fiTZXpN%2BnP%2BBx86tk1KbTjZYcIU0b1tF9psvFWEuDAKz1ZILmgY%2FMeYVOTQPLTCa4AiSv5xQivNGvfAvNS9Salp8VxxZX6d6KgwobLogPSmny9U7zj7fnz%2FYnTwuPZ3XH2re4Xx%2F4h1QvI7APKSd18AfVAXq0UeTfWZKH4EsAQSwMAjPCGV4H&X-Amz-Signature=a1ed7c80963933cf9e40e60e73b5718acdc95a3f53a55cf7239e3f0ea0edf2fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OQEKAN3%2F20261009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261009T033726Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQDGGNOx42nmrLnKMT08UV52V0PCE7uBO%2Ba8%2Bhm8amBthAIgfmHmMCu9oMrZDMRLfzqec9wsBCmogGmYL1NxgS050Voq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDG%2FxiikrH55DBQeNkSrcAxjz5rFxCj0Q%2FizgQ7RhLlgTgsb4WuwLvmUZH0MraylF2n8E6Vh0cwiqNzWkAEpwYWD5ZjdE847x1IxlyUfCA2ZJwdXtSuYFoLZTZDDrmySddnKKlrIh6KTBK%2FKTKLP5RyV64bE%2BKxy95bquQ%2FUKyJs2%2Bg0Yj54IfyT6B6OlAyFefwmpcLuU3Hw%2BmzCzk8Z09aj70yWFVsQDzpsMG3Aa5WnmLNTYAvWGsYbX05I9PbBa0svykwszfZjJpL3UdIzemqAgYlGb65RDZ1xUxWSSC1%2BEYIKOykL4DSJTkJIK5CM7L2pIwQn6iwILxFhihkRHuZ1T39i6QYMTABjP1k6UrUXL%2FBmTMhaeVhCkMp%2BeKjMMvDCVgcDnOpTDa9AaqyYqSxEB%2BvsMFZ3TpuFp09e8V8tXixQlAmAzvj35FqgjD0dIgl2krmorZmtcsShkDDIC8I71Z3tJlFUiPX67BRM8HCTWr0Tpt2RLJDc7YTmPNsa3kOmErxYBUxPN5zz1dpF2eo6YFyn62avCl7PNl3%2FqtYg0l8Y7m0Q4LSZDNmX5G6lETLQIYFD%2FLri%2FjbdqwqvVvrFK%2BLFS2xhGy9zD8Dx7I%2Fy7NcZZpOJJbcD7MF4Sgq3V3lQqqdQ1Ut9gy2kWMOq%2BodYGOqUBUKZpRiHyWjroyeQQQFWcxMZIeb8oJiH3Mi4u1g0fiTZXpN%2BnP%2BBx86tk1KbTjZYcIU0b1tF9psvFWEuDAKz1ZILmgY%2FMeYVOTQPLTCa4AiSv5xQivNGvfAvNS9Salp8VxxZX6d6KgwobLogPSmny9U7zj7fnz%2FYnTwuPZ3XH2re4Xx%2F4h1QvI7APKSd18AfVAXq0UeTfWZKH4EsAQSwMAjPCGV4H&X-Amz-Signature=35849af8d6d715d6d966a3719657d10d0040624ed999fb004558049c490b0beb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OQEKAN3%2F20261009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261009T033726Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQDGGNOx42nmrLnKMT08UV52V0PCE7uBO%2Ba8%2Bhm8amBthAIgfmHmMCu9oMrZDMRLfzqec9wsBCmogGmYL1NxgS050Voq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDG%2FxiikrH55DBQeNkSrcAxjz5rFxCj0Q%2FizgQ7RhLlgTgsb4WuwLvmUZH0MraylF2n8E6Vh0cwiqNzWkAEpwYWD5ZjdE847x1IxlyUfCA2ZJwdXtSuYFoLZTZDDrmySddnKKlrIh6KTBK%2FKTKLP5RyV64bE%2BKxy95bquQ%2FUKyJs2%2Bg0Yj54IfyT6B6OlAyFefwmpcLuU3Hw%2BmzCzk8Z09aj70yWFVsQDzpsMG3Aa5WnmLNTYAvWGsYbX05I9PbBa0svykwszfZjJpL3UdIzemqAgYlGb65RDZ1xUxWSSC1%2BEYIKOykL4DSJTkJIK5CM7L2pIwQn6iwILxFhihkRHuZ1T39i6QYMTABjP1k6UrUXL%2FBmTMhaeVhCkMp%2BeKjMMvDCVgcDnOpTDa9AaqyYqSxEB%2BvsMFZ3TpuFp09e8V8tXixQlAmAzvj35FqgjD0dIgl2krmorZmtcsShkDDIC8I71Z3tJlFUiPX67BRM8HCTWr0Tpt2RLJDc7YTmPNsa3kOmErxYBUxPN5zz1dpF2eo6YFyn62avCl7PNl3%2FqtYg0l8Y7m0Q4LSZDNmX5G6lETLQIYFD%2FLri%2FjbdqwqvVvrFK%2BLFS2xhGy9zD8Dx7I%2Fy7NcZZpOJJbcD7MF4Sgq3V3lQqqdQ1Ut9gy2kWMOq%2BodYGOqUBUKZpRiHyWjroyeQQQFWcxMZIeb8oJiH3Mi4u1g0fiTZXpN%2BnP%2BBx86tk1KbTjZYcIU0b1tF9psvFWEuDAKz1ZILmgY%2FMeYVOTQPLTCa4AiSv5xQivNGvfAvNS9Salp8VxxZX6d6KgwobLogPSmny9U7zj7fnz%2FYnTwuPZ3XH2re4Xx%2F4h1QvI7APKSd18AfVAXq0UeTfWZKH4EsAQSwMAjPCGV4H&X-Amz-Signature=387bc7a276dac04437adbc927de8b5df1f34dbdc71c9aa6f663274617017500f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OQEKAN3%2F20261009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261009T033726Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQDGGNOx42nmrLnKMT08UV52V0PCE7uBO%2Ba8%2Bhm8amBthAIgfmHmMCu9oMrZDMRLfzqec9wsBCmogGmYL1NxgS050Voq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDG%2FxiikrH55DBQeNkSrcAxjz5rFxCj0Q%2FizgQ7RhLlgTgsb4WuwLvmUZH0MraylF2n8E6Vh0cwiqNzWkAEpwYWD5ZjdE847x1IxlyUfCA2ZJwdXtSuYFoLZTZDDrmySddnKKlrIh6KTBK%2FKTKLP5RyV64bE%2BKxy95bquQ%2FUKyJs2%2Bg0Yj54IfyT6B6OlAyFefwmpcLuU3Hw%2BmzCzk8Z09aj70yWFVsQDzpsMG3Aa5WnmLNTYAvWGsYbX05I9PbBa0svykwszfZjJpL3UdIzemqAgYlGb65RDZ1xUxWSSC1%2BEYIKOykL4DSJTkJIK5CM7L2pIwQn6iwILxFhihkRHuZ1T39i6QYMTABjP1k6UrUXL%2FBmTMhaeVhCkMp%2BeKjMMvDCVgcDnOpTDa9AaqyYqSxEB%2BvsMFZ3TpuFp09e8V8tXixQlAmAzvj35FqgjD0dIgl2krmorZmtcsShkDDIC8I71Z3tJlFUiPX67BRM8HCTWr0Tpt2RLJDc7YTmPNsa3kOmErxYBUxPN5zz1dpF2eo6YFyn62avCl7PNl3%2FqtYg0l8Y7m0Q4LSZDNmX5G6lETLQIYFD%2FLri%2FjbdqwqvVvrFK%2BLFS2xhGy9zD8Dx7I%2Fy7NcZZpOJJbcD7MF4Sgq3V3lQqqdQ1Ut9gy2kWMOq%2BodYGOqUBUKZpRiHyWjroyeQQQFWcxMZIeb8oJiH3Mi4u1g0fiTZXpN%2BnP%2BBx86tk1KbTjZYcIU0b1tF9psvFWEuDAKz1ZILmgY%2FMeYVOTQPLTCa4AiSv5xQivNGvfAvNS9Salp8VxxZX6d6KgwobLogPSmny9U7zj7fnz%2FYnTwuPZ3XH2re4Xx%2F4h1QvI7APKSd18AfVAXq0UeTfWZKH4EsAQSwMAjPCGV4H&X-Amz-Signature=382969112cdaccc49d65a9b44af8ef7204498e7280ae41e889ce3d2fcc045e1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OQEKAN3%2F20261009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261009T033726Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQDGGNOx42nmrLnKMT08UV52V0PCE7uBO%2Ba8%2Bhm8amBthAIgfmHmMCu9oMrZDMRLfzqec9wsBCmogGmYL1NxgS050Voq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDG%2FxiikrH55DBQeNkSrcAxjz5rFxCj0Q%2FizgQ7RhLlgTgsb4WuwLvmUZH0MraylF2n8E6Vh0cwiqNzWkAEpwYWD5ZjdE847x1IxlyUfCA2ZJwdXtSuYFoLZTZDDrmySddnKKlrIh6KTBK%2FKTKLP5RyV64bE%2BKxy95bquQ%2FUKyJs2%2Bg0Yj54IfyT6B6OlAyFefwmpcLuU3Hw%2BmzCzk8Z09aj70yWFVsQDzpsMG3Aa5WnmLNTYAvWGsYbX05I9PbBa0svykwszfZjJpL3UdIzemqAgYlGb65RDZ1xUxWSSC1%2BEYIKOykL4DSJTkJIK5CM7L2pIwQn6iwILxFhihkRHuZ1T39i6QYMTABjP1k6UrUXL%2FBmTMhaeVhCkMp%2BeKjMMvDCVgcDnOpTDa9AaqyYqSxEB%2BvsMFZ3TpuFp09e8V8tXixQlAmAzvj35FqgjD0dIgl2krmorZmtcsShkDDIC8I71Z3tJlFUiPX67BRM8HCTWr0Tpt2RLJDc7YTmPNsa3kOmErxYBUxPN5zz1dpF2eo6YFyn62avCl7PNl3%2FqtYg0l8Y7m0Q4LSZDNmX5G6lETLQIYFD%2FLri%2FjbdqwqvVvrFK%2BLFS2xhGy9zD8Dx7I%2Fy7NcZZpOJJbcD7MF4Sgq3V3lQqqdQ1Ut9gy2kWMOq%2BodYGOqUBUKZpRiHyWjroyeQQQFWcxMZIeb8oJiH3Mi4u1g0fiTZXpN%2BnP%2BBx86tk1KbTjZYcIU0b1tF9psvFWEuDAKz1ZILmgY%2FMeYVOTQPLTCa4AiSv5xQivNGvfAvNS9Salp8VxxZX6d6KgwobLogPSmny9U7zj7fnz%2FYnTwuPZ3XH2re4Xx%2F4h1QvI7APKSd18AfVAXq0UeTfWZKH4EsAQSwMAjPCGV4H&X-Amz-Signature=4ceeabcf245817acdd96f286f4ed8521b2eeb609937b2da3eb46536615e75cc9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OQEKAN3%2F20261009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261009T033726Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQDGGNOx42nmrLnKMT08UV52V0PCE7uBO%2Ba8%2Bhm8amBthAIgfmHmMCu9oMrZDMRLfzqec9wsBCmogGmYL1NxgS050Voq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDG%2FxiikrH55DBQeNkSrcAxjz5rFxCj0Q%2FizgQ7RhLlgTgsb4WuwLvmUZH0MraylF2n8E6Vh0cwiqNzWkAEpwYWD5ZjdE847x1IxlyUfCA2ZJwdXtSuYFoLZTZDDrmySddnKKlrIh6KTBK%2FKTKLP5RyV64bE%2BKxy95bquQ%2FUKyJs2%2Bg0Yj54IfyT6B6OlAyFefwmpcLuU3Hw%2BmzCzk8Z09aj70yWFVsQDzpsMG3Aa5WnmLNTYAvWGsYbX05I9PbBa0svykwszfZjJpL3UdIzemqAgYlGb65RDZ1xUxWSSC1%2BEYIKOykL4DSJTkJIK5CM7L2pIwQn6iwILxFhihkRHuZ1T39i6QYMTABjP1k6UrUXL%2FBmTMhaeVhCkMp%2BeKjMMvDCVgcDnOpTDa9AaqyYqSxEB%2BvsMFZ3TpuFp09e8V8tXixQlAmAzvj35FqgjD0dIgl2krmorZmtcsShkDDIC8I71Z3tJlFUiPX67BRM8HCTWr0Tpt2RLJDc7YTmPNsa3kOmErxYBUxPN5zz1dpF2eo6YFyn62avCl7PNl3%2FqtYg0l8Y7m0Q4LSZDNmX5G6lETLQIYFD%2FLri%2FjbdqwqvVvrFK%2BLFS2xhGy9zD8Dx7I%2Fy7NcZZpOJJbcD7MF4Sgq3V3lQqqdQ1Ut9gy2kWMOq%2BodYGOqUBUKZpRiHyWjroyeQQQFWcxMZIeb8oJiH3Mi4u1g0fiTZXpN%2BnP%2BBx86tk1KbTjZYcIU0b1tF9psvFWEuDAKz1ZILmgY%2FMeYVOTQPLTCa4AiSv5xQivNGvfAvNS9Salp8VxxZX6d6KgwobLogPSmny9U7zj7fnz%2FYnTwuPZ3XH2re4Xx%2F4h1QvI7APKSd18AfVAXq0UeTfWZKH4EsAQSwMAjPCGV4H&X-Amz-Signature=c0583992d41644251395f6740f981edd53a3e0a48a2ea507789fde87ae6c8f67&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OQEKAN3%2F20261009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261009T033726Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQDGGNOx42nmrLnKMT08UV52V0PCE7uBO%2Ba8%2Bhm8amBthAIgfmHmMCu9oMrZDMRLfzqec9wsBCmogGmYL1NxgS050Voq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDG%2FxiikrH55DBQeNkSrcAxjz5rFxCj0Q%2FizgQ7RhLlgTgsb4WuwLvmUZH0MraylF2n8E6Vh0cwiqNzWkAEpwYWD5ZjdE847x1IxlyUfCA2ZJwdXtSuYFoLZTZDDrmySddnKKlrIh6KTBK%2FKTKLP5RyV64bE%2BKxy95bquQ%2FUKyJs2%2Bg0Yj54IfyT6B6OlAyFefwmpcLuU3Hw%2BmzCzk8Z09aj70yWFVsQDzpsMG3Aa5WnmLNTYAvWGsYbX05I9PbBa0svykwszfZjJpL3UdIzemqAgYlGb65RDZ1xUxWSSC1%2BEYIKOykL4DSJTkJIK5CM7L2pIwQn6iwILxFhihkRHuZ1T39i6QYMTABjP1k6UrUXL%2FBmTMhaeVhCkMp%2BeKjMMvDCVgcDnOpTDa9AaqyYqSxEB%2BvsMFZ3TpuFp09e8V8tXixQlAmAzvj35FqgjD0dIgl2krmorZmtcsShkDDIC8I71Z3tJlFUiPX67BRM8HCTWr0Tpt2RLJDc7YTmPNsa3kOmErxYBUxPN5zz1dpF2eo6YFyn62avCl7PNl3%2FqtYg0l8Y7m0Q4LSZDNmX5G6lETLQIYFD%2FLri%2FjbdqwqvVvrFK%2BLFS2xhGy9zD8Dx7I%2Fy7NcZZpOJJbcD7MF4Sgq3V3lQqqdQ1Ut9gy2kWMOq%2BodYGOqUBUKZpRiHyWjroyeQQQFWcxMZIeb8oJiH3Mi4u1g0fiTZXpN%2BnP%2BBx86tk1KbTjZYcIU0b1tF9psvFWEuDAKz1ZILmgY%2FMeYVOTQPLTCa4AiSv5xQivNGvfAvNS9Salp8VxxZX6d6KgwobLogPSmny9U7zj7fnz%2FYnTwuPZ3XH2re4Xx%2F4h1QvI7APKSd18AfVAXq0UeTfWZKH4EsAQSwMAjPCGV4H&X-Amz-Signature=8260709c2b309c0470cc75692bacbd938c2357d060ef5a882d59ed1ce0561138&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
