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
fetched_at: '2026-11-10T05:10:56.302Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X52SEILK%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCdPUcluF0SN6zCEcV%2FzCe7EKx087MHmPX8q9jJSEmseQIgUgeAweXObKZKFJAjaFqHJ6EEHk4VpN7t8cHhcY9DRQgq%2FwMIMBAAGgw2Mzc0MjMxODM4MDUiDJ5fr%2Fc1CNHMO9Vz6ircAz%2FlTiymLnDo1isRNKpwVRvIN6fKejV0tkVyct8SD68RXZzpsTFl8mK1uaRW83zIvM8sUph1mRksVDWn6VV7pP8g0MXVfAYWQzzcfMfMibvSuUrMPKJBMmhuTo%2FyoKRMOmxdTG%2B3p33VcrvOHNJKuEh0vuWKuj2ykoUtmx92Q32F7Exb2AP8DsK57VJZQV3D4PgLP0ZJsbt5Y%2FTTtjRJUcONuNioA5kR91gNsfZE0aEk%2BOWYCsqrovfVIwoI5l2Sloss%2FivmKfKQq061P0zxE1eHmj4s7Jo7uDQZuiLxXnDHrmW2ifoKRfKIYlkHh6naErY%2FQif1VnHkGv92vrPgRErgQ8459H3Cs6DlwG43eBzqticWiGeORAUGeoBixRN%2B7v%2BA%2Fhxs%2Frab0WFLv9w7a4rBZ98ILOMWBoNChxxapUeSmhdFeK5OxM4xYR%2B62UBk5izcKU0ZzXMbr7YxKcieF9AWmHIyD1410DYMisoRUzHAwwgBzvtES0Fpc5SVe%2BHfIU4Fl7spfCFD6fxcVXrH3ibCQS7rDhFyX3gmd60w5gEYEMnz8QuIE%2BGmiW12bTcblypNWP07mmRx007OLHvUJeM8QX8ujsX5yGD6RroTPij9JBDTOWASOyd67S84MKK7ydcGOqUBb8h52%2FwQS2glzuKSfotCEkbk2RbpC3LNByo7yqfSpwCCJUlyVZHVXQgf83KvH8w3W93FGCJdSeiRLeQNP65HO%2B4OlG5p8YGlf6wO8tuVYKSNxB8%2FjSPHJbQHl3Hmut46ZsQ2MnAgPQFROdXIemXwyaHyWLnSzVVgDYs32%2F86MGrtF6TTX60cSRDlRdIp6qMLe%2FzQ003vfBheo4FyirfmE4raEKgd&X-Amz-Signature=25f8971dd761c83475fb4000412a3b779c99995850e581b6b317408f4887e1f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X52SEILK%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCdPUcluF0SN6zCEcV%2FzCe7EKx087MHmPX8q9jJSEmseQIgUgeAweXObKZKFJAjaFqHJ6EEHk4VpN7t8cHhcY9DRQgq%2FwMIMBAAGgw2Mzc0MjMxODM4MDUiDJ5fr%2Fc1CNHMO9Vz6ircAz%2FlTiymLnDo1isRNKpwVRvIN6fKejV0tkVyct8SD68RXZzpsTFl8mK1uaRW83zIvM8sUph1mRksVDWn6VV7pP8g0MXVfAYWQzzcfMfMibvSuUrMPKJBMmhuTo%2FyoKRMOmxdTG%2B3p33VcrvOHNJKuEh0vuWKuj2ykoUtmx92Q32F7Exb2AP8DsK57VJZQV3D4PgLP0ZJsbt5Y%2FTTtjRJUcONuNioA5kR91gNsfZE0aEk%2BOWYCsqrovfVIwoI5l2Sloss%2FivmKfKQq061P0zxE1eHmj4s7Jo7uDQZuiLxXnDHrmW2ifoKRfKIYlkHh6naErY%2FQif1VnHkGv92vrPgRErgQ8459H3Cs6DlwG43eBzqticWiGeORAUGeoBixRN%2B7v%2BA%2Fhxs%2Frab0WFLv9w7a4rBZ98ILOMWBoNChxxapUeSmhdFeK5OxM4xYR%2B62UBk5izcKU0ZzXMbr7YxKcieF9AWmHIyD1410DYMisoRUzHAwwgBzvtES0Fpc5SVe%2BHfIU4Fl7spfCFD6fxcVXrH3ibCQS7rDhFyX3gmd60w5gEYEMnz8QuIE%2BGmiW12bTcblypNWP07mmRx007OLHvUJeM8QX8ujsX5yGD6RroTPij9JBDTOWASOyd67S84MKK7ydcGOqUBb8h52%2FwQS2glzuKSfotCEkbk2RbpC3LNByo7yqfSpwCCJUlyVZHVXQgf83KvH8w3W93FGCJdSeiRLeQNP65HO%2B4OlG5p8YGlf6wO8tuVYKSNxB8%2FjSPHJbQHl3Hmut46ZsQ2MnAgPQFROdXIemXwyaHyWLnSzVVgDYs32%2F86MGrtF6TTX60cSRDlRdIp6qMLe%2FzQ003vfBheo4FyirfmE4raEKgd&X-Amz-Signature=393569466c983da2644e0266ba06426ff98468e515f1d789c2ec89afc89f46b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X52SEILK%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCdPUcluF0SN6zCEcV%2FzCe7EKx087MHmPX8q9jJSEmseQIgUgeAweXObKZKFJAjaFqHJ6EEHk4VpN7t8cHhcY9DRQgq%2FwMIMBAAGgw2Mzc0MjMxODM4MDUiDJ5fr%2Fc1CNHMO9Vz6ircAz%2FlTiymLnDo1isRNKpwVRvIN6fKejV0tkVyct8SD68RXZzpsTFl8mK1uaRW83zIvM8sUph1mRksVDWn6VV7pP8g0MXVfAYWQzzcfMfMibvSuUrMPKJBMmhuTo%2FyoKRMOmxdTG%2B3p33VcrvOHNJKuEh0vuWKuj2ykoUtmx92Q32F7Exb2AP8DsK57VJZQV3D4PgLP0ZJsbt5Y%2FTTtjRJUcONuNioA5kR91gNsfZE0aEk%2BOWYCsqrovfVIwoI5l2Sloss%2FivmKfKQq061P0zxE1eHmj4s7Jo7uDQZuiLxXnDHrmW2ifoKRfKIYlkHh6naErY%2FQif1VnHkGv92vrPgRErgQ8459H3Cs6DlwG43eBzqticWiGeORAUGeoBixRN%2B7v%2BA%2Fhxs%2Frab0WFLv9w7a4rBZ98ILOMWBoNChxxapUeSmhdFeK5OxM4xYR%2B62UBk5izcKU0ZzXMbr7YxKcieF9AWmHIyD1410DYMisoRUzHAwwgBzvtES0Fpc5SVe%2BHfIU4Fl7spfCFD6fxcVXrH3ibCQS7rDhFyX3gmd60w5gEYEMnz8QuIE%2BGmiW12bTcblypNWP07mmRx007OLHvUJeM8QX8ujsX5yGD6RroTPij9JBDTOWASOyd67S84MKK7ydcGOqUBb8h52%2FwQS2glzuKSfotCEkbk2RbpC3LNByo7yqfSpwCCJUlyVZHVXQgf83KvH8w3W93FGCJdSeiRLeQNP65HO%2B4OlG5p8YGlf6wO8tuVYKSNxB8%2FjSPHJbQHl3Hmut46ZsQ2MnAgPQFROdXIemXwyaHyWLnSzVVgDYs32%2F86MGrtF6TTX60cSRDlRdIp6qMLe%2FzQ003vfBheo4FyirfmE4raEKgd&X-Amz-Signature=737579a44ed20ff8b58a4112f4e5662a3f0da6557fa412296bcd5578ff1c766b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X52SEILK%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCdPUcluF0SN6zCEcV%2FzCe7EKx087MHmPX8q9jJSEmseQIgUgeAweXObKZKFJAjaFqHJ6EEHk4VpN7t8cHhcY9DRQgq%2FwMIMBAAGgw2Mzc0MjMxODM4MDUiDJ5fr%2Fc1CNHMO9Vz6ircAz%2FlTiymLnDo1isRNKpwVRvIN6fKejV0tkVyct8SD68RXZzpsTFl8mK1uaRW83zIvM8sUph1mRksVDWn6VV7pP8g0MXVfAYWQzzcfMfMibvSuUrMPKJBMmhuTo%2FyoKRMOmxdTG%2B3p33VcrvOHNJKuEh0vuWKuj2ykoUtmx92Q32F7Exb2AP8DsK57VJZQV3D4PgLP0ZJsbt5Y%2FTTtjRJUcONuNioA5kR91gNsfZE0aEk%2BOWYCsqrovfVIwoI5l2Sloss%2FivmKfKQq061P0zxE1eHmj4s7Jo7uDQZuiLxXnDHrmW2ifoKRfKIYlkHh6naErY%2FQif1VnHkGv92vrPgRErgQ8459H3Cs6DlwG43eBzqticWiGeORAUGeoBixRN%2B7v%2BA%2Fhxs%2Frab0WFLv9w7a4rBZ98ILOMWBoNChxxapUeSmhdFeK5OxM4xYR%2B62UBk5izcKU0ZzXMbr7YxKcieF9AWmHIyD1410DYMisoRUzHAwwgBzvtES0Fpc5SVe%2BHfIU4Fl7spfCFD6fxcVXrH3ibCQS7rDhFyX3gmd60w5gEYEMnz8QuIE%2BGmiW12bTcblypNWP07mmRx007OLHvUJeM8QX8ujsX5yGD6RroTPij9JBDTOWASOyd67S84MKK7ydcGOqUBb8h52%2FwQS2glzuKSfotCEkbk2RbpC3LNByo7yqfSpwCCJUlyVZHVXQgf83KvH8w3W93FGCJdSeiRLeQNP65HO%2B4OlG5p8YGlf6wO8tuVYKSNxB8%2FjSPHJbQHl3Hmut46ZsQ2MnAgPQFROdXIemXwyaHyWLnSzVVgDYs32%2F86MGrtF6TTX60cSRDlRdIp6qMLe%2FzQ003vfBheo4FyirfmE4raEKgd&X-Amz-Signature=ab705cbcff892582104d6d01e428cb4ea8327fc0142b922d568a6ef33cbc4d1e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X52SEILK%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCdPUcluF0SN6zCEcV%2FzCe7EKx087MHmPX8q9jJSEmseQIgUgeAweXObKZKFJAjaFqHJ6EEHk4VpN7t8cHhcY9DRQgq%2FwMIMBAAGgw2Mzc0MjMxODM4MDUiDJ5fr%2Fc1CNHMO9Vz6ircAz%2FlTiymLnDo1isRNKpwVRvIN6fKejV0tkVyct8SD68RXZzpsTFl8mK1uaRW83zIvM8sUph1mRksVDWn6VV7pP8g0MXVfAYWQzzcfMfMibvSuUrMPKJBMmhuTo%2FyoKRMOmxdTG%2B3p33VcrvOHNJKuEh0vuWKuj2ykoUtmx92Q32F7Exb2AP8DsK57VJZQV3D4PgLP0ZJsbt5Y%2FTTtjRJUcONuNioA5kR91gNsfZE0aEk%2BOWYCsqrovfVIwoI5l2Sloss%2FivmKfKQq061P0zxE1eHmj4s7Jo7uDQZuiLxXnDHrmW2ifoKRfKIYlkHh6naErY%2FQif1VnHkGv92vrPgRErgQ8459H3Cs6DlwG43eBzqticWiGeORAUGeoBixRN%2B7v%2BA%2Fhxs%2Frab0WFLv9w7a4rBZ98ILOMWBoNChxxapUeSmhdFeK5OxM4xYR%2B62UBk5izcKU0ZzXMbr7YxKcieF9AWmHIyD1410DYMisoRUzHAwwgBzvtES0Fpc5SVe%2BHfIU4Fl7spfCFD6fxcVXrH3ibCQS7rDhFyX3gmd60w5gEYEMnz8QuIE%2BGmiW12bTcblypNWP07mmRx007OLHvUJeM8QX8ujsX5yGD6RroTPij9JBDTOWASOyd67S84MKK7ydcGOqUBb8h52%2FwQS2glzuKSfotCEkbk2RbpC3LNByo7yqfSpwCCJUlyVZHVXQgf83KvH8w3W93FGCJdSeiRLeQNP65HO%2B4OlG5p8YGlf6wO8tuVYKSNxB8%2FjSPHJbQHl3Hmut46ZsQ2MnAgPQFROdXIemXwyaHyWLnSzVVgDYs32%2F86MGrtF6TTX60cSRDlRdIp6qMLe%2FzQ003vfBheo4FyirfmE4raEKgd&X-Amz-Signature=92b5a2c6a176a01a0f90eaaa59a406c98fc2a402a94f248af589bddfecedd956&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X52SEILK%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCdPUcluF0SN6zCEcV%2FzCe7EKx087MHmPX8q9jJSEmseQIgUgeAweXObKZKFJAjaFqHJ6EEHk4VpN7t8cHhcY9DRQgq%2FwMIMBAAGgw2Mzc0MjMxODM4MDUiDJ5fr%2Fc1CNHMO9Vz6ircAz%2FlTiymLnDo1isRNKpwVRvIN6fKejV0tkVyct8SD68RXZzpsTFl8mK1uaRW83zIvM8sUph1mRksVDWn6VV7pP8g0MXVfAYWQzzcfMfMibvSuUrMPKJBMmhuTo%2FyoKRMOmxdTG%2B3p33VcrvOHNJKuEh0vuWKuj2ykoUtmx92Q32F7Exb2AP8DsK57VJZQV3D4PgLP0ZJsbt5Y%2FTTtjRJUcONuNioA5kR91gNsfZE0aEk%2BOWYCsqrovfVIwoI5l2Sloss%2FivmKfKQq061P0zxE1eHmj4s7Jo7uDQZuiLxXnDHrmW2ifoKRfKIYlkHh6naErY%2FQif1VnHkGv92vrPgRErgQ8459H3Cs6DlwG43eBzqticWiGeORAUGeoBixRN%2B7v%2BA%2Fhxs%2Frab0WFLv9w7a4rBZ98ILOMWBoNChxxapUeSmhdFeK5OxM4xYR%2B62UBk5izcKU0ZzXMbr7YxKcieF9AWmHIyD1410DYMisoRUzHAwwgBzvtES0Fpc5SVe%2BHfIU4Fl7spfCFD6fxcVXrH3ibCQS7rDhFyX3gmd60w5gEYEMnz8QuIE%2BGmiW12bTcblypNWP07mmRx007OLHvUJeM8QX8ujsX5yGD6RroTPij9JBDTOWASOyd67S84MKK7ydcGOqUBb8h52%2FwQS2glzuKSfotCEkbk2RbpC3LNByo7yqfSpwCCJUlyVZHVXQgf83KvH8w3W93FGCJdSeiRLeQNP65HO%2B4OlG5p8YGlf6wO8tuVYKSNxB8%2FjSPHJbQHl3Hmut46ZsQ2MnAgPQFROdXIemXwyaHyWLnSzVVgDYs32%2F86MGrtF6TTX60cSRDlRdIp6qMLe%2FzQ003vfBheo4FyirfmE4raEKgd&X-Amz-Signature=1c1a39c9aa715a9eb5f11b4ba5e89519adc36d73887f2a622d228a800145f52a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X52SEILK%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCdPUcluF0SN6zCEcV%2FzCe7EKx087MHmPX8q9jJSEmseQIgUgeAweXObKZKFJAjaFqHJ6EEHk4VpN7t8cHhcY9DRQgq%2FwMIMBAAGgw2Mzc0MjMxODM4MDUiDJ5fr%2Fc1CNHMO9Vz6ircAz%2FlTiymLnDo1isRNKpwVRvIN6fKejV0tkVyct8SD68RXZzpsTFl8mK1uaRW83zIvM8sUph1mRksVDWn6VV7pP8g0MXVfAYWQzzcfMfMibvSuUrMPKJBMmhuTo%2FyoKRMOmxdTG%2B3p33VcrvOHNJKuEh0vuWKuj2ykoUtmx92Q32F7Exb2AP8DsK57VJZQV3D4PgLP0ZJsbt5Y%2FTTtjRJUcONuNioA5kR91gNsfZE0aEk%2BOWYCsqrovfVIwoI5l2Sloss%2FivmKfKQq061P0zxE1eHmj4s7Jo7uDQZuiLxXnDHrmW2ifoKRfKIYlkHh6naErY%2FQif1VnHkGv92vrPgRErgQ8459H3Cs6DlwG43eBzqticWiGeORAUGeoBixRN%2B7v%2BA%2Fhxs%2Frab0WFLv9w7a4rBZ98ILOMWBoNChxxapUeSmhdFeK5OxM4xYR%2B62UBk5izcKU0ZzXMbr7YxKcieF9AWmHIyD1410DYMisoRUzHAwwgBzvtES0Fpc5SVe%2BHfIU4Fl7spfCFD6fxcVXrH3ibCQS7rDhFyX3gmd60w5gEYEMnz8QuIE%2BGmiW12bTcblypNWP07mmRx007OLHvUJeM8QX8ujsX5yGD6RroTPij9JBDTOWASOyd67S84MKK7ydcGOqUBb8h52%2FwQS2glzuKSfotCEkbk2RbpC3LNByo7yqfSpwCCJUlyVZHVXQgf83KvH8w3W93FGCJdSeiRLeQNP65HO%2B4OlG5p8YGlf6wO8tuVYKSNxB8%2FjSPHJbQHl3Hmut46ZsQ2MnAgPQFROdXIemXwyaHyWLnSzVVgDYs32%2F86MGrtF6TTX60cSRDlRdIp6qMLe%2FzQ003vfBheo4FyirfmE4raEKgd&X-Amz-Signature=fa8ea2c079a9c81a3349f433965911fa1351ba9ae4de9a97db6ce42490f711c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X52SEILK%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCdPUcluF0SN6zCEcV%2FzCe7EKx087MHmPX8q9jJSEmseQIgUgeAweXObKZKFJAjaFqHJ6EEHk4VpN7t8cHhcY9DRQgq%2FwMIMBAAGgw2Mzc0MjMxODM4MDUiDJ5fr%2Fc1CNHMO9Vz6ircAz%2FlTiymLnDo1isRNKpwVRvIN6fKejV0tkVyct8SD68RXZzpsTFl8mK1uaRW83zIvM8sUph1mRksVDWn6VV7pP8g0MXVfAYWQzzcfMfMibvSuUrMPKJBMmhuTo%2FyoKRMOmxdTG%2B3p33VcrvOHNJKuEh0vuWKuj2ykoUtmx92Q32F7Exb2AP8DsK57VJZQV3D4PgLP0ZJsbt5Y%2FTTtjRJUcONuNioA5kR91gNsfZE0aEk%2BOWYCsqrovfVIwoI5l2Sloss%2FivmKfKQq061P0zxE1eHmj4s7Jo7uDQZuiLxXnDHrmW2ifoKRfKIYlkHh6naErY%2FQif1VnHkGv92vrPgRErgQ8459H3Cs6DlwG43eBzqticWiGeORAUGeoBixRN%2B7v%2BA%2Fhxs%2Frab0WFLv9w7a4rBZ98ILOMWBoNChxxapUeSmhdFeK5OxM4xYR%2B62UBk5izcKU0ZzXMbr7YxKcieF9AWmHIyD1410DYMisoRUzHAwwgBzvtES0Fpc5SVe%2BHfIU4Fl7spfCFD6fxcVXrH3ibCQS7rDhFyX3gmd60w5gEYEMnz8QuIE%2BGmiW12bTcblypNWP07mmRx007OLHvUJeM8QX8ujsX5yGD6RroTPij9JBDTOWASOyd67S84MKK7ydcGOqUBb8h52%2FwQS2glzuKSfotCEkbk2RbpC3LNByo7yqfSpwCCJUlyVZHVXQgf83KvH8w3W93FGCJdSeiRLeQNP65HO%2B4OlG5p8YGlf6wO8tuVYKSNxB8%2FjSPHJbQHl3Hmut46ZsQ2MnAgPQFROdXIemXwyaHyWLnSzVVgDYs32%2F86MGrtF6TTX60cSRDlRdIp6qMLe%2FzQ003vfBheo4FyirfmE4raEKgd&X-Amz-Signature=e42bab5b6e94ac59431f245116dde2e7c260eee480724931f0ba8027fc1635af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X52SEILK%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCdPUcluF0SN6zCEcV%2FzCe7EKx087MHmPX8q9jJSEmseQIgUgeAweXObKZKFJAjaFqHJ6EEHk4VpN7t8cHhcY9DRQgq%2FwMIMBAAGgw2Mzc0MjMxODM4MDUiDJ5fr%2Fc1CNHMO9Vz6ircAz%2FlTiymLnDo1isRNKpwVRvIN6fKejV0tkVyct8SD68RXZzpsTFl8mK1uaRW83zIvM8sUph1mRksVDWn6VV7pP8g0MXVfAYWQzzcfMfMibvSuUrMPKJBMmhuTo%2FyoKRMOmxdTG%2B3p33VcrvOHNJKuEh0vuWKuj2ykoUtmx92Q32F7Exb2AP8DsK57VJZQV3D4PgLP0ZJsbt5Y%2FTTtjRJUcONuNioA5kR91gNsfZE0aEk%2BOWYCsqrovfVIwoI5l2Sloss%2FivmKfKQq061P0zxE1eHmj4s7Jo7uDQZuiLxXnDHrmW2ifoKRfKIYlkHh6naErY%2FQif1VnHkGv92vrPgRErgQ8459H3Cs6DlwG43eBzqticWiGeORAUGeoBixRN%2B7v%2BA%2Fhxs%2Frab0WFLv9w7a4rBZ98ILOMWBoNChxxapUeSmhdFeK5OxM4xYR%2B62UBk5izcKU0ZzXMbr7YxKcieF9AWmHIyD1410DYMisoRUzHAwwgBzvtES0Fpc5SVe%2BHfIU4Fl7spfCFD6fxcVXrH3ibCQS7rDhFyX3gmd60w5gEYEMnz8QuIE%2BGmiW12bTcblypNWP07mmRx007OLHvUJeM8QX8ujsX5yGD6RroTPij9JBDTOWASOyd67S84MKK7ydcGOqUBb8h52%2FwQS2glzuKSfotCEkbk2RbpC3LNByo7yqfSpwCCJUlyVZHVXQgf83KvH8w3W93FGCJdSeiRLeQNP65HO%2B4OlG5p8YGlf6wO8tuVYKSNxB8%2FjSPHJbQHl3Hmut46ZsQ2MnAgPQFROdXIemXwyaHyWLnSzVVgDYs32%2F86MGrtF6TTX60cSRDlRdIp6qMLe%2FzQ003vfBheo4FyirfmE4raEKgd&X-Amz-Signature=750c526f452d2cf459139a7da1249d03127aded327443cbc197f45498ba4796d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
