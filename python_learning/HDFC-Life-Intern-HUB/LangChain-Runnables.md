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
fetched_at: '2026-10-25T02:15:59.681Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RAKXDR66%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021556Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOc72%2FNeg8L7ga1nHgnKLUzuPfP3ZctSt8hFImiZijmgIgM17lEaoEZFoOmLGZpiPcPvSva%2FRrFhJwBB5WFPTv1%2BUqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNIoetMO0W94R4tRRSrcA9u5dPxgf8PlRyHP68lYM7vZnjnDUQxUsrXVrCALsfPnVcEkHU7thBq88hkpKFmlXe83B%2FP8HZEJS1k18uZWyVvhFC2mlwnUmBAO5waSXFZq9tlI0n%2F2OmvYfL82vpihA6%2FGTrddHrcSww%2BOLYtvCUucsPeUDUqSXjwWZnIfOaYrCXOpgAOzTRWG7S20lRy7hARy3czLl24wLaZSqU8j2Lz0Ii3DyjDkfkeLWVEoPydgPisrm0eAOaJjUrQNGZ458xA6hjF4ViFjvWYjHI2BlG1DC3Dnb4a%2FXPSeHkRQ2ZeWydmUq%2BGWRsAVMFhYmwTnVV0SPK4MD8HyL88eL2vDc34a8bseJxehZIIA5ryyVgl0ZZHmjNsx%2BTADSRTEs8LjBu0ddscA5zquN2Ld%2FmIXJGG8b%2F2skLqPxWXiN8aehO8deROHndYuWUzdmv06vWYRyfIT0JF7Z%2Fme6d8XP4C1yoliVuOM65jZuBdBmTpfQOQh%2BM4oMbgsNah6JUNpQQNOUMpgGVvU1JAspwPLGx2uG3FXSqFcLntvKMHd1Zp%2FbmeEizgW%2FaowwIrLi%2Bsr3qA%2BOqgZ0%2FzDikS0gl9Pwi5DwmZR4xEcOSjsuYpTc8lbtjWXP4GK0lYttz34rifVMJLU9NYGOqUBMCk54zxW2lZuTEiwTdGTduZ%2F%2BHmpKgdlFFtFqMTQ98kRd2Iv3WXe7vb8f8YbiVhVUkBUrMzeLjmZgaBqku%2BCWPlZVKfrBzUeV4B9HJUj%2BOPz7wO%2Bs0eWlwFgAou0FG5kqfn16Lh0YZygoKKWuWB7jwWkYB3BePH%2FjsekKYw6%2B8FjOysrUsrCS2w5XZu4HAqvgLZe4isgcY6PpQLj7EXoeINX9vCv&X-Amz-Signature=edd3805fc708f308a5b0be9579292ea86dfc36a6b1ab16f5b680598cbd78ceea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RAKXDR66%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021556Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOc72%2FNeg8L7ga1nHgnKLUzuPfP3ZctSt8hFImiZijmgIgM17lEaoEZFoOmLGZpiPcPvSva%2FRrFhJwBB5WFPTv1%2BUqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNIoetMO0W94R4tRRSrcA9u5dPxgf8PlRyHP68lYM7vZnjnDUQxUsrXVrCALsfPnVcEkHU7thBq88hkpKFmlXe83B%2FP8HZEJS1k18uZWyVvhFC2mlwnUmBAO5waSXFZq9tlI0n%2F2OmvYfL82vpihA6%2FGTrddHrcSww%2BOLYtvCUucsPeUDUqSXjwWZnIfOaYrCXOpgAOzTRWG7S20lRy7hARy3czLl24wLaZSqU8j2Lz0Ii3DyjDkfkeLWVEoPydgPisrm0eAOaJjUrQNGZ458xA6hjF4ViFjvWYjHI2BlG1DC3Dnb4a%2FXPSeHkRQ2ZeWydmUq%2BGWRsAVMFhYmwTnVV0SPK4MD8HyL88eL2vDc34a8bseJxehZIIA5ryyVgl0ZZHmjNsx%2BTADSRTEs8LjBu0ddscA5zquN2Ld%2FmIXJGG8b%2F2skLqPxWXiN8aehO8deROHndYuWUzdmv06vWYRyfIT0JF7Z%2Fme6d8XP4C1yoliVuOM65jZuBdBmTpfQOQh%2BM4oMbgsNah6JUNpQQNOUMpgGVvU1JAspwPLGx2uG3FXSqFcLntvKMHd1Zp%2FbmeEizgW%2FaowwIrLi%2Bsr3qA%2BOqgZ0%2FzDikS0gl9Pwi5DwmZR4xEcOSjsuYpTc8lbtjWXP4GK0lYttz34rifVMJLU9NYGOqUBMCk54zxW2lZuTEiwTdGTduZ%2F%2BHmpKgdlFFtFqMTQ98kRd2Iv3WXe7vb8f8YbiVhVUkBUrMzeLjmZgaBqku%2BCWPlZVKfrBzUeV4B9HJUj%2BOPz7wO%2Bs0eWlwFgAou0FG5kqfn16Lh0YZygoKKWuWB7jwWkYB3BePH%2FjsekKYw6%2B8FjOysrUsrCS2w5XZu4HAqvgLZe4isgcY6PpQLj7EXoeINX9vCv&X-Amz-Signature=4d5493cd0b33ea1838abac278e74f95178a1a452e806c25d36d3961af8327476&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RAKXDR66%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021556Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOc72%2FNeg8L7ga1nHgnKLUzuPfP3ZctSt8hFImiZijmgIgM17lEaoEZFoOmLGZpiPcPvSva%2FRrFhJwBB5WFPTv1%2BUqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNIoetMO0W94R4tRRSrcA9u5dPxgf8PlRyHP68lYM7vZnjnDUQxUsrXVrCALsfPnVcEkHU7thBq88hkpKFmlXe83B%2FP8HZEJS1k18uZWyVvhFC2mlwnUmBAO5waSXFZq9tlI0n%2F2OmvYfL82vpihA6%2FGTrddHrcSww%2BOLYtvCUucsPeUDUqSXjwWZnIfOaYrCXOpgAOzTRWG7S20lRy7hARy3czLl24wLaZSqU8j2Lz0Ii3DyjDkfkeLWVEoPydgPisrm0eAOaJjUrQNGZ458xA6hjF4ViFjvWYjHI2BlG1DC3Dnb4a%2FXPSeHkRQ2ZeWydmUq%2BGWRsAVMFhYmwTnVV0SPK4MD8HyL88eL2vDc34a8bseJxehZIIA5ryyVgl0ZZHmjNsx%2BTADSRTEs8LjBu0ddscA5zquN2Ld%2FmIXJGG8b%2F2skLqPxWXiN8aehO8deROHndYuWUzdmv06vWYRyfIT0JF7Z%2Fme6d8XP4C1yoliVuOM65jZuBdBmTpfQOQh%2BM4oMbgsNah6JUNpQQNOUMpgGVvU1JAspwPLGx2uG3FXSqFcLntvKMHd1Zp%2FbmeEizgW%2FaowwIrLi%2Bsr3qA%2BOqgZ0%2FzDikS0gl9Pwi5DwmZR4xEcOSjsuYpTc8lbtjWXP4GK0lYttz34rifVMJLU9NYGOqUBMCk54zxW2lZuTEiwTdGTduZ%2F%2BHmpKgdlFFtFqMTQ98kRd2Iv3WXe7vb8f8YbiVhVUkBUrMzeLjmZgaBqku%2BCWPlZVKfrBzUeV4B9HJUj%2BOPz7wO%2Bs0eWlwFgAou0FG5kqfn16Lh0YZygoKKWuWB7jwWkYB3BePH%2FjsekKYw6%2B8FjOysrUsrCS2w5XZu4HAqvgLZe4isgcY6PpQLj7EXoeINX9vCv&X-Amz-Signature=6c4f75e0c94e8a753f6d9fb593c4f2317fcb114067f130e6e0f9345732fc4c68&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RAKXDR66%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021556Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOc72%2FNeg8L7ga1nHgnKLUzuPfP3ZctSt8hFImiZijmgIgM17lEaoEZFoOmLGZpiPcPvSva%2FRrFhJwBB5WFPTv1%2BUqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNIoetMO0W94R4tRRSrcA9u5dPxgf8PlRyHP68lYM7vZnjnDUQxUsrXVrCALsfPnVcEkHU7thBq88hkpKFmlXe83B%2FP8HZEJS1k18uZWyVvhFC2mlwnUmBAO5waSXFZq9tlI0n%2F2OmvYfL82vpihA6%2FGTrddHrcSww%2BOLYtvCUucsPeUDUqSXjwWZnIfOaYrCXOpgAOzTRWG7S20lRy7hARy3czLl24wLaZSqU8j2Lz0Ii3DyjDkfkeLWVEoPydgPisrm0eAOaJjUrQNGZ458xA6hjF4ViFjvWYjHI2BlG1DC3Dnb4a%2FXPSeHkRQ2ZeWydmUq%2BGWRsAVMFhYmwTnVV0SPK4MD8HyL88eL2vDc34a8bseJxehZIIA5ryyVgl0ZZHmjNsx%2BTADSRTEs8LjBu0ddscA5zquN2Ld%2FmIXJGG8b%2F2skLqPxWXiN8aehO8deROHndYuWUzdmv06vWYRyfIT0JF7Z%2Fme6d8XP4C1yoliVuOM65jZuBdBmTpfQOQh%2BM4oMbgsNah6JUNpQQNOUMpgGVvU1JAspwPLGx2uG3FXSqFcLntvKMHd1Zp%2FbmeEizgW%2FaowwIrLi%2Bsr3qA%2BOqgZ0%2FzDikS0gl9Pwi5DwmZR4xEcOSjsuYpTc8lbtjWXP4GK0lYttz34rifVMJLU9NYGOqUBMCk54zxW2lZuTEiwTdGTduZ%2F%2BHmpKgdlFFtFqMTQ98kRd2Iv3WXe7vb8f8YbiVhVUkBUrMzeLjmZgaBqku%2BCWPlZVKfrBzUeV4B9HJUj%2BOPz7wO%2Bs0eWlwFgAou0FG5kqfn16Lh0YZygoKKWuWB7jwWkYB3BePH%2FjsekKYw6%2B8FjOysrUsrCS2w5XZu4HAqvgLZe4isgcY6PpQLj7EXoeINX9vCv&X-Amz-Signature=8afc3051d3e60f14638052104ac3be440f38ae76b19399352537ba15292b9dbc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RAKXDR66%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021556Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOc72%2FNeg8L7ga1nHgnKLUzuPfP3ZctSt8hFImiZijmgIgM17lEaoEZFoOmLGZpiPcPvSva%2FRrFhJwBB5WFPTv1%2BUqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNIoetMO0W94R4tRRSrcA9u5dPxgf8PlRyHP68lYM7vZnjnDUQxUsrXVrCALsfPnVcEkHU7thBq88hkpKFmlXe83B%2FP8HZEJS1k18uZWyVvhFC2mlwnUmBAO5waSXFZq9tlI0n%2F2OmvYfL82vpihA6%2FGTrddHrcSww%2BOLYtvCUucsPeUDUqSXjwWZnIfOaYrCXOpgAOzTRWG7S20lRy7hARy3czLl24wLaZSqU8j2Lz0Ii3DyjDkfkeLWVEoPydgPisrm0eAOaJjUrQNGZ458xA6hjF4ViFjvWYjHI2BlG1DC3Dnb4a%2FXPSeHkRQ2ZeWydmUq%2BGWRsAVMFhYmwTnVV0SPK4MD8HyL88eL2vDc34a8bseJxehZIIA5ryyVgl0ZZHmjNsx%2BTADSRTEs8LjBu0ddscA5zquN2Ld%2FmIXJGG8b%2F2skLqPxWXiN8aehO8deROHndYuWUzdmv06vWYRyfIT0JF7Z%2Fme6d8XP4C1yoliVuOM65jZuBdBmTpfQOQh%2BM4oMbgsNah6JUNpQQNOUMpgGVvU1JAspwPLGx2uG3FXSqFcLntvKMHd1Zp%2FbmeEizgW%2FaowwIrLi%2Bsr3qA%2BOqgZ0%2FzDikS0gl9Pwi5DwmZR4xEcOSjsuYpTc8lbtjWXP4GK0lYttz34rifVMJLU9NYGOqUBMCk54zxW2lZuTEiwTdGTduZ%2F%2BHmpKgdlFFtFqMTQ98kRd2Iv3WXe7vb8f8YbiVhVUkBUrMzeLjmZgaBqku%2BCWPlZVKfrBzUeV4B9HJUj%2BOPz7wO%2Bs0eWlwFgAou0FG5kqfn16Lh0YZygoKKWuWB7jwWkYB3BePH%2FjsekKYw6%2B8FjOysrUsrCS2w5XZu4HAqvgLZe4isgcY6PpQLj7EXoeINX9vCv&X-Amz-Signature=7e1a7aadabd8271deab708d3e7dd1188c7501ecbe8da81dbb0aeaf917fc9e38c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RAKXDR66%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021556Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOc72%2FNeg8L7ga1nHgnKLUzuPfP3ZctSt8hFImiZijmgIgM17lEaoEZFoOmLGZpiPcPvSva%2FRrFhJwBB5WFPTv1%2BUqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNIoetMO0W94R4tRRSrcA9u5dPxgf8PlRyHP68lYM7vZnjnDUQxUsrXVrCALsfPnVcEkHU7thBq88hkpKFmlXe83B%2FP8HZEJS1k18uZWyVvhFC2mlwnUmBAO5waSXFZq9tlI0n%2F2OmvYfL82vpihA6%2FGTrddHrcSww%2BOLYtvCUucsPeUDUqSXjwWZnIfOaYrCXOpgAOzTRWG7S20lRy7hARy3czLl24wLaZSqU8j2Lz0Ii3DyjDkfkeLWVEoPydgPisrm0eAOaJjUrQNGZ458xA6hjF4ViFjvWYjHI2BlG1DC3Dnb4a%2FXPSeHkRQ2ZeWydmUq%2BGWRsAVMFhYmwTnVV0SPK4MD8HyL88eL2vDc34a8bseJxehZIIA5ryyVgl0ZZHmjNsx%2BTADSRTEs8LjBu0ddscA5zquN2Ld%2FmIXJGG8b%2F2skLqPxWXiN8aehO8deROHndYuWUzdmv06vWYRyfIT0JF7Z%2Fme6d8XP4C1yoliVuOM65jZuBdBmTpfQOQh%2BM4oMbgsNah6JUNpQQNOUMpgGVvU1JAspwPLGx2uG3FXSqFcLntvKMHd1Zp%2FbmeEizgW%2FaowwIrLi%2Bsr3qA%2BOqgZ0%2FzDikS0gl9Pwi5DwmZR4xEcOSjsuYpTc8lbtjWXP4GK0lYttz34rifVMJLU9NYGOqUBMCk54zxW2lZuTEiwTdGTduZ%2F%2BHmpKgdlFFtFqMTQ98kRd2Iv3WXe7vb8f8YbiVhVUkBUrMzeLjmZgaBqku%2BCWPlZVKfrBzUeV4B9HJUj%2BOPz7wO%2Bs0eWlwFgAou0FG5kqfn16Lh0YZygoKKWuWB7jwWkYB3BePH%2FjsekKYw6%2B8FjOysrUsrCS2w5XZu4HAqvgLZe4isgcY6PpQLj7EXoeINX9vCv&X-Amz-Signature=12c073b7601a972725672af363de152ce74a7f8207b6e7defb1f0da1f25a29a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RAKXDR66%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021556Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOc72%2FNeg8L7ga1nHgnKLUzuPfP3ZctSt8hFImiZijmgIgM17lEaoEZFoOmLGZpiPcPvSva%2FRrFhJwBB5WFPTv1%2BUqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNIoetMO0W94R4tRRSrcA9u5dPxgf8PlRyHP68lYM7vZnjnDUQxUsrXVrCALsfPnVcEkHU7thBq88hkpKFmlXe83B%2FP8HZEJS1k18uZWyVvhFC2mlwnUmBAO5waSXFZq9tlI0n%2F2OmvYfL82vpihA6%2FGTrddHrcSww%2BOLYtvCUucsPeUDUqSXjwWZnIfOaYrCXOpgAOzTRWG7S20lRy7hARy3czLl24wLaZSqU8j2Lz0Ii3DyjDkfkeLWVEoPydgPisrm0eAOaJjUrQNGZ458xA6hjF4ViFjvWYjHI2BlG1DC3Dnb4a%2FXPSeHkRQ2ZeWydmUq%2BGWRsAVMFhYmwTnVV0SPK4MD8HyL88eL2vDc34a8bseJxehZIIA5ryyVgl0ZZHmjNsx%2BTADSRTEs8LjBu0ddscA5zquN2Ld%2FmIXJGG8b%2F2skLqPxWXiN8aehO8deROHndYuWUzdmv06vWYRyfIT0JF7Z%2Fme6d8XP4C1yoliVuOM65jZuBdBmTpfQOQh%2BM4oMbgsNah6JUNpQQNOUMpgGVvU1JAspwPLGx2uG3FXSqFcLntvKMHd1Zp%2FbmeEizgW%2FaowwIrLi%2Bsr3qA%2BOqgZ0%2FzDikS0gl9Pwi5DwmZR4xEcOSjsuYpTc8lbtjWXP4GK0lYttz34rifVMJLU9NYGOqUBMCk54zxW2lZuTEiwTdGTduZ%2F%2BHmpKgdlFFtFqMTQ98kRd2Iv3WXe7vb8f8YbiVhVUkBUrMzeLjmZgaBqku%2BCWPlZVKfrBzUeV4B9HJUj%2BOPz7wO%2Bs0eWlwFgAou0FG5kqfn16Lh0YZygoKKWuWB7jwWkYB3BePH%2FjsekKYw6%2B8FjOysrUsrCS2w5XZu4HAqvgLZe4isgcY6PpQLj7EXoeINX9vCv&X-Amz-Signature=5ff74841d15186643a24299c6fcd95db3e43790c91ea2f3359ee067a073e0333&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RAKXDR66%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021556Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOc72%2FNeg8L7ga1nHgnKLUzuPfP3ZctSt8hFImiZijmgIgM17lEaoEZFoOmLGZpiPcPvSva%2FRrFhJwBB5WFPTv1%2BUqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNIoetMO0W94R4tRRSrcA9u5dPxgf8PlRyHP68lYM7vZnjnDUQxUsrXVrCALsfPnVcEkHU7thBq88hkpKFmlXe83B%2FP8HZEJS1k18uZWyVvhFC2mlwnUmBAO5waSXFZq9tlI0n%2F2OmvYfL82vpihA6%2FGTrddHrcSww%2BOLYtvCUucsPeUDUqSXjwWZnIfOaYrCXOpgAOzTRWG7S20lRy7hARy3czLl24wLaZSqU8j2Lz0Ii3DyjDkfkeLWVEoPydgPisrm0eAOaJjUrQNGZ458xA6hjF4ViFjvWYjHI2BlG1DC3Dnb4a%2FXPSeHkRQ2ZeWydmUq%2BGWRsAVMFhYmwTnVV0SPK4MD8HyL88eL2vDc34a8bseJxehZIIA5ryyVgl0ZZHmjNsx%2BTADSRTEs8LjBu0ddscA5zquN2Ld%2FmIXJGG8b%2F2skLqPxWXiN8aehO8deROHndYuWUzdmv06vWYRyfIT0JF7Z%2Fme6d8XP4C1yoliVuOM65jZuBdBmTpfQOQh%2BM4oMbgsNah6JUNpQQNOUMpgGVvU1JAspwPLGx2uG3FXSqFcLntvKMHd1Zp%2FbmeEizgW%2FaowwIrLi%2Bsr3qA%2BOqgZ0%2FzDikS0gl9Pwi5DwmZR4xEcOSjsuYpTc8lbtjWXP4GK0lYttz34rifVMJLU9NYGOqUBMCk54zxW2lZuTEiwTdGTduZ%2F%2BHmpKgdlFFtFqMTQ98kRd2Iv3WXe7vb8f8YbiVhVUkBUrMzeLjmZgaBqku%2BCWPlZVKfrBzUeV4B9HJUj%2BOPz7wO%2Bs0eWlwFgAou0FG5kqfn16Lh0YZygoKKWuWB7jwWkYB3BePH%2FjsekKYw6%2B8FjOysrUsrCS2w5XZu4HAqvgLZe4isgcY6PpQLj7EXoeINX9vCv&X-Amz-Signature=d284828ca0eca4667164df8a415fdc495df228bcf4bcf307a7884182163f2ca0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RAKXDR66%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021556Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOc72%2FNeg8L7ga1nHgnKLUzuPfP3ZctSt8hFImiZijmgIgM17lEaoEZFoOmLGZpiPcPvSva%2FRrFhJwBB5WFPTv1%2BUqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNIoetMO0W94R4tRRSrcA9u5dPxgf8PlRyHP68lYM7vZnjnDUQxUsrXVrCALsfPnVcEkHU7thBq88hkpKFmlXe83B%2FP8HZEJS1k18uZWyVvhFC2mlwnUmBAO5waSXFZq9tlI0n%2F2OmvYfL82vpihA6%2FGTrddHrcSww%2BOLYtvCUucsPeUDUqSXjwWZnIfOaYrCXOpgAOzTRWG7S20lRy7hARy3czLl24wLaZSqU8j2Lz0Ii3DyjDkfkeLWVEoPydgPisrm0eAOaJjUrQNGZ458xA6hjF4ViFjvWYjHI2BlG1DC3Dnb4a%2FXPSeHkRQ2ZeWydmUq%2BGWRsAVMFhYmwTnVV0SPK4MD8HyL88eL2vDc34a8bseJxehZIIA5ryyVgl0ZZHmjNsx%2BTADSRTEs8LjBu0ddscA5zquN2Ld%2FmIXJGG8b%2F2skLqPxWXiN8aehO8deROHndYuWUzdmv06vWYRyfIT0JF7Z%2Fme6d8XP4C1yoliVuOM65jZuBdBmTpfQOQh%2BM4oMbgsNah6JUNpQQNOUMpgGVvU1JAspwPLGx2uG3FXSqFcLntvKMHd1Zp%2FbmeEizgW%2FaowwIrLi%2Bsr3qA%2BOqgZ0%2FzDikS0gl9Pwi5DwmZR4xEcOSjsuYpTc8lbtjWXP4GK0lYttz34rifVMJLU9NYGOqUBMCk54zxW2lZuTEiwTdGTduZ%2F%2BHmpKgdlFFtFqMTQ98kRd2Iv3WXe7vb8f8YbiVhVUkBUrMzeLjmZgaBqku%2BCWPlZVKfrBzUeV4B9HJUj%2BOPz7wO%2Bs0eWlwFgAou0FG5kqfn16Lh0YZygoKKWuWB7jwWkYB3BePH%2FjsekKYw6%2B8FjOysrUsrCS2w5XZu4HAqvgLZe4isgcY6PpQLj7EXoeINX9vCv&X-Amz-Signature=3e05a0a830438a07eb6a08019cd9cae0e58ac1c312e8f9a7233ad8c35709c4cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
