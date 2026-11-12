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
fetched_at: '2026-11-12T04:52:17.732Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z6MC2IQQ%2F20261112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261112T045210Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDdg1zC2K9m4ZOT5Dsf4KYLe8A1%2F2wdn7etMFuFfu8w1gIhAMPtLXdfUlp9BosxZl%2BgWT53DyOUq33165daw9wuW99rKv8DCGMQABoMNjM3NDIzMTgzODA1Igx%2Fm28sHRITdXWvBikq3APjL4JdKbPzaOIG%2Bre60qnh%2F9jooyGhoDSc9KjeDRFWwjn7jVcfWrqfp%2FkXE9U%2FTcRAieYIh2Ya06KVYCzWiHlK6fNF41KddtU%2BvwB4Tga3r41NxfvltJr6UiC%2BTyWVn4S0QaL045j3ZDF7HURB1o4lpkJSm5KieBeEKt52D1LFZh8c03lS%2FuEacIswAjXO8xfuNYezzOMw57KwLUMJ89BWAtl8qEwxIpgoGmu0o9OXOCmtfvDkSwAt4JKjtqtBPeq856BO%2BBSzkPJzBjNw25V8qDAA8rxLbp%2BOAWmT0h70AsEJyJLHekPbkS2W5ZWAQNNFp0BDgSReK5WYpy%2BlJa8zNmS5DQiVb4IPbULR73AkVe5rrplLp80D1BiOPUEAfVWxg%2BFz%2FgJFV0ioxNn%2FRi4sRTrmcIj5VaRQ8j1nWG%2FK5q%2BsQoLw52sw4bY8Jwrdw%2FH5jQLOtenV9FkWtxZ2l94UQRC5olr0HM%2B49dSb5e9DY%2B5E1sOBM%2FsX68tln%2BODUiUwet5QD9JionFwqu%2FSomb%2BShisdBo6Q7LPiWoksly%2Fkyca838Ic78YAECsGuEBcPFWRIc7ligPsBp%2BpWt%2FCXVXEZYwYc8O%2FXgyeAqnkS%2FvVgz17xhX97aoUEN2STCDv9TXBjqkAb1o5EKTij1EIt89SL7SCeSGHgek%2B7PiRKBLltUvZcpCv5pf%2BBvbdWoOJP2iTTZsCX%2B9nL%2BAAT3Bw5XUQflQH6zwDxstskH%2Fva9RBCTwrGSL4fuMnVUvMzCNCJ4eTQ7N4DBBKrjOhzbP7eOwq0N6Yoq0uyQUEk6pz4GhjHiKB4B8PC%2FRhbigq2WUz458EazHIXuq34yOLNVbYdrRvnI1Cz9b1ASq&X-Amz-Signature=49c32bb5382a6892120949b5fedcb3e92132da8d725e5f75a3bce908efdbc492&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z6MC2IQQ%2F20261112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261112T045210Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDdg1zC2K9m4ZOT5Dsf4KYLe8A1%2F2wdn7etMFuFfu8w1gIhAMPtLXdfUlp9BosxZl%2BgWT53DyOUq33165daw9wuW99rKv8DCGMQABoMNjM3NDIzMTgzODA1Igx%2Fm28sHRITdXWvBikq3APjL4JdKbPzaOIG%2Bre60qnh%2F9jooyGhoDSc9KjeDRFWwjn7jVcfWrqfp%2FkXE9U%2FTcRAieYIh2Ya06KVYCzWiHlK6fNF41KddtU%2BvwB4Tga3r41NxfvltJr6UiC%2BTyWVn4S0QaL045j3ZDF7HURB1o4lpkJSm5KieBeEKt52D1LFZh8c03lS%2FuEacIswAjXO8xfuNYezzOMw57KwLUMJ89BWAtl8qEwxIpgoGmu0o9OXOCmtfvDkSwAt4JKjtqtBPeq856BO%2BBSzkPJzBjNw25V8qDAA8rxLbp%2BOAWmT0h70AsEJyJLHekPbkS2W5ZWAQNNFp0BDgSReK5WYpy%2BlJa8zNmS5DQiVb4IPbULR73AkVe5rrplLp80D1BiOPUEAfVWxg%2BFz%2FgJFV0ioxNn%2FRi4sRTrmcIj5VaRQ8j1nWG%2FK5q%2BsQoLw52sw4bY8Jwrdw%2FH5jQLOtenV9FkWtxZ2l94UQRC5olr0HM%2B49dSb5e9DY%2B5E1sOBM%2FsX68tln%2BODUiUwet5QD9JionFwqu%2FSomb%2BShisdBo6Q7LPiWoksly%2Fkyca838Ic78YAECsGuEBcPFWRIc7ligPsBp%2BpWt%2FCXVXEZYwYc8O%2FXgyeAqnkS%2FvVgz17xhX97aoUEN2STCDv9TXBjqkAb1o5EKTij1EIt89SL7SCeSGHgek%2B7PiRKBLltUvZcpCv5pf%2BBvbdWoOJP2iTTZsCX%2B9nL%2BAAT3Bw5XUQflQH6zwDxstskH%2Fva9RBCTwrGSL4fuMnVUvMzCNCJ4eTQ7N4DBBKrjOhzbP7eOwq0N6Yoq0uyQUEk6pz4GhjHiKB4B8PC%2FRhbigq2WUz458EazHIXuq34yOLNVbYdrRvnI1Cz9b1ASq&X-Amz-Signature=8b18ed12eba50be5752886943d4545f34106cde1a3e6987325c638e610641fdf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z6MC2IQQ%2F20261112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261112T045210Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDdg1zC2K9m4ZOT5Dsf4KYLe8A1%2F2wdn7etMFuFfu8w1gIhAMPtLXdfUlp9BosxZl%2BgWT53DyOUq33165daw9wuW99rKv8DCGMQABoMNjM3NDIzMTgzODA1Igx%2Fm28sHRITdXWvBikq3APjL4JdKbPzaOIG%2Bre60qnh%2F9jooyGhoDSc9KjeDRFWwjn7jVcfWrqfp%2FkXE9U%2FTcRAieYIh2Ya06KVYCzWiHlK6fNF41KddtU%2BvwB4Tga3r41NxfvltJr6UiC%2BTyWVn4S0QaL045j3ZDF7HURB1o4lpkJSm5KieBeEKt52D1LFZh8c03lS%2FuEacIswAjXO8xfuNYezzOMw57KwLUMJ89BWAtl8qEwxIpgoGmu0o9OXOCmtfvDkSwAt4JKjtqtBPeq856BO%2BBSzkPJzBjNw25V8qDAA8rxLbp%2BOAWmT0h70AsEJyJLHekPbkS2W5ZWAQNNFp0BDgSReK5WYpy%2BlJa8zNmS5DQiVb4IPbULR73AkVe5rrplLp80D1BiOPUEAfVWxg%2BFz%2FgJFV0ioxNn%2FRi4sRTrmcIj5VaRQ8j1nWG%2FK5q%2BsQoLw52sw4bY8Jwrdw%2FH5jQLOtenV9FkWtxZ2l94UQRC5olr0HM%2B49dSb5e9DY%2B5E1sOBM%2FsX68tln%2BODUiUwet5QD9JionFwqu%2FSomb%2BShisdBo6Q7LPiWoksly%2Fkyca838Ic78YAECsGuEBcPFWRIc7ligPsBp%2BpWt%2FCXVXEZYwYc8O%2FXgyeAqnkS%2FvVgz17xhX97aoUEN2STCDv9TXBjqkAb1o5EKTij1EIt89SL7SCeSGHgek%2B7PiRKBLltUvZcpCv5pf%2BBvbdWoOJP2iTTZsCX%2B9nL%2BAAT3Bw5XUQflQH6zwDxstskH%2Fva9RBCTwrGSL4fuMnVUvMzCNCJ4eTQ7N4DBBKrjOhzbP7eOwq0N6Yoq0uyQUEk6pz4GhjHiKB4B8PC%2FRhbigq2WUz458EazHIXuq34yOLNVbYdrRvnI1Cz9b1ASq&X-Amz-Signature=4e96f7bcddaf4c10175d812b77940598038b26e617865ef00e3c18cff5bef630&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z6MC2IQQ%2F20261112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261112T045210Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDdg1zC2K9m4ZOT5Dsf4KYLe8A1%2F2wdn7etMFuFfu8w1gIhAMPtLXdfUlp9BosxZl%2BgWT53DyOUq33165daw9wuW99rKv8DCGMQABoMNjM3NDIzMTgzODA1Igx%2Fm28sHRITdXWvBikq3APjL4JdKbPzaOIG%2Bre60qnh%2F9jooyGhoDSc9KjeDRFWwjn7jVcfWrqfp%2FkXE9U%2FTcRAieYIh2Ya06KVYCzWiHlK6fNF41KddtU%2BvwB4Tga3r41NxfvltJr6UiC%2BTyWVn4S0QaL045j3ZDF7HURB1o4lpkJSm5KieBeEKt52D1LFZh8c03lS%2FuEacIswAjXO8xfuNYezzOMw57KwLUMJ89BWAtl8qEwxIpgoGmu0o9OXOCmtfvDkSwAt4JKjtqtBPeq856BO%2BBSzkPJzBjNw25V8qDAA8rxLbp%2BOAWmT0h70AsEJyJLHekPbkS2W5ZWAQNNFp0BDgSReK5WYpy%2BlJa8zNmS5DQiVb4IPbULR73AkVe5rrplLp80D1BiOPUEAfVWxg%2BFz%2FgJFV0ioxNn%2FRi4sRTrmcIj5VaRQ8j1nWG%2FK5q%2BsQoLw52sw4bY8Jwrdw%2FH5jQLOtenV9FkWtxZ2l94UQRC5olr0HM%2B49dSb5e9DY%2B5E1sOBM%2FsX68tln%2BODUiUwet5QD9JionFwqu%2FSomb%2BShisdBo6Q7LPiWoksly%2Fkyca838Ic78YAECsGuEBcPFWRIc7ligPsBp%2BpWt%2FCXVXEZYwYc8O%2FXgyeAqnkS%2FvVgz17xhX97aoUEN2STCDv9TXBjqkAb1o5EKTij1EIt89SL7SCeSGHgek%2B7PiRKBLltUvZcpCv5pf%2BBvbdWoOJP2iTTZsCX%2B9nL%2BAAT3Bw5XUQflQH6zwDxstskH%2Fva9RBCTwrGSL4fuMnVUvMzCNCJ4eTQ7N4DBBKrjOhzbP7eOwq0N6Yoq0uyQUEk6pz4GhjHiKB4B8PC%2FRhbigq2WUz458EazHIXuq34yOLNVbYdrRvnI1Cz9b1ASq&X-Amz-Signature=eea7f60980cdc2f5d62d9fd3dda6e9a1075b2fac08cfdb4cbed4e67c4ab5b23a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z6MC2IQQ%2F20261112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261112T045210Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDdg1zC2K9m4ZOT5Dsf4KYLe8A1%2F2wdn7etMFuFfu8w1gIhAMPtLXdfUlp9BosxZl%2BgWT53DyOUq33165daw9wuW99rKv8DCGMQABoMNjM3NDIzMTgzODA1Igx%2Fm28sHRITdXWvBikq3APjL4JdKbPzaOIG%2Bre60qnh%2F9jooyGhoDSc9KjeDRFWwjn7jVcfWrqfp%2FkXE9U%2FTcRAieYIh2Ya06KVYCzWiHlK6fNF41KddtU%2BvwB4Tga3r41NxfvltJr6UiC%2BTyWVn4S0QaL045j3ZDF7HURB1o4lpkJSm5KieBeEKt52D1LFZh8c03lS%2FuEacIswAjXO8xfuNYezzOMw57KwLUMJ89BWAtl8qEwxIpgoGmu0o9OXOCmtfvDkSwAt4JKjtqtBPeq856BO%2BBSzkPJzBjNw25V8qDAA8rxLbp%2BOAWmT0h70AsEJyJLHekPbkS2W5ZWAQNNFp0BDgSReK5WYpy%2BlJa8zNmS5DQiVb4IPbULR73AkVe5rrplLp80D1BiOPUEAfVWxg%2BFz%2FgJFV0ioxNn%2FRi4sRTrmcIj5VaRQ8j1nWG%2FK5q%2BsQoLw52sw4bY8Jwrdw%2FH5jQLOtenV9FkWtxZ2l94UQRC5olr0HM%2B49dSb5e9DY%2B5E1sOBM%2FsX68tln%2BODUiUwet5QD9JionFwqu%2FSomb%2BShisdBo6Q7LPiWoksly%2Fkyca838Ic78YAECsGuEBcPFWRIc7ligPsBp%2BpWt%2FCXVXEZYwYc8O%2FXgyeAqnkS%2FvVgz17xhX97aoUEN2STCDv9TXBjqkAb1o5EKTij1EIt89SL7SCeSGHgek%2B7PiRKBLltUvZcpCv5pf%2BBvbdWoOJP2iTTZsCX%2B9nL%2BAAT3Bw5XUQflQH6zwDxstskH%2Fva9RBCTwrGSL4fuMnVUvMzCNCJ4eTQ7N4DBBKrjOhzbP7eOwq0N6Yoq0uyQUEk6pz4GhjHiKB4B8PC%2FRhbigq2WUz458EazHIXuq34yOLNVbYdrRvnI1Cz9b1ASq&X-Amz-Signature=5666fcd6c8940641facc2a7f51b1a86f893e506700f3d3c8bdcc26a456928409&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z6MC2IQQ%2F20261112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261112T045210Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDdg1zC2K9m4ZOT5Dsf4KYLe8A1%2F2wdn7etMFuFfu8w1gIhAMPtLXdfUlp9BosxZl%2BgWT53DyOUq33165daw9wuW99rKv8DCGMQABoMNjM3NDIzMTgzODA1Igx%2Fm28sHRITdXWvBikq3APjL4JdKbPzaOIG%2Bre60qnh%2F9jooyGhoDSc9KjeDRFWwjn7jVcfWrqfp%2FkXE9U%2FTcRAieYIh2Ya06KVYCzWiHlK6fNF41KddtU%2BvwB4Tga3r41NxfvltJr6UiC%2BTyWVn4S0QaL045j3ZDF7HURB1o4lpkJSm5KieBeEKt52D1LFZh8c03lS%2FuEacIswAjXO8xfuNYezzOMw57KwLUMJ89BWAtl8qEwxIpgoGmu0o9OXOCmtfvDkSwAt4JKjtqtBPeq856BO%2BBSzkPJzBjNw25V8qDAA8rxLbp%2BOAWmT0h70AsEJyJLHekPbkS2W5ZWAQNNFp0BDgSReK5WYpy%2BlJa8zNmS5DQiVb4IPbULR73AkVe5rrplLp80D1BiOPUEAfVWxg%2BFz%2FgJFV0ioxNn%2FRi4sRTrmcIj5VaRQ8j1nWG%2FK5q%2BsQoLw52sw4bY8Jwrdw%2FH5jQLOtenV9FkWtxZ2l94UQRC5olr0HM%2B49dSb5e9DY%2B5E1sOBM%2FsX68tln%2BODUiUwet5QD9JionFwqu%2FSomb%2BShisdBo6Q7LPiWoksly%2Fkyca838Ic78YAECsGuEBcPFWRIc7ligPsBp%2BpWt%2FCXVXEZYwYc8O%2FXgyeAqnkS%2FvVgz17xhX97aoUEN2STCDv9TXBjqkAb1o5EKTij1EIt89SL7SCeSGHgek%2B7PiRKBLltUvZcpCv5pf%2BBvbdWoOJP2iTTZsCX%2B9nL%2BAAT3Bw5XUQflQH6zwDxstskH%2Fva9RBCTwrGSL4fuMnVUvMzCNCJ4eTQ7N4DBBKrjOhzbP7eOwq0N6Yoq0uyQUEk6pz4GhjHiKB4B8PC%2FRhbigq2WUz458EazHIXuq34yOLNVbYdrRvnI1Cz9b1ASq&X-Amz-Signature=8b385eb37cebd90be475c5a5140e8073c7c28fbbb13fd746672b07cc5d1aea26&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z6MC2IQQ%2F20261112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261112T045210Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDdg1zC2K9m4ZOT5Dsf4KYLe8A1%2F2wdn7etMFuFfu8w1gIhAMPtLXdfUlp9BosxZl%2BgWT53DyOUq33165daw9wuW99rKv8DCGMQABoMNjM3NDIzMTgzODA1Igx%2Fm28sHRITdXWvBikq3APjL4JdKbPzaOIG%2Bre60qnh%2F9jooyGhoDSc9KjeDRFWwjn7jVcfWrqfp%2FkXE9U%2FTcRAieYIh2Ya06KVYCzWiHlK6fNF41KddtU%2BvwB4Tga3r41NxfvltJr6UiC%2BTyWVn4S0QaL045j3ZDF7HURB1o4lpkJSm5KieBeEKt52D1LFZh8c03lS%2FuEacIswAjXO8xfuNYezzOMw57KwLUMJ89BWAtl8qEwxIpgoGmu0o9OXOCmtfvDkSwAt4JKjtqtBPeq856BO%2BBSzkPJzBjNw25V8qDAA8rxLbp%2BOAWmT0h70AsEJyJLHekPbkS2W5ZWAQNNFp0BDgSReK5WYpy%2BlJa8zNmS5DQiVb4IPbULR73AkVe5rrplLp80D1BiOPUEAfVWxg%2BFz%2FgJFV0ioxNn%2FRi4sRTrmcIj5VaRQ8j1nWG%2FK5q%2BsQoLw52sw4bY8Jwrdw%2FH5jQLOtenV9FkWtxZ2l94UQRC5olr0HM%2B49dSb5e9DY%2B5E1sOBM%2FsX68tln%2BODUiUwet5QD9JionFwqu%2FSomb%2BShisdBo6Q7LPiWoksly%2Fkyca838Ic78YAECsGuEBcPFWRIc7ligPsBp%2BpWt%2FCXVXEZYwYc8O%2FXgyeAqnkS%2FvVgz17xhX97aoUEN2STCDv9TXBjqkAb1o5EKTij1EIt89SL7SCeSGHgek%2B7PiRKBLltUvZcpCv5pf%2BBvbdWoOJP2iTTZsCX%2B9nL%2BAAT3Bw5XUQflQH6zwDxstskH%2Fva9RBCTwrGSL4fuMnVUvMzCNCJ4eTQ7N4DBBKrjOhzbP7eOwq0N6Yoq0uyQUEk6pz4GhjHiKB4B8PC%2FRhbigq2WUz458EazHIXuq34yOLNVbYdrRvnI1Cz9b1ASq&X-Amz-Signature=4f6a5b50f451e308aba92a3885fa54b88a5e568ef67320047d784399a4ad9f6a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z6MC2IQQ%2F20261112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261112T045210Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDdg1zC2K9m4ZOT5Dsf4KYLe8A1%2F2wdn7etMFuFfu8w1gIhAMPtLXdfUlp9BosxZl%2BgWT53DyOUq33165daw9wuW99rKv8DCGMQABoMNjM3NDIzMTgzODA1Igx%2Fm28sHRITdXWvBikq3APjL4JdKbPzaOIG%2Bre60qnh%2F9jooyGhoDSc9KjeDRFWwjn7jVcfWrqfp%2FkXE9U%2FTcRAieYIh2Ya06KVYCzWiHlK6fNF41KddtU%2BvwB4Tga3r41NxfvltJr6UiC%2BTyWVn4S0QaL045j3ZDF7HURB1o4lpkJSm5KieBeEKt52D1LFZh8c03lS%2FuEacIswAjXO8xfuNYezzOMw57KwLUMJ89BWAtl8qEwxIpgoGmu0o9OXOCmtfvDkSwAt4JKjtqtBPeq856BO%2BBSzkPJzBjNw25V8qDAA8rxLbp%2BOAWmT0h70AsEJyJLHekPbkS2W5ZWAQNNFp0BDgSReK5WYpy%2BlJa8zNmS5DQiVb4IPbULR73AkVe5rrplLp80D1BiOPUEAfVWxg%2BFz%2FgJFV0ioxNn%2FRi4sRTrmcIj5VaRQ8j1nWG%2FK5q%2BsQoLw52sw4bY8Jwrdw%2FH5jQLOtenV9FkWtxZ2l94UQRC5olr0HM%2B49dSb5e9DY%2B5E1sOBM%2FsX68tln%2BODUiUwet5QD9JionFwqu%2FSomb%2BShisdBo6Q7LPiWoksly%2Fkyca838Ic78YAECsGuEBcPFWRIc7ligPsBp%2BpWt%2FCXVXEZYwYc8O%2FXgyeAqnkS%2FvVgz17xhX97aoUEN2STCDv9TXBjqkAb1o5EKTij1EIt89SL7SCeSGHgek%2B7PiRKBLltUvZcpCv5pf%2BBvbdWoOJP2iTTZsCX%2B9nL%2BAAT3Bw5XUQflQH6zwDxstskH%2Fva9RBCTwrGSL4fuMnVUvMzCNCJ4eTQ7N4DBBKrjOhzbP7eOwq0N6Yoq0uyQUEk6pz4GhjHiKB4B8PC%2FRhbigq2WUz458EazHIXuq34yOLNVbYdrRvnI1Cz9b1ASq&X-Amz-Signature=5aa54e2bdc92ca453603b0f7a905fbbdde8bfafca6ac729451cb21db76dc09b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z6MC2IQQ%2F20261112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261112T045210Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDdg1zC2K9m4ZOT5Dsf4KYLe8A1%2F2wdn7etMFuFfu8w1gIhAMPtLXdfUlp9BosxZl%2BgWT53DyOUq33165daw9wuW99rKv8DCGMQABoMNjM3NDIzMTgzODA1Igx%2Fm28sHRITdXWvBikq3APjL4JdKbPzaOIG%2Bre60qnh%2F9jooyGhoDSc9KjeDRFWwjn7jVcfWrqfp%2FkXE9U%2FTcRAieYIh2Ya06KVYCzWiHlK6fNF41KddtU%2BvwB4Tga3r41NxfvltJr6UiC%2BTyWVn4S0QaL045j3ZDF7HURB1o4lpkJSm5KieBeEKt52D1LFZh8c03lS%2FuEacIswAjXO8xfuNYezzOMw57KwLUMJ89BWAtl8qEwxIpgoGmu0o9OXOCmtfvDkSwAt4JKjtqtBPeq856BO%2BBSzkPJzBjNw25V8qDAA8rxLbp%2BOAWmT0h70AsEJyJLHekPbkS2W5ZWAQNNFp0BDgSReK5WYpy%2BlJa8zNmS5DQiVb4IPbULR73AkVe5rrplLp80D1BiOPUEAfVWxg%2BFz%2FgJFV0ioxNn%2FRi4sRTrmcIj5VaRQ8j1nWG%2FK5q%2BsQoLw52sw4bY8Jwrdw%2FH5jQLOtenV9FkWtxZ2l94UQRC5olr0HM%2B49dSb5e9DY%2B5E1sOBM%2FsX68tln%2BODUiUwet5QD9JionFwqu%2FSomb%2BShisdBo6Q7LPiWoksly%2Fkyca838Ic78YAECsGuEBcPFWRIc7ligPsBp%2BpWt%2FCXVXEZYwYc8O%2FXgyeAqnkS%2FvVgz17xhX97aoUEN2STCDv9TXBjqkAb1o5EKTij1EIt89SL7SCeSGHgek%2B7PiRKBLltUvZcpCv5pf%2BBvbdWoOJP2iTTZsCX%2B9nL%2BAAT3Bw5XUQflQH6zwDxstskH%2Fva9RBCTwrGSL4fuMnVUvMzCNCJ4eTQ7N4DBBKrjOhzbP7eOwq0N6Yoq0uyQUEk6pz4GhjHiKB4B8PC%2FRhbigq2WUz458EazHIXuq34yOLNVbYdrRvnI1Cz9b1ASq&X-Amz-Signature=d21af5d816aeecdfdb95ab307c45ad7c3341bd7ddf509b32b7fb937386a13531&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
