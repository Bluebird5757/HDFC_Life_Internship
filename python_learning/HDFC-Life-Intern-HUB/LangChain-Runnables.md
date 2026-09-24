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
fetched_at: '2026-09-24T02:11:04.057Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663NF3HP5B%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQDA4st0sIdt44iaZkanDzDY2U2ykXhnycoeiHFmXHMBqQIgequRnGd%2Bx5iOXbQ3xtLBoJSrpjNU2rpc7VfVZBEBA9wqiAQIyf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOQUPyc%2BWDZBcUyijircA14ORRieF8GnUYhR4sPZZVy37%2BjbEnjlg1C7NkwxAZzd0iAavn%2FwcrNjyTrf0XkAPEr524h7j%2F2cSvpkQonY65m0HkzdHnGnhcT%2BTyu5twmOIoL%2BH6F%2FLzR%2Bs0d5AG5HFPJ5fuKUm8rwyWmXX07dmy39ea9JHbCFxeMLr5AfyYcPSOe6GySmpwgtJN0qHAki3UziDvlRVb2fBqQxI6k3sAUwOvefC3EQwMDvwVwXGb4o7d3bUDe6It5wGZdxHIQ41FgKTcfFU1NK4bStKGvruRcsPOu3Q%2FkDX0Mj8X6UBn7Pjwzoe9z2TUg2TX4RtTLVLYGWMZ3lsWIxjlvlme%2F3AKts73XbTAyXlyeFOWP3SxvqfpYCAeT7T71oY5cO2Zurvwzrol7%2FJDKw0d0XGrOzPPWgWZIa%2FAyYdzZTreqYrtdG%2FuIuTC%2FP5lEpO6gwG44XoILzipsXw0o%2FaNQL7Dv%2Be3K5aD5XjVw7DFCEpEAM8L5njD0%2FNe1wLaOl%2FWmAEHALTZGwWbNA3Is9h4YU2Ko3R909vqwCOiW8jIypd7cNpbsYq1jeDaDsTqbPWG%2BHsRi3F0t%2BYFvBa%2FBVOJEzQ8u4urXgt6sQllThPhksx2JVV7N6fZlVbC2FTSWOtjLFMLLN0dUGOqUBCrCxS2DNGJLdHvhF3ShDR6WjWXtL%2FT%2Bl4hM8DkuQYobHakmkGInaWfw9arUhLuePMyi5ZAZ0MH0PiXJyxXQUjS0sCRXf%2BwZBLkn5ryQQ9HOgfbm2924WE5FHQWRyI5KC9W7sHltE4NpmaYaO7sixslzfSSqhLIjahOkF9acZn0yWA0Wnvt9NS%2BbO0bsZHLWhazn%2B45PX00Vn4b25Vlt62KPBDp%2FM&X-Amz-Signature=c6088f5097d9f70fcdee16d03b8a1a2ed6a72a2c702ca4374870a3bdb05d5d20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663NF3HP5B%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQDA4st0sIdt44iaZkanDzDY2U2ykXhnycoeiHFmXHMBqQIgequRnGd%2Bx5iOXbQ3xtLBoJSrpjNU2rpc7VfVZBEBA9wqiAQIyf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOQUPyc%2BWDZBcUyijircA14ORRieF8GnUYhR4sPZZVy37%2BjbEnjlg1C7NkwxAZzd0iAavn%2FwcrNjyTrf0XkAPEr524h7j%2F2cSvpkQonY65m0HkzdHnGnhcT%2BTyu5twmOIoL%2BH6F%2FLzR%2Bs0d5AG5HFPJ5fuKUm8rwyWmXX07dmy39ea9JHbCFxeMLr5AfyYcPSOe6GySmpwgtJN0qHAki3UziDvlRVb2fBqQxI6k3sAUwOvefC3EQwMDvwVwXGb4o7d3bUDe6It5wGZdxHIQ41FgKTcfFU1NK4bStKGvruRcsPOu3Q%2FkDX0Mj8X6UBn7Pjwzoe9z2TUg2TX4RtTLVLYGWMZ3lsWIxjlvlme%2F3AKts73XbTAyXlyeFOWP3SxvqfpYCAeT7T71oY5cO2Zurvwzrol7%2FJDKw0d0XGrOzPPWgWZIa%2FAyYdzZTreqYrtdG%2FuIuTC%2FP5lEpO6gwG44XoILzipsXw0o%2FaNQL7Dv%2Be3K5aD5XjVw7DFCEpEAM8L5njD0%2FNe1wLaOl%2FWmAEHALTZGwWbNA3Is9h4YU2Ko3R909vqwCOiW8jIypd7cNpbsYq1jeDaDsTqbPWG%2BHsRi3F0t%2BYFvBa%2FBVOJEzQ8u4urXgt6sQllThPhksx2JVV7N6fZlVbC2FTSWOtjLFMLLN0dUGOqUBCrCxS2DNGJLdHvhF3ShDR6WjWXtL%2FT%2Bl4hM8DkuQYobHakmkGInaWfw9arUhLuePMyi5ZAZ0MH0PiXJyxXQUjS0sCRXf%2BwZBLkn5ryQQ9HOgfbm2924WE5FHQWRyI5KC9W7sHltE4NpmaYaO7sixslzfSSqhLIjahOkF9acZn0yWA0Wnvt9NS%2BbO0bsZHLWhazn%2B45PX00Vn4b25Vlt62KPBDp%2FM&X-Amz-Signature=dfe2817746a0e9dcce50bbce67b2ab54a564bf2ba55c1a706fba10ff966ca9ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663NF3HP5B%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQDA4st0sIdt44iaZkanDzDY2U2ykXhnycoeiHFmXHMBqQIgequRnGd%2Bx5iOXbQ3xtLBoJSrpjNU2rpc7VfVZBEBA9wqiAQIyf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOQUPyc%2BWDZBcUyijircA14ORRieF8GnUYhR4sPZZVy37%2BjbEnjlg1C7NkwxAZzd0iAavn%2FwcrNjyTrf0XkAPEr524h7j%2F2cSvpkQonY65m0HkzdHnGnhcT%2BTyu5twmOIoL%2BH6F%2FLzR%2Bs0d5AG5HFPJ5fuKUm8rwyWmXX07dmy39ea9JHbCFxeMLr5AfyYcPSOe6GySmpwgtJN0qHAki3UziDvlRVb2fBqQxI6k3sAUwOvefC3EQwMDvwVwXGb4o7d3bUDe6It5wGZdxHIQ41FgKTcfFU1NK4bStKGvruRcsPOu3Q%2FkDX0Mj8X6UBn7Pjwzoe9z2TUg2TX4RtTLVLYGWMZ3lsWIxjlvlme%2F3AKts73XbTAyXlyeFOWP3SxvqfpYCAeT7T71oY5cO2Zurvwzrol7%2FJDKw0d0XGrOzPPWgWZIa%2FAyYdzZTreqYrtdG%2FuIuTC%2FP5lEpO6gwG44XoILzipsXw0o%2FaNQL7Dv%2Be3K5aD5XjVw7DFCEpEAM8L5njD0%2FNe1wLaOl%2FWmAEHALTZGwWbNA3Is9h4YU2Ko3R909vqwCOiW8jIypd7cNpbsYq1jeDaDsTqbPWG%2BHsRi3F0t%2BYFvBa%2FBVOJEzQ8u4urXgt6sQllThPhksx2JVV7N6fZlVbC2FTSWOtjLFMLLN0dUGOqUBCrCxS2DNGJLdHvhF3ShDR6WjWXtL%2FT%2Bl4hM8DkuQYobHakmkGInaWfw9arUhLuePMyi5ZAZ0MH0PiXJyxXQUjS0sCRXf%2BwZBLkn5ryQQ9HOgfbm2924WE5FHQWRyI5KC9W7sHltE4NpmaYaO7sixslzfSSqhLIjahOkF9acZn0yWA0Wnvt9NS%2BbO0bsZHLWhazn%2B45PX00Vn4b25Vlt62KPBDp%2FM&X-Amz-Signature=b283c0b725c827a9639b65778f6cef8f5421004764a6d9873dd16543ad8bf2e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663NF3HP5B%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQDA4st0sIdt44iaZkanDzDY2U2ykXhnycoeiHFmXHMBqQIgequRnGd%2Bx5iOXbQ3xtLBoJSrpjNU2rpc7VfVZBEBA9wqiAQIyf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOQUPyc%2BWDZBcUyijircA14ORRieF8GnUYhR4sPZZVy37%2BjbEnjlg1C7NkwxAZzd0iAavn%2FwcrNjyTrf0XkAPEr524h7j%2F2cSvpkQonY65m0HkzdHnGnhcT%2BTyu5twmOIoL%2BH6F%2FLzR%2Bs0d5AG5HFPJ5fuKUm8rwyWmXX07dmy39ea9JHbCFxeMLr5AfyYcPSOe6GySmpwgtJN0qHAki3UziDvlRVb2fBqQxI6k3sAUwOvefC3EQwMDvwVwXGb4o7d3bUDe6It5wGZdxHIQ41FgKTcfFU1NK4bStKGvruRcsPOu3Q%2FkDX0Mj8X6UBn7Pjwzoe9z2TUg2TX4RtTLVLYGWMZ3lsWIxjlvlme%2F3AKts73XbTAyXlyeFOWP3SxvqfpYCAeT7T71oY5cO2Zurvwzrol7%2FJDKw0d0XGrOzPPWgWZIa%2FAyYdzZTreqYrtdG%2FuIuTC%2FP5lEpO6gwG44XoILzipsXw0o%2FaNQL7Dv%2Be3K5aD5XjVw7DFCEpEAM8L5njD0%2FNe1wLaOl%2FWmAEHALTZGwWbNA3Is9h4YU2Ko3R909vqwCOiW8jIypd7cNpbsYq1jeDaDsTqbPWG%2BHsRi3F0t%2BYFvBa%2FBVOJEzQ8u4urXgt6sQllThPhksx2JVV7N6fZlVbC2FTSWOtjLFMLLN0dUGOqUBCrCxS2DNGJLdHvhF3ShDR6WjWXtL%2FT%2Bl4hM8DkuQYobHakmkGInaWfw9arUhLuePMyi5ZAZ0MH0PiXJyxXQUjS0sCRXf%2BwZBLkn5ryQQ9HOgfbm2924WE5FHQWRyI5KC9W7sHltE4NpmaYaO7sixslzfSSqhLIjahOkF9acZn0yWA0Wnvt9NS%2BbO0bsZHLWhazn%2B45PX00Vn4b25Vlt62KPBDp%2FM&X-Amz-Signature=ef33870766637dcffe5c059bbbf8125cb03b3c9daacbfd7c4c8d01ffcb3580aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663NF3HP5B%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQDA4st0sIdt44iaZkanDzDY2U2ykXhnycoeiHFmXHMBqQIgequRnGd%2Bx5iOXbQ3xtLBoJSrpjNU2rpc7VfVZBEBA9wqiAQIyf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOQUPyc%2BWDZBcUyijircA14ORRieF8GnUYhR4sPZZVy37%2BjbEnjlg1C7NkwxAZzd0iAavn%2FwcrNjyTrf0XkAPEr524h7j%2F2cSvpkQonY65m0HkzdHnGnhcT%2BTyu5twmOIoL%2BH6F%2FLzR%2Bs0d5AG5HFPJ5fuKUm8rwyWmXX07dmy39ea9JHbCFxeMLr5AfyYcPSOe6GySmpwgtJN0qHAki3UziDvlRVb2fBqQxI6k3sAUwOvefC3EQwMDvwVwXGb4o7d3bUDe6It5wGZdxHIQ41FgKTcfFU1NK4bStKGvruRcsPOu3Q%2FkDX0Mj8X6UBn7Pjwzoe9z2TUg2TX4RtTLVLYGWMZ3lsWIxjlvlme%2F3AKts73XbTAyXlyeFOWP3SxvqfpYCAeT7T71oY5cO2Zurvwzrol7%2FJDKw0d0XGrOzPPWgWZIa%2FAyYdzZTreqYrtdG%2FuIuTC%2FP5lEpO6gwG44XoILzipsXw0o%2FaNQL7Dv%2Be3K5aD5XjVw7DFCEpEAM8L5njD0%2FNe1wLaOl%2FWmAEHALTZGwWbNA3Is9h4YU2Ko3R909vqwCOiW8jIypd7cNpbsYq1jeDaDsTqbPWG%2BHsRi3F0t%2BYFvBa%2FBVOJEzQ8u4urXgt6sQllThPhksx2JVV7N6fZlVbC2FTSWOtjLFMLLN0dUGOqUBCrCxS2DNGJLdHvhF3ShDR6WjWXtL%2FT%2Bl4hM8DkuQYobHakmkGInaWfw9arUhLuePMyi5ZAZ0MH0PiXJyxXQUjS0sCRXf%2BwZBLkn5ryQQ9HOgfbm2924WE5FHQWRyI5KC9W7sHltE4NpmaYaO7sixslzfSSqhLIjahOkF9acZn0yWA0Wnvt9NS%2BbO0bsZHLWhazn%2B45PX00Vn4b25Vlt62KPBDp%2FM&X-Amz-Signature=671dddf93605ba6e23f412af858946d7852fc90cc22b4b906f9e708d3e9fd6d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663NF3HP5B%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQDA4st0sIdt44iaZkanDzDY2U2ykXhnycoeiHFmXHMBqQIgequRnGd%2Bx5iOXbQ3xtLBoJSrpjNU2rpc7VfVZBEBA9wqiAQIyf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOQUPyc%2BWDZBcUyijircA14ORRieF8GnUYhR4sPZZVy37%2BjbEnjlg1C7NkwxAZzd0iAavn%2FwcrNjyTrf0XkAPEr524h7j%2F2cSvpkQonY65m0HkzdHnGnhcT%2BTyu5twmOIoL%2BH6F%2FLzR%2Bs0d5AG5HFPJ5fuKUm8rwyWmXX07dmy39ea9JHbCFxeMLr5AfyYcPSOe6GySmpwgtJN0qHAki3UziDvlRVb2fBqQxI6k3sAUwOvefC3EQwMDvwVwXGb4o7d3bUDe6It5wGZdxHIQ41FgKTcfFU1NK4bStKGvruRcsPOu3Q%2FkDX0Mj8X6UBn7Pjwzoe9z2TUg2TX4RtTLVLYGWMZ3lsWIxjlvlme%2F3AKts73XbTAyXlyeFOWP3SxvqfpYCAeT7T71oY5cO2Zurvwzrol7%2FJDKw0d0XGrOzPPWgWZIa%2FAyYdzZTreqYrtdG%2FuIuTC%2FP5lEpO6gwG44XoILzipsXw0o%2FaNQL7Dv%2Be3K5aD5XjVw7DFCEpEAM8L5njD0%2FNe1wLaOl%2FWmAEHALTZGwWbNA3Is9h4YU2Ko3R909vqwCOiW8jIypd7cNpbsYq1jeDaDsTqbPWG%2BHsRi3F0t%2BYFvBa%2FBVOJEzQ8u4urXgt6sQllThPhksx2JVV7N6fZlVbC2FTSWOtjLFMLLN0dUGOqUBCrCxS2DNGJLdHvhF3ShDR6WjWXtL%2FT%2Bl4hM8DkuQYobHakmkGInaWfw9arUhLuePMyi5ZAZ0MH0PiXJyxXQUjS0sCRXf%2BwZBLkn5ryQQ9HOgfbm2924WE5FHQWRyI5KC9W7sHltE4NpmaYaO7sixslzfSSqhLIjahOkF9acZn0yWA0Wnvt9NS%2BbO0bsZHLWhazn%2B45PX00Vn4b25Vlt62KPBDp%2FM&X-Amz-Signature=8742c23941a8136c432a8d075a6c91fc70d8b48c13456ec01528ad12ff81d7a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663NF3HP5B%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQDA4st0sIdt44iaZkanDzDY2U2ykXhnycoeiHFmXHMBqQIgequRnGd%2Bx5iOXbQ3xtLBoJSrpjNU2rpc7VfVZBEBA9wqiAQIyf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOQUPyc%2BWDZBcUyijircA14ORRieF8GnUYhR4sPZZVy37%2BjbEnjlg1C7NkwxAZzd0iAavn%2FwcrNjyTrf0XkAPEr524h7j%2F2cSvpkQonY65m0HkzdHnGnhcT%2BTyu5twmOIoL%2BH6F%2FLzR%2Bs0d5AG5HFPJ5fuKUm8rwyWmXX07dmy39ea9JHbCFxeMLr5AfyYcPSOe6GySmpwgtJN0qHAki3UziDvlRVb2fBqQxI6k3sAUwOvefC3EQwMDvwVwXGb4o7d3bUDe6It5wGZdxHIQ41FgKTcfFU1NK4bStKGvruRcsPOu3Q%2FkDX0Mj8X6UBn7Pjwzoe9z2TUg2TX4RtTLVLYGWMZ3lsWIxjlvlme%2F3AKts73XbTAyXlyeFOWP3SxvqfpYCAeT7T71oY5cO2Zurvwzrol7%2FJDKw0d0XGrOzPPWgWZIa%2FAyYdzZTreqYrtdG%2FuIuTC%2FP5lEpO6gwG44XoILzipsXw0o%2FaNQL7Dv%2Be3K5aD5XjVw7DFCEpEAM8L5njD0%2FNe1wLaOl%2FWmAEHALTZGwWbNA3Is9h4YU2Ko3R909vqwCOiW8jIypd7cNpbsYq1jeDaDsTqbPWG%2BHsRi3F0t%2BYFvBa%2FBVOJEzQ8u4urXgt6sQllThPhksx2JVV7N6fZlVbC2FTSWOtjLFMLLN0dUGOqUBCrCxS2DNGJLdHvhF3ShDR6WjWXtL%2FT%2Bl4hM8DkuQYobHakmkGInaWfw9arUhLuePMyi5ZAZ0MH0PiXJyxXQUjS0sCRXf%2BwZBLkn5ryQQ9HOgfbm2924WE5FHQWRyI5KC9W7sHltE4NpmaYaO7sixslzfSSqhLIjahOkF9acZn0yWA0Wnvt9NS%2BbO0bsZHLWhazn%2B45PX00Vn4b25Vlt62KPBDp%2FM&X-Amz-Signature=58b28c36c7f2a120e981e96139f370eb895e29fa56fd8c6ec97f4a4d4fbc5008&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663NF3HP5B%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQDA4st0sIdt44iaZkanDzDY2U2ykXhnycoeiHFmXHMBqQIgequRnGd%2Bx5iOXbQ3xtLBoJSrpjNU2rpc7VfVZBEBA9wqiAQIyf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOQUPyc%2BWDZBcUyijircA14ORRieF8GnUYhR4sPZZVy37%2BjbEnjlg1C7NkwxAZzd0iAavn%2FwcrNjyTrf0XkAPEr524h7j%2F2cSvpkQonY65m0HkzdHnGnhcT%2BTyu5twmOIoL%2BH6F%2FLzR%2Bs0d5AG5HFPJ5fuKUm8rwyWmXX07dmy39ea9JHbCFxeMLr5AfyYcPSOe6GySmpwgtJN0qHAki3UziDvlRVb2fBqQxI6k3sAUwOvefC3EQwMDvwVwXGb4o7d3bUDe6It5wGZdxHIQ41FgKTcfFU1NK4bStKGvruRcsPOu3Q%2FkDX0Mj8X6UBn7Pjwzoe9z2TUg2TX4RtTLVLYGWMZ3lsWIxjlvlme%2F3AKts73XbTAyXlyeFOWP3SxvqfpYCAeT7T71oY5cO2Zurvwzrol7%2FJDKw0d0XGrOzPPWgWZIa%2FAyYdzZTreqYrtdG%2FuIuTC%2FP5lEpO6gwG44XoILzipsXw0o%2FaNQL7Dv%2Be3K5aD5XjVw7DFCEpEAM8L5njD0%2FNe1wLaOl%2FWmAEHALTZGwWbNA3Is9h4YU2Ko3R909vqwCOiW8jIypd7cNpbsYq1jeDaDsTqbPWG%2BHsRi3F0t%2BYFvBa%2FBVOJEzQ8u4urXgt6sQllThPhksx2JVV7N6fZlVbC2FTSWOtjLFMLLN0dUGOqUBCrCxS2DNGJLdHvhF3ShDR6WjWXtL%2FT%2Bl4hM8DkuQYobHakmkGInaWfw9arUhLuePMyi5ZAZ0MH0PiXJyxXQUjS0sCRXf%2BwZBLkn5ryQQ9HOgfbm2924WE5FHQWRyI5KC9W7sHltE4NpmaYaO7sixslzfSSqhLIjahOkF9acZn0yWA0Wnvt9NS%2BbO0bsZHLWhazn%2B45PX00Vn4b25Vlt62KPBDp%2FM&X-Amz-Signature=5834b959cdbda829c2c0a4d0135890bc7828769004f67474a05cc7c3b36d822f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663NF3HP5B%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQDA4st0sIdt44iaZkanDzDY2U2ykXhnycoeiHFmXHMBqQIgequRnGd%2Bx5iOXbQ3xtLBoJSrpjNU2rpc7VfVZBEBA9wqiAQIyf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOQUPyc%2BWDZBcUyijircA14ORRieF8GnUYhR4sPZZVy37%2BjbEnjlg1C7NkwxAZzd0iAavn%2FwcrNjyTrf0XkAPEr524h7j%2F2cSvpkQonY65m0HkzdHnGnhcT%2BTyu5twmOIoL%2BH6F%2FLzR%2Bs0d5AG5HFPJ5fuKUm8rwyWmXX07dmy39ea9JHbCFxeMLr5AfyYcPSOe6GySmpwgtJN0qHAki3UziDvlRVb2fBqQxI6k3sAUwOvefC3EQwMDvwVwXGb4o7d3bUDe6It5wGZdxHIQ41FgKTcfFU1NK4bStKGvruRcsPOu3Q%2FkDX0Mj8X6UBn7Pjwzoe9z2TUg2TX4RtTLVLYGWMZ3lsWIxjlvlme%2F3AKts73XbTAyXlyeFOWP3SxvqfpYCAeT7T71oY5cO2Zurvwzrol7%2FJDKw0d0XGrOzPPWgWZIa%2FAyYdzZTreqYrtdG%2FuIuTC%2FP5lEpO6gwG44XoILzipsXw0o%2FaNQL7Dv%2Be3K5aD5XjVw7DFCEpEAM8L5njD0%2FNe1wLaOl%2FWmAEHALTZGwWbNA3Is9h4YU2Ko3R909vqwCOiW8jIypd7cNpbsYq1jeDaDsTqbPWG%2BHsRi3F0t%2BYFvBa%2FBVOJEzQ8u4urXgt6sQllThPhksx2JVV7N6fZlVbC2FTSWOtjLFMLLN0dUGOqUBCrCxS2DNGJLdHvhF3ShDR6WjWXtL%2FT%2Bl4hM8DkuQYobHakmkGInaWfw9arUhLuePMyi5ZAZ0MH0PiXJyxXQUjS0sCRXf%2BwZBLkn5ryQQ9HOgfbm2924WE5FHQWRyI5KC9W7sHltE4NpmaYaO7sixslzfSSqhLIjahOkF9acZn0yWA0Wnvt9NS%2BbO0bsZHLWhazn%2B45PX00Vn4b25Vlt62KPBDp%2FM&X-Amz-Signature=101fa54da2fe5bfdcc252d8c3376d6880b5d7ab764187d35c5b29aa20824062d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
