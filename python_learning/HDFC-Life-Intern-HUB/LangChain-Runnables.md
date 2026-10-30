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
fetched_at: '2026-10-30T03:29:26.664Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S4J2AXL7%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032921Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDCb%2BrrohrWjVLueUs0NPftxnXx8OIZA23YgKxBUQpOmQIgMrUZ5sAoo07WAsrQGoqTeLvMDS%2Bu%2Fs0N6Aqb3IUWGAoq%2FwMILBAAGgw2Mzc0MjMxODM4MDUiDGRNYZ7FHXYwkhb%2BySrcAxfb0T5sUB9k9tdDMEGBYD2kRQDj1XAVlyQH2A7igaoeXOLzCPE%2B674FJrCb5DGpmsg%2FjqXVhBAf5xGFz3Xcnmn1NqEKpO8Hka3QnaU9Vgw6hXzg5ZWteGqPhZ2BeU%2FmfQmLDXuGozA36iponMNpkoUjJNnM8GEXcLhxsHPeOoddce2nwuriw6E3xkwaLDUS1t%2FErJyYBnGmZYTzM4bRfn4lwogHPQYYTPAk1EsUSBrGoVMcpVvTdRxYGc0tRoxAlbXr8N1jqJ9jKSmC4n9hghpp62rX4RVauYbWW%2FOnfg6VQ%2FrFj8QMf7gmqXgsbGxYfVKeCTAajNNulLM9coMSh%2B4HMIpyaXLzm4p%2BRK4f7DDVqaCPmZuPa5v7C4Vu5CUMAteht2WLgvPT8IQpJxuHhIQR8OaMTvc6GKdWS4S3a%2Baa9whUD5UQc4kcONWm5SomJkJA9%2Beeb0BHws1j9MGnAp6aakCT24Uzy0zsaBK4jtRbKU7lWv9CCTcE258LWd4lYCUhEPfn3mzBmgrpLSL3%2B3zfw%2FZ5Z3wN4Tm3aIDbqyQ%2BEMC2XulPsRQqLehFm0Mng6vxk71TKYdtKbdsy3HcORaDsB9FfDiB2LDoMZ0KLOck1RjsdfvevzBRG%2FJBMNOckNcGOqUBq7SZvcU8LPSUxzXrr404lD7iBK3RmIolpGoEt%2FDJwr7nSd1Ol4o3wRgHJ317JXYUcyafCH2Wbm4Iw5MDPiXp9fLTNq6T99AxqPhbaWcJhHrqo1WBzhZy8O%2B30IRNPNeNPDBD3H8t5tv61CtNpoxxXj63JF4SHaEm1byi5Oz%2FhAiij0rXQFGTV0IPv7HkLOhhY2C8Psj2qMuPcxBsNMeibm96LSSn&X-Amz-Signature=41ffd590f81c77895f7e63eb18edcaa85c7d1ed0515a4194c3fdce40e11ef3e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S4J2AXL7%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032921Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDCb%2BrrohrWjVLueUs0NPftxnXx8OIZA23YgKxBUQpOmQIgMrUZ5sAoo07WAsrQGoqTeLvMDS%2Bu%2Fs0N6Aqb3IUWGAoq%2FwMILBAAGgw2Mzc0MjMxODM4MDUiDGRNYZ7FHXYwkhb%2BySrcAxfb0T5sUB9k9tdDMEGBYD2kRQDj1XAVlyQH2A7igaoeXOLzCPE%2B674FJrCb5DGpmsg%2FjqXVhBAf5xGFz3Xcnmn1NqEKpO8Hka3QnaU9Vgw6hXzg5ZWteGqPhZ2BeU%2FmfQmLDXuGozA36iponMNpkoUjJNnM8GEXcLhxsHPeOoddce2nwuriw6E3xkwaLDUS1t%2FErJyYBnGmZYTzM4bRfn4lwogHPQYYTPAk1EsUSBrGoVMcpVvTdRxYGc0tRoxAlbXr8N1jqJ9jKSmC4n9hghpp62rX4RVauYbWW%2FOnfg6VQ%2FrFj8QMf7gmqXgsbGxYfVKeCTAajNNulLM9coMSh%2B4HMIpyaXLzm4p%2BRK4f7DDVqaCPmZuPa5v7C4Vu5CUMAteht2WLgvPT8IQpJxuHhIQR8OaMTvc6GKdWS4S3a%2Baa9whUD5UQc4kcONWm5SomJkJA9%2Beeb0BHws1j9MGnAp6aakCT24Uzy0zsaBK4jtRbKU7lWv9CCTcE258LWd4lYCUhEPfn3mzBmgrpLSL3%2B3zfw%2FZ5Z3wN4Tm3aIDbqyQ%2BEMC2XulPsRQqLehFm0Mng6vxk71TKYdtKbdsy3HcORaDsB9FfDiB2LDoMZ0KLOck1RjsdfvevzBRG%2FJBMNOckNcGOqUBq7SZvcU8LPSUxzXrr404lD7iBK3RmIolpGoEt%2FDJwr7nSd1Ol4o3wRgHJ317JXYUcyafCH2Wbm4Iw5MDPiXp9fLTNq6T99AxqPhbaWcJhHrqo1WBzhZy8O%2B30IRNPNeNPDBD3H8t5tv61CtNpoxxXj63JF4SHaEm1byi5Oz%2FhAiij0rXQFGTV0IPv7HkLOhhY2C8Psj2qMuPcxBsNMeibm96LSSn&X-Amz-Signature=43967ddc3f5978d7449fe87119c592e49040a15335ae1d6c1277b86ee21f0510&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S4J2AXL7%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032921Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDCb%2BrrohrWjVLueUs0NPftxnXx8OIZA23YgKxBUQpOmQIgMrUZ5sAoo07WAsrQGoqTeLvMDS%2Bu%2Fs0N6Aqb3IUWGAoq%2FwMILBAAGgw2Mzc0MjMxODM4MDUiDGRNYZ7FHXYwkhb%2BySrcAxfb0T5sUB9k9tdDMEGBYD2kRQDj1XAVlyQH2A7igaoeXOLzCPE%2B674FJrCb5DGpmsg%2FjqXVhBAf5xGFz3Xcnmn1NqEKpO8Hka3QnaU9Vgw6hXzg5ZWteGqPhZ2BeU%2FmfQmLDXuGozA36iponMNpkoUjJNnM8GEXcLhxsHPeOoddce2nwuriw6E3xkwaLDUS1t%2FErJyYBnGmZYTzM4bRfn4lwogHPQYYTPAk1EsUSBrGoVMcpVvTdRxYGc0tRoxAlbXr8N1jqJ9jKSmC4n9hghpp62rX4RVauYbWW%2FOnfg6VQ%2FrFj8QMf7gmqXgsbGxYfVKeCTAajNNulLM9coMSh%2B4HMIpyaXLzm4p%2BRK4f7DDVqaCPmZuPa5v7C4Vu5CUMAteht2WLgvPT8IQpJxuHhIQR8OaMTvc6GKdWS4S3a%2Baa9whUD5UQc4kcONWm5SomJkJA9%2Beeb0BHws1j9MGnAp6aakCT24Uzy0zsaBK4jtRbKU7lWv9CCTcE258LWd4lYCUhEPfn3mzBmgrpLSL3%2B3zfw%2FZ5Z3wN4Tm3aIDbqyQ%2BEMC2XulPsRQqLehFm0Mng6vxk71TKYdtKbdsy3HcORaDsB9FfDiB2LDoMZ0KLOck1RjsdfvevzBRG%2FJBMNOckNcGOqUBq7SZvcU8LPSUxzXrr404lD7iBK3RmIolpGoEt%2FDJwr7nSd1Ol4o3wRgHJ317JXYUcyafCH2Wbm4Iw5MDPiXp9fLTNq6T99AxqPhbaWcJhHrqo1WBzhZy8O%2B30IRNPNeNPDBD3H8t5tv61CtNpoxxXj63JF4SHaEm1byi5Oz%2FhAiij0rXQFGTV0IPv7HkLOhhY2C8Psj2qMuPcxBsNMeibm96LSSn&X-Amz-Signature=ba1eef1325744dc93b6d69a0eb7516bb91743e323812a950b9d70d4837b15408&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S4J2AXL7%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032921Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDCb%2BrrohrWjVLueUs0NPftxnXx8OIZA23YgKxBUQpOmQIgMrUZ5sAoo07WAsrQGoqTeLvMDS%2Bu%2Fs0N6Aqb3IUWGAoq%2FwMILBAAGgw2Mzc0MjMxODM4MDUiDGRNYZ7FHXYwkhb%2BySrcAxfb0T5sUB9k9tdDMEGBYD2kRQDj1XAVlyQH2A7igaoeXOLzCPE%2B674FJrCb5DGpmsg%2FjqXVhBAf5xGFz3Xcnmn1NqEKpO8Hka3QnaU9Vgw6hXzg5ZWteGqPhZ2BeU%2FmfQmLDXuGozA36iponMNpkoUjJNnM8GEXcLhxsHPeOoddce2nwuriw6E3xkwaLDUS1t%2FErJyYBnGmZYTzM4bRfn4lwogHPQYYTPAk1EsUSBrGoVMcpVvTdRxYGc0tRoxAlbXr8N1jqJ9jKSmC4n9hghpp62rX4RVauYbWW%2FOnfg6VQ%2FrFj8QMf7gmqXgsbGxYfVKeCTAajNNulLM9coMSh%2B4HMIpyaXLzm4p%2BRK4f7DDVqaCPmZuPa5v7C4Vu5CUMAteht2WLgvPT8IQpJxuHhIQR8OaMTvc6GKdWS4S3a%2Baa9whUD5UQc4kcONWm5SomJkJA9%2Beeb0BHws1j9MGnAp6aakCT24Uzy0zsaBK4jtRbKU7lWv9CCTcE258LWd4lYCUhEPfn3mzBmgrpLSL3%2B3zfw%2FZ5Z3wN4Tm3aIDbqyQ%2BEMC2XulPsRQqLehFm0Mng6vxk71TKYdtKbdsy3HcORaDsB9FfDiB2LDoMZ0KLOck1RjsdfvevzBRG%2FJBMNOckNcGOqUBq7SZvcU8LPSUxzXrr404lD7iBK3RmIolpGoEt%2FDJwr7nSd1Ol4o3wRgHJ317JXYUcyafCH2Wbm4Iw5MDPiXp9fLTNq6T99AxqPhbaWcJhHrqo1WBzhZy8O%2B30IRNPNeNPDBD3H8t5tv61CtNpoxxXj63JF4SHaEm1byi5Oz%2FhAiij0rXQFGTV0IPv7HkLOhhY2C8Psj2qMuPcxBsNMeibm96LSSn&X-Amz-Signature=beddfe9f6d5d642269345ee0e9d1c54899b86e0000f944c4a801ec3da8aa6dbb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S4J2AXL7%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032921Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDCb%2BrrohrWjVLueUs0NPftxnXx8OIZA23YgKxBUQpOmQIgMrUZ5sAoo07WAsrQGoqTeLvMDS%2Bu%2Fs0N6Aqb3IUWGAoq%2FwMILBAAGgw2Mzc0MjMxODM4MDUiDGRNYZ7FHXYwkhb%2BySrcAxfb0T5sUB9k9tdDMEGBYD2kRQDj1XAVlyQH2A7igaoeXOLzCPE%2B674FJrCb5DGpmsg%2FjqXVhBAf5xGFz3Xcnmn1NqEKpO8Hka3QnaU9Vgw6hXzg5ZWteGqPhZ2BeU%2FmfQmLDXuGozA36iponMNpkoUjJNnM8GEXcLhxsHPeOoddce2nwuriw6E3xkwaLDUS1t%2FErJyYBnGmZYTzM4bRfn4lwogHPQYYTPAk1EsUSBrGoVMcpVvTdRxYGc0tRoxAlbXr8N1jqJ9jKSmC4n9hghpp62rX4RVauYbWW%2FOnfg6VQ%2FrFj8QMf7gmqXgsbGxYfVKeCTAajNNulLM9coMSh%2B4HMIpyaXLzm4p%2BRK4f7DDVqaCPmZuPa5v7C4Vu5CUMAteht2WLgvPT8IQpJxuHhIQR8OaMTvc6GKdWS4S3a%2Baa9whUD5UQc4kcONWm5SomJkJA9%2Beeb0BHws1j9MGnAp6aakCT24Uzy0zsaBK4jtRbKU7lWv9CCTcE258LWd4lYCUhEPfn3mzBmgrpLSL3%2B3zfw%2FZ5Z3wN4Tm3aIDbqyQ%2BEMC2XulPsRQqLehFm0Mng6vxk71TKYdtKbdsy3HcORaDsB9FfDiB2LDoMZ0KLOck1RjsdfvevzBRG%2FJBMNOckNcGOqUBq7SZvcU8LPSUxzXrr404lD7iBK3RmIolpGoEt%2FDJwr7nSd1Ol4o3wRgHJ317JXYUcyafCH2Wbm4Iw5MDPiXp9fLTNq6T99AxqPhbaWcJhHrqo1WBzhZy8O%2B30IRNPNeNPDBD3H8t5tv61CtNpoxxXj63JF4SHaEm1byi5Oz%2FhAiij0rXQFGTV0IPv7HkLOhhY2C8Psj2qMuPcxBsNMeibm96LSSn&X-Amz-Signature=8a3e7332be2d6a8bac0f01781a6ed1b9b0d818a4959f331de76897c9690ce70d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S4J2AXL7%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032921Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDCb%2BrrohrWjVLueUs0NPftxnXx8OIZA23YgKxBUQpOmQIgMrUZ5sAoo07WAsrQGoqTeLvMDS%2Bu%2Fs0N6Aqb3IUWGAoq%2FwMILBAAGgw2Mzc0MjMxODM4MDUiDGRNYZ7FHXYwkhb%2BySrcAxfb0T5sUB9k9tdDMEGBYD2kRQDj1XAVlyQH2A7igaoeXOLzCPE%2B674FJrCb5DGpmsg%2FjqXVhBAf5xGFz3Xcnmn1NqEKpO8Hka3QnaU9Vgw6hXzg5ZWteGqPhZ2BeU%2FmfQmLDXuGozA36iponMNpkoUjJNnM8GEXcLhxsHPeOoddce2nwuriw6E3xkwaLDUS1t%2FErJyYBnGmZYTzM4bRfn4lwogHPQYYTPAk1EsUSBrGoVMcpVvTdRxYGc0tRoxAlbXr8N1jqJ9jKSmC4n9hghpp62rX4RVauYbWW%2FOnfg6VQ%2FrFj8QMf7gmqXgsbGxYfVKeCTAajNNulLM9coMSh%2B4HMIpyaXLzm4p%2BRK4f7DDVqaCPmZuPa5v7C4Vu5CUMAteht2WLgvPT8IQpJxuHhIQR8OaMTvc6GKdWS4S3a%2Baa9whUD5UQc4kcONWm5SomJkJA9%2Beeb0BHws1j9MGnAp6aakCT24Uzy0zsaBK4jtRbKU7lWv9CCTcE258LWd4lYCUhEPfn3mzBmgrpLSL3%2B3zfw%2FZ5Z3wN4Tm3aIDbqyQ%2BEMC2XulPsRQqLehFm0Mng6vxk71TKYdtKbdsy3HcORaDsB9FfDiB2LDoMZ0KLOck1RjsdfvevzBRG%2FJBMNOckNcGOqUBq7SZvcU8LPSUxzXrr404lD7iBK3RmIolpGoEt%2FDJwr7nSd1Ol4o3wRgHJ317JXYUcyafCH2Wbm4Iw5MDPiXp9fLTNq6T99AxqPhbaWcJhHrqo1WBzhZy8O%2B30IRNPNeNPDBD3H8t5tv61CtNpoxxXj63JF4SHaEm1byi5Oz%2FhAiij0rXQFGTV0IPv7HkLOhhY2C8Psj2qMuPcxBsNMeibm96LSSn&X-Amz-Signature=6dfc394d01374c9cfde68c46329e70a85f9f3610f4e31b5b85ae414aacb4cfa0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S4J2AXL7%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032921Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDCb%2BrrohrWjVLueUs0NPftxnXx8OIZA23YgKxBUQpOmQIgMrUZ5sAoo07WAsrQGoqTeLvMDS%2Bu%2Fs0N6Aqb3IUWGAoq%2FwMILBAAGgw2Mzc0MjMxODM4MDUiDGRNYZ7FHXYwkhb%2BySrcAxfb0T5sUB9k9tdDMEGBYD2kRQDj1XAVlyQH2A7igaoeXOLzCPE%2B674FJrCb5DGpmsg%2FjqXVhBAf5xGFz3Xcnmn1NqEKpO8Hka3QnaU9Vgw6hXzg5ZWteGqPhZ2BeU%2FmfQmLDXuGozA36iponMNpkoUjJNnM8GEXcLhxsHPeOoddce2nwuriw6E3xkwaLDUS1t%2FErJyYBnGmZYTzM4bRfn4lwogHPQYYTPAk1EsUSBrGoVMcpVvTdRxYGc0tRoxAlbXr8N1jqJ9jKSmC4n9hghpp62rX4RVauYbWW%2FOnfg6VQ%2FrFj8QMf7gmqXgsbGxYfVKeCTAajNNulLM9coMSh%2B4HMIpyaXLzm4p%2BRK4f7DDVqaCPmZuPa5v7C4Vu5CUMAteht2WLgvPT8IQpJxuHhIQR8OaMTvc6GKdWS4S3a%2Baa9whUD5UQc4kcONWm5SomJkJA9%2Beeb0BHws1j9MGnAp6aakCT24Uzy0zsaBK4jtRbKU7lWv9CCTcE258LWd4lYCUhEPfn3mzBmgrpLSL3%2B3zfw%2FZ5Z3wN4Tm3aIDbqyQ%2BEMC2XulPsRQqLehFm0Mng6vxk71TKYdtKbdsy3HcORaDsB9FfDiB2LDoMZ0KLOck1RjsdfvevzBRG%2FJBMNOckNcGOqUBq7SZvcU8LPSUxzXrr404lD7iBK3RmIolpGoEt%2FDJwr7nSd1Ol4o3wRgHJ317JXYUcyafCH2Wbm4Iw5MDPiXp9fLTNq6T99AxqPhbaWcJhHrqo1WBzhZy8O%2B30IRNPNeNPDBD3H8t5tv61CtNpoxxXj63JF4SHaEm1byi5Oz%2FhAiij0rXQFGTV0IPv7HkLOhhY2C8Psj2qMuPcxBsNMeibm96LSSn&X-Amz-Signature=11894443f9db9793795ea822a21725293285bab5d74235bbed776d92512e0c85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S4J2AXL7%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032921Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDCb%2BrrohrWjVLueUs0NPftxnXx8OIZA23YgKxBUQpOmQIgMrUZ5sAoo07WAsrQGoqTeLvMDS%2Bu%2Fs0N6Aqb3IUWGAoq%2FwMILBAAGgw2Mzc0MjMxODM4MDUiDGRNYZ7FHXYwkhb%2BySrcAxfb0T5sUB9k9tdDMEGBYD2kRQDj1XAVlyQH2A7igaoeXOLzCPE%2B674FJrCb5DGpmsg%2FjqXVhBAf5xGFz3Xcnmn1NqEKpO8Hka3QnaU9Vgw6hXzg5ZWteGqPhZ2BeU%2FmfQmLDXuGozA36iponMNpkoUjJNnM8GEXcLhxsHPeOoddce2nwuriw6E3xkwaLDUS1t%2FErJyYBnGmZYTzM4bRfn4lwogHPQYYTPAk1EsUSBrGoVMcpVvTdRxYGc0tRoxAlbXr8N1jqJ9jKSmC4n9hghpp62rX4RVauYbWW%2FOnfg6VQ%2FrFj8QMf7gmqXgsbGxYfVKeCTAajNNulLM9coMSh%2B4HMIpyaXLzm4p%2BRK4f7DDVqaCPmZuPa5v7C4Vu5CUMAteht2WLgvPT8IQpJxuHhIQR8OaMTvc6GKdWS4S3a%2Baa9whUD5UQc4kcONWm5SomJkJA9%2Beeb0BHws1j9MGnAp6aakCT24Uzy0zsaBK4jtRbKU7lWv9CCTcE258LWd4lYCUhEPfn3mzBmgrpLSL3%2B3zfw%2FZ5Z3wN4Tm3aIDbqyQ%2BEMC2XulPsRQqLehFm0Mng6vxk71TKYdtKbdsy3HcORaDsB9FfDiB2LDoMZ0KLOck1RjsdfvevzBRG%2FJBMNOckNcGOqUBq7SZvcU8LPSUxzXrr404lD7iBK3RmIolpGoEt%2FDJwr7nSd1Ol4o3wRgHJ317JXYUcyafCH2Wbm4Iw5MDPiXp9fLTNq6T99AxqPhbaWcJhHrqo1WBzhZy8O%2B30IRNPNeNPDBD3H8t5tv61CtNpoxxXj63JF4SHaEm1byi5Oz%2FhAiij0rXQFGTV0IPv7HkLOhhY2C8Psj2qMuPcxBsNMeibm96LSSn&X-Amz-Signature=5962a47ab4b1568990459aaf9744beb64fa7ed07061529f432c7da1d32fd20a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S4J2AXL7%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032921Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDCb%2BrrohrWjVLueUs0NPftxnXx8OIZA23YgKxBUQpOmQIgMrUZ5sAoo07WAsrQGoqTeLvMDS%2Bu%2Fs0N6Aqb3IUWGAoq%2FwMILBAAGgw2Mzc0MjMxODM4MDUiDGRNYZ7FHXYwkhb%2BySrcAxfb0T5sUB9k9tdDMEGBYD2kRQDj1XAVlyQH2A7igaoeXOLzCPE%2B674FJrCb5DGpmsg%2FjqXVhBAf5xGFz3Xcnmn1NqEKpO8Hka3QnaU9Vgw6hXzg5ZWteGqPhZ2BeU%2FmfQmLDXuGozA36iponMNpkoUjJNnM8GEXcLhxsHPeOoddce2nwuriw6E3xkwaLDUS1t%2FErJyYBnGmZYTzM4bRfn4lwogHPQYYTPAk1EsUSBrGoVMcpVvTdRxYGc0tRoxAlbXr8N1jqJ9jKSmC4n9hghpp62rX4RVauYbWW%2FOnfg6VQ%2FrFj8QMf7gmqXgsbGxYfVKeCTAajNNulLM9coMSh%2B4HMIpyaXLzm4p%2BRK4f7DDVqaCPmZuPa5v7C4Vu5CUMAteht2WLgvPT8IQpJxuHhIQR8OaMTvc6GKdWS4S3a%2Baa9whUD5UQc4kcONWm5SomJkJA9%2Beeb0BHws1j9MGnAp6aakCT24Uzy0zsaBK4jtRbKU7lWv9CCTcE258LWd4lYCUhEPfn3mzBmgrpLSL3%2B3zfw%2FZ5Z3wN4Tm3aIDbqyQ%2BEMC2XulPsRQqLehFm0Mng6vxk71TKYdtKbdsy3HcORaDsB9FfDiB2LDoMZ0KLOck1RjsdfvevzBRG%2FJBMNOckNcGOqUBq7SZvcU8LPSUxzXrr404lD7iBK3RmIolpGoEt%2FDJwr7nSd1Ol4o3wRgHJ317JXYUcyafCH2Wbm4Iw5MDPiXp9fLTNq6T99AxqPhbaWcJhHrqo1WBzhZy8O%2B30IRNPNeNPDBD3H8t5tv61CtNpoxxXj63JF4SHaEm1byi5Oz%2FhAiij0rXQFGTV0IPv7HkLOhhY2C8Psj2qMuPcxBsNMeibm96LSSn&X-Amz-Signature=ce74a726227ba9277376a3fc817db7258e44858a338ed4ebb0486e54e78d3464&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
