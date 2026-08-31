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
fetched_at: '2026-08-31T02:17:40.406Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667YW46HGS%2F20260831%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260831T021736Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDiXfY2YtPySFBQjn8CsYSZQPvevYTgQ3PPLslKgHVoLgIgN4eQ3kSEJJZwfywqduqHCq5oiZ%2By9L7kwDsRuOAkb6oqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBBokJ7llCd8If%2B7hircA9JiHl7TXYB4wd6it%2FIcf8%2Bpxti5dp7ICSdg%2FMCo9uvNVjyFWUEQFbRHADmQZh%2BIMSF7KU3VW1fi8xYLtKKu7%2F7e2DePqzbDOjKHBISq0fYiLz3m1CaNEFONSBXpUkY4k%2BEb1mr%2FLXKXHPH9k3O2vtnIMu8nmTQTw%2F3Z2E59H%2FRKgAygevpnJ5zavAOLKhJZMaTbQTIWvcuQ0FqM4i5lqMx5jQQQIxMXFj0mMICebAMbbuF%2BQOEyK6nyrpZbCcJYMHS2aU7WO%2Bx4AVnEpxlA8mYBSVBVbc0TKsD8hWSAQFZqDBNHwwNEb9N%2BBulr6ifktxeWsn4RrojkT9bFGGR6mrmOGNFv062rrwK4iJcbbaYRj%2BVFo0iFPxKVBAiMdizvd%2FwNSvWC0nEFHI9TwGTO8o4aMR78vjtnG5QR%2BgTHZennmAK8T41wZYsSryxZYALhOeEmT3rSraUq9AUwcEVrVEOTO3tRKYx24q7N1w1bmvrozwrJHSOqzJMDl5WtTi6eW6u0R4c5HrKPKCYmtVtVcfmFuttgemwUR%2BodSfuaqGTaCHa2hPrKfBefzDI3zzG6Hq6MAgakKl2vXPd3XL6b2UxZaUzRtRuWX%2BpaJ5dyMPmY9biTOaBpkH9wq3BQMKCV09QGOqUBPP1oyMUn52duY4wxGg8nl4Dryr%2BoJLEqriiaB%2F3TJFyPh6nFONa3LtMRq%2BMJ2cwOaxmtgu0HU6%2BIeptDiLiRo0rzyMmP8Mj9nGSw4hz2aLB4VZyxVJl1qnGlc2%2FHECUPKlC6GqEhruDl7EzhOI%2FOLDqowP4SsDuEF0YEhw%2BtZTqjrHk%2BUzk9YSRdk5XNjZpN3bgYxUA27BeKFTVWkDduW6Imh1Hc&X-Amz-Signature=a77b92b846e3bb7351638de8e2676140c9e8cd8d86b7ac5acfbbed21e7c6b5be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667YW46HGS%2F20260831%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260831T021736Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDiXfY2YtPySFBQjn8CsYSZQPvevYTgQ3PPLslKgHVoLgIgN4eQ3kSEJJZwfywqduqHCq5oiZ%2By9L7kwDsRuOAkb6oqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBBokJ7llCd8If%2B7hircA9JiHl7TXYB4wd6it%2FIcf8%2Bpxti5dp7ICSdg%2FMCo9uvNVjyFWUEQFbRHADmQZh%2BIMSF7KU3VW1fi8xYLtKKu7%2F7e2DePqzbDOjKHBISq0fYiLz3m1CaNEFONSBXpUkY4k%2BEb1mr%2FLXKXHPH9k3O2vtnIMu8nmTQTw%2F3Z2E59H%2FRKgAygevpnJ5zavAOLKhJZMaTbQTIWvcuQ0FqM4i5lqMx5jQQQIxMXFj0mMICebAMbbuF%2BQOEyK6nyrpZbCcJYMHS2aU7WO%2Bx4AVnEpxlA8mYBSVBVbc0TKsD8hWSAQFZqDBNHwwNEb9N%2BBulr6ifktxeWsn4RrojkT9bFGGR6mrmOGNFv062rrwK4iJcbbaYRj%2BVFo0iFPxKVBAiMdizvd%2FwNSvWC0nEFHI9TwGTO8o4aMR78vjtnG5QR%2BgTHZennmAK8T41wZYsSryxZYALhOeEmT3rSraUq9AUwcEVrVEOTO3tRKYx24q7N1w1bmvrozwrJHSOqzJMDl5WtTi6eW6u0R4c5HrKPKCYmtVtVcfmFuttgemwUR%2BodSfuaqGTaCHa2hPrKfBefzDI3zzG6Hq6MAgakKl2vXPd3XL6b2UxZaUzRtRuWX%2BpaJ5dyMPmY9biTOaBpkH9wq3BQMKCV09QGOqUBPP1oyMUn52duY4wxGg8nl4Dryr%2BoJLEqriiaB%2F3TJFyPh6nFONa3LtMRq%2BMJ2cwOaxmtgu0HU6%2BIeptDiLiRo0rzyMmP8Mj9nGSw4hz2aLB4VZyxVJl1qnGlc2%2FHECUPKlC6GqEhruDl7EzhOI%2FOLDqowP4SsDuEF0YEhw%2BtZTqjrHk%2BUzk9YSRdk5XNjZpN3bgYxUA27BeKFTVWkDduW6Imh1Hc&X-Amz-Signature=32ec4c87d9c9a0eef7722a2ec21d33f186d1895b6f8a9e0e3cf4c39e93c06d43&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667YW46HGS%2F20260831%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260831T021736Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDiXfY2YtPySFBQjn8CsYSZQPvevYTgQ3PPLslKgHVoLgIgN4eQ3kSEJJZwfywqduqHCq5oiZ%2By9L7kwDsRuOAkb6oqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBBokJ7llCd8If%2B7hircA9JiHl7TXYB4wd6it%2FIcf8%2Bpxti5dp7ICSdg%2FMCo9uvNVjyFWUEQFbRHADmQZh%2BIMSF7KU3VW1fi8xYLtKKu7%2F7e2DePqzbDOjKHBISq0fYiLz3m1CaNEFONSBXpUkY4k%2BEb1mr%2FLXKXHPH9k3O2vtnIMu8nmTQTw%2F3Z2E59H%2FRKgAygevpnJ5zavAOLKhJZMaTbQTIWvcuQ0FqM4i5lqMx5jQQQIxMXFj0mMICebAMbbuF%2BQOEyK6nyrpZbCcJYMHS2aU7WO%2Bx4AVnEpxlA8mYBSVBVbc0TKsD8hWSAQFZqDBNHwwNEb9N%2BBulr6ifktxeWsn4RrojkT9bFGGR6mrmOGNFv062rrwK4iJcbbaYRj%2BVFo0iFPxKVBAiMdizvd%2FwNSvWC0nEFHI9TwGTO8o4aMR78vjtnG5QR%2BgTHZennmAK8T41wZYsSryxZYALhOeEmT3rSraUq9AUwcEVrVEOTO3tRKYx24q7N1w1bmvrozwrJHSOqzJMDl5WtTi6eW6u0R4c5HrKPKCYmtVtVcfmFuttgemwUR%2BodSfuaqGTaCHa2hPrKfBefzDI3zzG6Hq6MAgakKl2vXPd3XL6b2UxZaUzRtRuWX%2BpaJ5dyMPmY9biTOaBpkH9wq3BQMKCV09QGOqUBPP1oyMUn52duY4wxGg8nl4Dryr%2BoJLEqriiaB%2F3TJFyPh6nFONa3LtMRq%2BMJ2cwOaxmtgu0HU6%2BIeptDiLiRo0rzyMmP8Mj9nGSw4hz2aLB4VZyxVJl1qnGlc2%2FHECUPKlC6GqEhruDl7EzhOI%2FOLDqowP4SsDuEF0YEhw%2BtZTqjrHk%2BUzk9YSRdk5XNjZpN3bgYxUA27BeKFTVWkDduW6Imh1Hc&X-Amz-Signature=85444edd97042ed8bc2411176727393a7795f70c9a274b5131787ab20f86a647&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667YW46HGS%2F20260831%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260831T021736Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDiXfY2YtPySFBQjn8CsYSZQPvevYTgQ3PPLslKgHVoLgIgN4eQ3kSEJJZwfywqduqHCq5oiZ%2By9L7kwDsRuOAkb6oqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBBokJ7llCd8If%2B7hircA9JiHl7TXYB4wd6it%2FIcf8%2Bpxti5dp7ICSdg%2FMCo9uvNVjyFWUEQFbRHADmQZh%2BIMSF7KU3VW1fi8xYLtKKu7%2F7e2DePqzbDOjKHBISq0fYiLz3m1CaNEFONSBXpUkY4k%2BEb1mr%2FLXKXHPH9k3O2vtnIMu8nmTQTw%2F3Z2E59H%2FRKgAygevpnJ5zavAOLKhJZMaTbQTIWvcuQ0FqM4i5lqMx5jQQQIxMXFj0mMICebAMbbuF%2BQOEyK6nyrpZbCcJYMHS2aU7WO%2Bx4AVnEpxlA8mYBSVBVbc0TKsD8hWSAQFZqDBNHwwNEb9N%2BBulr6ifktxeWsn4RrojkT9bFGGR6mrmOGNFv062rrwK4iJcbbaYRj%2BVFo0iFPxKVBAiMdizvd%2FwNSvWC0nEFHI9TwGTO8o4aMR78vjtnG5QR%2BgTHZennmAK8T41wZYsSryxZYALhOeEmT3rSraUq9AUwcEVrVEOTO3tRKYx24q7N1w1bmvrozwrJHSOqzJMDl5WtTi6eW6u0R4c5HrKPKCYmtVtVcfmFuttgemwUR%2BodSfuaqGTaCHa2hPrKfBefzDI3zzG6Hq6MAgakKl2vXPd3XL6b2UxZaUzRtRuWX%2BpaJ5dyMPmY9biTOaBpkH9wq3BQMKCV09QGOqUBPP1oyMUn52duY4wxGg8nl4Dryr%2BoJLEqriiaB%2F3TJFyPh6nFONa3LtMRq%2BMJ2cwOaxmtgu0HU6%2BIeptDiLiRo0rzyMmP8Mj9nGSw4hz2aLB4VZyxVJl1qnGlc2%2FHECUPKlC6GqEhruDl7EzhOI%2FOLDqowP4SsDuEF0YEhw%2BtZTqjrHk%2BUzk9YSRdk5XNjZpN3bgYxUA27BeKFTVWkDduW6Imh1Hc&X-Amz-Signature=4d9047c28d13c7cce9fa265d762b73eadef2f3b3c5927baefc821350c1588c80&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667YW46HGS%2F20260831%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260831T021736Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDiXfY2YtPySFBQjn8CsYSZQPvevYTgQ3PPLslKgHVoLgIgN4eQ3kSEJJZwfywqduqHCq5oiZ%2By9L7kwDsRuOAkb6oqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBBokJ7llCd8If%2B7hircA9JiHl7TXYB4wd6it%2FIcf8%2Bpxti5dp7ICSdg%2FMCo9uvNVjyFWUEQFbRHADmQZh%2BIMSF7KU3VW1fi8xYLtKKu7%2F7e2DePqzbDOjKHBISq0fYiLz3m1CaNEFONSBXpUkY4k%2BEb1mr%2FLXKXHPH9k3O2vtnIMu8nmTQTw%2F3Z2E59H%2FRKgAygevpnJ5zavAOLKhJZMaTbQTIWvcuQ0FqM4i5lqMx5jQQQIxMXFj0mMICebAMbbuF%2BQOEyK6nyrpZbCcJYMHS2aU7WO%2Bx4AVnEpxlA8mYBSVBVbc0TKsD8hWSAQFZqDBNHwwNEb9N%2BBulr6ifktxeWsn4RrojkT9bFGGR6mrmOGNFv062rrwK4iJcbbaYRj%2BVFo0iFPxKVBAiMdizvd%2FwNSvWC0nEFHI9TwGTO8o4aMR78vjtnG5QR%2BgTHZennmAK8T41wZYsSryxZYALhOeEmT3rSraUq9AUwcEVrVEOTO3tRKYx24q7N1w1bmvrozwrJHSOqzJMDl5WtTi6eW6u0R4c5HrKPKCYmtVtVcfmFuttgemwUR%2BodSfuaqGTaCHa2hPrKfBefzDI3zzG6Hq6MAgakKl2vXPd3XL6b2UxZaUzRtRuWX%2BpaJ5dyMPmY9biTOaBpkH9wq3BQMKCV09QGOqUBPP1oyMUn52duY4wxGg8nl4Dryr%2BoJLEqriiaB%2F3TJFyPh6nFONa3LtMRq%2BMJ2cwOaxmtgu0HU6%2BIeptDiLiRo0rzyMmP8Mj9nGSw4hz2aLB4VZyxVJl1qnGlc2%2FHECUPKlC6GqEhruDl7EzhOI%2FOLDqowP4SsDuEF0YEhw%2BtZTqjrHk%2BUzk9YSRdk5XNjZpN3bgYxUA27BeKFTVWkDduW6Imh1Hc&X-Amz-Signature=4e17f1622001d085d7f6a1492d2a500c6a8bdc92449c2ad0f006897a2940f1d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667YW46HGS%2F20260831%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260831T021736Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDiXfY2YtPySFBQjn8CsYSZQPvevYTgQ3PPLslKgHVoLgIgN4eQ3kSEJJZwfywqduqHCq5oiZ%2By9L7kwDsRuOAkb6oqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBBokJ7llCd8If%2B7hircA9JiHl7TXYB4wd6it%2FIcf8%2Bpxti5dp7ICSdg%2FMCo9uvNVjyFWUEQFbRHADmQZh%2BIMSF7KU3VW1fi8xYLtKKu7%2F7e2DePqzbDOjKHBISq0fYiLz3m1CaNEFONSBXpUkY4k%2BEb1mr%2FLXKXHPH9k3O2vtnIMu8nmTQTw%2F3Z2E59H%2FRKgAygevpnJ5zavAOLKhJZMaTbQTIWvcuQ0FqM4i5lqMx5jQQQIxMXFj0mMICebAMbbuF%2BQOEyK6nyrpZbCcJYMHS2aU7WO%2Bx4AVnEpxlA8mYBSVBVbc0TKsD8hWSAQFZqDBNHwwNEb9N%2BBulr6ifktxeWsn4RrojkT9bFGGR6mrmOGNFv062rrwK4iJcbbaYRj%2BVFo0iFPxKVBAiMdizvd%2FwNSvWC0nEFHI9TwGTO8o4aMR78vjtnG5QR%2BgTHZennmAK8T41wZYsSryxZYALhOeEmT3rSraUq9AUwcEVrVEOTO3tRKYx24q7N1w1bmvrozwrJHSOqzJMDl5WtTi6eW6u0R4c5HrKPKCYmtVtVcfmFuttgemwUR%2BodSfuaqGTaCHa2hPrKfBefzDI3zzG6Hq6MAgakKl2vXPd3XL6b2UxZaUzRtRuWX%2BpaJ5dyMPmY9biTOaBpkH9wq3BQMKCV09QGOqUBPP1oyMUn52duY4wxGg8nl4Dryr%2BoJLEqriiaB%2F3TJFyPh6nFONa3LtMRq%2BMJ2cwOaxmtgu0HU6%2BIeptDiLiRo0rzyMmP8Mj9nGSw4hz2aLB4VZyxVJl1qnGlc2%2FHECUPKlC6GqEhruDl7EzhOI%2FOLDqowP4SsDuEF0YEhw%2BtZTqjrHk%2BUzk9YSRdk5XNjZpN3bgYxUA27BeKFTVWkDduW6Imh1Hc&X-Amz-Signature=b27de5df7e52b4abd4ece7eb780b219f49a1eff91d7953d9098bad0043f68ee2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667YW46HGS%2F20260831%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260831T021736Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDiXfY2YtPySFBQjn8CsYSZQPvevYTgQ3PPLslKgHVoLgIgN4eQ3kSEJJZwfywqduqHCq5oiZ%2By9L7kwDsRuOAkb6oqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBBokJ7llCd8If%2B7hircA9JiHl7TXYB4wd6it%2FIcf8%2Bpxti5dp7ICSdg%2FMCo9uvNVjyFWUEQFbRHADmQZh%2BIMSF7KU3VW1fi8xYLtKKu7%2F7e2DePqzbDOjKHBISq0fYiLz3m1CaNEFONSBXpUkY4k%2BEb1mr%2FLXKXHPH9k3O2vtnIMu8nmTQTw%2F3Z2E59H%2FRKgAygevpnJ5zavAOLKhJZMaTbQTIWvcuQ0FqM4i5lqMx5jQQQIxMXFj0mMICebAMbbuF%2BQOEyK6nyrpZbCcJYMHS2aU7WO%2Bx4AVnEpxlA8mYBSVBVbc0TKsD8hWSAQFZqDBNHwwNEb9N%2BBulr6ifktxeWsn4RrojkT9bFGGR6mrmOGNFv062rrwK4iJcbbaYRj%2BVFo0iFPxKVBAiMdizvd%2FwNSvWC0nEFHI9TwGTO8o4aMR78vjtnG5QR%2BgTHZennmAK8T41wZYsSryxZYALhOeEmT3rSraUq9AUwcEVrVEOTO3tRKYx24q7N1w1bmvrozwrJHSOqzJMDl5WtTi6eW6u0R4c5HrKPKCYmtVtVcfmFuttgemwUR%2BodSfuaqGTaCHa2hPrKfBefzDI3zzG6Hq6MAgakKl2vXPd3XL6b2UxZaUzRtRuWX%2BpaJ5dyMPmY9biTOaBpkH9wq3BQMKCV09QGOqUBPP1oyMUn52duY4wxGg8nl4Dryr%2BoJLEqriiaB%2F3TJFyPh6nFONa3LtMRq%2BMJ2cwOaxmtgu0HU6%2BIeptDiLiRo0rzyMmP8Mj9nGSw4hz2aLB4VZyxVJl1qnGlc2%2FHECUPKlC6GqEhruDl7EzhOI%2FOLDqowP4SsDuEF0YEhw%2BtZTqjrHk%2BUzk9YSRdk5XNjZpN3bgYxUA27BeKFTVWkDduW6Imh1Hc&X-Amz-Signature=0c4145309f60f98ab22c982932db2b397b169a341a1845b21f8ea7dcb73c3096&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667YW46HGS%2F20260831%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260831T021736Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDiXfY2YtPySFBQjn8CsYSZQPvevYTgQ3PPLslKgHVoLgIgN4eQ3kSEJJZwfywqduqHCq5oiZ%2By9L7kwDsRuOAkb6oqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBBokJ7llCd8If%2B7hircA9JiHl7TXYB4wd6it%2FIcf8%2Bpxti5dp7ICSdg%2FMCo9uvNVjyFWUEQFbRHADmQZh%2BIMSF7KU3VW1fi8xYLtKKu7%2F7e2DePqzbDOjKHBISq0fYiLz3m1CaNEFONSBXpUkY4k%2BEb1mr%2FLXKXHPH9k3O2vtnIMu8nmTQTw%2F3Z2E59H%2FRKgAygevpnJ5zavAOLKhJZMaTbQTIWvcuQ0FqM4i5lqMx5jQQQIxMXFj0mMICebAMbbuF%2BQOEyK6nyrpZbCcJYMHS2aU7WO%2Bx4AVnEpxlA8mYBSVBVbc0TKsD8hWSAQFZqDBNHwwNEb9N%2BBulr6ifktxeWsn4RrojkT9bFGGR6mrmOGNFv062rrwK4iJcbbaYRj%2BVFo0iFPxKVBAiMdizvd%2FwNSvWC0nEFHI9TwGTO8o4aMR78vjtnG5QR%2BgTHZennmAK8T41wZYsSryxZYALhOeEmT3rSraUq9AUwcEVrVEOTO3tRKYx24q7N1w1bmvrozwrJHSOqzJMDl5WtTi6eW6u0R4c5HrKPKCYmtVtVcfmFuttgemwUR%2BodSfuaqGTaCHa2hPrKfBefzDI3zzG6Hq6MAgakKl2vXPd3XL6b2UxZaUzRtRuWX%2BpaJ5dyMPmY9biTOaBpkH9wq3BQMKCV09QGOqUBPP1oyMUn52duY4wxGg8nl4Dryr%2BoJLEqriiaB%2F3TJFyPh6nFONa3LtMRq%2BMJ2cwOaxmtgu0HU6%2BIeptDiLiRo0rzyMmP8Mj9nGSw4hz2aLB4VZyxVJl1qnGlc2%2FHECUPKlC6GqEhruDl7EzhOI%2FOLDqowP4SsDuEF0YEhw%2BtZTqjrHk%2BUzk9YSRdk5XNjZpN3bgYxUA27BeKFTVWkDduW6Imh1Hc&X-Amz-Signature=ae418f0dc2e2a2d0923fc441f3b702a30a35720fc8b61c42fe4e84c272f310a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667YW46HGS%2F20260831%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260831T021736Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDiXfY2YtPySFBQjn8CsYSZQPvevYTgQ3PPLslKgHVoLgIgN4eQ3kSEJJZwfywqduqHCq5oiZ%2By9L7kwDsRuOAkb6oqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBBokJ7llCd8If%2B7hircA9JiHl7TXYB4wd6it%2FIcf8%2Bpxti5dp7ICSdg%2FMCo9uvNVjyFWUEQFbRHADmQZh%2BIMSF7KU3VW1fi8xYLtKKu7%2F7e2DePqzbDOjKHBISq0fYiLz3m1CaNEFONSBXpUkY4k%2BEb1mr%2FLXKXHPH9k3O2vtnIMu8nmTQTw%2F3Z2E59H%2FRKgAygevpnJ5zavAOLKhJZMaTbQTIWvcuQ0FqM4i5lqMx5jQQQIxMXFj0mMICebAMbbuF%2BQOEyK6nyrpZbCcJYMHS2aU7WO%2Bx4AVnEpxlA8mYBSVBVbc0TKsD8hWSAQFZqDBNHwwNEb9N%2BBulr6ifktxeWsn4RrojkT9bFGGR6mrmOGNFv062rrwK4iJcbbaYRj%2BVFo0iFPxKVBAiMdizvd%2FwNSvWC0nEFHI9TwGTO8o4aMR78vjtnG5QR%2BgTHZennmAK8T41wZYsSryxZYALhOeEmT3rSraUq9AUwcEVrVEOTO3tRKYx24q7N1w1bmvrozwrJHSOqzJMDl5WtTi6eW6u0R4c5HrKPKCYmtVtVcfmFuttgemwUR%2BodSfuaqGTaCHa2hPrKfBefzDI3zzG6Hq6MAgakKl2vXPd3XL6b2UxZaUzRtRuWX%2BpaJ5dyMPmY9biTOaBpkH9wq3BQMKCV09QGOqUBPP1oyMUn52duY4wxGg8nl4Dryr%2BoJLEqriiaB%2F3TJFyPh6nFONa3LtMRq%2BMJ2cwOaxmtgu0HU6%2BIeptDiLiRo0rzyMmP8Mj9nGSw4hz2aLB4VZyxVJl1qnGlc2%2FHECUPKlC6GqEhruDl7EzhOI%2FOLDqowP4SsDuEF0YEhw%2BtZTqjrHk%2BUzk9YSRdk5XNjZpN3bgYxUA27BeKFTVWkDduW6Imh1Hc&X-Amz-Signature=fa92ec85a954a55fe2647baeabe1a884498a6f3f26da7dc6c88188ca3dc93fa3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
