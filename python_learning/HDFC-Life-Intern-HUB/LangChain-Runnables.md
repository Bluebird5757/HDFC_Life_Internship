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
fetched_at: '2026-11-19T04:59:39.455Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BYJ6CXR%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQCf7015EzYiJiirX%2FGVWwuCw8Z37B1KKvpwfDXQ%2FejGFwIgPFiIR72Q%2BXcRhjkUdE%2FOHcfoD%2FoxAO%2FFj25DZ%2FIqUUMq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDPrv4dNSIli4397SYyrcA3BWvPdpGIWt5dY2fI0%2BLeANIDKl52LwMrUfWAf8X59ZUz47BwvzkYwJ2Zhgb5PASyMEc%2FbGanOR%2BskD0dB2R%2FbrmTKkURfBrkI3Yqre9EaLKwQb%2BSvKIJ%2FSXts7j6WjOqb7bILq4Fx1OEqvFQ37mwIV17hEJOmExQd2m6%2BU0VqzJ2XLB5Yx0zbTCjUQYdD%2FJEEBxrs6iTTLzeZKySbxYFhFC1KLyobCUSPQKbSJl3TNbsypavvQB1RRhmYa3M8lGbm6ob7mDON8aTaYt08brax4f7A3uNoPv358DhYCZZsEBpN6I3sJz5mFrYkk7%2BiGSknpEoTIvhqvvMG5uA4bmwNXNRJfXjGq4eL2L6C9UVHf9n%2B5y1A7dBDJcXyz4koRL4XkuwlQYaQdXsja%2BSAr88t9j9NE%2BwvynjYclmG1nAKQAOpxw2XWpe0r1MwSk4Ds2CRv6ZN0TO2gptCXRQj8g3WT6DN8z5P6OqmnRGWADWDBeFxJwB6uzL0As3bjy62jjlqFDjfyAfL281MXwEZqhrUH99T%2BnGx26Ir5bDiATmYSSoj7jC5IoS3GUA%2FnLraO%2BPz6%2FmdAt8o9IWilsSwy3NgOEP9qllr7u2NCDXPLY6Xhtyd1rjKZCDcp%2B%2BWxMNbw%2BdcGOqUBCL%2BRSqH6LLNte6mmNjzhJwsreU0LKxMsgWkT9CWY2GjMHCoG6t9tH9l0mD8smZKkCO0ltme1kCiHhPAucUGvKgG514nB2cqytwDvALRoYlrGUw37ujHmTHBWryL4m1iRNFLHgjO66nZtESh2Rgm6d2XtZ%2BH%2FHoWESeSC0y7sjgx6VCowE8%2FeiYetPrs2HtEpqIcMbExQXuJ3r85C0sgeh0ovsLJk&X-Amz-Signature=6083c1836b23f84d73bd7de069f58f2d4d413fadc32738ca0b47a626354f9e8d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BYJ6CXR%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQCf7015EzYiJiirX%2FGVWwuCw8Z37B1KKvpwfDXQ%2FejGFwIgPFiIR72Q%2BXcRhjkUdE%2FOHcfoD%2FoxAO%2FFj25DZ%2FIqUUMq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDPrv4dNSIli4397SYyrcA3BWvPdpGIWt5dY2fI0%2BLeANIDKl52LwMrUfWAf8X59ZUz47BwvzkYwJ2Zhgb5PASyMEc%2FbGanOR%2BskD0dB2R%2FbrmTKkURfBrkI3Yqre9EaLKwQb%2BSvKIJ%2FSXts7j6WjOqb7bILq4Fx1OEqvFQ37mwIV17hEJOmExQd2m6%2BU0VqzJ2XLB5Yx0zbTCjUQYdD%2FJEEBxrs6iTTLzeZKySbxYFhFC1KLyobCUSPQKbSJl3TNbsypavvQB1RRhmYa3M8lGbm6ob7mDON8aTaYt08brax4f7A3uNoPv358DhYCZZsEBpN6I3sJz5mFrYkk7%2BiGSknpEoTIvhqvvMG5uA4bmwNXNRJfXjGq4eL2L6C9UVHf9n%2B5y1A7dBDJcXyz4koRL4XkuwlQYaQdXsja%2BSAr88t9j9NE%2BwvynjYclmG1nAKQAOpxw2XWpe0r1MwSk4Ds2CRv6ZN0TO2gptCXRQj8g3WT6DN8z5P6OqmnRGWADWDBeFxJwB6uzL0As3bjy62jjlqFDjfyAfL281MXwEZqhrUH99T%2BnGx26Ir5bDiATmYSSoj7jC5IoS3GUA%2FnLraO%2BPz6%2FmdAt8o9IWilsSwy3NgOEP9qllr7u2NCDXPLY6Xhtyd1rjKZCDcp%2B%2BWxMNbw%2BdcGOqUBCL%2BRSqH6LLNte6mmNjzhJwsreU0LKxMsgWkT9CWY2GjMHCoG6t9tH9l0mD8smZKkCO0ltme1kCiHhPAucUGvKgG514nB2cqytwDvALRoYlrGUw37ujHmTHBWryL4m1iRNFLHgjO66nZtESh2Rgm6d2XtZ%2BH%2FHoWESeSC0y7sjgx6VCowE8%2FeiYetPrs2HtEpqIcMbExQXuJ3r85C0sgeh0ovsLJk&X-Amz-Signature=31e11d0b00549206863249a1c70d26cc2288b5eeb7a3453604129f47710d2e9f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BYJ6CXR%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQCf7015EzYiJiirX%2FGVWwuCw8Z37B1KKvpwfDXQ%2FejGFwIgPFiIR72Q%2BXcRhjkUdE%2FOHcfoD%2FoxAO%2FFj25DZ%2FIqUUMq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDPrv4dNSIli4397SYyrcA3BWvPdpGIWt5dY2fI0%2BLeANIDKl52LwMrUfWAf8X59ZUz47BwvzkYwJ2Zhgb5PASyMEc%2FbGanOR%2BskD0dB2R%2FbrmTKkURfBrkI3Yqre9EaLKwQb%2BSvKIJ%2FSXts7j6WjOqb7bILq4Fx1OEqvFQ37mwIV17hEJOmExQd2m6%2BU0VqzJ2XLB5Yx0zbTCjUQYdD%2FJEEBxrs6iTTLzeZKySbxYFhFC1KLyobCUSPQKbSJl3TNbsypavvQB1RRhmYa3M8lGbm6ob7mDON8aTaYt08brax4f7A3uNoPv358DhYCZZsEBpN6I3sJz5mFrYkk7%2BiGSknpEoTIvhqvvMG5uA4bmwNXNRJfXjGq4eL2L6C9UVHf9n%2B5y1A7dBDJcXyz4koRL4XkuwlQYaQdXsja%2BSAr88t9j9NE%2BwvynjYclmG1nAKQAOpxw2XWpe0r1MwSk4Ds2CRv6ZN0TO2gptCXRQj8g3WT6DN8z5P6OqmnRGWADWDBeFxJwB6uzL0As3bjy62jjlqFDjfyAfL281MXwEZqhrUH99T%2BnGx26Ir5bDiATmYSSoj7jC5IoS3GUA%2FnLraO%2BPz6%2FmdAt8o9IWilsSwy3NgOEP9qllr7u2NCDXPLY6Xhtyd1rjKZCDcp%2B%2BWxMNbw%2BdcGOqUBCL%2BRSqH6LLNte6mmNjzhJwsreU0LKxMsgWkT9CWY2GjMHCoG6t9tH9l0mD8smZKkCO0ltme1kCiHhPAucUGvKgG514nB2cqytwDvALRoYlrGUw37ujHmTHBWryL4m1iRNFLHgjO66nZtESh2Rgm6d2XtZ%2BH%2FHoWESeSC0y7sjgx6VCowE8%2FeiYetPrs2HtEpqIcMbExQXuJ3r85C0sgeh0ovsLJk&X-Amz-Signature=c5d15ccd365bc395cca072f06b0efded5e9699cb9f066d1a3bbb039e5975332a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BYJ6CXR%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQCf7015EzYiJiirX%2FGVWwuCw8Z37B1KKvpwfDXQ%2FejGFwIgPFiIR72Q%2BXcRhjkUdE%2FOHcfoD%2FoxAO%2FFj25DZ%2FIqUUMq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDPrv4dNSIli4397SYyrcA3BWvPdpGIWt5dY2fI0%2BLeANIDKl52LwMrUfWAf8X59ZUz47BwvzkYwJ2Zhgb5PASyMEc%2FbGanOR%2BskD0dB2R%2FbrmTKkURfBrkI3Yqre9EaLKwQb%2BSvKIJ%2FSXts7j6WjOqb7bILq4Fx1OEqvFQ37mwIV17hEJOmExQd2m6%2BU0VqzJ2XLB5Yx0zbTCjUQYdD%2FJEEBxrs6iTTLzeZKySbxYFhFC1KLyobCUSPQKbSJl3TNbsypavvQB1RRhmYa3M8lGbm6ob7mDON8aTaYt08brax4f7A3uNoPv358DhYCZZsEBpN6I3sJz5mFrYkk7%2BiGSknpEoTIvhqvvMG5uA4bmwNXNRJfXjGq4eL2L6C9UVHf9n%2B5y1A7dBDJcXyz4koRL4XkuwlQYaQdXsja%2BSAr88t9j9NE%2BwvynjYclmG1nAKQAOpxw2XWpe0r1MwSk4Ds2CRv6ZN0TO2gptCXRQj8g3WT6DN8z5P6OqmnRGWADWDBeFxJwB6uzL0As3bjy62jjlqFDjfyAfL281MXwEZqhrUH99T%2BnGx26Ir5bDiATmYSSoj7jC5IoS3GUA%2FnLraO%2BPz6%2FmdAt8o9IWilsSwy3NgOEP9qllr7u2NCDXPLY6Xhtyd1rjKZCDcp%2B%2BWxMNbw%2BdcGOqUBCL%2BRSqH6LLNte6mmNjzhJwsreU0LKxMsgWkT9CWY2GjMHCoG6t9tH9l0mD8smZKkCO0ltme1kCiHhPAucUGvKgG514nB2cqytwDvALRoYlrGUw37ujHmTHBWryL4m1iRNFLHgjO66nZtESh2Rgm6d2XtZ%2BH%2FHoWESeSC0y7sjgx6VCowE8%2FeiYetPrs2HtEpqIcMbExQXuJ3r85C0sgeh0ovsLJk&X-Amz-Signature=162fdfb45753d9b9d3af88497404863b8149aeedb2bff1207234be3d5635954d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BYJ6CXR%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQCf7015EzYiJiirX%2FGVWwuCw8Z37B1KKvpwfDXQ%2FejGFwIgPFiIR72Q%2BXcRhjkUdE%2FOHcfoD%2FoxAO%2FFj25DZ%2FIqUUMq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDPrv4dNSIli4397SYyrcA3BWvPdpGIWt5dY2fI0%2BLeANIDKl52LwMrUfWAf8X59ZUz47BwvzkYwJ2Zhgb5PASyMEc%2FbGanOR%2BskD0dB2R%2FbrmTKkURfBrkI3Yqre9EaLKwQb%2BSvKIJ%2FSXts7j6WjOqb7bILq4Fx1OEqvFQ37mwIV17hEJOmExQd2m6%2BU0VqzJ2XLB5Yx0zbTCjUQYdD%2FJEEBxrs6iTTLzeZKySbxYFhFC1KLyobCUSPQKbSJl3TNbsypavvQB1RRhmYa3M8lGbm6ob7mDON8aTaYt08brax4f7A3uNoPv358DhYCZZsEBpN6I3sJz5mFrYkk7%2BiGSknpEoTIvhqvvMG5uA4bmwNXNRJfXjGq4eL2L6C9UVHf9n%2B5y1A7dBDJcXyz4koRL4XkuwlQYaQdXsja%2BSAr88t9j9NE%2BwvynjYclmG1nAKQAOpxw2XWpe0r1MwSk4Ds2CRv6ZN0TO2gptCXRQj8g3WT6DN8z5P6OqmnRGWADWDBeFxJwB6uzL0As3bjy62jjlqFDjfyAfL281MXwEZqhrUH99T%2BnGx26Ir5bDiATmYSSoj7jC5IoS3GUA%2FnLraO%2BPz6%2FmdAt8o9IWilsSwy3NgOEP9qllr7u2NCDXPLY6Xhtyd1rjKZCDcp%2B%2BWxMNbw%2BdcGOqUBCL%2BRSqH6LLNte6mmNjzhJwsreU0LKxMsgWkT9CWY2GjMHCoG6t9tH9l0mD8smZKkCO0ltme1kCiHhPAucUGvKgG514nB2cqytwDvALRoYlrGUw37ujHmTHBWryL4m1iRNFLHgjO66nZtESh2Rgm6d2XtZ%2BH%2FHoWESeSC0y7sjgx6VCowE8%2FeiYetPrs2HtEpqIcMbExQXuJ3r85C0sgeh0ovsLJk&X-Amz-Signature=be77b02da583fe8f8289d826fda482ad8cb3dde02e9a0c52ee1c2168e2296f74&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BYJ6CXR%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQCf7015EzYiJiirX%2FGVWwuCw8Z37B1KKvpwfDXQ%2FejGFwIgPFiIR72Q%2BXcRhjkUdE%2FOHcfoD%2FoxAO%2FFj25DZ%2FIqUUMq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDPrv4dNSIli4397SYyrcA3BWvPdpGIWt5dY2fI0%2BLeANIDKl52LwMrUfWAf8X59ZUz47BwvzkYwJ2Zhgb5PASyMEc%2FbGanOR%2BskD0dB2R%2FbrmTKkURfBrkI3Yqre9EaLKwQb%2BSvKIJ%2FSXts7j6WjOqb7bILq4Fx1OEqvFQ37mwIV17hEJOmExQd2m6%2BU0VqzJ2XLB5Yx0zbTCjUQYdD%2FJEEBxrs6iTTLzeZKySbxYFhFC1KLyobCUSPQKbSJl3TNbsypavvQB1RRhmYa3M8lGbm6ob7mDON8aTaYt08brax4f7A3uNoPv358DhYCZZsEBpN6I3sJz5mFrYkk7%2BiGSknpEoTIvhqvvMG5uA4bmwNXNRJfXjGq4eL2L6C9UVHf9n%2B5y1A7dBDJcXyz4koRL4XkuwlQYaQdXsja%2BSAr88t9j9NE%2BwvynjYclmG1nAKQAOpxw2XWpe0r1MwSk4Ds2CRv6ZN0TO2gptCXRQj8g3WT6DN8z5P6OqmnRGWADWDBeFxJwB6uzL0As3bjy62jjlqFDjfyAfL281MXwEZqhrUH99T%2BnGx26Ir5bDiATmYSSoj7jC5IoS3GUA%2FnLraO%2BPz6%2FmdAt8o9IWilsSwy3NgOEP9qllr7u2NCDXPLY6Xhtyd1rjKZCDcp%2B%2BWxMNbw%2BdcGOqUBCL%2BRSqH6LLNte6mmNjzhJwsreU0LKxMsgWkT9CWY2GjMHCoG6t9tH9l0mD8smZKkCO0ltme1kCiHhPAucUGvKgG514nB2cqytwDvALRoYlrGUw37ujHmTHBWryL4m1iRNFLHgjO66nZtESh2Rgm6d2XtZ%2BH%2FHoWESeSC0y7sjgx6VCowE8%2FeiYetPrs2HtEpqIcMbExQXuJ3r85C0sgeh0ovsLJk&X-Amz-Signature=5cdd1890c20536242b7688fac8cbd9eea64713380aba091b5b8c0cace6b8e745&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BYJ6CXR%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQCf7015EzYiJiirX%2FGVWwuCw8Z37B1KKvpwfDXQ%2FejGFwIgPFiIR72Q%2BXcRhjkUdE%2FOHcfoD%2FoxAO%2FFj25DZ%2FIqUUMq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDPrv4dNSIli4397SYyrcA3BWvPdpGIWt5dY2fI0%2BLeANIDKl52LwMrUfWAf8X59ZUz47BwvzkYwJ2Zhgb5PASyMEc%2FbGanOR%2BskD0dB2R%2FbrmTKkURfBrkI3Yqre9EaLKwQb%2BSvKIJ%2FSXts7j6WjOqb7bILq4Fx1OEqvFQ37mwIV17hEJOmExQd2m6%2BU0VqzJ2XLB5Yx0zbTCjUQYdD%2FJEEBxrs6iTTLzeZKySbxYFhFC1KLyobCUSPQKbSJl3TNbsypavvQB1RRhmYa3M8lGbm6ob7mDON8aTaYt08brax4f7A3uNoPv358DhYCZZsEBpN6I3sJz5mFrYkk7%2BiGSknpEoTIvhqvvMG5uA4bmwNXNRJfXjGq4eL2L6C9UVHf9n%2B5y1A7dBDJcXyz4koRL4XkuwlQYaQdXsja%2BSAr88t9j9NE%2BwvynjYclmG1nAKQAOpxw2XWpe0r1MwSk4Ds2CRv6ZN0TO2gptCXRQj8g3WT6DN8z5P6OqmnRGWADWDBeFxJwB6uzL0As3bjy62jjlqFDjfyAfL281MXwEZqhrUH99T%2BnGx26Ir5bDiATmYSSoj7jC5IoS3GUA%2FnLraO%2BPz6%2FmdAt8o9IWilsSwy3NgOEP9qllr7u2NCDXPLY6Xhtyd1rjKZCDcp%2B%2BWxMNbw%2BdcGOqUBCL%2BRSqH6LLNte6mmNjzhJwsreU0LKxMsgWkT9CWY2GjMHCoG6t9tH9l0mD8smZKkCO0ltme1kCiHhPAucUGvKgG514nB2cqytwDvALRoYlrGUw37ujHmTHBWryL4m1iRNFLHgjO66nZtESh2Rgm6d2XtZ%2BH%2FHoWESeSC0y7sjgx6VCowE8%2FeiYetPrs2HtEpqIcMbExQXuJ3r85C0sgeh0ovsLJk&X-Amz-Signature=0dad54448717d6eb9558eb4f9cb63e383840e19bc35c75d6d215a5b83651a7b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BYJ6CXR%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQCf7015EzYiJiirX%2FGVWwuCw8Z37B1KKvpwfDXQ%2FejGFwIgPFiIR72Q%2BXcRhjkUdE%2FOHcfoD%2FoxAO%2FFj25DZ%2FIqUUMq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDPrv4dNSIli4397SYyrcA3BWvPdpGIWt5dY2fI0%2BLeANIDKl52LwMrUfWAf8X59ZUz47BwvzkYwJ2Zhgb5PASyMEc%2FbGanOR%2BskD0dB2R%2FbrmTKkURfBrkI3Yqre9EaLKwQb%2BSvKIJ%2FSXts7j6WjOqb7bILq4Fx1OEqvFQ37mwIV17hEJOmExQd2m6%2BU0VqzJ2XLB5Yx0zbTCjUQYdD%2FJEEBxrs6iTTLzeZKySbxYFhFC1KLyobCUSPQKbSJl3TNbsypavvQB1RRhmYa3M8lGbm6ob7mDON8aTaYt08brax4f7A3uNoPv358DhYCZZsEBpN6I3sJz5mFrYkk7%2BiGSknpEoTIvhqvvMG5uA4bmwNXNRJfXjGq4eL2L6C9UVHf9n%2B5y1A7dBDJcXyz4koRL4XkuwlQYaQdXsja%2BSAr88t9j9NE%2BwvynjYclmG1nAKQAOpxw2XWpe0r1MwSk4Ds2CRv6ZN0TO2gptCXRQj8g3WT6DN8z5P6OqmnRGWADWDBeFxJwB6uzL0As3bjy62jjlqFDjfyAfL281MXwEZqhrUH99T%2BnGx26Ir5bDiATmYSSoj7jC5IoS3GUA%2FnLraO%2BPz6%2FmdAt8o9IWilsSwy3NgOEP9qllr7u2NCDXPLY6Xhtyd1rjKZCDcp%2B%2BWxMNbw%2BdcGOqUBCL%2BRSqH6LLNte6mmNjzhJwsreU0LKxMsgWkT9CWY2GjMHCoG6t9tH9l0mD8smZKkCO0ltme1kCiHhPAucUGvKgG514nB2cqytwDvALRoYlrGUw37ujHmTHBWryL4m1iRNFLHgjO66nZtESh2Rgm6d2XtZ%2BH%2FHoWESeSC0y7sjgx6VCowE8%2FeiYetPrs2HtEpqIcMbExQXuJ3r85C0sgeh0ovsLJk&X-Amz-Signature=2cda520e626af2f26674e39f179a559aa53ad07e6642ba1d0bddbf260161543a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BYJ6CXR%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQCf7015EzYiJiirX%2FGVWwuCw8Z37B1KKvpwfDXQ%2FejGFwIgPFiIR72Q%2BXcRhjkUdE%2FOHcfoD%2FoxAO%2FFj25DZ%2FIqUUMq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDPrv4dNSIli4397SYyrcA3BWvPdpGIWt5dY2fI0%2BLeANIDKl52LwMrUfWAf8X59ZUz47BwvzkYwJ2Zhgb5PASyMEc%2FbGanOR%2BskD0dB2R%2FbrmTKkURfBrkI3Yqre9EaLKwQb%2BSvKIJ%2FSXts7j6WjOqb7bILq4Fx1OEqvFQ37mwIV17hEJOmExQd2m6%2BU0VqzJ2XLB5Yx0zbTCjUQYdD%2FJEEBxrs6iTTLzeZKySbxYFhFC1KLyobCUSPQKbSJl3TNbsypavvQB1RRhmYa3M8lGbm6ob7mDON8aTaYt08brax4f7A3uNoPv358DhYCZZsEBpN6I3sJz5mFrYkk7%2BiGSknpEoTIvhqvvMG5uA4bmwNXNRJfXjGq4eL2L6C9UVHf9n%2B5y1A7dBDJcXyz4koRL4XkuwlQYaQdXsja%2BSAr88t9j9NE%2BwvynjYclmG1nAKQAOpxw2XWpe0r1MwSk4Ds2CRv6ZN0TO2gptCXRQj8g3WT6DN8z5P6OqmnRGWADWDBeFxJwB6uzL0As3bjy62jjlqFDjfyAfL281MXwEZqhrUH99T%2BnGx26Ir5bDiATmYSSoj7jC5IoS3GUA%2FnLraO%2BPz6%2FmdAt8o9IWilsSwy3NgOEP9qllr7u2NCDXPLY6Xhtyd1rjKZCDcp%2B%2BWxMNbw%2BdcGOqUBCL%2BRSqH6LLNte6mmNjzhJwsreU0LKxMsgWkT9CWY2GjMHCoG6t9tH9l0mD8smZKkCO0ltme1kCiHhPAucUGvKgG514nB2cqytwDvALRoYlrGUw37ujHmTHBWryL4m1iRNFLHgjO66nZtESh2Rgm6d2XtZ%2BH%2FHoWESeSC0y7sjgx6VCowE8%2FeiYetPrs2HtEpqIcMbExQXuJ3r85C0sgeh0ovsLJk&X-Amz-Signature=a9e226154acc0cbd2aacaca94c1a3927a13075126c81a944526188391b50789e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
