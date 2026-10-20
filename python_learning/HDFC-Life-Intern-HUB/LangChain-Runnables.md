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
fetched_at: '2026-10-20T02:35:19.224Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHJ4NIF6%2F20261020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261020T023515Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIBvqzMTYsXLj2ektypy0ggHxnm%2FA82DrFIiNYxM%2FyvmSAiBImqIfVMJ4%2FEcX0%2FfaLYyL4MX7m7%2FVd9UL6uCZJ8BAqSr%2FAwg6EAAaDDYzNzQyMzE4MzgwNSIMIVHNTdQWW5ZO%2BLv%2FKtwDDHWYsKyHqD%2Ft7LNMw8dhT%2BNJg1upIhw1N8gCJpFRpoKK3HGqqwsr0VmGd8Oa0%2Ft%2BVZeZfrIUTuPXWWMJfOP922WMHK4FKs7bdFBhSgTIYjTVEHpENdomz%2ByRMf4cUkaT%2BOfEs%2FPhUs27l70K7IzpxZxsBPg7Pv2BNQJqwAiqxR0Y%2Fl4fFMYGDTusFIlS%2FxrQWnPae21EyogIP1fYwXwraLFzs3ouIK5eDggx9TJ%2BUhOuy7xRYw13HPRSJipA5mb%2FlEN1lQKXUeuqS6YjWLZDfDM26uxlpoLVflSi7qNFsS7ezDZJq2aL%2FmfprI0o%2FUohbYW87fRnoOuPDsUq5YkVi0am8YM05Om3C6ohKVOAnATZFJv2LjqBKBy3lVZM8MQ4Y2I3wBqM41EcA2np5Hi4QIXfoJ7WF%2BwqFrjpM558F87c9zDh6AJ0TTo9vaEqAj4vXlkjEdmge5hKJI7O8TrrOVD3MuAcBocYO9qAHaVJFCtbFogfJHZBcVZryCaMroptKCPJl2RjT%2FvHz%2BKA7wZWbTeOuUn9hqzkhH2ZfYkStDt43OD9YYpFvje70XLp0UFLVc4Lyh5qm%2FJo21f5XSx0ZfecHOO7lwOzBwUJQ7uNdqfoU09J%2FRhKnv%2B89bQwq4Db1gY6pgHFmiy26MrfGAze88yovZ3Mw%2B6SjWGD9q3NVw98zvznFT3Imwk8khaRpk%2BqhV%2BUmINO8Qd7nflgZcu4mBvnHbHuWfz%2B%2Ff3jRjwSRna%2F5mO6R9HgBYMzea0wef7oT5UjfPV57pPWLPC7dCctavsHBfxyGpAdw%2FDTD36EdycLbA%2FHtcfx1Mv0%2BMwtXQzsmEaRQ83P8VgOD1sTjyF%2BaZocIog7OAizwlef&X-Amz-Signature=930a73918dcefe0fa737ddd13c2ee024c0ad6a79368ea387cd13c4c2b9aa0924&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHJ4NIF6%2F20261020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261020T023515Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIBvqzMTYsXLj2ektypy0ggHxnm%2FA82DrFIiNYxM%2FyvmSAiBImqIfVMJ4%2FEcX0%2FfaLYyL4MX7m7%2FVd9UL6uCZJ8BAqSr%2FAwg6EAAaDDYzNzQyMzE4MzgwNSIMIVHNTdQWW5ZO%2BLv%2FKtwDDHWYsKyHqD%2Ft7LNMw8dhT%2BNJg1upIhw1N8gCJpFRpoKK3HGqqwsr0VmGd8Oa0%2Ft%2BVZeZfrIUTuPXWWMJfOP922WMHK4FKs7bdFBhSgTIYjTVEHpENdomz%2ByRMf4cUkaT%2BOfEs%2FPhUs27l70K7IzpxZxsBPg7Pv2BNQJqwAiqxR0Y%2Fl4fFMYGDTusFIlS%2FxrQWnPae21EyogIP1fYwXwraLFzs3ouIK5eDggx9TJ%2BUhOuy7xRYw13HPRSJipA5mb%2FlEN1lQKXUeuqS6YjWLZDfDM26uxlpoLVflSi7qNFsS7ezDZJq2aL%2FmfprI0o%2FUohbYW87fRnoOuPDsUq5YkVi0am8YM05Om3C6ohKVOAnATZFJv2LjqBKBy3lVZM8MQ4Y2I3wBqM41EcA2np5Hi4QIXfoJ7WF%2BwqFrjpM558F87c9zDh6AJ0TTo9vaEqAj4vXlkjEdmge5hKJI7O8TrrOVD3MuAcBocYO9qAHaVJFCtbFogfJHZBcVZryCaMroptKCPJl2RjT%2FvHz%2BKA7wZWbTeOuUn9hqzkhH2ZfYkStDt43OD9YYpFvje70XLp0UFLVc4Lyh5qm%2FJo21f5XSx0ZfecHOO7lwOzBwUJQ7uNdqfoU09J%2FRhKnv%2B89bQwq4Db1gY6pgHFmiy26MrfGAze88yovZ3Mw%2B6SjWGD9q3NVw98zvznFT3Imwk8khaRpk%2BqhV%2BUmINO8Qd7nflgZcu4mBvnHbHuWfz%2B%2Ff3jRjwSRna%2F5mO6R9HgBYMzea0wef7oT5UjfPV57pPWLPC7dCctavsHBfxyGpAdw%2FDTD36EdycLbA%2FHtcfx1Mv0%2BMwtXQzsmEaRQ83P8VgOD1sTjyF%2BaZocIog7OAizwlef&X-Amz-Signature=78353e0983f8f68e2ce4febb7ea34d8bf3df5dc5717dc11d68eda43478c6033a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHJ4NIF6%2F20261020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261020T023515Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIBvqzMTYsXLj2ektypy0ggHxnm%2FA82DrFIiNYxM%2FyvmSAiBImqIfVMJ4%2FEcX0%2FfaLYyL4MX7m7%2FVd9UL6uCZJ8BAqSr%2FAwg6EAAaDDYzNzQyMzE4MzgwNSIMIVHNTdQWW5ZO%2BLv%2FKtwDDHWYsKyHqD%2Ft7LNMw8dhT%2BNJg1upIhw1N8gCJpFRpoKK3HGqqwsr0VmGd8Oa0%2Ft%2BVZeZfrIUTuPXWWMJfOP922WMHK4FKs7bdFBhSgTIYjTVEHpENdomz%2ByRMf4cUkaT%2BOfEs%2FPhUs27l70K7IzpxZxsBPg7Pv2BNQJqwAiqxR0Y%2Fl4fFMYGDTusFIlS%2FxrQWnPae21EyogIP1fYwXwraLFzs3ouIK5eDggx9TJ%2BUhOuy7xRYw13HPRSJipA5mb%2FlEN1lQKXUeuqS6YjWLZDfDM26uxlpoLVflSi7qNFsS7ezDZJq2aL%2FmfprI0o%2FUohbYW87fRnoOuPDsUq5YkVi0am8YM05Om3C6ohKVOAnATZFJv2LjqBKBy3lVZM8MQ4Y2I3wBqM41EcA2np5Hi4QIXfoJ7WF%2BwqFrjpM558F87c9zDh6AJ0TTo9vaEqAj4vXlkjEdmge5hKJI7O8TrrOVD3MuAcBocYO9qAHaVJFCtbFogfJHZBcVZryCaMroptKCPJl2RjT%2FvHz%2BKA7wZWbTeOuUn9hqzkhH2ZfYkStDt43OD9YYpFvje70XLp0UFLVc4Lyh5qm%2FJo21f5XSx0ZfecHOO7lwOzBwUJQ7uNdqfoU09J%2FRhKnv%2B89bQwq4Db1gY6pgHFmiy26MrfGAze88yovZ3Mw%2B6SjWGD9q3NVw98zvznFT3Imwk8khaRpk%2BqhV%2BUmINO8Qd7nflgZcu4mBvnHbHuWfz%2B%2Ff3jRjwSRna%2F5mO6R9HgBYMzea0wef7oT5UjfPV57pPWLPC7dCctavsHBfxyGpAdw%2FDTD36EdycLbA%2FHtcfx1Mv0%2BMwtXQzsmEaRQ83P8VgOD1sTjyF%2BaZocIog7OAizwlef&X-Amz-Signature=e35324771a0798c0c605b20fb2d0282e1e98f178f65b55398e7c00522f43af36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHJ4NIF6%2F20261020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261020T023515Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIBvqzMTYsXLj2ektypy0ggHxnm%2FA82DrFIiNYxM%2FyvmSAiBImqIfVMJ4%2FEcX0%2FfaLYyL4MX7m7%2FVd9UL6uCZJ8BAqSr%2FAwg6EAAaDDYzNzQyMzE4MzgwNSIMIVHNTdQWW5ZO%2BLv%2FKtwDDHWYsKyHqD%2Ft7LNMw8dhT%2BNJg1upIhw1N8gCJpFRpoKK3HGqqwsr0VmGd8Oa0%2Ft%2BVZeZfrIUTuPXWWMJfOP922WMHK4FKs7bdFBhSgTIYjTVEHpENdomz%2ByRMf4cUkaT%2BOfEs%2FPhUs27l70K7IzpxZxsBPg7Pv2BNQJqwAiqxR0Y%2Fl4fFMYGDTusFIlS%2FxrQWnPae21EyogIP1fYwXwraLFzs3ouIK5eDggx9TJ%2BUhOuy7xRYw13HPRSJipA5mb%2FlEN1lQKXUeuqS6YjWLZDfDM26uxlpoLVflSi7qNFsS7ezDZJq2aL%2FmfprI0o%2FUohbYW87fRnoOuPDsUq5YkVi0am8YM05Om3C6ohKVOAnATZFJv2LjqBKBy3lVZM8MQ4Y2I3wBqM41EcA2np5Hi4QIXfoJ7WF%2BwqFrjpM558F87c9zDh6AJ0TTo9vaEqAj4vXlkjEdmge5hKJI7O8TrrOVD3MuAcBocYO9qAHaVJFCtbFogfJHZBcVZryCaMroptKCPJl2RjT%2FvHz%2BKA7wZWbTeOuUn9hqzkhH2ZfYkStDt43OD9YYpFvje70XLp0UFLVc4Lyh5qm%2FJo21f5XSx0ZfecHOO7lwOzBwUJQ7uNdqfoU09J%2FRhKnv%2B89bQwq4Db1gY6pgHFmiy26MrfGAze88yovZ3Mw%2B6SjWGD9q3NVw98zvznFT3Imwk8khaRpk%2BqhV%2BUmINO8Qd7nflgZcu4mBvnHbHuWfz%2B%2Ff3jRjwSRna%2F5mO6R9HgBYMzea0wef7oT5UjfPV57pPWLPC7dCctavsHBfxyGpAdw%2FDTD36EdycLbA%2FHtcfx1Mv0%2BMwtXQzsmEaRQ83P8VgOD1sTjyF%2BaZocIog7OAizwlef&X-Amz-Signature=26bcec9b186d5cf77a160153c8bf85580813578a05015cf2fd5d5b9bb7f9847c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHJ4NIF6%2F20261020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261020T023515Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIBvqzMTYsXLj2ektypy0ggHxnm%2FA82DrFIiNYxM%2FyvmSAiBImqIfVMJ4%2FEcX0%2FfaLYyL4MX7m7%2FVd9UL6uCZJ8BAqSr%2FAwg6EAAaDDYzNzQyMzE4MzgwNSIMIVHNTdQWW5ZO%2BLv%2FKtwDDHWYsKyHqD%2Ft7LNMw8dhT%2BNJg1upIhw1N8gCJpFRpoKK3HGqqwsr0VmGd8Oa0%2Ft%2BVZeZfrIUTuPXWWMJfOP922WMHK4FKs7bdFBhSgTIYjTVEHpENdomz%2ByRMf4cUkaT%2BOfEs%2FPhUs27l70K7IzpxZxsBPg7Pv2BNQJqwAiqxR0Y%2Fl4fFMYGDTusFIlS%2FxrQWnPae21EyogIP1fYwXwraLFzs3ouIK5eDggx9TJ%2BUhOuy7xRYw13HPRSJipA5mb%2FlEN1lQKXUeuqS6YjWLZDfDM26uxlpoLVflSi7qNFsS7ezDZJq2aL%2FmfprI0o%2FUohbYW87fRnoOuPDsUq5YkVi0am8YM05Om3C6ohKVOAnATZFJv2LjqBKBy3lVZM8MQ4Y2I3wBqM41EcA2np5Hi4QIXfoJ7WF%2BwqFrjpM558F87c9zDh6AJ0TTo9vaEqAj4vXlkjEdmge5hKJI7O8TrrOVD3MuAcBocYO9qAHaVJFCtbFogfJHZBcVZryCaMroptKCPJl2RjT%2FvHz%2BKA7wZWbTeOuUn9hqzkhH2ZfYkStDt43OD9YYpFvje70XLp0UFLVc4Lyh5qm%2FJo21f5XSx0ZfecHOO7lwOzBwUJQ7uNdqfoU09J%2FRhKnv%2B89bQwq4Db1gY6pgHFmiy26MrfGAze88yovZ3Mw%2B6SjWGD9q3NVw98zvznFT3Imwk8khaRpk%2BqhV%2BUmINO8Qd7nflgZcu4mBvnHbHuWfz%2B%2Ff3jRjwSRna%2F5mO6R9HgBYMzea0wef7oT5UjfPV57pPWLPC7dCctavsHBfxyGpAdw%2FDTD36EdycLbA%2FHtcfx1Mv0%2BMwtXQzsmEaRQ83P8VgOD1sTjyF%2BaZocIog7OAizwlef&X-Amz-Signature=a460784bfde5f95ccce00d990a2be87b53958c770fd14c3c049355c9fd32f790&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHJ4NIF6%2F20261020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261020T023515Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIBvqzMTYsXLj2ektypy0ggHxnm%2FA82DrFIiNYxM%2FyvmSAiBImqIfVMJ4%2FEcX0%2FfaLYyL4MX7m7%2FVd9UL6uCZJ8BAqSr%2FAwg6EAAaDDYzNzQyMzE4MzgwNSIMIVHNTdQWW5ZO%2BLv%2FKtwDDHWYsKyHqD%2Ft7LNMw8dhT%2BNJg1upIhw1N8gCJpFRpoKK3HGqqwsr0VmGd8Oa0%2Ft%2BVZeZfrIUTuPXWWMJfOP922WMHK4FKs7bdFBhSgTIYjTVEHpENdomz%2ByRMf4cUkaT%2BOfEs%2FPhUs27l70K7IzpxZxsBPg7Pv2BNQJqwAiqxR0Y%2Fl4fFMYGDTusFIlS%2FxrQWnPae21EyogIP1fYwXwraLFzs3ouIK5eDggx9TJ%2BUhOuy7xRYw13HPRSJipA5mb%2FlEN1lQKXUeuqS6YjWLZDfDM26uxlpoLVflSi7qNFsS7ezDZJq2aL%2FmfprI0o%2FUohbYW87fRnoOuPDsUq5YkVi0am8YM05Om3C6ohKVOAnATZFJv2LjqBKBy3lVZM8MQ4Y2I3wBqM41EcA2np5Hi4QIXfoJ7WF%2BwqFrjpM558F87c9zDh6AJ0TTo9vaEqAj4vXlkjEdmge5hKJI7O8TrrOVD3MuAcBocYO9qAHaVJFCtbFogfJHZBcVZryCaMroptKCPJl2RjT%2FvHz%2BKA7wZWbTeOuUn9hqzkhH2ZfYkStDt43OD9YYpFvje70XLp0UFLVc4Lyh5qm%2FJo21f5XSx0ZfecHOO7lwOzBwUJQ7uNdqfoU09J%2FRhKnv%2B89bQwq4Db1gY6pgHFmiy26MrfGAze88yovZ3Mw%2B6SjWGD9q3NVw98zvznFT3Imwk8khaRpk%2BqhV%2BUmINO8Qd7nflgZcu4mBvnHbHuWfz%2B%2Ff3jRjwSRna%2F5mO6R9HgBYMzea0wef7oT5UjfPV57pPWLPC7dCctavsHBfxyGpAdw%2FDTD36EdycLbA%2FHtcfx1Mv0%2BMwtXQzsmEaRQ83P8VgOD1sTjyF%2BaZocIog7OAizwlef&X-Amz-Signature=720eeb0dec7195c86dc99ade0727173e0f1857cb072f06845a377dbba133cb9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHJ4NIF6%2F20261020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261020T023515Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIBvqzMTYsXLj2ektypy0ggHxnm%2FA82DrFIiNYxM%2FyvmSAiBImqIfVMJ4%2FEcX0%2FfaLYyL4MX7m7%2FVd9UL6uCZJ8BAqSr%2FAwg6EAAaDDYzNzQyMzE4MzgwNSIMIVHNTdQWW5ZO%2BLv%2FKtwDDHWYsKyHqD%2Ft7LNMw8dhT%2BNJg1upIhw1N8gCJpFRpoKK3HGqqwsr0VmGd8Oa0%2Ft%2BVZeZfrIUTuPXWWMJfOP922WMHK4FKs7bdFBhSgTIYjTVEHpENdomz%2ByRMf4cUkaT%2BOfEs%2FPhUs27l70K7IzpxZxsBPg7Pv2BNQJqwAiqxR0Y%2Fl4fFMYGDTusFIlS%2FxrQWnPae21EyogIP1fYwXwraLFzs3ouIK5eDggx9TJ%2BUhOuy7xRYw13HPRSJipA5mb%2FlEN1lQKXUeuqS6YjWLZDfDM26uxlpoLVflSi7qNFsS7ezDZJq2aL%2FmfprI0o%2FUohbYW87fRnoOuPDsUq5YkVi0am8YM05Om3C6ohKVOAnATZFJv2LjqBKBy3lVZM8MQ4Y2I3wBqM41EcA2np5Hi4QIXfoJ7WF%2BwqFrjpM558F87c9zDh6AJ0TTo9vaEqAj4vXlkjEdmge5hKJI7O8TrrOVD3MuAcBocYO9qAHaVJFCtbFogfJHZBcVZryCaMroptKCPJl2RjT%2FvHz%2BKA7wZWbTeOuUn9hqzkhH2ZfYkStDt43OD9YYpFvje70XLp0UFLVc4Lyh5qm%2FJo21f5XSx0ZfecHOO7lwOzBwUJQ7uNdqfoU09J%2FRhKnv%2B89bQwq4Db1gY6pgHFmiy26MrfGAze88yovZ3Mw%2B6SjWGD9q3NVw98zvznFT3Imwk8khaRpk%2BqhV%2BUmINO8Qd7nflgZcu4mBvnHbHuWfz%2B%2Ff3jRjwSRna%2F5mO6R9HgBYMzea0wef7oT5UjfPV57pPWLPC7dCctavsHBfxyGpAdw%2FDTD36EdycLbA%2FHtcfx1Mv0%2BMwtXQzsmEaRQ83P8VgOD1sTjyF%2BaZocIog7OAizwlef&X-Amz-Signature=e2059a19762c015323fc1a0a4a664db80f5b5d1ea4ef3f50c8b9615762fde62d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHJ4NIF6%2F20261020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261020T023515Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIBvqzMTYsXLj2ektypy0ggHxnm%2FA82DrFIiNYxM%2FyvmSAiBImqIfVMJ4%2FEcX0%2FfaLYyL4MX7m7%2FVd9UL6uCZJ8BAqSr%2FAwg6EAAaDDYzNzQyMzE4MzgwNSIMIVHNTdQWW5ZO%2BLv%2FKtwDDHWYsKyHqD%2Ft7LNMw8dhT%2BNJg1upIhw1N8gCJpFRpoKK3HGqqwsr0VmGd8Oa0%2Ft%2BVZeZfrIUTuPXWWMJfOP922WMHK4FKs7bdFBhSgTIYjTVEHpENdomz%2ByRMf4cUkaT%2BOfEs%2FPhUs27l70K7IzpxZxsBPg7Pv2BNQJqwAiqxR0Y%2Fl4fFMYGDTusFIlS%2FxrQWnPae21EyogIP1fYwXwraLFzs3ouIK5eDggx9TJ%2BUhOuy7xRYw13HPRSJipA5mb%2FlEN1lQKXUeuqS6YjWLZDfDM26uxlpoLVflSi7qNFsS7ezDZJq2aL%2FmfprI0o%2FUohbYW87fRnoOuPDsUq5YkVi0am8YM05Om3C6ohKVOAnATZFJv2LjqBKBy3lVZM8MQ4Y2I3wBqM41EcA2np5Hi4QIXfoJ7WF%2BwqFrjpM558F87c9zDh6AJ0TTo9vaEqAj4vXlkjEdmge5hKJI7O8TrrOVD3MuAcBocYO9qAHaVJFCtbFogfJHZBcVZryCaMroptKCPJl2RjT%2FvHz%2BKA7wZWbTeOuUn9hqzkhH2ZfYkStDt43OD9YYpFvje70XLp0UFLVc4Lyh5qm%2FJo21f5XSx0ZfecHOO7lwOzBwUJQ7uNdqfoU09J%2FRhKnv%2B89bQwq4Db1gY6pgHFmiy26MrfGAze88yovZ3Mw%2B6SjWGD9q3NVw98zvznFT3Imwk8khaRpk%2BqhV%2BUmINO8Qd7nflgZcu4mBvnHbHuWfz%2B%2Ff3jRjwSRna%2F5mO6R9HgBYMzea0wef7oT5UjfPV57pPWLPC7dCctavsHBfxyGpAdw%2FDTD36EdycLbA%2FHtcfx1Mv0%2BMwtXQzsmEaRQ83P8VgOD1sTjyF%2BaZocIog7OAizwlef&X-Amz-Signature=55e6329891a5828673fcf6776a2eefee536447fe30757480a48ce11566836b53&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHJ4NIF6%2F20261020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261020T023515Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIBvqzMTYsXLj2ektypy0ggHxnm%2FA82DrFIiNYxM%2FyvmSAiBImqIfVMJ4%2FEcX0%2FfaLYyL4MX7m7%2FVd9UL6uCZJ8BAqSr%2FAwg6EAAaDDYzNzQyMzE4MzgwNSIMIVHNTdQWW5ZO%2BLv%2FKtwDDHWYsKyHqD%2Ft7LNMw8dhT%2BNJg1upIhw1N8gCJpFRpoKK3HGqqwsr0VmGd8Oa0%2Ft%2BVZeZfrIUTuPXWWMJfOP922WMHK4FKs7bdFBhSgTIYjTVEHpENdomz%2ByRMf4cUkaT%2BOfEs%2FPhUs27l70K7IzpxZxsBPg7Pv2BNQJqwAiqxR0Y%2Fl4fFMYGDTusFIlS%2FxrQWnPae21EyogIP1fYwXwraLFzs3ouIK5eDggx9TJ%2BUhOuy7xRYw13HPRSJipA5mb%2FlEN1lQKXUeuqS6YjWLZDfDM26uxlpoLVflSi7qNFsS7ezDZJq2aL%2FmfprI0o%2FUohbYW87fRnoOuPDsUq5YkVi0am8YM05Om3C6ohKVOAnATZFJv2LjqBKBy3lVZM8MQ4Y2I3wBqM41EcA2np5Hi4QIXfoJ7WF%2BwqFrjpM558F87c9zDh6AJ0TTo9vaEqAj4vXlkjEdmge5hKJI7O8TrrOVD3MuAcBocYO9qAHaVJFCtbFogfJHZBcVZryCaMroptKCPJl2RjT%2FvHz%2BKA7wZWbTeOuUn9hqzkhH2ZfYkStDt43OD9YYpFvje70XLp0UFLVc4Lyh5qm%2FJo21f5XSx0ZfecHOO7lwOzBwUJQ7uNdqfoU09J%2FRhKnv%2B89bQwq4Db1gY6pgHFmiy26MrfGAze88yovZ3Mw%2B6SjWGD9q3NVw98zvznFT3Imwk8khaRpk%2BqhV%2BUmINO8Qd7nflgZcu4mBvnHbHuWfz%2B%2Ff3jRjwSRna%2F5mO6R9HgBYMzea0wef7oT5UjfPV57pPWLPC7dCctavsHBfxyGpAdw%2FDTD36EdycLbA%2FHtcfx1Mv0%2BMwtXQzsmEaRQ83P8VgOD1sTjyF%2BaZocIog7OAizwlef&X-Amz-Signature=c86814b384cb4766e962e5c7c40e4876bc6c0b203b123ad0c81bbb36a30695f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
