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
fetched_at: '2026-09-13T02:01:38.239Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664J4P4RK7%2F20260913%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260913T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCS0khI%2FmoD%2FkcIIrNWsosHZD%2FvDANnZa3gbDigCxnd1gIhAO94h2643woqwJxwp7ufPVyDnQRJTXBOC9okJqmGIzkSKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwh5Sh0xns8DoPtSXwq3AM7RuKFJdRlrccfJBxQka88MF6rm9WqXGbx6z5WM9Ii1HhIe8tZwu9lrVn6Xhqeutgt4sFYtdsaif%2FDdTzgQoSUWWE47sJ8WE%2FqQrnUFNt1OHikCC5wX2fAFAuQi7K7L0gnf3xHm6lRJ3GyOs5BFWYMqej%2BtZgFaO8MWkl9ZHafJAwvl%2B5rdsulIjT7z7H1EZQgyH6vHrY8W1ZZuK%2BSwjkhivoC7B62b7kLxsyRQRdKMqBjERn1xBB0zdzZeXL1h%2BOAmdEECgK9XUG8S%2BNigHhqfYMWe7Su8jdsSKqLFCgRWr%2BB0vW2q7t5%2FqkrJExCsioXEMYTA7rSvZcMIyVl4Y%2FBeuJSscUZx9HXNdj98c6HU%2BrQXJFE%2B5KIjelSS3JQyhJHHDfzNtKVBSn9cB8oHLw8X%2BCmVaXHGh6gIjkzc%2FynamFZyFxkUcJZ5Tr8xjCLasfQPZN14t4T0k43dibKmmtxHp1sGt4Df8mkSmqSQHIX8sihSvIQUhrOFbvf0eQABToMr3JurJX6UnNgIe3D2m%2BYeMLpZx%2FPbuhecbnVaspSexSMx%2FGS4YQo%2Fuvt08EFXUawADZWs8S5dWaIqoQQyP7CZccHf%2FkuvkR3fzvSUXWGz0kqAy6JsYlab%2FmEmjDT%2FpfVBjqkAbPHl9o2B5gTJ4KE7Jf1xndudUvQ%2FS5Gz2bFEHQw307sN2HCSp86DlzekqoSFptUCW7oEglVOHIHoNFs44HPg4pftJ6%2FsQJNp229AyX13EYM0ib5MB4n1y0tbnnUJyvs1ChR9QZZ0IIecvpwS%2Bf4fFv9pdYen0oMK8PPTYeRmeB5eksxRS9Fe1whtEU7DKHFfxGKvdsaFF4r3EmmxuSGSdBWjTRu&X-Amz-Signature=8e98dc4bd379a78db0d7af9751a0bd963d926115dc723a1e01b3c0f3141af491&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664J4P4RK7%2F20260913%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260913T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCS0khI%2FmoD%2FkcIIrNWsosHZD%2FvDANnZa3gbDigCxnd1gIhAO94h2643woqwJxwp7ufPVyDnQRJTXBOC9okJqmGIzkSKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwh5Sh0xns8DoPtSXwq3AM7RuKFJdRlrccfJBxQka88MF6rm9WqXGbx6z5WM9Ii1HhIe8tZwu9lrVn6Xhqeutgt4sFYtdsaif%2FDdTzgQoSUWWE47sJ8WE%2FqQrnUFNt1OHikCC5wX2fAFAuQi7K7L0gnf3xHm6lRJ3GyOs5BFWYMqej%2BtZgFaO8MWkl9ZHafJAwvl%2B5rdsulIjT7z7H1EZQgyH6vHrY8W1ZZuK%2BSwjkhivoC7B62b7kLxsyRQRdKMqBjERn1xBB0zdzZeXL1h%2BOAmdEECgK9XUG8S%2BNigHhqfYMWe7Su8jdsSKqLFCgRWr%2BB0vW2q7t5%2FqkrJExCsioXEMYTA7rSvZcMIyVl4Y%2FBeuJSscUZx9HXNdj98c6HU%2BrQXJFE%2B5KIjelSS3JQyhJHHDfzNtKVBSn9cB8oHLw8X%2BCmVaXHGh6gIjkzc%2FynamFZyFxkUcJZ5Tr8xjCLasfQPZN14t4T0k43dibKmmtxHp1sGt4Df8mkSmqSQHIX8sihSvIQUhrOFbvf0eQABToMr3JurJX6UnNgIe3D2m%2BYeMLpZx%2FPbuhecbnVaspSexSMx%2FGS4YQo%2Fuvt08EFXUawADZWs8S5dWaIqoQQyP7CZccHf%2FkuvkR3fzvSUXWGz0kqAy6JsYlab%2FmEmjDT%2FpfVBjqkAbPHl9o2B5gTJ4KE7Jf1xndudUvQ%2FS5Gz2bFEHQw307sN2HCSp86DlzekqoSFptUCW7oEglVOHIHoNFs44HPg4pftJ6%2FsQJNp229AyX13EYM0ib5MB4n1y0tbnnUJyvs1ChR9QZZ0IIecvpwS%2Bf4fFv9pdYen0oMK8PPTYeRmeB5eksxRS9Fe1whtEU7DKHFfxGKvdsaFF4r3EmmxuSGSdBWjTRu&X-Amz-Signature=cde6f689cd752b24bf68211d07bfb6b669d62d7bcadd4dccb0d3f2950c3d63cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664J4P4RK7%2F20260913%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260913T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCS0khI%2FmoD%2FkcIIrNWsosHZD%2FvDANnZa3gbDigCxnd1gIhAO94h2643woqwJxwp7ufPVyDnQRJTXBOC9okJqmGIzkSKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwh5Sh0xns8DoPtSXwq3AM7RuKFJdRlrccfJBxQka88MF6rm9WqXGbx6z5WM9Ii1HhIe8tZwu9lrVn6Xhqeutgt4sFYtdsaif%2FDdTzgQoSUWWE47sJ8WE%2FqQrnUFNt1OHikCC5wX2fAFAuQi7K7L0gnf3xHm6lRJ3GyOs5BFWYMqej%2BtZgFaO8MWkl9ZHafJAwvl%2B5rdsulIjT7z7H1EZQgyH6vHrY8W1ZZuK%2BSwjkhivoC7B62b7kLxsyRQRdKMqBjERn1xBB0zdzZeXL1h%2BOAmdEECgK9XUG8S%2BNigHhqfYMWe7Su8jdsSKqLFCgRWr%2BB0vW2q7t5%2FqkrJExCsioXEMYTA7rSvZcMIyVl4Y%2FBeuJSscUZx9HXNdj98c6HU%2BrQXJFE%2B5KIjelSS3JQyhJHHDfzNtKVBSn9cB8oHLw8X%2BCmVaXHGh6gIjkzc%2FynamFZyFxkUcJZ5Tr8xjCLasfQPZN14t4T0k43dibKmmtxHp1sGt4Df8mkSmqSQHIX8sihSvIQUhrOFbvf0eQABToMr3JurJX6UnNgIe3D2m%2BYeMLpZx%2FPbuhecbnVaspSexSMx%2FGS4YQo%2Fuvt08EFXUawADZWs8S5dWaIqoQQyP7CZccHf%2FkuvkR3fzvSUXWGz0kqAy6JsYlab%2FmEmjDT%2FpfVBjqkAbPHl9o2B5gTJ4KE7Jf1xndudUvQ%2FS5Gz2bFEHQw307sN2HCSp86DlzekqoSFptUCW7oEglVOHIHoNFs44HPg4pftJ6%2FsQJNp229AyX13EYM0ib5MB4n1y0tbnnUJyvs1ChR9QZZ0IIecvpwS%2Bf4fFv9pdYen0oMK8PPTYeRmeB5eksxRS9Fe1whtEU7DKHFfxGKvdsaFF4r3EmmxuSGSdBWjTRu&X-Amz-Signature=a3dc37a547963e8d0cc4219dca2d0f0d3fae162a1439fcc34cbc942f3c4aa8f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664J4P4RK7%2F20260913%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260913T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCS0khI%2FmoD%2FkcIIrNWsosHZD%2FvDANnZa3gbDigCxnd1gIhAO94h2643woqwJxwp7ufPVyDnQRJTXBOC9okJqmGIzkSKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwh5Sh0xns8DoPtSXwq3AM7RuKFJdRlrccfJBxQka88MF6rm9WqXGbx6z5WM9Ii1HhIe8tZwu9lrVn6Xhqeutgt4sFYtdsaif%2FDdTzgQoSUWWE47sJ8WE%2FqQrnUFNt1OHikCC5wX2fAFAuQi7K7L0gnf3xHm6lRJ3GyOs5BFWYMqej%2BtZgFaO8MWkl9ZHafJAwvl%2B5rdsulIjT7z7H1EZQgyH6vHrY8W1ZZuK%2BSwjkhivoC7B62b7kLxsyRQRdKMqBjERn1xBB0zdzZeXL1h%2BOAmdEECgK9XUG8S%2BNigHhqfYMWe7Su8jdsSKqLFCgRWr%2BB0vW2q7t5%2FqkrJExCsioXEMYTA7rSvZcMIyVl4Y%2FBeuJSscUZx9HXNdj98c6HU%2BrQXJFE%2B5KIjelSS3JQyhJHHDfzNtKVBSn9cB8oHLw8X%2BCmVaXHGh6gIjkzc%2FynamFZyFxkUcJZ5Tr8xjCLasfQPZN14t4T0k43dibKmmtxHp1sGt4Df8mkSmqSQHIX8sihSvIQUhrOFbvf0eQABToMr3JurJX6UnNgIe3D2m%2BYeMLpZx%2FPbuhecbnVaspSexSMx%2FGS4YQo%2Fuvt08EFXUawADZWs8S5dWaIqoQQyP7CZccHf%2FkuvkR3fzvSUXWGz0kqAy6JsYlab%2FmEmjDT%2FpfVBjqkAbPHl9o2B5gTJ4KE7Jf1xndudUvQ%2FS5Gz2bFEHQw307sN2HCSp86DlzekqoSFptUCW7oEglVOHIHoNFs44HPg4pftJ6%2FsQJNp229AyX13EYM0ib5MB4n1y0tbnnUJyvs1ChR9QZZ0IIecvpwS%2Bf4fFv9pdYen0oMK8PPTYeRmeB5eksxRS9Fe1whtEU7DKHFfxGKvdsaFF4r3EmmxuSGSdBWjTRu&X-Amz-Signature=cd388272ce0bd407178f6c782ccd0112815044a867678f61e97e21bb8228c51a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664J4P4RK7%2F20260913%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260913T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCS0khI%2FmoD%2FkcIIrNWsosHZD%2FvDANnZa3gbDigCxnd1gIhAO94h2643woqwJxwp7ufPVyDnQRJTXBOC9okJqmGIzkSKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwh5Sh0xns8DoPtSXwq3AM7RuKFJdRlrccfJBxQka88MF6rm9WqXGbx6z5WM9Ii1HhIe8tZwu9lrVn6Xhqeutgt4sFYtdsaif%2FDdTzgQoSUWWE47sJ8WE%2FqQrnUFNt1OHikCC5wX2fAFAuQi7K7L0gnf3xHm6lRJ3GyOs5BFWYMqej%2BtZgFaO8MWkl9ZHafJAwvl%2B5rdsulIjT7z7H1EZQgyH6vHrY8W1ZZuK%2BSwjkhivoC7B62b7kLxsyRQRdKMqBjERn1xBB0zdzZeXL1h%2BOAmdEECgK9XUG8S%2BNigHhqfYMWe7Su8jdsSKqLFCgRWr%2BB0vW2q7t5%2FqkrJExCsioXEMYTA7rSvZcMIyVl4Y%2FBeuJSscUZx9HXNdj98c6HU%2BrQXJFE%2B5KIjelSS3JQyhJHHDfzNtKVBSn9cB8oHLw8X%2BCmVaXHGh6gIjkzc%2FynamFZyFxkUcJZ5Tr8xjCLasfQPZN14t4T0k43dibKmmtxHp1sGt4Df8mkSmqSQHIX8sihSvIQUhrOFbvf0eQABToMr3JurJX6UnNgIe3D2m%2BYeMLpZx%2FPbuhecbnVaspSexSMx%2FGS4YQo%2Fuvt08EFXUawADZWs8S5dWaIqoQQyP7CZccHf%2FkuvkR3fzvSUXWGz0kqAy6JsYlab%2FmEmjDT%2FpfVBjqkAbPHl9o2B5gTJ4KE7Jf1xndudUvQ%2FS5Gz2bFEHQw307sN2HCSp86DlzekqoSFptUCW7oEglVOHIHoNFs44HPg4pftJ6%2FsQJNp229AyX13EYM0ib5MB4n1y0tbnnUJyvs1ChR9QZZ0IIecvpwS%2Bf4fFv9pdYen0oMK8PPTYeRmeB5eksxRS9Fe1whtEU7DKHFfxGKvdsaFF4r3EmmxuSGSdBWjTRu&X-Amz-Signature=0637944016874e9db59523b8d07f5c389dab64047c493ecbaf28c77c3f872d0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664J4P4RK7%2F20260913%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260913T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCS0khI%2FmoD%2FkcIIrNWsosHZD%2FvDANnZa3gbDigCxnd1gIhAO94h2643woqwJxwp7ufPVyDnQRJTXBOC9okJqmGIzkSKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwh5Sh0xns8DoPtSXwq3AM7RuKFJdRlrccfJBxQka88MF6rm9WqXGbx6z5WM9Ii1HhIe8tZwu9lrVn6Xhqeutgt4sFYtdsaif%2FDdTzgQoSUWWE47sJ8WE%2FqQrnUFNt1OHikCC5wX2fAFAuQi7K7L0gnf3xHm6lRJ3GyOs5BFWYMqej%2BtZgFaO8MWkl9ZHafJAwvl%2B5rdsulIjT7z7H1EZQgyH6vHrY8W1ZZuK%2BSwjkhivoC7B62b7kLxsyRQRdKMqBjERn1xBB0zdzZeXL1h%2BOAmdEECgK9XUG8S%2BNigHhqfYMWe7Su8jdsSKqLFCgRWr%2BB0vW2q7t5%2FqkrJExCsioXEMYTA7rSvZcMIyVl4Y%2FBeuJSscUZx9HXNdj98c6HU%2BrQXJFE%2B5KIjelSS3JQyhJHHDfzNtKVBSn9cB8oHLw8X%2BCmVaXHGh6gIjkzc%2FynamFZyFxkUcJZ5Tr8xjCLasfQPZN14t4T0k43dibKmmtxHp1sGt4Df8mkSmqSQHIX8sihSvIQUhrOFbvf0eQABToMr3JurJX6UnNgIe3D2m%2BYeMLpZx%2FPbuhecbnVaspSexSMx%2FGS4YQo%2Fuvt08EFXUawADZWs8S5dWaIqoQQyP7CZccHf%2FkuvkR3fzvSUXWGz0kqAy6JsYlab%2FmEmjDT%2FpfVBjqkAbPHl9o2B5gTJ4KE7Jf1xndudUvQ%2FS5Gz2bFEHQw307sN2HCSp86DlzekqoSFptUCW7oEglVOHIHoNFs44HPg4pftJ6%2FsQJNp229AyX13EYM0ib5MB4n1y0tbnnUJyvs1ChR9QZZ0IIecvpwS%2Bf4fFv9pdYen0oMK8PPTYeRmeB5eksxRS9Fe1whtEU7DKHFfxGKvdsaFF4r3EmmxuSGSdBWjTRu&X-Amz-Signature=57ee954d26f8c03f1358e5993d44118a6733a3dfe6f9876957cc8d78dd4ec246&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664J4P4RK7%2F20260913%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260913T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCS0khI%2FmoD%2FkcIIrNWsosHZD%2FvDANnZa3gbDigCxnd1gIhAO94h2643woqwJxwp7ufPVyDnQRJTXBOC9okJqmGIzkSKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwh5Sh0xns8DoPtSXwq3AM7RuKFJdRlrccfJBxQka88MF6rm9WqXGbx6z5WM9Ii1HhIe8tZwu9lrVn6Xhqeutgt4sFYtdsaif%2FDdTzgQoSUWWE47sJ8WE%2FqQrnUFNt1OHikCC5wX2fAFAuQi7K7L0gnf3xHm6lRJ3GyOs5BFWYMqej%2BtZgFaO8MWkl9ZHafJAwvl%2B5rdsulIjT7z7H1EZQgyH6vHrY8W1ZZuK%2BSwjkhivoC7B62b7kLxsyRQRdKMqBjERn1xBB0zdzZeXL1h%2BOAmdEECgK9XUG8S%2BNigHhqfYMWe7Su8jdsSKqLFCgRWr%2BB0vW2q7t5%2FqkrJExCsioXEMYTA7rSvZcMIyVl4Y%2FBeuJSscUZx9HXNdj98c6HU%2BrQXJFE%2B5KIjelSS3JQyhJHHDfzNtKVBSn9cB8oHLw8X%2BCmVaXHGh6gIjkzc%2FynamFZyFxkUcJZ5Tr8xjCLasfQPZN14t4T0k43dibKmmtxHp1sGt4Df8mkSmqSQHIX8sihSvIQUhrOFbvf0eQABToMr3JurJX6UnNgIe3D2m%2BYeMLpZx%2FPbuhecbnVaspSexSMx%2FGS4YQo%2Fuvt08EFXUawADZWs8S5dWaIqoQQyP7CZccHf%2FkuvkR3fzvSUXWGz0kqAy6JsYlab%2FmEmjDT%2FpfVBjqkAbPHl9o2B5gTJ4KE7Jf1xndudUvQ%2FS5Gz2bFEHQw307sN2HCSp86DlzekqoSFptUCW7oEglVOHIHoNFs44HPg4pftJ6%2FsQJNp229AyX13EYM0ib5MB4n1y0tbnnUJyvs1ChR9QZZ0IIecvpwS%2Bf4fFv9pdYen0oMK8PPTYeRmeB5eksxRS9Fe1whtEU7DKHFfxGKvdsaFF4r3EmmxuSGSdBWjTRu&X-Amz-Signature=bf93fc409803bc9508b5975ef0bf15d3109fd56fba5696e4c9ec744718c93f54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664J4P4RK7%2F20260913%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260913T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCS0khI%2FmoD%2FkcIIrNWsosHZD%2FvDANnZa3gbDigCxnd1gIhAO94h2643woqwJxwp7ufPVyDnQRJTXBOC9okJqmGIzkSKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwh5Sh0xns8DoPtSXwq3AM7RuKFJdRlrccfJBxQka88MF6rm9WqXGbx6z5WM9Ii1HhIe8tZwu9lrVn6Xhqeutgt4sFYtdsaif%2FDdTzgQoSUWWE47sJ8WE%2FqQrnUFNt1OHikCC5wX2fAFAuQi7K7L0gnf3xHm6lRJ3GyOs5BFWYMqej%2BtZgFaO8MWkl9ZHafJAwvl%2B5rdsulIjT7z7H1EZQgyH6vHrY8W1ZZuK%2BSwjkhivoC7B62b7kLxsyRQRdKMqBjERn1xBB0zdzZeXL1h%2BOAmdEECgK9XUG8S%2BNigHhqfYMWe7Su8jdsSKqLFCgRWr%2BB0vW2q7t5%2FqkrJExCsioXEMYTA7rSvZcMIyVl4Y%2FBeuJSscUZx9HXNdj98c6HU%2BrQXJFE%2B5KIjelSS3JQyhJHHDfzNtKVBSn9cB8oHLw8X%2BCmVaXHGh6gIjkzc%2FynamFZyFxkUcJZ5Tr8xjCLasfQPZN14t4T0k43dibKmmtxHp1sGt4Df8mkSmqSQHIX8sihSvIQUhrOFbvf0eQABToMr3JurJX6UnNgIe3D2m%2BYeMLpZx%2FPbuhecbnVaspSexSMx%2FGS4YQo%2Fuvt08EFXUawADZWs8S5dWaIqoQQyP7CZccHf%2FkuvkR3fzvSUXWGz0kqAy6JsYlab%2FmEmjDT%2FpfVBjqkAbPHl9o2B5gTJ4KE7Jf1xndudUvQ%2FS5Gz2bFEHQw307sN2HCSp86DlzekqoSFptUCW7oEglVOHIHoNFs44HPg4pftJ6%2FsQJNp229AyX13EYM0ib5MB4n1y0tbnnUJyvs1ChR9QZZ0IIecvpwS%2Bf4fFv9pdYen0oMK8PPTYeRmeB5eksxRS9Fe1whtEU7DKHFfxGKvdsaFF4r3EmmxuSGSdBWjTRu&X-Amz-Signature=b64f54a64249ff73500e988570ca212caa805fb2d05b03cfc16279a3ab5c2540&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664J4P4RK7%2F20260913%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260913T020132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCS0khI%2FmoD%2FkcIIrNWsosHZD%2FvDANnZa3gbDigCxnd1gIhAO94h2643woqwJxwp7ufPVyDnQRJTXBOC9okJqmGIzkSKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwh5Sh0xns8DoPtSXwq3AM7RuKFJdRlrccfJBxQka88MF6rm9WqXGbx6z5WM9Ii1HhIe8tZwu9lrVn6Xhqeutgt4sFYtdsaif%2FDdTzgQoSUWWE47sJ8WE%2FqQrnUFNt1OHikCC5wX2fAFAuQi7K7L0gnf3xHm6lRJ3GyOs5BFWYMqej%2BtZgFaO8MWkl9ZHafJAwvl%2B5rdsulIjT7z7H1EZQgyH6vHrY8W1ZZuK%2BSwjkhivoC7B62b7kLxsyRQRdKMqBjERn1xBB0zdzZeXL1h%2BOAmdEECgK9XUG8S%2BNigHhqfYMWe7Su8jdsSKqLFCgRWr%2BB0vW2q7t5%2FqkrJExCsioXEMYTA7rSvZcMIyVl4Y%2FBeuJSscUZx9HXNdj98c6HU%2BrQXJFE%2B5KIjelSS3JQyhJHHDfzNtKVBSn9cB8oHLw8X%2BCmVaXHGh6gIjkzc%2FynamFZyFxkUcJZ5Tr8xjCLasfQPZN14t4T0k43dibKmmtxHp1sGt4Df8mkSmqSQHIX8sihSvIQUhrOFbvf0eQABToMr3JurJX6UnNgIe3D2m%2BYeMLpZx%2FPbuhecbnVaspSexSMx%2FGS4YQo%2Fuvt08EFXUawADZWs8S5dWaIqoQQyP7CZccHf%2FkuvkR3fzvSUXWGz0kqAy6JsYlab%2FmEmjDT%2FpfVBjqkAbPHl9o2B5gTJ4KE7Jf1xndudUvQ%2FS5Gz2bFEHQw307sN2HCSp86DlzekqoSFptUCW7oEglVOHIHoNFs44HPg4pftJ6%2FsQJNp229AyX13EYM0ib5MB4n1y0tbnnUJyvs1ChR9QZZ0IIecvpwS%2Bf4fFv9pdYen0oMK8PPTYeRmeB5eksxRS9Fe1whtEU7DKHFfxGKvdsaFF4r3EmmxuSGSdBWjTRu&X-Amz-Signature=192e27987493088c8c138e17420a3797d9080ecb24f7c0990e0d48e0e7909569&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
