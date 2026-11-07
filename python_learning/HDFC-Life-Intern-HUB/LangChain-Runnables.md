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
fetched_at: '2026-11-07T03:42:25.888Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GHX5JYY%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIGgxw%2FiRfABVMbMR346aCE42VkrBav%2FDYdDZfuXwRVd4AiBylPJ2OjqTRhfiyUmaxOQ%2FjhQ%2B23AZVOFUaXk1PRiIRCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1vmi8jiSByjsS4fiKtwDZnxvIJNPQbbx1FrC4k0V7ZXcX4YVV5FR%2F%2BbrH8AMulfHcqWkx20%2FvGncdfB1zqfbWIl7cG83PCxplBTAnHQBZRlGchPQWf6Lf%2Fj7X%2BXso9XYmHzgAfMbSJhaVu5TpxGDFGW81v6RPDjMJ%2B%2B%2B36xW%2BqeWQ29IRvXZAyFI2fKB2vnPjlmXCNF7g%2FzGvIPmGDc4EQq29AUAyS9a04%2F64r869wbgwHA20A%2BI4mgwY5G%2BUGsujyiVfKBD7MKPuyZ%2BQk519DvASfmNYIQ3ujsN3sSi45brilN8IGCxRmYohC5qD21%2Ffw0aOnUFn4iCxZXmRHmaXBFJUOGZVB1XGYCUm5ouCtdierKqmqpae5eYHCTskPL8nQZNZ3Y1JhQT8ul6Mf%2FRKBheXrcGJS538BSDVQszo3HmvW6MSg1I29MNK8C34P52rCu7Rj14IeguvfRyDx6Bt4lTzEvPvEBBi4Pjw9bEpJNywSXXn4GRSCZh8qz8Xy1q5M1U3KaU0Y%2FpQ8GBrk5vntiRP2%2BnR3k6NiFU33wCRHfqSeCCY0wdyf7%2F%2Fq1eWp6LmpY%2FWZzAPplnx7PWCAJQ%2FqFR9zduifR3GlJ7PMM7tG4JPDWh4VPXU0%2BgOj1J3RFN3j5o9IqPxk1X8OwwgZa61wY6pgHz2TstQI6QLUe21Q9IH2SvWTJYHNkriuZ%2BFQYzOK5Eqj%2BPGha%2FWU53tmsoc2p0MR08ZaxwoAsVaSsHnvGmSGrt3eP9wqdg070hZCu3Fi6w4qNx4WR5H4t%2BMYndWGKbn66IQT2EqjCrm6chg6G8kU%2BWyp2w%2FMAYmWP%2F4UBgsInfTeqTctQu%2F2Jjc1ulPAH2M3r5vGGnb4%2F%2B%2FqSSnMnJ4IotWaB1%2F9Dq&X-Amz-Signature=474bec9c16b5855892535a1d3fc3f228cf035579b5cc783cd368bdfc652f8962&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GHX5JYY%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIGgxw%2FiRfABVMbMR346aCE42VkrBav%2FDYdDZfuXwRVd4AiBylPJ2OjqTRhfiyUmaxOQ%2FjhQ%2B23AZVOFUaXk1PRiIRCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1vmi8jiSByjsS4fiKtwDZnxvIJNPQbbx1FrC4k0V7ZXcX4YVV5FR%2F%2BbrH8AMulfHcqWkx20%2FvGncdfB1zqfbWIl7cG83PCxplBTAnHQBZRlGchPQWf6Lf%2Fj7X%2BXso9XYmHzgAfMbSJhaVu5TpxGDFGW81v6RPDjMJ%2B%2B%2B36xW%2BqeWQ29IRvXZAyFI2fKB2vnPjlmXCNF7g%2FzGvIPmGDc4EQq29AUAyS9a04%2F64r869wbgwHA20A%2BI4mgwY5G%2BUGsujyiVfKBD7MKPuyZ%2BQk519DvASfmNYIQ3ujsN3sSi45brilN8IGCxRmYohC5qD21%2Ffw0aOnUFn4iCxZXmRHmaXBFJUOGZVB1XGYCUm5ouCtdierKqmqpae5eYHCTskPL8nQZNZ3Y1JhQT8ul6Mf%2FRKBheXrcGJS538BSDVQszo3HmvW6MSg1I29MNK8C34P52rCu7Rj14IeguvfRyDx6Bt4lTzEvPvEBBi4Pjw9bEpJNywSXXn4GRSCZh8qz8Xy1q5M1U3KaU0Y%2FpQ8GBrk5vntiRP2%2BnR3k6NiFU33wCRHfqSeCCY0wdyf7%2F%2Fq1eWp6LmpY%2FWZzAPplnx7PWCAJQ%2FqFR9zduifR3GlJ7PMM7tG4JPDWh4VPXU0%2BgOj1J3RFN3j5o9IqPxk1X8OwwgZa61wY6pgHz2TstQI6QLUe21Q9IH2SvWTJYHNkriuZ%2BFQYzOK5Eqj%2BPGha%2FWU53tmsoc2p0MR08ZaxwoAsVaSsHnvGmSGrt3eP9wqdg070hZCu3Fi6w4qNx4WR5H4t%2BMYndWGKbn66IQT2EqjCrm6chg6G8kU%2BWyp2w%2FMAYmWP%2F4UBgsInfTeqTctQu%2F2Jjc1ulPAH2M3r5vGGnb4%2F%2B%2FqSSnMnJ4IotWaB1%2F9Dq&X-Amz-Signature=d90f25f15d27dceb62c4a195264f66b7f427fc2a66c0b13be782fb55737f4399&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GHX5JYY%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIGgxw%2FiRfABVMbMR346aCE42VkrBav%2FDYdDZfuXwRVd4AiBylPJ2OjqTRhfiyUmaxOQ%2FjhQ%2B23AZVOFUaXk1PRiIRCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1vmi8jiSByjsS4fiKtwDZnxvIJNPQbbx1FrC4k0V7ZXcX4YVV5FR%2F%2BbrH8AMulfHcqWkx20%2FvGncdfB1zqfbWIl7cG83PCxplBTAnHQBZRlGchPQWf6Lf%2Fj7X%2BXso9XYmHzgAfMbSJhaVu5TpxGDFGW81v6RPDjMJ%2B%2B%2B36xW%2BqeWQ29IRvXZAyFI2fKB2vnPjlmXCNF7g%2FzGvIPmGDc4EQq29AUAyS9a04%2F64r869wbgwHA20A%2BI4mgwY5G%2BUGsujyiVfKBD7MKPuyZ%2BQk519DvASfmNYIQ3ujsN3sSi45brilN8IGCxRmYohC5qD21%2Ffw0aOnUFn4iCxZXmRHmaXBFJUOGZVB1XGYCUm5ouCtdierKqmqpae5eYHCTskPL8nQZNZ3Y1JhQT8ul6Mf%2FRKBheXrcGJS538BSDVQszo3HmvW6MSg1I29MNK8C34P52rCu7Rj14IeguvfRyDx6Bt4lTzEvPvEBBi4Pjw9bEpJNywSXXn4GRSCZh8qz8Xy1q5M1U3KaU0Y%2FpQ8GBrk5vntiRP2%2BnR3k6NiFU33wCRHfqSeCCY0wdyf7%2F%2Fq1eWp6LmpY%2FWZzAPplnx7PWCAJQ%2FqFR9zduifR3GlJ7PMM7tG4JPDWh4VPXU0%2BgOj1J3RFN3j5o9IqPxk1X8OwwgZa61wY6pgHz2TstQI6QLUe21Q9IH2SvWTJYHNkriuZ%2BFQYzOK5Eqj%2BPGha%2FWU53tmsoc2p0MR08ZaxwoAsVaSsHnvGmSGrt3eP9wqdg070hZCu3Fi6w4qNx4WR5H4t%2BMYndWGKbn66IQT2EqjCrm6chg6G8kU%2BWyp2w%2FMAYmWP%2F4UBgsInfTeqTctQu%2F2Jjc1ulPAH2M3r5vGGnb4%2F%2B%2FqSSnMnJ4IotWaB1%2F9Dq&X-Amz-Signature=15fe9dd08dd647da1f57c34e953e59a1d5256cd11b1cab62b66895a5e155d56e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GHX5JYY%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIGgxw%2FiRfABVMbMR346aCE42VkrBav%2FDYdDZfuXwRVd4AiBylPJ2OjqTRhfiyUmaxOQ%2FjhQ%2B23AZVOFUaXk1PRiIRCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1vmi8jiSByjsS4fiKtwDZnxvIJNPQbbx1FrC4k0V7ZXcX4YVV5FR%2F%2BbrH8AMulfHcqWkx20%2FvGncdfB1zqfbWIl7cG83PCxplBTAnHQBZRlGchPQWf6Lf%2Fj7X%2BXso9XYmHzgAfMbSJhaVu5TpxGDFGW81v6RPDjMJ%2B%2B%2B36xW%2BqeWQ29IRvXZAyFI2fKB2vnPjlmXCNF7g%2FzGvIPmGDc4EQq29AUAyS9a04%2F64r869wbgwHA20A%2BI4mgwY5G%2BUGsujyiVfKBD7MKPuyZ%2BQk519DvASfmNYIQ3ujsN3sSi45brilN8IGCxRmYohC5qD21%2Ffw0aOnUFn4iCxZXmRHmaXBFJUOGZVB1XGYCUm5ouCtdierKqmqpae5eYHCTskPL8nQZNZ3Y1JhQT8ul6Mf%2FRKBheXrcGJS538BSDVQszo3HmvW6MSg1I29MNK8C34P52rCu7Rj14IeguvfRyDx6Bt4lTzEvPvEBBi4Pjw9bEpJNywSXXn4GRSCZh8qz8Xy1q5M1U3KaU0Y%2FpQ8GBrk5vntiRP2%2BnR3k6NiFU33wCRHfqSeCCY0wdyf7%2F%2Fq1eWp6LmpY%2FWZzAPplnx7PWCAJQ%2FqFR9zduifR3GlJ7PMM7tG4JPDWh4VPXU0%2BgOj1J3RFN3j5o9IqPxk1X8OwwgZa61wY6pgHz2TstQI6QLUe21Q9IH2SvWTJYHNkriuZ%2BFQYzOK5Eqj%2BPGha%2FWU53tmsoc2p0MR08ZaxwoAsVaSsHnvGmSGrt3eP9wqdg070hZCu3Fi6w4qNx4WR5H4t%2BMYndWGKbn66IQT2EqjCrm6chg6G8kU%2BWyp2w%2FMAYmWP%2F4UBgsInfTeqTctQu%2F2Jjc1ulPAH2M3r5vGGnb4%2F%2B%2FqSSnMnJ4IotWaB1%2F9Dq&X-Amz-Signature=ae6c9d538edc2a2e669ff98f87db4cbceaee8eddc0f5575ac8538be0b5b867ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GHX5JYY%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIGgxw%2FiRfABVMbMR346aCE42VkrBav%2FDYdDZfuXwRVd4AiBylPJ2OjqTRhfiyUmaxOQ%2FjhQ%2B23AZVOFUaXk1PRiIRCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1vmi8jiSByjsS4fiKtwDZnxvIJNPQbbx1FrC4k0V7ZXcX4YVV5FR%2F%2BbrH8AMulfHcqWkx20%2FvGncdfB1zqfbWIl7cG83PCxplBTAnHQBZRlGchPQWf6Lf%2Fj7X%2BXso9XYmHzgAfMbSJhaVu5TpxGDFGW81v6RPDjMJ%2B%2B%2B36xW%2BqeWQ29IRvXZAyFI2fKB2vnPjlmXCNF7g%2FzGvIPmGDc4EQq29AUAyS9a04%2F64r869wbgwHA20A%2BI4mgwY5G%2BUGsujyiVfKBD7MKPuyZ%2BQk519DvASfmNYIQ3ujsN3sSi45brilN8IGCxRmYohC5qD21%2Ffw0aOnUFn4iCxZXmRHmaXBFJUOGZVB1XGYCUm5ouCtdierKqmqpae5eYHCTskPL8nQZNZ3Y1JhQT8ul6Mf%2FRKBheXrcGJS538BSDVQszo3HmvW6MSg1I29MNK8C34P52rCu7Rj14IeguvfRyDx6Bt4lTzEvPvEBBi4Pjw9bEpJNywSXXn4GRSCZh8qz8Xy1q5M1U3KaU0Y%2FpQ8GBrk5vntiRP2%2BnR3k6NiFU33wCRHfqSeCCY0wdyf7%2F%2Fq1eWp6LmpY%2FWZzAPplnx7PWCAJQ%2FqFR9zduifR3GlJ7PMM7tG4JPDWh4VPXU0%2BgOj1J3RFN3j5o9IqPxk1X8OwwgZa61wY6pgHz2TstQI6QLUe21Q9IH2SvWTJYHNkriuZ%2BFQYzOK5Eqj%2BPGha%2FWU53tmsoc2p0MR08ZaxwoAsVaSsHnvGmSGrt3eP9wqdg070hZCu3Fi6w4qNx4WR5H4t%2BMYndWGKbn66IQT2EqjCrm6chg6G8kU%2BWyp2w%2FMAYmWP%2F4UBgsInfTeqTctQu%2F2Jjc1ulPAH2M3r5vGGnb4%2F%2B%2FqSSnMnJ4IotWaB1%2F9Dq&X-Amz-Signature=0cb8b776d8cf7912245d24f753c4b44c881665a4b3677c6cd9c543922f65a1b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GHX5JYY%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIGgxw%2FiRfABVMbMR346aCE42VkrBav%2FDYdDZfuXwRVd4AiBylPJ2OjqTRhfiyUmaxOQ%2FjhQ%2B23AZVOFUaXk1PRiIRCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1vmi8jiSByjsS4fiKtwDZnxvIJNPQbbx1FrC4k0V7ZXcX4YVV5FR%2F%2BbrH8AMulfHcqWkx20%2FvGncdfB1zqfbWIl7cG83PCxplBTAnHQBZRlGchPQWf6Lf%2Fj7X%2BXso9XYmHzgAfMbSJhaVu5TpxGDFGW81v6RPDjMJ%2B%2B%2B36xW%2BqeWQ29IRvXZAyFI2fKB2vnPjlmXCNF7g%2FzGvIPmGDc4EQq29AUAyS9a04%2F64r869wbgwHA20A%2BI4mgwY5G%2BUGsujyiVfKBD7MKPuyZ%2BQk519DvASfmNYIQ3ujsN3sSi45brilN8IGCxRmYohC5qD21%2Ffw0aOnUFn4iCxZXmRHmaXBFJUOGZVB1XGYCUm5ouCtdierKqmqpae5eYHCTskPL8nQZNZ3Y1JhQT8ul6Mf%2FRKBheXrcGJS538BSDVQszo3HmvW6MSg1I29MNK8C34P52rCu7Rj14IeguvfRyDx6Bt4lTzEvPvEBBi4Pjw9bEpJNywSXXn4GRSCZh8qz8Xy1q5M1U3KaU0Y%2FpQ8GBrk5vntiRP2%2BnR3k6NiFU33wCRHfqSeCCY0wdyf7%2F%2Fq1eWp6LmpY%2FWZzAPplnx7PWCAJQ%2FqFR9zduifR3GlJ7PMM7tG4JPDWh4VPXU0%2BgOj1J3RFN3j5o9IqPxk1X8OwwgZa61wY6pgHz2TstQI6QLUe21Q9IH2SvWTJYHNkriuZ%2BFQYzOK5Eqj%2BPGha%2FWU53tmsoc2p0MR08ZaxwoAsVaSsHnvGmSGrt3eP9wqdg070hZCu3Fi6w4qNx4WR5H4t%2BMYndWGKbn66IQT2EqjCrm6chg6G8kU%2BWyp2w%2FMAYmWP%2F4UBgsInfTeqTctQu%2F2Jjc1ulPAH2M3r5vGGnb4%2F%2B%2FqSSnMnJ4IotWaB1%2F9Dq&X-Amz-Signature=34bc20148b95fe28c59cecf16906c87cec1a5b88ee5403601c3c5b7835bcb4b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GHX5JYY%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIGgxw%2FiRfABVMbMR346aCE42VkrBav%2FDYdDZfuXwRVd4AiBylPJ2OjqTRhfiyUmaxOQ%2FjhQ%2B23AZVOFUaXk1PRiIRCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1vmi8jiSByjsS4fiKtwDZnxvIJNPQbbx1FrC4k0V7ZXcX4YVV5FR%2F%2BbrH8AMulfHcqWkx20%2FvGncdfB1zqfbWIl7cG83PCxplBTAnHQBZRlGchPQWf6Lf%2Fj7X%2BXso9XYmHzgAfMbSJhaVu5TpxGDFGW81v6RPDjMJ%2B%2B%2B36xW%2BqeWQ29IRvXZAyFI2fKB2vnPjlmXCNF7g%2FzGvIPmGDc4EQq29AUAyS9a04%2F64r869wbgwHA20A%2BI4mgwY5G%2BUGsujyiVfKBD7MKPuyZ%2BQk519DvASfmNYIQ3ujsN3sSi45brilN8IGCxRmYohC5qD21%2Ffw0aOnUFn4iCxZXmRHmaXBFJUOGZVB1XGYCUm5ouCtdierKqmqpae5eYHCTskPL8nQZNZ3Y1JhQT8ul6Mf%2FRKBheXrcGJS538BSDVQszo3HmvW6MSg1I29MNK8C34P52rCu7Rj14IeguvfRyDx6Bt4lTzEvPvEBBi4Pjw9bEpJNywSXXn4GRSCZh8qz8Xy1q5M1U3KaU0Y%2FpQ8GBrk5vntiRP2%2BnR3k6NiFU33wCRHfqSeCCY0wdyf7%2F%2Fq1eWp6LmpY%2FWZzAPplnx7PWCAJQ%2FqFR9zduifR3GlJ7PMM7tG4JPDWh4VPXU0%2BgOj1J3RFN3j5o9IqPxk1X8OwwgZa61wY6pgHz2TstQI6QLUe21Q9IH2SvWTJYHNkriuZ%2BFQYzOK5Eqj%2BPGha%2FWU53tmsoc2p0MR08ZaxwoAsVaSsHnvGmSGrt3eP9wqdg070hZCu3Fi6w4qNx4WR5H4t%2BMYndWGKbn66IQT2EqjCrm6chg6G8kU%2BWyp2w%2FMAYmWP%2F4UBgsInfTeqTctQu%2F2Jjc1ulPAH2M3r5vGGnb4%2F%2B%2FqSSnMnJ4IotWaB1%2F9Dq&X-Amz-Signature=a37ec4b85255b15a0e4e2a089036b07ecf793149d7b9fd37cf264d705d1d2002&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GHX5JYY%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIGgxw%2FiRfABVMbMR346aCE42VkrBav%2FDYdDZfuXwRVd4AiBylPJ2OjqTRhfiyUmaxOQ%2FjhQ%2B23AZVOFUaXk1PRiIRCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1vmi8jiSByjsS4fiKtwDZnxvIJNPQbbx1FrC4k0V7ZXcX4YVV5FR%2F%2BbrH8AMulfHcqWkx20%2FvGncdfB1zqfbWIl7cG83PCxplBTAnHQBZRlGchPQWf6Lf%2Fj7X%2BXso9XYmHzgAfMbSJhaVu5TpxGDFGW81v6RPDjMJ%2B%2B%2B36xW%2BqeWQ29IRvXZAyFI2fKB2vnPjlmXCNF7g%2FzGvIPmGDc4EQq29AUAyS9a04%2F64r869wbgwHA20A%2BI4mgwY5G%2BUGsujyiVfKBD7MKPuyZ%2BQk519DvASfmNYIQ3ujsN3sSi45brilN8IGCxRmYohC5qD21%2Ffw0aOnUFn4iCxZXmRHmaXBFJUOGZVB1XGYCUm5ouCtdierKqmqpae5eYHCTskPL8nQZNZ3Y1JhQT8ul6Mf%2FRKBheXrcGJS538BSDVQszo3HmvW6MSg1I29MNK8C34P52rCu7Rj14IeguvfRyDx6Bt4lTzEvPvEBBi4Pjw9bEpJNywSXXn4GRSCZh8qz8Xy1q5M1U3KaU0Y%2FpQ8GBrk5vntiRP2%2BnR3k6NiFU33wCRHfqSeCCY0wdyf7%2F%2Fq1eWp6LmpY%2FWZzAPplnx7PWCAJQ%2FqFR9zduifR3GlJ7PMM7tG4JPDWh4VPXU0%2BgOj1J3RFN3j5o9IqPxk1X8OwwgZa61wY6pgHz2TstQI6QLUe21Q9IH2SvWTJYHNkriuZ%2BFQYzOK5Eqj%2BPGha%2FWU53tmsoc2p0MR08ZaxwoAsVaSsHnvGmSGrt3eP9wqdg070hZCu3Fi6w4qNx4WR5H4t%2BMYndWGKbn66IQT2EqjCrm6chg6G8kU%2BWyp2w%2FMAYmWP%2F4UBgsInfTeqTctQu%2F2Jjc1ulPAH2M3r5vGGnb4%2F%2B%2FqSSnMnJ4IotWaB1%2F9Dq&X-Amz-Signature=3de502d1a264c8782b28b8464dc562873f48c0d2fccc081ae4a88828ad456f38&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GHX5JYY%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIGgxw%2FiRfABVMbMR346aCE42VkrBav%2FDYdDZfuXwRVd4AiBylPJ2OjqTRhfiyUmaxOQ%2FjhQ%2B23AZVOFUaXk1PRiIRCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1vmi8jiSByjsS4fiKtwDZnxvIJNPQbbx1FrC4k0V7ZXcX4YVV5FR%2F%2BbrH8AMulfHcqWkx20%2FvGncdfB1zqfbWIl7cG83PCxplBTAnHQBZRlGchPQWf6Lf%2Fj7X%2BXso9XYmHzgAfMbSJhaVu5TpxGDFGW81v6RPDjMJ%2B%2B%2B36xW%2BqeWQ29IRvXZAyFI2fKB2vnPjlmXCNF7g%2FzGvIPmGDc4EQq29AUAyS9a04%2F64r869wbgwHA20A%2BI4mgwY5G%2BUGsujyiVfKBD7MKPuyZ%2BQk519DvASfmNYIQ3ujsN3sSi45brilN8IGCxRmYohC5qD21%2Ffw0aOnUFn4iCxZXmRHmaXBFJUOGZVB1XGYCUm5ouCtdierKqmqpae5eYHCTskPL8nQZNZ3Y1JhQT8ul6Mf%2FRKBheXrcGJS538BSDVQszo3HmvW6MSg1I29MNK8C34P52rCu7Rj14IeguvfRyDx6Bt4lTzEvPvEBBi4Pjw9bEpJNywSXXn4GRSCZh8qz8Xy1q5M1U3KaU0Y%2FpQ8GBrk5vntiRP2%2BnR3k6NiFU33wCRHfqSeCCY0wdyf7%2F%2Fq1eWp6LmpY%2FWZzAPplnx7PWCAJQ%2FqFR9zduifR3GlJ7PMM7tG4JPDWh4VPXU0%2BgOj1J3RFN3j5o9IqPxk1X8OwwgZa61wY6pgHz2TstQI6QLUe21Q9IH2SvWTJYHNkriuZ%2BFQYzOK5Eqj%2BPGha%2FWU53tmsoc2p0MR08ZaxwoAsVaSsHnvGmSGrt3eP9wqdg070hZCu3Fi6w4qNx4WR5H4t%2BMYndWGKbn66IQT2EqjCrm6chg6G8kU%2BWyp2w%2FMAYmWP%2F4UBgsInfTeqTctQu%2F2Jjc1ulPAH2M3r5vGGnb4%2F%2B%2FqSSnMnJ4IotWaB1%2F9Dq&X-Amz-Signature=54bc17a335784cab8c25a5a8d844c8b2a5c2bdd70a12a76ff09029a367a7c7d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
