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
fetched_at: '2026-09-04T01:57:45.516Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NPWKMJU%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015740Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQCWmo4jRrsU4VRdg%2Br9mPb8Xb3TH2ju%2FNOzHprKQSD0uQIgEGe%2FusMdAgCapckWS85H%2B9X8qKTiJluA1mNHmEDXOLQqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLWPQ%2FLa6tJCutORdyrcA804Q3OTlY0tpdc%2BuLZbnZyXEZO0yNLOjxxivCTGxKSlS4m2NtL%2FbOSAPdPDNMCqKUXc%2BrpjHdEepDPdOgbtj3xf2rYA29o4LeBISLtvCvWjF5p1TzXjyqMtX%2BXRGHvAkiVDEg950xdb96DP84zk2WrnAkIwEQrK%2FDrz4p1LPTDPQlK09V2slDIuhrPP%2BbBvuU9afLA2DxdZHy%2FmWL1O85kRCdOsvg3RvuHYqCpkM6Wu%2Fs3TzhbYwZ3Wvya9gwwf6a9r%2FHup3DNvrJV%2BXx7u1u1u3aia5D%2BYzTElZOEjENOe8%2B%2FbAjgBwFZ9ASy5kR0AWSaoeFcXiIgiA4bMsRnhN%2FnI2%2Bzx0ZUltdRf1Okn%2BG2C5PoJYFpILGmMfUQ4z53fkvatadjKl4fX0u1Ayb7q09QjOemX2jVjTjm27MAlQYU6G6k79clU5YyBfh7nhacnGxCX5zImqk6H8DN4rn1w0eJpB7yrII6W4bsGdk5bcLcnhkG1RJt9FCYmNhsXuvPmkHP%2BCJekj3zcZeqxZn07CAZwj25E9bqFBKM1mKCNMfYeQ0DBEGhEJnUr7aZszn4NObG1vGEr8LrQ7Ln2ZbSTWtA0qGp74LaALP1B4dpSvoEQ0BvARt6MBNJ%2FRVmmMN%2BL6NQGOqUBikO24EoJ%2F%2FK7B6DpqYVqkDcrvol3RULVivBrC0frC8TQbeA1c%2BYjQVLtoNTqouHeFtlKH%2B33bvL8TbE%2Fw95Rr%2F5wQTlyF%2Fh%2BT4W8ujRRbtmFZJfEOsnIq14M%2BvjWhF93el2sRKYuGFlQ7bswTnUQw9zN%2BD%2FYh1i6HmKm6iQr8iLu5fyZ7ooyaHTATNYHP67BTLXkqFB4TeB6Gr944xcMxqvUZAwA&X-Amz-Signature=c278d552a71c6df6fe999f6e6d0da2b4c37e6a314add663d29a8cf69a842136d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NPWKMJU%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015740Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQCWmo4jRrsU4VRdg%2Br9mPb8Xb3TH2ju%2FNOzHprKQSD0uQIgEGe%2FusMdAgCapckWS85H%2B9X8qKTiJluA1mNHmEDXOLQqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLWPQ%2FLa6tJCutORdyrcA804Q3OTlY0tpdc%2BuLZbnZyXEZO0yNLOjxxivCTGxKSlS4m2NtL%2FbOSAPdPDNMCqKUXc%2BrpjHdEepDPdOgbtj3xf2rYA29o4LeBISLtvCvWjF5p1TzXjyqMtX%2BXRGHvAkiVDEg950xdb96DP84zk2WrnAkIwEQrK%2FDrz4p1LPTDPQlK09V2slDIuhrPP%2BbBvuU9afLA2DxdZHy%2FmWL1O85kRCdOsvg3RvuHYqCpkM6Wu%2Fs3TzhbYwZ3Wvya9gwwf6a9r%2FHup3DNvrJV%2BXx7u1u1u3aia5D%2BYzTElZOEjENOe8%2B%2FbAjgBwFZ9ASy5kR0AWSaoeFcXiIgiA4bMsRnhN%2FnI2%2Bzx0ZUltdRf1Okn%2BG2C5PoJYFpILGmMfUQ4z53fkvatadjKl4fX0u1Ayb7q09QjOemX2jVjTjm27MAlQYU6G6k79clU5YyBfh7nhacnGxCX5zImqk6H8DN4rn1w0eJpB7yrII6W4bsGdk5bcLcnhkG1RJt9FCYmNhsXuvPmkHP%2BCJekj3zcZeqxZn07CAZwj25E9bqFBKM1mKCNMfYeQ0DBEGhEJnUr7aZszn4NObG1vGEr8LrQ7Ln2ZbSTWtA0qGp74LaALP1B4dpSvoEQ0BvARt6MBNJ%2FRVmmMN%2BL6NQGOqUBikO24EoJ%2F%2FK7B6DpqYVqkDcrvol3RULVivBrC0frC8TQbeA1c%2BYjQVLtoNTqouHeFtlKH%2B33bvL8TbE%2Fw95Rr%2F5wQTlyF%2Fh%2BT4W8ujRRbtmFZJfEOsnIq14M%2BvjWhF93el2sRKYuGFlQ7bswTnUQw9zN%2BD%2FYh1i6HmKm6iQr8iLu5fyZ7ooyaHTATNYHP67BTLXkqFB4TeB6Gr944xcMxqvUZAwA&X-Amz-Signature=47e95b1c24dac5797a7d5b0c3f5924a29110eba0bab81a5c363882a4ee1880bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NPWKMJU%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015740Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQCWmo4jRrsU4VRdg%2Br9mPb8Xb3TH2ju%2FNOzHprKQSD0uQIgEGe%2FusMdAgCapckWS85H%2B9X8qKTiJluA1mNHmEDXOLQqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLWPQ%2FLa6tJCutORdyrcA804Q3OTlY0tpdc%2BuLZbnZyXEZO0yNLOjxxivCTGxKSlS4m2NtL%2FbOSAPdPDNMCqKUXc%2BrpjHdEepDPdOgbtj3xf2rYA29o4LeBISLtvCvWjF5p1TzXjyqMtX%2BXRGHvAkiVDEg950xdb96DP84zk2WrnAkIwEQrK%2FDrz4p1LPTDPQlK09V2slDIuhrPP%2BbBvuU9afLA2DxdZHy%2FmWL1O85kRCdOsvg3RvuHYqCpkM6Wu%2Fs3TzhbYwZ3Wvya9gwwf6a9r%2FHup3DNvrJV%2BXx7u1u1u3aia5D%2BYzTElZOEjENOe8%2B%2FbAjgBwFZ9ASy5kR0AWSaoeFcXiIgiA4bMsRnhN%2FnI2%2Bzx0ZUltdRf1Okn%2BG2C5PoJYFpILGmMfUQ4z53fkvatadjKl4fX0u1Ayb7q09QjOemX2jVjTjm27MAlQYU6G6k79clU5YyBfh7nhacnGxCX5zImqk6H8DN4rn1w0eJpB7yrII6W4bsGdk5bcLcnhkG1RJt9FCYmNhsXuvPmkHP%2BCJekj3zcZeqxZn07CAZwj25E9bqFBKM1mKCNMfYeQ0DBEGhEJnUr7aZszn4NObG1vGEr8LrQ7Ln2ZbSTWtA0qGp74LaALP1B4dpSvoEQ0BvARt6MBNJ%2FRVmmMN%2BL6NQGOqUBikO24EoJ%2F%2FK7B6DpqYVqkDcrvol3RULVivBrC0frC8TQbeA1c%2BYjQVLtoNTqouHeFtlKH%2B33bvL8TbE%2Fw95Rr%2F5wQTlyF%2Fh%2BT4W8ujRRbtmFZJfEOsnIq14M%2BvjWhF93el2sRKYuGFlQ7bswTnUQw9zN%2BD%2FYh1i6HmKm6iQr8iLu5fyZ7ooyaHTATNYHP67BTLXkqFB4TeB6Gr944xcMxqvUZAwA&X-Amz-Signature=4a0ae350ae9164b66afa6fb2f4af05434556ca064cd4c03bb70e401b312395e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NPWKMJU%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015740Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQCWmo4jRrsU4VRdg%2Br9mPb8Xb3TH2ju%2FNOzHprKQSD0uQIgEGe%2FusMdAgCapckWS85H%2B9X8qKTiJluA1mNHmEDXOLQqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLWPQ%2FLa6tJCutORdyrcA804Q3OTlY0tpdc%2BuLZbnZyXEZO0yNLOjxxivCTGxKSlS4m2NtL%2FbOSAPdPDNMCqKUXc%2BrpjHdEepDPdOgbtj3xf2rYA29o4LeBISLtvCvWjF5p1TzXjyqMtX%2BXRGHvAkiVDEg950xdb96DP84zk2WrnAkIwEQrK%2FDrz4p1LPTDPQlK09V2slDIuhrPP%2BbBvuU9afLA2DxdZHy%2FmWL1O85kRCdOsvg3RvuHYqCpkM6Wu%2Fs3TzhbYwZ3Wvya9gwwf6a9r%2FHup3DNvrJV%2BXx7u1u1u3aia5D%2BYzTElZOEjENOe8%2B%2FbAjgBwFZ9ASy5kR0AWSaoeFcXiIgiA4bMsRnhN%2FnI2%2Bzx0ZUltdRf1Okn%2BG2C5PoJYFpILGmMfUQ4z53fkvatadjKl4fX0u1Ayb7q09QjOemX2jVjTjm27MAlQYU6G6k79clU5YyBfh7nhacnGxCX5zImqk6H8DN4rn1w0eJpB7yrII6W4bsGdk5bcLcnhkG1RJt9FCYmNhsXuvPmkHP%2BCJekj3zcZeqxZn07CAZwj25E9bqFBKM1mKCNMfYeQ0DBEGhEJnUr7aZszn4NObG1vGEr8LrQ7Ln2ZbSTWtA0qGp74LaALP1B4dpSvoEQ0BvARt6MBNJ%2FRVmmMN%2BL6NQGOqUBikO24EoJ%2F%2FK7B6DpqYVqkDcrvol3RULVivBrC0frC8TQbeA1c%2BYjQVLtoNTqouHeFtlKH%2B33bvL8TbE%2Fw95Rr%2F5wQTlyF%2Fh%2BT4W8ujRRbtmFZJfEOsnIq14M%2BvjWhF93el2sRKYuGFlQ7bswTnUQw9zN%2BD%2FYh1i6HmKm6iQr8iLu5fyZ7ooyaHTATNYHP67BTLXkqFB4TeB6Gr944xcMxqvUZAwA&X-Amz-Signature=96310e336f0566761f017887893fa9837025970eb110c7199c29f3c387b5446e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NPWKMJU%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015740Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQCWmo4jRrsU4VRdg%2Br9mPb8Xb3TH2ju%2FNOzHprKQSD0uQIgEGe%2FusMdAgCapckWS85H%2B9X8qKTiJluA1mNHmEDXOLQqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLWPQ%2FLa6tJCutORdyrcA804Q3OTlY0tpdc%2BuLZbnZyXEZO0yNLOjxxivCTGxKSlS4m2NtL%2FbOSAPdPDNMCqKUXc%2BrpjHdEepDPdOgbtj3xf2rYA29o4LeBISLtvCvWjF5p1TzXjyqMtX%2BXRGHvAkiVDEg950xdb96DP84zk2WrnAkIwEQrK%2FDrz4p1LPTDPQlK09V2slDIuhrPP%2BbBvuU9afLA2DxdZHy%2FmWL1O85kRCdOsvg3RvuHYqCpkM6Wu%2Fs3TzhbYwZ3Wvya9gwwf6a9r%2FHup3DNvrJV%2BXx7u1u1u3aia5D%2BYzTElZOEjENOe8%2B%2FbAjgBwFZ9ASy5kR0AWSaoeFcXiIgiA4bMsRnhN%2FnI2%2Bzx0ZUltdRf1Okn%2BG2C5PoJYFpILGmMfUQ4z53fkvatadjKl4fX0u1Ayb7q09QjOemX2jVjTjm27MAlQYU6G6k79clU5YyBfh7nhacnGxCX5zImqk6H8DN4rn1w0eJpB7yrII6W4bsGdk5bcLcnhkG1RJt9FCYmNhsXuvPmkHP%2BCJekj3zcZeqxZn07CAZwj25E9bqFBKM1mKCNMfYeQ0DBEGhEJnUr7aZszn4NObG1vGEr8LrQ7Ln2ZbSTWtA0qGp74LaALP1B4dpSvoEQ0BvARt6MBNJ%2FRVmmMN%2BL6NQGOqUBikO24EoJ%2F%2FK7B6DpqYVqkDcrvol3RULVivBrC0frC8TQbeA1c%2BYjQVLtoNTqouHeFtlKH%2B33bvL8TbE%2Fw95Rr%2F5wQTlyF%2Fh%2BT4W8ujRRbtmFZJfEOsnIq14M%2BvjWhF93el2sRKYuGFlQ7bswTnUQw9zN%2BD%2FYh1i6HmKm6iQr8iLu5fyZ7ooyaHTATNYHP67BTLXkqFB4TeB6Gr944xcMxqvUZAwA&X-Amz-Signature=755fabebb4684557cc192c6ffc1ff06bb7b3bd5adcdb9b9ab9e938a7b9829d86&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NPWKMJU%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015740Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQCWmo4jRrsU4VRdg%2Br9mPb8Xb3TH2ju%2FNOzHprKQSD0uQIgEGe%2FusMdAgCapckWS85H%2B9X8qKTiJluA1mNHmEDXOLQqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLWPQ%2FLa6tJCutORdyrcA804Q3OTlY0tpdc%2BuLZbnZyXEZO0yNLOjxxivCTGxKSlS4m2NtL%2FbOSAPdPDNMCqKUXc%2BrpjHdEepDPdOgbtj3xf2rYA29o4LeBISLtvCvWjF5p1TzXjyqMtX%2BXRGHvAkiVDEg950xdb96DP84zk2WrnAkIwEQrK%2FDrz4p1LPTDPQlK09V2slDIuhrPP%2BbBvuU9afLA2DxdZHy%2FmWL1O85kRCdOsvg3RvuHYqCpkM6Wu%2Fs3TzhbYwZ3Wvya9gwwf6a9r%2FHup3DNvrJV%2BXx7u1u1u3aia5D%2BYzTElZOEjENOe8%2B%2FbAjgBwFZ9ASy5kR0AWSaoeFcXiIgiA4bMsRnhN%2FnI2%2Bzx0ZUltdRf1Okn%2BG2C5PoJYFpILGmMfUQ4z53fkvatadjKl4fX0u1Ayb7q09QjOemX2jVjTjm27MAlQYU6G6k79clU5YyBfh7nhacnGxCX5zImqk6H8DN4rn1w0eJpB7yrII6W4bsGdk5bcLcnhkG1RJt9FCYmNhsXuvPmkHP%2BCJekj3zcZeqxZn07CAZwj25E9bqFBKM1mKCNMfYeQ0DBEGhEJnUr7aZszn4NObG1vGEr8LrQ7Ln2ZbSTWtA0qGp74LaALP1B4dpSvoEQ0BvARt6MBNJ%2FRVmmMN%2BL6NQGOqUBikO24EoJ%2F%2FK7B6DpqYVqkDcrvol3RULVivBrC0frC8TQbeA1c%2BYjQVLtoNTqouHeFtlKH%2B33bvL8TbE%2Fw95Rr%2F5wQTlyF%2Fh%2BT4W8ujRRbtmFZJfEOsnIq14M%2BvjWhF93el2sRKYuGFlQ7bswTnUQw9zN%2BD%2FYh1i6HmKm6iQr8iLu5fyZ7ooyaHTATNYHP67BTLXkqFB4TeB6Gr944xcMxqvUZAwA&X-Amz-Signature=fcb0567e121746eacae16d08a8c94d9417ec70edd0fd5e953cdf9767140dd300&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NPWKMJU%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015740Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQCWmo4jRrsU4VRdg%2Br9mPb8Xb3TH2ju%2FNOzHprKQSD0uQIgEGe%2FusMdAgCapckWS85H%2B9X8qKTiJluA1mNHmEDXOLQqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLWPQ%2FLa6tJCutORdyrcA804Q3OTlY0tpdc%2BuLZbnZyXEZO0yNLOjxxivCTGxKSlS4m2NtL%2FbOSAPdPDNMCqKUXc%2BrpjHdEepDPdOgbtj3xf2rYA29o4LeBISLtvCvWjF5p1TzXjyqMtX%2BXRGHvAkiVDEg950xdb96DP84zk2WrnAkIwEQrK%2FDrz4p1LPTDPQlK09V2slDIuhrPP%2BbBvuU9afLA2DxdZHy%2FmWL1O85kRCdOsvg3RvuHYqCpkM6Wu%2Fs3TzhbYwZ3Wvya9gwwf6a9r%2FHup3DNvrJV%2BXx7u1u1u3aia5D%2BYzTElZOEjENOe8%2B%2FbAjgBwFZ9ASy5kR0AWSaoeFcXiIgiA4bMsRnhN%2FnI2%2Bzx0ZUltdRf1Okn%2BG2C5PoJYFpILGmMfUQ4z53fkvatadjKl4fX0u1Ayb7q09QjOemX2jVjTjm27MAlQYU6G6k79clU5YyBfh7nhacnGxCX5zImqk6H8DN4rn1w0eJpB7yrII6W4bsGdk5bcLcnhkG1RJt9FCYmNhsXuvPmkHP%2BCJekj3zcZeqxZn07CAZwj25E9bqFBKM1mKCNMfYeQ0DBEGhEJnUr7aZszn4NObG1vGEr8LrQ7Ln2ZbSTWtA0qGp74LaALP1B4dpSvoEQ0BvARt6MBNJ%2FRVmmMN%2BL6NQGOqUBikO24EoJ%2F%2FK7B6DpqYVqkDcrvol3RULVivBrC0frC8TQbeA1c%2BYjQVLtoNTqouHeFtlKH%2B33bvL8TbE%2Fw95Rr%2F5wQTlyF%2Fh%2BT4W8ujRRbtmFZJfEOsnIq14M%2BvjWhF93el2sRKYuGFlQ7bswTnUQw9zN%2BD%2FYh1i6HmKm6iQr8iLu5fyZ7ooyaHTATNYHP67BTLXkqFB4TeB6Gr944xcMxqvUZAwA&X-Amz-Signature=2b76d1c4412b6c366a67160d29190164599daa198557e3043f649d3c40690acf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NPWKMJU%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015740Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQCWmo4jRrsU4VRdg%2Br9mPb8Xb3TH2ju%2FNOzHprKQSD0uQIgEGe%2FusMdAgCapckWS85H%2B9X8qKTiJluA1mNHmEDXOLQqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLWPQ%2FLa6tJCutORdyrcA804Q3OTlY0tpdc%2BuLZbnZyXEZO0yNLOjxxivCTGxKSlS4m2NtL%2FbOSAPdPDNMCqKUXc%2BrpjHdEepDPdOgbtj3xf2rYA29o4LeBISLtvCvWjF5p1TzXjyqMtX%2BXRGHvAkiVDEg950xdb96DP84zk2WrnAkIwEQrK%2FDrz4p1LPTDPQlK09V2slDIuhrPP%2BbBvuU9afLA2DxdZHy%2FmWL1O85kRCdOsvg3RvuHYqCpkM6Wu%2Fs3TzhbYwZ3Wvya9gwwf6a9r%2FHup3DNvrJV%2BXx7u1u1u3aia5D%2BYzTElZOEjENOe8%2B%2FbAjgBwFZ9ASy5kR0AWSaoeFcXiIgiA4bMsRnhN%2FnI2%2Bzx0ZUltdRf1Okn%2BG2C5PoJYFpILGmMfUQ4z53fkvatadjKl4fX0u1Ayb7q09QjOemX2jVjTjm27MAlQYU6G6k79clU5YyBfh7nhacnGxCX5zImqk6H8DN4rn1w0eJpB7yrII6W4bsGdk5bcLcnhkG1RJt9FCYmNhsXuvPmkHP%2BCJekj3zcZeqxZn07CAZwj25E9bqFBKM1mKCNMfYeQ0DBEGhEJnUr7aZszn4NObG1vGEr8LrQ7Ln2ZbSTWtA0qGp74LaALP1B4dpSvoEQ0BvARt6MBNJ%2FRVmmMN%2BL6NQGOqUBikO24EoJ%2F%2FK7B6DpqYVqkDcrvol3RULVivBrC0frC8TQbeA1c%2BYjQVLtoNTqouHeFtlKH%2B33bvL8TbE%2Fw95Rr%2F5wQTlyF%2Fh%2BT4W8ujRRbtmFZJfEOsnIq14M%2BvjWhF93el2sRKYuGFlQ7bswTnUQw9zN%2BD%2FYh1i6HmKm6iQr8iLu5fyZ7ooyaHTATNYHP67BTLXkqFB4TeB6Gr944xcMxqvUZAwA&X-Amz-Signature=e70c72f8ab14b87974129942fb398ee4ec29f0d578fa88203f57e5bbc30afb6e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NPWKMJU%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015740Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQCWmo4jRrsU4VRdg%2Br9mPb8Xb3TH2ju%2FNOzHprKQSD0uQIgEGe%2FusMdAgCapckWS85H%2B9X8qKTiJluA1mNHmEDXOLQqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLWPQ%2FLa6tJCutORdyrcA804Q3OTlY0tpdc%2BuLZbnZyXEZO0yNLOjxxivCTGxKSlS4m2NtL%2FbOSAPdPDNMCqKUXc%2BrpjHdEepDPdOgbtj3xf2rYA29o4LeBISLtvCvWjF5p1TzXjyqMtX%2BXRGHvAkiVDEg950xdb96DP84zk2WrnAkIwEQrK%2FDrz4p1LPTDPQlK09V2slDIuhrPP%2BbBvuU9afLA2DxdZHy%2FmWL1O85kRCdOsvg3RvuHYqCpkM6Wu%2Fs3TzhbYwZ3Wvya9gwwf6a9r%2FHup3DNvrJV%2BXx7u1u1u3aia5D%2BYzTElZOEjENOe8%2B%2FbAjgBwFZ9ASy5kR0AWSaoeFcXiIgiA4bMsRnhN%2FnI2%2Bzx0ZUltdRf1Okn%2BG2C5PoJYFpILGmMfUQ4z53fkvatadjKl4fX0u1Ayb7q09QjOemX2jVjTjm27MAlQYU6G6k79clU5YyBfh7nhacnGxCX5zImqk6H8DN4rn1w0eJpB7yrII6W4bsGdk5bcLcnhkG1RJt9FCYmNhsXuvPmkHP%2BCJekj3zcZeqxZn07CAZwj25E9bqFBKM1mKCNMfYeQ0DBEGhEJnUr7aZszn4NObG1vGEr8LrQ7Ln2ZbSTWtA0qGp74LaALP1B4dpSvoEQ0BvARt6MBNJ%2FRVmmMN%2BL6NQGOqUBikO24EoJ%2F%2FK7B6DpqYVqkDcrvol3RULVivBrC0frC8TQbeA1c%2BYjQVLtoNTqouHeFtlKH%2B33bvL8TbE%2Fw95Rr%2F5wQTlyF%2Fh%2BT4W8ujRRbtmFZJfEOsnIq14M%2BvjWhF93el2sRKYuGFlQ7bswTnUQw9zN%2BD%2FYh1i6HmKm6iQr8iLu5fyZ7ooyaHTATNYHP67BTLXkqFB4TeB6Gr944xcMxqvUZAwA&X-Amz-Signature=60ba94eff55e6aa930ac3fc3dbccf004575e1b6a597114a5aa534b04e966acc3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
