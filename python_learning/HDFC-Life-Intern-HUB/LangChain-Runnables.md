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
fetched_at: '2026-09-15T02:25:22.952Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZSWMHVC%2F20260915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260915T022518Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIEhmKUsLqf2eTqvldc9%2FbUtD9AC0KaT3eFw2NGL0KhXYAiAQEeNtXv7%2FTSpc1jrZIMlh8Mifdt7moDA2hhLWRQxFECqIBAjy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpWyPHyWeXhl2GLnFKtwDnGRCgOBrULzqGlJIsMSbJmohNENk7hXD9mNSVbrwOKpQm81YzXuhNUY7UCXjKpBFELkqI6Bh2I6sO%2FV4itugXqexNz%2BQfjK3ju%2FdHE7mBA6lGAj5fTvk43WPAYvH%2B%2BpB29SGoAUSXPEhSvdWa0oq%2FGTmLkIIY14fAo3Dg2t64%2BaSxLvPdDtYI5M6a0O1DaqhbZehWRROaVqpMsakrWyyJ%2B0ouM83OwXfQNMO7PMlxwQmnxh3puZIIveiajsVQycUxBXeduZElVMYanXqVcyjAv9hxCw7nyhrt4BB1xPBKjs7ZFbGTvsDiN2LfzbFiZnav%2FN8WNIeNECP%2FVxRF%2FWRtc6p6b%2BmLKidY%2BkUSc%2FYSCQ91RoTfxmsh23XsbQUyOkyOjBK%2Bx54JFgf1m%2Fq9%2BMXMYk%2FxMkWrwENUczgiCoNFunF6jH2UKQqBWuDjTQL8lTcc3qqv4OYhv9m%2FEKUXDPg4xAueVz%2Bg%2FdtOMvLUnJMdeBkLhWMJs1pbgrwY0TJydq5b6FngSewTZ%2Fu33cvi1vseGHvHTkUAbPNfpyBfYqkkEJiKuXAQE0T07S1v8SAuJTplWmeR6zyaxQU0kjrrsM6AeOQZruE6Kzr2kjaR8NQ9cfkcI46nGgpJulABsQwhKKi1QY6pgGVLc6NQZFv%2FfJuH6hcY12EneUv66zXhUpRbmLIpGNgrPPwOdxtEjqLduW%2FoUQkJ2lqKzny7wv6VGMpI5HJPX713kjvSHv1zcRMJZkMljkb6yWtn0vLhhyFXhuQdtSGzEmEoQNIHsmN%2F%2FQWciWvCBQ%2BxtD0QZyAgyuvUX%2FKy1tTE42gPce4bVWemgcNrRW3cWnIdHFKRhkGf0zQE%2Bm41uNOQlap5z0j&X-Amz-Signature=836880603b32de91db4187b8a29b05ad31f914861f8d6bc1bb513e99e9d8a2e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZSWMHVC%2F20260915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260915T022518Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIEhmKUsLqf2eTqvldc9%2FbUtD9AC0KaT3eFw2NGL0KhXYAiAQEeNtXv7%2FTSpc1jrZIMlh8Mifdt7moDA2hhLWRQxFECqIBAjy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpWyPHyWeXhl2GLnFKtwDnGRCgOBrULzqGlJIsMSbJmohNENk7hXD9mNSVbrwOKpQm81YzXuhNUY7UCXjKpBFELkqI6Bh2I6sO%2FV4itugXqexNz%2BQfjK3ju%2FdHE7mBA6lGAj5fTvk43WPAYvH%2B%2BpB29SGoAUSXPEhSvdWa0oq%2FGTmLkIIY14fAo3Dg2t64%2BaSxLvPdDtYI5M6a0O1DaqhbZehWRROaVqpMsakrWyyJ%2B0ouM83OwXfQNMO7PMlxwQmnxh3puZIIveiajsVQycUxBXeduZElVMYanXqVcyjAv9hxCw7nyhrt4BB1xPBKjs7ZFbGTvsDiN2LfzbFiZnav%2FN8WNIeNECP%2FVxRF%2FWRtc6p6b%2BmLKidY%2BkUSc%2FYSCQ91RoTfxmsh23XsbQUyOkyOjBK%2Bx54JFgf1m%2Fq9%2BMXMYk%2FxMkWrwENUczgiCoNFunF6jH2UKQqBWuDjTQL8lTcc3qqv4OYhv9m%2FEKUXDPg4xAueVz%2Bg%2FdtOMvLUnJMdeBkLhWMJs1pbgrwY0TJydq5b6FngSewTZ%2Fu33cvi1vseGHvHTkUAbPNfpyBfYqkkEJiKuXAQE0T07S1v8SAuJTplWmeR6zyaxQU0kjrrsM6AeOQZruE6Kzr2kjaR8NQ9cfkcI46nGgpJulABsQwhKKi1QY6pgGVLc6NQZFv%2FfJuH6hcY12EneUv66zXhUpRbmLIpGNgrPPwOdxtEjqLduW%2FoUQkJ2lqKzny7wv6VGMpI5HJPX713kjvSHv1zcRMJZkMljkb6yWtn0vLhhyFXhuQdtSGzEmEoQNIHsmN%2F%2FQWciWvCBQ%2BxtD0QZyAgyuvUX%2FKy1tTE42gPce4bVWemgcNrRW3cWnIdHFKRhkGf0zQE%2Bm41uNOQlap5z0j&X-Amz-Signature=1b946f07086e799a2cb1aea089b7864867de175a96d93b07991abeabd41f4294&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZSWMHVC%2F20260915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260915T022518Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIEhmKUsLqf2eTqvldc9%2FbUtD9AC0KaT3eFw2NGL0KhXYAiAQEeNtXv7%2FTSpc1jrZIMlh8Mifdt7moDA2hhLWRQxFECqIBAjy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpWyPHyWeXhl2GLnFKtwDnGRCgOBrULzqGlJIsMSbJmohNENk7hXD9mNSVbrwOKpQm81YzXuhNUY7UCXjKpBFELkqI6Bh2I6sO%2FV4itugXqexNz%2BQfjK3ju%2FdHE7mBA6lGAj5fTvk43WPAYvH%2B%2BpB29SGoAUSXPEhSvdWa0oq%2FGTmLkIIY14fAo3Dg2t64%2BaSxLvPdDtYI5M6a0O1DaqhbZehWRROaVqpMsakrWyyJ%2B0ouM83OwXfQNMO7PMlxwQmnxh3puZIIveiajsVQycUxBXeduZElVMYanXqVcyjAv9hxCw7nyhrt4BB1xPBKjs7ZFbGTvsDiN2LfzbFiZnav%2FN8WNIeNECP%2FVxRF%2FWRtc6p6b%2BmLKidY%2BkUSc%2FYSCQ91RoTfxmsh23XsbQUyOkyOjBK%2Bx54JFgf1m%2Fq9%2BMXMYk%2FxMkWrwENUczgiCoNFunF6jH2UKQqBWuDjTQL8lTcc3qqv4OYhv9m%2FEKUXDPg4xAueVz%2Bg%2FdtOMvLUnJMdeBkLhWMJs1pbgrwY0TJydq5b6FngSewTZ%2Fu33cvi1vseGHvHTkUAbPNfpyBfYqkkEJiKuXAQE0T07S1v8SAuJTplWmeR6zyaxQU0kjrrsM6AeOQZruE6Kzr2kjaR8NQ9cfkcI46nGgpJulABsQwhKKi1QY6pgGVLc6NQZFv%2FfJuH6hcY12EneUv66zXhUpRbmLIpGNgrPPwOdxtEjqLduW%2FoUQkJ2lqKzny7wv6VGMpI5HJPX713kjvSHv1zcRMJZkMljkb6yWtn0vLhhyFXhuQdtSGzEmEoQNIHsmN%2F%2FQWciWvCBQ%2BxtD0QZyAgyuvUX%2FKy1tTE42gPce4bVWemgcNrRW3cWnIdHFKRhkGf0zQE%2Bm41uNOQlap5z0j&X-Amz-Signature=24f48d07462cbf767dbe9c3b1f42440c224b10484c6e992c3488f75a30a90197&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZSWMHVC%2F20260915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260915T022518Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIEhmKUsLqf2eTqvldc9%2FbUtD9AC0KaT3eFw2NGL0KhXYAiAQEeNtXv7%2FTSpc1jrZIMlh8Mifdt7moDA2hhLWRQxFECqIBAjy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpWyPHyWeXhl2GLnFKtwDnGRCgOBrULzqGlJIsMSbJmohNENk7hXD9mNSVbrwOKpQm81YzXuhNUY7UCXjKpBFELkqI6Bh2I6sO%2FV4itugXqexNz%2BQfjK3ju%2FdHE7mBA6lGAj5fTvk43WPAYvH%2B%2BpB29SGoAUSXPEhSvdWa0oq%2FGTmLkIIY14fAo3Dg2t64%2BaSxLvPdDtYI5M6a0O1DaqhbZehWRROaVqpMsakrWyyJ%2B0ouM83OwXfQNMO7PMlxwQmnxh3puZIIveiajsVQycUxBXeduZElVMYanXqVcyjAv9hxCw7nyhrt4BB1xPBKjs7ZFbGTvsDiN2LfzbFiZnav%2FN8WNIeNECP%2FVxRF%2FWRtc6p6b%2BmLKidY%2BkUSc%2FYSCQ91RoTfxmsh23XsbQUyOkyOjBK%2Bx54JFgf1m%2Fq9%2BMXMYk%2FxMkWrwENUczgiCoNFunF6jH2UKQqBWuDjTQL8lTcc3qqv4OYhv9m%2FEKUXDPg4xAueVz%2Bg%2FdtOMvLUnJMdeBkLhWMJs1pbgrwY0TJydq5b6FngSewTZ%2Fu33cvi1vseGHvHTkUAbPNfpyBfYqkkEJiKuXAQE0T07S1v8SAuJTplWmeR6zyaxQU0kjrrsM6AeOQZruE6Kzr2kjaR8NQ9cfkcI46nGgpJulABsQwhKKi1QY6pgGVLc6NQZFv%2FfJuH6hcY12EneUv66zXhUpRbmLIpGNgrPPwOdxtEjqLduW%2FoUQkJ2lqKzny7wv6VGMpI5HJPX713kjvSHv1zcRMJZkMljkb6yWtn0vLhhyFXhuQdtSGzEmEoQNIHsmN%2F%2FQWciWvCBQ%2BxtD0QZyAgyuvUX%2FKy1tTE42gPce4bVWemgcNrRW3cWnIdHFKRhkGf0zQE%2Bm41uNOQlap5z0j&X-Amz-Signature=4d3df5a0b7e6dc6d2b4623a6155122c80a036bc499b64e13bfdaa28cbe6b0d34&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZSWMHVC%2F20260915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260915T022518Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIEhmKUsLqf2eTqvldc9%2FbUtD9AC0KaT3eFw2NGL0KhXYAiAQEeNtXv7%2FTSpc1jrZIMlh8Mifdt7moDA2hhLWRQxFECqIBAjy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpWyPHyWeXhl2GLnFKtwDnGRCgOBrULzqGlJIsMSbJmohNENk7hXD9mNSVbrwOKpQm81YzXuhNUY7UCXjKpBFELkqI6Bh2I6sO%2FV4itugXqexNz%2BQfjK3ju%2FdHE7mBA6lGAj5fTvk43WPAYvH%2B%2BpB29SGoAUSXPEhSvdWa0oq%2FGTmLkIIY14fAo3Dg2t64%2BaSxLvPdDtYI5M6a0O1DaqhbZehWRROaVqpMsakrWyyJ%2B0ouM83OwXfQNMO7PMlxwQmnxh3puZIIveiajsVQycUxBXeduZElVMYanXqVcyjAv9hxCw7nyhrt4BB1xPBKjs7ZFbGTvsDiN2LfzbFiZnav%2FN8WNIeNECP%2FVxRF%2FWRtc6p6b%2BmLKidY%2BkUSc%2FYSCQ91RoTfxmsh23XsbQUyOkyOjBK%2Bx54JFgf1m%2Fq9%2BMXMYk%2FxMkWrwENUczgiCoNFunF6jH2UKQqBWuDjTQL8lTcc3qqv4OYhv9m%2FEKUXDPg4xAueVz%2Bg%2FdtOMvLUnJMdeBkLhWMJs1pbgrwY0TJydq5b6FngSewTZ%2Fu33cvi1vseGHvHTkUAbPNfpyBfYqkkEJiKuXAQE0T07S1v8SAuJTplWmeR6zyaxQU0kjrrsM6AeOQZruE6Kzr2kjaR8NQ9cfkcI46nGgpJulABsQwhKKi1QY6pgGVLc6NQZFv%2FfJuH6hcY12EneUv66zXhUpRbmLIpGNgrPPwOdxtEjqLduW%2FoUQkJ2lqKzny7wv6VGMpI5HJPX713kjvSHv1zcRMJZkMljkb6yWtn0vLhhyFXhuQdtSGzEmEoQNIHsmN%2F%2FQWciWvCBQ%2BxtD0QZyAgyuvUX%2FKy1tTE42gPce4bVWemgcNrRW3cWnIdHFKRhkGf0zQE%2Bm41uNOQlap5z0j&X-Amz-Signature=1b6095e846f10e6ccad0ab845cff3ae1a19038f15b390002a39bb5d8dcf273db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZSWMHVC%2F20260915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260915T022518Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIEhmKUsLqf2eTqvldc9%2FbUtD9AC0KaT3eFw2NGL0KhXYAiAQEeNtXv7%2FTSpc1jrZIMlh8Mifdt7moDA2hhLWRQxFECqIBAjy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpWyPHyWeXhl2GLnFKtwDnGRCgOBrULzqGlJIsMSbJmohNENk7hXD9mNSVbrwOKpQm81YzXuhNUY7UCXjKpBFELkqI6Bh2I6sO%2FV4itugXqexNz%2BQfjK3ju%2FdHE7mBA6lGAj5fTvk43WPAYvH%2B%2BpB29SGoAUSXPEhSvdWa0oq%2FGTmLkIIY14fAo3Dg2t64%2BaSxLvPdDtYI5M6a0O1DaqhbZehWRROaVqpMsakrWyyJ%2B0ouM83OwXfQNMO7PMlxwQmnxh3puZIIveiajsVQycUxBXeduZElVMYanXqVcyjAv9hxCw7nyhrt4BB1xPBKjs7ZFbGTvsDiN2LfzbFiZnav%2FN8WNIeNECP%2FVxRF%2FWRtc6p6b%2BmLKidY%2BkUSc%2FYSCQ91RoTfxmsh23XsbQUyOkyOjBK%2Bx54JFgf1m%2Fq9%2BMXMYk%2FxMkWrwENUczgiCoNFunF6jH2UKQqBWuDjTQL8lTcc3qqv4OYhv9m%2FEKUXDPg4xAueVz%2Bg%2FdtOMvLUnJMdeBkLhWMJs1pbgrwY0TJydq5b6FngSewTZ%2Fu33cvi1vseGHvHTkUAbPNfpyBfYqkkEJiKuXAQE0T07S1v8SAuJTplWmeR6zyaxQU0kjrrsM6AeOQZruE6Kzr2kjaR8NQ9cfkcI46nGgpJulABsQwhKKi1QY6pgGVLc6NQZFv%2FfJuH6hcY12EneUv66zXhUpRbmLIpGNgrPPwOdxtEjqLduW%2FoUQkJ2lqKzny7wv6VGMpI5HJPX713kjvSHv1zcRMJZkMljkb6yWtn0vLhhyFXhuQdtSGzEmEoQNIHsmN%2F%2FQWciWvCBQ%2BxtD0QZyAgyuvUX%2FKy1tTE42gPce4bVWemgcNrRW3cWnIdHFKRhkGf0zQE%2Bm41uNOQlap5z0j&X-Amz-Signature=e3b60cfd97dc3b5a181556f73c42ed5a3261478e7e396d5b0f3fd66b3c577856&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZSWMHVC%2F20260915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260915T022518Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIEhmKUsLqf2eTqvldc9%2FbUtD9AC0KaT3eFw2NGL0KhXYAiAQEeNtXv7%2FTSpc1jrZIMlh8Mifdt7moDA2hhLWRQxFECqIBAjy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpWyPHyWeXhl2GLnFKtwDnGRCgOBrULzqGlJIsMSbJmohNENk7hXD9mNSVbrwOKpQm81YzXuhNUY7UCXjKpBFELkqI6Bh2I6sO%2FV4itugXqexNz%2BQfjK3ju%2FdHE7mBA6lGAj5fTvk43WPAYvH%2B%2BpB29SGoAUSXPEhSvdWa0oq%2FGTmLkIIY14fAo3Dg2t64%2BaSxLvPdDtYI5M6a0O1DaqhbZehWRROaVqpMsakrWyyJ%2B0ouM83OwXfQNMO7PMlxwQmnxh3puZIIveiajsVQycUxBXeduZElVMYanXqVcyjAv9hxCw7nyhrt4BB1xPBKjs7ZFbGTvsDiN2LfzbFiZnav%2FN8WNIeNECP%2FVxRF%2FWRtc6p6b%2BmLKidY%2BkUSc%2FYSCQ91RoTfxmsh23XsbQUyOkyOjBK%2Bx54JFgf1m%2Fq9%2BMXMYk%2FxMkWrwENUczgiCoNFunF6jH2UKQqBWuDjTQL8lTcc3qqv4OYhv9m%2FEKUXDPg4xAueVz%2Bg%2FdtOMvLUnJMdeBkLhWMJs1pbgrwY0TJydq5b6FngSewTZ%2Fu33cvi1vseGHvHTkUAbPNfpyBfYqkkEJiKuXAQE0T07S1v8SAuJTplWmeR6zyaxQU0kjrrsM6AeOQZruE6Kzr2kjaR8NQ9cfkcI46nGgpJulABsQwhKKi1QY6pgGVLc6NQZFv%2FfJuH6hcY12EneUv66zXhUpRbmLIpGNgrPPwOdxtEjqLduW%2FoUQkJ2lqKzny7wv6VGMpI5HJPX713kjvSHv1zcRMJZkMljkb6yWtn0vLhhyFXhuQdtSGzEmEoQNIHsmN%2F%2FQWciWvCBQ%2BxtD0QZyAgyuvUX%2FKy1tTE42gPce4bVWemgcNrRW3cWnIdHFKRhkGf0zQE%2Bm41uNOQlap5z0j&X-Amz-Signature=3970012efe59f7e4c1bb59078842ea6026e51c7512d96c7e25cda2abc945efa6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZSWMHVC%2F20260915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260915T022518Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIEhmKUsLqf2eTqvldc9%2FbUtD9AC0KaT3eFw2NGL0KhXYAiAQEeNtXv7%2FTSpc1jrZIMlh8Mifdt7moDA2hhLWRQxFECqIBAjy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpWyPHyWeXhl2GLnFKtwDnGRCgOBrULzqGlJIsMSbJmohNENk7hXD9mNSVbrwOKpQm81YzXuhNUY7UCXjKpBFELkqI6Bh2I6sO%2FV4itugXqexNz%2BQfjK3ju%2FdHE7mBA6lGAj5fTvk43WPAYvH%2B%2BpB29SGoAUSXPEhSvdWa0oq%2FGTmLkIIY14fAo3Dg2t64%2BaSxLvPdDtYI5M6a0O1DaqhbZehWRROaVqpMsakrWyyJ%2B0ouM83OwXfQNMO7PMlxwQmnxh3puZIIveiajsVQycUxBXeduZElVMYanXqVcyjAv9hxCw7nyhrt4BB1xPBKjs7ZFbGTvsDiN2LfzbFiZnav%2FN8WNIeNECP%2FVxRF%2FWRtc6p6b%2BmLKidY%2BkUSc%2FYSCQ91RoTfxmsh23XsbQUyOkyOjBK%2Bx54JFgf1m%2Fq9%2BMXMYk%2FxMkWrwENUczgiCoNFunF6jH2UKQqBWuDjTQL8lTcc3qqv4OYhv9m%2FEKUXDPg4xAueVz%2Bg%2FdtOMvLUnJMdeBkLhWMJs1pbgrwY0TJydq5b6FngSewTZ%2Fu33cvi1vseGHvHTkUAbPNfpyBfYqkkEJiKuXAQE0T07S1v8SAuJTplWmeR6zyaxQU0kjrrsM6AeOQZruE6Kzr2kjaR8NQ9cfkcI46nGgpJulABsQwhKKi1QY6pgGVLc6NQZFv%2FfJuH6hcY12EneUv66zXhUpRbmLIpGNgrPPwOdxtEjqLduW%2FoUQkJ2lqKzny7wv6VGMpI5HJPX713kjvSHv1zcRMJZkMljkb6yWtn0vLhhyFXhuQdtSGzEmEoQNIHsmN%2F%2FQWciWvCBQ%2BxtD0QZyAgyuvUX%2FKy1tTE42gPce4bVWemgcNrRW3cWnIdHFKRhkGf0zQE%2Bm41uNOQlap5z0j&X-Amz-Signature=ec9497886c6d7528bb687a127b6bad17b381938c400cbd5ce72c0515029aec27&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZSWMHVC%2F20260915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260915T022518Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIEhmKUsLqf2eTqvldc9%2FbUtD9AC0KaT3eFw2NGL0KhXYAiAQEeNtXv7%2FTSpc1jrZIMlh8Mifdt7moDA2hhLWRQxFECqIBAjy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpWyPHyWeXhl2GLnFKtwDnGRCgOBrULzqGlJIsMSbJmohNENk7hXD9mNSVbrwOKpQm81YzXuhNUY7UCXjKpBFELkqI6Bh2I6sO%2FV4itugXqexNz%2BQfjK3ju%2FdHE7mBA6lGAj5fTvk43WPAYvH%2B%2BpB29SGoAUSXPEhSvdWa0oq%2FGTmLkIIY14fAo3Dg2t64%2BaSxLvPdDtYI5M6a0O1DaqhbZehWRROaVqpMsakrWyyJ%2B0ouM83OwXfQNMO7PMlxwQmnxh3puZIIveiajsVQycUxBXeduZElVMYanXqVcyjAv9hxCw7nyhrt4BB1xPBKjs7ZFbGTvsDiN2LfzbFiZnav%2FN8WNIeNECP%2FVxRF%2FWRtc6p6b%2BmLKidY%2BkUSc%2FYSCQ91RoTfxmsh23XsbQUyOkyOjBK%2Bx54JFgf1m%2Fq9%2BMXMYk%2FxMkWrwENUczgiCoNFunF6jH2UKQqBWuDjTQL8lTcc3qqv4OYhv9m%2FEKUXDPg4xAueVz%2Bg%2FdtOMvLUnJMdeBkLhWMJs1pbgrwY0TJydq5b6FngSewTZ%2Fu33cvi1vseGHvHTkUAbPNfpyBfYqkkEJiKuXAQE0T07S1v8SAuJTplWmeR6zyaxQU0kjrrsM6AeOQZruE6Kzr2kjaR8NQ9cfkcI46nGgpJulABsQwhKKi1QY6pgGVLc6NQZFv%2FfJuH6hcY12EneUv66zXhUpRbmLIpGNgrPPwOdxtEjqLduW%2FoUQkJ2lqKzny7wv6VGMpI5HJPX713kjvSHv1zcRMJZkMljkb6yWtn0vLhhyFXhuQdtSGzEmEoQNIHsmN%2F%2FQWciWvCBQ%2BxtD0QZyAgyuvUX%2FKy1tTE42gPce4bVWemgcNrRW3cWnIdHFKRhkGf0zQE%2Bm41uNOQlap5z0j&X-Amz-Signature=07e987ecb2d000148757e60eaa9798f7f2fa189771a232330ab82298b0aa967a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
