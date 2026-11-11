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
fetched_at: '2026-11-11T03:43:57.988Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IPV6X22%2F20261111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261111T034353Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBlxwILSsLpxTQy%2Fy4JYjKaSjscUxZ85iXPbx%2F51ujF2AiAD2NBqYrZuDdxGADBMh1wx2%2BKGhCkZDxlrmg2w3%2BSO6Sr%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMzwW313gi24F2XFKRKtwDWpvqmionVt4PlodswDQgefm5KAZN2%2BFJk0mH20A08Kbkv2SpA7Pko3jjZcc1B64L9PjYJ9%2F3LikL%2B%2BVGxya0rXHqCvB1wTr75gplm2Zojo7Rc8VFheH70HirGe6ueTDb94xqrMYVB6gimqp0xEFH%2FIExbXqPjGqhpCm%2BcVVc8IMV4Z9xX5%2B6Mn2UbzOCZatgi1f%2BbcL68za2B6bB7SpAhQ7sEObmSwkvLIt0aVO1Zmrz5te2k75zPS9c1fYj%2FMSGQW5QDv9hf0RsENB4uT3bGYuEH2%2FuGwFRta0oyheDXUjgQ3Df1j3OzhXzQMDKd1N32xop8S4FU6w3JOi12L53L9y6wgXBaNM25DG7J0LXL%2FhRaeBuQ72YGwDlZJvET%2FzfBU9C1mbRjQJFNVHQivXoAc0pcxX1QB3kn9D5rDQuXAf1F5cdzQjOZI4D%2B4mSroCvlD11fpNzXTSWBe%2FJqvm7z7JvPu%2BT4yUzA7L6CfRJRxtTU2nX8S5Fk9fzDWOKmBvu14eZANd8pEheAb8AO0Z93PcdGv5mQ4Bx10UiPHU1UsE52xiZFUx0cSD2T97z0FxVTabr8SgH%2F89aMXa16E7965rZawunRFmtI8y14oF7h3vkeetGEXduEIwolVYwz4DP1wY6pgGchsp6Uh8vlycWzvo3GJy7U1AGLocBTpcf%2Ff6iAyOEhVzjwO4NqOQ%2FuqNKpMKTr6q269AAVtT4u8EELQwKrJ8zlQM67LyBJtMFDAdvhEH6Cf5ivg8ysQtGjF80t4qm%2Bs6MzlJMCN8uodYeTLhPxk3Xmzf4jnkJ4tFD4RClb%2Fbf31%2B3uF%2Bn8NEJlsCbVtjWBSfoXSc%2B5k%2BfrOEcM31AkMM36jGSBx8d&X-Amz-Signature=6f727146819985be48ae88c194cbfeaba8ab4deef163cf12d15010d02edc9f0e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IPV6X22%2F20261111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261111T034353Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBlxwILSsLpxTQy%2Fy4JYjKaSjscUxZ85iXPbx%2F51ujF2AiAD2NBqYrZuDdxGADBMh1wx2%2BKGhCkZDxlrmg2w3%2BSO6Sr%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMzwW313gi24F2XFKRKtwDWpvqmionVt4PlodswDQgefm5KAZN2%2BFJk0mH20A08Kbkv2SpA7Pko3jjZcc1B64L9PjYJ9%2F3LikL%2B%2BVGxya0rXHqCvB1wTr75gplm2Zojo7Rc8VFheH70HirGe6ueTDb94xqrMYVB6gimqp0xEFH%2FIExbXqPjGqhpCm%2BcVVc8IMV4Z9xX5%2B6Mn2UbzOCZatgi1f%2BbcL68za2B6bB7SpAhQ7sEObmSwkvLIt0aVO1Zmrz5te2k75zPS9c1fYj%2FMSGQW5QDv9hf0RsENB4uT3bGYuEH2%2FuGwFRta0oyheDXUjgQ3Df1j3OzhXzQMDKd1N32xop8S4FU6w3JOi12L53L9y6wgXBaNM25DG7J0LXL%2FhRaeBuQ72YGwDlZJvET%2FzfBU9C1mbRjQJFNVHQivXoAc0pcxX1QB3kn9D5rDQuXAf1F5cdzQjOZI4D%2B4mSroCvlD11fpNzXTSWBe%2FJqvm7z7JvPu%2BT4yUzA7L6CfRJRxtTU2nX8S5Fk9fzDWOKmBvu14eZANd8pEheAb8AO0Z93PcdGv5mQ4Bx10UiPHU1UsE52xiZFUx0cSD2T97z0FxVTabr8SgH%2F89aMXa16E7965rZawunRFmtI8y14oF7h3vkeetGEXduEIwolVYwz4DP1wY6pgGchsp6Uh8vlycWzvo3GJy7U1AGLocBTpcf%2Ff6iAyOEhVzjwO4NqOQ%2FuqNKpMKTr6q269AAVtT4u8EELQwKrJ8zlQM67LyBJtMFDAdvhEH6Cf5ivg8ysQtGjF80t4qm%2Bs6MzlJMCN8uodYeTLhPxk3Xmzf4jnkJ4tFD4RClb%2Fbf31%2B3uF%2Bn8NEJlsCbVtjWBSfoXSc%2B5k%2BfrOEcM31AkMM36jGSBx8d&X-Amz-Signature=e59395d79e808b16d69d9248f9017aca7b2b2f3b66c5f34f41390ec1f98c60cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IPV6X22%2F20261111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261111T034353Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBlxwILSsLpxTQy%2Fy4JYjKaSjscUxZ85iXPbx%2F51ujF2AiAD2NBqYrZuDdxGADBMh1wx2%2BKGhCkZDxlrmg2w3%2BSO6Sr%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMzwW313gi24F2XFKRKtwDWpvqmionVt4PlodswDQgefm5KAZN2%2BFJk0mH20A08Kbkv2SpA7Pko3jjZcc1B64L9PjYJ9%2F3LikL%2B%2BVGxya0rXHqCvB1wTr75gplm2Zojo7Rc8VFheH70HirGe6ueTDb94xqrMYVB6gimqp0xEFH%2FIExbXqPjGqhpCm%2BcVVc8IMV4Z9xX5%2B6Mn2UbzOCZatgi1f%2BbcL68za2B6bB7SpAhQ7sEObmSwkvLIt0aVO1Zmrz5te2k75zPS9c1fYj%2FMSGQW5QDv9hf0RsENB4uT3bGYuEH2%2FuGwFRta0oyheDXUjgQ3Df1j3OzhXzQMDKd1N32xop8S4FU6w3JOi12L53L9y6wgXBaNM25DG7J0LXL%2FhRaeBuQ72YGwDlZJvET%2FzfBU9C1mbRjQJFNVHQivXoAc0pcxX1QB3kn9D5rDQuXAf1F5cdzQjOZI4D%2B4mSroCvlD11fpNzXTSWBe%2FJqvm7z7JvPu%2BT4yUzA7L6CfRJRxtTU2nX8S5Fk9fzDWOKmBvu14eZANd8pEheAb8AO0Z93PcdGv5mQ4Bx10UiPHU1UsE52xiZFUx0cSD2T97z0FxVTabr8SgH%2F89aMXa16E7965rZawunRFmtI8y14oF7h3vkeetGEXduEIwolVYwz4DP1wY6pgGchsp6Uh8vlycWzvo3GJy7U1AGLocBTpcf%2Ff6iAyOEhVzjwO4NqOQ%2FuqNKpMKTr6q269AAVtT4u8EELQwKrJ8zlQM67LyBJtMFDAdvhEH6Cf5ivg8ysQtGjF80t4qm%2Bs6MzlJMCN8uodYeTLhPxk3Xmzf4jnkJ4tFD4RClb%2Fbf31%2B3uF%2Bn8NEJlsCbVtjWBSfoXSc%2B5k%2BfrOEcM31AkMM36jGSBx8d&X-Amz-Signature=14df9c11b42ac5041f9faab8b05709d9dd7413fa39ad8171ad8da110a4ff6a7f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IPV6X22%2F20261111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261111T034353Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBlxwILSsLpxTQy%2Fy4JYjKaSjscUxZ85iXPbx%2F51ujF2AiAD2NBqYrZuDdxGADBMh1wx2%2BKGhCkZDxlrmg2w3%2BSO6Sr%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMzwW313gi24F2XFKRKtwDWpvqmionVt4PlodswDQgefm5KAZN2%2BFJk0mH20A08Kbkv2SpA7Pko3jjZcc1B64L9PjYJ9%2F3LikL%2B%2BVGxya0rXHqCvB1wTr75gplm2Zojo7Rc8VFheH70HirGe6ueTDb94xqrMYVB6gimqp0xEFH%2FIExbXqPjGqhpCm%2BcVVc8IMV4Z9xX5%2B6Mn2UbzOCZatgi1f%2BbcL68za2B6bB7SpAhQ7sEObmSwkvLIt0aVO1Zmrz5te2k75zPS9c1fYj%2FMSGQW5QDv9hf0RsENB4uT3bGYuEH2%2FuGwFRta0oyheDXUjgQ3Df1j3OzhXzQMDKd1N32xop8S4FU6w3JOi12L53L9y6wgXBaNM25DG7J0LXL%2FhRaeBuQ72YGwDlZJvET%2FzfBU9C1mbRjQJFNVHQivXoAc0pcxX1QB3kn9D5rDQuXAf1F5cdzQjOZI4D%2B4mSroCvlD11fpNzXTSWBe%2FJqvm7z7JvPu%2BT4yUzA7L6CfRJRxtTU2nX8S5Fk9fzDWOKmBvu14eZANd8pEheAb8AO0Z93PcdGv5mQ4Bx10UiPHU1UsE52xiZFUx0cSD2T97z0FxVTabr8SgH%2F89aMXa16E7965rZawunRFmtI8y14oF7h3vkeetGEXduEIwolVYwz4DP1wY6pgGchsp6Uh8vlycWzvo3GJy7U1AGLocBTpcf%2Ff6iAyOEhVzjwO4NqOQ%2FuqNKpMKTr6q269AAVtT4u8EELQwKrJ8zlQM67LyBJtMFDAdvhEH6Cf5ivg8ysQtGjF80t4qm%2Bs6MzlJMCN8uodYeTLhPxk3Xmzf4jnkJ4tFD4RClb%2Fbf31%2B3uF%2Bn8NEJlsCbVtjWBSfoXSc%2B5k%2BfrOEcM31AkMM36jGSBx8d&X-Amz-Signature=333293ed7471a531ebfc7de1bfd5a964cf6188efa38b637a2269fec38c40376b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IPV6X22%2F20261111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261111T034353Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBlxwILSsLpxTQy%2Fy4JYjKaSjscUxZ85iXPbx%2F51ujF2AiAD2NBqYrZuDdxGADBMh1wx2%2BKGhCkZDxlrmg2w3%2BSO6Sr%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMzwW313gi24F2XFKRKtwDWpvqmionVt4PlodswDQgefm5KAZN2%2BFJk0mH20A08Kbkv2SpA7Pko3jjZcc1B64L9PjYJ9%2F3LikL%2B%2BVGxya0rXHqCvB1wTr75gplm2Zojo7Rc8VFheH70HirGe6ueTDb94xqrMYVB6gimqp0xEFH%2FIExbXqPjGqhpCm%2BcVVc8IMV4Z9xX5%2B6Mn2UbzOCZatgi1f%2BbcL68za2B6bB7SpAhQ7sEObmSwkvLIt0aVO1Zmrz5te2k75zPS9c1fYj%2FMSGQW5QDv9hf0RsENB4uT3bGYuEH2%2FuGwFRta0oyheDXUjgQ3Df1j3OzhXzQMDKd1N32xop8S4FU6w3JOi12L53L9y6wgXBaNM25DG7J0LXL%2FhRaeBuQ72YGwDlZJvET%2FzfBU9C1mbRjQJFNVHQivXoAc0pcxX1QB3kn9D5rDQuXAf1F5cdzQjOZI4D%2B4mSroCvlD11fpNzXTSWBe%2FJqvm7z7JvPu%2BT4yUzA7L6CfRJRxtTU2nX8S5Fk9fzDWOKmBvu14eZANd8pEheAb8AO0Z93PcdGv5mQ4Bx10UiPHU1UsE52xiZFUx0cSD2T97z0FxVTabr8SgH%2F89aMXa16E7965rZawunRFmtI8y14oF7h3vkeetGEXduEIwolVYwz4DP1wY6pgGchsp6Uh8vlycWzvo3GJy7U1AGLocBTpcf%2Ff6iAyOEhVzjwO4NqOQ%2FuqNKpMKTr6q269AAVtT4u8EELQwKrJ8zlQM67LyBJtMFDAdvhEH6Cf5ivg8ysQtGjF80t4qm%2Bs6MzlJMCN8uodYeTLhPxk3Xmzf4jnkJ4tFD4RClb%2Fbf31%2B3uF%2Bn8NEJlsCbVtjWBSfoXSc%2B5k%2BfrOEcM31AkMM36jGSBx8d&X-Amz-Signature=d2be84c7ecbce230341fa136e6c5f7a49d117af65df9d6a0e3860ef0ec47739c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IPV6X22%2F20261111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261111T034353Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBlxwILSsLpxTQy%2Fy4JYjKaSjscUxZ85iXPbx%2F51ujF2AiAD2NBqYrZuDdxGADBMh1wx2%2BKGhCkZDxlrmg2w3%2BSO6Sr%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMzwW313gi24F2XFKRKtwDWpvqmionVt4PlodswDQgefm5KAZN2%2BFJk0mH20A08Kbkv2SpA7Pko3jjZcc1B64L9PjYJ9%2F3LikL%2B%2BVGxya0rXHqCvB1wTr75gplm2Zojo7Rc8VFheH70HirGe6ueTDb94xqrMYVB6gimqp0xEFH%2FIExbXqPjGqhpCm%2BcVVc8IMV4Z9xX5%2B6Mn2UbzOCZatgi1f%2BbcL68za2B6bB7SpAhQ7sEObmSwkvLIt0aVO1Zmrz5te2k75zPS9c1fYj%2FMSGQW5QDv9hf0RsENB4uT3bGYuEH2%2FuGwFRta0oyheDXUjgQ3Df1j3OzhXzQMDKd1N32xop8S4FU6w3JOi12L53L9y6wgXBaNM25DG7J0LXL%2FhRaeBuQ72YGwDlZJvET%2FzfBU9C1mbRjQJFNVHQivXoAc0pcxX1QB3kn9D5rDQuXAf1F5cdzQjOZI4D%2B4mSroCvlD11fpNzXTSWBe%2FJqvm7z7JvPu%2BT4yUzA7L6CfRJRxtTU2nX8S5Fk9fzDWOKmBvu14eZANd8pEheAb8AO0Z93PcdGv5mQ4Bx10UiPHU1UsE52xiZFUx0cSD2T97z0FxVTabr8SgH%2F89aMXa16E7965rZawunRFmtI8y14oF7h3vkeetGEXduEIwolVYwz4DP1wY6pgGchsp6Uh8vlycWzvo3GJy7U1AGLocBTpcf%2Ff6iAyOEhVzjwO4NqOQ%2FuqNKpMKTr6q269AAVtT4u8EELQwKrJ8zlQM67LyBJtMFDAdvhEH6Cf5ivg8ysQtGjF80t4qm%2Bs6MzlJMCN8uodYeTLhPxk3Xmzf4jnkJ4tFD4RClb%2Fbf31%2B3uF%2Bn8NEJlsCbVtjWBSfoXSc%2B5k%2BfrOEcM31AkMM36jGSBx8d&X-Amz-Signature=9006b45410c2c48ae678d0e0aa9580a34b976a0502ab5ac61a5c57ba5eaa4d95&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IPV6X22%2F20261111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261111T034353Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBlxwILSsLpxTQy%2Fy4JYjKaSjscUxZ85iXPbx%2F51ujF2AiAD2NBqYrZuDdxGADBMh1wx2%2BKGhCkZDxlrmg2w3%2BSO6Sr%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMzwW313gi24F2XFKRKtwDWpvqmionVt4PlodswDQgefm5KAZN2%2BFJk0mH20A08Kbkv2SpA7Pko3jjZcc1B64L9PjYJ9%2F3LikL%2B%2BVGxya0rXHqCvB1wTr75gplm2Zojo7Rc8VFheH70HirGe6ueTDb94xqrMYVB6gimqp0xEFH%2FIExbXqPjGqhpCm%2BcVVc8IMV4Z9xX5%2B6Mn2UbzOCZatgi1f%2BbcL68za2B6bB7SpAhQ7sEObmSwkvLIt0aVO1Zmrz5te2k75zPS9c1fYj%2FMSGQW5QDv9hf0RsENB4uT3bGYuEH2%2FuGwFRta0oyheDXUjgQ3Df1j3OzhXzQMDKd1N32xop8S4FU6w3JOi12L53L9y6wgXBaNM25DG7J0LXL%2FhRaeBuQ72YGwDlZJvET%2FzfBU9C1mbRjQJFNVHQivXoAc0pcxX1QB3kn9D5rDQuXAf1F5cdzQjOZI4D%2B4mSroCvlD11fpNzXTSWBe%2FJqvm7z7JvPu%2BT4yUzA7L6CfRJRxtTU2nX8S5Fk9fzDWOKmBvu14eZANd8pEheAb8AO0Z93PcdGv5mQ4Bx10UiPHU1UsE52xiZFUx0cSD2T97z0FxVTabr8SgH%2F89aMXa16E7965rZawunRFmtI8y14oF7h3vkeetGEXduEIwolVYwz4DP1wY6pgGchsp6Uh8vlycWzvo3GJy7U1AGLocBTpcf%2Ff6iAyOEhVzjwO4NqOQ%2FuqNKpMKTr6q269AAVtT4u8EELQwKrJ8zlQM67LyBJtMFDAdvhEH6Cf5ivg8ysQtGjF80t4qm%2Bs6MzlJMCN8uodYeTLhPxk3Xmzf4jnkJ4tFD4RClb%2Fbf31%2B3uF%2Bn8NEJlsCbVtjWBSfoXSc%2B5k%2BfrOEcM31AkMM36jGSBx8d&X-Amz-Signature=cba43b31244d4f59e6bbfad3ea0bd4614d422b9177b61c1c9da1738a8eccb7a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IPV6X22%2F20261111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261111T034353Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBlxwILSsLpxTQy%2Fy4JYjKaSjscUxZ85iXPbx%2F51ujF2AiAD2NBqYrZuDdxGADBMh1wx2%2BKGhCkZDxlrmg2w3%2BSO6Sr%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMzwW313gi24F2XFKRKtwDWpvqmionVt4PlodswDQgefm5KAZN2%2BFJk0mH20A08Kbkv2SpA7Pko3jjZcc1B64L9PjYJ9%2F3LikL%2B%2BVGxya0rXHqCvB1wTr75gplm2Zojo7Rc8VFheH70HirGe6ueTDb94xqrMYVB6gimqp0xEFH%2FIExbXqPjGqhpCm%2BcVVc8IMV4Z9xX5%2B6Mn2UbzOCZatgi1f%2BbcL68za2B6bB7SpAhQ7sEObmSwkvLIt0aVO1Zmrz5te2k75zPS9c1fYj%2FMSGQW5QDv9hf0RsENB4uT3bGYuEH2%2FuGwFRta0oyheDXUjgQ3Df1j3OzhXzQMDKd1N32xop8S4FU6w3JOi12L53L9y6wgXBaNM25DG7J0LXL%2FhRaeBuQ72YGwDlZJvET%2FzfBU9C1mbRjQJFNVHQivXoAc0pcxX1QB3kn9D5rDQuXAf1F5cdzQjOZI4D%2B4mSroCvlD11fpNzXTSWBe%2FJqvm7z7JvPu%2BT4yUzA7L6CfRJRxtTU2nX8S5Fk9fzDWOKmBvu14eZANd8pEheAb8AO0Z93PcdGv5mQ4Bx10UiPHU1UsE52xiZFUx0cSD2T97z0FxVTabr8SgH%2F89aMXa16E7965rZawunRFmtI8y14oF7h3vkeetGEXduEIwolVYwz4DP1wY6pgGchsp6Uh8vlycWzvo3GJy7U1AGLocBTpcf%2Ff6iAyOEhVzjwO4NqOQ%2FuqNKpMKTr6q269AAVtT4u8EELQwKrJ8zlQM67LyBJtMFDAdvhEH6Cf5ivg8ysQtGjF80t4qm%2Bs6MzlJMCN8uodYeTLhPxk3Xmzf4jnkJ4tFD4RClb%2Fbf31%2B3uF%2Bn8NEJlsCbVtjWBSfoXSc%2B5k%2BfrOEcM31AkMM36jGSBx8d&X-Amz-Signature=1d61d317d07cbdf949484da97d6602de0b48f7ffd0d367b49ace18f26e861582&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IPV6X22%2F20261111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261111T034353Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBlxwILSsLpxTQy%2Fy4JYjKaSjscUxZ85iXPbx%2F51ujF2AiAD2NBqYrZuDdxGADBMh1wx2%2BKGhCkZDxlrmg2w3%2BSO6Sr%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMzwW313gi24F2XFKRKtwDWpvqmionVt4PlodswDQgefm5KAZN2%2BFJk0mH20A08Kbkv2SpA7Pko3jjZcc1B64L9PjYJ9%2F3LikL%2B%2BVGxya0rXHqCvB1wTr75gplm2Zojo7Rc8VFheH70HirGe6ueTDb94xqrMYVB6gimqp0xEFH%2FIExbXqPjGqhpCm%2BcVVc8IMV4Z9xX5%2B6Mn2UbzOCZatgi1f%2BbcL68za2B6bB7SpAhQ7sEObmSwkvLIt0aVO1Zmrz5te2k75zPS9c1fYj%2FMSGQW5QDv9hf0RsENB4uT3bGYuEH2%2FuGwFRta0oyheDXUjgQ3Df1j3OzhXzQMDKd1N32xop8S4FU6w3JOi12L53L9y6wgXBaNM25DG7J0LXL%2FhRaeBuQ72YGwDlZJvET%2FzfBU9C1mbRjQJFNVHQivXoAc0pcxX1QB3kn9D5rDQuXAf1F5cdzQjOZI4D%2B4mSroCvlD11fpNzXTSWBe%2FJqvm7z7JvPu%2BT4yUzA7L6CfRJRxtTU2nX8S5Fk9fzDWOKmBvu14eZANd8pEheAb8AO0Z93PcdGv5mQ4Bx10UiPHU1UsE52xiZFUx0cSD2T97z0FxVTabr8SgH%2F89aMXa16E7965rZawunRFmtI8y14oF7h3vkeetGEXduEIwolVYwz4DP1wY6pgGchsp6Uh8vlycWzvo3GJy7U1AGLocBTpcf%2Ff6iAyOEhVzjwO4NqOQ%2FuqNKpMKTr6q269AAVtT4u8EELQwKrJ8zlQM67LyBJtMFDAdvhEH6Cf5ivg8ysQtGjF80t4qm%2Bs6MzlJMCN8uodYeTLhPxk3Xmzf4jnkJ4tFD4RClb%2Fbf31%2B3uF%2Bn8NEJlsCbVtjWBSfoXSc%2B5k%2BfrOEcM31AkMM36jGSBx8d&X-Amz-Signature=0e45c3322545ff4da7d7b212d40c470775625bd177496eec892b6abff71b50a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
