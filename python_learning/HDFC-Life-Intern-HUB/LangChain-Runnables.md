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
fetched_at: '2026-10-18T02:05:01.839Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQWEV5NU%2F20261018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261018T020456Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQDb2mW2bShwA8mmDGoIDr4dlHBuNI7%2Bq4nx8VjDFBtHwQIgcGCUZ4K48thtISG9kLdFJklCfmzgV9LAQY0bA9k5xp8q%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDPgIzDaVpeeHLABnuSrcA%2F1HQje%2FCFshONtGg20kpMaSkli%2B%2F9%2F00EL1rtmWJLlROtZ3ZOSBfosnciXBo7p%2BxhF%2BPwRdKGyy5umThlCP2YtCtoLGGuhMniiRVDZBF3TaePqID2e7r7wf%2Fv2cK1NwKZ%2FtldWTnTVu%2FXMa%2BYWP7UXLg%2Fg2FfvhOJcta0KXXgFH24DdnWAeyW14ESzieLgcsXnblTuo8tdt7NbFK0TknJZytOVAf3qnxZVabAShZz2QsZsNV1zz8AeYJZ%2FzGOV8V8xQmH4BTtl0KTR1%2FmE4CuRSEhU%2Fio1VhxYuVk1ma8wfrxujD0wC2%2Be74MkHJLhlJqAhzS32iUZN70uC6t0wffLg5%2FaaiMezFxvFsZDIinDnQNBeSBQKxTAWodGuUrJ7izfnM9cgh%2FVNPUkAakCLQZCrdrW498zahjQvn8qCnQq2%2BU%2BKXoLsFoFZnMXbjK3nisgfLl3Mwe8NU1aUwLcOlKMsP8PTcq4%2F1BSAH9zPrc5Psy2gVPlYuGQIY%2Bm5T6vZ8bbTXYdfd6yB36t3iQkXrj6409oL1HtTskAQp2NLw%2FY1004ARv9alI2V1JeLcQ6P9hq9MVtzluzum34uWY6%2BsR8d%2FB1g7xl1ZuxQKpo%2BGiEeyXvpt63mIz6Vr9B9MKah0NYGOqUBDBddRzxvQ%2F%2F%2FeMBW9SvaCTOL7nb9jX%2BNlj69q6UOpbgqacZCgAghi1tyIjeXRi71gpgAhe19hF55eYEq5DSGfnVTZV1uJOHAZ3UY5QLJNZJoemJnrjV%2B%2Flccx%2FV5FMpUshm6r4Sl6hJWppBP%2FYX9P8I8pXoUP9tpeW7lLPvl%2BIRfrcNErrG%2B9qtCJMml6ycZbH0C28uCc9QdQaflJbpQd9zet6Ld&X-Amz-Signature=f3c1f8e524d88bcf6dcaff0a102a02747a578e59b4efe5c10d8a4fc656b5235a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQWEV5NU%2F20261018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261018T020456Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQDb2mW2bShwA8mmDGoIDr4dlHBuNI7%2Bq4nx8VjDFBtHwQIgcGCUZ4K48thtISG9kLdFJklCfmzgV9LAQY0bA9k5xp8q%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDPgIzDaVpeeHLABnuSrcA%2F1HQje%2FCFshONtGg20kpMaSkli%2B%2F9%2F00EL1rtmWJLlROtZ3ZOSBfosnciXBo7p%2BxhF%2BPwRdKGyy5umThlCP2YtCtoLGGuhMniiRVDZBF3TaePqID2e7r7wf%2Fv2cK1NwKZ%2FtldWTnTVu%2FXMa%2BYWP7UXLg%2Fg2FfvhOJcta0KXXgFH24DdnWAeyW14ESzieLgcsXnblTuo8tdt7NbFK0TknJZytOVAf3qnxZVabAShZz2QsZsNV1zz8AeYJZ%2FzGOV8V8xQmH4BTtl0KTR1%2FmE4CuRSEhU%2Fio1VhxYuVk1ma8wfrxujD0wC2%2Be74MkHJLhlJqAhzS32iUZN70uC6t0wffLg5%2FaaiMezFxvFsZDIinDnQNBeSBQKxTAWodGuUrJ7izfnM9cgh%2FVNPUkAakCLQZCrdrW498zahjQvn8qCnQq2%2BU%2BKXoLsFoFZnMXbjK3nisgfLl3Mwe8NU1aUwLcOlKMsP8PTcq4%2F1BSAH9zPrc5Psy2gVPlYuGQIY%2Bm5T6vZ8bbTXYdfd6yB36t3iQkXrj6409oL1HtTskAQp2NLw%2FY1004ARv9alI2V1JeLcQ6P9hq9MVtzluzum34uWY6%2BsR8d%2FB1g7xl1ZuxQKpo%2BGiEeyXvpt63mIz6Vr9B9MKah0NYGOqUBDBddRzxvQ%2F%2F%2FeMBW9SvaCTOL7nb9jX%2BNlj69q6UOpbgqacZCgAghi1tyIjeXRi71gpgAhe19hF55eYEq5DSGfnVTZV1uJOHAZ3UY5QLJNZJoemJnrjV%2B%2Flccx%2FV5FMpUshm6r4Sl6hJWppBP%2FYX9P8I8pXoUP9tpeW7lLPvl%2BIRfrcNErrG%2B9qtCJMml6ycZbH0C28uCc9QdQaflJbpQd9zet6Ld&X-Amz-Signature=4767d7c04aa732a4196a6bf3e7b63fa07403d78f82463b5ccfdff973b9eccbd5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQWEV5NU%2F20261018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261018T020456Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQDb2mW2bShwA8mmDGoIDr4dlHBuNI7%2Bq4nx8VjDFBtHwQIgcGCUZ4K48thtISG9kLdFJklCfmzgV9LAQY0bA9k5xp8q%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDPgIzDaVpeeHLABnuSrcA%2F1HQje%2FCFshONtGg20kpMaSkli%2B%2F9%2F00EL1rtmWJLlROtZ3ZOSBfosnciXBo7p%2BxhF%2BPwRdKGyy5umThlCP2YtCtoLGGuhMniiRVDZBF3TaePqID2e7r7wf%2Fv2cK1NwKZ%2FtldWTnTVu%2FXMa%2BYWP7UXLg%2Fg2FfvhOJcta0KXXgFH24DdnWAeyW14ESzieLgcsXnblTuo8tdt7NbFK0TknJZytOVAf3qnxZVabAShZz2QsZsNV1zz8AeYJZ%2FzGOV8V8xQmH4BTtl0KTR1%2FmE4CuRSEhU%2Fio1VhxYuVk1ma8wfrxujD0wC2%2Be74MkHJLhlJqAhzS32iUZN70uC6t0wffLg5%2FaaiMezFxvFsZDIinDnQNBeSBQKxTAWodGuUrJ7izfnM9cgh%2FVNPUkAakCLQZCrdrW498zahjQvn8qCnQq2%2BU%2BKXoLsFoFZnMXbjK3nisgfLl3Mwe8NU1aUwLcOlKMsP8PTcq4%2F1BSAH9zPrc5Psy2gVPlYuGQIY%2Bm5T6vZ8bbTXYdfd6yB36t3iQkXrj6409oL1HtTskAQp2NLw%2FY1004ARv9alI2V1JeLcQ6P9hq9MVtzluzum34uWY6%2BsR8d%2FB1g7xl1ZuxQKpo%2BGiEeyXvpt63mIz6Vr9B9MKah0NYGOqUBDBddRzxvQ%2F%2F%2FeMBW9SvaCTOL7nb9jX%2BNlj69q6UOpbgqacZCgAghi1tyIjeXRi71gpgAhe19hF55eYEq5DSGfnVTZV1uJOHAZ3UY5QLJNZJoemJnrjV%2B%2Flccx%2FV5FMpUshm6r4Sl6hJWppBP%2FYX9P8I8pXoUP9tpeW7lLPvl%2BIRfrcNErrG%2B9qtCJMml6ycZbH0C28uCc9QdQaflJbpQd9zet6Ld&X-Amz-Signature=3473b5291bc09024150419832c087f02d623850333c6e25bbadf4bf7f071a060&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQWEV5NU%2F20261018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261018T020456Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQDb2mW2bShwA8mmDGoIDr4dlHBuNI7%2Bq4nx8VjDFBtHwQIgcGCUZ4K48thtISG9kLdFJklCfmzgV9LAQY0bA9k5xp8q%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDPgIzDaVpeeHLABnuSrcA%2F1HQje%2FCFshONtGg20kpMaSkli%2B%2F9%2F00EL1rtmWJLlROtZ3ZOSBfosnciXBo7p%2BxhF%2BPwRdKGyy5umThlCP2YtCtoLGGuhMniiRVDZBF3TaePqID2e7r7wf%2Fv2cK1NwKZ%2FtldWTnTVu%2FXMa%2BYWP7UXLg%2Fg2FfvhOJcta0KXXgFH24DdnWAeyW14ESzieLgcsXnblTuo8tdt7NbFK0TknJZytOVAf3qnxZVabAShZz2QsZsNV1zz8AeYJZ%2FzGOV8V8xQmH4BTtl0KTR1%2FmE4CuRSEhU%2Fio1VhxYuVk1ma8wfrxujD0wC2%2Be74MkHJLhlJqAhzS32iUZN70uC6t0wffLg5%2FaaiMezFxvFsZDIinDnQNBeSBQKxTAWodGuUrJ7izfnM9cgh%2FVNPUkAakCLQZCrdrW498zahjQvn8qCnQq2%2BU%2BKXoLsFoFZnMXbjK3nisgfLl3Mwe8NU1aUwLcOlKMsP8PTcq4%2F1BSAH9zPrc5Psy2gVPlYuGQIY%2Bm5T6vZ8bbTXYdfd6yB36t3iQkXrj6409oL1HtTskAQp2NLw%2FY1004ARv9alI2V1JeLcQ6P9hq9MVtzluzum34uWY6%2BsR8d%2FB1g7xl1ZuxQKpo%2BGiEeyXvpt63mIz6Vr9B9MKah0NYGOqUBDBddRzxvQ%2F%2F%2FeMBW9SvaCTOL7nb9jX%2BNlj69q6UOpbgqacZCgAghi1tyIjeXRi71gpgAhe19hF55eYEq5DSGfnVTZV1uJOHAZ3UY5QLJNZJoemJnrjV%2B%2Flccx%2FV5FMpUshm6r4Sl6hJWppBP%2FYX9P8I8pXoUP9tpeW7lLPvl%2BIRfrcNErrG%2B9qtCJMml6ycZbH0C28uCc9QdQaflJbpQd9zet6Ld&X-Amz-Signature=a3059becead0e731c1d59ee3ca187a30442acff15b6914476983259b26c3fb3c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQWEV5NU%2F20261018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261018T020456Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQDb2mW2bShwA8mmDGoIDr4dlHBuNI7%2Bq4nx8VjDFBtHwQIgcGCUZ4K48thtISG9kLdFJklCfmzgV9LAQY0bA9k5xp8q%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDPgIzDaVpeeHLABnuSrcA%2F1HQje%2FCFshONtGg20kpMaSkli%2B%2F9%2F00EL1rtmWJLlROtZ3ZOSBfosnciXBo7p%2BxhF%2BPwRdKGyy5umThlCP2YtCtoLGGuhMniiRVDZBF3TaePqID2e7r7wf%2Fv2cK1NwKZ%2FtldWTnTVu%2FXMa%2BYWP7UXLg%2Fg2FfvhOJcta0KXXgFH24DdnWAeyW14ESzieLgcsXnblTuo8tdt7NbFK0TknJZytOVAf3qnxZVabAShZz2QsZsNV1zz8AeYJZ%2FzGOV8V8xQmH4BTtl0KTR1%2FmE4CuRSEhU%2Fio1VhxYuVk1ma8wfrxujD0wC2%2Be74MkHJLhlJqAhzS32iUZN70uC6t0wffLg5%2FaaiMezFxvFsZDIinDnQNBeSBQKxTAWodGuUrJ7izfnM9cgh%2FVNPUkAakCLQZCrdrW498zahjQvn8qCnQq2%2BU%2BKXoLsFoFZnMXbjK3nisgfLl3Mwe8NU1aUwLcOlKMsP8PTcq4%2F1BSAH9zPrc5Psy2gVPlYuGQIY%2Bm5T6vZ8bbTXYdfd6yB36t3iQkXrj6409oL1HtTskAQp2NLw%2FY1004ARv9alI2V1JeLcQ6P9hq9MVtzluzum34uWY6%2BsR8d%2FB1g7xl1ZuxQKpo%2BGiEeyXvpt63mIz6Vr9B9MKah0NYGOqUBDBddRzxvQ%2F%2F%2FeMBW9SvaCTOL7nb9jX%2BNlj69q6UOpbgqacZCgAghi1tyIjeXRi71gpgAhe19hF55eYEq5DSGfnVTZV1uJOHAZ3UY5QLJNZJoemJnrjV%2B%2Flccx%2FV5FMpUshm6r4Sl6hJWppBP%2FYX9P8I8pXoUP9tpeW7lLPvl%2BIRfrcNErrG%2B9qtCJMml6ycZbH0C28uCc9QdQaflJbpQd9zet6Ld&X-Amz-Signature=b53f1591b42e97cf67cabcdb898324275ad11fb5013e64839aae6904ec2916a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQWEV5NU%2F20261018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261018T020456Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQDb2mW2bShwA8mmDGoIDr4dlHBuNI7%2Bq4nx8VjDFBtHwQIgcGCUZ4K48thtISG9kLdFJklCfmzgV9LAQY0bA9k5xp8q%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDPgIzDaVpeeHLABnuSrcA%2F1HQje%2FCFshONtGg20kpMaSkli%2B%2F9%2F00EL1rtmWJLlROtZ3ZOSBfosnciXBo7p%2BxhF%2BPwRdKGyy5umThlCP2YtCtoLGGuhMniiRVDZBF3TaePqID2e7r7wf%2Fv2cK1NwKZ%2FtldWTnTVu%2FXMa%2BYWP7UXLg%2Fg2FfvhOJcta0KXXgFH24DdnWAeyW14ESzieLgcsXnblTuo8tdt7NbFK0TknJZytOVAf3qnxZVabAShZz2QsZsNV1zz8AeYJZ%2FzGOV8V8xQmH4BTtl0KTR1%2FmE4CuRSEhU%2Fio1VhxYuVk1ma8wfrxujD0wC2%2Be74MkHJLhlJqAhzS32iUZN70uC6t0wffLg5%2FaaiMezFxvFsZDIinDnQNBeSBQKxTAWodGuUrJ7izfnM9cgh%2FVNPUkAakCLQZCrdrW498zahjQvn8qCnQq2%2BU%2BKXoLsFoFZnMXbjK3nisgfLl3Mwe8NU1aUwLcOlKMsP8PTcq4%2F1BSAH9zPrc5Psy2gVPlYuGQIY%2Bm5T6vZ8bbTXYdfd6yB36t3iQkXrj6409oL1HtTskAQp2NLw%2FY1004ARv9alI2V1JeLcQ6P9hq9MVtzluzum34uWY6%2BsR8d%2FB1g7xl1ZuxQKpo%2BGiEeyXvpt63mIz6Vr9B9MKah0NYGOqUBDBddRzxvQ%2F%2F%2FeMBW9SvaCTOL7nb9jX%2BNlj69q6UOpbgqacZCgAghi1tyIjeXRi71gpgAhe19hF55eYEq5DSGfnVTZV1uJOHAZ3UY5QLJNZJoemJnrjV%2B%2Flccx%2FV5FMpUshm6r4Sl6hJWppBP%2FYX9P8I8pXoUP9tpeW7lLPvl%2BIRfrcNErrG%2B9qtCJMml6ycZbH0C28uCc9QdQaflJbpQd9zet6Ld&X-Amz-Signature=4780723b276687b1bbd6daea790d42b53e10a748026fb18aae07bc90efa84c8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQWEV5NU%2F20261018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261018T020456Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQDb2mW2bShwA8mmDGoIDr4dlHBuNI7%2Bq4nx8VjDFBtHwQIgcGCUZ4K48thtISG9kLdFJklCfmzgV9LAQY0bA9k5xp8q%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDPgIzDaVpeeHLABnuSrcA%2F1HQje%2FCFshONtGg20kpMaSkli%2B%2F9%2F00EL1rtmWJLlROtZ3ZOSBfosnciXBo7p%2BxhF%2BPwRdKGyy5umThlCP2YtCtoLGGuhMniiRVDZBF3TaePqID2e7r7wf%2Fv2cK1NwKZ%2FtldWTnTVu%2FXMa%2BYWP7UXLg%2Fg2FfvhOJcta0KXXgFH24DdnWAeyW14ESzieLgcsXnblTuo8tdt7NbFK0TknJZytOVAf3qnxZVabAShZz2QsZsNV1zz8AeYJZ%2FzGOV8V8xQmH4BTtl0KTR1%2FmE4CuRSEhU%2Fio1VhxYuVk1ma8wfrxujD0wC2%2Be74MkHJLhlJqAhzS32iUZN70uC6t0wffLg5%2FaaiMezFxvFsZDIinDnQNBeSBQKxTAWodGuUrJ7izfnM9cgh%2FVNPUkAakCLQZCrdrW498zahjQvn8qCnQq2%2BU%2BKXoLsFoFZnMXbjK3nisgfLl3Mwe8NU1aUwLcOlKMsP8PTcq4%2F1BSAH9zPrc5Psy2gVPlYuGQIY%2Bm5T6vZ8bbTXYdfd6yB36t3iQkXrj6409oL1HtTskAQp2NLw%2FY1004ARv9alI2V1JeLcQ6P9hq9MVtzluzum34uWY6%2BsR8d%2FB1g7xl1ZuxQKpo%2BGiEeyXvpt63mIz6Vr9B9MKah0NYGOqUBDBddRzxvQ%2F%2F%2FeMBW9SvaCTOL7nb9jX%2BNlj69q6UOpbgqacZCgAghi1tyIjeXRi71gpgAhe19hF55eYEq5DSGfnVTZV1uJOHAZ3UY5QLJNZJoemJnrjV%2B%2Flccx%2FV5FMpUshm6r4Sl6hJWppBP%2FYX9P8I8pXoUP9tpeW7lLPvl%2BIRfrcNErrG%2B9qtCJMml6ycZbH0C28uCc9QdQaflJbpQd9zet6Ld&X-Amz-Signature=16e44d72e07237f0fd30820cfe97b4246b0a3aa93795d4cd6f46b0fc5421e0f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQWEV5NU%2F20261018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261018T020456Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQDb2mW2bShwA8mmDGoIDr4dlHBuNI7%2Bq4nx8VjDFBtHwQIgcGCUZ4K48thtISG9kLdFJklCfmzgV9LAQY0bA9k5xp8q%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDPgIzDaVpeeHLABnuSrcA%2F1HQje%2FCFshONtGg20kpMaSkli%2B%2F9%2F00EL1rtmWJLlROtZ3ZOSBfosnciXBo7p%2BxhF%2BPwRdKGyy5umThlCP2YtCtoLGGuhMniiRVDZBF3TaePqID2e7r7wf%2Fv2cK1NwKZ%2FtldWTnTVu%2FXMa%2BYWP7UXLg%2Fg2FfvhOJcta0KXXgFH24DdnWAeyW14ESzieLgcsXnblTuo8tdt7NbFK0TknJZytOVAf3qnxZVabAShZz2QsZsNV1zz8AeYJZ%2FzGOV8V8xQmH4BTtl0KTR1%2FmE4CuRSEhU%2Fio1VhxYuVk1ma8wfrxujD0wC2%2Be74MkHJLhlJqAhzS32iUZN70uC6t0wffLg5%2FaaiMezFxvFsZDIinDnQNBeSBQKxTAWodGuUrJ7izfnM9cgh%2FVNPUkAakCLQZCrdrW498zahjQvn8qCnQq2%2BU%2BKXoLsFoFZnMXbjK3nisgfLl3Mwe8NU1aUwLcOlKMsP8PTcq4%2F1BSAH9zPrc5Psy2gVPlYuGQIY%2Bm5T6vZ8bbTXYdfd6yB36t3iQkXrj6409oL1HtTskAQp2NLw%2FY1004ARv9alI2V1JeLcQ6P9hq9MVtzluzum34uWY6%2BsR8d%2FB1g7xl1ZuxQKpo%2BGiEeyXvpt63mIz6Vr9B9MKah0NYGOqUBDBddRzxvQ%2F%2F%2FeMBW9SvaCTOL7nb9jX%2BNlj69q6UOpbgqacZCgAghi1tyIjeXRi71gpgAhe19hF55eYEq5DSGfnVTZV1uJOHAZ3UY5QLJNZJoemJnrjV%2B%2Flccx%2FV5FMpUshm6r4Sl6hJWppBP%2FYX9P8I8pXoUP9tpeW7lLPvl%2BIRfrcNErrG%2B9qtCJMml6ycZbH0C28uCc9QdQaflJbpQd9zet6Ld&X-Amz-Signature=252145458531b8d5f799e58ff5cb343a47a85d449d7352a62f67f66feb52e9a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQWEV5NU%2F20261018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261018T020456Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQDb2mW2bShwA8mmDGoIDr4dlHBuNI7%2Bq4nx8VjDFBtHwQIgcGCUZ4K48thtISG9kLdFJklCfmzgV9LAQY0bA9k5xp8q%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDPgIzDaVpeeHLABnuSrcA%2F1HQje%2FCFshONtGg20kpMaSkli%2B%2F9%2F00EL1rtmWJLlROtZ3ZOSBfosnciXBo7p%2BxhF%2BPwRdKGyy5umThlCP2YtCtoLGGuhMniiRVDZBF3TaePqID2e7r7wf%2Fv2cK1NwKZ%2FtldWTnTVu%2FXMa%2BYWP7UXLg%2Fg2FfvhOJcta0KXXgFH24DdnWAeyW14ESzieLgcsXnblTuo8tdt7NbFK0TknJZytOVAf3qnxZVabAShZz2QsZsNV1zz8AeYJZ%2FzGOV8V8xQmH4BTtl0KTR1%2FmE4CuRSEhU%2Fio1VhxYuVk1ma8wfrxujD0wC2%2Be74MkHJLhlJqAhzS32iUZN70uC6t0wffLg5%2FaaiMezFxvFsZDIinDnQNBeSBQKxTAWodGuUrJ7izfnM9cgh%2FVNPUkAakCLQZCrdrW498zahjQvn8qCnQq2%2BU%2BKXoLsFoFZnMXbjK3nisgfLl3Mwe8NU1aUwLcOlKMsP8PTcq4%2F1BSAH9zPrc5Psy2gVPlYuGQIY%2Bm5T6vZ8bbTXYdfd6yB36t3iQkXrj6409oL1HtTskAQp2NLw%2FY1004ARv9alI2V1JeLcQ6P9hq9MVtzluzum34uWY6%2BsR8d%2FB1g7xl1ZuxQKpo%2BGiEeyXvpt63mIz6Vr9B9MKah0NYGOqUBDBddRzxvQ%2F%2F%2FeMBW9SvaCTOL7nb9jX%2BNlj69q6UOpbgqacZCgAghi1tyIjeXRi71gpgAhe19hF55eYEq5DSGfnVTZV1uJOHAZ3UY5QLJNZJoemJnrjV%2B%2Flccx%2FV5FMpUshm6r4Sl6hJWppBP%2FYX9P8I8pXoUP9tpeW7lLPvl%2BIRfrcNErrG%2B9qtCJMml6ycZbH0C28uCc9QdQaflJbpQd9zet6Ld&X-Amz-Signature=ae5f0462a3c887b2f39fd56da9f398fd3ee9f665339bad6e6729f2238f39e3b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
