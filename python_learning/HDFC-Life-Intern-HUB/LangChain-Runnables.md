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
fetched_at: '2026-09-09T02:06:32.370Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQ34ISS7%2F20260909%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260909T020627Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEAsTpHpLB8REi5r17XkjSazM1F1fHlT9NolFMARmN8aAiEA0pFCzDKZVnLwk4lLRN38eN32ylQxFtmG5%2FGSM3teXUwq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDMajfRvbg2%2FAKoPmZCrcA7Bzjhk1kluuFOuiN7HHAEatpBft8s48sN5ymHHEwMKncFPPtwRuxicxU94X2VgVFblN3k7jgBt%2FbuDkQ8N52jyJHsC%2Fq7tKe1vDb4FY5heipekLoslXpYzVCNzytHvX%2BaVpvnYtI%2Fqc7jwWxJAa5HVRud3KFQospuaGRXDZap0DiFy7OZ7TVU%2F1WnExmXHbjiJmJi4q6dYfrs8rkt%2B6%2F1%2FfqMSyJSz1Hrb87T3rKGXlI6DeB73rwFeyMcBjEB8kXjrIYe1DZ4Eqb8NXpyi%2By1pqgfr9bYCOKpsziIa3Wn6Zx6opOR9y7kvvJce3Bu%2F3TPYPsszQsSCUkt9x3NnJcgJQIw%2B1ozSxKvV33MQfhAyyNo%2BKYwWXYzDU6JTkrL3wwJxdt5hCRANBroteRu0IOCJD4wr1cuZ3C%2FGfCyfcI1fqsPeJLpvoS0C%2Fzbzb3YoUf%2FBSAEe66QfxHK2Zyw38zrNuZXBistFz%2BX%2BP0Drg73mP6iTyGvfj%2BzHRjC9%2Bqmn1H7u%2BVrS9t4Oh6%2Bc2h%2B8mmo4rdMeP1PLxaWoHzYHTsK3uKHhFizuZuEefI27WL0X%2F6IothT0o6DhYT06KOKwtkABqgmBkU57PQVndkYUjEwRdRRpNsfh0yGNvPBg2MNvYgtUGOqUBv0svf4vh7My7wfZIP%2BEr%2BH%2FELkvhaWuKVI%2BGQAb78UPANGC%2BZUIS1OCA8tH1qkSLglRUskaVkX6isgEs6dgHnZ3USrNHyP3%2BCPKjfa%2BNA0tD81G8%2B%2BcGWUYJp%2FtqzOBdN5wiThIa4eK3lckamiaa%2FXnUE8%2FEsmqQJXzlF3ttpfFDSJPr%2FlOvPsk5nZNd5rA3W7BThhJdU1kmBnglufSUAsmzInSh&X-Amz-Signature=ab50612e4f9097397399002c681b4e4abd0eb1cfaf13bbcb2932b18b1f8a25b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQ34ISS7%2F20260909%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260909T020627Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEAsTpHpLB8REi5r17XkjSazM1F1fHlT9NolFMARmN8aAiEA0pFCzDKZVnLwk4lLRN38eN32ylQxFtmG5%2FGSM3teXUwq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDMajfRvbg2%2FAKoPmZCrcA7Bzjhk1kluuFOuiN7HHAEatpBft8s48sN5ymHHEwMKncFPPtwRuxicxU94X2VgVFblN3k7jgBt%2FbuDkQ8N52jyJHsC%2Fq7tKe1vDb4FY5heipekLoslXpYzVCNzytHvX%2BaVpvnYtI%2Fqc7jwWxJAa5HVRud3KFQospuaGRXDZap0DiFy7OZ7TVU%2F1WnExmXHbjiJmJi4q6dYfrs8rkt%2B6%2F1%2FfqMSyJSz1Hrb87T3rKGXlI6DeB73rwFeyMcBjEB8kXjrIYe1DZ4Eqb8NXpyi%2By1pqgfr9bYCOKpsziIa3Wn6Zx6opOR9y7kvvJce3Bu%2F3TPYPsszQsSCUkt9x3NnJcgJQIw%2B1ozSxKvV33MQfhAyyNo%2BKYwWXYzDU6JTkrL3wwJxdt5hCRANBroteRu0IOCJD4wr1cuZ3C%2FGfCyfcI1fqsPeJLpvoS0C%2Fzbzb3YoUf%2FBSAEe66QfxHK2Zyw38zrNuZXBistFz%2BX%2BP0Drg73mP6iTyGvfj%2BzHRjC9%2Bqmn1H7u%2BVrS9t4Oh6%2Bc2h%2B8mmo4rdMeP1PLxaWoHzYHTsK3uKHhFizuZuEefI27WL0X%2F6IothT0o6DhYT06KOKwtkABqgmBkU57PQVndkYUjEwRdRRpNsfh0yGNvPBg2MNvYgtUGOqUBv0svf4vh7My7wfZIP%2BEr%2BH%2FELkvhaWuKVI%2BGQAb78UPANGC%2BZUIS1OCA8tH1qkSLglRUskaVkX6isgEs6dgHnZ3USrNHyP3%2BCPKjfa%2BNA0tD81G8%2B%2BcGWUYJp%2FtqzOBdN5wiThIa4eK3lckamiaa%2FXnUE8%2FEsmqQJXzlF3ttpfFDSJPr%2FlOvPsk5nZNd5rA3W7BThhJdU1kmBnglufSUAsmzInSh&X-Amz-Signature=13b2c8014b8481317ed1deea42aeb06eee9885f142944717b911221510654732&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQ34ISS7%2F20260909%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260909T020627Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEAsTpHpLB8REi5r17XkjSazM1F1fHlT9NolFMARmN8aAiEA0pFCzDKZVnLwk4lLRN38eN32ylQxFtmG5%2FGSM3teXUwq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDMajfRvbg2%2FAKoPmZCrcA7Bzjhk1kluuFOuiN7HHAEatpBft8s48sN5ymHHEwMKncFPPtwRuxicxU94X2VgVFblN3k7jgBt%2FbuDkQ8N52jyJHsC%2Fq7tKe1vDb4FY5heipekLoslXpYzVCNzytHvX%2BaVpvnYtI%2Fqc7jwWxJAa5HVRud3KFQospuaGRXDZap0DiFy7OZ7TVU%2F1WnExmXHbjiJmJi4q6dYfrs8rkt%2B6%2F1%2FfqMSyJSz1Hrb87T3rKGXlI6DeB73rwFeyMcBjEB8kXjrIYe1DZ4Eqb8NXpyi%2By1pqgfr9bYCOKpsziIa3Wn6Zx6opOR9y7kvvJce3Bu%2F3TPYPsszQsSCUkt9x3NnJcgJQIw%2B1ozSxKvV33MQfhAyyNo%2BKYwWXYzDU6JTkrL3wwJxdt5hCRANBroteRu0IOCJD4wr1cuZ3C%2FGfCyfcI1fqsPeJLpvoS0C%2Fzbzb3YoUf%2FBSAEe66QfxHK2Zyw38zrNuZXBistFz%2BX%2BP0Drg73mP6iTyGvfj%2BzHRjC9%2Bqmn1H7u%2BVrS9t4Oh6%2Bc2h%2B8mmo4rdMeP1PLxaWoHzYHTsK3uKHhFizuZuEefI27WL0X%2F6IothT0o6DhYT06KOKwtkABqgmBkU57PQVndkYUjEwRdRRpNsfh0yGNvPBg2MNvYgtUGOqUBv0svf4vh7My7wfZIP%2BEr%2BH%2FELkvhaWuKVI%2BGQAb78UPANGC%2BZUIS1OCA8tH1qkSLglRUskaVkX6isgEs6dgHnZ3USrNHyP3%2BCPKjfa%2BNA0tD81G8%2B%2BcGWUYJp%2FtqzOBdN5wiThIa4eK3lckamiaa%2FXnUE8%2FEsmqQJXzlF3ttpfFDSJPr%2FlOvPsk5nZNd5rA3W7BThhJdU1kmBnglufSUAsmzInSh&X-Amz-Signature=add69994105d738bba8ba465621eac8a9c481cd3955761a597dcb2032df388b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQ34ISS7%2F20260909%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260909T020627Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEAsTpHpLB8REi5r17XkjSazM1F1fHlT9NolFMARmN8aAiEA0pFCzDKZVnLwk4lLRN38eN32ylQxFtmG5%2FGSM3teXUwq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDMajfRvbg2%2FAKoPmZCrcA7Bzjhk1kluuFOuiN7HHAEatpBft8s48sN5ymHHEwMKncFPPtwRuxicxU94X2VgVFblN3k7jgBt%2FbuDkQ8N52jyJHsC%2Fq7tKe1vDb4FY5heipekLoslXpYzVCNzytHvX%2BaVpvnYtI%2Fqc7jwWxJAa5HVRud3KFQospuaGRXDZap0DiFy7OZ7TVU%2F1WnExmXHbjiJmJi4q6dYfrs8rkt%2B6%2F1%2FfqMSyJSz1Hrb87T3rKGXlI6DeB73rwFeyMcBjEB8kXjrIYe1DZ4Eqb8NXpyi%2By1pqgfr9bYCOKpsziIa3Wn6Zx6opOR9y7kvvJce3Bu%2F3TPYPsszQsSCUkt9x3NnJcgJQIw%2B1ozSxKvV33MQfhAyyNo%2BKYwWXYzDU6JTkrL3wwJxdt5hCRANBroteRu0IOCJD4wr1cuZ3C%2FGfCyfcI1fqsPeJLpvoS0C%2Fzbzb3YoUf%2FBSAEe66QfxHK2Zyw38zrNuZXBistFz%2BX%2BP0Drg73mP6iTyGvfj%2BzHRjC9%2Bqmn1H7u%2BVrS9t4Oh6%2Bc2h%2B8mmo4rdMeP1PLxaWoHzYHTsK3uKHhFizuZuEefI27WL0X%2F6IothT0o6DhYT06KOKwtkABqgmBkU57PQVndkYUjEwRdRRpNsfh0yGNvPBg2MNvYgtUGOqUBv0svf4vh7My7wfZIP%2BEr%2BH%2FELkvhaWuKVI%2BGQAb78UPANGC%2BZUIS1OCA8tH1qkSLglRUskaVkX6isgEs6dgHnZ3USrNHyP3%2BCPKjfa%2BNA0tD81G8%2B%2BcGWUYJp%2FtqzOBdN5wiThIa4eK3lckamiaa%2FXnUE8%2FEsmqQJXzlF3ttpfFDSJPr%2FlOvPsk5nZNd5rA3W7BThhJdU1kmBnglufSUAsmzInSh&X-Amz-Signature=13f6595d681109c3253533bc5d2073094627122a98c6c8e9e71a8dddea6f55ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQ34ISS7%2F20260909%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260909T020627Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEAsTpHpLB8REi5r17XkjSazM1F1fHlT9NolFMARmN8aAiEA0pFCzDKZVnLwk4lLRN38eN32ylQxFtmG5%2FGSM3teXUwq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDMajfRvbg2%2FAKoPmZCrcA7Bzjhk1kluuFOuiN7HHAEatpBft8s48sN5ymHHEwMKncFPPtwRuxicxU94X2VgVFblN3k7jgBt%2FbuDkQ8N52jyJHsC%2Fq7tKe1vDb4FY5heipekLoslXpYzVCNzytHvX%2BaVpvnYtI%2Fqc7jwWxJAa5HVRud3KFQospuaGRXDZap0DiFy7OZ7TVU%2F1WnExmXHbjiJmJi4q6dYfrs8rkt%2B6%2F1%2FfqMSyJSz1Hrb87T3rKGXlI6DeB73rwFeyMcBjEB8kXjrIYe1DZ4Eqb8NXpyi%2By1pqgfr9bYCOKpsziIa3Wn6Zx6opOR9y7kvvJce3Bu%2F3TPYPsszQsSCUkt9x3NnJcgJQIw%2B1ozSxKvV33MQfhAyyNo%2BKYwWXYzDU6JTkrL3wwJxdt5hCRANBroteRu0IOCJD4wr1cuZ3C%2FGfCyfcI1fqsPeJLpvoS0C%2Fzbzb3YoUf%2FBSAEe66QfxHK2Zyw38zrNuZXBistFz%2BX%2BP0Drg73mP6iTyGvfj%2BzHRjC9%2Bqmn1H7u%2BVrS9t4Oh6%2Bc2h%2B8mmo4rdMeP1PLxaWoHzYHTsK3uKHhFizuZuEefI27WL0X%2F6IothT0o6DhYT06KOKwtkABqgmBkU57PQVndkYUjEwRdRRpNsfh0yGNvPBg2MNvYgtUGOqUBv0svf4vh7My7wfZIP%2BEr%2BH%2FELkvhaWuKVI%2BGQAb78UPANGC%2BZUIS1OCA8tH1qkSLglRUskaVkX6isgEs6dgHnZ3USrNHyP3%2BCPKjfa%2BNA0tD81G8%2B%2BcGWUYJp%2FtqzOBdN5wiThIa4eK3lckamiaa%2FXnUE8%2FEsmqQJXzlF3ttpfFDSJPr%2FlOvPsk5nZNd5rA3W7BThhJdU1kmBnglufSUAsmzInSh&X-Amz-Signature=0f47524afb9a7de56a7bb3cd542e68aa045f3cd39016900f51d9be112ae6c9aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQ34ISS7%2F20260909%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260909T020627Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEAsTpHpLB8REi5r17XkjSazM1F1fHlT9NolFMARmN8aAiEA0pFCzDKZVnLwk4lLRN38eN32ylQxFtmG5%2FGSM3teXUwq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDMajfRvbg2%2FAKoPmZCrcA7Bzjhk1kluuFOuiN7HHAEatpBft8s48sN5ymHHEwMKncFPPtwRuxicxU94X2VgVFblN3k7jgBt%2FbuDkQ8N52jyJHsC%2Fq7tKe1vDb4FY5heipekLoslXpYzVCNzytHvX%2BaVpvnYtI%2Fqc7jwWxJAa5HVRud3KFQospuaGRXDZap0DiFy7OZ7TVU%2F1WnExmXHbjiJmJi4q6dYfrs8rkt%2B6%2F1%2FfqMSyJSz1Hrb87T3rKGXlI6DeB73rwFeyMcBjEB8kXjrIYe1DZ4Eqb8NXpyi%2By1pqgfr9bYCOKpsziIa3Wn6Zx6opOR9y7kvvJce3Bu%2F3TPYPsszQsSCUkt9x3NnJcgJQIw%2B1ozSxKvV33MQfhAyyNo%2BKYwWXYzDU6JTkrL3wwJxdt5hCRANBroteRu0IOCJD4wr1cuZ3C%2FGfCyfcI1fqsPeJLpvoS0C%2Fzbzb3YoUf%2FBSAEe66QfxHK2Zyw38zrNuZXBistFz%2BX%2BP0Drg73mP6iTyGvfj%2BzHRjC9%2Bqmn1H7u%2BVrS9t4Oh6%2Bc2h%2B8mmo4rdMeP1PLxaWoHzYHTsK3uKHhFizuZuEefI27WL0X%2F6IothT0o6DhYT06KOKwtkABqgmBkU57PQVndkYUjEwRdRRpNsfh0yGNvPBg2MNvYgtUGOqUBv0svf4vh7My7wfZIP%2BEr%2BH%2FELkvhaWuKVI%2BGQAb78UPANGC%2BZUIS1OCA8tH1qkSLglRUskaVkX6isgEs6dgHnZ3USrNHyP3%2BCPKjfa%2BNA0tD81G8%2B%2BcGWUYJp%2FtqzOBdN5wiThIa4eK3lckamiaa%2FXnUE8%2FEsmqQJXzlF3ttpfFDSJPr%2FlOvPsk5nZNd5rA3W7BThhJdU1kmBnglufSUAsmzInSh&X-Amz-Signature=2024ae2054949dc62b0d3931aee6b7d0f29917472d99ace780078231c2356a82&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQ34ISS7%2F20260909%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260909T020627Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEAsTpHpLB8REi5r17XkjSazM1F1fHlT9NolFMARmN8aAiEA0pFCzDKZVnLwk4lLRN38eN32ylQxFtmG5%2FGSM3teXUwq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDMajfRvbg2%2FAKoPmZCrcA7Bzjhk1kluuFOuiN7HHAEatpBft8s48sN5ymHHEwMKncFPPtwRuxicxU94X2VgVFblN3k7jgBt%2FbuDkQ8N52jyJHsC%2Fq7tKe1vDb4FY5heipekLoslXpYzVCNzytHvX%2BaVpvnYtI%2Fqc7jwWxJAa5HVRud3KFQospuaGRXDZap0DiFy7OZ7TVU%2F1WnExmXHbjiJmJi4q6dYfrs8rkt%2B6%2F1%2FfqMSyJSz1Hrb87T3rKGXlI6DeB73rwFeyMcBjEB8kXjrIYe1DZ4Eqb8NXpyi%2By1pqgfr9bYCOKpsziIa3Wn6Zx6opOR9y7kvvJce3Bu%2F3TPYPsszQsSCUkt9x3NnJcgJQIw%2B1ozSxKvV33MQfhAyyNo%2BKYwWXYzDU6JTkrL3wwJxdt5hCRANBroteRu0IOCJD4wr1cuZ3C%2FGfCyfcI1fqsPeJLpvoS0C%2Fzbzb3YoUf%2FBSAEe66QfxHK2Zyw38zrNuZXBistFz%2BX%2BP0Drg73mP6iTyGvfj%2BzHRjC9%2Bqmn1H7u%2BVrS9t4Oh6%2Bc2h%2B8mmo4rdMeP1PLxaWoHzYHTsK3uKHhFizuZuEefI27WL0X%2F6IothT0o6DhYT06KOKwtkABqgmBkU57PQVndkYUjEwRdRRpNsfh0yGNvPBg2MNvYgtUGOqUBv0svf4vh7My7wfZIP%2BEr%2BH%2FELkvhaWuKVI%2BGQAb78UPANGC%2BZUIS1OCA8tH1qkSLglRUskaVkX6isgEs6dgHnZ3USrNHyP3%2BCPKjfa%2BNA0tD81G8%2B%2BcGWUYJp%2FtqzOBdN5wiThIa4eK3lckamiaa%2FXnUE8%2FEsmqQJXzlF3ttpfFDSJPr%2FlOvPsk5nZNd5rA3W7BThhJdU1kmBnglufSUAsmzInSh&X-Amz-Signature=51d61fd64c63b134da10a6c7b3e6c533ee5386a94dca09a8032fdd799adc2660&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQ34ISS7%2F20260909%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260909T020627Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEAsTpHpLB8REi5r17XkjSazM1F1fHlT9NolFMARmN8aAiEA0pFCzDKZVnLwk4lLRN38eN32ylQxFtmG5%2FGSM3teXUwq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDMajfRvbg2%2FAKoPmZCrcA7Bzjhk1kluuFOuiN7HHAEatpBft8s48sN5ymHHEwMKncFPPtwRuxicxU94X2VgVFblN3k7jgBt%2FbuDkQ8N52jyJHsC%2Fq7tKe1vDb4FY5heipekLoslXpYzVCNzytHvX%2BaVpvnYtI%2Fqc7jwWxJAa5HVRud3KFQospuaGRXDZap0DiFy7OZ7TVU%2F1WnExmXHbjiJmJi4q6dYfrs8rkt%2B6%2F1%2FfqMSyJSz1Hrb87T3rKGXlI6DeB73rwFeyMcBjEB8kXjrIYe1DZ4Eqb8NXpyi%2By1pqgfr9bYCOKpsziIa3Wn6Zx6opOR9y7kvvJce3Bu%2F3TPYPsszQsSCUkt9x3NnJcgJQIw%2B1ozSxKvV33MQfhAyyNo%2BKYwWXYzDU6JTkrL3wwJxdt5hCRANBroteRu0IOCJD4wr1cuZ3C%2FGfCyfcI1fqsPeJLpvoS0C%2Fzbzb3YoUf%2FBSAEe66QfxHK2Zyw38zrNuZXBistFz%2BX%2BP0Drg73mP6iTyGvfj%2BzHRjC9%2Bqmn1H7u%2BVrS9t4Oh6%2Bc2h%2B8mmo4rdMeP1PLxaWoHzYHTsK3uKHhFizuZuEefI27WL0X%2F6IothT0o6DhYT06KOKwtkABqgmBkU57PQVndkYUjEwRdRRpNsfh0yGNvPBg2MNvYgtUGOqUBv0svf4vh7My7wfZIP%2BEr%2BH%2FELkvhaWuKVI%2BGQAb78UPANGC%2BZUIS1OCA8tH1qkSLglRUskaVkX6isgEs6dgHnZ3USrNHyP3%2BCPKjfa%2BNA0tD81G8%2B%2BcGWUYJp%2FtqzOBdN5wiThIa4eK3lckamiaa%2FXnUE8%2FEsmqQJXzlF3ttpfFDSJPr%2FlOvPsk5nZNd5rA3W7BThhJdU1kmBnglufSUAsmzInSh&X-Amz-Signature=8be9ff656664ab2ddd83d58a85ff26926aff865f73d219298af58fed9884507c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQ34ISS7%2F20260909%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260909T020627Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEAsTpHpLB8REi5r17XkjSazM1F1fHlT9NolFMARmN8aAiEA0pFCzDKZVnLwk4lLRN38eN32ylQxFtmG5%2FGSM3teXUwq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDMajfRvbg2%2FAKoPmZCrcA7Bzjhk1kluuFOuiN7HHAEatpBft8s48sN5ymHHEwMKncFPPtwRuxicxU94X2VgVFblN3k7jgBt%2FbuDkQ8N52jyJHsC%2Fq7tKe1vDb4FY5heipekLoslXpYzVCNzytHvX%2BaVpvnYtI%2Fqc7jwWxJAa5HVRud3KFQospuaGRXDZap0DiFy7OZ7TVU%2F1WnExmXHbjiJmJi4q6dYfrs8rkt%2B6%2F1%2FfqMSyJSz1Hrb87T3rKGXlI6DeB73rwFeyMcBjEB8kXjrIYe1DZ4Eqb8NXpyi%2By1pqgfr9bYCOKpsziIa3Wn6Zx6opOR9y7kvvJce3Bu%2F3TPYPsszQsSCUkt9x3NnJcgJQIw%2B1ozSxKvV33MQfhAyyNo%2BKYwWXYzDU6JTkrL3wwJxdt5hCRANBroteRu0IOCJD4wr1cuZ3C%2FGfCyfcI1fqsPeJLpvoS0C%2Fzbzb3YoUf%2FBSAEe66QfxHK2Zyw38zrNuZXBistFz%2BX%2BP0Drg73mP6iTyGvfj%2BzHRjC9%2Bqmn1H7u%2BVrS9t4Oh6%2Bc2h%2B8mmo4rdMeP1PLxaWoHzYHTsK3uKHhFizuZuEefI27WL0X%2F6IothT0o6DhYT06KOKwtkABqgmBkU57PQVndkYUjEwRdRRpNsfh0yGNvPBg2MNvYgtUGOqUBv0svf4vh7My7wfZIP%2BEr%2BH%2FELkvhaWuKVI%2BGQAb78UPANGC%2BZUIS1OCA8tH1qkSLglRUskaVkX6isgEs6dgHnZ3USrNHyP3%2BCPKjfa%2BNA0tD81G8%2B%2BcGWUYJp%2FtqzOBdN5wiThIa4eK3lckamiaa%2FXnUE8%2FEsmqQJXzlF3ttpfFDSJPr%2FlOvPsk5nZNd5rA3W7BThhJdU1kmBnglufSUAsmzInSh&X-Amz-Signature=2855e190a8935f514e9bc4d11c1500d1bd2904250fed407d1aae96800052978c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
