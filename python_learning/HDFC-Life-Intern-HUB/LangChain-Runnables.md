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
fetched_at: '2026-11-20T04:51:00.414Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X56NKNUH%2F20261120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261120T045055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCkhvt%2BCh2LBSwDsYk1NQGA5eG%2BPEGSJSuyZ5lsybELBwIhALEznB9upRiUmLZhw13C%2FnB5J9pj5s43GqTOn6tzZ3azKv8DCCMQABoMNjM3NDIzMTgzODA1IgwuZ0DELmnHoT1T5NMq3AMMT2XzX5CqG%2FOHyphyKmBVkYW1DRRx2NqQVBPwGBoACsEd5gVEnLNbHL9QT4zKar2cWstVccojw71jK79MopACRSLmFHgRjRs%2F048VpvaF9lkZ8bltvUe3bMwIWnmql2W9HoFhaas6OW2fXaNVSqJkAgtk1e0DnE82BbGxT4aBlSZPjXaZF4CtGOyna6FMH3S5LkaROSp0D1E5AjOX6quSq9TH5F3pYavVaiYnfjBWkOddslixAT3EXq9cmnMl7nzEdg9NniOMbqGEz0EBha6rCIFRnrZrzdD1ZioltI85ZPVtcN796xb0V4%2BTQCp5uN2HUu7vSXLYXzdlCbpiqFaGjOYHqqIt%2FP3Jnr97YzIrTYV7PTAoE%2B0QZ3kkhVWJ7qrl9MFnCJSP5xGtQZeg1lu%2FKmqLy6q%2Bw9wY1ps58i6AhbjR2z4UVi84xO12YGE7wH3FfFibRbLANtMR0us2R8GmFJ7a%2Bmt1VxaB59SwCP4oRWtSZzm7ZWfgLGgaViuqOBInJgcFdgfiwLpXCLeWvVYuxBiGJoTrb58WP6uxT3UWiNbRjBQjI56aN%2BeeDVrmcLrB5MQA%2FpnWPfKKZT82XBTAITtXWLYNOJvDqR3NZDCvRG6hlKGXM1HqcQzibjCg4%2F7XBjqkAaQC7jAmNv7U03d20yBjCurlenp47korAlT76F2RObA2JtRrK0ZCnqMqbGT0eQTdR4%2BvPkw6n4IVVFU1hpssWrQA%2FMC625LSyA1YWRIBzcU%2BY%2FxnMIZgDO0YH9c2oV7x8ljgO218KwH2bN08ESiQHnMPsUoe09PQDKzEKd8Yz9EPstd85kxtjToGCV82AmXo3teYue48%2FIwI5mfK3Bw37NHSW0Fa&X-Amz-Signature=8f0ff44014533cf06ff3e8bc8c56c345c43a28e3c679e7f440821bd4f5e6521d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X56NKNUH%2F20261120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261120T045055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCkhvt%2BCh2LBSwDsYk1NQGA5eG%2BPEGSJSuyZ5lsybELBwIhALEznB9upRiUmLZhw13C%2FnB5J9pj5s43GqTOn6tzZ3azKv8DCCMQABoMNjM3NDIzMTgzODA1IgwuZ0DELmnHoT1T5NMq3AMMT2XzX5CqG%2FOHyphyKmBVkYW1DRRx2NqQVBPwGBoACsEd5gVEnLNbHL9QT4zKar2cWstVccojw71jK79MopACRSLmFHgRjRs%2F048VpvaF9lkZ8bltvUe3bMwIWnmql2W9HoFhaas6OW2fXaNVSqJkAgtk1e0DnE82BbGxT4aBlSZPjXaZF4CtGOyna6FMH3S5LkaROSp0D1E5AjOX6quSq9TH5F3pYavVaiYnfjBWkOddslixAT3EXq9cmnMl7nzEdg9NniOMbqGEz0EBha6rCIFRnrZrzdD1ZioltI85ZPVtcN796xb0V4%2BTQCp5uN2HUu7vSXLYXzdlCbpiqFaGjOYHqqIt%2FP3Jnr97YzIrTYV7PTAoE%2B0QZ3kkhVWJ7qrl9MFnCJSP5xGtQZeg1lu%2FKmqLy6q%2Bw9wY1ps58i6AhbjR2z4UVi84xO12YGE7wH3FfFibRbLANtMR0us2R8GmFJ7a%2Bmt1VxaB59SwCP4oRWtSZzm7ZWfgLGgaViuqOBInJgcFdgfiwLpXCLeWvVYuxBiGJoTrb58WP6uxT3UWiNbRjBQjI56aN%2BeeDVrmcLrB5MQA%2FpnWPfKKZT82XBTAITtXWLYNOJvDqR3NZDCvRG6hlKGXM1HqcQzibjCg4%2F7XBjqkAaQC7jAmNv7U03d20yBjCurlenp47korAlT76F2RObA2JtRrK0ZCnqMqbGT0eQTdR4%2BvPkw6n4IVVFU1hpssWrQA%2FMC625LSyA1YWRIBzcU%2BY%2FxnMIZgDO0YH9c2oV7x8ljgO218KwH2bN08ESiQHnMPsUoe09PQDKzEKd8Yz9EPstd85kxtjToGCV82AmXo3teYue48%2FIwI5mfK3Bw37NHSW0Fa&X-Amz-Signature=acc74be9fc8f7a22f3590ab8e5015b8fc57877cf9c7cd7c11ac1c42c5054b984&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X56NKNUH%2F20261120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261120T045055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCkhvt%2BCh2LBSwDsYk1NQGA5eG%2BPEGSJSuyZ5lsybELBwIhALEznB9upRiUmLZhw13C%2FnB5J9pj5s43GqTOn6tzZ3azKv8DCCMQABoMNjM3NDIzMTgzODA1IgwuZ0DELmnHoT1T5NMq3AMMT2XzX5CqG%2FOHyphyKmBVkYW1DRRx2NqQVBPwGBoACsEd5gVEnLNbHL9QT4zKar2cWstVccojw71jK79MopACRSLmFHgRjRs%2F048VpvaF9lkZ8bltvUe3bMwIWnmql2W9HoFhaas6OW2fXaNVSqJkAgtk1e0DnE82BbGxT4aBlSZPjXaZF4CtGOyna6FMH3S5LkaROSp0D1E5AjOX6quSq9TH5F3pYavVaiYnfjBWkOddslixAT3EXq9cmnMl7nzEdg9NniOMbqGEz0EBha6rCIFRnrZrzdD1ZioltI85ZPVtcN796xb0V4%2BTQCp5uN2HUu7vSXLYXzdlCbpiqFaGjOYHqqIt%2FP3Jnr97YzIrTYV7PTAoE%2B0QZ3kkhVWJ7qrl9MFnCJSP5xGtQZeg1lu%2FKmqLy6q%2Bw9wY1ps58i6AhbjR2z4UVi84xO12YGE7wH3FfFibRbLANtMR0us2R8GmFJ7a%2Bmt1VxaB59SwCP4oRWtSZzm7ZWfgLGgaViuqOBInJgcFdgfiwLpXCLeWvVYuxBiGJoTrb58WP6uxT3UWiNbRjBQjI56aN%2BeeDVrmcLrB5MQA%2FpnWPfKKZT82XBTAITtXWLYNOJvDqR3NZDCvRG6hlKGXM1HqcQzibjCg4%2F7XBjqkAaQC7jAmNv7U03d20yBjCurlenp47korAlT76F2RObA2JtRrK0ZCnqMqbGT0eQTdR4%2BvPkw6n4IVVFU1hpssWrQA%2FMC625LSyA1YWRIBzcU%2BY%2FxnMIZgDO0YH9c2oV7x8ljgO218KwH2bN08ESiQHnMPsUoe09PQDKzEKd8Yz9EPstd85kxtjToGCV82AmXo3teYue48%2FIwI5mfK3Bw37NHSW0Fa&X-Amz-Signature=cb71e848e67e7d0cd8624f887874a01201c833527a0d3996aab9eea8e4d57036&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X56NKNUH%2F20261120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261120T045055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCkhvt%2BCh2LBSwDsYk1NQGA5eG%2BPEGSJSuyZ5lsybELBwIhALEznB9upRiUmLZhw13C%2FnB5J9pj5s43GqTOn6tzZ3azKv8DCCMQABoMNjM3NDIzMTgzODA1IgwuZ0DELmnHoT1T5NMq3AMMT2XzX5CqG%2FOHyphyKmBVkYW1DRRx2NqQVBPwGBoACsEd5gVEnLNbHL9QT4zKar2cWstVccojw71jK79MopACRSLmFHgRjRs%2F048VpvaF9lkZ8bltvUe3bMwIWnmql2W9HoFhaas6OW2fXaNVSqJkAgtk1e0DnE82BbGxT4aBlSZPjXaZF4CtGOyna6FMH3S5LkaROSp0D1E5AjOX6quSq9TH5F3pYavVaiYnfjBWkOddslixAT3EXq9cmnMl7nzEdg9NniOMbqGEz0EBha6rCIFRnrZrzdD1ZioltI85ZPVtcN796xb0V4%2BTQCp5uN2HUu7vSXLYXzdlCbpiqFaGjOYHqqIt%2FP3Jnr97YzIrTYV7PTAoE%2B0QZ3kkhVWJ7qrl9MFnCJSP5xGtQZeg1lu%2FKmqLy6q%2Bw9wY1ps58i6AhbjR2z4UVi84xO12YGE7wH3FfFibRbLANtMR0us2R8GmFJ7a%2Bmt1VxaB59SwCP4oRWtSZzm7ZWfgLGgaViuqOBInJgcFdgfiwLpXCLeWvVYuxBiGJoTrb58WP6uxT3UWiNbRjBQjI56aN%2BeeDVrmcLrB5MQA%2FpnWPfKKZT82XBTAITtXWLYNOJvDqR3NZDCvRG6hlKGXM1HqcQzibjCg4%2F7XBjqkAaQC7jAmNv7U03d20yBjCurlenp47korAlT76F2RObA2JtRrK0ZCnqMqbGT0eQTdR4%2BvPkw6n4IVVFU1hpssWrQA%2FMC625LSyA1YWRIBzcU%2BY%2FxnMIZgDO0YH9c2oV7x8ljgO218KwH2bN08ESiQHnMPsUoe09PQDKzEKd8Yz9EPstd85kxtjToGCV82AmXo3teYue48%2FIwI5mfK3Bw37NHSW0Fa&X-Amz-Signature=d6b84657ef832109facc159bbfc24b586dfb03d325b89555bfeb994f0ed1e25a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X56NKNUH%2F20261120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261120T045055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCkhvt%2BCh2LBSwDsYk1NQGA5eG%2BPEGSJSuyZ5lsybELBwIhALEznB9upRiUmLZhw13C%2FnB5J9pj5s43GqTOn6tzZ3azKv8DCCMQABoMNjM3NDIzMTgzODA1IgwuZ0DELmnHoT1T5NMq3AMMT2XzX5CqG%2FOHyphyKmBVkYW1DRRx2NqQVBPwGBoACsEd5gVEnLNbHL9QT4zKar2cWstVccojw71jK79MopACRSLmFHgRjRs%2F048VpvaF9lkZ8bltvUe3bMwIWnmql2W9HoFhaas6OW2fXaNVSqJkAgtk1e0DnE82BbGxT4aBlSZPjXaZF4CtGOyna6FMH3S5LkaROSp0D1E5AjOX6quSq9TH5F3pYavVaiYnfjBWkOddslixAT3EXq9cmnMl7nzEdg9NniOMbqGEz0EBha6rCIFRnrZrzdD1ZioltI85ZPVtcN796xb0V4%2BTQCp5uN2HUu7vSXLYXzdlCbpiqFaGjOYHqqIt%2FP3Jnr97YzIrTYV7PTAoE%2B0QZ3kkhVWJ7qrl9MFnCJSP5xGtQZeg1lu%2FKmqLy6q%2Bw9wY1ps58i6AhbjR2z4UVi84xO12YGE7wH3FfFibRbLANtMR0us2R8GmFJ7a%2Bmt1VxaB59SwCP4oRWtSZzm7ZWfgLGgaViuqOBInJgcFdgfiwLpXCLeWvVYuxBiGJoTrb58WP6uxT3UWiNbRjBQjI56aN%2BeeDVrmcLrB5MQA%2FpnWPfKKZT82XBTAITtXWLYNOJvDqR3NZDCvRG6hlKGXM1HqcQzibjCg4%2F7XBjqkAaQC7jAmNv7U03d20yBjCurlenp47korAlT76F2RObA2JtRrK0ZCnqMqbGT0eQTdR4%2BvPkw6n4IVVFU1hpssWrQA%2FMC625LSyA1YWRIBzcU%2BY%2FxnMIZgDO0YH9c2oV7x8ljgO218KwH2bN08ESiQHnMPsUoe09PQDKzEKd8Yz9EPstd85kxtjToGCV82AmXo3teYue48%2FIwI5mfK3Bw37NHSW0Fa&X-Amz-Signature=c4f75dcc0a8462b62601d770949232e91c57f14eb67d45e71941234a685bcb17&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X56NKNUH%2F20261120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261120T045055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCkhvt%2BCh2LBSwDsYk1NQGA5eG%2BPEGSJSuyZ5lsybELBwIhALEznB9upRiUmLZhw13C%2FnB5J9pj5s43GqTOn6tzZ3azKv8DCCMQABoMNjM3NDIzMTgzODA1IgwuZ0DELmnHoT1T5NMq3AMMT2XzX5CqG%2FOHyphyKmBVkYW1DRRx2NqQVBPwGBoACsEd5gVEnLNbHL9QT4zKar2cWstVccojw71jK79MopACRSLmFHgRjRs%2F048VpvaF9lkZ8bltvUe3bMwIWnmql2W9HoFhaas6OW2fXaNVSqJkAgtk1e0DnE82BbGxT4aBlSZPjXaZF4CtGOyna6FMH3S5LkaROSp0D1E5AjOX6quSq9TH5F3pYavVaiYnfjBWkOddslixAT3EXq9cmnMl7nzEdg9NniOMbqGEz0EBha6rCIFRnrZrzdD1ZioltI85ZPVtcN796xb0V4%2BTQCp5uN2HUu7vSXLYXzdlCbpiqFaGjOYHqqIt%2FP3Jnr97YzIrTYV7PTAoE%2B0QZ3kkhVWJ7qrl9MFnCJSP5xGtQZeg1lu%2FKmqLy6q%2Bw9wY1ps58i6AhbjR2z4UVi84xO12YGE7wH3FfFibRbLANtMR0us2R8GmFJ7a%2Bmt1VxaB59SwCP4oRWtSZzm7ZWfgLGgaViuqOBInJgcFdgfiwLpXCLeWvVYuxBiGJoTrb58WP6uxT3UWiNbRjBQjI56aN%2BeeDVrmcLrB5MQA%2FpnWPfKKZT82XBTAITtXWLYNOJvDqR3NZDCvRG6hlKGXM1HqcQzibjCg4%2F7XBjqkAaQC7jAmNv7U03d20yBjCurlenp47korAlT76F2RObA2JtRrK0ZCnqMqbGT0eQTdR4%2BvPkw6n4IVVFU1hpssWrQA%2FMC625LSyA1YWRIBzcU%2BY%2FxnMIZgDO0YH9c2oV7x8ljgO218KwH2bN08ESiQHnMPsUoe09PQDKzEKd8Yz9EPstd85kxtjToGCV82AmXo3teYue48%2FIwI5mfK3Bw37NHSW0Fa&X-Amz-Signature=46ce5394e4778abe8b1c81fdff5e9a132e1089ae530d4c8d5d14425a4c6553b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X56NKNUH%2F20261120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261120T045055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCkhvt%2BCh2LBSwDsYk1NQGA5eG%2BPEGSJSuyZ5lsybELBwIhALEznB9upRiUmLZhw13C%2FnB5J9pj5s43GqTOn6tzZ3azKv8DCCMQABoMNjM3NDIzMTgzODA1IgwuZ0DELmnHoT1T5NMq3AMMT2XzX5CqG%2FOHyphyKmBVkYW1DRRx2NqQVBPwGBoACsEd5gVEnLNbHL9QT4zKar2cWstVccojw71jK79MopACRSLmFHgRjRs%2F048VpvaF9lkZ8bltvUe3bMwIWnmql2W9HoFhaas6OW2fXaNVSqJkAgtk1e0DnE82BbGxT4aBlSZPjXaZF4CtGOyna6FMH3S5LkaROSp0D1E5AjOX6quSq9TH5F3pYavVaiYnfjBWkOddslixAT3EXq9cmnMl7nzEdg9NniOMbqGEz0EBha6rCIFRnrZrzdD1ZioltI85ZPVtcN796xb0V4%2BTQCp5uN2HUu7vSXLYXzdlCbpiqFaGjOYHqqIt%2FP3Jnr97YzIrTYV7PTAoE%2B0QZ3kkhVWJ7qrl9MFnCJSP5xGtQZeg1lu%2FKmqLy6q%2Bw9wY1ps58i6AhbjR2z4UVi84xO12YGE7wH3FfFibRbLANtMR0us2R8GmFJ7a%2Bmt1VxaB59SwCP4oRWtSZzm7ZWfgLGgaViuqOBInJgcFdgfiwLpXCLeWvVYuxBiGJoTrb58WP6uxT3UWiNbRjBQjI56aN%2BeeDVrmcLrB5MQA%2FpnWPfKKZT82XBTAITtXWLYNOJvDqR3NZDCvRG6hlKGXM1HqcQzibjCg4%2F7XBjqkAaQC7jAmNv7U03d20yBjCurlenp47korAlT76F2RObA2JtRrK0ZCnqMqbGT0eQTdR4%2BvPkw6n4IVVFU1hpssWrQA%2FMC625LSyA1YWRIBzcU%2BY%2FxnMIZgDO0YH9c2oV7x8ljgO218KwH2bN08ESiQHnMPsUoe09PQDKzEKd8Yz9EPstd85kxtjToGCV82AmXo3teYue48%2FIwI5mfK3Bw37NHSW0Fa&X-Amz-Signature=9590e986b340be573c05d4c04025afb45ebddc846889fe6e047c594df4eaecbf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X56NKNUH%2F20261120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261120T045055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCkhvt%2BCh2LBSwDsYk1NQGA5eG%2BPEGSJSuyZ5lsybELBwIhALEznB9upRiUmLZhw13C%2FnB5J9pj5s43GqTOn6tzZ3azKv8DCCMQABoMNjM3NDIzMTgzODA1IgwuZ0DELmnHoT1T5NMq3AMMT2XzX5CqG%2FOHyphyKmBVkYW1DRRx2NqQVBPwGBoACsEd5gVEnLNbHL9QT4zKar2cWstVccojw71jK79MopACRSLmFHgRjRs%2F048VpvaF9lkZ8bltvUe3bMwIWnmql2W9HoFhaas6OW2fXaNVSqJkAgtk1e0DnE82BbGxT4aBlSZPjXaZF4CtGOyna6FMH3S5LkaROSp0D1E5AjOX6quSq9TH5F3pYavVaiYnfjBWkOddslixAT3EXq9cmnMl7nzEdg9NniOMbqGEz0EBha6rCIFRnrZrzdD1ZioltI85ZPVtcN796xb0V4%2BTQCp5uN2HUu7vSXLYXzdlCbpiqFaGjOYHqqIt%2FP3Jnr97YzIrTYV7PTAoE%2B0QZ3kkhVWJ7qrl9MFnCJSP5xGtQZeg1lu%2FKmqLy6q%2Bw9wY1ps58i6AhbjR2z4UVi84xO12YGE7wH3FfFibRbLANtMR0us2R8GmFJ7a%2Bmt1VxaB59SwCP4oRWtSZzm7ZWfgLGgaViuqOBInJgcFdgfiwLpXCLeWvVYuxBiGJoTrb58WP6uxT3UWiNbRjBQjI56aN%2BeeDVrmcLrB5MQA%2FpnWPfKKZT82XBTAITtXWLYNOJvDqR3NZDCvRG6hlKGXM1HqcQzibjCg4%2F7XBjqkAaQC7jAmNv7U03d20yBjCurlenp47korAlT76F2RObA2JtRrK0ZCnqMqbGT0eQTdR4%2BvPkw6n4IVVFU1hpssWrQA%2FMC625LSyA1YWRIBzcU%2BY%2FxnMIZgDO0YH9c2oV7x8ljgO218KwH2bN08ESiQHnMPsUoe09PQDKzEKd8Yz9EPstd85kxtjToGCV82AmXo3teYue48%2FIwI5mfK3Bw37NHSW0Fa&X-Amz-Signature=2f4bc848060e3e5d153beeb074df011c24e5ae495acf154f0311b20889466774&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X56NKNUH%2F20261120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261120T045055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCkhvt%2BCh2LBSwDsYk1NQGA5eG%2BPEGSJSuyZ5lsybELBwIhALEznB9upRiUmLZhw13C%2FnB5J9pj5s43GqTOn6tzZ3azKv8DCCMQABoMNjM3NDIzMTgzODA1IgwuZ0DELmnHoT1T5NMq3AMMT2XzX5CqG%2FOHyphyKmBVkYW1DRRx2NqQVBPwGBoACsEd5gVEnLNbHL9QT4zKar2cWstVccojw71jK79MopACRSLmFHgRjRs%2F048VpvaF9lkZ8bltvUe3bMwIWnmql2W9HoFhaas6OW2fXaNVSqJkAgtk1e0DnE82BbGxT4aBlSZPjXaZF4CtGOyna6FMH3S5LkaROSp0D1E5AjOX6quSq9TH5F3pYavVaiYnfjBWkOddslixAT3EXq9cmnMl7nzEdg9NniOMbqGEz0EBha6rCIFRnrZrzdD1ZioltI85ZPVtcN796xb0V4%2BTQCp5uN2HUu7vSXLYXzdlCbpiqFaGjOYHqqIt%2FP3Jnr97YzIrTYV7PTAoE%2B0QZ3kkhVWJ7qrl9MFnCJSP5xGtQZeg1lu%2FKmqLy6q%2Bw9wY1ps58i6AhbjR2z4UVi84xO12YGE7wH3FfFibRbLANtMR0us2R8GmFJ7a%2Bmt1VxaB59SwCP4oRWtSZzm7ZWfgLGgaViuqOBInJgcFdgfiwLpXCLeWvVYuxBiGJoTrb58WP6uxT3UWiNbRjBQjI56aN%2BeeDVrmcLrB5MQA%2FpnWPfKKZT82XBTAITtXWLYNOJvDqR3NZDCvRG6hlKGXM1HqcQzibjCg4%2F7XBjqkAaQC7jAmNv7U03d20yBjCurlenp47korAlT76F2RObA2JtRrK0ZCnqMqbGT0eQTdR4%2BvPkw6n4IVVFU1hpssWrQA%2FMC625LSyA1YWRIBzcU%2BY%2FxnMIZgDO0YH9c2oV7x8ljgO218KwH2bN08ESiQHnMPsUoe09PQDKzEKd8Yz9EPstd85kxtjToGCV82AmXo3teYue48%2FIwI5mfK3Bw37NHSW0Fa&X-Amz-Signature=4cb624cd95f4345f8a666b29541317e19e6bdf1fa40c194dbbdcfa2e091b69b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
