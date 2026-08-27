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
fetched_at: '2026-08-27T05:43:17.506Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UEVJIZ62%2F20260827%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260827T054313Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDTUlMBdZq2y2HQQaofwu61S7vL7G%2FnTNQ0A09ftcShyAIgCcn7M%2BdiOjUXniFp1xVIDPGbvlfLBEY00ttaCWxLY6Eq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDGsmz9I8eKF%2FQdF79yrcA7VlgnmzLTvxVCk7rMwPsjljzylSkRCoPOz%2BR31oc5WuMYo2VGmgZOgByxdbYSh11z7lvm71xYoKgnVw5ehFqPvcrlVRm3prHq8TLza95EFqdkg%2FRjdABisRJFDqJKrKPYxbUA5f2JYfVVSgJnNwh19P2odURmMcZbJucYj70hZ7fVHYR93mtQSxxfr7AiujeT65nrU2t1WjMoR6r1CAlEnkmbokm0s9uBLmx6vsbeiWfksuS8jdMPdbO6AnwrgB8%2F9aJvRz1vydHOPLXmo3Da9HPiPlU3BimtqtntT63L9JUewiRm1HGp6H1RL%2FvO21Pv2xlqxeG3iQJ4Pf%2BW%2FtDT%2Fqe27iT2cmWd6YaeWoUhmJUuRVqOayLjOP5dy%2F9xlksva9E%2BeoiGa5Y20H7XNg56HBxC%2BznEUqbIYf9gctzPk%2FcHYYwT3nNkmF%2FRghlpdvVUhBBcRc2KqPYiS3%2FiqT9ZYBmpsoj%2BPLHGQSsK%2BvFqJwcH%2FUDZ72SvSgJ%2FJt3v1tlFMExLeUrov6U8a%2BMpXgLQqn14agzSXFacU7gxnjQCXgxdvyEpkdLGWJtknbnl2sKTbEnFVyXsrNZWSr9u40IKubYzt83hwkvUF%2B6EdZEUg5SsFLCboFi6CLPSbUMNbcvtQGOqUBjGTqU19aQjQ3JXZr6W5DP78IzJqafbNM%2FfMQnqDo8i8HmYsvMiPvm8YVJmQMXDmjm7CackAyxR8wv27%2FConvZnWcNSunzPkasSubszriMhsXIxwfOUYRI2OMd%2B%2B0Dmf0dw6JEPwllEQisRzr7EJlwJFK%2BQoGRuLfNu4hGOUwxSTj%2BZCLpnYJUy4mHlukuvBWE93NjoinpSt7vtX6SdJb81csss1o&X-Amz-Signature=b0edb9c301e08861ca92c1cb81b7056d80c13722b9435688ed63532ac8461d46&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UEVJIZ62%2F20260827%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260827T054313Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDTUlMBdZq2y2HQQaofwu61S7vL7G%2FnTNQ0A09ftcShyAIgCcn7M%2BdiOjUXniFp1xVIDPGbvlfLBEY00ttaCWxLY6Eq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDGsmz9I8eKF%2FQdF79yrcA7VlgnmzLTvxVCk7rMwPsjljzylSkRCoPOz%2BR31oc5WuMYo2VGmgZOgByxdbYSh11z7lvm71xYoKgnVw5ehFqPvcrlVRm3prHq8TLza95EFqdkg%2FRjdABisRJFDqJKrKPYxbUA5f2JYfVVSgJnNwh19P2odURmMcZbJucYj70hZ7fVHYR93mtQSxxfr7AiujeT65nrU2t1WjMoR6r1CAlEnkmbokm0s9uBLmx6vsbeiWfksuS8jdMPdbO6AnwrgB8%2F9aJvRz1vydHOPLXmo3Da9HPiPlU3BimtqtntT63L9JUewiRm1HGp6H1RL%2FvO21Pv2xlqxeG3iQJ4Pf%2BW%2FtDT%2Fqe27iT2cmWd6YaeWoUhmJUuRVqOayLjOP5dy%2F9xlksva9E%2BeoiGa5Y20H7XNg56HBxC%2BznEUqbIYf9gctzPk%2FcHYYwT3nNkmF%2FRghlpdvVUhBBcRc2KqPYiS3%2FiqT9ZYBmpsoj%2BPLHGQSsK%2BvFqJwcH%2FUDZ72SvSgJ%2FJt3v1tlFMExLeUrov6U8a%2BMpXgLQqn14agzSXFacU7gxnjQCXgxdvyEpkdLGWJtknbnl2sKTbEnFVyXsrNZWSr9u40IKubYzt83hwkvUF%2B6EdZEUg5SsFLCboFi6CLPSbUMNbcvtQGOqUBjGTqU19aQjQ3JXZr6W5DP78IzJqafbNM%2FfMQnqDo8i8HmYsvMiPvm8YVJmQMXDmjm7CackAyxR8wv27%2FConvZnWcNSunzPkasSubszriMhsXIxwfOUYRI2OMd%2B%2B0Dmf0dw6JEPwllEQisRzr7EJlwJFK%2BQoGRuLfNu4hGOUwxSTj%2BZCLpnYJUy4mHlukuvBWE93NjoinpSt7vtX6SdJb81csss1o&X-Amz-Signature=c186cc6ac3ffcfa940906f5c91bce7e940395e42b71278a7285ddbcc7eb0cf62&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UEVJIZ62%2F20260827%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260827T054313Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDTUlMBdZq2y2HQQaofwu61S7vL7G%2FnTNQ0A09ftcShyAIgCcn7M%2BdiOjUXniFp1xVIDPGbvlfLBEY00ttaCWxLY6Eq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDGsmz9I8eKF%2FQdF79yrcA7VlgnmzLTvxVCk7rMwPsjljzylSkRCoPOz%2BR31oc5WuMYo2VGmgZOgByxdbYSh11z7lvm71xYoKgnVw5ehFqPvcrlVRm3prHq8TLza95EFqdkg%2FRjdABisRJFDqJKrKPYxbUA5f2JYfVVSgJnNwh19P2odURmMcZbJucYj70hZ7fVHYR93mtQSxxfr7AiujeT65nrU2t1WjMoR6r1CAlEnkmbokm0s9uBLmx6vsbeiWfksuS8jdMPdbO6AnwrgB8%2F9aJvRz1vydHOPLXmo3Da9HPiPlU3BimtqtntT63L9JUewiRm1HGp6H1RL%2FvO21Pv2xlqxeG3iQJ4Pf%2BW%2FtDT%2Fqe27iT2cmWd6YaeWoUhmJUuRVqOayLjOP5dy%2F9xlksva9E%2BeoiGa5Y20H7XNg56HBxC%2BznEUqbIYf9gctzPk%2FcHYYwT3nNkmF%2FRghlpdvVUhBBcRc2KqPYiS3%2FiqT9ZYBmpsoj%2BPLHGQSsK%2BvFqJwcH%2FUDZ72SvSgJ%2FJt3v1tlFMExLeUrov6U8a%2BMpXgLQqn14agzSXFacU7gxnjQCXgxdvyEpkdLGWJtknbnl2sKTbEnFVyXsrNZWSr9u40IKubYzt83hwkvUF%2B6EdZEUg5SsFLCboFi6CLPSbUMNbcvtQGOqUBjGTqU19aQjQ3JXZr6W5DP78IzJqafbNM%2FfMQnqDo8i8HmYsvMiPvm8YVJmQMXDmjm7CackAyxR8wv27%2FConvZnWcNSunzPkasSubszriMhsXIxwfOUYRI2OMd%2B%2B0Dmf0dw6JEPwllEQisRzr7EJlwJFK%2BQoGRuLfNu4hGOUwxSTj%2BZCLpnYJUy4mHlukuvBWE93NjoinpSt7vtX6SdJb81csss1o&X-Amz-Signature=9e8541eacfdf69d776f4fdc01a22b2c4874ec9865a219adf6cfe84339fbca1dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UEVJIZ62%2F20260827%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260827T054313Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDTUlMBdZq2y2HQQaofwu61S7vL7G%2FnTNQ0A09ftcShyAIgCcn7M%2BdiOjUXniFp1xVIDPGbvlfLBEY00ttaCWxLY6Eq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDGsmz9I8eKF%2FQdF79yrcA7VlgnmzLTvxVCk7rMwPsjljzylSkRCoPOz%2BR31oc5WuMYo2VGmgZOgByxdbYSh11z7lvm71xYoKgnVw5ehFqPvcrlVRm3prHq8TLza95EFqdkg%2FRjdABisRJFDqJKrKPYxbUA5f2JYfVVSgJnNwh19P2odURmMcZbJucYj70hZ7fVHYR93mtQSxxfr7AiujeT65nrU2t1WjMoR6r1CAlEnkmbokm0s9uBLmx6vsbeiWfksuS8jdMPdbO6AnwrgB8%2F9aJvRz1vydHOPLXmo3Da9HPiPlU3BimtqtntT63L9JUewiRm1HGp6H1RL%2FvO21Pv2xlqxeG3iQJ4Pf%2BW%2FtDT%2Fqe27iT2cmWd6YaeWoUhmJUuRVqOayLjOP5dy%2F9xlksva9E%2BeoiGa5Y20H7XNg56HBxC%2BznEUqbIYf9gctzPk%2FcHYYwT3nNkmF%2FRghlpdvVUhBBcRc2KqPYiS3%2FiqT9ZYBmpsoj%2BPLHGQSsK%2BvFqJwcH%2FUDZ72SvSgJ%2FJt3v1tlFMExLeUrov6U8a%2BMpXgLQqn14agzSXFacU7gxnjQCXgxdvyEpkdLGWJtknbnl2sKTbEnFVyXsrNZWSr9u40IKubYzt83hwkvUF%2B6EdZEUg5SsFLCboFi6CLPSbUMNbcvtQGOqUBjGTqU19aQjQ3JXZr6W5DP78IzJqafbNM%2FfMQnqDo8i8HmYsvMiPvm8YVJmQMXDmjm7CackAyxR8wv27%2FConvZnWcNSunzPkasSubszriMhsXIxwfOUYRI2OMd%2B%2B0Dmf0dw6JEPwllEQisRzr7EJlwJFK%2BQoGRuLfNu4hGOUwxSTj%2BZCLpnYJUy4mHlukuvBWE93NjoinpSt7vtX6SdJb81csss1o&X-Amz-Signature=f7193c68e72b9fd7c95768019e30d53c1b00e88b0e2fcbf84974fb669ba1b854&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UEVJIZ62%2F20260827%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260827T054313Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDTUlMBdZq2y2HQQaofwu61S7vL7G%2FnTNQ0A09ftcShyAIgCcn7M%2BdiOjUXniFp1xVIDPGbvlfLBEY00ttaCWxLY6Eq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDGsmz9I8eKF%2FQdF79yrcA7VlgnmzLTvxVCk7rMwPsjljzylSkRCoPOz%2BR31oc5WuMYo2VGmgZOgByxdbYSh11z7lvm71xYoKgnVw5ehFqPvcrlVRm3prHq8TLza95EFqdkg%2FRjdABisRJFDqJKrKPYxbUA5f2JYfVVSgJnNwh19P2odURmMcZbJucYj70hZ7fVHYR93mtQSxxfr7AiujeT65nrU2t1WjMoR6r1CAlEnkmbokm0s9uBLmx6vsbeiWfksuS8jdMPdbO6AnwrgB8%2F9aJvRz1vydHOPLXmo3Da9HPiPlU3BimtqtntT63L9JUewiRm1HGp6H1RL%2FvO21Pv2xlqxeG3iQJ4Pf%2BW%2FtDT%2Fqe27iT2cmWd6YaeWoUhmJUuRVqOayLjOP5dy%2F9xlksva9E%2BeoiGa5Y20H7XNg56HBxC%2BznEUqbIYf9gctzPk%2FcHYYwT3nNkmF%2FRghlpdvVUhBBcRc2KqPYiS3%2FiqT9ZYBmpsoj%2BPLHGQSsK%2BvFqJwcH%2FUDZ72SvSgJ%2FJt3v1tlFMExLeUrov6U8a%2BMpXgLQqn14agzSXFacU7gxnjQCXgxdvyEpkdLGWJtknbnl2sKTbEnFVyXsrNZWSr9u40IKubYzt83hwkvUF%2B6EdZEUg5SsFLCboFi6CLPSbUMNbcvtQGOqUBjGTqU19aQjQ3JXZr6W5DP78IzJqafbNM%2FfMQnqDo8i8HmYsvMiPvm8YVJmQMXDmjm7CackAyxR8wv27%2FConvZnWcNSunzPkasSubszriMhsXIxwfOUYRI2OMd%2B%2B0Dmf0dw6JEPwllEQisRzr7EJlwJFK%2BQoGRuLfNu4hGOUwxSTj%2BZCLpnYJUy4mHlukuvBWE93NjoinpSt7vtX6SdJb81csss1o&X-Amz-Signature=bbf164c6cbf4c1c749930ccdd98c56bbff0a72a63a77e73bc1a00e298eff178d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UEVJIZ62%2F20260827%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260827T054313Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDTUlMBdZq2y2HQQaofwu61S7vL7G%2FnTNQ0A09ftcShyAIgCcn7M%2BdiOjUXniFp1xVIDPGbvlfLBEY00ttaCWxLY6Eq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDGsmz9I8eKF%2FQdF79yrcA7VlgnmzLTvxVCk7rMwPsjljzylSkRCoPOz%2BR31oc5WuMYo2VGmgZOgByxdbYSh11z7lvm71xYoKgnVw5ehFqPvcrlVRm3prHq8TLza95EFqdkg%2FRjdABisRJFDqJKrKPYxbUA5f2JYfVVSgJnNwh19P2odURmMcZbJucYj70hZ7fVHYR93mtQSxxfr7AiujeT65nrU2t1WjMoR6r1CAlEnkmbokm0s9uBLmx6vsbeiWfksuS8jdMPdbO6AnwrgB8%2F9aJvRz1vydHOPLXmo3Da9HPiPlU3BimtqtntT63L9JUewiRm1HGp6H1RL%2FvO21Pv2xlqxeG3iQJ4Pf%2BW%2FtDT%2Fqe27iT2cmWd6YaeWoUhmJUuRVqOayLjOP5dy%2F9xlksva9E%2BeoiGa5Y20H7XNg56HBxC%2BznEUqbIYf9gctzPk%2FcHYYwT3nNkmF%2FRghlpdvVUhBBcRc2KqPYiS3%2FiqT9ZYBmpsoj%2BPLHGQSsK%2BvFqJwcH%2FUDZ72SvSgJ%2FJt3v1tlFMExLeUrov6U8a%2BMpXgLQqn14agzSXFacU7gxnjQCXgxdvyEpkdLGWJtknbnl2sKTbEnFVyXsrNZWSr9u40IKubYzt83hwkvUF%2B6EdZEUg5SsFLCboFi6CLPSbUMNbcvtQGOqUBjGTqU19aQjQ3JXZr6W5DP78IzJqafbNM%2FfMQnqDo8i8HmYsvMiPvm8YVJmQMXDmjm7CackAyxR8wv27%2FConvZnWcNSunzPkasSubszriMhsXIxwfOUYRI2OMd%2B%2B0Dmf0dw6JEPwllEQisRzr7EJlwJFK%2BQoGRuLfNu4hGOUwxSTj%2BZCLpnYJUy4mHlukuvBWE93NjoinpSt7vtX6SdJb81csss1o&X-Amz-Signature=bbc8816d151180cd5756d096c2601dca5651da15aeff1b032a2b0e5ef93d3953&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UEVJIZ62%2F20260827%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260827T054313Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDTUlMBdZq2y2HQQaofwu61S7vL7G%2FnTNQ0A09ftcShyAIgCcn7M%2BdiOjUXniFp1xVIDPGbvlfLBEY00ttaCWxLY6Eq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDGsmz9I8eKF%2FQdF79yrcA7VlgnmzLTvxVCk7rMwPsjljzylSkRCoPOz%2BR31oc5WuMYo2VGmgZOgByxdbYSh11z7lvm71xYoKgnVw5ehFqPvcrlVRm3prHq8TLza95EFqdkg%2FRjdABisRJFDqJKrKPYxbUA5f2JYfVVSgJnNwh19P2odURmMcZbJucYj70hZ7fVHYR93mtQSxxfr7AiujeT65nrU2t1WjMoR6r1CAlEnkmbokm0s9uBLmx6vsbeiWfksuS8jdMPdbO6AnwrgB8%2F9aJvRz1vydHOPLXmo3Da9HPiPlU3BimtqtntT63L9JUewiRm1HGp6H1RL%2FvO21Pv2xlqxeG3iQJ4Pf%2BW%2FtDT%2Fqe27iT2cmWd6YaeWoUhmJUuRVqOayLjOP5dy%2F9xlksva9E%2BeoiGa5Y20H7XNg56HBxC%2BznEUqbIYf9gctzPk%2FcHYYwT3nNkmF%2FRghlpdvVUhBBcRc2KqPYiS3%2FiqT9ZYBmpsoj%2BPLHGQSsK%2BvFqJwcH%2FUDZ72SvSgJ%2FJt3v1tlFMExLeUrov6U8a%2BMpXgLQqn14agzSXFacU7gxnjQCXgxdvyEpkdLGWJtknbnl2sKTbEnFVyXsrNZWSr9u40IKubYzt83hwkvUF%2B6EdZEUg5SsFLCboFi6CLPSbUMNbcvtQGOqUBjGTqU19aQjQ3JXZr6W5DP78IzJqafbNM%2FfMQnqDo8i8HmYsvMiPvm8YVJmQMXDmjm7CackAyxR8wv27%2FConvZnWcNSunzPkasSubszriMhsXIxwfOUYRI2OMd%2B%2B0Dmf0dw6JEPwllEQisRzr7EJlwJFK%2BQoGRuLfNu4hGOUwxSTj%2BZCLpnYJUy4mHlukuvBWE93NjoinpSt7vtX6SdJb81csss1o&X-Amz-Signature=5226ba1f86bb058a0bb3012ccbdd073fee81336a8d23606c157124bf6906b718&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UEVJIZ62%2F20260827%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260827T054313Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDTUlMBdZq2y2HQQaofwu61S7vL7G%2FnTNQ0A09ftcShyAIgCcn7M%2BdiOjUXniFp1xVIDPGbvlfLBEY00ttaCWxLY6Eq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDGsmz9I8eKF%2FQdF79yrcA7VlgnmzLTvxVCk7rMwPsjljzylSkRCoPOz%2BR31oc5WuMYo2VGmgZOgByxdbYSh11z7lvm71xYoKgnVw5ehFqPvcrlVRm3prHq8TLza95EFqdkg%2FRjdABisRJFDqJKrKPYxbUA5f2JYfVVSgJnNwh19P2odURmMcZbJucYj70hZ7fVHYR93mtQSxxfr7AiujeT65nrU2t1WjMoR6r1CAlEnkmbokm0s9uBLmx6vsbeiWfksuS8jdMPdbO6AnwrgB8%2F9aJvRz1vydHOPLXmo3Da9HPiPlU3BimtqtntT63L9JUewiRm1HGp6H1RL%2FvO21Pv2xlqxeG3iQJ4Pf%2BW%2FtDT%2Fqe27iT2cmWd6YaeWoUhmJUuRVqOayLjOP5dy%2F9xlksva9E%2BeoiGa5Y20H7XNg56HBxC%2BznEUqbIYf9gctzPk%2FcHYYwT3nNkmF%2FRghlpdvVUhBBcRc2KqPYiS3%2FiqT9ZYBmpsoj%2BPLHGQSsK%2BvFqJwcH%2FUDZ72SvSgJ%2FJt3v1tlFMExLeUrov6U8a%2BMpXgLQqn14agzSXFacU7gxnjQCXgxdvyEpkdLGWJtknbnl2sKTbEnFVyXsrNZWSr9u40IKubYzt83hwkvUF%2B6EdZEUg5SsFLCboFi6CLPSbUMNbcvtQGOqUBjGTqU19aQjQ3JXZr6W5DP78IzJqafbNM%2FfMQnqDo8i8HmYsvMiPvm8YVJmQMXDmjm7CackAyxR8wv27%2FConvZnWcNSunzPkasSubszriMhsXIxwfOUYRI2OMd%2B%2B0Dmf0dw6JEPwllEQisRzr7EJlwJFK%2BQoGRuLfNu4hGOUwxSTj%2BZCLpnYJUy4mHlukuvBWE93NjoinpSt7vtX6SdJb81csss1o&X-Amz-Signature=f4384f36b0d9d14d1f03d2937f450ed6de042e414b5c89837233971d40d5c13b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UEVJIZ62%2F20260827%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260827T054313Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDTUlMBdZq2y2HQQaofwu61S7vL7G%2FnTNQ0A09ftcShyAIgCcn7M%2BdiOjUXniFp1xVIDPGbvlfLBEY00ttaCWxLY6Eq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDGsmz9I8eKF%2FQdF79yrcA7VlgnmzLTvxVCk7rMwPsjljzylSkRCoPOz%2BR31oc5WuMYo2VGmgZOgByxdbYSh11z7lvm71xYoKgnVw5ehFqPvcrlVRm3prHq8TLza95EFqdkg%2FRjdABisRJFDqJKrKPYxbUA5f2JYfVVSgJnNwh19P2odURmMcZbJucYj70hZ7fVHYR93mtQSxxfr7AiujeT65nrU2t1WjMoR6r1CAlEnkmbokm0s9uBLmx6vsbeiWfksuS8jdMPdbO6AnwrgB8%2F9aJvRz1vydHOPLXmo3Da9HPiPlU3BimtqtntT63L9JUewiRm1HGp6H1RL%2FvO21Pv2xlqxeG3iQJ4Pf%2BW%2FtDT%2Fqe27iT2cmWd6YaeWoUhmJUuRVqOayLjOP5dy%2F9xlksva9E%2BeoiGa5Y20H7XNg56HBxC%2BznEUqbIYf9gctzPk%2FcHYYwT3nNkmF%2FRghlpdvVUhBBcRc2KqPYiS3%2FiqT9ZYBmpsoj%2BPLHGQSsK%2BvFqJwcH%2FUDZ72SvSgJ%2FJt3v1tlFMExLeUrov6U8a%2BMpXgLQqn14agzSXFacU7gxnjQCXgxdvyEpkdLGWJtknbnl2sKTbEnFVyXsrNZWSr9u40IKubYzt83hwkvUF%2B6EdZEUg5SsFLCboFi6CLPSbUMNbcvtQGOqUBjGTqU19aQjQ3JXZr6W5DP78IzJqafbNM%2FfMQnqDo8i8HmYsvMiPvm8YVJmQMXDmjm7CackAyxR8wv27%2FConvZnWcNSunzPkasSubszriMhsXIxwfOUYRI2OMd%2B%2B0Dmf0dw6JEPwllEQisRzr7EJlwJFK%2BQoGRuLfNu4hGOUwxSTj%2BZCLpnYJUy4mHlukuvBWE93NjoinpSt7vtX6SdJb81csss1o&X-Amz-Signature=ab9104364b4618c15a30821754b58205f4d3e36eae5061c181535d82700d59e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
