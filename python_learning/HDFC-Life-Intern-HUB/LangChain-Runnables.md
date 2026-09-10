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
fetched_at: '2026-09-10T02:03:34.715Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PLWIZMW%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020330Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSxYKWEvMNpGvx7N5n%2BcMV05jJAjmWN16AZvA9QNgL5gIhALKOPVQnuppTU0ARSEMgeeMiLoneTcSBDznaKtCYcbPXKv8DCHsQABoMNjM3NDIzMTgzODA1IgxgFMlOBQlsGOk8b8Yq3APfo7E53WMaSszrLm6Pmx1Ka5CK0%2Bs2cDcUMsRNQKctV5AnM6lQey2ePdmt807OHaeSoEhXSHYeAxdK5R3mV1YH%2FQvwZepTqOsk0PuMfUQs7tCm4kNHUWw%2B3sIlaPyqx8rfNnpe2FYzojwQ5v2%2Fmee8Lk8FBlWp9cGK9hYDt9BarVOsZUXspzrA16%2BNdBeAkGDlsV%2BVd%2FHcrLS46sOAnr%2F1aM9iCpRwU7BX6P%2Fpps00iy09j2o3OdOOLzLlwDttAaM4inhjLDqgFSoP5Qh6zoLS5QGb%2BBZ9NKTK2ReTxKzOZxM%2BSy2WUsyHUGwe6r%2BeBbabWN1ShDhB0u2BWnO30dx72Zgoy4nZMqdMaSqqL09M%2FLKAEnsr5HjeFsPxtpH%2FLL0bjaqH%2FgbAcEXxqAXD2XfsAaaZYxl2OTMIpYCkhNk352sj99pS7Yk2gjDynH%2BDtxIRiDbJmQvNBMW1%2BBKhszw353oScHPk7JqRVKmuAEv3MHOR1SkPgJDUf4LsNNfSjCCrxUUm%2F6GeUiAzFOzWN%2FGNs%2Fob9V4edJKRKtIYiN5iwbb7BFw%2BtXkN7sNf9p5Z2rDE7oysac6CXXXu33r2J9DXdEGSQmozx%2BPuRLa76%2Fhs4zc9YZw%2FJSCUL6OePzDEkojVBjqkAa5eR7Nyqvehu7F2FHi32jmqPMNBopAwmRx6UpHZkvkE6FxyazaIdDH1%2F%2BBhlAmv4VHsRnjQTicU6BHPKX1ZlW%2BeGGGl0jw%2FcvhQ9%2FNnhXHLqJHOuvyz%2FqK43vjMTEREUG41DEBZbMalDK6e4IaEVqWB4B6U7jcKLaLVy1fv3kdpEqqcsBLGlBM2bag5zIw5DOUclak%2FzWbFYsGtD%2F3FJ%2FMxXryz&X-Amz-Signature=559a194fcd2999de6e8fce1f65e86612f14b67745099793e8f476e8e5eae2e18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PLWIZMW%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020330Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSxYKWEvMNpGvx7N5n%2BcMV05jJAjmWN16AZvA9QNgL5gIhALKOPVQnuppTU0ARSEMgeeMiLoneTcSBDznaKtCYcbPXKv8DCHsQABoMNjM3NDIzMTgzODA1IgxgFMlOBQlsGOk8b8Yq3APfo7E53WMaSszrLm6Pmx1Ka5CK0%2Bs2cDcUMsRNQKctV5AnM6lQey2ePdmt807OHaeSoEhXSHYeAxdK5R3mV1YH%2FQvwZepTqOsk0PuMfUQs7tCm4kNHUWw%2B3sIlaPyqx8rfNnpe2FYzojwQ5v2%2Fmee8Lk8FBlWp9cGK9hYDt9BarVOsZUXspzrA16%2BNdBeAkGDlsV%2BVd%2FHcrLS46sOAnr%2F1aM9iCpRwU7BX6P%2Fpps00iy09j2o3OdOOLzLlwDttAaM4inhjLDqgFSoP5Qh6zoLS5QGb%2BBZ9NKTK2ReTxKzOZxM%2BSy2WUsyHUGwe6r%2BeBbabWN1ShDhB0u2BWnO30dx72Zgoy4nZMqdMaSqqL09M%2FLKAEnsr5HjeFsPxtpH%2FLL0bjaqH%2FgbAcEXxqAXD2XfsAaaZYxl2OTMIpYCkhNk352sj99pS7Yk2gjDynH%2BDtxIRiDbJmQvNBMW1%2BBKhszw353oScHPk7JqRVKmuAEv3MHOR1SkPgJDUf4LsNNfSjCCrxUUm%2F6GeUiAzFOzWN%2FGNs%2Fob9V4edJKRKtIYiN5iwbb7BFw%2BtXkN7sNf9p5Z2rDE7oysac6CXXXu33r2J9DXdEGSQmozx%2BPuRLa76%2Fhs4zc9YZw%2FJSCUL6OePzDEkojVBjqkAa5eR7Nyqvehu7F2FHi32jmqPMNBopAwmRx6UpHZkvkE6FxyazaIdDH1%2F%2BBhlAmv4VHsRnjQTicU6BHPKX1ZlW%2BeGGGl0jw%2FcvhQ9%2FNnhXHLqJHOuvyz%2FqK43vjMTEREUG41DEBZbMalDK6e4IaEVqWB4B6U7jcKLaLVy1fv3kdpEqqcsBLGlBM2bag5zIw5DOUclak%2FzWbFYsGtD%2F3FJ%2FMxXryz&X-Amz-Signature=045fa46fcc8716d3f9025caa8f80bbd1402075db4acbe7378d50eb19e1915922&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PLWIZMW%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020330Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSxYKWEvMNpGvx7N5n%2BcMV05jJAjmWN16AZvA9QNgL5gIhALKOPVQnuppTU0ARSEMgeeMiLoneTcSBDznaKtCYcbPXKv8DCHsQABoMNjM3NDIzMTgzODA1IgxgFMlOBQlsGOk8b8Yq3APfo7E53WMaSszrLm6Pmx1Ka5CK0%2Bs2cDcUMsRNQKctV5AnM6lQey2ePdmt807OHaeSoEhXSHYeAxdK5R3mV1YH%2FQvwZepTqOsk0PuMfUQs7tCm4kNHUWw%2B3sIlaPyqx8rfNnpe2FYzojwQ5v2%2Fmee8Lk8FBlWp9cGK9hYDt9BarVOsZUXspzrA16%2BNdBeAkGDlsV%2BVd%2FHcrLS46sOAnr%2F1aM9iCpRwU7BX6P%2Fpps00iy09j2o3OdOOLzLlwDttAaM4inhjLDqgFSoP5Qh6zoLS5QGb%2BBZ9NKTK2ReTxKzOZxM%2BSy2WUsyHUGwe6r%2BeBbabWN1ShDhB0u2BWnO30dx72Zgoy4nZMqdMaSqqL09M%2FLKAEnsr5HjeFsPxtpH%2FLL0bjaqH%2FgbAcEXxqAXD2XfsAaaZYxl2OTMIpYCkhNk352sj99pS7Yk2gjDynH%2BDtxIRiDbJmQvNBMW1%2BBKhszw353oScHPk7JqRVKmuAEv3MHOR1SkPgJDUf4LsNNfSjCCrxUUm%2F6GeUiAzFOzWN%2FGNs%2Fob9V4edJKRKtIYiN5iwbb7BFw%2BtXkN7sNf9p5Z2rDE7oysac6CXXXu33r2J9DXdEGSQmozx%2BPuRLa76%2Fhs4zc9YZw%2FJSCUL6OePzDEkojVBjqkAa5eR7Nyqvehu7F2FHi32jmqPMNBopAwmRx6UpHZkvkE6FxyazaIdDH1%2F%2BBhlAmv4VHsRnjQTicU6BHPKX1ZlW%2BeGGGl0jw%2FcvhQ9%2FNnhXHLqJHOuvyz%2FqK43vjMTEREUG41DEBZbMalDK6e4IaEVqWB4B6U7jcKLaLVy1fv3kdpEqqcsBLGlBM2bag5zIw5DOUclak%2FzWbFYsGtD%2F3FJ%2FMxXryz&X-Amz-Signature=8729db93ff451e710a7adc69e2c212b16e4773461b635cf02e66bfe85475c0a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PLWIZMW%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020330Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSxYKWEvMNpGvx7N5n%2BcMV05jJAjmWN16AZvA9QNgL5gIhALKOPVQnuppTU0ARSEMgeeMiLoneTcSBDznaKtCYcbPXKv8DCHsQABoMNjM3NDIzMTgzODA1IgxgFMlOBQlsGOk8b8Yq3APfo7E53WMaSszrLm6Pmx1Ka5CK0%2Bs2cDcUMsRNQKctV5AnM6lQey2ePdmt807OHaeSoEhXSHYeAxdK5R3mV1YH%2FQvwZepTqOsk0PuMfUQs7tCm4kNHUWw%2B3sIlaPyqx8rfNnpe2FYzojwQ5v2%2Fmee8Lk8FBlWp9cGK9hYDt9BarVOsZUXspzrA16%2BNdBeAkGDlsV%2BVd%2FHcrLS46sOAnr%2F1aM9iCpRwU7BX6P%2Fpps00iy09j2o3OdOOLzLlwDttAaM4inhjLDqgFSoP5Qh6zoLS5QGb%2BBZ9NKTK2ReTxKzOZxM%2BSy2WUsyHUGwe6r%2BeBbabWN1ShDhB0u2BWnO30dx72Zgoy4nZMqdMaSqqL09M%2FLKAEnsr5HjeFsPxtpH%2FLL0bjaqH%2FgbAcEXxqAXD2XfsAaaZYxl2OTMIpYCkhNk352sj99pS7Yk2gjDynH%2BDtxIRiDbJmQvNBMW1%2BBKhszw353oScHPk7JqRVKmuAEv3MHOR1SkPgJDUf4LsNNfSjCCrxUUm%2F6GeUiAzFOzWN%2FGNs%2Fob9V4edJKRKtIYiN5iwbb7BFw%2BtXkN7sNf9p5Z2rDE7oysac6CXXXu33r2J9DXdEGSQmozx%2BPuRLa76%2Fhs4zc9YZw%2FJSCUL6OePzDEkojVBjqkAa5eR7Nyqvehu7F2FHi32jmqPMNBopAwmRx6UpHZkvkE6FxyazaIdDH1%2F%2BBhlAmv4VHsRnjQTicU6BHPKX1ZlW%2BeGGGl0jw%2FcvhQ9%2FNnhXHLqJHOuvyz%2FqK43vjMTEREUG41DEBZbMalDK6e4IaEVqWB4B6U7jcKLaLVy1fv3kdpEqqcsBLGlBM2bag5zIw5DOUclak%2FzWbFYsGtD%2F3FJ%2FMxXryz&X-Amz-Signature=cb67f25c04da62c670bf3a33271edc575c081409d2b55d38268fe713401a38b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PLWIZMW%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020330Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSxYKWEvMNpGvx7N5n%2BcMV05jJAjmWN16AZvA9QNgL5gIhALKOPVQnuppTU0ARSEMgeeMiLoneTcSBDznaKtCYcbPXKv8DCHsQABoMNjM3NDIzMTgzODA1IgxgFMlOBQlsGOk8b8Yq3APfo7E53WMaSszrLm6Pmx1Ka5CK0%2Bs2cDcUMsRNQKctV5AnM6lQey2ePdmt807OHaeSoEhXSHYeAxdK5R3mV1YH%2FQvwZepTqOsk0PuMfUQs7tCm4kNHUWw%2B3sIlaPyqx8rfNnpe2FYzojwQ5v2%2Fmee8Lk8FBlWp9cGK9hYDt9BarVOsZUXspzrA16%2BNdBeAkGDlsV%2BVd%2FHcrLS46sOAnr%2F1aM9iCpRwU7BX6P%2Fpps00iy09j2o3OdOOLzLlwDttAaM4inhjLDqgFSoP5Qh6zoLS5QGb%2BBZ9NKTK2ReTxKzOZxM%2BSy2WUsyHUGwe6r%2BeBbabWN1ShDhB0u2BWnO30dx72Zgoy4nZMqdMaSqqL09M%2FLKAEnsr5HjeFsPxtpH%2FLL0bjaqH%2FgbAcEXxqAXD2XfsAaaZYxl2OTMIpYCkhNk352sj99pS7Yk2gjDynH%2BDtxIRiDbJmQvNBMW1%2BBKhszw353oScHPk7JqRVKmuAEv3MHOR1SkPgJDUf4LsNNfSjCCrxUUm%2F6GeUiAzFOzWN%2FGNs%2Fob9V4edJKRKtIYiN5iwbb7BFw%2BtXkN7sNf9p5Z2rDE7oysac6CXXXu33r2J9DXdEGSQmozx%2BPuRLa76%2Fhs4zc9YZw%2FJSCUL6OePzDEkojVBjqkAa5eR7Nyqvehu7F2FHi32jmqPMNBopAwmRx6UpHZkvkE6FxyazaIdDH1%2F%2BBhlAmv4VHsRnjQTicU6BHPKX1ZlW%2BeGGGl0jw%2FcvhQ9%2FNnhXHLqJHOuvyz%2FqK43vjMTEREUG41DEBZbMalDK6e4IaEVqWB4B6U7jcKLaLVy1fv3kdpEqqcsBLGlBM2bag5zIw5DOUclak%2FzWbFYsGtD%2F3FJ%2FMxXryz&X-Amz-Signature=ea06d327b24e21db62929eba43e36673b063e446019c78d80f38c62eb65ca4ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PLWIZMW%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020330Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSxYKWEvMNpGvx7N5n%2BcMV05jJAjmWN16AZvA9QNgL5gIhALKOPVQnuppTU0ARSEMgeeMiLoneTcSBDznaKtCYcbPXKv8DCHsQABoMNjM3NDIzMTgzODA1IgxgFMlOBQlsGOk8b8Yq3APfo7E53WMaSszrLm6Pmx1Ka5CK0%2Bs2cDcUMsRNQKctV5AnM6lQey2ePdmt807OHaeSoEhXSHYeAxdK5R3mV1YH%2FQvwZepTqOsk0PuMfUQs7tCm4kNHUWw%2B3sIlaPyqx8rfNnpe2FYzojwQ5v2%2Fmee8Lk8FBlWp9cGK9hYDt9BarVOsZUXspzrA16%2BNdBeAkGDlsV%2BVd%2FHcrLS46sOAnr%2F1aM9iCpRwU7BX6P%2Fpps00iy09j2o3OdOOLzLlwDttAaM4inhjLDqgFSoP5Qh6zoLS5QGb%2BBZ9NKTK2ReTxKzOZxM%2BSy2WUsyHUGwe6r%2BeBbabWN1ShDhB0u2BWnO30dx72Zgoy4nZMqdMaSqqL09M%2FLKAEnsr5HjeFsPxtpH%2FLL0bjaqH%2FgbAcEXxqAXD2XfsAaaZYxl2OTMIpYCkhNk352sj99pS7Yk2gjDynH%2BDtxIRiDbJmQvNBMW1%2BBKhszw353oScHPk7JqRVKmuAEv3MHOR1SkPgJDUf4LsNNfSjCCrxUUm%2F6GeUiAzFOzWN%2FGNs%2Fob9V4edJKRKtIYiN5iwbb7BFw%2BtXkN7sNf9p5Z2rDE7oysac6CXXXu33r2J9DXdEGSQmozx%2BPuRLa76%2Fhs4zc9YZw%2FJSCUL6OePzDEkojVBjqkAa5eR7Nyqvehu7F2FHi32jmqPMNBopAwmRx6UpHZkvkE6FxyazaIdDH1%2F%2BBhlAmv4VHsRnjQTicU6BHPKX1ZlW%2BeGGGl0jw%2FcvhQ9%2FNnhXHLqJHOuvyz%2FqK43vjMTEREUG41DEBZbMalDK6e4IaEVqWB4B6U7jcKLaLVy1fv3kdpEqqcsBLGlBM2bag5zIw5DOUclak%2FzWbFYsGtD%2F3FJ%2FMxXryz&X-Amz-Signature=e4f35003337d276f46a4e4c723e10da09f12fe698a046169ae8606bd94ce7e33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PLWIZMW%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020330Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSxYKWEvMNpGvx7N5n%2BcMV05jJAjmWN16AZvA9QNgL5gIhALKOPVQnuppTU0ARSEMgeeMiLoneTcSBDznaKtCYcbPXKv8DCHsQABoMNjM3NDIzMTgzODA1IgxgFMlOBQlsGOk8b8Yq3APfo7E53WMaSszrLm6Pmx1Ka5CK0%2Bs2cDcUMsRNQKctV5AnM6lQey2ePdmt807OHaeSoEhXSHYeAxdK5R3mV1YH%2FQvwZepTqOsk0PuMfUQs7tCm4kNHUWw%2B3sIlaPyqx8rfNnpe2FYzojwQ5v2%2Fmee8Lk8FBlWp9cGK9hYDt9BarVOsZUXspzrA16%2BNdBeAkGDlsV%2BVd%2FHcrLS46sOAnr%2F1aM9iCpRwU7BX6P%2Fpps00iy09j2o3OdOOLzLlwDttAaM4inhjLDqgFSoP5Qh6zoLS5QGb%2BBZ9NKTK2ReTxKzOZxM%2BSy2WUsyHUGwe6r%2BeBbabWN1ShDhB0u2BWnO30dx72Zgoy4nZMqdMaSqqL09M%2FLKAEnsr5HjeFsPxtpH%2FLL0bjaqH%2FgbAcEXxqAXD2XfsAaaZYxl2OTMIpYCkhNk352sj99pS7Yk2gjDynH%2BDtxIRiDbJmQvNBMW1%2BBKhszw353oScHPk7JqRVKmuAEv3MHOR1SkPgJDUf4LsNNfSjCCrxUUm%2F6GeUiAzFOzWN%2FGNs%2Fob9V4edJKRKtIYiN5iwbb7BFw%2BtXkN7sNf9p5Z2rDE7oysac6CXXXu33r2J9DXdEGSQmozx%2BPuRLa76%2Fhs4zc9YZw%2FJSCUL6OePzDEkojVBjqkAa5eR7Nyqvehu7F2FHi32jmqPMNBopAwmRx6UpHZkvkE6FxyazaIdDH1%2F%2BBhlAmv4VHsRnjQTicU6BHPKX1ZlW%2BeGGGl0jw%2FcvhQ9%2FNnhXHLqJHOuvyz%2FqK43vjMTEREUG41DEBZbMalDK6e4IaEVqWB4B6U7jcKLaLVy1fv3kdpEqqcsBLGlBM2bag5zIw5DOUclak%2FzWbFYsGtD%2F3FJ%2FMxXryz&X-Amz-Signature=9539b7da9a071bdab0192ce1a8d52366baf0c811d681e40c0a5ca5efc288de3e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PLWIZMW%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020330Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSxYKWEvMNpGvx7N5n%2BcMV05jJAjmWN16AZvA9QNgL5gIhALKOPVQnuppTU0ARSEMgeeMiLoneTcSBDznaKtCYcbPXKv8DCHsQABoMNjM3NDIzMTgzODA1IgxgFMlOBQlsGOk8b8Yq3APfo7E53WMaSszrLm6Pmx1Ka5CK0%2Bs2cDcUMsRNQKctV5AnM6lQey2ePdmt807OHaeSoEhXSHYeAxdK5R3mV1YH%2FQvwZepTqOsk0PuMfUQs7tCm4kNHUWw%2B3sIlaPyqx8rfNnpe2FYzojwQ5v2%2Fmee8Lk8FBlWp9cGK9hYDt9BarVOsZUXspzrA16%2BNdBeAkGDlsV%2BVd%2FHcrLS46sOAnr%2F1aM9iCpRwU7BX6P%2Fpps00iy09j2o3OdOOLzLlwDttAaM4inhjLDqgFSoP5Qh6zoLS5QGb%2BBZ9NKTK2ReTxKzOZxM%2BSy2WUsyHUGwe6r%2BeBbabWN1ShDhB0u2BWnO30dx72Zgoy4nZMqdMaSqqL09M%2FLKAEnsr5HjeFsPxtpH%2FLL0bjaqH%2FgbAcEXxqAXD2XfsAaaZYxl2OTMIpYCkhNk352sj99pS7Yk2gjDynH%2BDtxIRiDbJmQvNBMW1%2BBKhszw353oScHPk7JqRVKmuAEv3MHOR1SkPgJDUf4LsNNfSjCCrxUUm%2F6GeUiAzFOzWN%2FGNs%2Fob9V4edJKRKtIYiN5iwbb7BFw%2BtXkN7sNf9p5Z2rDE7oysac6CXXXu33r2J9DXdEGSQmozx%2BPuRLa76%2Fhs4zc9YZw%2FJSCUL6OePzDEkojVBjqkAa5eR7Nyqvehu7F2FHi32jmqPMNBopAwmRx6UpHZkvkE6FxyazaIdDH1%2F%2BBhlAmv4VHsRnjQTicU6BHPKX1ZlW%2BeGGGl0jw%2FcvhQ9%2FNnhXHLqJHOuvyz%2FqK43vjMTEREUG41DEBZbMalDK6e4IaEVqWB4B6U7jcKLaLVy1fv3kdpEqqcsBLGlBM2bag5zIw5DOUclak%2FzWbFYsGtD%2F3FJ%2FMxXryz&X-Amz-Signature=fbd478fde73e2d3528a405f5276e3366a80f82d5d312b98761f99220b706e1c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PLWIZMW%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020330Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSxYKWEvMNpGvx7N5n%2BcMV05jJAjmWN16AZvA9QNgL5gIhALKOPVQnuppTU0ARSEMgeeMiLoneTcSBDznaKtCYcbPXKv8DCHsQABoMNjM3NDIzMTgzODA1IgxgFMlOBQlsGOk8b8Yq3APfo7E53WMaSszrLm6Pmx1Ka5CK0%2Bs2cDcUMsRNQKctV5AnM6lQey2ePdmt807OHaeSoEhXSHYeAxdK5R3mV1YH%2FQvwZepTqOsk0PuMfUQs7tCm4kNHUWw%2B3sIlaPyqx8rfNnpe2FYzojwQ5v2%2Fmee8Lk8FBlWp9cGK9hYDt9BarVOsZUXspzrA16%2BNdBeAkGDlsV%2BVd%2FHcrLS46sOAnr%2F1aM9iCpRwU7BX6P%2Fpps00iy09j2o3OdOOLzLlwDttAaM4inhjLDqgFSoP5Qh6zoLS5QGb%2BBZ9NKTK2ReTxKzOZxM%2BSy2WUsyHUGwe6r%2BeBbabWN1ShDhB0u2BWnO30dx72Zgoy4nZMqdMaSqqL09M%2FLKAEnsr5HjeFsPxtpH%2FLL0bjaqH%2FgbAcEXxqAXD2XfsAaaZYxl2OTMIpYCkhNk352sj99pS7Yk2gjDynH%2BDtxIRiDbJmQvNBMW1%2BBKhszw353oScHPk7JqRVKmuAEv3MHOR1SkPgJDUf4LsNNfSjCCrxUUm%2F6GeUiAzFOzWN%2FGNs%2Fob9V4edJKRKtIYiN5iwbb7BFw%2BtXkN7sNf9p5Z2rDE7oysac6CXXXu33r2J9DXdEGSQmozx%2BPuRLa76%2Fhs4zc9YZw%2FJSCUL6OePzDEkojVBjqkAa5eR7Nyqvehu7F2FHi32jmqPMNBopAwmRx6UpHZkvkE6FxyazaIdDH1%2F%2BBhlAmv4VHsRnjQTicU6BHPKX1ZlW%2BeGGGl0jw%2FcvhQ9%2FNnhXHLqJHOuvyz%2FqK43vjMTEREUG41DEBZbMalDK6e4IaEVqWB4B6U7jcKLaLVy1fv3kdpEqqcsBLGlBM2bag5zIw5DOUclak%2FzWbFYsGtD%2F3FJ%2FMxXryz&X-Amz-Signature=7b114e6958133d28f39bc7b5441e1c1931eda0d604b818413e7eda99059d56f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
