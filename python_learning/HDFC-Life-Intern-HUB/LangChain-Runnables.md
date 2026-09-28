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
fetched_at: '2026-09-28T02:32:24.255Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634ZUAMUE%2F20260928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260928T023220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJIMEYCIQDWs2HvWHZA2WebYSDWUHfAq1LruflCK2vBmt5Dazhf1AIhAN5kfIvPKCEFXCilYV%2FxSqOuzWgIlTtAV%2BhHO2Wi%2Bv9kKv8DCCsQABoMNjM3NDIzMTgzODA1Igw6K0gWhENXFXYJWE4q3AMy%2FTgDQ9nMcsWzb7rsk17Aj%2FondicIwJ5vXphrB%2BOTQhhKNt6Xx%2BfP5mPlaFsPkqYR5edeJ7w6FfcJ1XBsx1FNkGb8J1e2k7PDFe%2FVcQJb5dcERiBF3%2Bzlz9p43HMAWcXDckFq4n2ZgBKb2DSNlNaTi73WjmJqBOXjJD491iDwBpAdOIahEMye5WhtKk2y84RE9TzZIFdT1r4%2BtiXyn4zq4XwBKUEf%2FRcdeD4zg17NHpoCbbNJ%2FDVwwIWywZWiz%2BTi1NUdfohBAuEDA56DpZy26cOskOrsg2e%2Fwdogc5S%2FyWSpwOvgSVVhKkM%2BeQRCaRVO9fbJjNFvPWGBXMtHkB%2B1KkfxGEsshABoui1bxlvwmHZrsJnI1bQ8cEXDIAR0zB%2Fl%2FU3o5KJo8gRCrfnk26bWc829H%2B%2FtqDv1wp8h8U%2F8OAECXV649r3H%2BpQuy06Dnp1j28jlRD4ofsnKIAc0HO3mJKK7e7%2FFE8mI9RDH%2BBvgVh5b2CLlTydvecRcdftgHyOBDPFBYdb9QKIxHfpKuV8PnnR%2Byep4Vr0blCsZUlcvdUkykcDQUGxNTgJZbO6QBJsSML7emSNtd3FwgiB2WvNJ9VCfY%2BuofWsMx7epJVDoK0yC6QLYXFfrq6B1xTCKnufVBjqkARDYML9Gr%2Bsm8aez%2F06o%2B0F63FWQXEGUXaKG5e7U8TuQwMqSkz69xj9dzHvWYU%2B2XvrvuDBQ%2FI0QBsJKWPG4xOZKSYMp%2Bq3eTeg5CmBGrZiJNAcJW8KpWo2Rfgc75z%2BzWSj4vNb2HkCBhzoeR4ZityhlLdn1j4Z4Vkv8azczdXzUM1%2BR%2FBmgyqa68CapmdT2y52VviZtNElT1GPnaO4DqY%2FvvW%2Br&X-Amz-Signature=a56bd0ff54b7189d6f0358c6a6ca9d99509669a40310dd96b671bb8506724602&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634ZUAMUE%2F20260928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260928T023220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJIMEYCIQDWs2HvWHZA2WebYSDWUHfAq1LruflCK2vBmt5Dazhf1AIhAN5kfIvPKCEFXCilYV%2FxSqOuzWgIlTtAV%2BhHO2Wi%2Bv9kKv8DCCsQABoMNjM3NDIzMTgzODA1Igw6K0gWhENXFXYJWE4q3AMy%2FTgDQ9nMcsWzb7rsk17Aj%2FondicIwJ5vXphrB%2BOTQhhKNt6Xx%2BfP5mPlaFsPkqYR5edeJ7w6FfcJ1XBsx1FNkGb8J1e2k7PDFe%2FVcQJb5dcERiBF3%2Bzlz9p43HMAWcXDckFq4n2ZgBKb2DSNlNaTi73WjmJqBOXjJD491iDwBpAdOIahEMye5WhtKk2y84RE9TzZIFdT1r4%2BtiXyn4zq4XwBKUEf%2FRcdeD4zg17NHpoCbbNJ%2FDVwwIWywZWiz%2BTi1NUdfohBAuEDA56DpZy26cOskOrsg2e%2Fwdogc5S%2FyWSpwOvgSVVhKkM%2BeQRCaRVO9fbJjNFvPWGBXMtHkB%2B1KkfxGEsshABoui1bxlvwmHZrsJnI1bQ8cEXDIAR0zB%2Fl%2FU3o5KJo8gRCrfnk26bWc829H%2B%2FtqDv1wp8h8U%2F8OAECXV649r3H%2BpQuy06Dnp1j28jlRD4ofsnKIAc0HO3mJKK7e7%2FFE8mI9RDH%2BBvgVh5b2CLlTydvecRcdftgHyOBDPFBYdb9QKIxHfpKuV8PnnR%2Byep4Vr0blCsZUlcvdUkykcDQUGxNTgJZbO6QBJsSML7emSNtd3FwgiB2WvNJ9VCfY%2BuofWsMx7epJVDoK0yC6QLYXFfrq6B1xTCKnufVBjqkARDYML9Gr%2Bsm8aez%2F06o%2B0F63FWQXEGUXaKG5e7U8TuQwMqSkz69xj9dzHvWYU%2B2XvrvuDBQ%2FI0QBsJKWPG4xOZKSYMp%2Bq3eTeg5CmBGrZiJNAcJW8KpWo2Rfgc75z%2BzWSj4vNb2HkCBhzoeR4ZityhlLdn1j4Z4Vkv8azczdXzUM1%2BR%2FBmgyqa68CapmdT2y52VviZtNElT1GPnaO4DqY%2FvvW%2Br&X-Amz-Signature=181e2c81ebcdc3b344e11f9472552dc25b49082b3bc7dc31fb0d1ea9fd1701ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634ZUAMUE%2F20260928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260928T023220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJIMEYCIQDWs2HvWHZA2WebYSDWUHfAq1LruflCK2vBmt5Dazhf1AIhAN5kfIvPKCEFXCilYV%2FxSqOuzWgIlTtAV%2BhHO2Wi%2Bv9kKv8DCCsQABoMNjM3NDIzMTgzODA1Igw6K0gWhENXFXYJWE4q3AMy%2FTgDQ9nMcsWzb7rsk17Aj%2FondicIwJ5vXphrB%2BOTQhhKNt6Xx%2BfP5mPlaFsPkqYR5edeJ7w6FfcJ1XBsx1FNkGb8J1e2k7PDFe%2FVcQJb5dcERiBF3%2Bzlz9p43HMAWcXDckFq4n2ZgBKb2DSNlNaTi73WjmJqBOXjJD491iDwBpAdOIahEMye5WhtKk2y84RE9TzZIFdT1r4%2BtiXyn4zq4XwBKUEf%2FRcdeD4zg17NHpoCbbNJ%2FDVwwIWywZWiz%2BTi1NUdfohBAuEDA56DpZy26cOskOrsg2e%2Fwdogc5S%2FyWSpwOvgSVVhKkM%2BeQRCaRVO9fbJjNFvPWGBXMtHkB%2B1KkfxGEsshABoui1bxlvwmHZrsJnI1bQ8cEXDIAR0zB%2Fl%2FU3o5KJo8gRCrfnk26bWc829H%2B%2FtqDv1wp8h8U%2F8OAECXV649r3H%2BpQuy06Dnp1j28jlRD4ofsnKIAc0HO3mJKK7e7%2FFE8mI9RDH%2BBvgVh5b2CLlTydvecRcdftgHyOBDPFBYdb9QKIxHfpKuV8PnnR%2Byep4Vr0blCsZUlcvdUkykcDQUGxNTgJZbO6QBJsSML7emSNtd3FwgiB2WvNJ9VCfY%2BuofWsMx7epJVDoK0yC6QLYXFfrq6B1xTCKnufVBjqkARDYML9Gr%2Bsm8aez%2F06o%2B0F63FWQXEGUXaKG5e7U8TuQwMqSkz69xj9dzHvWYU%2B2XvrvuDBQ%2FI0QBsJKWPG4xOZKSYMp%2Bq3eTeg5CmBGrZiJNAcJW8KpWo2Rfgc75z%2BzWSj4vNb2HkCBhzoeR4ZityhlLdn1j4Z4Vkv8azczdXzUM1%2BR%2FBmgyqa68CapmdT2y52VviZtNElT1GPnaO4DqY%2FvvW%2Br&X-Amz-Signature=e7d321f8c686f7944b9f742d531259b3fa7233eda235729bc3bd2c2e7d0c79f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634ZUAMUE%2F20260928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260928T023220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJIMEYCIQDWs2HvWHZA2WebYSDWUHfAq1LruflCK2vBmt5Dazhf1AIhAN5kfIvPKCEFXCilYV%2FxSqOuzWgIlTtAV%2BhHO2Wi%2Bv9kKv8DCCsQABoMNjM3NDIzMTgzODA1Igw6K0gWhENXFXYJWE4q3AMy%2FTgDQ9nMcsWzb7rsk17Aj%2FondicIwJ5vXphrB%2BOTQhhKNt6Xx%2BfP5mPlaFsPkqYR5edeJ7w6FfcJ1XBsx1FNkGb8J1e2k7PDFe%2FVcQJb5dcERiBF3%2Bzlz9p43HMAWcXDckFq4n2ZgBKb2DSNlNaTi73WjmJqBOXjJD491iDwBpAdOIahEMye5WhtKk2y84RE9TzZIFdT1r4%2BtiXyn4zq4XwBKUEf%2FRcdeD4zg17NHpoCbbNJ%2FDVwwIWywZWiz%2BTi1NUdfohBAuEDA56DpZy26cOskOrsg2e%2Fwdogc5S%2FyWSpwOvgSVVhKkM%2BeQRCaRVO9fbJjNFvPWGBXMtHkB%2B1KkfxGEsshABoui1bxlvwmHZrsJnI1bQ8cEXDIAR0zB%2Fl%2FU3o5KJo8gRCrfnk26bWc829H%2B%2FtqDv1wp8h8U%2F8OAECXV649r3H%2BpQuy06Dnp1j28jlRD4ofsnKIAc0HO3mJKK7e7%2FFE8mI9RDH%2BBvgVh5b2CLlTydvecRcdftgHyOBDPFBYdb9QKIxHfpKuV8PnnR%2Byep4Vr0blCsZUlcvdUkykcDQUGxNTgJZbO6QBJsSML7emSNtd3FwgiB2WvNJ9VCfY%2BuofWsMx7epJVDoK0yC6QLYXFfrq6B1xTCKnufVBjqkARDYML9Gr%2Bsm8aez%2F06o%2B0F63FWQXEGUXaKG5e7U8TuQwMqSkz69xj9dzHvWYU%2B2XvrvuDBQ%2FI0QBsJKWPG4xOZKSYMp%2Bq3eTeg5CmBGrZiJNAcJW8KpWo2Rfgc75z%2BzWSj4vNb2HkCBhzoeR4ZityhlLdn1j4Z4Vkv8azczdXzUM1%2BR%2FBmgyqa68CapmdT2y52VviZtNElT1GPnaO4DqY%2FvvW%2Br&X-Amz-Signature=d7743921efaee2ce8038c2f05130ad9710651a353f8016a7549c9323fd045b08&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634ZUAMUE%2F20260928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260928T023220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJIMEYCIQDWs2HvWHZA2WebYSDWUHfAq1LruflCK2vBmt5Dazhf1AIhAN5kfIvPKCEFXCilYV%2FxSqOuzWgIlTtAV%2BhHO2Wi%2Bv9kKv8DCCsQABoMNjM3NDIzMTgzODA1Igw6K0gWhENXFXYJWE4q3AMy%2FTgDQ9nMcsWzb7rsk17Aj%2FondicIwJ5vXphrB%2BOTQhhKNt6Xx%2BfP5mPlaFsPkqYR5edeJ7w6FfcJ1XBsx1FNkGb8J1e2k7PDFe%2FVcQJb5dcERiBF3%2Bzlz9p43HMAWcXDckFq4n2ZgBKb2DSNlNaTi73WjmJqBOXjJD491iDwBpAdOIahEMye5WhtKk2y84RE9TzZIFdT1r4%2BtiXyn4zq4XwBKUEf%2FRcdeD4zg17NHpoCbbNJ%2FDVwwIWywZWiz%2BTi1NUdfohBAuEDA56DpZy26cOskOrsg2e%2Fwdogc5S%2FyWSpwOvgSVVhKkM%2BeQRCaRVO9fbJjNFvPWGBXMtHkB%2B1KkfxGEsshABoui1bxlvwmHZrsJnI1bQ8cEXDIAR0zB%2Fl%2FU3o5KJo8gRCrfnk26bWc829H%2B%2FtqDv1wp8h8U%2F8OAECXV649r3H%2BpQuy06Dnp1j28jlRD4ofsnKIAc0HO3mJKK7e7%2FFE8mI9RDH%2BBvgVh5b2CLlTydvecRcdftgHyOBDPFBYdb9QKIxHfpKuV8PnnR%2Byep4Vr0blCsZUlcvdUkykcDQUGxNTgJZbO6QBJsSML7emSNtd3FwgiB2WvNJ9VCfY%2BuofWsMx7epJVDoK0yC6QLYXFfrq6B1xTCKnufVBjqkARDYML9Gr%2Bsm8aez%2F06o%2B0F63FWQXEGUXaKG5e7U8TuQwMqSkz69xj9dzHvWYU%2B2XvrvuDBQ%2FI0QBsJKWPG4xOZKSYMp%2Bq3eTeg5CmBGrZiJNAcJW8KpWo2Rfgc75z%2BzWSj4vNb2HkCBhzoeR4ZityhlLdn1j4Z4Vkv8azczdXzUM1%2BR%2FBmgyqa68CapmdT2y52VviZtNElT1GPnaO4DqY%2FvvW%2Br&X-Amz-Signature=fcb3253b0091989b2cded4e3f379e37a9b15d6c53dc1f31516477fd14345edd4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634ZUAMUE%2F20260928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260928T023220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJIMEYCIQDWs2HvWHZA2WebYSDWUHfAq1LruflCK2vBmt5Dazhf1AIhAN5kfIvPKCEFXCilYV%2FxSqOuzWgIlTtAV%2BhHO2Wi%2Bv9kKv8DCCsQABoMNjM3NDIzMTgzODA1Igw6K0gWhENXFXYJWE4q3AMy%2FTgDQ9nMcsWzb7rsk17Aj%2FondicIwJ5vXphrB%2BOTQhhKNt6Xx%2BfP5mPlaFsPkqYR5edeJ7w6FfcJ1XBsx1FNkGb8J1e2k7PDFe%2FVcQJb5dcERiBF3%2Bzlz9p43HMAWcXDckFq4n2ZgBKb2DSNlNaTi73WjmJqBOXjJD491iDwBpAdOIahEMye5WhtKk2y84RE9TzZIFdT1r4%2BtiXyn4zq4XwBKUEf%2FRcdeD4zg17NHpoCbbNJ%2FDVwwIWywZWiz%2BTi1NUdfohBAuEDA56DpZy26cOskOrsg2e%2Fwdogc5S%2FyWSpwOvgSVVhKkM%2BeQRCaRVO9fbJjNFvPWGBXMtHkB%2B1KkfxGEsshABoui1bxlvwmHZrsJnI1bQ8cEXDIAR0zB%2Fl%2FU3o5KJo8gRCrfnk26bWc829H%2B%2FtqDv1wp8h8U%2F8OAECXV649r3H%2BpQuy06Dnp1j28jlRD4ofsnKIAc0HO3mJKK7e7%2FFE8mI9RDH%2BBvgVh5b2CLlTydvecRcdftgHyOBDPFBYdb9QKIxHfpKuV8PnnR%2Byep4Vr0blCsZUlcvdUkykcDQUGxNTgJZbO6QBJsSML7emSNtd3FwgiB2WvNJ9VCfY%2BuofWsMx7epJVDoK0yC6QLYXFfrq6B1xTCKnufVBjqkARDYML9Gr%2Bsm8aez%2F06o%2B0F63FWQXEGUXaKG5e7U8TuQwMqSkz69xj9dzHvWYU%2B2XvrvuDBQ%2FI0QBsJKWPG4xOZKSYMp%2Bq3eTeg5CmBGrZiJNAcJW8KpWo2Rfgc75z%2BzWSj4vNb2HkCBhzoeR4ZityhlLdn1j4Z4Vkv8azczdXzUM1%2BR%2FBmgyqa68CapmdT2y52VviZtNElT1GPnaO4DqY%2FvvW%2Br&X-Amz-Signature=da6641f6d8d04e603905b4ed5b8e9bf7fe99fc49f596ad312e98597f8c0fca90&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634ZUAMUE%2F20260928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260928T023220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJIMEYCIQDWs2HvWHZA2WebYSDWUHfAq1LruflCK2vBmt5Dazhf1AIhAN5kfIvPKCEFXCilYV%2FxSqOuzWgIlTtAV%2BhHO2Wi%2Bv9kKv8DCCsQABoMNjM3NDIzMTgzODA1Igw6K0gWhENXFXYJWE4q3AMy%2FTgDQ9nMcsWzb7rsk17Aj%2FondicIwJ5vXphrB%2BOTQhhKNt6Xx%2BfP5mPlaFsPkqYR5edeJ7w6FfcJ1XBsx1FNkGb8J1e2k7PDFe%2FVcQJb5dcERiBF3%2Bzlz9p43HMAWcXDckFq4n2ZgBKb2DSNlNaTi73WjmJqBOXjJD491iDwBpAdOIahEMye5WhtKk2y84RE9TzZIFdT1r4%2BtiXyn4zq4XwBKUEf%2FRcdeD4zg17NHpoCbbNJ%2FDVwwIWywZWiz%2BTi1NUdfohBAuEDA56DpZy26cOskOrsg2e%2Fwdogc5S%2FyWSpwOvgSVVhKkM%2BeQRCaRVO9fbJjNFvPWGBXMtHkB%2B1KkfxGEsshABoui1bxlvwmHZrsJnI1bQ8cEXDIAR0zB%2Fl%2FU3o5KJo8gRCrfnk26bWc829H%2B%2FtqDv1wp8h8U%2F8OAECXV649r3H%2BpQuy06Dnp1j28jlRD4ofsnKIAc0HO3mJKK7e7%2FFE8mI9RDH%2BBvgVh5b2CLlTydvecRcdftgHyOBDPFBYdb9QKIxHfpKuV8PnnR%2Byep4Vr0blCsZUlcvdUkykcDQUGxNTgJZbO6QBJsSML7emSNtd3FwgiB2WvNJ9VCfY%2BuofWsMx7epJVDoK0yC6QLYXFfrq6B1xTCKnufVBjqkARDYML9Gr%2Bsm8aez%2F06o%2B0F63FWQXEGUXaKG5e7U8TuQwMqSkz69xj9dzHvWYU%2B2XvrvuDBQ%2FI0QBsJKWPG4xOZKSYMp%2Bq3eTeg5CmBGrZiJNAcJW8KpWo2Rfgc75z%2BzWSj4vNb2HkCBhzoeR4ZityhlLdn1j4Z4Vkv8azczdXzUM1%2BR%2FBmgyqa68CapmdT2y52VviZtNElT1GPnaO4DqY%2FvvW%2Br&X-Amz-Signature=28861ecc06f266b9a300444311529ba0e33672f7831aab64e186c0059585ed87&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634ZUAMUE%2F20260928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260928T023220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJIMEYCIQDWs2HvWHZA2WebYSDWUHfAq1LruflCK2vBmt5Dazhf1AIhAN5kfIvPKCEFXCilYV%2FxSqOuzWgIlTtAV%2BhHO2Wi%2Bv9kKv8DCCsQABoMNjM3NDIzMTgzODA1Igw6K0gWhENXFXYJWE4q3AMy%2FTgDQ9nMcsWzb7rsk17Aj%2FondicIwJ5vXphrB%2BOTQhhKNt6Xx%2BfP5mPlaFsPkqYR5edeJ7w6FfcJ1XBsx1FNkGb8J1e2k7PDFe%2FVcQJb5dcERiBF3%2Bzlz9p43HMAWcXDckFq4n2ZgBKb2DSNlNaTi73WjmJqBOXjJD491iDwBpAdOIahEMye5WhtKk2y84RE9TzZIFdT1r4%2BtiXyn4zq4XwBKUEf%2FRcdeD4zg17NHpoCbbNJ%2FDVwwIWywZWiz%2BTi1NUdfohBAuEDA56DpZy26cOskOrsg2e%2Fwdogc5S%2FyWSpwOvgSVVhKkM%2BeQRCaRVO9fbJjNFvPWGBXMtHkB%2B1KkfxGEsshABoui1bxlvwmHZrsJnI1bQ8cEXDIAR0zB%2Fl%2FU3o5KJo8gRCrfnk26bWc829H%2B%2FtqDv1wp8h8U%2F8OAECXV649r3H%2BpQuy06Dnp1j28jlRD4ofsnKIAc0HO3mJKK7e7%2FFE8mI9RDH%2BBvgVh5b2CLlTydvecRcdftgHyOBDPFBYdb9QKIxHfpKuV8PnnR%2Byep4Vr0blCsZUlcvdUkykcDQUGxNTgJZbO6QBJsSML7emSNtd3FwgiB2WvNJ9VCfY%2BuofWsMx7epJVDoK0yC6QLYXFfrq6B1xTCKnufVBjqkARDYML9Gr%2Bsm8aez%2F06o%2B0F63FWQXEGUXaKG5e7U8TuQwMqSkz69xj9dzHvWYU%2B2XvrvuDBQ%2FI0QBsJKWPG4xOZKSYMp%2Bq3eTeg5CmBGrZiJNAcJW8KpWo2Rfgc75z%2BzWSj4vNb2HkCBhzoeR4ZityhlLdn1j4Z4Vkv8azczdXzUM1%2BR%2FBmgyqa68CapmdT2y52VviZtNElT1GPnaO4DqY%2FvvW%2Br&X-Amz-Signature=172c692f12ec5b6b190b4d23f1f69d84eb1a65de8994183f3ee6aee021aefe2b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634ZUAMUE%2F20260928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260928T023220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJIMEYCIQDWs2HvWHZA2WebYSDWUHfAq1LruflCK2vBmt5Dazhf1AIhAN5kfIvPKCEFXCilYV%2FxSqOuzWgIlTtAV%2BhHO2Wi%2Bv9kKv8DCCsQABoMNjM3NDIzMTgzODA1Igw6K0gWhENXFXYJWE4q3AMy%2FTgDQ9nMcsWzb7rsk17Aj%2FondicIwJ5vXphrB%2BOTQhhKNt6Xx%2BfP5mPlaFsPkqYR5edeJ7w6FfcJ1XBsx1FNkGb8J1e2k7PDFe%2FVcQJb5dcERiBF3%2Bzlz9p43HMAWcXDckFq4n2ZgBKb2DSNlNaTi73WjmJqBOXjJD491iDwBpAdOIahEMye5WhtKk2y84RE9TzZIFdT1r4%2BtiXyn4zq4XwBKUEf%2FRcdeD4zg17NHpoCbbNJ%2FDVwwIWywZWiz%2BTi1NUdfohBAuEDA56DpZy26cOskOrsg2e%2Fwdogc5S%2FyWSpwOvgSVVhKkM%2BeQRCaRVO9fbJjNFvPWGBXMtHkB%2B1KkfxGEsshABoui1bxlvwmHZrsJnI1bQ8cEXDIAR0zB%2Fl%2FU3o5KJo8gRCrfnk26bWc829H%2B%2FtqDv1wp8h8U%2F8OAECXV649r3H%2BpQuy06Dnp1j28jlRD4ofsnKIAc0HO3mJKK7e7%2FFE8mI9RDH%2BBvgVh5b2CLlTydvecRcdftgHyOBDPFBYdb9QKIxHfpKuV8PnnR%2Byep4Vr0blCsZUlcvdUkykcDQUGxNTgJZbO6QBJsSML7emSNtd3FwgiB2WvNJ9VCfY%2BuofWsMx7epJVDoK0yC6QLYXFfrq6B1xTCKnufVBjqkARDYML9Gr%2Bsm8aez%2F06o%2B0F63FWQXEGUXaKG5e7U8TuQwMqSkz69xj9dzHvWYU%2B2XvrvuDBQ%2FI0QBsJKWPG4xOZKSYMp%2Bq3eTeg5CmBGrZiJNAcJW8KpWo2Rfgc75z%2BzWSj4vNb2HkCBhzoeR4ZityhlLdn1j4Z4Vkv8azczdXzUM1%2BR%2FBmgyqa68CapmdT2y52VviZtNElT1GPnaO4DqY%2FvvW%2Br&X-Amz-Signature=4f9c0a9dd97dc542733fd216c249a75355730fdaccdec34253a2facc852dd806&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
