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
fetched_at: '2026-11-13T04:03:40.646Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5NA7YW3%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040336Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWq%2B6r%2FZrUnz4NUMP6tIpt1rpl9y%2BMWfnOx28b2MkxZAIhAN6NARyuQbfetFJe0o3cpM98OqaNJfX2fp52URMROeblKv8DCHoQABoMNjM3NDIzMTgzODA1IgwgWSGtBsDGiaPAqVYq3ANjICkSj0bcPv3pyIxecJxxggFLEKWPu8I0oyk%2FEc0af5Ti5FqakygSYMMXR9%2BlkFrkHJmN3exeHiYQ%2Bt9YrSCn%2FHpMmNzEVEE8au2qDR3hjZIuZwyI4pY%2F2fTWdBlzCq6w9RlkFJt1m0RAOdql6MPc1SqQXsgeqnTSiRp4WiKG8eOJkow8sgCsE4aUljJ9Fdp1fTSa5b%2B4l8WVcmA7YoZBXMQXhnUj9kxVz32SYHD1cnoAZIy5bh7GPGwkGIeND8UU13wFrcH6W0r6j%2BtJZifJ%2FQg45rabos8r7XS3WlGULgqFyWvzS0U8dcSBU7kk0dZQ8PXUgKPYsaBf77YRle0YS43CUK1%2B0rqhppdOBBgb2e14hW3HszWlzFvTuGpJDWv7yjq0C4LaF4aaXGvxuTfBW4thuSHnJtduHCYmxc5q3oM%2FAWVh2vcWSQwx4tUDnLGqJqbNhV6MDvv7W9OlH94%2B8ws0XRoCWzbkcLnX%2FWZVgm5A6j14Ruh12Yfcg%2BUPTj2CZsdfhAQDNBl221W3CJXNtsXU88HhqZLNCp4Sky3hp0YLRJnMg1ojZrYMX43zD21tLDS%2FYlOECsWJpCcDIT2%2BMIxdHS%2B8tp8pIvQbLtpUMFTbWeTOCKlymLT2VzCR2dnXBjqkAZ2Hk1i1wAeaKliaWfmpD0xPefSh4aEMQi8pikj3TDuqqeNy9qT8sxGCAWen8YkDwfyNnSMZfl48H1B4gujmgAjtc5xqxT8iKW%2FrKNe2YnOaKMDQQlzEszOlJg%2BgyLuHJP7JHaz9kInZf%2FvQVa7AwOGKglvbxR9Jxje09RgWhxf2UkGxJkkN4OBdhw7Q5ZQFy4Wb5ymleBwhEXniUvh%2Fq83%2F6%2FYe&X-Amz-Signature=f40310dfee99d3d4d8066c7f8dd0b658aa873d09bcd0308ec6a5250dbaa4269b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5NA7YW3%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040336Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWq%2B6r%2FZrUnz4NUMP6tIpt1rpl9y%2BMWfnOx28b2MkxZAIhAN6NARyuQbfetFJe0o3cpM98OqaNJfX2fp52URMROeblKv8DCHoQABoMNjM3NDIzMTgzODA1IgwgWSGtBsDGiaPAqVYq3ANjICkSj0bcPv3pyIxecJxxggFLEKWPu8I0oyk%2FEc0af5Ti5FqakygSYMMXR9%2BlkFrkHJmN3exeHiYQ%2Bt9YrSCn%2FHpMmNzEVEE8au2qDR3hjZIuZwyI4pY%2F2fTWdBlzCq6w9RlkFJt1m0RAOdql6MPc1SqQXsgeqnTSiRp4WiKG8eOJkow8sgCsE4aUljJ9Fdp1fTSa5b%2B4l8WVcmA7YoZBXMQXhnUj9kxVz32SYHD1cnoAZIy5bh7GPGwkGIeND8UU13wFrcH6W0r6j%2BtJZifJ%2FQg45rabos8r7XS3WlGULgqFyWvzS0U8dcSBU7kk0dZQ8PXUgKPYsaBf77YRle0YS43CUK1%2B0rqhppdOBBgb2e14hW3HszWlzFvTuGpJDWv7yjq0C4LaF4aaXGvxuTfBW4thuSHnJtduHCYmxc5q3oM%2FAWVh2vcWSQwx4tUDnLGqJqbNhV6MDvv7W9OlH94%2B8ws0XRoCWzbkcLnX%2FWZVgm5A6j14Ruh12Yfcg%2BUPTj2CZsdfhAQDNBl221W3CJXNtsXU88HhqZLNCp4Sky3hp0YLRJnMg1ojZrYMX43zD21tLDS%2FYlOECsWJpCcDIT2%2BMIxdHS%2B8tp8pIvQbLtpUMFTbWeTOCKlymLT2VzCR2dnXBjqkAZ2Hk1i1wAeaKliaWfmpD0xPefSh4aEMQi8pikj3TDuqqeNy9qT8sxGCAWen8YkDwfyNnSMZfl48H1B4gujmgAjtc5xqxT8iKW%2FrKNe2YnOaKMDQQlzEszOlJg%2BgyLuHJP7JHaz9kInZf%2FvQVa7AwOGKglvbxR9Jxje09RgWhxf2UkGxJkkN4OBdhw7Q5ZQFy4Wb5ymleBwhEXniUvh%2Fq83%2F6%2FYe&X-Amz-Signature=c40387423b2accf794b9795b0c755bec8f960ac176b280160954aab312387148&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5NA7YW3%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040336Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWq%2B6r%2FZrUnz4NUMP6tIpt1rpl9y%2BMWfnOx28b2MkxZAIhAN6NARyuQbfetFJe0o3cpM98OqaNJfX2fp52URMROeblKv8DCHoQABoMNjM3NDIzMTgzODA1IgwgWSGtBsDGiaPAqVYq3ANjICkSj0bcPv3pyIxecJxxggFLEKWPu8I0oyk%2FEc0af5Ti5FqakygSYMMXR9%2BlkFrkHJmN3exeHiYQ%2Bt9YrSCn%2FHpMmNzEVEE8au2qDR3hjZIuZwyI4pY%2F2fTWdBlzCq6w9RlkFJt1m0RAOdql6MPc1SqQXsgeqnTSiRp4WiKG8eOJkow8sgCsE4aUljJ9Fdp1fTSa5b%2B4l8WVcmA7YoZBXMQXhnUj9kxVz32SYHD1cnoAZIy5bh7GPGwkGIeND8UU13wFrcH6W0r6j%2BtJZifJ%2FQg45rabos8r7XS3WlGULgqFyWvzS0U8dcSBU7kk0dZQ8PXUgKPYsaBf77YRle0YS43CUK1%2B0rqhppdOBBgb2e14hW3HszWlzFvTuGpJDWv7yjq0C4LaF4aaXGvxuTfBW4thuSHnJtduHCYmxc5q3oM%2FAWVh2vcWSQwx4tUDnLGqJqbNhV6MDvv7W9OlH94%2B8ws0XRoCWzbkcLnX%2FWZVgm5A6j14Ruh12Yfcg%2BUPTj2CZsdfhAQDNBl221W3CJXNtsXU88HhqZLNCp4Sky3hp0YLRJnMg1ojZrYMX43zD21tLDS%2FYlOECsWJpCcDIT2%2BMIxdHS%2B8tp8pIvQbLtpUMFTbWeTOCKlymLT2VzCR2dnXBjqkAZ2Hk1i1wAeaKliaWfmpD0xPefSh4aEMQi8pikj3TDuqqeNy9qT8sxGCAWen8YkDwfyNnSMZfl48H1B4gujmgAjtc5xqxT8iKW%2FrKNe2YnOaKMDQQlzEszOlJg%2BgyLuHJP7JHaz9kInZf%2FvQVa7AwOGKglvbxR9Jxje09RgWhxf2UkGxJkkN4OBdhw7Q5ZQFy4Wb5ymleBwhEXniUvh%2Fq83%2F6%2FYe&X-Amz-Signature=eb15e81bb4849dde97c84dee366764e98f7cb66a6c9b73696530b17aad44661a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5NA7YW3%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040336Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWq%2B6r%2FZrUnz4NUMP6tIpt1rpl9y%2BMWfnOx28b2MkxZAIhAN6NARyuQbfetFJe0o3cpM98OqaNJfX2fp52URMROeblKv8DCHoQABoMNjM3NDIzMTgzODA1IgwgWSGtBsDGiaPAqVYq3ANjICkSj0bcPv3pyIxecJxxggFLEKWPu8I0oyk%2FEc0af5Ti5FqakygSYMMXR9%2BlkFrkHJmN3exeHiYQ%2Bt9YrSCn%2FHpMmNzEVEE8au2qDR3hjZIuZwyI4pY%2F2fTWdBlzCq6w9RlkFJt1m0RAOdql6MPc1SqQXsgeqnTSiRp4WiKG8eOJkow8sgCsE4aUljJ9Fdp1fTSa5b%2B4l8WVcmA7YoZBXMQXhnUj9kxVz32SYHD1cnoAZIy5bh7GPGwkGIeND8UU13wFrcH6W0r6j%2BtJZifJ%2FQg45rabos8r7XS3WlGULgqFyWvzS0U8dcSBU7kk0dZQ8PXUgKPYsaBf77YRle0YS43CUK1%2B0rqhppdOBBgb2e14hW3HszWlzFvTuGpJDWv7yjq0C4LaF4aaXGvxuTfBW4thuSHnJtduHCYmxc5q3oM%2FAWVh2vcWSQwx4tUDnLGqJqbNhV6MDvv7W9OlH94%2B8ws0XRoCWzbkcLnX%2FWZVgm5A6j14Ruh12Yfcg%2BUPTj2CZsdfhAQDNBl221W3CJXNtsXU88HhqZLNCp4Sky3hp0YLRJnMg1ojZrYMX43zD21tLDS%2FYlOECsWJpCcDIT2%2BMIxdHS%2B8tp8pIvQbLtpUMFTbWeTOCKlymLT2VzCR2dnXBjqkAZ2Hk1i1wAeaKliaWfmpD0xPefSh4aEMQi8pikj3TDuqqeNy9qT8sxGCAWen8YkDwfyNnSMZfl48H1B4gujmgAjtc5xqxT8iKW%2FrKNe2YnOaKMDQQlzEszOlJg%2BgyLuHJP7JHaz9kInZf%2FvQVa7AwOGKglvbxR9Jxje09RgWhxf2UkGxJkkN4OBdhw7Q5ZQFy4Wb5ymleBwhEXniUvh%2Fq83%2F6%2FYe&X-Amz-Signature=a66310f385cdb3e1defbe2eb010268016f65341fafd194b9b6d4859c0c0434cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5NA7YW3%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040336Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWq%2B6r%2FZrUnz4NUMP6tIpt1rpl9y%2BMWfnOx28b2MkxZAIhAN6NARyuQbfetFJe0o3cpM98OqaNJfX2fp52URMROeblKv8DCHoQABoMNjM3NDIzMTgzODA1IgwgWSGtBsDGiaPAqVYq3ANjICkSj0bcPv3pyIxecJxxggFLEKWPu8I0oyk%2FEc0af5Ti5FqakygSYMMXR9%2BlkFrkHJmN3exeHiYQ%2Bt9YrSCn%2FHpMmNzEVEE8au2qDR3hjZIuZwyI4pY%2F2fTWdBlzCq6w9RlkFJt1m0RAOdql6MPc1SqQXsgeqnTSiRp4WiKG8eOJkow8sgCsE4aUljJ9Fdp1fTSa5b%2B4l8WVcmA7YoZBXMQXhnUj9kxVz32SYHD1cnoAZIy5bh7GPGwkGIeND8UU13wFrcH6W0r6j%2BtJZifJ%2FQg45rabos8r7XS3WlGULgqFyWvzS0U8dcSBU7kk0dZQ8PXUgKPYsaBf77YRle0YS43CUK1%2B0rqhppdOBBgb2e14hW3HszWlzFvTuGpJDWv7yjq0C4LaF4aaXGvxuTfBW4thuSHnJtduHCYmxc5q3oM%2FAWVh2vcWSQwx4tUDnLGqJqbNhV6MDvv7W9OlH94%2B8ws0XRoCWzbkcLnX%2FWZVgm5A6j14Ruh12Yfcg%2BUPTj2CZsdfhAQDNBl221W3CJXNtsXU88HhqZLNCp4Sky3hp0YLRJnMg1ojZrYMX43zD21tLDS%2FYlOECsWJpCcDIT2%2BMIxdHS%2B8tp8pIvQbLtpUMFTbWeTOCKlymLT2VzCR2dnXBjqkAZ2Hk1i1wAeaKliaWfmpD0xPefSh4aEMQi8pikj3TDuqqeNy9qT8sxGCAWen8YkDwfyNnSMZfl48H1B4gujmgAjtc5xqxT8iKW%2FrKNe2YnOaKMDQQlzEszOlJg%2BgyLuHJP7JHaz9kInZf%2FvQVa7AwOGKglvbxR9Jxje09RgWhxf2UkGxJkkN4OBdhw7Q5ZQFy4Wb5ymleBwhEXniUvh%2Fq83%2F6%2FYe&X-Amz-Signature=77ef149da983f479795cfbcba6c79fcbf04ece95c0e24cb8302cf198da523401&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5NA7YW3%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040336Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWq%2B6r%2FZrUnz4NUMP6tIpt1rpl9y%2BMWfnOx28b2MkxZAIhAN6NARyuQbfetFJe0o3cpM98OqaNJfX2fp52URMROeblKv8DCHoQABoMNjM3NDIzMTgzODA1IgwgWSGtBsDGiaPAqVYq3ANjICkSj0bcPv3pyIxecJxxggFLEKWPu8I0oyk%2FEc0af5Ti5FqakygSYMMXR9%2BlkFrkHJmN3exeHiYQ%2Bt9YrSCn%2FHpMmNzEVEE8au2qDR3hjZIuZwyI4pY%2F2fTWdBlzCq6w9RlkFJt1m0RAOdql6MPc1SqQXsgeqnTSiRp4WiKG8eOJkow8sgCsE4aUljJ9Fdp1fTSa5b%2B4l8WVcmA7YoZBXMQXhnUj9kxVz32SYHD1cnoAZIy5bh7GPGwkGIeND8UU13wFrcH6W0r6j%2BtJZifJ%2FQg45rabos8r7XS3WlGULgqFyWvzS0U8dcSBU7kk0dZQ8PXUgKPYsaBf77YRle0YS43CUK1%2B0rqhppdOBBgb2e14hW3HszWlzFvTuGpJDWv7yjq0C4LaF4aaXGvxuTfBW4thuSHnJtduHCYmxc5q3oM%2FAWVh2vcWSQwx4tUDnLGqJqbNhV6MDvv7W9OlH94%2B8ws0XRoCWzbkcLnX%2FWZVgm5A6j14Ruh12Yfcg%2BUPTj2CZsdfhAQDNBl221W3CJXNtsXU88HhqZLNCp4Sky3hp0YLRJnMg1ojZrYMX43zD21tLDS%2FYlOECsWJpCcDIT2%2BMIxdHS%2B8tp8pIvQbLtpUMFTbWeTOCKlymLT2VzCR2dnXBjqkAZ2Hk1i1wAeaKliaWfmpD0xPefSh4aEMQi8pikj3TDuqqeNy9qT8sxGCAWen8YkDwfyNnSMZfl48H1B4gujmgAjtc5xqxT8iKW%2FrKNe2YnOaKMDQQlzEszOlJg%2BgyLuHJP7JHaz9kInZf%2FvQVa7AwOGKglvbxR9Jxje09RgWhxf2UkGxJkkN4OBdhw7Q5ZQFy4Wb5ymleBwhEXniUvh%2Fq83%2F6%2FYe&X-Amz-Signature=5b7806e871a320eba951dcf3672a12627cbe134d0d2eeb87a7e9ae1140ac1bf0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5NA7YW3%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040336Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWq%2B6r%2FZrUnz4NUMP6tIpt1rpl9y%2BMWfnOx28b2MkxZAIhAN6NARyuQbfetFJe0o3cpM98OqaNJfX2fp52URMROeblKv8DCHoQABoMNjM3NDIzMTgzODA1IgwgWSGtBsDGiaPAqVYq3ANjICkSj0bcPv3pyIxecJxxggFLEKWPu8I0oyk%2FEc0af5Ti5FqakygSYMMXR9%2BlkFrkHJmN3exeHiYQ%2Bt9YrSCn%2FHpMmNzEVEE8au2qDR3hjZIuZwyI4pY%2F2fTWdBlzCq6w9RlkFJt1m0RAOdql6MPc1SqQXsgeqnTSiRp4WiKG8eOJkow8sgCsE4aUljJ9Fdp1fTSa5b%2B4l8WVcmA7YoZBXMQXhnUj9kxVz32SYHD1cnoAZIy5bh7GPGwkGIeND8UU13wFrcH6W0r6j%2BtJZifJ%2FQg45rabos8r7XS3WlGULgqFyWvzS0U8dcSBU7kk0dZQ8PXUgKPYsaBf77YRle0YS43CUK1%2B0rqhppdOBBgb2e14hW3HszWlzFvTuGpJDWv7yjq0C4LaF4aaXGvxuTfBW4thuSHnJtduHCYmxc5q3oM%2FAWVh2vcWSQwx4tUDnLGqJqbNhV6MDvv7W9OlH94%2B8ws0XRoCWzbkcLnX%2FWZVgm5A6j14Ruh12Yfcg%2BUPTj2CZsdfhAQDNBl221W3CJXNtsXU88HhqZLNCp4Sky3hp0YLRJnMg1ojZrYMX43zD21tLDS%2FYlOECsWJpCcDIT2%2BMIxdHS%2B8tp8pIvQbLtpUMFTbWeTOCKlymLT2VzCR2dnXBjqkAZ2Hk1i1wAeaKliaWfmpD0xPefSh4aEMQi8pikj3TDuqqeNy9qT8sxGCAWen8YkDwfyNnSMZfl48H1B4gujmgAjtc5xqxT8iKW%2FrKNe2YnOaKMDQQlzEszOlJg%2BgyLuHJP7JHaz9kInZf%2FvQVa7AwOGKglvbxR9Jxje09RgWhxf2UkGxJkkN4OBdhw7Q5ZQFy4Wb5ymleBwhEXniUvh%2Fq83%2F6%2FYe&X-Amz-Signature=306db211d1569408a9fc4edd4548c58a0f2267f70e1c939a58fe631f5ab46be4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5NA7YW3%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040336Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWq%2B6r%2FZrUnz4NUMP6tIpt1rpl9y%2BMWfnOx28b2MkxZAIhAN6NARyuQbfetFJe0o3cpM98OqaNJfX2fp52URMROeblKv8DCHoQABoMNjM3NDIzMTgzODA1IgwgWSGtBsDGiaPAqVYq3ANjICkSj0bcPv3pyIxecJxxggFLEKWPu8I0oyk%2FEc0af5Ti5FqakygSYMMXR9%2BlkFrkHJmN3exeHiYQ%2Bt9YrSCn%2FHpMmNzEVEE8au2qDR3hjZIuZwyI4pY%2F2fTWdBlzCq6w9RlkFJt1m0RAOdql6MPc1SqQXsgeqnTSiRp4WiKG8eOJkow8sgCsE4aUljJ9Fdp1fTSa5b%2B4l8WVcmA7YoZBXMQXhnUj9kxVz32SYHD1cnoAZIy5bh7GPGwkGIeND8UU13wFrcH6W0r6j%2BtJZifJ%2FQg45rabos8r7XS3WlGULgqFyWvzS0U8dcSBU7kk0dZQ8PXUgKPYsaBf77YRle0YS43CUK1%2B0rqhppdOBBgb2e14hW3HszWlzFvTuGpJDWv7yjq0C4LaF4aaXGvxuTfBW4thuSHnJtduHCYmxc5q3oM%2FAWVh2vcWSQwx4tUDnLGqJqbNhV6MDvv7W9OlH94%2B8ws0XRoCWzbkcLnX%2FWZVgm5A6j14Ruh12Yfcg%2BUPTj2CZsdfhAQDNBl221W3CJXNtsXU88HhqZLNCp4Sky3hp0YLRJnMg1ojZrYMX43zD21tLDS%2FYlOECsWJpCcDIT2%2BMIxdHS%2B8tp8pIvQbLtpUMFTbWeTOCKlymLT2VzCR2dnXBjqkAZ2Hk1i1wAeaKliaWfmpD0xPefSh4aEMQi8pikj3TDuqqeNy9qT8sxGCAWen8YkDwfyNnSMZfl48H1B4gujmgAjtc5xqxT8iKW%2FrKNe2YnOaKMDQQlzEszOlJg%2BgyLuHJP7JHaz9kInZf%2FvQVa7AwOGKglvbxR9Jxje09RgWhxf2UkGxJkkN4OBdhw7Q5ZQFy4Wb5ymleBwhEXniUvh%2Fq83%2F6%2FYe&X-Amz-Signature=6033044b064995a310c024465d9fc85034c1c6d16283efcedfdf61b7dea575d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5NA7YW3%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040336Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWq%2B6r%2FZrUnz4NUMP6tIpt1rpl9y%2BMWfnOx28b2MkxZAIhAN6NARyuQbfetFJe0o3cpM98OqaNJfX2fp52URMROeblKv8DCHoQABoMNjM3NDIzMTgzODA1IgwgWSGtBsDGiaPAqVYq3ANjICkSj0bcPv3pyIxecJxxggFLEKWPu8I0oyk%2FEc0af5Ti5FqakygSYMMXR9%2BlkFrkHJmN3exeHiYQ%2Bt9YrSCn%2FHpMmNzEVEE8au2qDR3hjZIuZwyI4pY%2F2fTWdBlzCq6w9RlkFJt1m0RAOdql6MPc1SqQXsgeqnTSiRp4WiKG8eOJkow8sgCsE4aUljJ9Fdp1fTSa5b%2B4l8WVcmA7YoZBXMQXhnUj9kxVz32SYHD1cnoAZIy5bh7GPGwkGIeND8UU13wFrcH6W0r6j%2BtJZifJ%2FQg45rabos8r7XS3WlGULgqFyWvzS0U8dcSBU7kk0dZQ8PXUgKPYsaBf77YRle0YS43CUK1%2B0rqhppdOBBgb2e14hW3HszWlzFvTuGpJDWv7yjq0C4LaF4aaXGvxuTfBW4thuSHnJtduHCYmxc5q3oM%2FAWVh2vcWSQwx4tUDnLGqJqbNhV6MDvv7W9OlH94%2B8ws0XRoCWzbkcLnX%2FWZVgm5A6j14Ruh12Yfcg%2BUPTj2CZsdfhAQDNBl221W3CJXNtsXU88HhqZLNCp4Sky3hp0YLRJnMg1ojZrYMX43zD21tLDS%2FYlOECsWJpCcDIT2%2BMIxdHS%2B8tp8pIvQbLtpUMFTbWeTOCKlymLT2VzCR2dnXBjqkAZ2Hk1i1wAeaKliaWfmpD0xPefSh4aEMQi8pikj3TDuqqeNy9qT8sxGCAWen8YkDwfyNnSMZfl48H1B4gujmgAjtc5xqxT8iKW%2FrKNe2YnOaKMDQQlzEszOlJg%2BgyLuHJP7JHaz9kInZf%2FvQVa7AwOGKglvbxR9Jxje09RgWhxf2UkGxJkkN4OBdhw7Q5ZQFy4Wb5ymleBwhEXniUvh%2Fq83%2F6%2FYe&X-Amz-Signature=3279b2de8a1623676b80484cc99560c2d23e1a618af9369929f69b9921e6dedc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
