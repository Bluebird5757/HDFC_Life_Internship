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
fetched_at: '2026-10-23T02:30:47.538Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDP6NH7M%2F20261023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261023T023042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH9DqqGCR2IIqbqqVA0KIknj0%2B80Td2Q7iAI2e4o9dHCAiBNGf4GBcvJfz%2BSZX10eDwmwqQEPXzkmkn3mU5hBLdnzSqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMgtR3bSYY3DRkfuqiKtwDZ09y0njsY8y8Z%2BFoV4ADNjPwKemwhlpWz4IcRUyu46KmE3uP4DTFtYRR6jzMJ7dxsQYtGoxRIlfazQQ805gUy9iZTJB%2BEGMR3eXUHeZNkdJPEt67deBI2HhPwkP8ys2gVxv3IMLGTriSLvC3RRwwTbQET7CiNXdjcqvZm3CAUoMzRigwrKteaXch%2BOFtwVRB7ndMQeH3EAt48vh%2FXOQ%2FdHFwfF6u7%2F031E8nYORs32%2BK%2BlORk8iD9di3yLVLk1PeDNjFgVh4KeTgsma260HLIyCccXtx6wuOwdmV85pz9%2F5oA%2FkJtZn%2FFLIbFyDw9w1WeulP2BDgZ9nNqS2ovgcUGXifjm%2F0S9c6Gu01JJ%2F5jNQBk7uCkwSXeqHGRGzOyoZKBtITyVWQmA%2F0nP%2B5HQnGWMuOEXdMGv7lE9HNtpTQvQYdbuDGQWioxNFcNi1vNh4yTL%2FuNPfnvt5YOwKDu35n84uaosvq4ZebzMC%2F%2Fif0A6XqTyrcBnWauFaE1mxXWI7xBcW%2FdGsR3TPUC44f4IgLmk4fYm0rRAjK1H9hWkDiL8jHbl%2BjTD2H3HsZnY3MbKWKKw5VAA%2BJWx2IW2jXR0M1RaeiE34vDIqWlsHZ2pg2iV81HDY1j%2BILqjtBdpcw%2Bufq1gY6pgFvgdHQmByqOEM%2Fd5BI2l39VNme9bGSM2b8FoC%2FPqDXCPaR4z1MI%2FndPr7smYAGPAMjnB7%2FNwXjQ3EJBeFGHdGw23IFKTLXzHZMxQiqFSzpQGFTD0VN1uDodThK2xHeBYn%2FhKVbpLpiNgM%2BSJzRwzfjsM4ODnRaBDBUAI0og0bGxz3f82F15sUzm9DEs0r6HN6RFFof0KPZOLPT0RJOd1CJMSejI8h8&X-Amz-Signature=3d3daf0553372aa6f9f60baa20ce04c8030f26521b4dd43970236b02aa2ae7f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDP6NH7M%2F20261023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261023T023042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH9DqqGCR2IIqbqqVA0KIknj0%2B80Td2Q7iAI2e4o9dHCAiBNGf4GBcvJfz%2BSZX10eDwmwqQEPXzkmkn3mU5hBLdnzSqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMgtR3bSYY3DRkfuqiKtwDZ09y0njsY8y8Z%2BFoV4ADNjPwKemwhlpWz4IcRUyu46KmE3uP4DTFtYRR6jzMJ7dxsQYtGoxRIlfazQQ805gUy9iZTJB%2BEGMR3eXUHeZNkdJPEt67deBI2HhPwkP8ys2gVxv3IMLGTriSLvC3RRwwTbQET7CiNXdjcqvZm3CAUoMzRigwrKteaXch%2BOFtwVRB7ndMQeH3EAt48vh%2FXOQ%2FdHFwfF6u7%2F031E8nYORs32%2BK%2BlORk8iD9di3yLVLk1PeDNjFgVh4KeTgsma260HLIyCccXtx6wuOwdmV85pz9%2F5oA%2FkJtZn%2FFLIbFyDw9w1WeulP2BDgZ9nNqS2ovgcUGXifjm%2F0S9c6Gu01JJ%2F5jNQBk7uCkwSXeqHGRGzOyoZKBtITyVWQmA%2F0nP%2B5HQnGWMuOEXdMGv7lE9HNtpTQvQYdbuDGQWioxNFcNi1vNh4yTL%2FuNPfnvt5YOwKDu35n84uaosvq4ZebzMC%2F%2Fif0A6XqTyrcBnWauFaE1mxXWI7xBcW%2FdGsR3TPUC44f4IgLmk4fYm0rRAjK1H9hWkDiL8jHbl%2BjTD2H3HsZnY3MbKWKKw5VAA%2BJWx2IW2jXR0M1RaeiE34vDIqWlsHZ2pg2iV81HDY1j%2BILqjtBdpcw%2Bufq1gY6pgFvgdHQmByqOEM%2Fd5BI2l39VNme9bGSM2b8FoC%2FPqDXCPaR4z1MI%2FndPr7smYAGPAMjnB7%2FNwXjQ3EJBeFGHdGw23IFKTLXzHZMxQiqFSzpQGFTD0VN1uDodThK2xHeBYn%2FhKVbpLpiNgM%2BSJzRwzfjsM4ODnRaBDBUAI0og0bGxz3f82F15sUzm9DEs0r6HN6RFFof0KPZOLPT0RJOd1CJMSejI8h8&X-Amz-Signature=eeae5d841ba1f5b643bb33da9f3652a69c4194e63ca3c2d1bb3a8070cc412268&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDP6NH7M%2F20261023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261023T023042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH9DqqGCR2IIqbqqVA0KIknj0%2B80Td2Q7iAI2e4o9dHCAiBNGf4GBcvJfz%2BSZX10eDwmwqQEPXzkmkn3mU5hBLdnzSqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMgtR3bSYY3DRkfuqiKtwDZ09y0njsY8y8Z%2BFoV4ADNjPwKemwhlpWz4IcRUyu46KmE3uP4DTFtYRR6jzMJ7dxsQYtGoxRIlfazQQ805gUy9iZTJB%2BEGMR3eXUHeZNkdJPEt67deBI2HhPwkP8ys2gVxv3IMLGTriSLvC3RRwwTbQET7CiNXdjcqvZm3CAUoMzRigwrKteaXch%2BOFtwVRB7ndMQeH3EAt48vh%2FXOQ%2FdHFwfF6u7%2F031E8nYORs32%2BK%2BlORk8iD9di3yLVLk1PeDNjFgVh4KeTgsma260HLIyCccXtx6wuOwdmV85pz9%2F5oA%2FkJtZn%2FFLIbFyDw9w1WeulP2BDgZ9nNqS2ovgcUGXifjm%2F0S9c6Gu01JJ%2F5jNQBk7uCkwSXeqHGRGzOyoZKBtITyVWQmA%2F0nP%2B5HQnGWMuOEXdMGv7lE9HNtpTQvQYdbuDGQWioxNFcNi1vNh4yTL%2FuNPfnvt5YOwKDu35n84uaosvq4ZebzMC%2F%2Fif0A6XqTyrcBnWauFaE1mxXWI7xBcW%2FdGsR3TPUC44f4IgLmk4fYm0rRAjK1H9hWkDiL8jHbl%2BjTD2H3HsZnY3MbKWKKw5VAA%2BJWx2IW2jXR0M1RaeiE34vDIqWlsHZ2pg2iV81HDY1j%2BILqjtBdpcw%2Bufq1gY6pgFvgdHQmByqOEM%2Fd5BI2l39VNme9bGSM2b8FoC%2FPqDXCPaR4z1MI%2FndPr7smYAGPAMjnB7%2FNwXjQ3EJBeFGHdGw23IFKTLXzHZMxQiqFSzpQGFTD0VN1uDodThK2xHeBYn%2FhKVbpLpiNgM%2BSJzRwzfjsM4ODnRaBDBUAI0og0bGxz3f82F15sUzm9DEs0r6HN6RFFof0KPZOLPT0RJOd1CJMSejI8h8&X-Amz-Signature=86ccc653a817877319afb48d120a7f7fb48aeb08fcf78ba5862c6e406b419988&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDP6NH7M%2F20261023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261023T023042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH9DqqGCR2IIqbqqVA0KIknj0%2B80Td2Q7iAI2e4o9dHCAiBNGf4GBcvJfz%2BSZX10eDwmwqQEPXzkmkn3mU5hBLdnzSqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMgtR3bSYY3DRkfuqiKtwDZ09y0njsY8y8Z%2BFoV4ADNjPwKemwhlpWz4IcRUyu46KmE3uP4DTFtYRR6jzMJ7dxsQYtGoxRIlfazQQ805gUy9iZTJB%2BEGMR3eXUHeZNkdJPEt67deBI2HhPwkP8ys2gVxv3IMLGTriSLvC3RRwwTbQET7CiNXdjcqvZm3CAUoMzRigwrKteaXch%2BOFtwVRB7ndMQeH3EAt48vh%2FXOQ%2FdHFwfF6u7%2F031E8nYORs32%2BK%2BlORk8iD9di3yLVLk1PeDNjFgVh4KeTgsma260HLIyCccXtx6wuOwdmV85pz9%2F5oA%2FkJtZn%2FFLIbFyDw9w1WeulP2BDgZ9nNqS2ovgcUGXifjm%2F0S9c6Gu01JJ%2F5jNQBk7uCkwSXeqHGRGzOyoZKBtITyVWQmA%2F0nP%2B5HQnGWMuOEXdMGv7lE9HNtpTQvQYdbuDGQWioxNFcNi1vNh4yTL%2FuNPfnvt5YOwKDu35n84uaosvq4ZebzMC%2F%2Fif0A6XqTyrcBnWauFaE1mxXWI7xBcW%2FdGsR3TPUC44f4IgLmk4fYm0rRAjK1H9hWkDiL8jHbl%2BjTD2H3HsZnY3MbKWKKw5VAA%2BJWx2IW2jXR0M1RaeiE34vDIqWlsHZ2pg2iV81HDY1j%2BILqjtBdpcw%2Bufq1gY6pgFvgdHQmByqOEM%2Fd5BI2l39VNme9bGSM2b8FoC%2FPqDXCPaR4z1MI%2FndPr7smYAGPAMjnB7%2FNwXjQ3EJBeFGHdGw23IFKTLXzHZMxQiqFSzpQGFTD0VN1uDodThK2xHeBYn%2FhKVbpLpiNgM%2BSJzRwzfjsM4ODnRaBDBUAI0og0bGxz3f82F15sUzm9DEs0r6HN6RFFof0KPZOLPT0RJOd1CJMSejI8h8&X-Amz-Signature=9bd4263a0483ff601fd86f615087dd82dbaef43dbc6383d6f8f2c2760920e4ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDP6NH7M%2F20261023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261023T023042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH9DqqGCR2IIqbqqVA0KIknj0%2B80Td2Q7iAI2e4o9dHCAiBNGf4GBcvJfz%2BSZX10eDwmwqQEPXzkmkn3mU5hBLdnzSqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMgtR3bSYY3DRkfuqiKtwDZ09y0njsY8y8Z%2BFoV4ADNjPwKemwhlpWz4IcRUyu46KmE3uP4DTFtYRR6jzMJ7dxsQYtGoxRIlfazQQ805gUy9iZTJB%2BEGMR3eXUHeZNkdJPEt67deBI2HhPwkP8ys2gVxv3IMLGTriSLvC3RRwwTbQET7CiNXdjcqvZm3CAUoMzRigwrKteaXch%2BOFtwVRB7ndMQeH3EAt48vh%2FXOQ%2FdHFwfF6u7%2F031E8nYORs32%2BK%2BlORk8iD9di3yLVLk1PeDNjFgVh4KeTgsma260HLIyCccXtx6wuOwdmV85pz9%2F5oA%2FkJtZn%2FFLIbFyDw9w1WeulP2BDgZ9nNqS2ovgcUGXifjm%2F0S9c6Gu01JJ%2F5jNQBk7uCkwSXeqHGRGzOyoZKBtITyVWQmA%2F0nP%2B5HQnGWMuOEXdMGv7lE9HNtpTQvQYdbuDGQWioxNFcNi1vNh4yTL%2FuNPfnvt5YOwKDu35n84uaosvq4ZebzMC%2F%2Fif0A6XqTyrcBnWauFaE1mxXWI7xBcW%2FdGsR3TPUC44f4IgLmk4fYm0rRAjK1H9hWkDiL8jHbl%2BjTD2H3HsZnY3MbKWKKw5VAA%2BJWx2IW2jXR0M1RaeiE34vDIqWlsHZ2pg2iV81HDY1j%2BILqjtBdpcw%2Bufq1gY6pgFvgdHQmByqOEM%2Fd5BI2l39VNme9bGSM2b8FoC%2FPqDXCPaR4z1MI%2FndPr7smYAGPAMjnB7%2FNwXjQ3EJBeFGHdGw23IFKTLXzHZMxQiqFSzpQGFTD0VN1uDodThK2xHeBYn%2FhKVbpLpiNgM%2BSJzRwzfjsM4ODnRaBDBUAI0og0bGxz3f82F15sUzm9DEs0r6HN6RFFof0KPZOLPT0RJOd1CJMSejI8h8&X-Amz-Signature=e8809024af734b8fa62d63f02471779403bfe193ee1b6983b23f9eb04cc0175e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDP6NH7M%2F20261023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261023T023042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH9DqqGCR2IIqbqqVA0KIknj0%2B80Td2Q7iAI2e4o9dHCAiBNGf4GBcvJfz%2BSZX10eDwmwqQEPXzkmkn3mU5hBLdnzSqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMgtR3bSYY3DRkfuqiKtwDZ09y0njsY8y8Z%2BFoV4ADNjPwKemwhlpWz4IcRUyu46KmE3uP4DTFtYRR6jzMJ7dxsQYtGoxRIlfazQQ805gUy9iZTJB%2BEGMR3eXUHeZNkdJPEt67deBI2HhPwkP8ys2gVxv3IMLGTriSLvC3RRwwTbQET7CiNXdjcqvZm3CAUoMzRigwrKteaXch%2BOFtwVRB7ndMQeH3EAt48vh%2FXOQ%2FdHFwfF6u7%2F031E8nYORs32%2BK%2BlORk8iD9di3yLVLk1PeDNjFgVh4KeTgsma260HLIyCccXtx6wuOwdmV85pz9%2F5oA%2FkJtZn%2FFLIbFyDw9w1WeulP2BDgZ9nNqS2ovgcUGXifjm%2F0S9c6Gu01JJ%2F5jNQBk7uCkwSXeqHGRGzOyoZKBtITyVWQmA%2F0nP%2B5HQnGWMuOEXdMGv7lE9HNtpTQvQYdbuDGQWioxNFcNi1vNh4yTL%2FuNPfnvt5YOwKDu35n84uaosvq4ZebzMC%2F%2Fif0A6XqTyrcBnWauFaE1mxXWI7xBcW%2FdGsR3TPUC44f4IgLmk4fYm0rRAjK1H9hWkDiL8jHbl%2BjTD2H3HsZnY3MbKWKKw5VAA%2BJWx2IW2jXR0M1RaeiE34vDIqWlsHZ2pg2iV81HDY1j%2BILqjtBdpcw%2Bufq1gY6pgFvgdHQmByqOEM%2Fd5BI2l39VNme9bGSM2b8FoC%2FPqDXCPaR4z1MI%2FndPr7smYAGPAMjnB7%2FNwXjQ3EJBeFGHdGw23IFKTLXzHZMxQiqFSzpQGFTD0VN1uDodThK2xHeBYn%2FhKVbpLpiNgM%2BSJzRwzfjsM4ODnRaBDBUAI0og0bGxz3f82F15sUzm9DEs0r6HN6RFFof0KPZOLPT0RJOd1CJMSejI8h8&X-Amz-Signature=1ae1b3cc75211e7ce0736e8df4656985d0721d32d30725d0fabed421e7677cff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDP6NH7M%2F20261023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261023T023042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH9DqqGCR2IIqbqqVA0KIknj0%2B80Td2Q7iAI2e4o9dHCAiBNGf4GBcvJfz%2BSZX10eDwmwqQEPXzkmkn3mU5hBLdnzSqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMgtR3bSYY3DRkfuqiKtwDZ09y0njsY8y8Z%2BFoV4ADNjPwKemwhlpWz4IcRUyu46KmE3uP4DTFtYRR6jzMJ7dxsQYtGoxRIlfazQQ805gUy9iZTJB%2BEGMR3eXUHeZNkdJPEt67deBI2HhPwkP8ys2gVxv3IMLGTriSLvC3RRwwTbQET7CiNXdjcqvZm3CAUoMzRigwrKteaXch%2BOFtwVRB7ndMQeH3EAt48vh%2FXOQ%2FdHFwfF6u7%2F031E8nYORs32%2BK%2BlORk8iD9di3yLVLk1PeDNjFgVh4KeTgsma260HLIyCccXtx6wuOwdmV85pz9%2F5oA%2FkJtZn%2FFLIbFyDw9w1WeulP2BDgZ9nNqS2ovgcUGXifjm%2F0S9c6Gu01JJ%2F5jNQBk7uCkwSXeqHGRGzOyoZKBtITyVWQmA%2F0nP%2B5HQnGWMuOEXdMGv7lE9HNtpTQvQYdbuDGQWioxNFcNi1vNh4yTL%2FuNPfnvt5YOwKDu35n84uaosvq4ZebzMC%2F%2Fif0A6XqTyrcBnWauFaE1mxXWI7xBcW%2FdGsR3TPUC44f4IgLmk4fYm0rRAjK1H9hWkDiL8jHbl%2BjTD2H3HsZnY3MbKWKKw5VAA%2BJWx2IW2jXR0M1RaeiE34vDIqWlsHZ2pg2iV81HDY1j%2BILqjtBdpcw%2Bufq1gY6pgFvgdHQmByqOEM%2Fd5BI2l39VNme9bGSM2b8FoC%2FPqDXCPaR4z1MI%2FndPr7smYAGPAMjnB7%2FNwXjQ3EJBeFGHdGw23IFKTLXzHZMxQiqFSzpQGFTD0VN1uDodThK2xHeBYn%2FhKVbpLpiNgM%2BSJzRwzfjsM4ODnRaBDBUAI0og0bGxz3f82F15sUzm9DEs0r6HN6RFFof0KPZOLPT0RJOd1CJMSejI8h8&X-Amz-Signature=da8c52fb7e144a44f4782e03f9c14bcd4e1ef32d9b7e0e8dad923f11b8e96a56&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDP6NH7M%2F20261023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261023T023042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH9DqqGCR2IIqbqqVA0KIknj0%2B80Td2Q7iAI2e4o9dHCAiBNGf4GBcvJfz%2BSZX10eDwmwqQEPXzkmkn3mU5hBLdnzSqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMgtR3bSYY3DRkfuqiKtwDZ09y0njsY8y8Z%2BFoV4ADNjPwKemwhlpWz4IcRUyu46KmE3uP4DTFtYRR6jzMJ7dxsQYtGoxRIlfazQQ805gUy9iZTJB%2BEGMR3eXUHeZNkdJPEt67deBI2HhPwkP8ys2gVxv3IMLGTriSLvC3RRwwTbQET7CiNXdjcqvZm3CAUoMzRigwrKteaXch%2BOFtwVRB7ndMQeH3EAt48vh%2FXOQ%2FdHFwfF6u7%2F031E8nYORs32%2BK%2BlORk8iD9di3yLVLk1PeDNjFgVh4KeTgsma260HLIyCccXtx6wuOwdmV85pz9%2F5oA%2FkJtZn%2FFLIbFyDw9w1WeulP2BDgZ9nNqS2ovgcUGXifjm%2F0S9c6Gu01JJ%2F5jNQBk7uCkwSXeqHGRGzOyoZKBtITyVWQmA%2F0nP%2B5HQnGWMuOEXdMGv7lE9HNtpTQvQYdbuDGQWioxNFcNi1vNh4yTL%2FuNPfnvt5YOwKDu35n84uaosvq4ZebzMC%2F%2Fif0A6XqTyrcBnWauFaE1mxXWI7xBcW%2FdGsR3TPUC44f4IgLmk4fYm0rRAjK1H9hWkDiL8jHbl%2BjTD2H3HsZnY3MbKWKKw5VAA%2BJWx2IW2jXR0M1RaeiE34vDIqWlsHZ2pg2iV81HDY1j%2BILqjtBdpcw%2Bufq1gY6pgFvgdHQmByqOEM%2Fd5BI2l39VNme9bGSM2b8FoC%2FPqDXCPaR4z1MI%2FndPr7smYAGPAMjnB7%2FNwXjQ3EJBeFGHdGw23IFKTLXzHZMxQiqFSzpQGFTD0VN1uDodThK2xHeBYn%2FhKVbpLpiNgM%2BSJzRwzfjsM4ODnRaBDBUAI0og0bGxz3f82F15sUzm9DEs0r6HN6RFFof0KPZOLPT0RJOd1CJMSejI8h8&X-Amz-Signature=160984cea3109782d0c06cb462674211b3bdc8ea6fffde6ba621d5b09a5b69bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDP6NH7M%2F20261023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261023T023042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH9DqqGCR2IIqbqqVA0KIknj0%2B80Td2Q7iAI2e4o9dHCAiBNGf4GBcvJfz%2BSZX10eDwmwqQEPXzkmkn3mU5hBLdnzSqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMgtR3bSYY3DRkfuqiKtwDZ09y0njsY8y8Z%2BFoV4ADNjPwKemwhlpWz4IcRUyu46KmE3uP4DTFtYRR6jzMJ7dxsQYtGoxRIlfazQQ805gUy9iZTJB%2BEGMR3eXUHeZNkdJPEt67deBI2HhPwkP8ys2gVxv3IMLGTriSLvC3RRwwTbQET7CiNXdjcqvZm3CAUoMzRigwrKteaXch%2BOFtwVRB7ndMQeH3EAt48vh%2FXOQ%2FdHFwfF6u7%2F031E8nYORs32%2BK%2BlORk8iD9di3yLVLk1PeDNjFgVh4KeTgsma260HLIyCccXtx6wuOwdmV85pz9%2F5oA%2FkJtZn%2FFLIbFyDw9w1WeulP2BDgZ9nNqS2ovgcUGXifjm%2F0S9c6Gu01JJ%2F5jNQBk7uCkwSXeqHGRGzOyoZKBtITyVWQmA%2F0nP%2B5HQnGWMuOEXdMGv7lE9HNtpTQvQYdbuDGQWioxNFcNi1vNh4yTL%2FuNPfnvt5YOwKDu35n84uaosvq4ZebzMC%2F%2Fif0A6XqTyrcBnWauFaE1mxXWI7xBcW%2FdGsR3TPUC44f4IgLmk4fYm0rRAjK1H9hWkDiL8jHbl%2BjTD2H3HsZnY3MbKWKKw5VAA%2BJWx2IW2jXR0M1RaeiE34vDIqWlsHZ2pg2iV81HDY1j%2BILqjtBdpcw%2Bufq1gY6pgFvgdHQmByqOEM%2Fd5BI2l39VNme9bGSM2b8FoC%2FPqDXCPaR4z1MI%2FndPr7smYAGPAMjnB7%2FNwXjQ3EJBeFGHdGw23IFKTLXzHZMxQiqFSzpQGFTD0VN1uDodThK2xHeBYn%2FhKVbpLpiNgM%2BSJzRwzfjsM4ODnRaBDBUAI0og0bGxz3f82F15sUzm9DEs0r6HN6RFFof0KPZOLPT0RJOd1CJMSejI8h8&X-Amz-Signature=544b480c2a49f5e9420ef3d8c9ac38c5a2f2901d2b1a6f41d2d6f1aca9e0e8df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
