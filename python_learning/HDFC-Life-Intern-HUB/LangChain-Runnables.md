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
fetched_at: '2026-11-23T04:43:06.893Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPFAPDFN%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044302Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbsrm4mKkXFslkVRRTf%2Blo7tJalFnFm1Am8VeAOsIrHwIhAJEPic%2BgtGwOwweV7r1TaFriwsLqXU7DwDDeqXmDslrwKv8DCG0QABoMNjM3NDIzMTgzODA1Igy3liNiImVsAkD2gxEq3AM9X6uqDulz95sPMMQHxDOv22v6ewH1BjOjbUKgjWpuWCyLRgW459xoubj%2F3GkSvzTVoHmZJ5skYh%2BKyQDANka4IWiDDiYvqquNlwWB2V%2B4bZGDzXQlStQPjFWZfFj0AhnFs3GBnPp%2BPN%2B%2FmIbn9C7Vu6UwmaXLw%2FhvW%2FI7mqKtg3KdRilubbPEbQ9ZfrzlhXdff1h%2FeJXInjGA7g70dGWDr3Rcrw8oNvX9mdma0xeZC6T1wCoANNlDNOO3sRrb%2FJ2frIiNypifvm016wcISS3LUs%2BUI0UA%2F8GYBSlZScCAOQo71%2BXHxeHqruCDfLHy2dAuFeAhNDnhfsShUFxM3L%2FoA7pdU2ocJPFY%2BzcoCSi33TDNu6amHfESw0kksbVXLBDzBlasuMAzfHk7Y1TN6UhfHrX64rtmU97Dap8Y94BGHSe6NVoDvyiCu2k4fsvTOuSgk06kkBbyZnV9ZNbziA2Fm50Dr%2FzaBQnz8M8xju92IhV976FttQ7%2FxlrMBLM5vqFMdDym0JkFN4gx0LzOoZJInFPaCFYzZMH5p83B9ElDP%2FhT04qWdxLGHYHbzACcxeeXj%2FrJhM67XU1nMRCL6WWSIxwEiiI3qm3Im3s4LTo6qcZOdtJi2W3%2FUPoxKjDh%2B47YBjqkAW2ir1z1N1DoMC1x4ab3iqzeMbrhsuF3JkeukOUDb8ePdktqdc0Hjxb8ymWH13mlZHC1Z3fUWnXgpeFblsTXRm3G8qcjhfRfNfWwv5aRxM9Y615A%2BG9iVlXKnhGsXq2IVcdoFFrClpj25CtH%2FXrYZKSFq0YxX2UN%2B0gJuYhnQkdae8l8fOJnGreJIRJlsgnH6BwEa%2FFBtnDU%2FtqDVBvgqhiAcHPs&X-Amz-Signature=ac5cd8fd85b3b0c9c03c491936d6593876589838949f5c8153d38c329833a94c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPFAPDFN%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044302Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbsrm4mKkXFslkVRRTf%2Blo7tJalFnFm1Am8VeAOsIrHwIhAJEPic%2BgtGwOwweV7r1TaFriwsLqXU7DwDDeqXmDslrwKv8DCG0QABoMNjM3NDIzMTgzODA1Igy3liNiImVsAkD2gxEq3AM9X6uqDulz95sPMMQHxDOv22v6ewH1BjOjbUKgjWpuWCyLRgW459xoubj%2F3GkSvzTVoHmZJ5skYh%2BKyQDANka4IWiDDiYvqquNlwWB2V%2B4bZGDzXQlStQPjFWZfFj0AhnFs3GBnPp%2BPN%2B%2FmIbn9C7Vu6UwmaXLw%2FhvW%2FI7mqKtg3KdRilubbPEbQ9ZfrzlhXdff1h%2FeJXInjGA7g70dGWDr3Rcrw8oNvX9mdma0xeZC6T1wCoANNlDNOO3sRrb%2FJ2frIiNypifvm016wcISS3LUs%2BUI0UA%2F8GYBSlZScCAOQo71%2BXHxeHqruCDfLHy2dAuFeAhNDnhfsShUFxM3L%2FoA7pdU2ocJPFY%2BzcoCSi33TDNu6amHfESw0kksbVXLBDzBlasuMAzfHk7Y1TN6UhfHrX64rtmU97Dap8Y94BGHSe6NVoDvyiCu2k4fsvTOuSgk06kkBbyZnV9ZNbziA2Fm50Dr%2FzaBQnz8M8xju92IhV976FttQ7%2FxlrMBLM5vqFMdDym0JkFN4gx0LzOoZJInFPaCFYzZMH5p83B9ElDP%2FhT04qWdxLGHYHbzACcxeeXj%2FrJhM67XU1nMRCL6WWSIxwEiiI3qm3Im3s4LTo6qcZOdtJi2W3%2FUPoxKjDh%2B47YBjqkAW2ir1z1N1DoMC1x4ab3iqzeMbrhsuF3JkeukOUDb8ePdktqdc0Hjxb8ymWH13mlZHC1Z3fUWnXgpeFblsTXRm3G8qcjhfRfNfWwv5aRxM9Y615A%2BG9iVlXKnhGsXq2IVcdoFFrClpj25CtH%2FXrYZKSFq0YxX2UN%2B0gJuYhnQkdae8l8fOJnGreJIRJlsgnH6BwEa%2FFBtnDU%2FtqDVBvgqhiAcHPs&X-Amz-Signature=2aa7997fab9155757cc1c432663e72f2103ea76c7eced511e73bafbb54683071&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPFAPDFN%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044302Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbsrm4mKkXFslkVRRTf%2Blo7tJalFnFm1Am8VeAOsIrHwIhAJEPic%2BgtGwOwweV7r1TaFriwsLqXU7DwDDeqXmDslrwKv8DCG0QABoMNjM3NDIzMTgzODA1Igy3liNiImVsAkD2gxEq3AM9X6uqDulz95sPMMQHxDOv22v6ewH1BjOjbUKgjWpuWCyLRgW459xoubj%2F3GkSvzTVoHmZJ5skYh%2BKyQDANka4IWiDDiYvqquNlwWB2V%2B4bZGDzXQlStQPjFWZfFj0AhnFs3GBnPp%2BPN%2B%2FmIbn9C7Vu6UwmaXLw%2FhvW%2FI7mqKtg3KdRilubbPEbQ9ZfrzlhXdff1h%2FeJXInjGA7g70dGWDr3Rcrw8oNvX9mdma0xeZC6T1wCoANNlDNOO3sRrb%2FJ2frIiNypifvm016wcISS3LUs%2BUI0UA%2F8GYBSlZScCAOQo71%2BXHxeHqruCDfLHy2dAuFeAhNDnhfsShUFxM3L%2FoA7pdU2ocJPFY%2BzcoCSi33TDNu6amHfESw0kksbVXLBDzBlasuMAzfHk7Y1TN6UhfHrX64rtmU97Dap8Y94BGHSe6NVoDvyiCu2k4fsvTOuSgk06kkBbyZnV9ZNbziA2Fm50Dr%2FzaBQnz8M8xju92IhV976FttQ7%2FxlrMBLM5vqFMdDym0JkFN4gx0LzOoZJInFPaCFYzZMH5p83B9ElDP%2FhT04qWdxLGHYHbzACcxeeXj%2FrJhM67XU1nMRCL6WWSIxwEiiI3qm3Im3s4LTo6qcZOdtJi2W3%2FUPoxKjDh%2B47YBjqkAW2ir1z1N1DoMC1x4ab3iqzeMbrhsuF3JkeukOUDb8ePdktqdc0Hjxb8ymWH13mlZHC1Z3fUWnXgpeFblsTXRm3G8qcjhfRfNfWwv5aRxM9Y615A%2BG9iVlXKnhGsXq2IVcdoFFrClpj25CtH%2FXrYZKSFq0YxX2UN%2B0gJuYhnQkdae8l8fOJnGreJIRJlsgnH6BwEa%2FFBtnDU%2FtqDVBvgqhiAcHPs&X-Amz-Signature=eae809a12f3861f29190835ae1c3919223c24d92c6340a7da9b6a05f09714638&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPFAPDFN%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044302Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbsrm4mKkXFslkVRRTf%2Blo7tJalFnFm1Am8VeAOsIrHwIhAJEPic%2BgtGwOwweV7r1TaFriwsLqXU7DwDDeqXmDslrwKv8DCG0QABoMNjM3NDIzMTgzODA1Igy3liNiImVsAkD2gxEq3AM9X6uqDulz95sPMMQHxDOv22v6ewH1BjOjbUKgjWpuWCyLRgW459xoubj%2F3GkSvzTVoHmZJ5skYh%2BKyQDANka4IWiDDiYvqquNlwWB2V%2B4bZGDzXQlStQPjFWZfFj0AhnFs3GBnPp%2BPN%2B%2FmIbn9C7Vu6UwmaXLw%2FhvW%2FI7mqKtg3KdRilubbPEbQ9ZfrzlhXdff1h%2FeJXInjGA7g70dGWDr3Rcrw8oNvX9mdma0xeZC6T1wCoANNlDNOO3sRrb%2FJ2frIiNypifvm016wcISS3LUs%2BUI0UA%2F8GYBSlZScCAOQo71%2BXHxeHqruCDfLHy2dAuFeAhNDnhfsShUFxM3L%2FoA7pdU2ocJPFY%2BzcoCSi33TDNu6amHfESw0kksbVXLBDzBlasuMAzfHk7Y1TN6UhfHrX64rtmU97Dap8Y94BGHSe6NVoDvyiCu2k4fsvTOuSgk06kkBbyZnV9ZNbziA2Fm50Dr%2FzaBQnz8M8xju92IhV976FttQ7%2FxlrMBLM5vqFMdDym0JkFN4gx0LzOoZJInFPaCFYzZMH5p83B9ElDP%2FhT04qWdxLGHYHbzACcxeeXj%2FrJhM67XU1nMRCL6WWSIxwEiiI3qm3Im3s4LTo6qcZOdtJi2W3%2FUPoxKjDh%2B47YBjqkAW2ir1z1N1DoMC1x4ab3iqzeMbrhsuF3JkeukOUDb8ePdktqdc0Hjxb8ymWH13mlZHC1Z3fUWnXgpeFblsTXRm3G8qcjhfRfNfWwv5aRxM9Y615A%2BG9iVlXKnhGsXq2IVcdoFFrClpj25CtH%2FXrYZKSFq0YxX2UN%2B0gJuYhnQkdae8l8fOJnGreJIRJlsgnH6BwEa%2FFBtnDU%2FtqDVBvgqhiAcHPs&X-Amz-Signature=e84026d971d4f3c32e4e7e1dc183ddf0cca09d66467f2706ca283c2f63c0387c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPFAPDFN%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044302Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbsrm4mKkXFslkVRRTf%2Blo7tJalFnFm1Am8VeAOsIrHwIhAJEPic%2BgtGwOwweV7r1TaFriwsLqXU7DwDDeqXmDslrwKv8DCG0QABoMNjM3NDIzMTgzODA1Igy3liNiImVsAkD2gxEq3AM9X6uqDulz95sPMMQHxDOv22v6ewH1BjOjbUKgjWpuWCyLRgW459xoubj%2F3GkSvzTVoHmZJ5skYh%2BKyQDANka4IWiDDiYvqquNlwWB2V%2B4bZGDzXQlStQPjFWZfFj0AhnFs3GBnPp%2BPN%2B%2FmIbn9C7Vu6UwmaXLw%2FhvW%2FI7mqKtg3KdRilubbPEbQ9ZfrzlhXdff1h%2FeJXInjGA7g70dGWDr3Rcrw8oNvX9mdma0xeZC6T1wCoANNlDNOO3sRrb%2FJ2frIiNypifvm016wcISS3LUs%2BUI0UA%2F8GYBSlZScCAOQo71%2BXHxeHqruCDfLHy2dAuFeAhNDnhfsShUFxM3L%2FoA7pdU2ocJPFY%2BzcoCSi33TDNu6amHfESw0kksbVXLBDzBlasuMAzfHk7Y1TN6UhfHrX64rtmU97Dap8Y94BGHSe6NVoDvyiCu2k4fsvTOuSgk06kkBbyZnV9ZNbziA2Fm50Dr%2FzaBQnz8M8xju92IhV976FttQ7%2FxlrMBLM5vqFMdDym0JkFN4gx0LzOoZJInFPaCFYzZMH5p83B9ElDP%2FhT04qWdxLGHYHbzACcxeeXj%2FrJhM67XU1nMRCL6WWSIxwEiiI3qm3Im3s4LTo6qcZOdtJi2W3%2FUPoxKjDh%2B47YBjqkAW2ir1z1N1DoMC1x4ab3iqzeMbrhsuF3JkeukOUDb8ePdktqdc0Hjxb8ymWH13mlZHC1Z3fUWnXgpeFblsTXRm3G8qcjhfRfNfWwv5aRxM9Y615A%2BG9iVlXKnhGsXq2IVcdoFFrClpj25CtH%2FXrYZKSFq0YxX2UN%2B0gJuYhnQkdae8l8fOJnGreJIRJlsgnH6BwEa%2FFBtnDU%2FtqDVBvgqhiAcHPs&X-Amz-Signature=cc49ff4073d8e9cb40b22fcc41a23153de98efe12e11ea903677e83cf2739074&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPFAPDFN%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044302Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbsrm4mKkXFslkVRRTf%2Blo7tJalFnFm1Am8VeAOsIrHwIhAJEPic%2BgtGwOwweV7r1TaFriwsLqXU7DwDDeqXmDslrwKv8DCG0QABoMNjM3NDIzMTgzODA1Igy3liNiImVsAkD2gxEq3AM9X6uqDulz95sPMMQHxDOv22v6ewH1BjOjbUKgjWpuWCyLRgW459xoubj%2F3GkSvzTVoHmZJ5skYh%2BKyQDANka4IWiDDiYvqquNlwWB2V%2B4bZGDzXQlStQPjFWZfFj0AhnFs3GBnPp%2BPN%2B%2FmIbn9C7Vu6UwmaXLw%2FhvW%2FI7mqKtg3KdRilubbPEbQ9ZfrzlhXdff1h%2FeJXInjGA7g70dGWDr3Rcrw8oNvX9mdma0xeZC6T1wCoANNlDNOO3sRrb%2FJ2frIiNypifvm016wcISS3LUs%2BUI0UA%2F8GYBSlZScCAOQo71%2BXHxeHqruCDfLHy2dAuFeAhNDnhfsShUFxM3L%2FoA7pdU2ocJPFY%2BzcoCSi33TDNu6amHfESw0kksbVXLBDzBlasuMAzfHk7Y1TN6UhfHrX64rtmU97Dap8Y94BGHSe6NVoDvyiCu2k4fsvTOuSgk06kkBbyZnV9ZNbziA2Fm50Dr%2FzaBQnz8M8xju92IhV976FttQ7%2FxlrMBLM5vqFMdDym0JkFN4gx0LzOoZJInFPaCFYzZMH5p83B9ElDP%2FhT04qWdxLGHYHbzACcxeeXj%2FrJhM67XU1nMRCL6WWSIxwEiiI3qm3Im3s4LTo6qcZOdtJi2W3%2FUPoxKjDh%2B47YBjqkAW2ir1z1N1DoMC1x4ab3iqzeMbrhsuF3JkeukOUDb8ePdktqdc0Hjxb8ymWH13mlZHC1Z3fUWnXgpeFblsTXRm3G8qcjhfRfNfWwv5aRxM9Y615A%2BG9iVlXKnhGsXq2IVcdoFFrClpj25CtH%2FXrYZKSFq0YxX2UN%2B0gJuYhnQkdae8l8fOJnGreJIRJlsgnH6BwEa%2FFBtnDU%2FtqDVBvgqhiAcHPs&X-Amz-Signature=82983435fea1f39d74acee6921a5ef031657d32cdc5b6ac6a4ef3e535aecdd51&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPFAPDFN%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044302Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbsrm4mKkXFslkVRRTf%2Blo7tJalFnFm1Am8VeAOsIrHwIhAJEPic%2BgtGwOwweV7r1TaFriwsLqXU7DwDDeqXmDslrwKv8DCG0QABoMNjM3NDIzMTgzODA1Igy3liNiImVsAkD2gxEq3AM9X6uqDulz95sPMMQHxDOv22v6ewH1BjOjbUKgjWpuWCyLRgW459xoubj%2F3GkSvzTVoHmZJ5skYh%2BKyQDANka4IWiDDiYvqquNlwWB2V%2B4bZGDzXQlStQPjFWZfFj0AhnFs3GBnPp%2BPN%2B%2FmIbn9C7Vu6UwmaXLw%2FhvW%2FI7mqKtg3KdRilubbPEbQ9ZfrzlhXdff1h%2FeJXInjGA7g70dGWDr3Rcrw8oNvX9mdma0xeZC6T1wCoANNlDNOO3sRrb%2FJ2frIiNypifvm016wcISS3LUs%2BUI0UA%2F8GYBSlZScCAOQo71%2BXHxeHqruCDfLHy2dAuFeAhNDnhfsShUFxM3L%2FoA7pdU2ocJPFY%2BzcoCSi33TDNu6amHfESw0kksbVXLBDzBlasuMAzfHk7Y1TN6UhfHrX64rtmU97Dap8Y94BGHSe6NVoDvyiCu2k4fsvTOuSgk06kkBbyZnV9ZNbziA2Fm50Dr%2FzaBQnz8M8xju92IhV976FttQ7%2FxlrMBLM5vqFMdDym0JkFN4gx0LzOoZJInFPaCFYzZMH5p83B9ElDP%2FhT04qWdxLGHYHbzACcxeeXj%2FrJhM67XU1nMRCL6WWSIxwEiiI3qm3Im3s4LTo6qcZOdtJi2W3%2FUPoxKjDh%2B47YBjqkAW2ir1z1N1DoMC1x4ab3iqzeMbrhsuF3JkeukOUDb8ePdktqdc0Hjxb8ymWH13mlZHC1Z3fUWnXgpeFblsTXRm3G8qcjhfRfNfWwv5aRxM9Y615A%2BG9iVlXKnhGsXq2IVcdoFFrClpj25CtH%2FXrYZKSFq0YxX2UN%2B0gJuYhnQkdae8l8fOJnGreJIRJlsgnH6BwEa%2FFBtnDU%2FtqDVBvgqhiAcHPs&X-Amz-Signature=f3e91dedc035cb7a3f75e6bf5c1151d04d447c9014c15d23c898884da06fb717&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPFAPDFN%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044302Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbsrm4mKkXFslkVRRTf%2Blo7tJalFnFm1Am8VeAOsIrHwIhAJEPic%2BgtGwOwweV7r1TaFriwsLqXU7DwDDeqXmDslrwKv8DCG0QABoMNjM3NDIzMTgzODA1Igy3liNiImVsAkD2gxEq3AM9X6uqDulz95sPMMQHxDOv22v6ewH1BjOjbUKgjWpuWCyLRgW459xoubj%2F3GkSvzTVoHmZJ5skYh%2BKyQDANka4IWiDDiYvqquNlwWB2V%2B4bZGDzXQlStQPjFWZfFj0AhnFs3GBnPp%2BPN%2B%2FmIbn9C7Vu6UwmaXLw%2FhvW%2FI7mqKtg3KdRilubbPEbQ9ZfrzlhXdff1h%2FeJXInjGA7g70dGWDr3Rcrw8oNvX9mdma0xeZC6T1wCoANNlDNOO3sRrb%2FJ2frIiNypifvm016wcISS3LUs%2BUI0UA%2F8GYBSlZScCAOQo71%2BXHxeHqruCDfLHy2dAuFeAhNDnhfsShUFxM3L%2FoA7pdU2ocJPFY%2BzcoCSi33TDNu6amHfESw0kksbVXLBDzBlasuMAzfHk7Y1TN6UhfHrX64rtmU97Dap8Y94BGHSe6NVoDvyiCu2k4fsvTOuSgk06kkBbyZnV9ZNbziA2Fm50Dr%2FzaBQnz8M8xju92IhV976FttQ7%2FxlrMBLM5vqFMdDym0JkFN4gx0LzOoZJInFPaCFYzZMH5p83B9ElDP%2FhT04qWdxLGHYHbzACcxeeXj%2FrJhM67XU1nMRCL6WWSIxwEiiI3qm3Im3s4LTo6qcZOdtJi2W3%2FUPoxKjDh%2B47YBjqkAW2ir1z1N1DoMC1x4ab3iqzeMbrhsuF3JkeukOUDb8ePdktqdc0Hjxb8ymWH13mlZHC1Z3fUWnXgpeFblsTXRm3G8qcjhfRfNfWwv5aRxM9Y615A%2BG9iVlXKnhGsXq2IVcdoFFrClpj25CtH%2FXrYZKSFq0YxX2UN%2B0gJuYhnQkdae8l8fOJnGreJIRJlsgnH6BwEa%2FFBtnDU%2FtqDVBvgqhiAcHPs&X-Amz-Signature=a01c40cbdcc5be259b57bc7e1e95841a30d8227db851b9030997a446eb0a8435&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPFAPDFN%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044302Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbsrm4mKkXFslkVRRTf%2Blo7tJalFnFm1Am8VeAOsIrHwIhAJEPic%2BgtGwOwweV7r1TaFriwsLqXU7DwDDeqXmDslrwKv8DCG0QABoMNjM3NDIzMTgzODA1Igy3liNiImVsAkD2gxEq3AM9X6uqDulz95sPMMQHxDOv22v6ewH1BjOjbUKgjWpuWCyLRgW459xoubj%2F3GkSvzTVoHmZJ5skYh%2BKyQDANka4IWiDDiYvqquNlwWB2V%2B4bZGDzXQlStQPjFWZfFj0AhnFs3GBnPp%2BPN%2B%2FmIbn9C7Vu6UwmaXLw%2FhvW%2FI7mqKtg3KdRilubbPEbQ9ZfrzlhXdff1h%2FeJXInjGA7g70dGWDr3Rcrw8oNvX9mdma0xeZC6T1wCoANNlDNOO3sRrb%2FJ2frIiNypifvm016wcISS3LUs%2BUI0UA%2F8GYBSlZScCAOQo71%2BXHxeHqruCDfLHy2dAuFeAhNDnhfsShUFxM3L%2FoA7pdU2ocJPFY%2BzcoCSi33TDNu6amHfESw0kksbVXLBDzBlasuMAzfHk7Y1TN6UhfHrX64rtmU97Dap8Y94BGHSe6NVoDvyiCu2k4fsvTOuSgk06kkBbyZnV9ZNbziA2Fm50Dr%2FzaBQnz8M8xju92IhV976FttQ7%2FxlrMBLM5vqFMdDym0JkFN4gx0LzOoZJInFPaCFYzZMH5p83B9ElDP%2FhT04qWdxLGHYHbzACcxeeXj%2FrJhM67XU1nMRCL6WWSIxwEiiI3qm3Im3s4LTo6qcZOdtJi2W3%2FUPoxKjDh%2B47YBjqkAW2ir1z1N1DoMC1x4ab3iqzeMbrhsuF3JkeukOUDb8ePdktqdc0Hjxb8ymWH13mlZHC1Z3fUWnXgpeFblsTXRm3G8qcjhfRfNfWwv5aRxM9Y615A%2BG9iVlXKnhGsXq2IVcdoFFrClpj25CtH%2FXrYZKSFq0YxX2UN%2B0gJuYhnQkdae8l8fOJnGreJIRJlsgnH6BwEa%2FFBtnDU%2FtqDVBvgqhiAcHPs&X-Amz-Signature=882ca00f8dedea7f1be0aebc0389754215477449b1ebd5d657e01309f9a3ed0f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
