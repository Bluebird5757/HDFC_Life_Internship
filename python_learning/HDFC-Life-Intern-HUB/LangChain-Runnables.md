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
fetched_at: '2026-09-16T02:18:57.712Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTIR7AUZ%2F20260916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260916T021854Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDGhC8ggY11V7V20uMhk1mzip5iqVcVTwECk7Oadgxz7AIgLWHdxB5hCa1zb0i3O2JNCrbY4C35flJAtjKOgcvkYnMq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDEFqYlJV8%2FdPxndonircA2AjYDAbR2dpfS5tkDtjtxndm%2Bjow4moYoNtefveubMsSg8kE3RHJz3WuhEIKskP1q5kAEWnnf5JQmb%2FnU3fYd%2BlPe5AkV88nncadb6mhYeGBTr6kKCsn6FbYc2FL5JZ%2F40zdV%2BwT8s4ZF6Xm6X1VNh9NOdTIELBYbfNCef93Ngoe6atkNIXao0ctxTWYquj9FfAz9s1sO%2BYSIG%2FyCwtihyuaVpjeuNVf%2B1er2F6Orw2QaAwvSQw%2B%2BwV5XLTCNXdNPKjAGq8fn9db%2FHj%2FO60%2BRdu5xdzZhSYEYnbQJBOfTr5dsFYke%2F1XZJmSqKDPTAzP5i8KGY%2FknC%2Bn3fRDh2Vw8PBe%2FDOCAK9%2BgXtycDUB7NLYDGefbKEU2w6LlrLzq3xAGLhj6WNqCVoE4wXM8DmVB%2Bzb4LH983s7NBzZcwVylAo2HeCGx1tzMOZtkg8tCAcOTKjOzAybRKggU%2Bq6iXNRqm5ITTwN9bx%2B3sZa%2BEblMES7oiCIZr8n%2FlB%2BcaXCO9Xj%2F4k7WNtYp8qCeoHz%2FShDalgKr8w%2FPbq6c2Ao3GtrSusGTba6oVBcPnTZznaEsyEksmevWkfikRVb%2BXk7YY3ixymVJOpx2D0VDW%2BDSU%2BaSV5MHtfLPVA7a%2BD467zMN%2Bxp9UGOqUB%2B4xhQOD5EuEOyDAXv5fGHjSVuRf2Ywb1bjl4loGreWae7hA0zaN46z1N6NpIGdGrir6HWRRCHPT4WA3Z6s92Cvr5wYDT2PdSLdIlKP2PHRPHRkrvylcrP6IMLNR6swaE0eneHYOh%2BHV1qGIeEX9ZEco6zFo5jCFUINVhoAClH3pAAYEusG8brTdCXx0mi%2FK9DjjvyoZNSep2wK%2Bm66iqVdGshddz&X-Amz-Signature=a4b0108e9bb2d0ac668a0923663e5fccd204726bf1fb29eb0cce3fd1c2f40666&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTIR7AUZ%2F20260916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260916T021854Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDGhC8ggY11V7V20uMhk1mzip5iqVcVTwECk7Oadgxz7AIgLWHdxB5hCa1zb0i3O2JNCrbY4C35flJAtjKOgcvkYnMq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDEFqYlJV8%2FdPxndonircA2AjYDAbR2dpfS5tkDtjtxndm%2Bjow4moYoNtefveubMsSg8kE3RHJz3WuhEIKskP1q5kAEWnnf5JQmb%2FnU3fYd%2BlPe5AkV88nncadb6mhYeGBTr6kKCsn6FbYc2FL5JZ%2F40zdV%2BwT8s4ZF6Xm6X1VNh9NOdTIELBYbfNCef93Ngoe6atkNIXao0ctxTWYquj9FfAz9s1sO%2BYSIG%2FyCwtihyuaVpjeuNVf%2B1er2F6Orw2QaAwvSQw%2B%2BwV5XLTCNXdNPKjAGq8fn9db%2FHj%2FO60%2BRdu5xdzZhSYEYnbQJBOfTr5dsFYke%2F1XZJmSqKDPTAzP5i8KGY%2FknC%2Bn3fRDh2Vw8PBe%2FDOCAK9%2BgXtycDUB7NLYDGefbKEU2w6LlrLzq3xAGLhj6WNqCVoE4wXM8DmVB%2Bzb4LH983s7NBzZcwVylAo2HeCGx1tzMOZtkg8tCAcOTKjOzAybRKggU%2Bq6iXNRqm5ITTwN9bx%2B3sZa%2BEblMES7oiCIZr8n%2FlB%2BcaXCO9Xj%2F4k7WNtYp8qCeoHz%2FShDalgKr8w%2FPbq6c2Ao3GtrSusGTba6oVBcPnTZznaEsyEksmevWkfikRVb%2BXk7YY3ixymVJOpx2D0VDW%2BDSU%2BaSV5MHtfLPVA7a%2BD467zMN%2Bxp9UGOqUB%2B4xhQOD5EuEOyDAXv5fGHjSVuRf2Ywb1bjl4loGreWae7hA0zaN46z1N6NpIGdGrir6HWRRCHPT4WA3Z6s92Cvr5wYDT2PdSLdIlKP2PHRPHRkrvylcrP6IMLNR6swaE0eneHYOh%2BHV1qGIeEX9ZEco6zFo5jCFUINVhoAClH3pAAYEusG8brTdCXx0mi%2FK9DjjvyoZNSep2wK%2Bm66iqVdGshddz&X-Amz-Signature=66173347c5396d3341e8060f602c948001d232f0f917f68d46c42e5653894022&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTIR7AUZ%2F20260916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260916T021854Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDGhC8ggY11V7V20uMhk1mzip5iqVcVTwECk7Oadgxz7AIgLWHdxB5hCa1zb0i3O2JNCrbY4C35flJAtjKOgcvkYnMq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDEFqYlJV8%2FdPxndonircA2AjYDAbR2dpfS5tkDtjtxndm%2Bjow4moYoNtefveubMsSg8kE3RHJz3WuhEIKskP1q5kAEWnnf5JQmb%2FnU3fYd%2BlPe5AkV88nncadb6mhYeGBTr6kKCsn6FbYc2FL5JZ%2F40zdV%2BwT8s4ZF6Xm6X1VNh9NOdTIELBYbfNCef93Ngoe6atkNIXao0ctxTWYquj9FfAz9s1sO%2BYSIG%2FyCwtihyuaVpjeuNVf%2B1er2F6Orw2QaAwvSQw%2B%2BwV5XLTCNXdNPKjAGq8fn9db%2FHj%2FO60%2BRdu5xdzZhSYEYnbQJBOfTr5dsFYke%2F1XZJmSqKDPTAzP5i8KGY%2FknC%2Bn3fRDh2Vw8PBe%2FDOCAK9%2BgXtycDUB7NLYDGefbKEU2w6LlrLzq3xAGLhj6WNqCVoE4wXM8DmVB%2Bzb4LH983s7NBzZcwVylAo2HeCGx1tzMOZtkg8tCAcOTKjOzAybRKggU%2Bq6iXNRqm5ITTwN9bx%2B3sZa%2BEblMES7oiCIZr8n%2FlB%2BcaXCO9Xj%2F4k7WNtYp8qCeoHz%2FShDalgKr8w%2FPbq6c2Ao3GtrSusGTba6oVBcPnTZznaEsyEksmevWkfikRVb%2BXk7YY3ixymVJOpx2D0VDW%2BDSU%2BaSV5MHtfLPVA7a%2BD467zMN%2Bxp9UGOqUB%2B4xhQOD5EuEOyDAXv5fGHjSVuRf2Ywb1bjl4loGreWae7hA0zaN46z1N6NpIGdGrir6HWRRCHPT4WA3Z6s92Cvr5wYDT2PdSLdIlKP2PHRPHRkrvylcrP6IMLNR6swaE0eneHYOh%2BHV1qGIeEX9ZEco6zFo5jCFUINVhoAClH3pAAYEusG8brTdCXx0mi%2FK9DjjvyoZNSep2wK%2Bm66iqVdGshddz&X-Amz-Signature=9c48f8629cb44f6dc23091868daca059587ce8c14c91aff2ebdc3110fcae9a74&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTIR7AUZ%2F20260916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260916T021854Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDGhC8ggY11V7V20uMhk1mzip5iqVcVTwECk7Oadgxz7AIgLWHdxB5hCa1zb0i3O2JNCrbY4C35flJAtjKOgcvkYnMq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDEFqYlJV8%2FdPxndonircA2AjYDAbR2dpfS5tkDtjtxndm%2Bjow4moYoNtefveubMsSg8kE3RHJz3WuhEIKskP1q5kAEWnnf5JQmb%2FnU3fYd%2BlPe5AkV88nncadb6mhYeGBTr6kKCsn6FbYc2FL5JZ%2F40zdV%2BwT8s4ZF6Xm6X1VNh9NOdTIELBYbfNCef93Ngoe6atkNIXao0ctxTWYquj9FfAz9s1sO%2BYSIG%2FyCwtihyuaVpjeuNVf%2B1er2F6Orw2QaAwvSQw%2B%2BwV5XLTCNXdNPKjAGq8fn9db%2FHj%2FO60%2BRdu5xdzZhSYEYnbQJBOfTr5dsFYke%2F1XZJmSqKDPTAzP5i8KGY%2FknC%2Bn3fRDh2Vw8PBe%2FDOCAK9%2BgXtycDUB7NLYDGefbKEU2w6LlrLzq3xAGLhj6WNqCVoE4wXM8DmVB%2Bzb4LH983s7NBzZcwVylAo2HeCGx1tzMOZtkg8tCAcOTKjOzAybRKggU%2Bq6iXNRqm5ITTwN9bx%2B3sZa%2BEblMES7oiCIZr8n%2FlB%2BcaXCO9Xj%2F4k7WNtYp8qCeoHz%2FShDalgKr8w%2FPbq6c2Ao3GtrSusGTba6oVBcPnTZznaEsyEksmevWkfikRVb%2BXk7YY3ixymVJOpx2D0VDW%2BDSU%2BaSV5MHtfLPVA7a%2BD467zMN%2Bxp9UGOqUB%2B4xhQOD5EuEOyDAXv5fGHjSVuRf2Ywb1bjl4loGreWae7hA0zaN46z1N6NpIGdGrir6HWRRCHPT4WA3Z6s92Cvr5wYDT2PdSLdIlKP2PHRPHRkrvylcrP6IMLNR6swaE0eneHYOh%2BHV1qGIeEX9ZEco6zFo5jCFUINVhoAClH3pAAYEusG8brTdCXx0mi%2FK9DjjvyoZNSep2wK%2Bm66iqVdGshddz&X-Amz-Signature=0401219bdf99179abe5fda435f394fc5394e43b77ef53ed92953c5572dc07058&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTIR7AUZ%2F20260916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260916T021854Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDGhC8ggY11V7V20uMhk1mzip5iqVcVTwECk7Oadgxz7AIgLWHdxB5hCa1zb0i3O2JNCrbY4C35flJAtjKOgcvkYnMq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDEFqYlJV8%2FdPxndonircA2AjYDAbR2dpfS5tkDtjtxndm%2Bjow4moYoNtefveubMsSg8kE3RHJz3WuhEIKskP1q5kAEWnnf5JQmb%2FnU3fYd%2BlPe5AkV88nncadb6mhYeGBTr6kKCsn6FbYc2FL5JZ%2F40zdV%2BwT8s4ZF6Xm6X1VNh9NOdTIELBYbfNCef93Ngoe6atkNIXao0ctxTWYquj9FfAz9s1sO%2BYSIG%2FyCwtihyuaVpjeuNVf%2B1er2F6Orw2QaAwvSQw%2B%2BwV5XLTCNXdNPKjAGq8fn9db%2FHj%2FO60%2BRdu5xdzZhSYEYnbQJBOfTr5dsFYke%2F1XZJmSqKDPTAzP5i8KGY%2FknC%2Bn3fRDh2Vw8PBe%2FDOCAK9%2BgXtycDUB7NLYDGefbKEU2w6LlrLzq3xAGLhj6WNqCVoE4wXM8DmVB%2Bzb4LH983s7NBzZcwVylAo2HeCGx1tzMOZtkg8tCAcOTKjOzAybRKggU%2Bq6iXNRqm5ITTwN9bx%2B3sZa%2BEblMES7oiCIZr8n%2FlB%2BcaXCO9Xj%2F4k7WNtYp8qCeoHz%2FShDalgKr8w%2FPbq6c2Ao3GtrSusGTba6oVBcPnTZznaEsyEksmevWkfikRVb%2BXk7YY3ixymVJOpx2D0VDW%2BDSU%2BaSV5MHtfLPVA7a%2BD467zMN%2Bxp9UGOqUB%2B4xhQOD5EuEOyDAXv5fGHjSVuRf2Ywb1bjl4loGreWae7hA0zaN46z1N6NpIGdGrir6HWRRCHPT4WA3Z6s92Cvr5wYDT2PdSLdIlKP2PHRPHRkrvylcrP6IMLNR6swaE0eneHYOh%2BHV1qGIeEX9ZEco6zFo5jCFUINVhoAClH3pAAYEusG8brTdCXx0mi%2FK9DjjvyoZNSep2wK%2Bm66iqVdGshddz&X-Amz-Signature=0414f4268af2206cb518b0de369b6aa3792a22eb26a671a7fd89c35a7d2edbeb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTIR7AUZ%2F20260916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260916T021854Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDGhC8ggY11V7V20uMhk1mzip5iqVcVTwECk7Oadgxz7AIgLWHdxB5hCa1zb0i3O2JNCrbY4C35flJAtjKOgcvkYnMq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDEFqYlJV8%2FdPxndonircA2AjYDAbR2dpfS5tkDtjtxndm%2Bjow4moYoNtefveubMsSg8kE3RHJz3WuhEIKskP1q5kAEWnnf5JQmb%2FnU3fYd%2BlPe5AkV88nncadb6mhYeGBTr6kKCsn6FbYc2FL5JZ%2F40zdV%2BwT8s4ZF6Xm6X1VNh9NOdTIELBYbfNCef93Ngoe6atkNIXao0ctxTWYquj9FfAz9s1sO%2BYSIG%2FyCwtihyuaVpjeuNVf%2B1er2F6Orw2QaAwvSQw%2B%2BwV5XLTCNXdNPKjAGq8fn9db%2FHj%2FO60%2BRdu5xdzZhSYEYnbQJBOfTr5dsFYke%2F1XZJmSqKDPTAzP5i8KGY%2FknC%2Bn3fRDh2Vw8PBe%2FDOCAK9%2BgXtycDUB7NLYDGefbKEU2w6LlrLzq3xAGLhj6WNqCVoE4wXM8DmVB%2Bzb4LH983s7NBzZcwVylAo2HeCGx1tzMOZtkg8tCAcOTKjOzAybRKggU%2Bq6iXNRqm5ITTwN9bx%2B3sZa%2BEblMES7oiCIZr8n%2FlB%2BcaXCO9Xj%2F4k7WNtYp8qCeoHz%2FShDalgKr8w%2FPbq6c2Ao3GtrSusGTba6oVBcPnTZznaEsyEksmevWkfikRVb%2BXk7YY3ixymVJOpx2D0VDW%2BDSU%2BaSV5MHtfLPVA7a%2BD467zMN%2Bxp9UGOqUB%2B4xhQOD5EuEOyDAXv5fGHjSVuRf2Ywb1bjl4loGreWae7hA0zaN46z1N6NpIGdGrir6HWRRCHPT4WA3Z6s92Cvr5wYDT2PdSLdIlKP2PHRPHRkrvylcrP6IMLNR6swaE0eneHYOh%2BHV1qGIeEX9ZEco6zFo5jCFUINVhoAClH3pAAYEusG8brTdCXx0mi%2FK9DjjvyoZNSep2wK%2Bm66iqVdGshddz&X-Amz-Signature=f6dfac3f1a217ebd12125a6901e7bc74ad2b78777c6299f55d349d65c3736456&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTIR7AUZ%2F20260916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260916T021854Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDGhC8ggY11V7V20uMhk1mzip5iqVcVTwECk7Oadgxz7AIgLWHdxB5hCa1zb0i3O2JNCrbY4C35flJAtjKOgcvkYnMq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDEFqYlJV8%2FdPxndonircA2AjYDAbR2dpfS5tkDtjtxndm%2Bjow4moYoNtefveubMsSg8kE3RHJz3WuhEIKskP1q5kAEWnnf5JQmb%2FnU3fYd%2BlPe5AkV88nncadb6mhYeGBTr6kKCsn6FbYc2FL5JZ%2F40zdV%2BwT8s4ZF6Xm6X1VNh9NOdTIELBYbfNCef93Ngoe6atkNIXao0ctxTWYquj9FfAz9s1sO%2BYSIG%2FyCwtihyuaVpjeuNVf%2B1er2F6Orw2QaAwvSQw%2B%2BwV5XLTCNXdNPKjAGq8fn9db%2FHj%2FO60%2BRdu5xdzZhSYEYnbQJBOfTr5dsFYke%2F1XZJmSqKDPTAzP5i8KGY%2FknC%2Bn3fRDh2Vw8PBe%2FDOCAK9%2BgXtycDUB7NLYDGefbKEU2w6LlrLzq3xAGLhj6WNqCVoE4wXM8DmVB%2Bzb4LH983s7NBzZcwVylAo2HeCGx1tzMOZtkg8tCAcOTKjOzAybRKggU%2Bq6iXNRqm5ITTwN9bx%2B3sZa%2BEblMES7oiCIZr8n%2FlB%2BcaXCO9Xj%2F4k7WNtYp8qCeoHz%2FShDalgKr8w%2FPbq6c2Ao3GtrSusGTba6oVBcPnTZznaEsyEksmevWkfikRVb%2BXk7YY3ixymVJOpx2D0VDW%2BDSU%2BaSV5MHtfLPVA7a%2BD467zMN%2Bxp9UGOqUB%2B4xhQOD5EuEOyDAXv5fGHjSVuRf2Ywb1bjl4loGreWae7hA0zaN46z1N6NpIGdGrir6HWRRCHPT4WA3Z6s92Cvr5wYDT2PdSLdIlKP2PHRPHRkrvylcrP6IMLNR6swaE0eneHYOh%2BHV1qGIeEX9ZEco6zFo5jCFUINVhoAClH3pAAYEusG8brTdCXx0mi%2FK9DjjvyoZNSep2wK%2Bm66iqVdGshddz&X-Amz-Signature=9bde663e540c6c966b8951a7abf12d1a9244ee11b5782161c4f7bed51b3aff91&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTIR7AUZ%2F20260916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260916T021854Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDGhC8ggY11V7V20uMhk1mzip5iqVcVTwECk7Oadgxz7AIgLWHdxB5hCa1zb0i3O2JNCrbY4C35flJAtjKOgcvkYnMq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDEFqYlJV8%2FdPxndonircA2AjYDAbR2dpfS5tkDtjtxndm%2Bjow4moYoNtefveubMsSg8kE3RHJz3WuhEIKskP1q5kAEWnnf5JQmb%2FnU3fYd%2BlPe5AkV88nncadb6mhYeGBTr6kKCsn6FbYc2FL5JZ%2F40zdV%2BwT8s4ZF6Xm6X1VNh9NOdTIELBYbfNCef93Ngoe6atkNIXao0ctxTWYquj9FfAz9s1sO%2BYSIG%2FyCwtihyuaVpjeuNVf%2B1er2F6Orw2QaAwvSQw%2B%2BwV5XLTCNXdNPKjAGq8fn9db%2FHj%2FO60%2BRdu5xdzZhSYEYnbQJBOfTr5dsFYke%2F1XZJmSqKDPTAzP5i8KGY%2FknC%2Bn3fRDh2Vw8PBe%2FDOCAK9%2BgXtycDUB7NLYDGefbKEU2w6LlrLzq3xAGLhj6WNqCVoE4wXM8DmVB%2Bzb4LH983s7NBzZcwVylAo2HeCGx1tzMOZtkg8tCAcOTKjOzAybRKggU%2Bq6iXNRqm5ITTwN9bx%2B3sZa%2BEblMES7oiCIZr8n%2FlB%2BcaXCO9Xj%2F4k7WNtYp8qCeoHz%2FShDalgKr8w%2FPbq6c2Ao3GtrSusGTba6oVBcPnTZznaEsyEksmevWkfikRVb%2BXk7YY3ixymVJOpx2D0VDW%2BDSU%2BaSV5MHtfLPVA7a%2BD467zMN%2Bxp9UGOqUB%2B4xhQOD5EuEOyDAXv5fGHjSVuRf2Ywb1bjl4loGreWae7hA0zaN46z1N6NpIGdGrir6HWRRCHPT4WA3Z6s92Cvr5wYDT2PdSLdIlKP2PHRPHRkrvylcrP6IMLNR6swaE0eneHYOh%2BHV1qGIeEX9ZEco6zFo5jCFUINVhoAClH3pAAYEusG8brTdCXx0mi%2FK9DjjvyoZNSep2wK%2Bm66iqVdGshddz&X-Amz-Signature=af89ced5c4643572685c632e4159c3fe1a6c03bef1d249d329aba4463a366848&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTIR7AUZ%2F20260916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260916T021854Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDGhC8ggY11V7V20uMhk1mzip5iqVcVTwECk7Oadgxz7AIgLWHdxB5hCa1zb0i3O2JNCrbY4C35flJAtjKOgcvkYnMq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDEFqYlJV8%2FdPxndonircA2AjYDAbR2dpfS5tkDtjtxndm%2Bjow4moYoNtefveubMsSg8kE3RHJz3WuhEIKskP1q5kAEWnnf5JQmb%2FnU3fYd%2BlPe5AkV88nncadb6mhYeGBTr6kKCsn6FbYc2FL5JZ%2F40zdV%2BwT8s4ZF6Xm6X1VNh9NOdTIELBYbfNCef93Ngoe6atkNIXao0ctxTWYquj9FfAz9s1sO%2BYSIG%2FyCwtihyuaVpjeuNVf%2B1er2F6Orw2QaAwvSQw%2B%2BwV5XLTCNXdNPKjAGq8fn9db%2FHj%2FO60%2BRdu5xdzZhSYEYnbQJBOfTr5dsFYke%2F1XZJmSqKDPTAzP5i8KGY%2FknC%2Bn3fRDh2Vw8PBe%2FDOCAK9%2BgXtycDUB7NLYDGefbKEU2w6LlrLzq3xAGLhj6WNqCVoE4wXM8DmVB%2Bzb4LH983s7NBzZcwVylAo2HeCGx1tzMOZtkg8tCAcOTKjOzAybRKggU%2Bq6iXNRqm5ITTwN9bx%2B3sZa%2BEblMES7oiCIZr8n%2FlB%2BcaXCO9Xj%2F4k7WNtYp8qCeoHz%2FShDalgKr8w%2FPbq6c2Ao3GtrSusGTba6oVBcPnTZznaEsyEksmevWkfikRVb%2BXk7YY3ixymVJOpx2D0VDW%2BDSU%2BaSV5MHtfLPVA7a%2BD467zMN%2Bxp9UGOqUB%2B4xhQOD5EuEOyDAXv5fGHjSVuRf2Ywb1bjl4loGreWae7hA0zaN46z1N6NpIGdGrir6HWRRCHPT4WA3Z6s92Cvr5wYDT2PdSLdIlKP2PHRPHRkrvylcrP6IMLNR6swaE0eneHYOh%2BHV1qGIeEX9ZEco6zFo5jCFUINVhoAClH3pAAYEusG8brTdCXx0mi%2FK9DjjvyoZNSep2wK%2Bm66iqVdGshddz&X-Amz-Signature=d8ae838b2c23ca16e806b90833b0091521133c25b98fd79cc27f91b4b3ab90d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
