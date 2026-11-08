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
fetched_at: '2026-11-08T03:18:56.486Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NLWYHKK%2F20261108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261108T031853Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDklhNfmLxiBG2N%2F2lVdWFWmUlindz6PfOdzyu0bsKhXQIgeOD20aIZXy6VV%2FmjT2snZmFclqbS0h6RH6fNSx9UZ80q%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDB%2FZPyG2HIh3SAoBpyrcA%2BM1DjRJWs9oq%2B9hckQr0AWWOO22D0P%2Bxv9uamwEoS8UhT%2Fi5m1m5dwtcevGqICRPUBFh%2FDhM9gsx%2FMZfoBnpqtJHMxX5MoXs%2FRRpNmGUVVKMlZ4xhcb%2BGJ5YO9TCgb9emOoliocIcM%2B4dzrWBZW3AgdsjpUYvLcN8sdvOvPMarHj2zIIzQ4X1Awau9YT57%2BeciUnZTf7d%2F1R31xKqAgEwKjY7d7H%2BcMHTk6I9oo0%2F6uNe9ok2s7QppqEnd%2FZf0MgKTvzZFxMrtkisoIcDLyZ2%2BYQ8U4gefWBe8p9ucH9%2FqG%2FTwPbeQFslQxWO1%2FSbjHY4TZECfLL5IM0Z5fFFx7lJEQBvF%2FNGnWjNB2ut25h%2FQddKViRm1p7T2wGfyCb3UTDXbKEEt%2FXeuiuYkh0kdeTbpvyO%2FXqp8al8%2Bwk1Se7jecsEVC%2Bp4NBMGXOVmyYJEYNIdokkGg7zGsjLVeocba3iYyTK1VPitTN%2BDaHf5INFZsjB0z6TSNTvdrsF1yEzm6mrc4XFmgsy%2FbXjvT2MbEidgoA0IeVOncIO%2FMLdtuBATISPmSAYm3ESadb1Zy9ZaHabwoYhMogWnPcpl%2F11AKZ%2BzCu2%2BE%2FO2OpUbnRIsvfrQI80T71hsKPNe7krtbMOWzv9cGOqUBs3FWK7N0M3quklR1biz5ShuvTdL5QkHwgy7t3WRAQ3lhid7VNvc9f9cG7iqDvqBlRzGEQSQ%2FL9396slHPXcnrLNvKc%2BHBZ5VQa6Smm2Q%2BtJm863tqr6oDlfA5KHUNmVz%2FNAowfkg21fIYkT10YRD8yORiYGlSdKQnJg4ieDifMpxKXNGyRWEmuXrxtUgwpG%2BpaSm0PvL8RjVjyWE17rX8arSZk1I&X-Amz-Signature=f545967ca23bec528d5b488bcaa0722101701af884eda46d615e583cc9987b83&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NLWYHKK%2F20261108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261108T031853Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDklhNfmLxiBG2N%2F2lVdWFWmUlindz6PfOdzyu0bsKhXQIgeOD20aIZXy6VV%2FmjT2snZmFclqbS0h6RH6fNSx9UZ80q%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDB%2FZPyG2HIh3SAoBpyrcA%2BM1DjRJWs9oq%2B9hckQr0AWWOO22D0P%2Bxv9uamwEoS8UhT%2Fi5m1m5dwtcevGqICRPUBFh%2FDhM9gsx%2FMZfoBnpqtJHMxX5MoXs%2FRRpNmGUVVKMlZ4xhcb%2BGJ5YO9TCgb9emOoliocIcM%2B4dzrWBZW3AgdsjpUYvLcN8sdvOvPMarHj2zIIzQ4X1Awau9YT57%2BeciUnZTf7d%2F1R31xKqAgEwKjY7d7H%2BcMHTk6I9oo0%2F6uNe9ok2s7QppqEnd%2FZf0MgKTvzZFxMrtkisoIcDLyZ2%2BYQ8U4gefWBe8p9ucH9%2FqG%2FTwPbeQFslQxWO1%2FSbjHY4TZECfLL5IM0Z5fFFx7lJEQBvF%2FNGnWjNB2ut25h%2FQddKViRm1p7T2wGfyCb3UTDXbKEEt%2FXeuiuYkh0kdeTbpvyO%2FXqp8al8%2Bwk1Se7jecsEVC%2Bp4NBMGXOVmyYJEYNIdokkGg7zGsjLVeocba3iYyTK1VPitTN%2BDaHf5INFZsjB0z6TSNTvdrsF1yEzm6mrc4XFmgsy%2FbXjvT2MbEidgoA0IeVOncIO%2FMLdtuBATISPmSAYm3ESadb1Zy9ZaHabwoYhMogWnPcpl%2F11AKZ%2BzCu2%2BE%2FO2OpUbnRIsvfrQI80T71hsKPNe7krtbMOWzv9cGOqUBs3FWK7N0M3quklR1biz5ShuvTdL5QkHwgy7t3WRAQ3lhid7VNvc9f9cG7iqDvqBlRzGEQSQ%2FL9396slHPXcnrLNvKc%2BHBZ5VQa6Smm2Q%2BtJm863tqr6oDlfA5KHUNmVz%2FNAowfkg21fIYkT10YRD8yORiYGlSdKQnJg4ieDifMpxKXNGyRWEmuXrxtUgwpG%2BpaSm0PvL8RjVjyWE17rX8arSZk1I&X-Amz-Signature=9c3498b54e41c1fb5d3855deee0a0c653a0a4f2375edb113cb74b0cf4c3280ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NLWYHKK%2F20261108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261108T031853Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDklhNfmLxiBG2N%2F2lVdWFWmUlindz6PfOdzyu0bsKhXQIgeOD20aIZXy6VV%2FmjT2snZmFclqbS0h6RH6fNSx9UZ80q%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDB%2FZPyG2HIh3SAoBpyrcA%2BM1DjRJWs9oq%2B9hckQr0AWWOO22D0P%2Bxv9uamwEoS8UhT%2Fi5m1m5dwtcevGqICRPUBFh%2FDhM9gsx%2FMZfoBnpqtJHMxX5MoXs%2FRRpNmGUVVKMlZ4xhcb%2BGJ5YO9TCgb9emOoliocIcM%2B4dzrWBZW3AgdsjpUYvLcN8sdvOvPMarHj2zIIzQ4X1Awau9YT57%2BeciUnZTf7d%2F1R31xKqAgEwKjY7d7H%2BcMHTk6I9oo0%2F6uNe9ok2s7QppqEnd%2FZf0MgKTvzZFxMrtkisoIcDLyZ2%2BYQ8U4gefWBe8p9ucH9%2FqG%2FTwPbeQFslQxWO1%2FSbjHY4TZECfLL5IM0Z5fFFx7lJEQBvF%2FNGnWjNB2ut25h%2FQddKViRm1p7T2wGfyCb3UTDXbKEEt%2FXeuiuYkh0kdeTbpvyO%2FXqp8al8%2Bwk1Se7jecsEVC%2Bp4NBMGXOVmyYJEYNIdokkGg7zGsjLVeocba3iYyTK1VPitTN%2BDaHf5INFZsjB0z6TSNTvdrsF1yEzm6mrc4XFmgsy%2FbXjvT2MbEidgoA0IeVOncIO%2FMLdtuBATISPmSAYm3ESadb1Zy9ZaHabwoYhMogWnPcpl%2F11AKZ%2BzCu2%2BE%2FO2OpUbnRIsvfrQI80T71hsKPNe7krtbMOWzv9cGOqUBs3FWK7N0M3quklR1biz5ShuvTdL5QkHwgy7t3WRAQ3lhid7VNvc9f9cG7iqDvqBlRzGEQSQ%2FL9396slHPXcnrLNvKc%2BHBZ5VQa6Smm2Q%2BtJm863tqr6oDlfA5KHUNmVz%2FNAowfkg21fIYkT10YRD8yORiYGlSdKQnJg4ieDifMpxKXNGyRWEmuXrxtUgwpG%2BpaSm0PvL8RjVjyWE17rX8arSZk1I&X-Amz-Signature=c384a4d2a08c9dbfbaa7db0e11c732e451a64f9183e4a28faf2a97c77bfc77fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NLWYHKK%2F20261108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261108T031853Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDklhNfmLxiBG2N%2F2lVdWFWmUlindz6PfOdzyu0bsKhXQIgeOD20aIZXy6VV%2FmjT2snZmFclqbS0h6RH6fNSx9UZ80q%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDB%2FZPyG2HIh3SAoBpyrcA%2BM1DjRJWs9oq%2B9hckQr0AWWOO22D0P%2Bxv9uamwEoS8UhT%2Fi5m1m5dwtcevGqICRPUBFh%2FDhM9gsx%2FMZfoBnpqtJHMxX5MoXs%2FRRpNmGUVVKMlZ4xhcb%2BGJ5YO9TCgb9emOoliocIcM%2B4dzrWBZW3AgdsjpUYvLcN8sdvOvPMarHj2zIIzQ4X1Awau9YT57%2BeciUnZTf7d%2F1R31xKqAgEwKjY7d7H%2BcMHTk6I9oo0%2F6uNe9ok2s7QppqEnd%2FZf0MgKTvzZFxMrtkisoIcDLyZ2%2BYQ8U4gefWBe8p9ucH9%2FqG%2FTwPbeQFslQxWO1%2FSbjHY4TZECfLL5IM0Z5fFFx7lJEQBvF%2FNGnWjNB2ut25h%2FQddKViRm1p7T2wGfyCb3UTDXbKEEt%2FXeuiuYkh0kdeTbpvyO%2FXqp8al8%2Bwk1Se7jecsEVC%2Bp4NBMGXOVmyYJEYNIdokkGg7zGsjLVeocba3iYyTK1VPitTN%2BDaHf5INFZsjB0z6TSNTvdrsF1yEzm6mrc4XFmgsy%2FbXjvT2MbEidgoA0IeVOncIO%2FMLdtuBATISPmSAYm3ESadb1Zy9ZaHabwoYhMogWnPcpl%2F11AKZ%2BzCu2%2BE%2FO2OpUbnRIsvfrQI80T71hsKPNe7krtbMOWzv9cGOqUBs3FWK7N0M3quklR1biz5ShuvTdL5QkHwgy7t3WRAQ3lhid7VNvc9f9cG7iqDvqBlRzGEQSQ%2FL9396slHPXcnrLNvKc%2BHBZ5VQa6Smm2Q%2BtJm863tqr6oDlfA5KHUNmVz%2FNAowfkg21fIYkT10YRD8yORiYGlSdKQnJg4ieDifMpxKXNGyRWEmuXrxtUgwpG%2BpaSm0PvL8RjVjyWE17rX8arSZk1I&X-Amz-Signature=09e13a922c4280de5cf907df75416e9921e12bfc59a23ff4f51a9070c187bd2f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NLWYHKK%2F20261108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261108T031853Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDklhNfmLxiBG2N%2F2lVdWFWmUlindz6PfOdzyu0bsKhXQIgeOD20aIZXy6VV%2FmjT2snZmFclqbS0h6RH6fNSx9UZ80q%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDB%2FZPyG2HIh3SAoBpyrcA%2BM1DjRJWs9oq%2B9hckQr0AWWOO22D0P%2Bxv9uamwEoS8UhT%2Fi5m1m5dwtcevGqICRPUBFh%2FDhM9gsx%2FMZfoBnpqtJHMxX5MoXs%2FRRpNmGUVVKMlZ4xhcb%2BGJ5YO9TCgb9emOoliocIcM%2B4dzrWBZW3AgdsjpUYvLcN8sdvOvPMarHj2zIIzQ4X1Awau9YT57%2BeciUnZTf7d%2F1R31xKqAgEwKjY7d7H%2BcMHTk6I9oo0%2F6uNe9ok2s7QppqEnd%2FZf0MgKTvzZFxMrtkisoIcDLyZ2%2BYQ8U4gefWBe8p9ucH9%2FqG%2FTwPbeQFslQxWO1%2FSbjHY4TZECfLL5IM0Z5fFFx7lJEQBvF%2FNGnWjNB2ut25h%2FQddKViRm1p7T2wGfyCb3UTDXbKEEt%2FXeuiuYkh0kdeTbpvyO%2FXqp8al8%2Bwk1Se7jecsEVC%2Bp4NBMGXOVmyYJEYNIdokkGg7zGsjLVeocba3iYyTK1VPitTN%2BDaHf5INFZsjB0z6TSNTvdrsF1yEzm6mrc4XFmgsy%2FbXjvT2MbEidgoA0IeVOncIO%2FMLdtuBATISPmSAYm3ESadb1Zy9ZaHabwoYhMogWnPcpl%2F11AKZ%2BzCu2%2BE%2FO2OpUbnRIsvfrQI80T71hsKPNe7krtbMOWzv9cGOqUBs3FWK7N0M3quklR1biz5ShuvTdL5QkHwgy7t3WRAQ3lhid7VNvc9f9cG7iqDvqBlRzGEQSQ%2FL9396slHPXcnrLNvKc%2BHBZ5VQa6Smm2Q%2BtJm863tqr6oDlfA5KHUNmVz%2FNAowfkg21fIYkT10YRD8yORiYGlSdKQnJg4ieDifMpxKXNGyRWEmuXrxtUgwpG%2BpaSm0PvL8RjVjyWE17rX8arSZk1I&X-Amz-Signature=39bc64bd686809fb1437164219d409e0b31c8a6fd4dd7ce54dab2af53c8cd751&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NLWYHKK%2F20261108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261108T031853Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDklhNfmLxiBG2N%2F2lVdWFWmUlindz6PfOdzyu0bsKhXQIgeOD20aIZXy6VV%2FmjT2snZmFclqbS0h6RH6fNSx9UZ80q%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDB%2FZPyG2HIh3SAoBpyrcA%2BM1DjRJWs9oq%2B9hckQr0AWWOO22D0P%2Bxv9uamwEoS8UhT%2Fi5m1m5dwtcevGqICRPUBFh%2FDhM9gsx%2FMZfoBnpqtJHMxX5MoXs%2FRRpNmGUVVKMlZ4xhcb%2BGJ5YO9TCgb9emOoliocIcM%2B4dzrWBZW3AgdsjpUYvLcN8sdvOvPMarHj2zIIzQ4X1Awau9YT57%2BeciUnZTf7d%2F1R31xKqAgEwKjY7d7H%2BcMHTk6I9oo0%2F6uNe9ok2s7QppqEnd%2FZf0MgKTvzZFxMrtkisoIcDLyZ2%2BYQ8U4gefWBe8p9ucH9%2FqG%2FTwPbeQFslQxWO1%2FSbjHY4TZECfLL5IM0Z5fFFx7lJEQBvF%2FNGnWjNB2ut25h%2FQddKViRm1p7T2wGfyCb3UTDXbKEEt%2FXeuiuYkh0kdeTbpvyO%2FXqp8al8%2Bwk1Se7jecsEVC%2Bp4NBMGXOVmyYJEYNIdokkGg7zGsjLVeocba3iYyTK1VPitTN%2BDaHf5INFZsjB0z6TSNTvdrsF1yEzm6mrc4XFmgsy%2FbXjvT2MbEidgoA0IeVOncIO%2FMLdtuBATISPmSAYm3ESadb1Zy9ZaHabwoYhMogWnPcpl%2F11AKZ%2BzCu2%2BE%2FO2OpUbnRIsvfrQI80T71hsKPNe7krtbMOWzv9cGOqUBs3FWK7N0M3quklR1biz5ShuvTdL5QkHwgy7t3WRAQ3lhid7VNvc9f9cG7iqDvqBlRzGEQSQ%2FL9396slHPXcnrLNvKc%2BHBZ5VQa6Smm2Q%2BtJm863tqr6oDlfA5KHUNmVz%2FNAowfkg21fIYkT10YRD8yORiYGlSdKQnJg4ieDifMpxKXNGyRWEmuXrxtUgwpG%2BpaSm0PvL8RjVjyWE17rX8arSZk1I&X-Amz-Signature=2498cc2c29ced4671c6f044aa6b9cf4e571a74c6ab566d77d705b61c5e1c7781&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NLWYHKK%2F20261108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261108T031853Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDklhNfmLxiBG2N%2F2lVdWFWmUlindz6PfOdzyu0bsKhXQIgeOD20aIZXy6VV%2FmjT2snZmFclqbS0h6RH6fNSx9UZ80q%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDB%2FZPyG2HIh3SAoBpyrcA%2BM1DjRJWs9oq%2B9hckQr0AWWOO22D0P%2Bxv9uamwEoS8UhT%2Fi5m1m5dwtcevGqICRPUBFh%2FDhM9gsx%2FMZfoBnpqtJHMxX5MoXs%2FRRpNmGUVVKMlZ4xhcb%2BGJ5YO9TCgb9emOoliocIcM%2B4dzrWBZW3AgdsjpUYvLcN8sdvOvPMarHj2zIIzQ4X1Awau9YT57%2BeciUnZTf7d%2F1R31xKqAgEwKjY7d7H%2BcMHTk6I9oo0%2F6uNe9ok2s7QppqEnd%2FZf0MgKTvzZFxMrtkisoIcDLyZ2%2BYQ8U4gefWBe8p9ucH9%2FqG%2FTwPbeQFslQxWO1%2FSbjHY4TZECfLL5IM0Z5fFFx7lJEQBvF%2FNGnWjNB2ut25h%2FQddKViRm1p7T2wGfyCb3UTDXbKEEt%2FXeuiuYkh0kdeTbpvyO%2FXqp8al8%2Bwk1Se7jecsEVC%2Bp4NBMGXOVmyYJEYNIdokkGg7zGsjLVeocba3iYyTK1VPitTN%2BDaHf5INFZsjB0z6TSNTvdrsF1yEzm6mrc4XFmgsy%2FbXjvT2MbEidgoA0IeVOncIO%2FMLdtuBATISPmSAYm3ESadb1Zy9ZaHabwoYhMogWnPcpl%2F11AKZ%2BzCu2%2BE%2FO2OpUbnRIsvfrQI80T71hsKPNe7krtbMOWzv9cGOqUBs3FWK7N0M3quklR1biz5ShuvTdL5QkHwgy7t3WRAQ3lhid7VNvc9f9cG7iqDvqBlRzGEQSQ%2FL9396slHPXcnrLNvKc%2BHBZ5VQa6Smm2Q%2BtJm863tqr6oDlfA5KHUNmVz%2FNAowfkg21fIYkT10YRD8yORiYGlSdKQnJg4ieDifMpxKXNGyRWEmuXrxtUgwpG%2BpaSm0PvL8RjVjyWE17rX8arSZk1I&X-Amz-Signature=e92408c777346473ac4f20be87a437228f226e81e247ead34c70884a8ef7f412&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NLWYHKK%2F20261108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261108T031853Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDklhNfmLxiBG2N%2F2lVdWFWmUlindz6PfOdzyu0bsKhXQIgeOD20aIZXy6VV%2FmjT2snZmFclqbS0h6RH6fNSx9UZ80q%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDB%2FZPyG2HIh3SAoBpyrcA%2BM1DjRJWs9oq%2B9hckQr0AWWOO22D0P%2Bxv9uamwEoS8UhT%2Fi5m1m5dwtcevGqICRPUBFh%2FDhM9gsx%2FMZfoBnpqtJHMxX5MoXs%2FRRpNmGUVVKMlZ4xhcb%2BGJ5YO9TCgb9emOoliocIcM%2B4dzrWBZW3AgdsjpUYvLcN8sdvOvPMarHj2zIIzQ4X1Awau9YT57%2BeciUnZTf7d%2F1R31xKqAgEwKjY7d7H%2BcMHTk6I9oo0%2F6uNe9ok2s7QppqEnd%2FZf0MgKTvzZFxMrtkisoIcDLyZ2%2BYQ8U4gefWBe8p9ucH9%2FqG%2FTwPbeQFslQxWO1%2FSbjHY4TZECfLL5IM0Z5fFFx7lJEQBvF%2FNGnWjNB2ut25h%2FQddKViRm1p7T2wGfyCb3UTDXbKEEt%2FXeuiuYkh0kdeTbpvyO%2FXqp8al8%2Bwk1Se7jecsEVC%2Bp4NBMGXOVmyYJEYNIdokkGg7zGsjLVeocba3iYyTK1VPitTN%2BDaHf5INFZsjB0z6TSNTvdrsF1yEzm6mrc4XFmgsy%2FbXjvT2MbEidgoA0IeVOncIO%2FMLdtuBATISPmSAYm3ESadb1Zy9ZaHabwoYhMogWnPcpl%2F11AKZ%2BzCu2%2BE%2FO2OpUbnRIsvfrQI80T71hsKPNe7krtbMOWzv9cGOqUBs3FWK7N0M3quklR1biz5ShuvTdL5QkHwgy7t3WRAQ3lhid7VNvc9f9cG7iqDvqBlRzGEQSQ%2FL9396slHPXcnrLNvKc%2BHBZ5VQa6Smm2Q%2BtJm863tqr6oDlfA5KHUNmVz%2FNAowfkg21fIYkT10YRD8yORiYGlSdKQnJg4ieDifMpxKXNGyRWEmuXrxtUgwpG%2BpaSm0PvL8RjVjyWE17rX8arSZk1I&X-Amz-Signature=21d308874ae24f64be28e81706504be76a855907776f42b786d6b840a8ca454e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NLWYHKK%2F20261108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261108T031853Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDklhNfmLxiBG2N%2F2lVdWFWmUlindz6PfOdzyu0bsKhXQIgeOD20aIZXy6VV%2FmjT2snZmFclqbS0h6RH6fNSx9UZ80q%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDB%2FZPyG2HIh3SAoBpyrcA%2BM1DjRJWs9oq%2B9hckQr0AWWOO22D0P%2Bxv9uamwEoS8UhT%2Fi5m1m5dwtcevGqICRPUBFh%2FDhM9gsx%2FMZfoBnpqtJHMxX5MoXs%2FRRpNmGUVVKMlZ4xhcb%2BGJ5YO9TCgb9emOoliocIcM%2B4dzrWBZW3AgdsjpUYvLcN8sdvOvPMarHj2zIIzQ4X1Awau9YT57%2BeciUnZTf7d%2F1R31xKqAgEwKjY7d7H%2BcMHTk6I9oo0%2F6uNe9ok2s7QppqEnd%2FZf0MgKTvzZFxMrtkisoIcDLyZ2%2BYQ8U4gefWBe8p9ucH9%2FqG%2FTwPbeQFslQxWO1%2FSbjHY4TZECfLL5IM0Z5fFFx7lJEQBvF%2FNGnWjNB2ut25h%2FQddKViRm1p7T2wGfyCb3UTDXbKEEt%2FXeuiuYkh0kdeTbpvyO%2FXqp8al8%2Bwk1Se7jecsEVC%2Bp4NBMGXOVmyYJEYNIdokkGg7zGsjLVeocba3iYyTK1VPitTN%2BDaHf5INFZsjB0z6TSNTvdrsF1yEzm6mrc4XFmgsy%2FbXjvT2MbEidgoA0IeVOncIO%2FMLdtuBATISPmSAYm3ESadb1Zy9ZaHabwoYhMogWnPcpl%2F11AKZ%2BzCu2%2BE%2FO2OpUbnRIsvfrQI80T71hsKPNe7krtbMOWzv9cGOqUBs3FWK7N0M3quklR1biz5ShuvTdL5QkHwgy7t3WRAQ3lhid7VNvc9f9cG7iqDvqBlRzGEQSQ%2FL9396slHPXcnrLNvKc%2BHBZ5VQa6Smm2Q%2BtJm863tqr6oDlfA5KHUNmVz%2FNAowfkg21fIYkT10YRD8yORiYGlSdKQnJg4ieDifMpxKXNGyRWEmuXrxtUgwpG%2BpaSm0PvL8RjVjyWE17rX8arSZk1I&X-Amz-Signature=3d8a229215d44054c5cf8ff97e72d00504a8abd3751aa7379ad1a3d15985c0de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
