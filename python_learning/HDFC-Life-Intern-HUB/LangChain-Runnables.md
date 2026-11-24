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
fetched_at: '2026-11-24T07:33:55.448Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TWLWTHI6%2F20261124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261124T073350Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHRlsuUDA0OLj%2BKHiGOHq6Qbm3Lh6LogM0fxdqSZPkL3AiBnR2bGbUul%2F3bZ6dMFoSi%2FNaFv7j6nBqpZXOk7G301uCqIBAiG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLh7xNPuO0N%2FBEf9CKtwDhroq%2FkFOn7lDB8mMNC5dy0mL5RCcrUUHTRLj2ML6htgBCwLEeoPgZYolBx5qGAILt4vNn%2BcheDQGi1g01jyrgTxznzWbZla3fhvK92Kjrf%2FQ9cBMCntScxbmlOKSlfHxsiECm%2BUuZt4im1RgD6sXxUT5zja8jyH0RGBTaVH3Zbw5twwr%2FOIqkLX29kpyhDOBtFIoPBGk65GlrgBlAux7J9cvQvs7d6h2Mc8%2BwFxnt4SCIbNNkFeVy%2BQenQlpmA1tax2y3V93pWBS3MFXUYamzW6Z%2B%2BmDfHUK3C84BNZZicw5NjmuPlTPW9hKHkIq1zHXJFAIYOtgyveh7aUpKyiBB98ZdiEc%2Fvw96DkgaU60A8duUyYvIzqHmwM1JV71QHOmFQunCzx1JgayBBn9kpfTHvcTn3SFTTCZ%2BibBOXfyUuDmEs0Ml293%2FDukWY30qmtHz6bsl0GUyMAT5Br8UkApQv9Ys7ki8e3bvE8qhwKqa9mjo1lMWpxVjtxglgH6WyGr8duQC8MMwPQhXwf6Q7aSFj0mYeLzcG377LZOym0d3FIs%2F3wwVxz%2FBfCGdepIiKYiBbcXc0I48l59KXsApTKULwTQZ9lb23MjHirIDdcovH4l3Ou44BPCjV15ljIwgcKU2AY6pgGi9Nlp%2Bz4gsXnKQQ0PiZFnt7guFBMEpwGmD3p26yeADgzEyFRrBrgP5aI3Q0D3F6camrCGTO%2FILJI2ycKqkYaTnKk3R9qxi16iQCVy56DUfN%2Bv%2F7%2B6Ocb%2BE6ocMem1L5UV3SshRcNwWZz5c%2FW9c8%2B75lUlz6a6Fqd6AhzqHyAsWfNVSWb6atw%2FUflXOfk1EpPPXatKLYK9I5pLLKDlWHIho%2FswmlZy&X-Amz-Signature=b7a51dfd7fc0ca4db5727ccad6bce538a5704951ea28530eba8309de7f6c35c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TWLWTHI6%2F20261124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261124T073350Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHRlsuUDA0OLj%2BKHiGOHq6Qbm3Lh6LogM0fxdqSZPkL3AiBnR2bGbUul%2F3bZ6dMFoSi%2FNaFv7j6nBqpZXOk7G301uCqIBAiG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLh7xNPuO0N%2FBEf9CKtwDhroq%2FkFOn7lDB8mMNC5dy0mL5RCcrUUHTRLj2ML6htgBCwLEeoPgZYolBx5qGAILt4vNn%2BcheDQGi1g01jyrgTxznzWbZla3fhvK92Kjrf%2FQ9cBMCntScxbmlOKSlfHxsiECm%2BUuZt4im1RgD6sXxUT5zja8jyH0RGBTaVH3Zbw5twwr%2FOIqkLX29kpyhDOBtFIoPBGk65GlrgBlAux7J9cvQvs7d6h2Mc8%2BwFxnt4SCIbNNkFeVy%2BQenQlpmA1tax2y3V93pWBS3MFXUYamzW6Z%2B%2BmDfHUK3C84BNZZicw5NjmuPlTPW9hKHkIq1zHXJFAIYOtgyveh7aUpKyiBB98ZdiEc%2Fvw96DkgaU60A8duUyYvIzqHmwM1JV71QHOmFQunCzx1JgayBBn9kpfTHvcTn3SFTTCZ%2BibBOXfyUuDmEs0Ml293%2FDukWY30qmtHz6bsl0GUyMAT5Br8UkApQv9Ys7ki8e3bvE8qhwKqa9mjo1lMWpxVjtxglgH6WyGr8duQC8MMwPQhXwf6Q7aSFj0mYeLzcG377LZOym0d3FIs%2F3wwVxz%2FBfCGdepIiKYiBbcXc0I48l59KXsApTKULwTQZ9lb23MjHirIDdcovH4l3Ou44BPCjV15ljIwgcKU2AY6pgGi9Nlp%2Bz4gsXnKQQ0PiZFnt7guFBMEpwGmD3p26yeADgzEyFRrBrgP5aI3Q0D3F6camrCGTO%2FILJI2ycKqkYaTnKk3R9qxi16iQCVy56DUfN%2Bv%2F7%2B6Ocb%2BE6ocMem1L5UV3SshRcNwWZz5c%2FW9c8%2B75lUlz6a6Fqd6AhzqHyAsWfNVSWb6atw%2FUflXOfk1EpPPXatKLYK9I5pLLKDlWHIho%2FswmlZy&X-Amz-Signature=45b2143853d3a1753d97af97ee304d76dda28ecd0ae869090492e4772597af79&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TWLWTHI6%2F20261124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261124T073350Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHRlsuUDA0OLj%2BKHiGOHq6Qbm3Lh6LogM0fxdqSZPkL3AiBnR2bGbUul%2F3bZ6dMFoSi%2FNaFv7j6nBqpZXOk7G301uCqIBAiG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLh7xNPuO0N%2FBEf9CKtwDhroq%2FkFOn7lDB8mMNC5dy0mL5RCcrUUHTRLj2ML6htgBCwLEeoPgZYolBx5qGAILt4vNn%2BcheDQGi1g01jyrgTxznzWbZla3fhvK92Kjrf%2FQ9cBMCntScxbmlOKSlfHxsiECm%2BUuZt4im1RgD6sXxUT5zja8jyH0RGBTaVH3Zbw5twwr%2FOIqkLX29kpyhDOBtFIoPBGk65GlrgBlAux7J9cvQvs7d6h2Mc8%2BwFxnt4SCIbNNkFeVy%2BQenQlpmA1tax2y3V93pWBS3MFXUYamzW6Z%2B%2BmDfHUK3C84BNZZicw5NjmuPlTPW9hKHkIq1zHXJFAIYOtgyveh7aUpKyiBB98ZdiEc%2Fvw96DkgaU60A8duUyYvIzqHmwM1JV71QHOmFQunCzx1JgayBBn9kpfTHvcTn3SFTTCZ%2BibBOXfyUuDmEs0Ml293%2FDukWY30qmtHz6bsl0GUyMAT5Br8UkApQv9Ys7ki8e3bvE8qhwKqa9mjo1lMWpxVjtxglgH6WyGr8duQC8MMwPQhXwf6Q7aSFj0mYeLzcG377LZOym0d3FIs%2F3wwVxz%2FBfCGdepIiKYiBbcXc0I48l59KXsApTKULwTQZ9lb23MjHirIDdcovH4l3Ou44BPCjV15ljIwgcKU2AY6pgGi9Nlp%2Bz4gsXnKQQ0PiZFnt7guFBMEpwGmD3p26yeADgzEyFRrBrgP5aI3Q0D3F6camrCGTO%2FILJI2ycKqkYaTnKk3R9qxi16iQCVy56DUfN%2Bv%2F7%2B6Ocb%2BE6ocMem1L5UV3SshRcNwWZz5c%2FW9c8%2B75lUlz6a6Fqd6AhzqHyAsWfNVSWb6atw%2FUflXOfk1EpPPXatKLYK9I5pLLKDlWHIho%2FswmlZy&X-Amz-Signature=62a4318a9a914163d0bebcc85e67515efcb5e8e2ba649773eff3303bd4772ac5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TWLWTHI6%2F20261124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261124T073350Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHRlsuUDA0OLj%2BKHiGOHq6Qbm3Lh6LogM0fxdqSZPkL3AiBnR2bGbUul%2F3bZ6dMFoSi%2FNaFv7j6nBqpZXOk7G301uCqIBAiG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLh7xNPuO0N%2FBEf9CKtwDhroq%2FkFOn7lDB8mMNC5dy0mL5RCcrUUHTRLj2ML6htgBCwLEeoPgZYolBx5qGAILt4vNn%2BcheDQGi1g01jyrgTxznzWbZla3fhvK92Kjrf%2FQ9cBMCntScxbmlOKSlfHxsiECm%2BUuZt4im1RgD6sXxUT5zja8jyH0RGBTaVH3Zbw5twwr%2FOIqkLX29kpyhDOBtFIoPBGk65GlrgBlAux7J9cvQvs7d6h2Mc8%2BwFxnt4SCIbNNkFeVy%2BQenQlpmA1tax2y3V93pWBS3MFXUYamzW6Z%2B%2BmDfHUK3C84BNZZicw5NjmuPlTPW9hKHkIq1zHXJFAIYOtgyveh7aUpKyiBB98ZdiEc%2Fvw96DkgaU60A8duUyYvIzqHmwM1JV71QHOmFQunCzx1JgayBBn9kpfTHvcTn3SFTTCZ%2BibBOXfyUuDmEs0Ml293%2FDukWY30qmtHz6bsl0GUyMAT5Br8UkApQv9Ys7ki8e3bvE8qhwKqa9mjo1lMWpxVjtxglgH6WyGr8duQC8MMwPQhXwf6Q7aSFj0mYeLzcG377LZOym0d3FIs%2F3wwVxz%2FBfCGdepIiKYiBbcXc0I48l59KXsApTKULwTQZ9lb23MjHirIDdcovH4l3Ou44BPCjV15ljIwgcKU2AY6pgGi9Nlp%2Bz4gsXnKQQ0PiZFnt7guFBMEpwGmD3p26yeADgzEyFRrBrgP5aI3Q0D3F6camrCGTO%2FILJI2ycKqkYaTnKk3R9qxi16iQCVy56DUfN%2Bv%2F7%2B6Ocb%2BE6ocMem1L5UV3SshRcNwWZz5c%2FW9c8%2B75lUlz6a6Fqd6AhzqHyAsWfNVSWb6atw%2FUflXOfk1EpPPXatKLYK9I5pLLKDlWHIho%2FswmlZy&X-Amz-Signature=c3a2aa4b1875b6dfb09da025c7f81fc5b946f553763c3280bb6c204ade30f47b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TWLWTHI6%2F20261124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261124T073350Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHRlsuUDA0OLj%2BKHiGOHq6Qbm3Lh6LogM0fxdqSZPkL3AiBnR2bGbUul%2F3bZ6dMFoSi%2FNaFv7j6nBqpZXOk7G301uCqIBAiG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLh7xNPuO0N%2FBEf9CKtwDhroq%2FkFOn7lDB8mMNC5dy0mL5RCcrUUHTRLj2ML6htgBCwLEeoPgZYolBx5qGAILt4vNn%2BcheDQGi1g01jyrgTxznzWbZla3fhvK92Kjrf%2FQ9cBMCntScxbmlOKSlfHxsiECm%2BUuZt4im1RgD6sXxUT5zja8jyH0RGBTaVH3Zbw5twwr%2FOIqkLX29kpyhDOBtFIoPBGk65GlrgBlAux7J9cvQvs7d6h2Mc8%2BwFxnt4SCIbNNkFeVy%2BQenQlpmA1tax2y3V93pWBS3MFXUYamzW6Z%2B%2BmDfHUK3C84BNZZicw5NjmuPlTPW9hKHkIq1zHXJFAIYOtgyveh7aUpKyiBB98ZdiEc%2Fvw96DkgaU60A8duUyYvIzqHmwM1JV71QHOmFQunCzx1JgayBBn9kpfTHvcTn3SFTTCZ%2BibBOXfyUuDmEs0Ml293%2FDukWY30qmtHz6bsl0GUyMAT5Br8UkApQv9Ys7ki8e3bvE8qhwKqa9mjo1lMWpxVjtxglgH6WyGr8duQC8MMwPQhXwf6Q7aSFj0mYeLzcG377LZOym0d3FIs%2F3wwVxz%2FBfCGdepIiKYiBbcXc0I48l59KXsApTKULwTQZ9lb23MjHirIDdcovH4l3Ou44BPCjV15ljIwgcKU2AY6pgGi9Nlp%2Bz4gsXnKQQ0PiZFnt7guFBMEpwGmD3p26yeADgzEyFRrBrgP5aI3Q0D3F6camrCGTO%2FILJI2ycKqkYaTnKk3R9qxi16iQCVy56DUfN%2Bv%2F7%2B6Ocb%2BE6ocMem1L5UV3SshRcNwWZz5c%2FW9c8%2B75lUlz6a6Fqd6AhzqHyAsWfNVSWb6atw%2FUflXOfk1EpPPXatKLYK9I5pLLKDlWHIho%2FswmlZy&X-Amz-Signature=6b3ddab304962ce1dc24ce231e6e497023bfde436a23f99ad334fbe18a7e0ece&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TWLWTHI6%2F20261124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261124T073350Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHRlsuUDA0OLj%2BKHiGOHq6Qbm3Lh6LogM0fxdqSZPkL3AiBnR2bGbUul%2F3bZ6dMFoSi%2FNaFv7j6nBqpZXOk7G301uCqIBAiG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLh7xNPuO0N%2FBEf9CKtwDhroq%2FkFOn7lDB8mMNC5dy0mL5RCcrUUHTRLj2ML6htgBCwLEeoPgZYolBx5qGAILt4vNn%2BcheDQGi1g01jyrgTxznzWbZla3fhvK92Kjrf%2FQ9cBMCntScxbmlOKSlfHxsiECm%2BUuZt4im1RgD6sXxUT5zja8jyH0RGBTaVH3Zbw5twwr%2FOIqkLX29kpyhDOBtFIoPBGk65GlrgBlAux7J9cvQvs7d6h2Mc8%2BwFxnt4SCIbNNkFeVy%2BQenQlpmA1tax2y3V93pWBS3MFXUYamzW6Z%2B%2BmDfHUK3C84BNZZicw5NjmuPlTPW9hKHkIq1zHXJFAIYOtgyveh7aUpKyiBB98ZdiEc%2Fvw96DkgaU60A8duUyYvIzqHmwM1JV71QHOmFQunCzx1JgayBBn9kpfTHvcTn3SFTTCZ%2BibBOXfyUuDmEs0Ml293%2FDukWY30qmtHz6bsl0GUyMAT5Br8UkApQv9Ys7ki8e3bvE8qhwKqa9mjo1lMWpxVjtxglgH6WyGr8duQC8MMwPQhXwf6Q7aSFj0mYeLzcG377LZOym0d3FIs%2F3wwVxz%2FBfCGdepIiKYiBbcXc0I48l59KXsApTKULwTQZ9lb23MjHirIDdcovH4l3Ou44BPCjV15ljIwgcKU2AY6pgGi9Nlp%2Bz4gsXnKQQ0PiZFnt7guFBMEpwGmD3p26yeADgzEyFRrBrgP5aI3Q0D3F6camrCGTO%2FILJI2ycKqkYaTnKk3R9qxi16iQCVy56DUfN%2Bv%2F7%2B6Ocb%2BE6ocMem1L5UV3SshRcNwWZz5c%2FW9c8%2B75lUlz6a6Fqd6AhzqHyAsWfNVSWb6atw%2FUflXOfk1EpPPXatKLYK9I5pLLKDlWHIho%2FswmlZy&X-Amz-Signature=42652a687dad9c512ab4e902c729e76a600964a4d17c9787b6d45d34e43f6d58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TWLWTHI6%2F20261124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261124T073350Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHRlsuUDA0OLj%2BKHiGOHq6Qbm3Lh6LogM0fxdqSZPkL3AiBnR2bGbUul%2F3bZ6dMFoSi%2FNaFv7j6nBqpZXOk7G301uCqIBAiG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLh7xNPuO0N%2FBEf9CKtwDhroq%2FkFOn7lDB8mMNC5dy0mL5RCcrUUHTRLj2ML6htgBCwLEeoPgZYolBx5qGAILt4vNn%2BcheDQGi1g01jyrgTxznzWbZla3fhvK92Kjrf%2FQ9cBMCntScxbmlOKSlfHxsiECm%2BUuZt4im1RgD6sXxUT5zja8jyH0RGBTaVH3Zbw5twwr%2FOIqkLX29kpyhDOBtFIoPBGk65GlrgBlAux7J9cvQvs7d6h2Mc8%2BwFxnt4SCIbNNkFeVy%2BQenQlpmA1tax2y3V93pWBS3MFXUYamzW6Z%2B%2BmDfHUK3C84BNZZicw5NjmuPlTPW9hKHkIq1zHXJFAIYOtgyveh7aUpKyiBB98ZdiEc%2Fvw96DkgaU60A8duUyYvIzqHmwM1JV71QHOmFQunCzx1JgayBBn9kpfTHvcTn3SFTTCZ%2BibBOXfyUuDmEs0Ml293%2FDukWY30qmtHz6bsl0GUyMAT5Br8UkApQv9Ys7ki8e3bvE8qhwKqa9mjo1lMWpxVjtxglgH6WyGr8duQC8MMwPQhXwf6Q7aSFj0mYeLzcG377LZOym0d3FIs%2F3wwVxz%2FBfCGdepIiKYiBbcXc0I48l59KXsApTKULwTQZ9lb23MjHirIDdcovH4l3Ou44BPCjV15ljIwgcKU2AY6pgGi9Nlp%2Bz4gsXnKQQ0PiZFnt7guFBMEpwGmD3p26yeADgzEyFRrBrgP5aI3Q0D3F6camrCGTO%2FILJI2ycKqkYaTnKk3R9qxi16iQCVy56DUfN%2Bv%2F7%2B6Ocb%2BE6ocMem1L5UV3SshRcNwWZz5c%2FW9c8%2B75lUlz6a6Fqd6AhzqHyAsWfNVSWb6atw%2FUflXOfk1EpPPXatKLYK9I5pLLKDlWHIho%2FswmlZy&X-Amz-Signature=3f07531b9f2b2f02a673aa4a3caff7f1b590774a84caff17787bb6ef956b4513&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TWLWTHI6%2F20261124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261124T073350Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHRlsuUDA0OLj%2BKHiGOHq6Qbm3Lh6LogM0fxdqSZPkL3AiBnR2bGbUul%2F3bZ6dMFoSi%2FNaFv7j6nBqpZXOk7G301uCqIBAiG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLh7xNPuO0N%2FBEf9CKtwDhroq%2FkFOn7lDB8mMNC5dy0mL5RCcrUUHTRLj2ML6htgBCwLEeoPgZYolBx5qGAILt4vNn%2BcheDQGi1g01jyrgTxznzWbZla3fhvK92Kjrf%2FQ9cBMCntScxbmlOKSlfHxsiECm%2BUuZt4im1RgD6sXxUT5zja8jyH0RGBTaVH3Zbw5twwr%2FOIqkLX29kpyhDOBtFIoPBGk65GlrgBlAux7J9cvQvs7d6h2Mc8%2BwFxnt4SCIbNNkFeVy%2BQenQlpmA1tax2y3V93pWBS3MFXUYamzW6Z%2B%2BmDfHUK3C84BNZZicw5NjmuPlTPW9hKHkIq1zHXJFAIYOtgyveh7aUpKyiBB98ZdiEc%2Fvw96DkgaU60A8duUyYvIzqHmwM1JV71QHOmFQunCzx1JgayBBn9kpfTHvcTn3SFTTCZ%2BibBOXfyUuDmEs0Ml293%2FDukWY30qmtHz6bsl0GUyMAT5Br8UkApQv9Ys7ki8e3bvE8qhwKqa9mjo1lMWpxVjtxglgH6WyGr8duQC8MMwPQhXwf6Q7aSFj0mYeLzcG377LZOym0d3FIs%2F3wwVxz%2FBfCGdepIiKYiBbcXc0I48l59KXsApTKULwTQZ9lb23MjHirIDdcovH4l3Ou44BPCjV15ljIwgcKU2AY6pgGi9Nlp%2Bz4gsXnKQQ0PiZFnt7guFBMEpwGmD3p26yeADgzEyFRrBrgP5aI3Q0D3F6camrCGTO%2FILJI2ycKqkYaTnKk3R9qxi16iQCVy56DUfN%2Bv%2F7%2B6Ocb%2BE6ocMem1L5UV3SshRcNwWZz5c%2FW9c8%2B75lUlz6a6Fqd6AhzqHyAsWfNVSWb6atw%2FUflXOfk1EpPPXatKLYK9I5pLLKDlWHIho%2FswmlZy&X-Amz-Signature=a7b9b54b539f9b0fa2761d18a7ffda7ccc2859b25da9fe0ab085381b61905a5a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TWLWTHI6%2F20261124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261124T073350Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHRlsuUDA0OLj%2BKHiGOHq6Qbm3Lh6LogM0fxdqSZPkL3AiBnR2bGbUul%2F3bZ6dMFoSi%2FNaFv7j6nBqpZXOk7G301uCqIBAiG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLh7xNPuO0N%2FBEf9CKtwDhroq%2FkFOn7lDB8mMNC5dy0mL5RCcrUUHTRLj2ML6htgBCwLEeoPgZYolBx5qGAILt4vNn%2BcheDQGi1g01jyrgTxznzWbZla3fhvK92Kjrf%2FQ9cBMCntScxbmlOKSlfHxsiECm%2BUuZt4im1RgD6sXxUT5zja8jyH0RGBTaVH3Zbw5twwr%2FOIqkLX29kpyhDOBtFIoPBGk65GlrgBlAux7J9cvQvs7d6h2Mc8%2BwFxnt4SCIbNNkFeVy%2BQenQlpmA1tax2y3V93pWBS3MFXUYamzW6Z%2B%2BmDfHUK3C84BNZZicw5NjmuPlTPW9hKHkIq1zHXJFAIYOtgyveh7aUpKyiBB98ZdiEc%2Fvw96DkgaU60A8duUyYvIzqHmwM1JV71QHOmFQunCzx1JgayBBn9kpfTHvcTn3SFTTCZ%2BibBOXfyUuDmEs0Ml293%2FDukWY30qmtHz6bsl0GUyMAT5Br8UkApQv9Ys7ki8e3bvE8qhwKqa9mjo1lMWpxVjtxglgH6WyGr8duQC8MMwPQhXwf6Q7aSFj0mYeLzcG377LZOym0d3FIs%2F3wwVxz%2FBfCGdepIiKYiBbcXc0I48l59KXsApTKULwTQZ9lb23MjHirIDdcovH4l3Ou44BPCjV15ljIwgcKU2AY6pgGi9Nlp%2Bz4gsXnKQQ0PiZFnt7guFBMEpwGmD3p26yeADgzEyFRrBrgP5aI3Q0D3F6camrCGTO%2FILJI2ycKqkYaTnKk3R9qxi16iQCVy56DUfN%2Bv%2F7%2B6Ocb%2BE6ocMem1L5UV3SshRcNwWZz5c%2FW9c8%2B75lUlz6a6Fqd6AhzqHyAsWfNVSWb6atw%2FUflXOfk1EpPPXatKLYK9I5pLLKDlWHIho%2FswmlZy&X-Amz-Signature=5ddec71005900c1aeae190c8940d4d80aac4de34e2c5c67fc119d959dcaccee1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
