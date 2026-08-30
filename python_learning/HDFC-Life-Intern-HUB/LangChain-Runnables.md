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
fetched_at: '2026-08-30T02:22:25.863Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZY4WCCD%2F20260830%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260830T022221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNpQiNvEqWn7Llv2miELboVNnoEIQC4UnpHU5yrDrQVwIhAODD9a8%2BR7hZwUHEkKFm1szOaRB08zyI0v%2FSc5cC8i3nKv8DCHIQABoMNjM3NDIzMTgzODA1IgwKXLgAh9oW1HdkQiIq3APpXIJuNKYn7RGwUHqNVIbAuKr5%2F0YTzBw8k86p%2Bj5aST1v%2BT9CzhEepFva1nDwJUH9n8D1TmKEYunO7Mk%2FOeeLGDKJWYO65RgeqgmyEcBhz%2FJkmU3xCPYhT5pptVWmMB3LTmWhYs0XfaJ%2FG18ttZxazpO6kAMUqnAmeVRZGqx3ISOspw%2BY2Md1BzFrUfzaJCnhzphTHFVOh7wizOIca2TzVwiEZPqxpY%2Fil%2FemOzkcjDiygcCsPYzAxyIknBktZmmZU3ejBmQQzfv8cBZllWgWGdV3YbZNvMFzfl4ueEQcBZL3DSXvX9Awr7oDSgQFlI4u3SFGEUBsaaN%2Bfj%2FFoHlyu6r%2BshI4ATGy%2F16%2BMpFokoGUJCXkghqx445Pz70n5lafSDL%2BYfIM%2Brp009XZZzkR8TzL15G9Uc1YAmh5fHLfnDYRDUiuzNJfkVntVvRR7DFtUW%2BgcGST1203uO%2BohrSMVVKw5st%2FV%2B%2F3%2FnXiJ5HyaTYJ7I7LPYwaMxBttrD5XdgRKhnaHNMKgVzLm4cQDm3B9qovw0q1TG2TFM7IK0N3CRnHV8v4piP5tAq0BS4p2GYXBp2uvQe6uTRgjvHRC91T2m7P7KUBKWwix%2B3En0CKfhpRijdLY9yAJlW99TCGh87UBjqkAYd2Qd3WKCWPJZ91ZSBjDEn6aU7s17AnGAedq8gS5zHiA4gpgfYCawc9jQ1qeRmxRuQfEHZbNkP55YdKsVOIWeAdyiZ4XNNdL%2FwYrMpjdrK5gUHQO6uO%2Bf6b2542hwuKhctIotcg2t8RIT0vDNiOPVcxfvr9rxe6KkKDCJGHcydLH3qlGtmOo8amyH%2FT09B6KGWzvr0MEznvbOtfZ0CYYrlpUvxL&X-Amz-Signature=b79596fca1d1d8a534313ca19fe7e3a10a8a51bf39c5770c3da00d26125aa3f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZY4WCCD%2F20260830%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260830T022221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNpQiNvEqWn7Llv2miELboVNnoEIQC4UnpHU5yrDrQVwIhAODD9a8%2BR7hZwUHEkKFm1szOaRB08zyI0v%2FSc5cC8i3nKv8DCHIQABoMNjM3NDIzMTgzODA1IgwKXLgAh9oW1HdkQiIq3APpXIJuNKYn7RGwUHqNVIbAuKr5%2F0YTzBw8k86p%2Bj5aST1v%2BT9CzhEepFva1nDwJUH9n8D1TmKEYunO7Mk%2FOeeLGDKJWYO65RgeqgmyEcBhz%2FJkmU3xCPYhT5pptVWmMB3LTmWhYs0XfaJ%2FG18ttZxazpO6kAMUqnAmeVRZGqx3ISOspw%2BY2Md1BzFrUfzaJCnhzphTHFVOh7wizOIca2TzVwiEZPqxpY%2Fil%2FemOzkcjDiygcCsPYzAxyIknBktZmmZU3ejBmQQzfv8cBZllWgWGdV3YbZNvMFzfl4ueEQcBZL3DSXvX9Awr7oDSgQFlI4u3SFGEUBsaaN%2Bfj%2FFoHlyu6r%2BshI4ATGy%2F16%2BMpFokoGUJCXkghqx445Pz70n5lafSDL%2BYfIM%2Brp009XZZzkR8TzL15G9Uc1YAmh5fHLfnDYRDUiuzNJfkVntVvRR7DFtUW%2BgcGST1203uO%2BohrSMVVKw5st%2FV%2B%2F3%2FnXiJ5HyaTYJ7I7LPYwaMxBttrD5XdgRKhnaHNMKgVzLm4cQDm3B9qovw0q1TG2TFM7IK0N3CRnHV8v4piP5tAq0BS4p2GYXBp2uvQe6uTRgjvHRC91T2m7P7KUBKWwix%2B3En0CKfhpRijdLY9yAJlW99TCGh87UBjqkAYd2Qd3WKCWPJZ91ZSBjDEn6aU7s17AnGAedq8gS5zHiA4gpgfYCawc9jQ1qeRmxRuQfEHZbNkP55YdKsVOIWeAdyiZ4XNNdL%2FwYrMpjdrK5gUHQO6uO%2Bf6b2542hwuKhctIotcg2t8RIT0vDNiOPVcxfvr9rxe6KkKDCJGHcydLH3qlGtmOo8amyH%2FT09B6KGWzvr0MEznvbOtfZ0CYYrlpUvxL&X-Amz-Signature=6792567528cb7cfe940364ab3cab41ad841090d7ee0a431d107a56d44d576cf9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZY4WCCD%2F20260830%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260830T022221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNpQiNvEqWn7Llv2miELboVNnoEIQC4UnpHU5yrDrQVwIhAODD9a8%2BR7hZwUHEkKFm1szOaRB08zyI0v%2FSc5cC8i3nKv8DCHIQABoMNjM3NDIzMTgzODA1IgwKXLgAh9oW1HdkQiIq3APpXIJuNKYn7RGwUHqNVIbAuKr5%2F0YTzBw8k86p%2Bj5aST1v%2BT9CzhEepFva1nDwJUH9n8D1TmKEYunO7Mk%2FOeeLGDKJWYO65RgeqgmyEcBhz%2FJkmU3xCPYhT5pptVWmMB3LTmWhYs0XfaJ%2FG18ttZxazpO6kAMUqnAmeVRZGqx3ISOspw%2BY2Md1BzFrUfzaJCnhzphTHFVOh7wizOIca2TzVwiEZPqxpY%2Fil%2FemOzkcjDiygcCsPYzAxyIknBktZmmZU3ejBmQQzfv8cBZllWgWGdV3YbZNvMFzfl4ueEQcBZL3DSXvX9Awr7oDSgQFlI4u3SFGEUBsaaN%2Bfj%2FFoHlyu6r%2BshI4ATGy%2F16%2BMpFokoGUJCXkghqx445Pz70n5lafSDL%2BYfIM%2Brp009XZZzkR8TzL15G9Uc1YAmh5fHLfnDYRDUiuzNJfkVntVvRR7DFtUW%2BgcGST1203uO%2BohrSMVVKw5st%2FV%2B%2F3%2FnXiJ5HyaTYJ7I7LPYwaMxBttrD5XdgRKhnaHNMKgVzLm4cQDm3B9qovw0q1TG2TFM7IK0N3CRnHV8v4piP5tAq0BS4p2GYXBp2uvQe6uTRgjvHRC91T2m7P7KUBKWwix%2B3En0CKfhpRijdLY9yAJlW99TCGh87UBjqkAYd2Qd3WKCWPJZ91ZSBjDEn6aU7s17AnGAedq8gS5zHiA4gpgfYCawc9jQ1qeRmxRuQfEHZbNkP55YdKsVOIWeAdyiZ4XNNdL%2FwYrMpjdrK5gUHQO6uO%2Bf6b2542hwuKhctIotcg2t8RIT0vDNiOPVcxfvr9rxe6KkKDCJGHcydLH3qlGtmOo8amyH%2FT09B6KGWzvr0MEznvbOtfZ0CYYrlpUvxL&X-Amz-Signature=cc46bed7b7ab77b11f64070ac1482c17874cc04713f718df905d320642473465&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZY4WCCD%2F20260830%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260830T022221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNpQiNvEqWn7Llv2miELboVNnoEIQC4UnpHU5yrDrQVwIhAODD9a8%2BR7hZwUHEkKFm1szOaRB08zyI0v%2FSc5cC8i3nKv8DCHIQABoMNjM3NDIzMTgzODA1IgwKXLgAh9oW1HdkQiIq3APpXIJuNKYn7RGwUHqNVIbAuKr5%2F0YTzBw8k86p%2Bj5aST1v%2BT9CzhEepFva1nDwJUH9n8D1TmKEYunO7Mk%2FOeeLGDKJWYO65RgeqgmyEcBhz%2FJkmU3xCPYhT5pptVWmMB3LTmWhYs0XfaJ%2FG18ttZxazpO6kAMUqnAmeVRZGqx3ISOspw%2BY2Md1BzFrUfzaJCnhzphTHFVOh7wizOIca2TzVwiEZPqxpY%2Fil%2FemOzkcjDiygcCsPYzAxyIknBktZmmZU3ejBmQQzfv8cBZllWgWGdV3YbZNvMFzfl4ueEQcBZL3DSXvX9Awr7oDSgQFlI4u3SFGEUBsaaN%2Bfj%2FFoHlyu6r%2BshI4ATGy%2F16%2BMpFokoGUJCXkghqx445Pz70n5lafSDL%2BYfIM%2Brp009XZZzkR8TzL15G9Uc1YAmh5fHLfnDYRDUiuzNJfkVntVvRR7DFtUW%2BgcGST1203uO%2BohrSMVVKw5st%2FV%2B%2F3%2FnXiJ5HyaTYJ7I7LPYwaMxBttrD5XdgRKhnaHNMKgVzLm4cQDm3B9qovw0q1TG2TFM7IK0N3CRnHV8v4piP5tAq0BS4p2GYXBp2uvQe6uTRgjvHRC91T2m7P7KUBKWwix%2B3En0CKfhpRijdLY9yAJlW99TCGh87UBjqkAYd2Qd3WKCWPJZ91ZSBjDEn6aU7s17AnGAedq8gS5zHiA4gpgfYCawc9jQ1qeRmxRuQfEHZbNkP55YdKsVOIWeAdyiZ4XNNdL%2FwYrMpjdrK5gUHQO6uO%2Bf6b2542hwuKhctIotcg2t8RIT0vDNiOPVcxfvr9rxe6KkKDCJGHcydLH3qlGtmOo8amyH%2FT09B6KGWzvr0MEznvbOtfZ0CYYrlpUvxL&X-Amz-Signature=6bba3261883449f1b79fd9fe64a138e530863ec9ff884cdfc4d741c2f5a9f5dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZY4WCCD%2F20260830%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260830T022221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNpQiNvEqWn7Llv2miELboVNnoEIQC4UnpHU5yrDrQVwIhAODD9a8%2BR7hZwUHEkKFm1szOaRB08zyI0v%2FSc5cC8i3nKv8DCHIQABoMNjM3NDIzMTgzODA1IgwKXLgAh9oW1HdkQiIq3APpXIJuNKYn7RGwUHqNVIbAuKr5%2F0YTzBw8k86p%2Bj5aST1v%2BT9CzhEepFva1nDwJUH9n8D1TmKEYunO7Mk%2FOeeLGDKJWYO65RgeqgmyEcBhz%2FJkmU3xCPYhT5pptVWmMB3LTmWhYs0XfaJ%2FG18ttZxazpO6kAMUqnAmeVRZGqx3ISOspw%2BY2Md1BzFrUfzaJCnhzphTHFVOh7wizOIca2TzVwiEZPqxpY%2Fil%2FemOzkcjDiygcCsPYzAxyIknBktZmmZU3ejBmQQzfv8cBZllWgWGdV3YbZNvMFzfl4ueEQcBZL3DSXvX9Awr7oDSgQFlI4u3SFGEUBsaaN%2Bfj%2FFoHlyu6r%2BshI4ATGy%2F16%2BMpFokoGUJCXkghqx445Pz70n5lafSDL%2BYfIM%2Brp009XZZzkR8TzL15G9Uc1YAmh5fHLfnDYRDUiuzNJfkVntVvRR7DFtUW%2BgcGST1203uO%2BohrSMVVKw5st%2FV%2B%2F3%2FnXiJ5HyaTYJ7I7LPYwaMxBttrD5XdgRKhnaHNMKgVzLm4cQDm3B9qovw0q1TG2TFM7IK0N3CRnHV8v4piP5tAq0BS4p2GYXBp2uvQe6uTRgjvHRC91T2m7P7KUBKWwix%2B3En0CKfhpRijdLY9yAJlW99TCGh87UBjqkAYd2Qd3WKCWPJZ91ZSBjDEn6aU7s17AnGAedq8gS5zHiA4gpgfYCawc9jQ1qeRmxRuQfEHZbNkP55YdKsVOIWeAdyiZ4XNNdL%2FwYrMpjdrK5gUHQO6uO%2Bf6b2542hwuKhctIotcg2t8RIT0vDNiOPVcxfvr9rxe6KkKDCJGHcydLH3qlGtmOo8amyH%2FT09B6KGWzvr0MEznvbOtfZ0CYYrlpUvxL&X-Amz-Signature=d5907f94f38039eb08c6ea540ccb8178779841e7d7cc1d6b99a8ada5c182a3f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZY4WCCD%2F20260830%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260830T022221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNpQiNvEqWn7Llv2miELboVNnoEIQC4UnpHU5yrDrQVwIhAODD9a8%2BR7hZwUHEkKFm1szOaRB08zyI0v%2FSc5cC8i3nKv8DCHIQABoMNjM3NDIzMTgzODA1IgwKXLgAh9oW1HdkQiIq3APpXIJuNKYn7RGwUHqNVIbAuKr5%2F0YTzBw8k86p%2Bj5aST1v%2BT9CzhEepFva1nDwJUH9n8D1TmKEYunO7Mk%2FOeeLGDKJWYO65RgeqgmyEcBhz%2FJkmU3xCPYhT5pptVWmMB3LTmWhYs0XfaJ%2FG18ttZxazpO6kAMUqnAmeVRZGqx3ISOspw%2BY2Md1BzFrUfzaJCnhzphTHFVOh7wizOIca2TzVwiEZPqxpY%2Fil%2FemOzkcjDiygcCsPYzAxyIknBktZmmZU3ejBmQQzfv8cBZllWgWGdV3YbZNvMFzfl4ueEQcBZL3DSXvX9Awr7oDSgQFlI4u3SFGEUBsaaN%2Bfj%2FFoHlyu6r%2BshI4ATGy%2F16%2BMpFokoGUJCXkghqx445Pz70n5lafSDL%2BYfIM%2Brp009XZZzkR8TzL15G9Uc1YAmh5fHLfnDYRDUiuzNJfkVntVvRR7DFtUW%2BgcGST1203uO%2BohrSMVVKw5st%2FV%2B%2F3%2FnXiJ5HyaTYJ7I7LPYwaMxBttrD5XdgRKhnaHNMKgVzLm4cQDm3B9qovw0q1TG2TFM7IK0N3CRnHV8v4piP5tAq0BS4p2GYXBp2uvQe6uTRgjvHRC91T2m7P7KUBKWwix%2B3En0CKfhpRijdLY9yAJlW99TCGh87UBjqkAYd2Qd3WKCWPJZ91ZSBjDEn6aU7s17AnGAedq8gS5zHiA4gpgfYCawc9jQ1qeRmxRuQfEHZbNkP55YdKsVOIWeAdyiZ4XNNdL%2FwYrMpjdrK5gUHQO6uO%2Bf6b2542hwuKhctIotcg2t8RIT0vDNiOPVcxfvr9rxe6KkKDCJGHcydLH3qlGtmOo8amyH%2FT09B6KGWzvr0MEznvbOtfZ0CYYrlpUvxL&X-Amz-Signature=4c4921123051529d5727d4e3882666d70bc621555f42f67a043f19f89e5b3e50&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZY4WCCD%2F20260830%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260830T022221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNpQiNvEqWn7Llv2miELboVNnoEIQC4UnpHU5yrDrQVwIhAODD9a8%2BR7hZwUHEkKFm1szOaRB08zyI0v%2FSc5cC8i3nKv8DCHIQABoMNjM3NDIzMTgzODA1IgwKXLgAh9oW1HdkQiIq3APpXIJuNKYn7RGwUHqNVIbAuKr5%2F0YTzBw8k86p%2Bj5aST1v%2BT9CzhEepFva1nDwJUH9n8D1TmKEYunO7Mk%2FOeeLGDKJWYO65RgeqgmyEcBhz%2FJkmU3xCPYhT5pptVWmMB3LTmWhYs0XfaJ%2FG18ttZxazpO6kAMUqnAmeVRZGqx3ISOspw%2BY2Md1BzFrUfzaJCnhzphTHFVOh7wizOIca2TzVwiEZPqxpY%2Fil%2FemOzkcjDiygcCsPYzAxyIknBktZmmZU3ejBmQQzfv8cBZllWgWGdV3YbZNvMFzfl4ueEQcBZL3DSXvX9Awr7oDSgQFlI4u3SFGEUBsaaN%2Bfj%2FFoHlyu6r%2BshI4ATGy%2F16%2BMpFokoGUJCXkghqx445Pz70n5lafSDL%2BYfIM%2Brp009XZZzkR8TzL15G9Uc1YAmh5fHLfnDYRDUiuzNJfkVntVvRR7DFtUW%2BgcGST1203uO%2BohrSMVVKw5st%2FV%2B%2F3%2FnXiJ5HyaTYJ7I7LPYwaMxBttrD5XdgRKhnaHNMKgVzLm4cQDm3B9qovw0q1TG2TFM7IK0N3CRnHV8v4piP5tAq0BS4p2GYXBp2uvQe6uTRgjvHRC91T2m7P7KUBKWwix%2B3En0CKfhpRijdLY9yAJlW99TCGh87UBjqkAYd2Qd3WKCWPJZ91ZSBjDEn6aU7s17AnGAedq8gS5zHiA4gpgfYCawc9jQ1qeRmxRuQfEHZbNkP55YdKsVOIWeAdyiZ4XNNdL%2FwYrMpjdrK5gUHQO6uO%2Bf6b2542hwuKhctIotcg2t8RIT0vDNiOPVcxfvr9rxe6KkKDCJGHcydLH3qlGtmOo8amyH%2FT09B6KGWzvr0MEznvbOtfZ0CYYrlpUvxL&X-Amz-Signature=0074781e31df3b5b479709dc4aedd7337d9f02b10ff4b4781cc433029f5fcc9f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZY4WCCD%2F20260830%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260830T022221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNpQiNvEqWn7Llv2miELboVNnoEIQC4UnpHU5yrDrQVwIhAODD9a8%2BR7hZwUHEkKFm1szOaRB08zyI0v%2FSc5cC8i3nKv8DCHIQABoMNjM3NDIzMTgzODA1IgwKXLgAh9oW1HdkQiIq3APpXIJuNKYn7RGwUHqNVIbAuKr5%2F0YTzBw8k86p%2Bj5aST1v%2BT9CzhEepFva1nDwJUH9n8D1TmKEYunO7Mk%2FOeeLGDKJWYO65RgeqgmyEcBhz%2FJkmU3xCPYhT5pptVWmMB3LTmWhYs0XfaJ%2FG18ttZxazpO6kAMUqnAmeVRZGqx3ISOspw%2BY2Md1BzFrUfzaJCnhzphTHFVOh7wizOIca2TzVwiEZPqxpY%2Fil%2FemOzkcjDiygcCsPYzAxyIknBktZmmZU3ejBmQQzfv8cBZllWgWGdV3YbZNvMFzfl4ueEQcBZL3DSXvX9Awr7oDSgQFlI4u3SFGEUBsaaN%2Bfj%2FFoHlyu6r%2BshI4ATGy%2F16%2BMpFokoGUJCXkghqx445Pz70n5lafSDL%2BYfIM%2Brp009XZZzkR8TzL15G9Uc1YAmh5fHLfnDYRDUiuzNJfkVntVvRR7DFtUW%2BgcGST1203uO%2BohrSMVVKw5st%2FV%2B%2F3%2FnXiJ5HyaTYJ7I7LPYwaMxBttrD5XdgRKhnaHNMKgVzLm4cQDm3B9qovw0q1TG2TFM7IK0N3CRnHV8v4piP5tAq0BS4p2GYXBp2uvQe6uTRgjvHRC91T2m7P7KUBKWwix%2B3En0CKfhpRijdLY9yAJlW99TCGh87UBjqkAYd2Qd3WKCWPJZ91ZSBjDEn6aU7s17AnGAedq8gS5zHiA4gpgfYCawc9jQ1qeRmxRuQfEHZbNkP55YdKsVOIWeAdyiZ4XNNdL%2FwYrMpjdrK5gUHQO6uO%2Bf6b2542hwuKhctIotcg2t8RIT0vDNiOPVcxfvr9rxe6KkKDCJGHcydLH3qlGtmOo8amyH%2FT09B6KGWzvr0MEznvbOtfZ0CYYrlpUvxL&X-Amz-Signature=d314108fb94ab7af2ec3337e2e7657cdc65aa49550c7ff98dc5ba7d5c11c8528&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZY4WCCD%2F20260830%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260830T022221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNpQiNvEqWn7Llv2miELboVNnoEIQC4UnpHU5yrDrQVwIhAODD9a8%2BR7hZwUHEkKFm1szOaRB08zyI0v%2FSc5cC8i3nKv8DCHIQABoMNjM3NDIzMTgzODA1IgwKXLgAh9oW1HdkQiIq3APpXIJuNKYn7RGwUHqNVIbAuKr5%2F0YTzBw8k86p%2Bj5aST1v%2BT9CzhEepFva1nDwJUH9n8D1TmKEYunO7Mk%2FOeeLGDKJWYO65RgeqgmyEcBhz%2FJkmU3xCPYhT5pptVWmMB3LTmWhYs0XfaJ%2FG18ttZxazpO6kAMUqnAmeVRZGqx3ISOspw%2BY2Md1BzFrUfzaJCnhzphTHFVOh7wizOIca2TzVwiEZPqxpY%2Fil%2FemOzkcjDiygcCsPYzAxyIknBktZmmZU3ejBmQQzfv8cBZllWgWGdV3YbZNvMFzfl4ueEQcBZL3DSXvX9Awr7oDSgQFlI4u3SFGEUBsaaN%2Bfj%2FFoHlyu6r%2BshI4ATGy%2F16%2BMpFokoGUJCXkghqx445Pz70n5lafSDL%2BYfIM%2Brp009XZZzkR8TzL15G9Uc1YAmh5fHLfnDYRDUiuzNJfkVntVvRR7DFtUW%2BgcGST1203uO%2BohrSMVVKw5st%2FV%2B%2F3%2FnXiJ5HyaTYJ7I7LPYwaMxBttrD5XdgRKhnaHNMKgVzLm4cQDm3B9qovw0q1TG2TFM7IK0N3CRnHV8v4piP5tAq0BS4p2GYXBp2uvQe6uTRgjvHRC91T2m7P7KUBKWwix%2B3En0CKfhpRijdLY9yAJlW99TCGh87UBjqkAYd2Qd3WKCWPJZ91ZSBjDEn6aU7s17AnGAedq8gS5zHiA4gpgfYCawc9jQ1qeRmxRuQfEHZbNkP55YdKsVOIWeAdyiZ4XNNdL%2FwYrMpjdrK5gUHQO6uO%2Bf6b2542hwuKhctIotcg2t8RIT0vDNiOPVcxfvr9rxe6KkKDCJGHcydLH3qlGtmOo8amyH%2FT09B6KGWzvr0MEznvbOtfZ0CYYrlpUvxL&X-Amz-Signature=1d47f58693a132809bc704636e9f914c4b5fd7c83d2449318baf361c1ba1bb93&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
