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
fetched_at: '2026-11-15T03:46:24.653Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCJDRKCC%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034619Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDCQKwa%2FrwEUw0NXRYf3Z828q2nzX44iJKvS5qn0x7FSAiEA42Fy7%2BxsB2Fl7zvebL%2FRzDb1npozLBJeWtEAaZO0euIqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAqluhm1MWeX2nGMRircAyumjzoZHcCJdiyr0R0DLCGcPY7zz5gOX%2BqraS4PPkz3kl3PIdFSpdaXvSjgc0uLfI6VHp4dybmsJiSbB8QKK13%2FORjrkLZyoCYh8UPfPd5rKXXZKJba3HwP0GNmrR8l5C1TE%2Fd0lfiwKJViqSX%2FDItMjL9Ro%2Fdi9OIvDEPwNLnbmzx5SRctwBFxpPdPLlcQ%2B9%2FncPcTwzzp1oDcz4iV5Lok5mM7eIE%2Frz3EwTvoVcNf%2FABlBAygEjOH8J5nhl9L5oIj90HnxTa3W9oXBW96ralHcgT1b7IqjddjVMYGvxJI8bVOKNeLofBm8TTzyw9Q9%2FSiOi09afAHmwjwoW2yXRHs2Zo6b0J%2BpHxzY8MjYRkkXu55O%2FwS5I1rawGgP6A0eIyV%2FV1wzQUd1gpj2YF5oYNEGKdJDQI4Y1xwhqR9q56DfZcX9mA%2BJjyXERARhbrYPcqhc2L0njZgl%2FPgrJd3WS3yZ6VPqf7oZRVTLGKCHrFAL00iJm3OSW7GP0hZDGeObeIx%2B056xZzDNBeT0zmmMo1pLkzBkayDR3Az3UYi0kkeZUf2w6M82g5E%2FaFYguokahu%2FHUgByfkKGYh%2FmSHyHsLFijsbyKLuOuwWVam7pll1QZ7Bhx9FCsGcDLcbMLnb49cGOqUBt%2Fe3ZvRBkiVAn0UcYERRhvQ%2Fsd2VqNK2R3T6RqS3v4fLFvI1QVtfewAiPqnNLdeWs7nacZBTG5COCH15XegUExumhobMgVSByEINrM2tv2w7grA7apkxxuJi%2BYCtvIwYdyRqXaTmgfYW%2FekYemYFQJ9o4q8QbWSAhjjB9aQ4MD2Z85mXDDvDwyLW58nkCK%2FwLK%2Bywhwe91NB9Zi0%2B5q7jcwpieY9&X-Amz-Signature=8f951d015acc5e798312692361376a7a905c2320638c189ba54d2a1a97077c1c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCJDRKCC%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034619Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDCQKwa%2FrwEUw0NXRYf3Z828q2nzX44iJKvS5qn0x7FSAiEA42Fy7%2BxsB2Fl7zvebL%2FRzDb1npozLBJeWtEAaZO0euIqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAqluhm1MWeX2nGMRircAyumjzoZHcCJdiyr0R0DLCGcPY7zz5gOX%2BqraS4PPkz3kl3PIdFSpdaXvSjgc0uLfI6VHp4dybmsJiSbB8QKK13%2FORjrkLZyoCYh8UPfPd5rKXXZKJba3HwP0GNmrR8l5C1TE%2Fd0lfiwKJViqSX%2FDItMjL9Ro%2Fdi9OIvDEPwNLnbmzx5SRctwBFxpPdPLlcQ%2B9%2FncPcTwzzp1oDcz4iV5Lok5mM7eIE%2Frz3EwTvoVcNf%2FABlBAygEjOH8J5nhl9L5oIj90HnxTa3W9oXBW96ralHcgT1b7IqjddjVMYGvxJI8bVOKNeLofBm8TTzyw9Q9%2FSiOi09afAHmwjwoW2yXRHs2Zo6b0J%2BpHxzY8MjYRkkXu55O%2FwS5I1rawGgP6A0eIyV%2FV1wzQUd1gpj2YF5oYNEGKdJDQI4Y1xwhqR9q56DfZcX9mA%2BJjyXERARhbrYPcqhc2L0njZgl%2FPgrJd3WS3yZ6VPqf7oZRVTLGKCHrFAL00iJm3OSW7GP0hZDGeObeIx%2B056xZzDNBeT0zmmMo1pLkzBkayDR3Az3UYi0kkeZUf2w6M82g5E%2FaFYguokahu%2FHUgByfkKGYh%2FmSHyHsLFijsbyKLuOuwWVam7pll1QZ7Bhx9FCsGcDLcbMLnb49cGOqUBt%2Fe3ZvRBkiVAn0UcYERRhvQ%2Fsd2VqNK2R3T6RqS3v4fLFvI1QVtfewAiPqnNLdeWs7nacZBTG5COCH15XegUExumhobMgVSByEINrM2tv2w7grA7apkxxuJi%2BYCtvIwYdyRqXaTmgfYW%2FekYemYFQJ9o4q8QbWSAhjjB9aQ4MD2Z85mXDDvDwyLW58nkCK%2FwLK%2Bywhwe91NB9Zi0%2B5q7jcwpieY9&X-Amz-Signature=b8372c0fc9dff50b35cd9f6b80a79c4915c2ac61930b553c27184eb359337fbf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCJDRKCC%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034619Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDCQKwa%2FrwEUw0NXRYf3Z828q2nzX44iJKvS5qn0x7FSAiEA42Fy7%2BxsB2Fl7zvebL%2FRzDb1npozLBJeWtEAaZO0euIqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAqluhm1MWeX2nGMRircAyumjzoZHcCJdiyr0R0DLCGcPY7zz5gOX%2BqraS4PPkz3kl3PIdFSpdaXvSjgc0uLfI6VHp4dybmsJiSbB8QKK13%2FORjrkLZyoCYh8UPfPd5rKXXZKJba3HwP0GNmrR8l5C1TE%2Fd0lfiwKJViqSX%2FDItMjL9Ro%2Fdi9OIvDEPwNLnbmzx5SRctwBFxpPdPLlcQ%2B9%2FncPcTwzzp1oDcz4iV5Lok5mM7eIE%2Frz3EwTvoVcNf%2FABlBAygEjOH8J5nhl9L5oIj90HnxTa3W9oXBW96ralHcgT1b7IqjddjVMYGvxJI8bVOKNeLofBm8TTzyw9Q9%2FSiOi09afAHmwjwoW2yXRHs2Zo6b0J%2BpHxzY8MjYRkkXu55O%2FwS5I1rawGgP6A0eIyV%2FV1wzQUd1gpj2YF5oYNEGKdJDQI4Y1xwhqR9q56DfZcX9mA%2BJjyXERARhbrYPcqhc2L0njZgl%2FPgrJd3WS3yZ6VPqf7oZRVTLGKCHrFAL00iJm3OSW7GP0hZDGeObeIx%2B056xZzDNBeT0zmmMo1pLkzBkayDR3Az3UYi0kkeZUf2w6M82g5E%2FaFYguokahu%2FHUgByfkKGYh%2FmSHyHsLFijsbyKLuOuwWVam7pll1QZ7Bhx9FCsGcDLcbMLnb49cGOqUBt%2Fe3ZvRBkiVAn0UcYERRhvQ%2Fsd2VqNK2R3T6RqS3v4fLFvI1QVtfewAiPqnNLdeWs7nacZBTG5COCH15XegUExumhobMgVSByEINrM2tv2w7grA7apkxxuJi%2BYCtvIwYdyRqXaTmgfYW%2FekYemYFQJ9o4q8QbWSAhjjB9aQ4MD2Z85mXDDvDwyLW58nkCK%2FwLK%2Bywhwe91NB9Zi0%2B5q7jcwpieY9&X-Amz-Signature=9d71b2f1c07ef32951b24c26f5091864ecb839879bd07b12eb76bfbcbd7f581a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCJDRKCC%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034619Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDCQKwa%2FrwEUw0NXRYf3Z828q2nzX44iJKvS5qn0x7FSAiEA42Fy7%2BxsB2Fl7zvebL%2FRzDb1npozLBJeWtEAaZO0euIqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAqluhm1MWeX2nGMRircAyumjzoZHcCJdiyr0R0DLCGcPY7zz5gOX%2BqraS4PPkz3kl3PIdFSpdaXvSjgc0uLfI6VHp4dybmsJiSbB8QKK13%2FORjrkLZyoCYh8UPfPd5rKXXZKJba3HwP0GNmrR8l5C1TE%2Fd0lfiwKJViqSX%2FDItMjL9Ro%2Fdi9OIvDEPwNLnbmzx5SRctwBFxpPdPLlcQ%2B9%2FncPcTwzzp1oDcz4iV5Lok5mM7eIE%2Frz3EwTvoVcNf%2FABlBAygEjOH8J5nhl9L5oIj90HnxTa3W9oXBW96ralHcgT1b7IqjddjVMYGvxJI8bVOKNeLofBm8TTzyw9Q9%2FSiOi09afAHmwjwoW2yXRHs2Zo6b0J%2BpHxzY8MjYRkkXu55O%2FwS5I1rawGgP6A0eIyV%2FV1wzQUd1gpj2YF5oYNEGKdJDQI4Y1xwhqR9q56DfZcX9mA%2BJjyXERARhbrYPcqhc2L0njZgl%2FPgrJd3WS3yZ6VPqf7oZRVTLGKCHrFAL00iJm3OSW7GP0hZDGeObeIx%2B056xZzDNBeT0zmmMo1pLkzBkayDR3Az3UYi0kkeZUf2w6M82g5E%2FaFYguokahu%2FHUgByfkKGYh%2FmSHyHsLFijsbyKLuOuwWVam7pll1QZ7Bhx9FCsGcDLcbMLnb49cGOqUBt%2Fe3ZvRBkiVAn0UcYERRhvQ%2Fsd2VqNK2R3T6RqS3v4fLFvI1QVtfewAiPqnNLdeWs7nacZBTG5COCH15XegUExumhobMgVSByEINrM2tv2w7grA7apkxxuJi%2BYCtvIwYdyRqXaTmgfYW%2FekYemYFQJ9o4q8QbWSAhjjB9aQ4MD2Z85mXDDvDwyLW58nkCK%2FwLK%2Bywhwe91NB9Zi0%2B5q7jcwpieY9&X-Amz-Signature=f1327c4504a58d337dee1ad817f9bcc8c465487b1e9fe76d8dc795c4fd868f21&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCJDRKCC%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034619Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDCQKwa%2FrwEUw0NXRYf3Z828q2nzX44iJKvS5qn0x7FSAiEA42Fy7%2BxsB2Fl7zvebL%2FRzDb1npozLBJeWtEAaZO0euIqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAqluhm1MWeX2nGMRircAyumjzoZHcCJdiyr0R0DLCGcPY7zz5gOX%2BqraS4PPkz3kl3PIdFSpdaXvSjgc0uLfI6VHp4dybmsJiSbB8QKK13%2FORjrkLZyoCYh8UPfPd5rKXXZKJba3HwP0GNmrR8l5C1TE%2Fd0lfiwKJViqSX%2FDItMjL9Ro%2Fdi9OIvDEPwNLnbmzx5SRctwBFxpPdPLlcQ%2B9%2FncPcTwzzp1oDcz4iV5Lok5mM7eIE%2Frz3EwTvoVcNf%2FABlBAygEjOH8J5nhl9L5oIj90HnxTa3W9oXBW96ralHcgT1b7IqjddjVMYGvxJI8bVOKNeLofBm8TTzyw9Q9%2FSiOi09afAHmwjwoW2yXRHs2Zo6b0J%2BpHxzY8MjYRkkXu55O%2FwS5I1rawGgP6A0eIyV%2FV1wzQUd1gpj2YF5oYNEGKdJDQI4Y1xwhqR9q56DfZcX9mA%2BJjyXERARhbrYPcqhc2L0njZgl%2FPgrJd3WS3yZ6VPqf7oZRVTLGKCHrFAL00iJm3OSW7GP0hZDGeObeIx%2B056xZzDNBeT0zmmMo1pLkzBkayDR3Az3UYi0kkeZUf2w6M82g5E%2FaFYguokahu%2FHUgByfkKGYh%2FmSHyHsLFijsbyKLuOuwWVam7pll1QZ7Bhx9FCsGcDLcbMLnb49cGOqUBt%2Fe3ZvRBkiVAn0UcYERRhvQ%2Fsd2VqNK2R3T6RqS3v4fLFvI1QVtfewAiPqnNLdeWs7nacZBTG5COCH15XegUExumhobMgVSByEINrM2tv2w7grA7apkxxuJi%2BYCtvIwYdyRqXaTmgfYW%2FekYemYFQJ9o4q8QbWSAhjjB9aQ4MD2Z85mXDDvDwyLW58nkCK%2FwLK%2Bywhwe91NB9Zi0%2B5q7jcwpieY9&X-Amz-Signature=2759c923a892f60295f3772cc0817d332bce385091eb63c56540a0af5f670cbd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCJDRKCC%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034619Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDCQKwa%2FrwEUw0NXRYf3Z828q2nzX44iJKvS5qn0x7FSAiEA42Fy7%2BxsB2Fl7zvebL%2FRzDb1npozLBJeWtEAaZO0euIqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAqluhm1MWeX2nGMRircAyumjzoZHcCJdiyr0R0DLCGcPY7zz5gOX%2BqraS4PPkz3kl3PIdFSpdaXvSjgc0uLfI6VHp4dybmsJiSbB8QKK13%2FORjrkLZyoCYh8UPfPd5rKXXZKJba3HwP0GNmrR8l5C1TE%2Fd0lfiwKJViqSX%2FDItMjL9Ro%2Fdi9OIvDEPwNLnbmzx5SRctwBFxpPdPLlcQ%2B9%2FncPcTwzzp1oDcz4iV5Lok5mM7eIE%2Frz3EwTvoVcNf%2FABlBAygEjOH8J5nhl9L5oIj90HnxTa3W9oXBW96ralHcgT1b7IqjddjVMYGvxJI8bVOKNeLofBm8TTzyw9Q9%2FSiOi09afAHmwjwoW2yXRHs2Zo6b0J%2BpHxzY8MjYRkkXu55O%2FwS5I1rawGgP6A0eIyV%2FV1wzQUd1gpj2YF5oYNEGKdJDQI4Y1xwhqR9q56DfZcX9mA%2BJjyXERARhbrYPcqhc2L0njZgl%2FPgrJd3WS3yZ6VPqf7oZRVTLGKCHrFAL00iJm3OSW7GP0hZDGeObeIx%2B056xZzDNBeT0zmmMo1pLkzBkayDR3Az3UYi0kkeZUf2w6M82g5E%2FaFYguokahu%2FHUgByfkKGYh%2FmSHyHsLFijsbyKLuOuwWVam7pll1QZ7Bhx9FCsGcDLcbMLnb49cGOqUBt%2Fe3ZvRBkiVAn0UcYERRhvQ%2Fsd2VqNK2R3T6RqS3v4fLFvI1QVtfewAiPqnNLdeWs7nacZBTG5COCH15XegUExumhobMgVSByEINrM2tv2w7grA7apkxxuJi%2BYCtvIwYdyRqXaTmgfYW%2FekYemYFQJ9o4q8QbWSAhjjB9aQ4MD2Z85mXDDvDwyLW58nkCK%2FwLK%2Bywhwe91NB9Zi0%2B5q7jcwpieY9&X-Amz-Signature=5977ef97ff9a264199a456c069948d248e4f790f090551eaa3d40b5dfaaf06fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCJDRKCC%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034619Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDCQKwa%2FrwEUw0NXRYf3Z828q2nzX44iJKvS5qn0x7FSAiEA42Fy7%2BxsB2Fl7zvebL%2FRzDb1npozLBJeWtEAaZO0euIqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAqluhm1MWeX2nGMRircAyumjzoZHcCJdiyr0R0DLCGcPY7zz5gOX%2BqraS4PPkz3kl3PIdFSpdaXvSjgc0uLfI6VHp4dybmsJiSbB8QKK13%2FORjrkLZyoCYh8UPfPd5rKXXZKJba3HwP0GNmrR8l5C1TE%2Fd0lfiwKJViqSX%2FDItMjL9Ro%2Fdi9OIvDEPwNLnbmzx5SRctwBFxpPdPLlcQ%2B9%2FncPcTwzzp1oDcz4iV5Lok5mM7eIE%2Frz3EwTvoVcNf%2FABlBAygEjOH8J5nhl9L5oIj90HnxTa3W9oXBW96ralHcgT1b7IqjddjVMYGvxJI8bVOKNeLofBm8TTzyw9Q9%2FSiOi09afAHmwjwoW2yXRHs2Zo6b0J%2BpHxzY8MjYRkkXu55O%2FwS5I1rawGgP6A0eIyV%2FV1wzQUd1gpj2YF5oYNEGKdJDQI4Y1xwhqR9q56DfZcX9mA%2BJjyXERARhbrYPcqhc2L0njZgl%2FPgrJd3WS3yZ6VPqf7oZRVTLGKCHrFAL00iJm3OSW7GP0hZDGeObeIx%2B056xZzDNBeT0zmmMo1pLkzBkayDR3Az3UYi0kkeZUf2w6M82g5E%2FaFYguokahu%2FHUgByfkKGYh%2FmSHyHsLFijsbyKLuOuwWVam7pll1QZ7Bhx9FCsGcDLcbMLnb49cGOqUBt%2Fe3ZvRBkiVAn0UcYERRhvQ%2Fsd2VqNK2R3T6RqS3v4fLFvI1QVtfewAiPqnNLdeWs7nacZBTG5COCH15XegUExumhobMgVSByEINrM2tv2w7grA7apkxxuJi%2BYCtvIwYdyRqXaTmgfYW%2FekYemYFQJ9o4q8QbWSAhjjB9aQ4MD2Z85mXDDvDwyLW58nkCK%2FwLK%2Bywhwe91NB9Zi0%2B5q7jcwpieY9&X-Amz-Signature=97376d8970b15c6bb188c61c6df384f777c3c01650afa8058c64e1be21b1c6d5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCJDRKCC%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034619Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDCQKwa%2FrwEUw0NXRYf3Z828q2nzX44iJKvS5qn0x7FSAiEA42Fy7%2BxsB2Fl7zvebL%2FRzDb1npozLBJeWtEAaZO0euIqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAqluhm1MWeX2nGMRircAyumjzoZHcCJdiyr0R0DLCGcPY7zz5gOX%2BqraS4PPkz3kl3PIdFSpdaXvSjgc0uLfI6VHp4dybmsJiSbB8QKK13%2FORjrkLZyoCYh8UPfPd5rKXXZKJba3HwP0GNmrR8l5C1TE%2Fd0lfiwKJViqSX%2FDItMjL9Ro%2Fdi9OIvDEPwNLnbmzx5SRctwBFxpPdPLlcQ%2B9%2FncPcTwzzp1oDcz4iV5Lok5mM7eIE%2Frz3EwTvoVcNf%2FABlBAygEjOH8J5nhl9L5oIj90HnxTa3W9oXBW96ralHcgT1b7IqjddjVMYGvxJI8bVOKNeLofBm8TTzyw9Q9%2FSiOi09afAHmwjwoW2yXRHs2Zo6b0J%2BpHxzY8MjYRkkXu55O%2FwS5I1rawGgP6A0eIyV%2FV1wzQUd1gpj2YF5oYNEGKdJDQI4Y1xwhqR9q56DfZcX9mA%2BJjyXERARhbrYPcqhc2L0njZgl%2FPgrJd3WS3yZ6VPqf7oZRVTLGKCHrFAL00iJm3OSW7GP0hZDGeObeIx%2B056xZzDNBeT0zmmMo1pLkzBkayDR3Az3UYi0kkeZUf2w6M82g5E%2FaFYguokahu%2FHUgByfkKGYh%2FmSHyHsLFijsbyKLuOuwWVam7pll1QZ7Bhx9FCsGcDLcbMLnb49cGOqUBt%2Fe3ZvRBkiVAn0UcYERRhvQ%2Fsd2VqNK2R3T6RqS3v4fLFvI1QVtfewAiPqnNLdeWs7nacZBTG5COCH15XegUExumhobMgVSByEINrM2tv2w7grA7apkxxuJi%2BYCtvIwYdyRqXaTmgfYW%2FekYemYFQJ9o4q8QbWSAhjjB9aQ4MD2Z85mXDDvDwyLW58nkCK%2FwLK%2Bywhwe91NB9Zi0%2B5q7jcwpieY9&X-Amz-Signature=da305561b8cfc9c19eeef37491029a695a980d954f77f5838f564b28979c1127&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCJDRKCC%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034619Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDCQKwa%2FrwEUw0NXRYf3Z828q2nzX44iJKvS5qn0x7FSAiEA42Fy7%2BxsB2Fl7zvebL%2FRzDb1npozLBJeWtEAaZO0euIqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAqluhm1MWeX2nGMRircAyumjzoZHcCJdiyr0R0DLCGcPY7zz5gOX%2BqraS4PPkz3kl3PIdFSpdaXvSjgc0uLfI6VHp4dybmsJiSbB8QKK13%2FORjrkLZyoCYh8UPfPd5rKXXZKJba3HwP0GNmrR8l5C1TE%2Fd0lfiwKJViqSX%2FDItMjL9Ro%2Fdi9OIvDEPwNLnbmzx5SRctwBFxpPdPLlcQ%2B9%2FncPcTwzzp1oDcz4iV5Lok5mM7eIE%2Frz3EwTvoVcNf%2FABlBAygEjOH8J5nhl9L5oIj90HnxTa3W9oXBW96ralHcgT1b7IqjddjVMYGvxJI8bVOKNeLofBm8TTzyw9Q9%2FSiOi09afAHmwjwoW2yXRHs2Zo6b0J%2BpHxzY8MjYRkkXu55O%2FwS5I1rawGgP6A0eIyV%2FV1wzQUd1gpj2YF5oYNEGKdJDQI4Y1xwhqR9q56DfZcX9mA%2BJjyXERARhbrYPcqhc2L0njZgl%2FPgrJd3WS3yZ6VPqf7oZRVTLGKCHrFAL00iJm3OSW7GP0hZDGeObeIx%2B056xZzDNBeT0zmmMo1pLkzBkayDR3Az3UYi0kkeZUf2w6M82g5E%2FaFYguokahu%2FHUgByfkKGYh%2FmSHyHsLFijsbyKLuOuwWVam7pll1QZ7Bhx9FCsGcDLcbMLnb49cGOqUBt%2Fe3ZvRBkiVAn0UcYERRhvQ%2Fsd2VqNK2R3T6RqS3v4fLFvI1QVtfewAiPqnNLdeWs7nacZBTG5COCH15XegUExumhobMgVSByEINrM2tv2w7grA7apkxxuJi%2BYCtvIwYdyRqXaTmgfYW%2FekYemYFQJ9o4q8QbWSAhjjB9aQ4MD2Z85mXDDvDwyLW58nkCK%2FwLK%2Bywhwe91NB9Zi0%2B5q7jcwpieY9&X-Amz-Signature=1da21bc172a880d75236650a2c041e4d8e0ddfe012a2869cccc5fd807578b4c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
