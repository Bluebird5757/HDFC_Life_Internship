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
fetched_at: '2026-09-21T02:18:57.683Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZHETXTM%2F20260921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260921T021852Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE38m%2BwoOzs04mNyAr6xYfQrV4dLuW9bgxBMW2qJW50iAiBZ43KMABT38wDTbKmVKvQxVcrGxEC2%2Fb6TUDSHHNIJUiqIBAiB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7hTkF0Kcidm7%2B%2BxJKtwD5GMevIV3IXDg%2F0acINtPCTM3TVAbKasVN0XCC4rpiIlAU0rAHowM6Tkrjl8Wx3LKmBQVIkow7Xpcr5cHAkcoN%2Fyk%2FxPPoBrN0gbeGAHEEhfHvCyi%2F2DqaSqssFfpAN9tIbr1%2Bqiy6OWvGZUiYC2T6tm8CH69zXI2%2FmhMAxb7qzzdlvUlRh8ZNBJ7P1sE3RNa%2Fj0RM35h0BjOysu%2FhDpWlar%2FAV5N9U3itcA8pZgBgT3tC8q%2F4FVyWE327N0zVf9QyvVuZIA4IT%2FM9wUE6z65rN%2B6VrPuLG2%2BDTivb8jvEzluhaonHABPdLZH9SE4jwZh6TgxZzhc5RpxWe7PMKn7lOXXaRMcr%2BbAzQddC%2BlBoog74tJs8KDNrLW9a012M0U1K6RYXgsIhscnFhRanQKO%2FGTmb8mHRTYaPdSsC%2Ftd0XBCriQWQ%2BhkMj8XcJVgtyRAeWPH9FbChGpv8Swrsz5zCIgCqqsBDoallTGR%2BDZYosy%2FYpSPrQVM16hirGTlLTG1oCkFOXgItEtQl0IIIhhsXRftdEDalhLhMIwJ4mEC4hX8U5Lt9Vf6Ru7493VPVP1Z6rIrJW4mh%2F7lX8E2p%2Bt5TcOTfa2cUXBr3FTVhBGnWLGW%2B7oKPMw%2BX4D5e1gwueTB1QY6pgECooK6yF4OSZN9TOZOUJpYhFEDuueRaKxxrdPFkJCnUs3r2Hnz1XSq85Lq2yyAKIzAyXrXIpn%2FIxYfObhrJmrfd6%2FuAqNIaS44aDpdY4PPAXJagh1JWR2nzSstiExZYm%2F3N73PVQuHH7THxfWJa7d5Ic2R%2FV3SYY9iIMc3YpRlRiPVOC9bldthuOZWx4YxAMk%2F5j5sDY2lS%2FBYN%2Be14qHMif1NU%2FFc&X-Amz-Signature=890e8e2f1fec10170a79fdd80eab166e8bcb78dfceeef0d0a979acf42a321675&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZHETXTM%2F20260921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260921T021852Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE38m%2BwoOzs04mNyAr6xYfQrV4dLuW9bgxBMW2qJW50iAiBZ43KMABT38wDTbKmVKvQxVcrGxEC2%2Fb6TUDSHHNIJUiqIBAiB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7hTkF0Kcidm7%2B%2BxJKtwD5GMevIV3IXDg%2F0acINtPCTM3TVAbKasVN0XCC4rpiIlAU0rAHowM6Tkrjl8Wx3LKmBQVIkow7Xpcr5cHAkcoN%2Fyk%2FxPPoBrN0gbeGAHEEhfHvCyi%2F2DqaSqssFfpAN9tIbr1%2Bqiy6OWvGZUiYC2T6tm8CH69zXI2%2FmhMAxb7qzzdlvUlRh8ZNBJ7P1sE3RNa%2Fj0RM35h0BjOysu%2FhDpWlar%2FAV5N9U3itcA8pZgBgT3tC8q%2F4FVyWE327N0zVf9QyvVuZIA4IT%2FM9wUE6z65rN%2B6VrPuLG2%2BDTivb8jvEzluhaonHABPdLZH9SE4jwZh6TgxZzhc5RpxWe7PMKn7lOXXaRMcr%2BbAzQddC%2BlBoog74tJs8KDNrLW9a012M0U1K6RYXgsIhscnFhRanQKO%2FGTmb8mHRTYaPdSsC%2Ftd0XBCriQWQ%2BhkMj8XcJVgtyRAeWPH9FbChGpv8Swrsz5zCIgCqqsBDoallTGR%2BDZYosy%2FYpSPrQVM16hirGTlLTG1oCkFOXgItEtQl0IIIhhsXRftdEDalhLhMIwJ4mEC4hX8U5Lt9Vf6Ru7493VPVP1Z6rIrJW4mh%2F7lX8E2p%2Bt5TcOTfa2cUXBr3FTVhBGnWLGW%2B7oKPMw%2BX4D5e1gwueTB1QY6pgECooK6yF4OSZN9TOZOUJpYhFEDuueRaKxxrdPFkJCnUs3r2Hnz1XSq85Lq2yyAKIzAyXrXIpn%2FIxYfObhrJmrfd6%2FuAqNIaS44aDpdY4PPAXJagh1JWR2nzSstiExZYm%2F3N73PVQuHH7THxfWJa7d5Ic2R%2FV3SYY9iIMc3YpRlRiPVOC9bldthuOZWx4YxAMk%2F5j5sDY2lS%2FBYN%2Be14qHMif1NU%2FFc&X-Amz-Signature=67679f280d46a117f4603546f9f04c8715e596eab382f4b2877d6082294ea9db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZHETXTM%2F20260921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260921T021852Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE38m%2BwoOzs04mNyAr6xYfQrV4dLuW9bgxBMW2qJW50iAiBZ43KMABT38wDTbKmVKvQxVcrGxEC2%2Fb6TUDSHHNIJUiqIBAiB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7hTkF0Kcidm7%2B%2BxJKtwD5GMevIV3IXDg%2F0acINtPCTM3TVAbKasVN0XCC4rpiIlAU0rAHowM6Tkrjl8Wx3LKmBQVIkow7Xpcr5cHAkcoN%2Fyk%2FxPPoBrN0gbeGAHEEhfHvCyi%2F2DqaSqssFfpAN9tIbr1%2Bqiy6OWvGZUiYC2T6tm8CH69zXI2%2FmhMAxb7qzzdlvUlRh8ZNBJ7P1sE3RNa%2Fj0RM35h0BjOysu%2FhDpWlar%2FAV5N9U3itcA8pZgBgT3tC8q%2F4FVyWE327N0zVf9QyvVuZIA4IT%2FM9wUE6z65rN%2B6VrPuLG2%2BDTivb8jvEzluhaonHABPdLZH9SE4jwZh6TgxZzhc5RpxWe7PMKn7lOXXaRMcr%2BbAzQddC%2BlBoog74tJs8KDNrLW9a012M0U1K6RYXgsIhscnFhRanQKO%2FGTmb8mHRTYaPdSsC%2Ftd0XBCriQWQ%2BhkMj8XcJVgtyRAeWPH9FbChGpv8Swrsz5zCIgCqqsBDoallTGR%2BDZYosy%2FYpSPrQVM16hirGTlLTG1oCkFOXgItEtQl0IIIhhsXRftdEDalhLhMIwJ4mEC4hX8U5Lt9Vf6Ru7493VPVP1Z6rIrJW4mh%2F7lX8E2p%2Bt5TcOTfa2cUXBr3FTVhBGnWLGW%2B7oKPMw%2BX4D5e1gwueTB1QY6pgECooK6yF4OSZN9TOZOUJpYhFEDuueRaKxxrdPFkJCnUs3r2Hnz1XSq85Lq2yyAKIzAyXrXIpn%2FIxYfObhrJmrfd6%2FuAqNIaS44aDpdY4PPAXJagh1JWR2nzSstiExZYm%2F3N73PVQuHH7THxfWJa7d5Ic2R%2FV3SYY9iIMc3YpRlRiPVOC9bldthuOZWx4YxAMk%2F5j5sDY2lS%2FBYN%2Be14qHMif1NU%2FFc&X-Amz-Signature=79e73492501a78be5556941fa96509890611eb1952cc07c15000fb9f85d44f54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZHETXTM%2F20260921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260921T021852Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE38m%2BwoOzs04mNyAr6xYfQrV4dLuW9bgxBMW2qJW50iAiBZ43KMABT38wDTbKmVKvQxVcrGxEC2%2Fb6TUDSHHNIJUiqIBAiB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7hTkF0Kcidm7%2B%2BxJKtwD5GMevIV3IXDg%2F0acINtPCTM3TVAbKasVN0XCC4rpiIlAU0rAHowM6Tkrjl8Wx3LKmBQVIkow7Xpcr5cHAkcoN%2Fyk%2FxPPoBrN0gbeGAHEEhfHvCyi%2F2DqaSqssFfpAN9tIbr1%2Bqiy6OWvGZUiYC2T6tm8CH69zXI2%2FmhMAxb7qzzdlvUlRh8ZNBJ7P1sE3RNa%2Fj0RM35h0BjOysu%2FhDpWlar%2FAV5N9U3itcA8pZgBgT3tC8q%2F4FVyWE327N0zVf9QyvVuZIA4IT%2FM9wUE6z65rN%2B6VrPuLG2%2BDTivb8jvEzluhaonHABPdLZH9SE4jwZh6TgxZzhc5RpxWe7PMKn7lOXXaRMcr%2BbAzQddC%2BlBoog74tJs8KDNrLW9a012M0U1K6RYXgsIhscnFhRanQKO%2FGTmb8mHRTYaPdSsC%2Ftd0XBCriQWQ%2BhkMj8XcJVgtyRAeWPH9FbChGpv8Swrsz5zCIgCqqsBDoallTGR%2BDZYosy%2FYpSPrQVM16hirGTlLTG1oCkFOXgItEtQl0IIIhhsXRftdEDalhLhMIwJ4mEC4hX8U5Lt9Vf6Ru7493VPVP1Z6rIrJW4mh%2F7lX8E2p%2Bt5TcOTfa2cUXBr3FTVhBGnWLGW%2B7oKPMw%2BX4D5e1gwueTB1QY6pgECooK6yF4OSZN9TOZOUJpYhFEDuueRaKxxrdPFkJCnUs3r2Hnz1XSq85Lq2yyAKIzAyXrXIpn%2FIxYfObhrJmrfd6%2FuAqNIaS44aDpdY4PPAXJagh1JWR2nzSstiExZYm%2F3N73PVQuHH7THxfWJa7d5Ic2R%2FV3SYY9iIMc3YpRlRiPVOC9bldthuOZWx4YxAMk%2F5j5sDY2lS%2FBYN%2Be14qHMif1NU%2FFc&X-Amz-Signature=29617b73c0290730c494dd06b2d706cbb3b069fafb93f915f1cabeb5f9909a1a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZHETXTM%2F20260921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260921T021852Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE38m%2BwoOzs04mNyAr6xYfQrV4dLuW9bgxBMW2qJW50iAiBZ43KMABT38wDTbKmVKvQxVcrGxEC2%2Fb6TUDSHHNIJUiqIBAiB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7hTkF0Kcidm7%2B%2BxJKtwD5GMevIV3IXDg%2F0acINtPCTM3TVAbKasVN0XCC4rpiIlAU0rAHowM6Tkrjl8Wx3LKmBQVIkow7Xpcr5cHAkcoN%2Fyk%2FxPPoBrN0gbeGAHEEhfHvCyi%2F2DqaSqssFfpAN9tIbr1%2Bqiy6OWvGZUiYC2T6tm8CH69zXI2%2FmhMAxb7qzzdlvUlRh8ZNBJ7P1sE3RNa%2Fj0RM35h0BjOysu%2FhDpWlar%2FAV5N9U3itcA8pZgBgT3tC8q%2F4FVyWE327N0zVf9QyvVuZIA4IT%2FM9wUE6z65rN%2B6VrPuLG2%2BDTivb8jvEzluhaonHABPdLZH9SE4jwZh6TgxZzhc5RpxWe7PMKn7lOXXaRMcr%2BbAzQddC%2BlBoog74tJs8KDNrLW9a012M0U1K6RYXgsIhscnFhRanQKO%2FGTmb8mHRTYaPdSsC%2Ftd0XBCriQWQ%2BhkMj8XcJVgtyRAeWPH9FbChGpv8Swrsz5zCIgCqqsBDoallTGR%2BDZYosy%2FYpSPrQVM16hirGTlLTG1oCkFOXgItEtQl0IIIhhsXRftdEDalhLhMIwJ4mEC4hX8U5Lt9Vf6Ru7493VPVP1Z6rIrJW4mh%2F7lX8E2p%2Bt5TcOTfa2cUXBr3FTVhBGnWLGW%2B7oKPMw%2BX4D5e1gwueTB1QY6pgECooK6yF4OSZN9TOZOUJpYhFEDuueRaKxxrdPFkJCnUs3r2Hnz1XSq85Lq2yyAKIzAyXrXIpn%2FIxYfObhrJmrfd6%2FuAqNIaS44aDpdY4PPAXJagh1JWR2nzSstiExZYm%2F3N73PVQuHH7THxfWJa7d5Ic2R%2FV3SYY9iIMc3YpRlRiPVOC9bldthuOZWx4YxAMk%2F5j5sDY2lS%2FBYN%2Be14qHMif1NU%2FFc&X-Amz-Signature=d7a861009b7022a70e5a8040a3ef702429793c5647757e069aaa61b509200427&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZHETXTM%2F20260921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260921T021852Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE38m%2BwoOzs04mNyAr6xYfQrV4dLuW9bgxBMW2qJW50iAiBZ43KMABT38wDTbKmVKvQxVcrGxEC2%2Fb6TUDSHHNIJUiqIBAiB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7hTkF0Kcidm7%2B%2BxJKtwD5GMevIV3IXDg%2F0acINtPCTM3TVAbKasVN0XCC4rpiIlAU0rAHowM6Tkrjl8Wx3LKmBQVIkow7Xpcr5cHAkcoN%2Fyk%2FxPPoBrN0gbeGAHEEhfHvCyi%2F2DqaSqssFfpAN9tIbr1%2Bqiy6OWvGZUiYC2T6tm8CH69zXI2%2FmhMAxb7qzzdlvUlRh8ZNBJ7P1sE3RNa%2Fj0RM35h0BjOysu%2FhDpWlar%2FAV5N9U3itcA8pZgBgT3tC8q%2F4FVyWE327N0zVf9QyvVuZIA4IT%2FM9wUE6z65rN%2B6VrPuLG2%2BDTivb8jvEzluhaonHABPdLZH9SE4jwZh6TgxZzhc5RpxWe7PMKn7lOXXaRMcr%2BbAzQddC%2BlBoog74tJs8KDNrLW9a012M0U1K6RYXgsIhscnFhRanQKO%2FGTmb8mHRTYaPdSsC%2Ftd0XBCriQWQ%2BhkMj8XcJVgtyRAeWPH9FbChGpv8Swrsz5zCIgCqqsBDoallTGR%2BDZYosy%2FYpSPrQVM16hirGTlLTG1oCkFOXgItEtQl0IIIhhsXRftdEDalhLhMIwJ4mEC4hX8U5Lt9Vf6Ru7493VPVP1Z6rIrJW4mh%2F7lX8E2p%2Bt5TcOTfa2cUXBr3FTVhBGnWLGW%2B7oKPMw%2BX4D5e1gwueTB1QY6pgECooK6yF4OSZN9TOZOUJpYhFEDuueRaKxxrdPFkJCnUs3r2Hnz1XSq85Lq2yyAKIzAyXrXIpn%2FIxYfObhrJmrfd6%2FuAqNIaS44aDpdY4PPAXJagh1JWR2nzSstiExZYm%2F3N73PVQuHH7THxfWJa7d5Ic2R%2FV3SYY9iIMc3YpRlRiPVOC9bldthuOZWx4YxAMk%2F5j5sDY2lS%2FBYN%2Be14qHMif1NU%2FFc&X-Amz-Signature=cde0e85ef2024c648db9cf2717ac2df59d40c2c341267a82c20378f19946a82c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZHETXTM%2F20260921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260921T021852Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE38m%2BwoOzs04mNyAr6xYfQrV4dLuW9bgxBMW2qJW50iAiBZ43KMABT38wDTbKmVKvQxVcrGxEC2%2Fb6TUDSHHNIJUiqIBAiB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7hTkF0Kcidm7%2B%2BxJKtwD5GMevIV3IXDg%2F0acINtPCTM3TVAbKasVN0XCC4rpiIlAU0rAHowM6Tkrjl8Wx3LKmBQVIkow7Xpcr5cHAkcoN%2Fyk%2FxPPoBrN0gbeGAHEEhfHvCyi%2F2DqaSqssFfpAN9tIbr1%2Bqiy6OWvGZUiYC2T6tm8CH69zXI2%2FmhMAxb7qzzdlvUlRh8ZNBJ7P1sE3RNa%2Fj0RM35h0BjOysu%2FhDpWlar%2FAV5N9U3itcA8pZgBgT3tC8q%2F4FVyWE327N0zVf9QyvVuZIA4IT%2FM9wUE6z65rN%2B6VrPuLG2%2BDTivb8jvEzluhaonHABPdLZH9SE4jwZh6TgxZzhc5RpxWe7PMKn7lOXXaRMcr%2BbAzQddC%2BlBoog74tJs8KDNrLW9a012M0U1K6RYXgsIhscnFhRanQKO%2FGTmb8mHRTYaPdSsC%2Ftd0XBCriQWQ%2BhkMj8XcJVgtyRAeWPH9FbChGpv8Swrsz5zCIgCqqsBDoallTGR%2BDZYosy%2FYpSPrQVM16hirGTlLTG1oCkFOXgItEtQl0IIIhhsXRftdEDalhLhMIwJ4mEC4hX8U5Lt9Vf6Ru7493VPVP1Z6rIrJW4mh%2F7lX8E2p%2Bt5TcOTfa2cUXBr3FTVhBGnWLGW%2B7oKPMw%2BX4D5e1gwueTB1QY6pgECooK6yF4OSZN9TOZOUJpYhFEDuueRaKxxrdPFkJCnUs3r2Hnz1XSq85Lq2yyAKIzAyXrXIpn%2FIxYfObhrJmrfd6%2FuAqNIaS44aDpdY4PPAXJagh1JWR2nzSstiExZYm%2F3N73PVQuHH7THxfWJa7d5Ic2R%2FV3SYY9iIMc3YpRlRiPVOC9bldthuOZWx4YxAMk%2F5j5sDY2lS%2FBYN%2Be14qHMif1NU%2FFc&X-Amz-Signature=d0404279fbd873a5bfe392201e5dd0bf1d3d31525b10216988fad58602295f2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZHETXTM%2F20260921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260921T021852Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE38m%2BwoOzs04mNyAr6xYfQrV4dLuW9bgxBMW2qJW50iAiBZ43KMABT38wDTbKmVKvQxVcrGxEC2%2Fb6TUDSHHNIJUiqIBAiB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7hTkF0Kcidm7%2B%2BxJKtwD5GMevIV3IXDg%2F0acINtPCTM3TVAbKasVN0XCC4rpiIlAU0rAHowM6Tkrjl8Wx3LKmBQVIkow7Xpcr5cHAkcoN%2Fyk%2FxPPoBrN0gbeGAHEEhfHvCyi%2F2DqaSqssFfpAN9tIbr1%2Bqiy6OWvGZUiYC2T6tm8CH69zXI2%2FmhMAxb7qzzdlvUlRh8ZNBJ7P1sE3RNa%2Fj0RM35h0BjOysu%2FhDpWlar%2FAV5N9U3itcA8pZgBgT3tC8q%2F4FVyWE327N0zVf9QyvVuZIA4IT%2FM9wUE6z65rN%2B6VrPuLG2%2BDTivb8jvEzluhaonHABPdLZH9SE4jwZh6TgxZzhc5RpxWe7PMKn7lOXXaRMcr%2BbAzQddC%2BlBoog74tJs8KDNrLW9a012M0U1K6RYXgsIhscnFhRanQKO%2FGTmb8mHRTYaPdSsC%2Ftd0XBCriQWQ%2BhkMj8XcJVgtyRAeWPH9FbChGpv8Swrsz5zCIgCqqsBDoallTGR%2BDZYosy%2FYpSPrQVM16hirGTlLTG1oCkFOXgItEtQl0IIIhhsXRftdEDalhLhMIwJ4mEC4hX8U5Lt9Vf6Ru7493VPVP1Z6rIrJW4mh%2F7lX8E2p%2Bt5TcOTfa2cUXBr3FTVhBGnWLGW%2B7oKPMw%2BX4D5e1gwueTB1QY6pgECooK6yF4OSZN9TOZOUJpYhFEDuueRaKxxrdPFkJCnUs3r2Hnz1XSq85Lq2yyAKIzAyXrXIpn%2FIxYfObhrJmrfd6%2FuAqNIaS44aDpdY4PPAXJagh1JWR2nzSstiExZYm%2F3N73PVQuHH7THxfWJa7d5Ic2R%2FV3SYY9iIMc3YpRlRiPVOC9bldthuOZWx4YxAMk%2F5j5sDY2lS%2FBYN%2Be14qHMif1NU%2FFc&X-Amz-Signature=cf8102a0e27f9baa47f69c036ac47451b253f85153c68b9d10edf63ffa1a46d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZHETXTM%2F20260921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260921T021852Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE38m%2BwoOzs04mNyAr6xYfQrV4dLuW9bgxBMW2qJW50iAiBZ43KMABT38wDTbKmVKvQxVcrGxEC2%2Fb6TUDSHHNIJUiqIBAiB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7hTkF0Kcidm7%2B%2BxJKtwD5GMevIV3IXDg%2F0acINtPCTM3TVAbKasVN0XCC4rpiIlAU0rAHowM6Tkrjl8Wx3LKmBQVIkow7Xpcr5cHAkcoN%2Fyk%2FxPPoBrN0gbeGAHEEhfHvCyi%2F2DqaSqssFfpAN9tIbr1%2Bqiy6OWvGZUiYC2T6tm8CH69zXI2%2FmhMAxb7qzzdlvUlRh8ZNBJ7P1sE3RNa%2Fj0RM35h0BjOysu%2FhDpWlar%2FAV5N9U3itcA8pZgBgT3tC8q%2F4FVyWE327N0zVf9QyvVuZIA4IT%2FM9wUE6z65rN%2B6VrPuLG2%2BDTivb8jvEzluhaonHABPdLZH9SE4jwZh6TgxZzhc5RpxWe7PMKn7lOXXaRMcr%2BbAzQddC%2BlBoog74tJs8KDNrLW9a012M0U1K6RYXgsIhscnFhRanQKO%2FGTmb8mHRTYaPdSsC%2Ftd0XBCriQWQ%2BhkMj8XcJVgtyRAeWPH9FbChGpv8Swrsz5zCIgCqqsBDoallTGR%2BDZYosy%2FYpSPrQVM16hirGTlLTG1oCkFOXgItEtQl0IIIhhsXRftdEDalhLhMIwJ4mEC4hX8U5Lt9Vf6Ru7493VPVP1Z6rIrJW4mh%2F7lX8E2p%2Bt5TcOTfa2cUXBr3FTVhBGnWLGW%2B7oKPMw%2BX4D5e1gwueTB1QY6pgECooK6yF4OSZN9TOZOUJpYhFEDuueRaKxxrdPFkJCnUs3r2Hnz1XSq85Lq2yyAKIzAyXrXIpn%2FIxYfObhrJmrfd6%2FuAqNIaS44aDpdY4PPAXJagh1JWR2nzSstiExZYm%2F3N73PVQuHH7THxfWJa7d5Ic2R%2FV3SYY9iIMc3YpRlRiPVOC9bldthuOZWx4YxAMk%2F5j5sDY2lS%2FBYN%2Be14qHMif1NU%2FFc&X-Amz-Signature=d28b237bb1c4c25a6431e5ae8c0a35761d7e29f6540ca2b63e3847400763bfb2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
