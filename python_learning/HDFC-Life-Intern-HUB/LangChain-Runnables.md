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
fetched_at: '2026-11-21T04:45:31.653Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HZF3JYS%2F20261121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261121T044526Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCICm8CSUiWTIg%2Bt9LY4kbpqvS%2Fp3EVrnnDjDtM4PLW6e1AiEApP3GJJbPDthB1a5KWoFkooAsu%2FooA5jkaiFzuxBZa60q%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDOYUhoi0fOgdm5wh9ircAweq8SzRMoi0IfjOGbNCi1FNVQElTRKpIYOifq8BJF0DsLGpt1J0t9TjNvr%2FdR449Ek6wu4kTT1R3f4RtHMQkiIiS%2FYhPcJhDh3VDhiFniCc%2B%2FuqBOMdjJG0Vw6QC6rRCx5fHYlcYeLP5OFMnWlTAFF0%2FCPTQfql3bnR5zJQ3psH%2BuwUTIG9F56kRP7CMskOqjSr4SPmHSAYetQN8%2Fr55d2rcteUYXFuPSpw8Qa5n%2BOMJ2zPmydMVDtOf7cEmWzCI62b%2BKdmM%2BvcOvg1BUNvefDeGUe%2Bm5MMPjQYQIJgRmPev%2B59avu0gE7liBw%2B99SpiAg%2B5Ol%2BXobSXXwIwanJ4woVpSjNRLh8BMld9Aq3TXnRfMkzWXTYeY7muysR49IcA%2BGQj587%2FVTQrpHfVq%2Fqz0o2FTaDLpDCMq1%2FBQVnebVnrS28hWvRO%2B0Q4ZcloAmzPxq1YHbS7ob3NKbO9nkP1yukcLNEUBJACFGiIZJBXrzkwqwkfPH6tRxwEH3LkXjNfAGDqWR%2Fiz1fctdL05JMiwsDuLluDNB72GAonuJW6QOvxuG8G5B2LxnQX%2FoCvW2ufzi4PAad2UWzfvtyJQWP9MtLc38rlJ2BIVj3tulwj7oPt3vu4ceMdzMsB0tAMJeOhNgGOqUBhuYLdi7BTayZ9zxp4LgMdfmHw6yItrtMyWuGtYMlWIp9FqRnrHw9UGzWVlks9dVgnJMAiEaQZ1W%2FD%2FoCLvHg3oYiADxE4B%2BqnRx5%2BHLrYELNOCI9gNcw73Jh447mc%2FQsLvjjdsjGiqsrMQx534anioFTn%2FKvPEhxc26v29NYUdbZnFb2lxAA9keH3UMSYPiFr398I8itC4X69xltsr%2B31sdckX9F&X-Amz-Signature=dc85125b0fcdb04ac34ba1acc208df65335e988bce3eddde651ce2cad97066ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HZF3JYS%2F20261121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261121T044526Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCICm8CSUiWTIg%2Bt9LY4kbpqvS%2Fp3EVrnnDjDtM4PLW6e1AiEApP3GJJbPDthB1a5KWoFkooAsu%2FooA5jkaiFzuxBZa60q%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDOYUhoi0fOgdm5wh9ircAweq8SzRMoi0IfjOGbNCi1FNVQElTRKpIYOifq8BJF0DsLGpt1J0t9TjNvr%2FdR449Ek6wu4kTT1R3f4RtHMQkiIiS%2FYhPcJhDh3VDhiFniCc%2B%2FuqBOMdjJG0Vw6QC6rRCx5fHYlcYeLP5OFMnWlTAFF0%2FCPTQfql3bnR5zJQ3psH%2BuwUTIG9F56kRP7CMskOqjSr4SPmHSAYetQN8%2Fr55d2rcteUYXFuPSpw8Qa5n%2BOMJ2zPmydMVDtOf7cEmWzCI62b%2BKdmM%2BvcOvg1BUNvefDeGUe%2Bm5MMPjQYQIJgRmPev%2B59avu0gE7liBw%2B99SpiAg%2B5Ol%2BXobSXXwIwanJ4woVpSjNRLh8BMld9Aq3TXnRfMkzWXTYeY7muysR49IcA%2BGQj587%2FVTQrpHfVq%2Fqz0o2FTaDLpDCMq1%2FBQVnebVnrS28hWvRO%2B0Q4ZcloAmzPxq1YHbS7ob3NKbO9nkP1yukcLNEUBJACFGiIZJBXrzkwqwkfPH6tRxwEH3LkXjNfAGDqWR%2Fiz1fctdL05JMiwsDuLluDNB72GAonuJW6QOvxuG8G5B2LxnQX%2FoCvW2ufzi4PAad2UWzfvtyJQWP9MtLc38rlJ2BIVj3tulwj7oPt3vu4ceMdzMsB0tAMJeOhNgGOqUBhuYLdi7BTayZ9zxp4LgMdfmHw6yItrtMyWuGtYMlWIp9FqRnrHw9UGzWVlks9dVgnJMAiEaQZ1W%2FD%2FoCLvHg3oYiADxE4B%2BqnRx5%2BHLrYELNOCI9gNcw73Jh447mc%2FQsLvjjdsjGiqsrMQx534anioFTn%2FKvPEhxc26v29NYUdbZnFb2lxAA9keH3UMSYPiFr398I8itC4X69xltsr%2B31sdckX9F&X-Amz-Signature=c5d938b9ea27bab56952a9b0b7b2f73a3d1be12de2e336a2d37850701ab6e7f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HZF3JYS%2F20261121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261121T044526Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCICm8CSUiWTIg%2Bt9LY4kbpqvS%2Fp3EVrnnDjDtM4PLW6e1AiEApP3GJJbPDthB1a5KWoFkooAsu%2FooA5jkaiFzuxBZa60q%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDOYUhoi0fOgdm5wh9ircAweq8SzRMoi0IfjOGbNCi1FNVQElTRKpIYOifq8BJF0DsLGpt1J0t9TjNvr%2FdR449Ek6wu4kTT1R3f4RtHMQkiIiS%2FYhPcJhDh3VDhiFniCc%2B%2FuqBOMdjJG0Vw6QC6rRCx5fHYlcYeLP5OFMnWlTAFF0%2FCPTQfql3bnR5zJQ3psH%2BuwUTIG9F56kRP7CMskOqjSr4SPmHSAYetQN8%2Fr55d2rcteUYXFuPSpw8Qa5n%2BOMJ2zPmydMVDtOf7cEmWzCI62b%2BKdmM%2BvcOvg1BUNvefDeGUe%2Bm5MMPjQYQIJgRmPev%2B59avu0gE7liBw%2B99SpiAg%2B5Ol%2BXobSXXwIwanJ4woVpSjNRLh8BMld9Aq3TXnRfMkzWXTYeY7muysR49IcA%2BGQj587%2FVTQrpHfVq%2Fqz0o2FTaDLpDCMq1%2FBQVnebVnrS28hWvRO%2B0Q4ZcloAmzPxq1YHbS7ob3NKbO9nkP1yukcLNEUBJACFGiIZJBXrzkwqwkfPH6tRxwEH3LkXjNfAGDqWR%2Fiz1fctdL05JMiwsDuLluDNB72GAonuJW6QOvxuG8G5B2LxnQX%2FoCvW2ufzi4PAad2UWzfvtyJQWP9MtLc38rlJ2BIVj3tulwj7oPt3vu4ceMdzMsB0tAMJeOhNgGOqUBhuYLdi7BTayZ9zxp4LgMdfmHw6yItrtMyWuGtYMlWIp9FqRnrHw9UGzWVlks9dVgnJMAiEaQZ1W%2FD%2FoCLvHg3oYiADxE4B%2BqnRx5%2BHLrYELNOCI9gNcw73Jh447mc%2FQsLvjjdsjGiqsrMQx534anioFTn%2FKvPEhxc26v29NYUdbZnFb2lxAA9keH3UMSYPiFr398I8itC4X69xltsr%2B31sdckX9F&X-Amz-Signature=2a3330d91a48e1cbb26c8c5e69bb776ba102dc80ece5e42c514ede059b9ef448&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HZF3JYS%2F20261121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261121T044526Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCICm8CSUiWTIg%2Bt9LY4kbpqvS%2Fp3EVrnnDjDtM4PLW6e1AiEApP3GJJbPDthB1a5KWoFkooAsu%2FooA5jkaiFzuxBZa60q%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDOYUhoi0fOgdm5wh9ircAweq8SzRMoi0IfjOGbNCi1FNVQElTRKpIYOifq8BJF0DsLGpt1J0t9TjNvr%2FdR449Ek6wu4kTT1R3f4RtHMQkiIiS%2FYhPcJhDh3VDhiFniCc%2B%2FuqBOMdjJG0Vw6QC6rRCx5fHYlcYeLP5OFMnWlTAFF0%2FCPTQfql3bnR5zJQ3psH%2BuwUTIG9F56kRP7CMskOqjSr4SPmHSAYetQN8%2Fr55d2rcteUYXFuPSpw8Qa5n%2BOMJ2zPmydMVDtOf7cEmWzCI62b%2BKdmM%2BvcOvg1BUNvefDeGUe%2Bm5MMPjQYQIJgRmPev%2B59avu0gE7liBw%2B99SpiAg%2B5Ol%2BXobSXXwIwanJ4woVpSjNRLh8BMld9Aq3TXnRfMkzWXTYeY7muysR49IcA%2BGQj587%2FVTQrpHfVq%2Fqz0o2FTaDLpDCMq1%2FBQVnebVnrS28hWvRO%2B0Q4ZcloAmzPxq1YHbS7ob3NKbO9nkP1yukcLNEUBJACFGiIZJBXrzkwqwkfPH6tRxwEH3LkXjNfAGDqWR%2Fiz1fctdL05JMiwsDuLluDNB72GAonuJW6QOvxuG8G5B2LxnQX%2FoCvW2ufzi4PAad2UWzfvtyJQWP9MtLc38rlJ2BIVj3tulwj7oPt3vu4ceMdzMsB0tAMJeOhNgGOqUBhuYLdi7BTayZ9zxp4LgMdfmHw6yItrtMyWuGtYMlWIp9FqRnrHw9UGzWVlks9dVgnJMAiEaQZ1W%2FD%2FoCLvHg3oYiADxE4B%2BqnRx5%2BHLrYELNOCI9gNcw73Jh447mc%2FQsLvjjdsjGiqsrMQx534anioFTn%2FKvPEhxc26v29NYUdbZnFb2lxAA9keH3UMSYPiFr398I8itC4X69xltsr%2B31sdckX9F&X-Amz-Signature=3c3fc1e2d8b67b2c9009f43500acda2f18a018af3b5dd84c5ec3639c5f6acb0b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HZF3JYS%2F20261121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261121T044526Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCICm8CSUiWTIg%2Bt9LY4kbpqvS%2Fp3EVrnnDjDtM4PLW6e1AiEApP3GJJbPDthB1a5KWoFkooAsu%2FooA5jkaiFzuxBZa60q%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDOYUhoi0fOgdm5wh9ircAweq8SzRMoi0IfjOGbNCi1FNVQElTRKpIYOifq8BJF0DsLGpt1J0t9TjNvr%2FdR449Ek6wu4kTT1R3f4RtHMQkiIiS%2FYhPcJhDh3VDhiFniCc%2B%2FuqBOMdjJG0Vw6QC6rRCx5fHYlcYeLP5OFMnWlTAFF0%2FCPTQfql3bnR5zJQ3psH%2BuwUTIG9F56kRP7CMskOqjSr4SPmHSAYetQN8%2Fr55d2rcteUYXFuPSpw8Qa5n%2BOMJ2zPmydMVDtOf7cEmWzCI62b%2BKdmM%2BvcOvg1BUNvefDeGUe%2Bm5MMPjQYQIJgRmPev%2B59avu0gE7liBw%2B99SpiAg%2B5Ol%2BXobSXXwIwanJ4woVpSjNRLh8BMld9Aq3TXnRfMkzWXTYeY7muysR49IcA%2BGQj587%2FVTQrpHfVq%2Fqz0o2FTaDLpDCMq1%2FBQVnebVnrS28hWvRO%2B0Q4ZcloAmzPxq1YHbS7ob3NKbO9nkP1yukcLNEUBJACFGiIZJBXrzkwqwkfPH6tRxwEH3LkXjNfAGDqWR%2Fiz1fctdL05JMiwsDuLluDNB72GAonuJW6QOvxuG8G5B2LxnQX%2FoCvW2ufzi4PAad2UWzfvtyJQWP9MtLc38rlJ2BIVj3tulwj7oPt3vu4ceMdzMsB0tAMJeOhNgGOqUBhuYLdi7BTayZ9zxp4LgMdfmHw6yItrtMyWuGtYMlWIp9FqRnrHw9UGzWVlks9dVgnJMAiEaQZ1W%2FD%2FoCLvHg3oYiADxE4B%2BqnRx5%2BHLrYELNOCI9gNcw73Jh447mc%2FQsLvjjdsjGiqsrMQx534anioFTn%2FKvPEhxc26v29NYUdbZnFb2lxAA9keH3UMSYPiFr398I8itC4X69xltsr%2B31sdckX9F&X-Amz-Signature=797bc69d1eba340d7a3c801661f2eff3f2f58231f5055c6e79891d78d8ec16fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HZF3JYS%2F20261121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261121T044526Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCICm8CSUiWTIg%2Bt9LY4kbpqvS%2Fp3EVrnnDjDtM4PLW6e1AiEApP3GJJbPDthB1a5KWoFkooAsu%2FooA5jkaiFzuxBZa60q%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDOYUhoi0fOgdm5wh9ircAweq8SzRMoi0IfjOGbNCi1FNVQElTRKpIYOifq8BJF0DsLGpt1J0t9TjNvr%2FdR449Ek6wu4kTT1R3f4RtHMQkiIiS%2FYhPcJhDh3VDhiFniCc%2B%2FuqBOMdjJG0Vw6QC6rRCx5fHYlcYeLP5OFMnWlTAFF0%2FCPTQfql3bnR5zJQ3psH%2BuwUTIG9F56kRP7CMskOqjSr4SPmHSAYetQN8%2Fr55d2rcteUYXFuPSpw8Qa5n%2BOMJ2zPmydMVDtOf7cEmWzCI62b%2BKdmM%2BvcOvg1BUNvefDeGUe%2Bm5MMPjQYQIJgRmPev%2B59avu0gE7liBw%2B99SpiAg%2B5Ol%2BXobSXXwIwanJ4woVpSjNRLh8BMld9Aq3TXnRfMkzWXTYeY7muysR49IcA%2BGQj587%2FVTQrpHfVq%2Fqz0o2FTaDLpDCMq1%2FBQVnebVnrS28hWvRO%2B0Q4ZcloAmzPxq1YHbS7ob3NKbO9nkP1yukcLNEUBJACFGiIZJBXrzkwqwkfPH6tRxwEH3LkXjNfAGDqWR%2Fiz1fctdL05JMiwsDuLluDNB72GAonuJW6QOvxuG8G5B2LxnQX%2FoCvW2ufzi4PAad2UWzfvtyJQWP9MtLc38rlJ2BIVj3tulwj7oPt3vu4ceMdzMsB0tAMJeOhNgGOqUBhuYLdi7BTayZ9zxp4LgMdfmHw6yItrtMyWuGtYMlWIp9FqRnrHw9UGzWVlks9dVgnJMAiEaQZ1W%2FD%2FoCLvHg3oYiADxE4B%2BqnRx5%2BHLrYELNOCI9gNcw73Jh447mc%2FQsLvjjdsjGiqsrMQx534anioFTn%2FKvPEhxc26v29NYUdbZnFb2lxAA9keH3UMSYPiFr398I8itC4X69xltsr%2B31sdckX9F&X-Amz-Signature=e592bfc59cadc452271eccce4882e901d38ce5f766a6a0734daf36d70a015d17&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HZF3JYS%2F20261121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261121T044526Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCICm8CSUiWTIg%2Bt9LY4kbpqvS%2Fp3EVrnnDjDtM4PLW6e1AiEApP3GJJbPDthB1a5KWoFkooAsu%2FooA5jkaiFzuxBZa60q%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDOYUhoi0fOgdm5wh9ircAweq8SzRMoi0IfjOGbNCi1FNVQElTRKpIYOifq8BJF0DsLGpt1J0t9TjNvr%2FdR449Ek6wu4kTT1R3f4RtHMQkiIiS%2FYhPcJhDh3VDhiFniCc%2B%2FuqBOMdjJG0Vw6QC6rRCx5fHYlcYeLP5OFMnWlTAFF0%2FCPTQfql3bnR5zJQ3psH%2BuwUTIG9F56kRP7CMskOqjSr4SPmHSAYetQN8%2Fr55d2rcteUYXFuPSpw8Qa5n%2BOMJ2zPmydMVDtOf7cEmWzCI62b%2BKdmM%2BvcOvg1BUNvefDeGUe%2Bm5MMPjQYQIJgRmPev%2B59avu0gE7liBw%2B99SpiAg%2B5Ol%2BXobSXXwIwanJ4woVpSjNRLh8BMld9Aq3TXnRfMkzWXTYeY7muysR49IcA%2BGQj587%2FVTQrpHfVq%2Fqz0o2FTaDLpDCMq1%2FBQVnebVnrS28hWvRO%2B0Q4ZcloAmzPxq1YHbS7ob3NKbO9nkP1yukcLNEUBJACFGiIZJBXrzkwqwkfPH6tRxwEH3LkXjNfAGDqWR%2Fiz1fctdL05JMiwsDuLluDNB72GAonuJW6QOvxuG8G5B2LxnQX%2FoCvW2ufzi4PAad2UWzfvtyJQWP9MtLc38rlJ2BIVj3tulwj7oPt3vu4ceMdzMsB0tAMJeOhNgGOqUBhuYLdi7BTayZ9zxp4LgMdfmHw6yItrtMyWuGtYMlWIp9FqRnrHw9UGzWVlks9dVgnJMAiEaQZ1W%2FD%2FoCLvHg3oYiADxE4B%2BqnRx5%2BHLrYELNOCI9gNcw73Jh447mc%2FQsLvjjdsjGiqsrMQx534anioFTn%2FKvPEhxc26v29NYUdbZnFb2lxAA9keH3UMSYPiFr398I8itC4X69xltsr%2B31sdckX9F&X-Amz-Signature=1a25dd0c907881c853913f3b98cf877d252f279e226c72443f4d25b630daee0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HZF3JYS%2F20261121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261121T044526Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCICm8CSUiWTIg%2Bt9LY4kbpqvS%2Fp3EVrnnDjDtM4PLW6e1AiEApP3GJJbPDthB1a5KWoFkooAsu%2FooA5jkaiFzuxBZa60q%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDOYUhoi0fOgdm5wh9ircAweq8SzRMoi0IfjOGbNCi1FNVQElTRKpIYOifq8BJF0DsLGpt1J0t9TjNvr%2FdR449Ek6wu4kTT1R3f4RtHMQkiIiS%2FYhPcJhDh3VDhiFniCc%2B%2FuqBOMdjJG0Vw6QC6rRCx5fHYlcYeLP5OFMnWlTAFF0%2FCPTQfql3bnR5zJQ3psH%2BuwUTIG9F56kRP7CMskOqjSr4SPmHSAYetQN8%2Fr55d2rcteUYXFuPSpw8Qa5n%2BOMJ2zPmydMVDtOf7cEmWzCI62b%2BKdmM%2BvcOvg1BUNvefDeGUe%2Bm5MMPjQYQIJgRmPev%2B59avu0gE7liBw%2B99SpiAg%2B5Ol%2BXobSXXwIwanJ4woVpSjNRLh8BMld9Aq3TXnRfMkzWXTYeY7muysR49IcA%2BGQj587%2FVTQrpHfVq%2Fqz0o2FTaDLpDCMq1%2FBQVnebVnrS28hWvRO%2B0Q4ZcloAmzPxq1YHbS7ob3NKbO9nkP1yukcLNEUBJACFGiIZJBXrzkwqwkfPH6tRxwEH3LkXjNfAGDqWR%2Fiz1fctdL05JMiwsDuLluDNB72GAonuJW6QOvxuG8G5B2LxnQX%2FoCvW2ufzi4PAad2UWzfvtyJQWP9MtLc38rlJ2BIVj3tulwj7oPt3vu4ceMdzMsB0tAMJeOhNgGOqUBhuYLdi7BTayZ9zxp4LgMdfmHw6yItrtMyWuGtYMlWIp9FqRnrHw9UGzWVlks9dVgnJMAiEaQZ1W%2FD%2FoCLvHg3oYiADxE4B%2BqnRx5%2BHLrYELNOCI9gNcw73Jh447mc%2FQsLvjjdsjGiqsrMQx534anioFTn%2FKvPEhxc26v29NYUdbZnFb2lxAA9keH3UMSYPiFr398I8itC4X69xltsr%2B31sdckX9F&X-Amz-Signature=6d2f302e1c0f8c631d1c6a44a59ba635bed63e540cca80f859917bbf4ecbe253&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HZF3JYS%2F20261121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261121T044526Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCICm8CSUiWTIg%2Bt9LY4kbpqvS%2Fp3EVrnnDjDtM4PLW6e1AiEApP3GJJbPDthB1a5KWoFkooAsu%2FooA5jkaiFzuxBZa60q%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDOYUhoi0fOgdm5wh9ircAweq8SzRMoi0IfjOGbNCi1FNVQElTRKpIYOifq8BJF0DsLGpt1J0t9TjNvr%2FdR449Ek6wu4kTT1R3f4RtHMQkiIiS%2FYhPcJhDh3VDhiFniCc%2B%2FuqBOMdjJG0Vw6QC6rRCx5fHYlcYeLP5OFMnWlTAFF0%2FCPTQfql3bnR5zJQ3psH%2BuwUTIG9F56kRP7CMskOqjSr4SPmHSAYetQN8%2Fr55d2rcteUYXFuPSpw8Qa5n%2BOMJ2zPmydMVDtOf7cEmWzCI62b%2BKdmM%2BvcOvg1BUNvefDeGUe%2Bm5MMPjQYQIJgRmPev%2B59avu0gE7liBw%2B99SpiAg%2B5Ol%2BXobSXXwIwanJ4woVpSjNRLh8BMld9Aq3TXnRfMkzWXTYeY7muysR49IcA%2BGQj587%2FVTQrpHfVq%2Fqz0o2FTaDLpDCMq1%2FBQVnebVnrS28hWvRO%2B0Q4ZcloAmzPxq1YHbS7ob3NKbO9nkP1yukcLNEUBJACFGiIZJBXrzkwqwkfPH6tRxwEH3LkXjNfAGDqWR%2Fiz1fctdL05JMiwsDuLluDNB72GAonuJW6QOvxuG8G5B2LxnQX%2FoCvW2ufzi4PAad2UWzfvtyJQWP9MtLc38rlJ2BIVj3tulwj7oPt3vu4ceMdzMsB0tAMJeOhNgGOqUBhuYLdi7BTayZ9zxp4LgMdfmHw6yItrtMyWuGtYMlWIp9FqRnrHw9UGzWVlks9dVgnJMAiEaQZ1W%2FD%2FoCLvHg3oYiADxE4B%2BqnRx5%2BHLrYELNOCI9gNcw73Jh447mc%2FQsLvjjdsjGiqsrMQx534anioFTn%2FKvPEhxc26v29NYUdbZnFb2lxAA9keH3UMSYPiFr398I8itC4X69xltsr%2B31sdckX9F&X-Amz-Signature=6519620ce6b20551af273d725b914253680a975092e4280a1a8de7bde44be6fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
