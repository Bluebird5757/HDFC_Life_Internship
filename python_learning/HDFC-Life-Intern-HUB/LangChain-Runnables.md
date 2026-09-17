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
fetched_at: '2026-09-17T02:23:14.459Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVKUGADB%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022307Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQD94C7rXY1QacIwMKH1JVK0J%2Fu3AspKlErKrTnstxbMVQIhALvxuwgQEJt1sl9ZUMvwSOUWdVUfCp9F0%2BXU8pNt0l4hKv8DCCEQABoMNjM3NDIzMTgzODA1Igy4tlE87uGYzhVatfwq3AMD2YkhEzuF1mekgmPxnjFtTSWyAyxF0%2F0hyKyvU%2BTxm81zKUzqiCmg7XhGzqba8DC6mvX6daMBsjKCBnQyZIoZJTqNBmfwCwFH9UAObzhSEILbFyzEXXkdHEhqDsyo1aOPofN8rNHwQ9UfktxZbxJMbJ96Ih5HhZKkHCyuHvfjhAUGKqczWmjdMyeItEA5fhX415RP5HnyxHk4Qzv9Wsqa%2FQAFM1mbuAeOjSJO1uU16iLZO3G%2FMIUUHLRK4AvXGTLeSwUgdn49LFhLQaVY7H%2FYutoUtLLWRuM85R4xrqUJe2zN2kW1jqd3nD%2BuDz8KtJz2pZcafAB%2Fg3mJ1NXwn5JoA94XfHvXWofrMnNk%2Br1GaLz7BunpjAbi6A4P4yUOVnHRThjKS7tzZbFhLK29VGMULZsH1KAdEwzDUVmwNr7UZZi0XqaFhMA%2Bh47Jaxor1mY%2Fvhccjj4e6SIV1XreWuNtzzObv%2FClU4M0P6VSfx2BJ17USrNvHI0%2B7LhOLs4%2FzO7jQ3bSuagYsjqp7lV6JOPuWrD6p0VC8%2FQBlUX5d4ToKE6OUorBLdLIk1gLgrRFhEkgTaXefpLOLLcwvDPQcHQM9HA0htDqs0jAwtEhUVwspWCHgcyV027QlszyOzCz4qzVBjqkAVAH%2FNEGNtzPKl3hMfjJKeRvLXEJpkTOR%2BO6BD4n5wPohhSMHUTS0J38VeVxeKQD554JZsAXWmsrudWB8RRjEr0ONSXrdox%2FVGFvfSNuEodY%2BU1Odt7JDLaGXqFPCWc1JcSwMS6aNq4%2BLsjSwYYDUPfZI7U%2BIZqy80As5sOuKM2633mro85KOVXxnGO8kwn4JpDQyyzlLKuqAVprK7ofOP6mFEhV&X-Amz-Signature=5967768d4644bb353aa5b564e7235b0884f9fe48f43806dd6a396fa16585d7da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVKUGADB%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022307Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQD94C7rXY1QacIwMKH1JVK0J%2Fu3AspKlErKrTnstxbMVQIhALvxuwgQEJt1sl9ZUMvwSOUWdVUfCp9F0%2BXU8pNt0l4hKv8DCCEQABoMNjM3NDIzMTgzODA1Igy4tlE87uGYzhVatfwq3AMD2YkhEzuF1mekgmPxnjFtTSWyAyxF0%2F0hyKyvU%2BTxm81zKUzqiCmg7XhGzqba8DC6mvX6daMBsjKCBnQyZIoZJTqNBmfwCwFH9UAObzhSEILbFyzEXXkdHEhqDsyo1aOPofN8rNHwQ9UfktxZbxJMbJ96Ih5HhZKkHCyuHvfjhAUGKqczWmjdMyeItEA5fhX415RP5HnyxHk4Qzv9Wsqa%2FQAFM1mbuAeOjSJO1uU16iLZO3G%2FMIUUHLRK4AvXGTLeSwUgdn49LFhLQaVY7H%2FYutoUtLLWRuM85R4xrqUJe2zN2kW1jqd3nD%2BuDz8KtJz2pZcafAB%2Fg3mJ1NXwn5JoA94XfHvXWofrMnNk%2Br1GaLz7BunpjAbi6A4P4yUOVnHRThjKS7tzZbFhLK29VGMULZsH1KAdEwzDUVmwNr7UZZi0XqaFhMA%2Bh47Jaxor1mY%2Fvhccjj4e6SIV1XreWuNtzzObv%2FClU4M0P6VSfx2BJ17USrNvHI0%2B7LhOLs4%2FzO7jQ3bSuagYsjqp7lV6JOPuWrD6p0VC8%2FQBlUX5d4ToKE6OUorBLdLIk1gLgrRFhEkgTaXefpLOLLcwvDPQcHQM9HA0htDqs0jAwtEhUVwspWCHgcyV027QlszyOzCz4qzVBjqkAVAH%2FNEGNtzPKl3hMfjJKeRvLXEJpkTOR%2BO6BD4n5wPohhSMHUTS0J38VeVxeKQD554JZsAXWmsrudWB8RRjEr0ONSXrdox%2FVGFvfSNuEodY%2BU1Odt7JDLaGXqFPCWc1JcSwMS6aNq4%2BLsjSwYYDUPfZI7U%2BIZqy80As5sOuKM2633mro85KOVXxnGO8kwn4JpDQyyzlLKuqAVprK7ofOP6mFEhV&X-Amz-Signature=ad37ac14633b6201277f9d4e8635989631a35fb6d3865305cd1cefbb90bc7082&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVKUGADB%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022307Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQD94C7rXY1QacIwMKH1JVK0J%2Fu3AspKlErKrTnstxbMVQIhALvxuwgQEJt1sl9ZUMvwSOUWdVUfCp9F0%2BXU8pNt0l4hKv8DCCEQABoMNjM3NDIzMTgzODA1Igy4tlE87uGYzhVatfwq3AMD2YkhEzuF1mekgmPxnjFtTSWyAyxF0%2F0hyKyvU%2BTxm81zKUzqiCmg7XhGzqba8DC6mvX6daMBsjKCBnQyZIoZJTqNBmfwCwFH9UAObzhSEILbFyzEXXkdHEhqDsyo1aOPofN8rNHwQ9UfktxZbxJMbJ96Ih5HhZKkHCyuHvfjhAUGKqczWmjdMyeItEA5fhX415RP5HnyxHk4Qzv9Wsqa%2FQAFM1mbuAeOjSJO1uU16iLZO3G%2FMIUUHLRK4AvXGTLeSwUgdn49LFhLQaVY7H%2FYutoUtLLWRuM85R4xrqUJe2zN2kW1jqd3nD%2BuDz8KtJz2pZcafAB%2Fg3mJ1NXwn5JoA94XfHvXWofrMnNk%2Br1GaLz7BunpjAbi6A4P4yUOVnHRThjKS7tzZbFhLK29VGMULZsH1KAdEwzDUVmwNr7UZZi0XqaFhMA%2Bh47Jaxor1mY%2Fvhccjj4e6SIV1XreWuNtzzObv%2FClU4M0P6VSfx2BJ17USrNvHI0%2B7LhOLs4%2FzO7jQ3bSuagYsjqp7lV6JOPuWrD6p0VC8%2FQBlUX5d4ToKE6OUorBLdLIk1gLgrRFhEkgTaXefpLOLLcwvDPQcHQM9HA0htDqs0jAwtEhUVwspWCHgcyV027QlszyOzCz4qzVBjqkAVAH%2FNEGNtzPKl3hMfjJKeRvLXEJpkTOR%2BO6BD4n5wPohhSMHUTS0J38VeVxeKQD554JZsAXWmsrudWB8RRjEr0ONSXrdox%2FVGFvfSNuEodY%2BU1Odt7JDLaGXqFPCWc1JcSwMS6aNq4%2BLsjSwYYDUPfZI7U%2BIZqy80As5sOuKM2633mro85KOVXxnGO8kwn4JpDQyyzlLKuqAVprK7ofOP6mFEhV&X-Amz-Signature=28c7e643151ccd7c1ade507e958b7351a19da820f3a839825bae9787f4f9a1d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVKUGADB%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022307Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQD94C7rXY1QacIwMKH1JVK0J%2Fu3AspKlErKrTnstxbMVQIhALvxuwgQEJt1sl9ZUMvwSOUWdVUfCp9F0%2BXU8pNt0l4hKv8DCCEQABoMNjM3NDIzMTgzODA1Igy4tlE87uGYzhVatfwq3AMD2YkhEzuF1mekgmPxnjFtTSWyAyxF0%2F0hyKyvU%2BTxm81zKUzqiCmg7XhGzqba8DC6mvX6daMBsjKCBnQyZIoZJTqNBmfwCwFH9UAObzhSEILbFyzEXXkdHEhqDsyo1aOPofN8rNHwQ9UfktxZbxJMbJ96Ih5HhZKkHCyuHvfjhAUGKqczWmjdMyeItEA5fhX415RP5HnyxHk4Qzv9Wsqa%2FQAFM1mbuAeOjSJO1uU16iLZO3G%2FMIUUHLRK4AvXGTLeSwUgdn49LFhLQaVY7H%2FYutoUtLLWRuM85R4xrqUJe2zN2kW1jqd3nD%2BuDz8KtJz2pZcafAB%2Fg3mJ1NXwn5JoA94XfHvXWofrMnNk%2Br1GaLz7BunpjAbi6A4P4yUOVnHRThjKS7tzZbFhLK29VGMULZsH1KAdEwzDUVmwNr7UZZi0XqaFhMA%2Bh47Jaxor1mY%2Fvhccjj4e6SIV1XreWuNtzzObv%2FClU4M0P6VSfx2BJ17USrNvHI0%2B7LhOLs4%2FzO7jQ3bSuagYsjqp7lV6JOPuWrD6p0VC8%2FQBlUX5d4ToKE6OUorBLdLIk1gLgrRFhEkgTaXefpLOLLcwvDPQcHQM9HA0htDqs0jAwtEhUVwspWCHgcyV027QlszyOzCz4qzVBjqkAVAH%2FNEGNtzPKl3hMfjJKeRvLXEJpkTOR%2BO6BD4n5wPohhSMHUTS0J38VeVxeKQD554JZsAXWmsrudWB8RRjEr0ONSXrdox%2FVGFvfSNuEodY%2BU1Odt7JDLaGXqFPCWc1JcSwMS6aNq4%2BLsjSwYYDUPfZI7U%2BIZqy80As5sOuKM2633mro85KOVXxnGO8kwn4JpDQyyzlLKuqAVprK7ofOP6mFEhV&X-Amz-Signature=b9d980f8c2e35a4d1542f5bd9ef056b6eb3c5311eb1492cc04694ce65d1daf5a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVKUGADB%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022307Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQD94C7rXY1QacIwMKH1JVK0J%2Fu3AspKlErKrTnstxbMVQIhALvxuwgQEJt1sl9ZUMvwSOUWdVUfCp9F0%2BXU8pNt0l4hKv8DCCEQABoMNjM3NDIzMTgzODA1Igy4tlE87uGYzhVatfwq3AMD2YkhEzuF1mekgmPxnjFtTSWyAyxF0%2F0hyKyvU%2BTxm81zKUzqiCmg7XhGzqba8DC6mvX6daMBsjKCBnQyZIoZJTqNBmfwCwFH9UAObzhSEILbFyzEXXkdHEhqDsyo1aOPofN8rNHwQ9UfktxZbxJMbJ96Ih5HhZKkHCyuHvfjhAUGKqczWmjdMyeItEA5fhX415RP5HnyxHk4Qzv9Wsqa%2FQAFM1mbuAeOjSJO1uU16iLZO3G%2FMIUUHLRK4AvXGTLeSwUgdn49LFhLQaVY7H%2FYutoUtLLWRuM85R4xrqUJe2zN2kW1jqd3nD%2BuDz8KtJz2pZcafAB%2Fg3mJ1NXwn5JoA94XfHvXWofrMnNk%2Br1GaLz7BunpjAbi6A4P4yUOVnHRThjKS7tzZbFhLK29VGMULZsH1KAdEwzDUVmwNr7UZZi0XqaFhMA%2Bh47Jaxor1mY%2Fvhccjj4e6SIV1XreWuNtzzObv%2FClU4M0P6VSfx2BJ17USrNvHI0%2B7LhOLs4%2FzO7jQ3bSuagYsjqp7lV6JOPuWrD6p0VC8%2FQBlUX5d4ToKE6OUorBLdLIk1gLgrRFhEkgTaXefpLOLLcwvDPQcHQM9HA0htDqs0jAwtEhUVwspWCHgcyV027QlszyOzCz4qzVBjqkAVAH%2FNEGNtzPKl3hMfjJKeRvLXEJpkTOR%2BO6BD4n5wPohhSMHUTS0J38VeVxeKQD554JZsAXWmsrudWB8RRjEr0ONSXrdox%2FVGFvfSNuEodY%2BU1Odt7JDLaGXqFPCWc1JcSwMS6aNq4%2BLsjSwYYDUPfZI7U%2BIZqy80As5sOuKM2633mro85KOVXxnGO8kwn4JpDQyyzlLKuqAVprK7ofOP6mFEhV&X-Amz-Signature=a2120aec9ca04f62c57714914c2571e55e50431f7d3b011e6d8d0ae9e63b81e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVKUGADB%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022307Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQD94C7rXY1QacIwMKH1JVK0J%2Fu3AspKlErKrTnstxbMVQIhALvxuwgQEJt1sl9ZUMvwSOUWdVUfCp9F0%2BXU8pNt0l4hKv8DCCEQABoMNjM3NDIzMTgzODA1Igy4tlE87uGYzhVatfwq3AMD2YkhEzuF1mekgmPxnjFtTSWyAyxF0%2F0hyKyvU%2BTxm81zKUzqiCmg7XhGzqba8DC6mvX6daMBsjKCBnQyZIoZJTqNBmfwCwFH9UAObzhSEILbFyzEXXkdHEhqDsyo1aOPofN8rNHwQ9UfktxZbxJMbJ96Ih5HhZKkHCyuHvfjhAUGKqczWmjdMyeItEA5fhX415RP5HnyxHk4Qzv9Wsqa%2FQAFM1mbuAeOjSJO1uU16iLZO3G%2FMIUUHLRK4AvXGTLeSwUgdn49LFhLQaVY7H%2FYutoUtLLWRuM85R4xrqUJe2zN2kW1jqd3nD%2BuDz8KtJz2pZcafAB%2Fg3mJ1NXwn5JoA94XfHvXWofrMnNk%2Br1GaLz7BunpjAbi6A4P4yUOVnHRThjKS7tzZbFhLK29VGMULZsH1KAdEwzDUVmwNr7UZZi0XqaFhMA%2Bh47Jaxor1mY%2Fvhccjj4e6SIV1XreWuNtzzObv%2FClU4M0P6VSfx2BJ17USrNvHI0%2B7LhOLs4%2FzO7jQ3bSuagYsjqp7lV6JOPuWrD6p0VC8%2FQBlUX5d4ToKE6OUorBLdLIk1gLgrRFhEkgTaXefpLOLLcwvDPQcHQM9HA0htDqs0jAwtEhUVwspWCHgcyV027QlszyOzCz4qzVBjqkAVAH%2FNEGNtzPKl3hMfjJKeRvLXEJpkTOR%2BO6BD4n5wPohhSMHUTS0J38VeVxeKQD554JZsAXWmsrudWB8RRjEr0ONSXrdox%2FVGFvfSNuEodY%2BU1Odt7JDLaGXqFPCWc1JcSwMS6aNq4%2BLsjSwYYDUPfZI7U%2BIZqy80As5sOuKM2633mro85KOVXxnGO8kwn4JpDQyyzlLKuqAVprK7ofOP6mFEhV&X-Amz-Signature=e61db73e5a7fb89867068eb3c02ff75761c588cf3d07326c9337ea15d1745d27&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVKUGADB%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022307Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQD94C7rXY1QacIwMKH1JVK0J%2Fu3AspKlErKrTnstxbMVQIhALvxuwgQEJt1sl9ZUMvwSOUWdVUfCp9F0%2BXU8pNt0l4hKv8DCCEQABoMNjM3NDIzMTgzODA1Igy4tlE87uGYzhVatfwq3AMD2YkhEzuF1mekgmPxnjFtTSWyAyxF0%2F0hyKyvU%2BTxm81zKUzqiCmg7XhGzqba8DC6mvX6daMBsjKCBnQyZIoZJTqNBmfwCwFH9UAObzhSEILbFyzEXXkdHEhqDsyo1aOPofN8rNHwQ9UfktxZbxJMbJ96Ih5HhZKkHCyuHvfjhAUGKqczWmjdMyeItEA5fhX415RP5HnyxHk4Qzv9Wsqa%2FQAFM1mbuAeOjSJO1uU16iLZO3G%2FMIUUHLRK4AvXGTLeSwUgdn49LFhLQaVY7H%2FYutoUtLLWRuM85R4xrqUJe2zN2kW1jqd3nD%2BuDz8KtJz2pZcafAB%2Fg3mJ1NXwn5JoA94XfHvXWofrMnNk%2Br1GaLz7BunpjAbi6A4P4yUOVnHRThjKS7tzZbFhLK29VGMULZsH1KAdEwzDUVmwNr7UZZi0XqaFhMA%2Bh47Jaxor1mY%2Fvhccjj4e6SIV1XreWuNtzzObv%2FClU4M0P6VSfx2BJ17USrNvHI0%2B7LhOLs4%2FzO7jQ3bSuagYsjqp7lV6JOPuWrD6p0VC8%2FQBlUX5d4ToKE6OUorBLdLIk1gLgrRFhEkgTaXefpLOLLcwvDPQcHQM9HA0htDqs0jAwtEhUVwspWCHgcyV027QlszyOzCz4qzVBjqkAVAH%2FNEGNtzPKl3hMfjJKeRvLXEJpkTOR%2BO6BD4n5wPohhSMHUTS0J38VeVxeKQD554JZsAXWmsrudWB8RRjEr0ONSXrdox%2FVGFvfSNuEodY%2BU1Odt7JDLaGXqFPCWc1JcSwMS6aNq4%2BLsjSwYYDUPfZI7U%2BIZqy80As5sOuKM2633mro85KOVXxnGO8kwn4JpDQyyzlLKuqAVprK7ofOP6mFEhV&X-Amz-Signature=32e118e8fab7f246d2bf77ab75bf6d961d7d9b5df5c2f9dbcbc738c0fe86fc90&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVKUGADB%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022307Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQD94C7rXY1QacIwMKH1JVK0J%2Fu3AspKlErKrTnstxbMVQIhALvxuwgQEJt1sl9ZUMvwSOUWdVUfCp9F0%2BXU8pNt0l4hKv8DCCEQABoMNjM3NDIzMTgzODA1Igy4tlE87uGYzhVatfwq3AMD2YkhEzuF1mekgmPxnjFtTSWyAyxF0%2F0hyKyvU%2BTxm81zKUzqiCmg7XhGzqba8DC6mvX6daMBsjKCBnQyZIoZJTqNBmfwCwFH9UAObzhSEILbFyzEXXkdHEhqDsyo1aOPofN8rNHwQ9UfktxZbxJMbJ96Ih5HhZKkHCyuHvfjhAUGKqczWmjdMyeItEA5fhX415RP5HnyxHk4Qzv9Wsqa%2FQAFM1mbuAeOjSJO1uU16iLZO3G%2FMIUUHLRK4AvXGTLeSwUgdn49LFhLQaVY7H%2FYutoUtLLWRuM85R4xrqUJe2zN2kW1jqd3nD%2BuDz8KtJz2pZcafAB%2Fg3mJ1NXwn5JoA94XfHvXWofrMnNk%2Br1GaLz7BunpjAbi6A4P4yUOVnHRThjKS7tzZbFhLK29VGMULZsH1KAdEwzDUVmwNr7UZZi0XqaFhMA%2Bh47Jaxor1mY%2Fvhccjj4e6SIV1XreWuNtzzObv%2FClU4M0P6VSfx2BJ17USrNvHI0%2B7LhOLs4%2FzO7jQ3bSuagYsjqp7lV6JOPuWrD6p0VC8%2FQBlUX5d4ToKE6OUorBLdLIk1gLgrRFhEkgTaXefpLOLLcwvDPQcHQM9HA0htDqs0jAwtEhUVwspWCHgcyV027QlszyOzCz4qzVBjqkAVAH%2FNEGNtzPKl3hMfjJKeRvLXEJpkTOR%2BO6BD4n5wPohhSMHUTS0J38VeVxeKQD554JZsAXWmsrudWB8RRjEr0ONSXrdox%2FVGFvfSNuEodY%2BU1Odt7JDLaGXqFPCWc1JcSwMS6aNq4%2BLsjSwYYDUPfZI7U%2BIZqy80As5sOuKM2633mro85KOVXxnGO8kwn4JpDQyyzlLKuqAVprK7ofOP6mFEhV&X-Amz-Signature=f89d196c55c7b35a939674e59189dde87238168bd5a4805044db0850cf139f20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVKUGADB%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022307Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQD94C7rXY1QacIwMKH1JVK0J%2Fu3AspKlErKrTnstxbMVQIhALvxuwgQEJt1sl9ZUMvwSOUWdVUfCp9F0%2BXU8pNt0l4hKv8DCCEQABoMNjM3NDIzMTgzODA1Igy4tlE87uGYzhVatfwq3AMD2YkhEzuF1mekgmPxnjFtTSWyAyxF0%2F0hyKyvU%2BTxm81zKUzqiCmg7XhGzqba8DC6mvX6daMBsjKCBnQyZIoZJTqNBmfwCwFH9UAObzhSEILbFyzEXXkdHEhqDsyo1aOPofN8rNHwQ9UfktxZbxJMbJ96Ih5HhZKkHCyuHvfjhAUGKqczWmjdMyeItEA5fhX415RP5HnyxHk4Qzv9Wsqa%2FQAFM1mbuAeOjSJO1uU16iLZO3G%2FMIUUHLRK4AvXGTLeSwUgdn49LFhLQaVY7H%2FYutoUtLLWRuM85R4xrqUJe2zN2kW1jqd3nD%2BuDz8KtJz2pZcafAB%2Fg3mJ1NXwn5JoA94XfHvXWofrMnNk%2Br1GaLz7BunpjAbi6A4P4yUOVnHRThjKS7tzZbFhLK29VGMULZsH1KAdEwzDUVmwNr7UZZi0XqaFhMA%2Bh47Jaxor1mY%2Fvhccjj4e6SIV1XreWuNtzzObv%2FClU4M0P6VSfx2BJ17USrNvHI0%2B7LhOLs4%2FzO7jQ3bSuagYsjqp7lV6JOPuWrD6p0VC8%2FQBlUX5d4ToKE6OUorBLdLIk1gLgrRFhEkgTaXefpLOLLcwvDPQcHQM9HA0htDqs0jAwtEhUVwspWCHgcyV027QlszyOzCz4qzVBjqkAVAH%2FNEGNtzPKl3hMfjJKeRvLXEJpkTOR%2BO6BD4n5wPohhSMHUTS0J38VeVxeKQD554JZsAXWmsrudWB8RRjEr0ONSXrdox%2FVGFvfSNuEodY%2BU1Odt7JDLaGXqFPCWc1JcSwMS6aNq4%2BLsjSwYYDUPfZI7U%2BIZqy80As5sOuKM2633mro85KOVXxnGO8kwn4JpDQyyzlLKuqAVprK7ofOP6mFEhV&X-Amz-Signature=e0a4e097efd02e5d7308e75ea768bde61bfa859dab3b6202e2fe997aaab6cdff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
