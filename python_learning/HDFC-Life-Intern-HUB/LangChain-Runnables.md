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
fetched_at: '2026-10-06T03:48:47.413Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXT62QWS%2F20261006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261006T034842Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCIArO7qrXQEWiaqLpkRIV7xqCFChSs8v2PuWFahPmdfhEAiBpOXpx98ylBsY9xeOHNNcOx9Av3Zr8gmbJ4VSFTOsmmSqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpxGg19D5ErkIMw3wKtwDhGut92pwuDL%2F8hbxa%2BJAjFPeAL2moq24bMnvRisH%2Bemv5jRD%2BGaQ0AFQxVWqQNEVz%2FTCR311HGQEloLpXQz43VX7O5QmOw2RDX%2BO5hdxgmVvMbCh47bOF4t8%2FvGORi0ZHV4jIT3UQmkpCWTaPEh2eJ98yZQZu7w9Pvb3rIfMUNt7IHQCZT4MJ0N%2BNOyhBQ0bSbvtyj9q4jYQJa4qyBeO4apWPUB4iBw2LYnCpxHk8F2kBei7Mgyz%2BNvPsGVveYu%2FxeZht5IAspmgCwiOrvnPztNnY1E4w88iXZ0nZWEOavy6282fv7A%2FOLTst%2BBiQsl2MB1iAHkgI3hihx1vFRQeBTFLPIGOBDwYkUeAOUpp9XnCadYb%2Fz2ESFtxDOhGlUVoAJ5wWZyeGhfcUXv7NngFeiqpIK9xJ%2B72CRcYKKy5AfJuFKpwTX62239Bdcmenr78fLVMWkb6Pg38c2lVvrgJWzNGZj6aHKf7Itfr%2FZrY5JIUOkAhkG20OczXWWM%2F5lA99K2EqH0SR8aZccp9kp6Dm51REzQqJGWjtjVXi0RWTYTjfkT9c%2Fsw6U1GdTERUdcAY2Ahuj2htYm1qhX5UKgeJZNoo0kDRdy6CKuUuTSaqUO0Xf6p%2FhJ1ZXmiGG8w%2F9qR1gY6pgE2nc9U6mpWlfgZ8rBHwMlmmCeeRJO2Nc4loUx7AEUBczUn6AT86roJLO1g%2BE8teUGoKawcmfczsUYZcsav1xExYAWpZH8xo1Of1UZiuh6s0oYewDiIWwTe2bHviGT1yGpSF9hwkEAzShrB99Ki1AX4N6Ms0ZyqEIW9kkKUTMsYmo6X99cieFntHrI3trDsirWzHclBitXMl5Jud%2FaicBq2BDjeKh4e&X-Amz-Signature=b4e75b70c6214ba89b861a9c2bb4d88f5dc156b3e5dc5a025e719e54c944ce48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXT62QWS%2F20261006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261006T034842Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCIArO7qrXQEWiaqLpkRIV7xqCFChSs8v2PuWFahPmdfhEAiBpOXpx98ylBsY9xeOHNNcOx9Av3Zr8gmbJ4VSFTOsmmSqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpxGg19D5ErkIMw3wKtwDhGut92pwuDL%2F8hbxa%2BJAjFPeAL2moq24bMnvRisH%2Bemv5jRD%2BGaQ0AFQxVWqQNEVz%2FTCR311HGQEloLpXQz43VX7O5QmOw2RDX%2BO5hdxgmVvMbCh47bOF4t8%2FvGORi0ZHV4jIT3UQmkpCWTaPEh2eJ98yZQZu7w9Pvb3rIfMUNt7IHQCZT4MJ0N%2BNOyhBQ0bSbvtyj9q4jYQJa4qyBeO4apWPUB4iBw2LYnCpxHk8F2kBei7Mgyz%2BNvPsGVveYu%2FxeZht5IAspmgCwiOrvnPztNnY1E4w88iXZ0nZWEOavy6282fv7A%2FOLTst%2BBiQsl2MB1iAHkgI3hihx1vFRQeBTFLPIGOBDwYkUeAOUpp9XnCadYb%2Fz2ESFtxDOhGlUVoAJ5wWZyeGhfcUXv7NngFeiqpIK9xJ%2B72CRcYKKy5AfJuFKpwTX62239Bdcmenr78fLVMWkb6Pg38c2lVvrgJWzNGZj6aHKf7Itfr%2FZrY5JIUOkAhkG20OczXWWM%2F5lA99K2EqH0SR8aZccp9kp6Dm51REzQqJGWjtjVXi0RWTYTjfkT9c%2Fsw6U1GdTERUdcAY2Ahuj2htYm1qhX5UKgeJZNoo0kDRdy6CKuUuTSaqUO0Xf6p%2FhJ1ZXmiGG8w%2F9qR1gY6pgE2nc9U6mpWlfgZ8rBHwMlmmCeeRJO2Nc4loUx7AEUBczUn6AT86roJLO1g%2BE8teUGoKawcmfczsUYZcsav1xExYAWpZH8xo1Of1UZiuh6s0oYewDiIWwTe2bHviGT1yGpSF9hwkEAzShrB99Ki1AX4N6Ms0ZyqEIW9kkKUTMsYmo6X99cieFntHrI3trDsirWzHclBitXMl5Jud%2FaicBq2BDjeKh4e&X-Amz-Signature=12a41e37d26023a255642a4c01b6bcfe9e8207e75072d2d0f75f7aa501517326&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXT62QWS%2F20261006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261006T034842Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCIArO7qrXQEWiaqLpkRIV7xqCFChSs8v2PuWFahPmdfhEAiBpOXpx98ylBsY9xeOHNNcOx9Av3Zr8gmbJ4VSFTOsmmSqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpxGg19D5ErkIMw3wKtwDhGut92pwuDL%2F8hbxa%2BJAjFPeAL2moq24bMnvRisH%2Bemv5jRD%2BGaQ0AFQxVWqQNEVz%2FTCR311HGQEloLpXQz43VX7O5QmOw2RDX%2BO5hdxgmVvMbCh47bOF4t8%2FvGORi0ZHV4jIT3UQmkpCWTaPEh2eJ98yZQZu7w9Pvb3rIfMUNt7IHQCZT4MJ0N%2BNOyhBQ0bSbvtyj9q4jYQJa4qyBeO4apWPUB4iBw2LYnCpxHk8F2kBei7Mgyz%2BNvPsGVveYu%2FxeZht5IAspmgCwiOrvnPztNnY1E4w88iXZ0nZWEOavy6282fv7A%2FOLTst%2BBiQsl2MB1iAHkgI3hihx1vFRQeBTFLPIGOBDwYkUeAOUpp9XnCadYb%2Fz2ESFtxDOhGlUVoAJ5wWZyeGhfcUXv7NngFeiqpIK9xJ%2B72CRcYKKy5AfJuFKpwTX62239Bdcmenr78fLVMWkb6Pg38c2lVvrgJWzNGZj6aHKf7Itfr%2FZrY5JIUOkAhkG20OczXWWM%2F5lA99K2EqH0SR8aZccp9kp6Dm51REzQqJGWjtjVXi0RWTYTjfkT9c%2Fsw6U1GdTERUdcAY2Ahuj2htYm1qhX5UKgeJZNoo0kDRdy6CKuUuTSaqUO0Xf6p%2FhJ1ZXmiGG8w%2F9qR1gY6pgE2nc9U6mpWlfgZ8rBHwMlmmCeeRJO2Nc4loUx7AEUBczUn6AT86roJLO1g%2BE8teUGoKawcmfczsUYZcsav1xExYAWpZH8xo1Of1UZiuh6s0oYewDiIWwTe2bHviGT1yGpSF9hwkEAzShrB99Ki1AX4N6Ms0ZyqEIW9kkKUTMsYmo6X99cieFntHrI3trDsirWzHclBitXMl5Jud%2FaicBq2BDjeKh4e&X-Amz-Signature=0022201e22701490e9cdc7c14ab90abd297a8081e1b68540726e5cb6f174f5e5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXT62QWS%2F20261006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261006T034842Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCIArO7qrXQEWiaqLpkRIV7xqCFChSs8v2PuWFahPmdfhEAiBpOXpx98ylBsY9xeOHNNcOx9Av3Zr8gmbJ4VSFTOsmmSqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpxGg19D5ErkIMw3wKtwDhGut92pwuDL%2F8hbxa%2BJAjFPeAL2moq24bMnvRisH%2Bemv5jRD%2BGaQ0AFQxVWqQNEVz%2FTCR311HGQEloLpXQz43VX7O5QmOw2RDX%2BO5hdxgmVvMbCh47bOF4t8%2FvGORi0ZHV4jIT3UQmkpCWTaPEh2eJ98yZQZu7w9Pvb3rIfMUNt7IHQCZT4MJ0N%2BNOyhBQ0bSbvtyj9q4jYQJa4qyBeO4apWPUB4iBw2LYnCpxHk8F2kBei7Mgyz%2BNvPsGVveYu%2FxeZht5IAspmgCwiOrvnPztNnY1E4w88iXZ0nZWEOavy6282fv7A%2FOLTst%2BBiQsl2MB1iAHkgI3hihx1vFRQeBTFLPIGOBDwYkUeAOUpp9XnCadYb%2Fz2ESFtxDOhGlUVoAJ5wWZyeGhfcUXv7NngFeiqpIK9xJ%2B72CRcYKKy5AfJuFKpwTX62239Bdcmenr78fLVMWkb6Pg38c2lVvrgJWzNGZj6aHKf7Itfr%2FZrY5JIUOkAhkG20OczXWWM%2F5lA99K2EqH0SR8aZccp9kp6Dm51REzQqJGWjtjVXi0RWTYTjfkT9c%2Fsw6U1GdTERUdcAY2Ahuj2htYm1qhX5UKgeJZNoo0kDRdy6CKuUuTSaqUO0Xf6p%2FhJ1ZXmiGG8w%2F9qR1gY6pgE2nc9U6mpWlfgZ8rBHwMlmmCeeRJO2Nc4loUx7AEUBczUn6AT86roJLO1g%2BE8teUGoKawcmfczsUYZcsav1xExYAWpZH8xo1Of1UZiuh6s0oYewDiIWwTe2bHviGT1yGpSF9hwkEAzShrB99Ki1AX4N6Ms0ZyqEIW9kkKUTMsYmo6X99cieFntHrI3trDsirWzHclBitXMl5Jud%2FaicBq2BDjeKh4e&X-Amz-Signature=9a8ebbcf7210a64a7a571ad4a8c1bc34512836746a7b09ab2273080f38d0dd05&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXT62QWS%2F20261006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261006T034842Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCIArO7qrXQEWiaqLpkRIV7xqCFChSs8v2PuWFahPmdfhEAiBpOXpx98ylBsY9xeOHNNcOx9Av3Zr8gmbJ4VSFTOsmmSqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpxGg19D5ErkIMw3wKtwDhGut92pwuDL%2F8hbxa%2BJAjFPeAL2moq24bMnvRisH%2Bemv5jRD%2BGaQ0AFQxVWqQNEVz%2FTCR311HGQEloLpXQz43VX7O5QmOw2RDX%2BO5hdxgmVvMbCh47bOF4t8%2FvGORi0ZHV4jIT3UQmkpCWTaPEh2eJ98yZQZu7w9Pvb3rIfMUNt7IHQCZT4MJ0N%2BNOyhBQ0bSbvtyj9q4jYQJa4qyBeO4apWPUB4iBw2LYnCpxHk8F2kBei7Mgyz%2BNvPsGVveYu%2FxeZht5IAspmgCwiOrvnPztNnY1E4w88iXZ0nZWEOavy6282fv7A%2FOLTst%2BBiQsl2MB1iAHkgI3hihx1vFRQeBTFLPIGOBDwYkUeAOUpp9XnCadYb%2Fz2ESFtxDOhGlUVoAJ5wWZyeGhfcUXv7NngFeiqpIK9xJ%2B72CRcYKKy5AfJuFKpwTX62239Bdcmenr78fLVMWkb6Pg38c2lVvrgJWzNGZj6aHKf7Itfr%2FZrY5JIUOkAhkG20OczXWWM%2F5lA99K2EqH0SR8aZccp9kp6Dm51REzQqJGWjtjVXi0RWTYTjfkT9c%2Fsw6U1GdTERUdcAY2Ahuj2htYm1qhX5UKgeJZNoo0kDRdy6CKuUuTSaqUO0Xf6p%2FhJ1ZXmiGG8w%2F9qR1gY6pgE2nc9U6mpWlfgZ8rBHwMlmmCeeRJO2Nc4loUx7AEUBczUn6AT86roJLO1g%2BE8teUGoKawcmfczsUYZcsav1xExYAWpZH8xo1Of1UZiuh6s0oYewDiIWwTe2bHviGT1yGpSF9hwkEAzShrB99Ki1AX4N6Ms0ZyqEIW9kkKUTMsYmo6X99cieFntHrI3trDsirWzHclBitXMl5Jud%2FaicBq2BDjeKh4e&X-Amz-Signature=e4d11f5a59993426d904d8a038eeedc62c218f1e01014a1034fbe50ac9404e0f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXT62QWS%2F20261006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261006T034842Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCIArO7qrXQEWiaqLpkRIV7xqCFChSs8v2PuWFahPmdfhEAiBpOXpx98ylBsY9xeOHNNcOx9Av3Zr8gmbJ4VSFTOsmmSqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpxGg19D5ErkIMw3wKtwDhGut92pwuDL%2F8hbxa%2BJAjFPeAL2moq24bMnvRisH%2Bemv5jRD%2BGaQ0AFQxVWqQNEVz%2FTCR311HGQEloLpXQz43VX7O5QmOw2RDX%2BO5hdxgmVvMbCh47bOF4t8%2FvGORi0ZHV4jIT3UQmkpCWTaPEh2eJ98yZQZu7w9Pvb3rIfMUNt7IHQCZT4MJ0N%2BNOyhBQ0bSbvtyj9q4jYQJa4qyBeO4apWPUB4iBw2LYnCpxHk8F2kBei7Mgyz%2BNvPsGVveYu%2FxeZht5IAspmgCwiOrvnPztNnY1E4w88iXZ0nZWEOavy6282fv7A%2FOLTst%2BBiQsl2MB1iAHkgI3hihx1vFRQeBTFLPIGOBDwYkUeAOUpp9XnCadYb%2Fz2ESFtxDOhGlUVoAJ5wWZyeGhfcUXv7NngFeiqpIK9xJ%2B72CRcYKKy5AfJuFKpwTX62239Bdcmenr78fLVMWkb6Pg38c2lVvrgJWzNGZj6aHKf7Itfr%2FZrY5JIUOkAhkG20OczXWWM%2F5lA99K2EqH0SR8aZccp9kp6Dm51REzQqJGWjtjVXi0RWTYTjfkT9c%2Fsw6U1GdTERUdcAY2Ahuj2htYm1qhX5UKgeJZNoo0kDRdy6CKuUuTSaqUO0Xf6p%2FhJ1ZXmiGG8w%2F9qR1gY6pgE2nc9U6mpWlfgZ8rBHwMlmmCeeRJO2Nc4loUx7AEUBczUn6AT86roJLO1g%2BE8teUGoKawcmfczsUYZcsav1xExYAWpZH8xo1Of1UZiuh6s0oYewDiIWwTe2bHviGT1yGpSF9hwkEAzShrB99Ki1AX4N6Ms0ZyqEIW9kkKUTMsYmo6X99cieFntHrI3trDsirWzHclBitXMl5Jud%2FaicBq2BDjeKh4e&X-Amz-Signature=5fc8acda0f557e20014ebeca508a17a6955d133879aa49fc0965b3696913a7b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXT62QWS%2F20261006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261006T034842Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCIArO7qrXQEWiaqLpkRIV7xqCFChSs8v2PuWFahPmdfhEAiBpOXpx98ylBsY9xeOHNNcOx9Av3Zr8gmbJ4VSFTOsmmSqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpxGg19D5ErkIMw3wKtwDhGut92pwuDL%2F8hbxa%2BJAjFPeAL2moq24bMnvRisH%2Bemv5jRD%2BGaQ0AFQxVWqQNEVz%2FTCR311HGQEloLpXQz43VX7O5QmOw2RDX%2BO5hdxgmVvMbCh47bOF4t8%2FvGORi0ZHV4jIT3UQmkpCWTaPEh2eJ98yZQZu7w9Pvb3rIfMUNt7IHQCZT4MJ0N%2BNOyhBQ0bSbvtyj9q4jYQJa4qyBeO4apWPUB4iBw2LYnCpxHk8F2kBei7Mgyz%2BNvPsGVveYu%2FxeZht5IAspmgCwiOrvnPztNnY1E4w88iXZ0nZWEOavy6282fv7A%2FOLTst%2BBiQsl2MB1iAHkgI3hihx1vFRQeBTFLPIGOBDwYkUeAOUpp9XnCadYb%2Fz2ESFtxDOhGlUVoAJ5wWZyeGhfcUXv7NngFeiqpIK9xJ%2B72CRcYKKy5AfJuFKpwTX62239Bdcmenr78fLVMWkb6Pg38c2lVvrgJWzNGZj6aHKf7Itfr%2FZrY5JIUOkAhkG20OczXWWM%2F5lA99K2EqH0SR8aZccp9kp6Dm51REzQqJGWjtjVXi0RWTYTjfkT9c%2Fsw6U1GdTERUdcAY2Ahuj2htYm1qhX5UKgeJZNoo0kDRdy6CKuUuTSaqUO0Xf6p%2FhJ1ZXmiGG8w%2F9qR1gY6pgE2nc9U6mpWlfgZ8rBHwMlmmCeeRJO2Nc4loUx7AEUBczUn6AT86roJLO1g%2BE8teUGoKawcmfczsUYZcsav1xExYAWpZH8xo1Of1UZiuh6s0oYewDiIWwTe2bHviGT1yGpSF9hwkEAzShrB99Ki1AX4N6Ms0ZyqEIW9kkKUTMsYmo6X99cieFntHrI3trDsirWzHclBitXMl5Jud%2FaicBq2BDjeKh4e&X-Amz-Signature=5f81cc30cb8b13c5428d4e195b00a5edb31285fdb6deecdefc0bd84c23cb9599&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXT62QWS%2F20261006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261006T034842Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCIArO7qrXQEWiaqLpkRIV7xqCFChSs8v2PuWFahPmdfhEAiBpOXpx98ylBsY9xeOHNNcOx9Av3Zr8gmbJ4VSFTOsmmSqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpxGg19D5ErkIMw3wKtwDhGut92pwuDL%2F8hbxa%2BJAjFPeAL2moq24bMnvRisH%2Bemv5jRD%2BGaQ0AFQxVWqQNEVz%2FTCR311HGQEloLpXQz43VX7O5QmOw2RDX%2BO5hdxgmVvMbCh47bOF4t8%2FvGORi0ZHV4jIT3UQmkpCWTaPEh2eJ98yZQZu7w9Pvb3rIfMUNt7IHQCZT4MJ0N%2BNOyhBQ0bSbvtyj9q4jYQJa4qyBeO4apWPUB4iBw2LYnCpxHk8F2kBei7Mgyz%2BNvPsGVveYu%2FxeZht5IAspmgCwiOrvnPztNnY1E4w88iXZ0nZWEOavy6282fv7A%2FOLTst%2BBiQsl2MB1iAHkgI3hihx1vFRQeBTFLPIGOBDwYkUeAOUpp9XnCadYb%2Fz2ESFtxDOhGlUVoAJ5wWZyeGhfcUXv7NngFeiqpIK9xJ%2B72CRcYKKy5AfJuFKpwTX62239Bdcmenr78fLVMWkb6Pg38c2lVvrgJWzNGZj6aHKf7Itfr%2FZrY5JIUOkAhkG20OczXWWM%2F5lA99K2EqH0SR8aZccp9kp6Dm51REzQqJGWjtjVXi0RWTYTjfkT9c%2Fsw6U1GdTERUdcAY2Ahuj2htYm1qhX5UKgeJZNoo0kDRdy6CKuUuTSaqUO0Xf6p%2FhJ1ZXmiGG8w%2F9qR1gY6pgE2nc9U6mpWlfgZ8rBHwMlmmCeeRJO2Nc4loUx7AEUBczUn6AT86roJLO1g%2BE8teUGoKawcmfczsUYZcsav1xExYAWpZH8xo1Of1UZiuh6s0oYewDiIWwTe2bHviGT1yGpSF9hwkEAzShrB99Ki1AX4N6Ms0ZyqEIW9kkKUTMsYmo6X99cieFntHrI3trDsirWzHclBitXMl5Jud%2FaicBq2BDjeKh4e&X-Amz-Signature=19772c908818ffdada9a79b898fae98ee67b0023146b66d2c8ed84c94161420b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXT62QWS%2F20261006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261006T034842Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCIArO7qrXQEWiaqLpkRIV7xqCFChSs8v2PuWFahPmdfhEAiBpOXpx98ylBsY9xeOHNNcOx9Av3Zr8gmbJ4VSFTOsmmSqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpxGg19D5ErkIMw3wKtwDhGut92pwuDL%2F8hbxa%2BJAjFPeAL2moq24bMnvRisH%2Bemv5jRD%2BGaQ0AFQxVWqQNEVz%2FTCR311HGQEloLpXQz43VX7O5QmOw2RDX%2BO5hdxgmVvMbCh47bOF4t8%2FvGORi0ZHV4jIT3UQmkpCWTaPEh2eJ98yZQZu7w9Pvb3rIfMUNt7IHQCZT4MJ0N%2BNOyhBQ0bSbvtyj9q4jYQJa4qyBeO4apWPUB4iBw2LYnCpxHk8F2kBei7Mgyz%2BNvPsGVveYu%2FxeZht5IAspmgCwiOrvnPztNnY1E4w88iXZ0nZWEOavy6282fv7A%2FOLTst%2BBiQsl2MB1iAHkgI3hihx1vFRQeBTFLPIGOBDwYkUeAOUpp9XnCadYb%2Fz2ESFtxDOhGlUVoAJ5wWZyeGhfcUXv7NngFeiqpIK9xJ%2B72CRcYKKy5AfJuFKpwTX62239Bdcmenr78fLVMWkb6Pg38c2lVvrgJWzNGZj6aHKf7Itfr%2FZrY5JIUOkAhkG20OczXWWM%2F5lA99K2EqH0SR8aZccp9kp6Dm51REzQqJGWjtjVXi0RWTYTjfkT9c%2Fsw6U1GdTERUdcAY2Ahuj2htYm1qhX5UKgeJZNoo0kDRdy6CKuUuTSaqUO0Xf6p%2FhJ1ZXmiGG8w%2F9qR1gY6pgE2nc9U6mpWlfgZ8rBHwMlmmCeeRJO2Nc4loUx7AEUBczUn6AT86roJLO1g%2BE8teUGoKawcmfczsUYZcsav1xExYAWpZH8xo1Of1UZiuh6s0oYewDiIWwTe2bHviGT1yGpSF9hwkEAzShrB99Ki1AX4N6Ms0ZyqEIW9kkKUTMsYmo6X99cieFntHrI3trDsirWzHclBitXMl5Jud%2FaicBq2BDjeKh4e&X-Amz-Signature=ba551543da7d34ff01cf91035195ba2b92174664d29664c4c6d520d47b56dbd4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
