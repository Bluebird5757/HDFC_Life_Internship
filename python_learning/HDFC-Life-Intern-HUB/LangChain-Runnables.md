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
fetched_at: '2026-09-26T02:31:25.844Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLLQYTCE%2F20260926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260926T023120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIANVehqz9lf9C6B%2BvQd5Lq5%2FQH%2FhC%2BbMkTp2QQnwHPxlAiB8NdN0UFkLIsirIJ8Cp35c0f%2Fa%2FAlTNeqfhHMwiHR6cCqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAljaUFuTaIb%2BxH7SKtwDxIolirLbtGxiYLv0eD32QKz9d8ZYNNXIFlcJYYAOPjkv25s6uGOO%2BPP3DDWDV3ZSPNRTQf9EAu03yYDhuMv%2BmsVSZ67Th8rVEy2xZw0Bo6mEU9kGDRW6idK1KP2CfeJxyfX1DMyNb%2FElUHbuTPfIKfJAbiH6qIIzW3APsIBQVyoXvqMHw%2BVE7%2B32BbEbC5lIhtKbjoq%2FDECE9CbDYKzIQSJQkX52Y8Yo%2FsYkE0XiJ2xdEN2mYE6g7TdRrt0uEtjGn8T%2BCzdixZiPaOadNEL2IKlszuGfwWidfK3GHL53hQ7AOHYmguQsyvLv4Ly3DBsrZVF3I3OgU5ayjc%2B6PALhh46vOsMMVwwKZVelG7YOs5aCXFs6jkXvlxqg5fMDPG6t1qLai%2BVDeTfgYIeohm%2FusJDhqDP3mh6Kid4agsftqkTwPdnVSZ2S19SSqWiLPJPQGiDWSwxUix3ZcVCXn0IPVOrSz%2FmhZNcBLKvt%2FntKM5FD7YexIT7uDzELtByARBOV4DWNfKHhVR4tgZkgT9MlNjpvHz2YlXF9ROGBuxUh1I%2BHN7jJkPuUFbShZd9W%2BUDeGYdxqEwSL5ONPy%2FmdTiLzfmAvbD9vihcT6RFWbVukgvYm3V7JXeOHPaI3uYwkanc1QY6pgEJebyLa3cRKONy0BZoXaDyjkEtsnUPYP7RSo9LriKsIMw0fCYbtYvofOm3Pel4ejzz5liYUk2ULuAv9XE4BKOA914Lpz0OaSA3AT%2Fcr7Ft6kIa3Zy%2BYGWV%2FbJBvtTlUaBIIeFZvlRU%2Br1luKFd2AzEWkrZDtAiddWS8sk3cxd7ruBzrO%2Fb1rgxMAHJhkRznc87uNdjPCY3BAHFdjlpqHaKZ5dA%2FH81&X-Amz-Signature=aff477f331716e170dfe8a60c2f70fb93cc87cd947f5f7e10c4b1b5049a813e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLLQYTCE%2F20260926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260926T023120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIANVehqz9lf9C6B%2BvQd5Lq5%2FQH%2FhC%2BbMkTp2QQnwHPxlAiB8NdN0UFkLIsirIJ8Cp35c0f%2Fa%2FAlTNeqfhHMwiHR6cCqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAljaUFuTaIb%2BxH7SKtwDxIolirLbtGxiYLv0eD32QKz9d8ZYNNXIFlcJYYAOPjkv25s6uGOO%2BPP3DDWDV3ZSPNRTQf9EAu03yYDhuMv%2BmsVSZ67Th8rVEy2xZw0Bo6mEU9kGDRW6idK1KP2CfeJxyfX1DMyNb%2FElUHbuTPfIKfJAbiH6qIIzW3APsIBQVyoXvqMHw%2BVE7%2B32BbEbC5lIhtKbjoq%2FDECE9CbDYKzIQSJQkX52Y8Yo%2FsYkE0XiJ2xdEN2mYE6g7TdRrt0uEtjGn8T%2BCzdixZiPaOadNEL2IKlszuGfwWidfK3GHL53hQ7AOHYmguQsyvLv4Ly3DBsrZVF3I3OgU5ayjc%2B6PALhh46vOsMMVwwKZVelG7YOs5aCXFs6jkXvlxqg5fMDPG6t1qLai%2BVDeTfgYIeohm%2FusJDhqDP3mh6Kid4agsftqkTwPdnVSZ2S19SSqWiLPJPQGiDWSwxUix3ZcVCXn0IPVOrSz%2FmhZNcBLKvt%2FntKM5FD7YexIT7uDzELtByARBOV4DWNfKHhVR4tgZkgT9MlNjpvHz2YlXF9ROGBuxUh1I%2BHN7jJkPuUFbShZd9W%2BUDeGYdxqEwSL5ONPy%2FmdTiLzfmAvbD9vihcT6RFWbVukgvYm3V7JXeOHPaI3uYwkanc1QY6pgEJebyLa3cRKONy0BZoXaDyjkEtsnUPYP7RSo9LriKsIMw0fCYbtYvofOm3Pel4ejzz5liYUk2ULuAv9XE4BKOA914Lpz0OaSA3AT%2Fcr7Ft6kIa3Zy%2BYGWV%2FbJBvtTlUaBIIeFZvlRU%2Br1luKFd2AzEWkrZDtAiddWS8sk3cxd7ruBzrO%2Fb1rgxMAHJhkRznc87uNdjPCY3BAHFdjlpqHaKZ5dA%2FH81&X-Amz-Signature=6f16fcfec4ae2850bc1af48d9ea7a0766d322d4b9f4c12138ceed26f8d6945d4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLLQYTCE%2F20260926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260926T023120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIANVehqz9lf9C6B%2BvQd5Lq5%2FQH%2FhC%2BbMkTp2QQnwHPxlAiB8NdN0UFkLIsirIJ8Cp35c0f%2Fa%2FAlTNeqfhHMwiHR6cCqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAljaUFuTaIb%2BxH7SKtwDxIolirLbtGxiYLv0eD32QKz9d8ZYNNXIFlcJYYAOPjkv25s6uGOO%2BPP3DDWDV3ZSPNRTQf9EAu03yYDhuMv%2BmsVSZ67Th8rVEy2xZw0Bo6mEU9kGDRW6idK1KP2CfeJxyfX1DMyNb%2FElUHbuTPfIKfJAbiH6qIIzW3APsIBQVyoXvqMHw%2BVE7%2B32BbEbC5lIhtKbjoq%2FDECE9CbDYKzIQSJQkX52Y8Yo%2FsYkE0XiJ2xdEN2mYE6g7TdRrt0uEtjGn8T%2BCzdixZiPaOadNEL2IKlszuGfwWidfK3GHL53hQ7AOHYmguQsyvLv4Ly3DBsrZVF3I3OgU5ayjc%2B6PALhh46vOsMMVwwKZVelG7YOs5aCXFs6jkXvlxqg5fMDPG6t1qLai%2BVDeTfgYIeohm%2FusJDhqDP3mh6Kid4agsftqkTwPdnVSZ2S19SSqWiLPJPQGiDWSwxUix3ZcVCXn0IPVOrSz%2FmhZNcBLKvt%2FntKM5FD7YexIT7uDzELtByARBOV4DWNfKHhVR4tgZkgT9MlNjpvHz2YlXF9ROGBuxUh1I%2BHN7jJkPuUFbShZd9W%2BUDeGYdxqEwSL5ONPy%2FmdTiLzfmAvbD9vihcT6RFWbVukgvYm3V7JXeOHPaI3uYwkanc1QY6pgEJebyLa3cRKONy0BZoXaDyjkEtsnUPYP7RSo9LriKsIMw0fCYbtYvofOm3Pel4ejzz5liYUk2ULuAv9XE4BKOA914Lpz0OaSA3AT%2Fcr7Ft6kIa3Zy%2BYGWV%2FbJBvtTlUaBIIeFZvlRU%2Br1luKFd2AzEWkrZDtAiddWS8sk3cxd7ruBzrO%2Fb1rgxMAHJhkRznc87uNdjPCY3BAHFdjlpqHaKZ5dA%2FH81&X-Amz-Signature=7bd4b49308d283f919f73bd7676496a2009710fe5a694839413964246bcfb903&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLLQYTCE%2F20260926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260926T023120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIANVehqz9lf9C6B%2BvQd5Lq5%2FQH%2FhC%2BbMkTp2QQnwHPxlAiB8NdN0UFkLIsirIJ8Cp35c0f%2Fa%2FAlTNeqfhHMwiHR6cCqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAljaUFuTaIb%2BxH7SKtwDxIolirLbtGxiYLv0eD32QKz9d8ZYNNXIFlcJYYAOPjkv25s6uGOO%2BPP3DDWDV3ZSPNRTQf9EAu03yYDhuMv%2BmsVSZ67Th8rVEy2xZw0Bo6mEU9kGDRW6idK1KP2CfeJxyfX1DMyNb%2FElUHbuTPfIKfJAbiH6qIIzW3APsIBQVyoXvqMHw%2BVE7%2B32BbEbC5lIhtKbjoq%2FDECE9CbDYKzIQSJQkX52Y8Yo%2FsYkE0XiJ2xdEN2mYE6g7TdRrt0uEtjGn8T%2BCzdixZiPaOadNEL2IKlszuGfwWidfK3GHL53hQ7AOHYmguQsyvLv4Ly3DBsrZVF3I3OgU5ayjc%2B6PALhh46vOsMMVwwKZVelG7YOs5aCXFs6jkXvlxqg5fMDPG6t1qLai%2BVDeTfgYIeohm%2FusJDhqDP3mh6Kid4agsftqkTwPdnVSZ2S19SSqWiLPJPQGiDWSwxUix3ZcVCXn0IPVOrSz%2FmhZNcBLKvt%2FntKM5FD7YexIT7uDzELtByARBOV4DWNfKHhVR4tgZkgT9MlNjpvHz2YlXF9ROGBuxUh1I%2BHN7jJkPuUFbShZd9W%2BUDeGYdxqEwSL5ONPy%2FmdTiLzfmAvbD9vihcT6RFWbVukgvYm3V7JXeOHPaI3uYwkanc1QY6pgEJebyLa3cRKONy0BZoXaDyjkEtsnUPYP7RSo9LriKsIMw0fCYbtYvofOm3Pel4ejzz5liYUk2ULuAv9XE4BKOA914Lpz0OaSA3AT%2Fcr7Ft6kIa3Zy%2BYGWV%2FbJBvtTlUaBIIeFZvlRU%2Br1luKFd2AzEWkrZDtAiddWS8sk3cxd7ruBzrO%2Fb1rgxMAHJhkRznc87uNdjPCY3BAHFdjlpqHaKZ5dA%2FH81&X-Amz-Signature=07840a5f59168ac50555be544dbfbde234959a11bc30a843de8e260f37295976&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLLQYTCE%2F20260926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260926T023120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIANVehqz9lf9C6B%2BvQd5Lq5%2FQH%2FhC%2BbMkTp2QQnwHPxlAiB8NdN0UFkLIsirIJ8Cp35c0f%2Fa%2FAlTNeqfhHMwiHR6cCqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAljaUFuTaIb%2BxH7SKtwDxIolirLbtGxiYLv0eD32QKz9d8ZYNNXIFlcJYYAOPjkv25s6uGOO%2BPP3DDWDV3ZSPNRTQf9EAu03yYDhuMv%2BmsVSZ67Th8rVEy2xZw0Bo6mEU9kGDRW6idK1KP2CfeJxyfX1DMyNb%2FElUHbuTPfIKfJAbiH6qIIzW3APsIBQVyoXvqMHw%2BVE7%2B32BbEbC5lIhtKbjoq%2FDECE9CbDYKzIQSJQkX52Y8Yo%2FsYkE0XiJ2xdEN2mYE6g7TdRrt0uEtjGn8T%2BCzdixZiPaOadNEL2IKlszuGfwWidfK3GHL53hQ7AOHYmguQsyvLv4Ly3DBsrZVF3I3OgU5ayjc%2B6PALhh46vOsMMVwwKZVelG7YOs5aCXFs6jkXvlxqg5fMDPG6t1qLai%2BVDeTfgYIeohm%2FusJDhqDP3mh6Kid4agsftqkTwPdnVSZ2S19SSqWiLPJPQGiDWSwxUix3ZcVCXn0IPVOrSz%2FmhZNcBLKvt%2FntKM5FD7YexIT7uDzELtByARBOV4DWNfKHhVR4tgZkgT9MlNjpvHz2YlXF9ROGBuxUh1I%2BHN7jJkPuUFbShZd9W%2BUDeGYdxqEwSL5ONPy%2FmdTiLzfmAvbD9vihcT6RFWbVukgvYm3V7JXeOHPaI3uYwkanc1QY6pgEJebyLa3cRKONy0BZoXaDyjkEtsnUPYP7RSo9LriKsIMw0fCYbtYvofOm3Pel4ejzz5liYUk2ULuAv9XE4BKOA914Lpz0OaSA3AT%2Fcr7Ft6kIa3Zy%2BYGWV%2FbJBvtTlUaBIIeFZvlRU%2Br1luKFd2AzEWkrZDtAiddWS8sk3cxd7ruBzrO%2Fb1rgxMAHJhkRznc87uNdjPCY3BAHFdjlpqHaKZ5dA%2FH81&X-Amz-Signature=19cc430c205b72acb45107cb20c48d2b1408f45e93e20ab3dd90464ae3a65833&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLLQYTCE%2F20260926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260926T023120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIANVehqz9lf9C6B%2BvQd5Lq5%2FQH%2FhC%2BbMkTp2QQnwHPxlAiB8NdN0UFkLIsirIJ8Cp35c0f%2Fa%2FAlTNeqfhHMwiHR6cCqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAljaUFuTaIb%2BxH7SKtwDxIolirLbtGxiYLv0eD32QKz9d8ZYNNXIFlcJYYAOPjkv25s6uGOO%2BPP3DDWDV3ZSPNRTQf9EAu03yYDhuMv%2BmsVSZ67Th8rVEy2xZw0Bo6mEU9kGDRW6idK1KP2CfeJxyfX1DMyNb%2FElUHbuTPfIKfJAbiH6qIIzW3APsIBQVyoXvqMHw%2BVE7%2B32BbEbC5lIhtKbjoq%2FDECE9CbDYKzIQSJQkX52Y8Yo%2FsYkE0XiJ2xdEN2mYE6g7TdRrt0uEtjGn8T%2BCzdixZiPaOadNEL2IKlszuGfwWidfK3GHL53hQ7AOHYmguQsyvLv4Ly3DBsrZVF3I3OgU5ayjc%2B6PALhh46vOsMMVwwKZVelG7YOs5aCXFs6jkXvlxqg5fMDPG6t1qLai%2BVDeTfgYIeohm%2FusJDhqDP3mh6Kid4agsftqkTwPdnVSZ2S19SSqWiLPJPQGiDWSwxUix3ZcVCXn0IPVOrSz%2FmhZNcBLKvt%2FntKM5FD7YexIT7uDzELtByARBOV4DWNfKHhVR4tgZkgT9MlNjpvHz2YlXF9ROGBuxUh1I%2BHN7jJkPuUFbShZd9W%2BUDeGYdxqEwSL5ONPy%2FmdTiLzfmAvbD9vihcT6RFWbVukgvYm3V7JXeOHPaI3uYwkanc1QY6pgEJebyLa3cRKONy0BZoXaDyjkEtsnUPYP7RSo9LriKsIMw0fCYbtYvofOm3Pel4ejzz5liYUk2ULuAv9XE4BKOA914Lpz0OaSA3AT%2Fcr7Ft6kIa3Zy%2BYGWV%2FbJBvtTlUaBIIeFZvlRU%2Br1luKFd2AzEWkrZDtAiddWS8sk3cxd7ruBzrO%2Fb1rgxMAHJhkRznc87uNdjPCY3BAHFdjlpqHaKZ5dA%2FH81&X-Amz-Signature=68d1d71898d929e20c3aee5625b58955aff86546c7a09fbe0d29d7f8d92077f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLLQYTCE%2F20260926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260926T023120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIANVehqz9lf9C6B%2BvQd5Lq5%2FQH%2FhC%2BbMkTp2QQnwHPxlAiB8NdN0UFkLIsirIJ8Cp35c0f%2Fa%2FAlTNeqfhHMwiHR6cCqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAljaUFuTaIb%2BxH7SKtwDxIolirLbtGxiYLv0eD32QKz9d8ZYNNXIFlcJYYAOPjkv25s6uGOO%2BPP3DDWDV3ZSPNRTQf9EAu03yYDhuMv%2BmsVSZ67Th8rVEy2xZw0Bo6mEU9kGDRW6idK1KP2CfeJxyfX1DMyNb%2FElUHbuTPfIKfJAbiH6qIIzW3APsIBQVyoXvqMHw%2BVE7%2B32BbEbC5lIhtKbjoq%2FDECE9CbDYKzIQSJQkX52Y8Yo%2FsYkE0XiJ2xdEN2mYE6g7TdRrt0uEtjGn8T%2BCzdixZiPaOadNEL2IKlszuGfwWidfK3GHL53hQ7AOHYmguQsyvLv4Ly3DBsrZVF3I3OgU5ayjc%2B6PALhh46vOsMMVwwKZVelG7YOs5aCXFs6jkXvlxqg5fMDPG6t1qLai%2BVDeTfgYIeohm%2FusJDhqDP3mh6Kid4agsftqkTwPdnVSZ2S19SSqWiLPJPQGiDWSwxUix3ZcVCXn0IPVOrSz%2FmhZNcBLKvt%2FntKM5FD7YexIT7uDzELtByARBOV4DWNfKHhVR4tgZkgT9MlNjpvHz2YlXF9ROGBuxUh1I%2BHN7jJkPuUFbShZd9W%2BUDeGYdxqEwSL5ONPy%2FmdTiLzfmAvbD9vihcT6RFWbVukgvYm3V7JXeOHPaI3uYwkanc1QY6pgEJebyLa3cRKONy0BZoXaDyjkEtsnUPYP7RSo9LriKsIMw0fCYbtYvofOm3Pel4ejzz5liYUk2ULuAv9XE4BKOA914Lpz0OaSA3AT%2Fcr7Ft6kIa3Zy%2BYGWV%2FbJBvtTlUaBIIeFZvlRU%2Br1luKFd2AzEWkrZDtAiddWS8sk3cxd7ruBzrO%2Fb1rgxMAHJhkRznc87uNdjPCY3BAHFdjlpqHaKZ5dA%2FH81&X-Amz-Signature=f6f5f8bb1b24112e9fa0f692197d5adb33937eeb28d965db4153cc6d4e198731&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLLQYTCE%2F20260926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260926T023120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIANVehqz9lf9C6B%2BvQd5Lq5%2FQH%2FhC%2BbMkTp2QQnwHPxlAiB8NdN0UFkLIsirIJ8Cp35c0f%2Fa%2FAlTNeqfhHMwiHR6cCqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAljaUFuTaIb%2BxH7SKtwDxIolirLbtGxiYLv0eD32QKz9d8ZYNNXIFlcJYYAOPjkv25s6uGOO%2BPP3DDWDV3ZSPNRTQf9EAu03yYDhuMv%2BmsVSZ67Th8rVEy2xZw0Bo6mEU9kGDRW6idK1KP2CfeJxyfX1DMyNb%2FElUHbuTPfIKfJAbiH6qIIzW3APsIBQVyoXvqMHw%2BVE7%2B32BbEbC5lIhtKbjoq%2FDECE9CbDYKzIQSJQkX52Y8Yo%2FsYkE0XiJ2xdEN2mYE6g7TdRrt0uEtjGn8T%2BCzdixZiPaOadNEL2IKlszuGfwWidfK3GHL53hQ7AOHYmguQsyvLv4Ly3DBsrZVF3I3OgU5ayjc%2B6PALhh46vOsMMVwwKZVelG7YOs5aCXFs6jkXvlxqg5fMDPG6t1qLai%2BVDeTfgYIeohm%2FusJDhqDP3mh6Kid4agsftqkTwPdnVSZ2S19SSqWiLPJPQGiDWSwxUix3ZcVCXn0IPVOrSz%2FmhZNcBLKvt%2FntKM5FD7YexIT7uDzELtByARBOV4DWNfKHhVR4tgZkgT9MlNjpvHz2YlXF9ROGBuxUh1I%2BHN7jJkPuUFbShZd9W%2BUDeGYdxqEwSL5ONPy%2FmdTiLzfmAvbD9vihcT6RFWbVukgvYm3V7JXeOHPaI3uYwkanc1QY6pgEJebyLa3cRKONy0BZoXaDyjkEtsnUPYP7RSo9LriKsIMw0fCYbtYvofOm3Pel4ejzz5liYUk2ULuAv9XE4BKOA914Lpz0OaSA3AT%2Fcr7Ft6kIa3Zy%2BYGWV%2FbJBvtTlUaBIIeFZvlRU%2Br1luKFd2AzEWkrZDtAiddWS8sk3cxd7ruBzrO%2Fb1rgxMAHJhkRznc87uNdjPCY3BAHFdjlpqHaKZ5dA%2FH81&X-Amz-Signature=d4e4c79ce9edc5830378afa1b2ef8b79f914c4ae5ad06c24fc166e880fab3f9f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLLQYTCE%2F20260926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260926T023120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIANVehqz9lf9C6B%2BvQd5Lq5%2FQH%2FhC%2BbMkTp2QQnwHPxlAiB8NdN0UFkLIsirIJ8Cp35c0f%2Fa%2FAlTNeqfhHMwiHR6cCqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAljaUFuTaIb%2BxH7SKtwDxIolirLbtGxiYLv0eD32QKz9d8ZYNNXIFlcJYYAOPjkv25s6uGOO%2BPP3DDWDV3ZSPNRTQf9EAu03yYDhuMv%2BmsVSZ67Th8rVEy2xZw0Bo6mEU9kGDRW6idK1KP2CfeJxyfX1DMyNb%2FElUHbuTPfIKfJAbiH6qIIzW3APsIBQVyoXvqMHw%2BVE7%2B32BbEbC5lIhtKbjoq%2FDECE9CbDYKzIQSJQkX52Y8Yo%2FsYkE0XiJ2xdEN2mYE6g7TdRrt0uEtjGn8T%2BCzdixZiPaOadNEL2IKlszuGfwWidfK3GHL53hQ7AOHYmguQsyvLv4Ly3DBsrZVF3I3OgU5ayjc%2B6PALhh46vOsMMVwwKZVelG7YOs5aCXFs6jkXvlxqg5fMDPG6t1qLai%2BVDeTfgYIeohm%2FusJDhqDP3mh6Kid4agsftqkTwPdnVSZ2S19SSqWiLPJPQGiDWSwxUix3ZcVCXn0IPVOrSz%2FmhZNcBLKvt%2FntKM5FD7YexIT7uDzELtByARBOV4DWNfKHhVR4tgZkgT9MlNjpvHz2YlXF9ROGBuxUh1I%2BHN7jJkPuUFbShZd9W%2BUDeGYdxqEwSL5ONPy%2FmdTiLzfmAvbD9vihcT6RFWbVukgvYm3V7JXeOHPaI3uYwkanc1QY6pgEJebyLa3cRKONy0BZoXaDyjkEtsnUPYP7RSo9LriKsIMw0fCYbtYvofOm3Pel4ejzz5liYUk2ULuAv9XE4BKOA914Lpz0OaSA3AT%2Fcr7Ft6kIa3Zy%2BYGWV%2FbJBvtTlUaBIIeFZvlRU%2Br1luKFd2AzEWkrZDtAiddWS8sk3cxd7ruBzrO%2Fb1rgxMAHJhkRznc87uNdjPCY3BAHFdjlpqHaKZ5dA%2FH81&X-Amz-Signature=655a235d0c96db4f5457560898b12316b6ec801a1abea80ab03288449f86c8d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
