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
fetched_at: '2026-10-05T02:59:35.029Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSLMNWOB%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025931Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDbi6QmtBvaDX8ByicF70PFUZqP7OEYjzPCfyGimVK%2FZwIhAI7H80AkMQjT4EyRurvn8hl10HNW8%2F3FgzaMYE1URY1UKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyzY5%2FwdUh%2BhEIR2dEq3AOcZ%2BlULS7UVT81e8QurTmyxb0duEgpl070CFZRNYMPIJD%2BCx39n1M8TsV4n38dBa17R8JJ5PNaWEKX%2ByPi6dM6X%2BUSAuRC7F%2FxO3PQT6HqFakBW7kmLUhBRUlHc4%2Bl1iMou1o6Ne9OT8I0JWcS1M1%2FZga0JDJ1OppiVQ4jdmXiYZQ%2F1Hy8QE5QRSgWH9wUGyVxTlG5JHdUdJm4dPW%2BH5e1M9YWcQLzquu5J6NsLQoZCopPvOnE%2BE9VODwy1PE5NRyndXX6U1DSsJFV%2BqZFK2iQH50%2BI8yd6w0PyPTI4EYt%2FEfB0GL0TBenUjOZYnlrPldBTD6BOZN4WBdKmp2lk6E7WO%2BeFbsZVKJytHmSyH7Is8dC5%2BKYdOjFySt%2BBlGQl7P6gVoq5X2fe2%2FWwyvOjjZ8qOpAKH8idNhlpYvmZOSemJoHerLbs4I4KVXMuMzyDzpZDaAudyRbfLaYDF6Bd9zy3f4E1vQdMiy4PAom3fd2448quaS%2FbT3e6xfqcP1BEx2eNZpInFkYiWWt6193Z%2BNBKceFLLvHRE6PQlLKRrMDc80qtQF67L2GSUYw0RQdPEL%2FAl3crWAoFaToGACH%2FNoHDEFhiBkPPDCGVo4iQy2eSa5nVHxSFScn9fU8lzCe%2F4vWBjqkAQV64E%2B8n8wuoeXSiYDakOHWNXnIp2qkj%2FKPaYdte2%2BA2D4xKlK482IDdsPKcaNGoAKJsimDNHfAvBHrnLn1zO1mZw2sQhTLnMg6bQ5LgSU%2F3ZZDwBUqEYBD39GK4zWeYlBSkmV3dp8AJJiN0Mrxtwz9UysOsxWR8Jr1lDEqe77ZzmdRZ2sDLmzy%2FUEK5Ds2ux0Pnt7bTEmN9SvHaigj0gijM7po&X-Amz-Signature=b665ce1401d448e482073ed3cf85465ee057829597e395dbce02c8d44d47f2e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSLMNWOB%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025931Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDbi6QmtBvaDX8ByicF70PFUZqP7OEYjzPCfyGimVK%2FZwIhAI7H80AkMQjT4EyRurvn8hl10HNW8%2F3FgzaMYE1URY1UKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyzY5%2FwdUh%2BhEIR2dEq3AOcZ%2BlULS7UVT81e8QurTmyxb0duEgpl070CFZRNYMPIJD%2BCx39n1M8TsV4n38dBa17R8JJ5PNaWEKX%2ByPi6dM6X%2BUSAuRC7F%2FxO3PQT6HqFakBW7kmLUhBRUlHc4%2Bl1iMou1o6Ne9OT8I0JWcS1M1%2FZga0JDJ1OppiVQ4jdmXiYZQ%2F1Hy8QE5QRSgWH9wUGyVxTlG5JHdUdJm4dPW%2BH5e1M9YWcQLzquu5J6NsLQoZCopPvOnE%2BE9VODwy1PE5NRyndXX6U1DSsJFV%2BqZFK2iQH50%2BI8yd6w0PyPTI4EYt%2FEfB0GL0TBenUjOZYnlrPldBTD6BOZN4WBdKmp2lk6E7WO%2BeFbsZVKJytHmSyH7Is8dC5%2BKYdOjFySt%2BBlGQl7P6gVoq5X2fe2%2FWwyvOjjZ8qOpAKH8idNhlpYvmZOSemJoHerLbs4I4KVXMuMzyDzpZDaAudyRbfLaYDF6Bd9zy3f4E1vQdMiy4PAom3fd2448quaS%2FbT3e6xfqcP1BEx2eNZpInFkYiWWt6193Z%2BNBKceFLLvHRE6PQlLKRrMDc80qtQF67L2GSUYw0RQdPEL%2FAl3crWAoFaToGACH%2FNoHDEFhiBkPPDCGVo4iQy2eSa5nVHxSFScn9fU8lzCe%2F4vWBjqkAQV64E%2B8n8wuoeXSiYDakOHWNXnIp2qkj%2FKPaYdte2%2BA2D4xKlK482IDdsPKcaNGoAKJsimDNHfAvBHrnLn1zO1mZw2sQhTLnMg6bQ5LgSU%2F3ZZDwBUqEYBD39GK4zWeYlBSkmV3dp8AJJiN0Mrxtwz9UysOsxWR8Jr1lDEqe77ZzmdRZ2sDLmzy%2FUEK5Ds2ux0Pnt7bTEmN9SvHaigj0gijM7po&X-Amz-Signature=b15b451337e7190dfe1df9dfc84090e6e1b5375ec74d5c4212a489c55455d1c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSLMNWOB%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025931Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDbi6QmtBvaDX8ByicF70PFUZqP7OEYjzPCfyGimVK%2FZwIhAI7H80AkMQjT4EyRurvn8hl10HNW8%2F3FgzaMYE1URY1UKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyzY5%2FwdUh%2BhEIR2dEq3AOcZ%2BlULS7UVT81e8QurTmyxb0duEgpl070CFZRNYMPIJD%2BCx39n1M8TsV4n38dBa17R8JJ5PNaWEKX%2ByPi6dM6X%2BUSAuRC7F%2FxO3PQT6HqFakBW7kmLUhBRUlHc4%2Bl1iMou1o6Ne9OT8I0JWcS1M1%2FZga0JDJ1OppiVQ4jdmXiYZQ%2F1Hy8QE5QRSgWH9wUGyVxTlG5JHdUdJm4dPW%2BH5e1M9YWcQLzquu5J6NsLQoZCopPvOnE%2BE9VODwy1PE5NRyndXX6U1DSsJFV%2BqZFK2iQH50%2BI8yd6w0PyPTI4EYt%2FEfB0GL0TBenUjOZYnlrPldBTD6BOZN4WBdKmp2lk6E7WO%2BeFbsZVKJytHmSyH7Is8dC5%2BKYdOjFySt%2BBlGQl7P6gVoq5X2fe2%2FWwyvOjjZ8qOpAKH8idNhlpYvmZOSemJoHerLbs4I4KVXMuMzyDzpZDaAudyRbfLaYDF6Bd9zy3f4E1vQdMiy4PAom3fd2448quaS%2FbT3e6xfqcP1BEx2eNZpInFkYiWWt6193Z%2BNBKceFLLvHRE6PQlLKRrMDc80qtQF67L2GSUYw0RQdPEL%2FAl3crWAoFaToGACH%2FNoHDEFhiBkPPDCGVo4iQy2eSa5nVHxSFScn9fU8lzCe%2F4vWBjqkAQV64E%2B8n8wuoeXSiYDakOHWNXnIp2qkj%2FKPaYdte2%2BA2D4xKlK482IDdsPKcaNGoAKJsimDNHfAvBHrnLn1zO1mZw2sQhTLnMg6bQ5LgSU%2F3ZZDwBUqEYBD39GK4zWeYlBSkmV3dp8AJJiN0Mrxtwz9UysOsxWR8Jr1lDEqe77ZzmdRZ2sDLmzy%2FUEK5Ds2ux0Pnt7bTEmN9SvHaigj0gijM7po&X-Amz-Signature=56bd64fd98886484940f74714f55f697d4c0f14af8f6b619b8dcaa01710a3cbd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSLMNWOB%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025931Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDbi6QmtBvaDX8ByicF70PFUZqP7OEYjzPCfyGimVK%2FZwIhAI7H80AkMQjT4EyRurvn8hl10HNW8%2F3FgzaMYE1URY1UKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyzY5%2FwdUh%2BhEIR2dEq3AOcZ%2BlULS7UVT81e8QurTmyxb0duEgpl070CFZRNYMPIJD%2BCx39n1M8TsV4n38dBa17R8JJ5PNaWEKX%2ByPi6dM6X%2BUSAuRC7F%2FxO3PQT6HqFakBW7kmLUhBRUlHc4%2Bl1iMou1o6Ne9OT8I0JWcS1M1%2FZga0JDJ1OppiVQ4jdmXiYZQ%2F1Hy8QE5QRSgWH9wUGyVxTlG5JHdUdJm4dPW%2BH5e1M9YWcQLzquu5J6NsLQoZCopPvOnE%2BE9VODwy1PE5NRyndXX6U1DSsJFV%2BqZFK2iQH50%2BI8yd6w0PyPTI4EYt%2FEfB0GL0TBenUjOZYnlrPldBTD6BOZN4WBdKmp2lk6E7WO%2BeFbsZVKJytHmSyH7Is8dC5%2BKYdOjFySt%2BBlGQl7P6gVoq5X2fe2%2FWwyvOjjZ8qOpAKH8idNhlpYvmZOSemJoHerLbs4I4KVXMuMzyDzpZDaAudyRbfLaYDF6Bd9zy3f4E1vQdMiy4PAom3fd2448quaS%2FbT3e6xfqcP1BEx2eNZpInFkYiWWt6193Z%2BNBKceFLLvHRE6PQlLKRrMDc80qtQF67L2GSUYw0RQdPEL%2FAl3crWAoFaToGACH%2FNoHDEFhiBkPPDCGVo4iQy2eSa5nVHxSFScn9fU8lzCe%2F4vWBjqkAQV64E%2B8n8wuoeXSiYDakOHWNXnIp2qkj%2FKPaYdte2%2BA2D4xKlK482IDdsPKcaNGoAKJsimDNHfAvBHrnLn1zO1mZw2sQhTLnMg6bQ5LgSU%2F3ZZDwBUqEYBD39GK4zWeYlBSkmV3dp8AJJiN0Mrxtwz9UysOsxWR8Jr1lDEqe77ZzmdRZ2sDLmzy%2FUEK5Ds2ux0Pnt7bTEmN9SvHaigj0gijM7po&X-Amz-Signature=c70e72a288ad53454399feb152346f48b984409b7de11dbe7973b62dfafc574f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSLMNWOB%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025931Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDbi6QmtBvaDX8ByicF70PFUZqP7OEYjzPCfyGimVK%2FZwIhAI7H80AkMQjT4EyRurvn8hl10HNW8%2F3FgzaMYE1URY1UKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyzY5%2FwdUh%2BhEIR2dEq3AOcZ%2BlULS7UVT81e8QurTmyxb0duEgpl070CFZRNYMPIJD%2BCx39n1M8TsV4n38dBa17R8JJ5PNaWEKX%2ByPi6dM6X%2BUSAuRC7F%2FxO3PQT6HqFakBW7kmLUhBRUlHc4%2Bl1iMou1o6Ne9OT8I0JWcS1M1%2FZga0JDJ1OppiVQ4jdmXiYZQ%2F1Hy8QE5QRSgWH9wUGyVxTlG5JHdUdJm4dPW%2BH5e1M9YWcQLzquu5J6NsLQoZCopPvOnE%2BE9VODwy1PE5NRyndXX6U1DSsJFV%2BqZFK2iQH50%2BI8yd6w0PyPTI4EYt%2FEfB0GL0TBenUjOZYnlrPldBTD6BOZN4WBdKmp2lk6E7WO%2BeFbsZVKJytHmSyH7Is8dC5%2BKYdOjFySt%2BBlGQl7P6gVoq5X2fe2%2FWwyvOjjZ8qOpAKH8idNhlpYvmZOSemJoHerLbs4I4KVXMuMzyDzpZDaAudyRbfLaYDF6Bd9zy3f4E1vQdMiy4PAom3fd2448quaS%2FbT3e6xfqcP1BEx2eNZpInFkYiWWt6193Z%2BNBKceFLLvHRE6PQlLKRrMDc80qtQF67L2GSUYw0RQdPEL%2FAl3crWAoFaToGACH%2FNoHDEFhiBkPPDCGVo4iQy2eSa5nVHxSFScn9fU8lzCe%2F4vWBjqkAQV64E%2B8n8wuoeXSiYDakOHWNXnIp2qkj%2FKPaYdte2%2BA2D4xKlK482IDdsPKcaNGoAKJsimDNHfAvBHrnLn1zO1mZw2sQhTLnMg6bQ5LgSU%2F3ZZDwBUqEYBD39GK4zWeYlBSkmV3dp8AJJiN0Mrxtwz9UysOsxWR8Jr1lDEqe77ZzmdRZ2sDLmzy%2FUEK5Ds2ux0Pnt7bTEmN9SvHaigj0gijM7po&X-Amz-Signature=fdef02d03a54de791cd6b96f6be822c2a2c37b201c961f2b5fae9a31df5eaf81&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSLMNWOB%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025931Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDbi6QmtBvaDX8ByicF70PFUZqP7OEYjzPCfyGimVK%2FZwIhAI7H80AkMQjT4EyRurvn8hl10HNW8%2F3FgzaMYE1URY1UKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyzY5%2FwdUh%2BhEIR2dEq3AOcZ%2BlULS7UVT81e8QurTmyxb0duEgpl070CFZRNYMPIJD%2BCx39n1M8TsV4n38dBa17R8JJ5PNaWEKX%2ByPi6dM6X%2BUSAuRC7F%2FxO3PQT6HqFakBW7kmLUhBRUlHc4%2Bl1iMou1o6Ne9OT8I0JWcS1M1%2FZga0JDJ1OppiVQ4jdmXiYZQ%2F1Hy8QE5QRSgWH9wUGyVxTlG5JHdUdJm4dPW%2BH5e1M9YWcQLzquu5J6NsLQoZCopPvOnE%2BE9VODwy1PE5NRyndXX6U1DSsJFV%2BqZFK2iQH50%2BI8yd6w0PyPTI4EYt%2FEfB0GL0TBenUjOZYnlrPldBTD6BOZN4WBdKmp2lk6E7WO%2BeFbsZVKJytHmSyH7Is8dC5%2BKYdOjFySt%2BBlGQl7P6gVoq5X2fe2%2FWwyvOjjZ8qOpAKH8idNhlpYvmZOSemJoHerLbs4I4KVXMuMzyDzpZDaAudyRbfLaYDF6Bd9zy3f4E1vQdMiy4PAom3fd2448quaS%2FbT3e6xfqcP1BEx2eNZpInFkYiWWt6193Z%2BNBKceFLLvHRE6PQlLKRrMDc80qtQF67L2GSUYw0RQdPEL%2FAl3crWAoFaToGACH%2FNoHDEFhiBkPPDCGVo4iQy2eSa5nVHxSFScn9fU8lzCe%2F4vWBjqkAQV64E%2B8n8wuoeXSiYDakOHWNXnIp2qkj%2FKPaYdte2%2BA2D4xKlK482IDdsPKcaNGoAKJsimDNHfAvBHrnLn1zO1mZw2sQhTLnMg6bQ5LgSU%2F3ZZDwBUqEYBD39GK4zWeYlBSkmV3dp8AJJiN0Mrxtwz9UysOsxWR8Jr1lDEqe77ZzmdRZ2sDLmzy%2FUEK5Ds2ux0Pnt7bTEmN9SvHaigj0gijM7po&X-Amz-Signature=220a24877d17484a9c6837b701f75d5e99f54028a0841761a18bd85e731ada45&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSLMNWOB%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025931Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDbi6QmtBvaDX8ByicF70PFUZqP7OEYjzPCfyGimVK%2FZwIhAI7H80AkMQjT4EyRurvn8hl10HNW8%2F3FgzaMYE1URY1UKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyzY5%2FwdUh%2BhEIR2dEq3AOcZ%2BlULS7UVT81e8QurTmyxb0duEgpl070CFZRNYMPIJD%2BCx39n1M8TsV4n38dBa17R8JJ5PNaWEKX%2ByPi6dM6X%2BUSAuRC7F%2FxO3PQT6HqFakBW7kmLUhBRUlHc4%2Bl1iMou1o6Ne9OT8I0JWcS1M1%2FZga0JDJ1OppiVQ4jdmXiYZQ%2F1Hy8QE5QRSgWH9wUGyVxTlG5JHdUdJm4dPW%2BH5e1M9YWcQLzquu5J6NsLQoZCopPvOnE%2BE9VODwy1PE5NRyndXX6U1DSsJFV%2BqZFK2iQH50%2BI8yd6w0PyPTI4EYt%2FEfB0GL0TBenUjOZYnlrPldBTD6BOZN4WBdKmp2lk6E7WO%2BeFbsZVKJytHmSyH7Is8dC5%2BKYdOjFySt%2BBlGQl7P6gVoq5X2fe2%2FWwyvOjjZ8qOpAKH8idNhlpYvmZOSemJoHerLbs4I4KVXMuMzyDzpZDaAudyRbfLaYDF6Bd9zy3f4E1vQdMiy4PAom3fd2448quaS%2FbT3e6xfqcP1BEx2eNZpInFkYiWWt6193Z%2BNBKceFLLvHRE6PQlLKRrMDc80qtQF67L2GSUYw0RQdPEL%2FAl3crWAoFaToGACH%2FNoHDEFhiBkPPDCGVo4iQy2eSa5nVHxSFScn9fU8lzCe%2F4vWBjqkAQV64E%2B8n8wuoeXSiYDakOHWNXnIp2qkj%2FKPaYdte2%2BA2D4xKlK482IDdsPKcaNGoAKJsimDNHfAvBHrnLn1zO1mZw2sQhTLnMg6bQ5LgSU%2F3ZZDwBUqEYBD39GK4zWeYlBSkmV3dp8AJJiN0Mrxtwz9UysOsxWR8Jr1lDEqe77ZzmdRZ2sDLmzy%2FUEK5Ds2ux0Pnt7bTEmN9SvHaigj0gijM7po&X-Amz-Signature=fa038b57c6b95bf06de7df38fe14b783bbe1ab1fc872779b3bf7742d55f67851&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSLMNWOB%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025931Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDbi6QmtBvaDX8ByicF70PFUZqP7OEYjzPCfyGimVK%2FZwIhAI7H80AkMQjT4EyRurvn8hl10HNW8%2F3FgzaMYE1URY1UKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyzY5%2FwdUh%2BhEIR2dEq3AOcZ%2BlULS7UVT81e8QurTmyxb0duEgpl070CFZRNYMPIJD%2BCx39n1M8TsV4n38dBa17R8JJ5PNaWEKX%2ByPi6dM6X%2BUSAuRC7F%2FxO3PQT6HqFakBW7kmLUhBRUlHc4%2Bl1iMou1o6Ne9OT8I0JWcS1M1%2FZga0JDJ1OppiVQ4jdmXiYZQ%2F1Hy8QE5QRSgWH9wUGyVxTlG5JHdUdJm4dPW%2BH5e1M9YWcQLzquu5J6NsLQoZCopPvOnE%2BE9VODwy1PE5NRyndXX6U1DSsJFV%2BqZFK2iQH50%2BI8yd6w0PyPTI4EYt%2FEfB0GL0TBenUjOZYnlrPldBTD6BOZN4WBdKmp2lk6E7WO%2BeFbsZVKJytHmSyH7Is8dC5%2BKYdOjFySt%2BBlGQl7P6gVoq5X2fe2%2FWwyvOjjZ8qOpAKH8idNhlpYvmZOSemJoHerLbs4I4KVXMuMzyDzpZDaAudyRbfLaYDF6Bd9zy3f4E1vQdMiy4PAom3fd2448quaS%2FbT3e6xfqcP1BEx2eNZpInFkYiWWt6193Z%2BNBKceFLLvHRE6PQlLKRrMDc80qtQF67L2GSUYw0RQdPEL%2FAl3crWAoFaToGACH%2FNoHDEFhiBkPPDCGVo4iQy2eSa5nVHxSFScn9fU8lzCe%2F4vWBjqkAQV64E%2B8n8wuoeXSiYDakOHWNXnIp2qkj%2FKPaYdte2%2BA2D4xKlK482IDdsPKcaNGoAKJsimDNHfAvBHrnLn1zO1mZw2sQhTLnMg6bQ5LgSU%2F3ZZDwBUqEYBD39GK4zWeYlBSkmV3dp8AJJiN0Mrxtwz9UysOsxWR8Jr1lDEqe77ZzmdRZ2sDLmzy%2FUEK5Ds2ux0Pnt7bTEmN9SvHaigj0gijM7po&X-Amz-Signature=7436b769a29970ed390cc60dc0ddf69e95e24a5c6393806e7831ab2a66b277ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSLMNWOB%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025931Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDbi6QmtBvaDX8ByicF70PFUZqP7OEYjzPCfyGimVK%2FZwIhAI7H80AkMQjT4EyRurvn8hl10HNW8%2F3FgzaMYE1URY1UKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyzY5%2FwdUh%2BhEIR2dEq3AOcZ%2BlULS7UVT81e8QurTmyxb0duEgpl070CFZRNYMPIJD%2BCx39n1M8TsV4n38dBa17R8JJ5PNaWEKX%2ByPi6dM6X%2BUSAuRC7F%2FxO3PQT6HqFakBW7kmLUhBRUlHc4%2Bl1iMou1o6Ne9OT8I0JWcS1M1%2FZga0JDJ1OppiVQ4jdmXiYZQ%2F1Hy8QE5QRSgWH9wUGyVxTlG5JHdUdJm4dPW%2BH5e1M9YWcQLzquu5J6NsLQoZCopPvOnE%2BE9VODwy1PE5NRyndXX6U1DSsJFV%2BqZFK2iQH50%2BI8yd6w0PyPTI4EYt%2FEfB0GL0TBenUjOZYnlrPldBTD6BOZN4WBdKmp2lk6E7WO%2BeFbsZVKJytHmSyH7Is8dC5%2BKYdOjFySt%2BBlGQl7P6gVoq5X2fe2%2FWwyvOjjZ8qOpAKH8idNhlpYvmZOSemJoHerLbs4I4KVXMuMzyDzpZDaAudyRbfLaYDF6Bd9zy3f4E1vQdMiy4PAom3fd2448quaS%2FbT3e6xfqcP1BEx2eNZpInFkYiWWt6193Z%2BNBKceFLLvHRE6PQlLKRrMDc80qtQF67L2GSUYw0RQdPEL%2FAl3crWAoFaToGACH%2FNoHDEFhiBkPPDCGVo4iQy2eSa5nVHxSFScn9fU8lzCe%2F4vWBjqkAQV64E%2B8n8wuoeXSiYDakOHWNXnIp2qkj%2FKPaYdte2%2BA2D4xKlK482IDdsPKcaNGoAKJsimDNHfAvBHrnLn1zO1mZw2sQhTLnMg6bQ5LgSU%2F3ZZDwBUqEYBD39GK4zWeYlBSkmV3dp8AJJiN0Mrxtwz9UysOsxWR8Jr1lDEqe77ZzmdRZ2sDLmzy%2FUEK5Ds2ux0Pnt7bTEmN9SvHaigj0gijM7po&X-Amz-Signature=cc4b534fa192b17b38591c0f84af100790a209f1b58c3b0c986fc8e6ba57892b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
