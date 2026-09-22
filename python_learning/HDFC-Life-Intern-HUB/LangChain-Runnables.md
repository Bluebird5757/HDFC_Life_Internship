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
fetched_at: '2026-09-22T02:22:57.274Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWOBTBAO%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022251Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpf3%2FuhMwT%2F0QA9NNLuC0FdzeNyUIO9WU5iXNEQowA4gIhAKcVckz4obLdAP0qfPx%2F9XdwKMHzHkLC%2FrGGCpmzr2phKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz2lFpEZOcThV6isrgq3AMy78vOhD4OFL4OMkj%2FGmJc5NTF34FzmxPa2aztWiI94w1QhDDNNcQ8BtenMMf%2BMiUc0BfiMis74sSnZ8a3lp3ViQ%2Bfl4eQSCOBAWJZRhqSO8vnNZZOex%2Bk%2FagBKvJ3k%2BdS7Y6D6UXkJe1Q0Gn%2F0Smjf3b%2FCCC6%2F37oWfsi1%2F8cPpDjom0uWaBZPicSLSh7LQYyGYRuc4xuBCX81pgTCBI07srHuWU6yC1KZYEtWGkoy4YglX3fjuX3a%2FIRyd4LS9oc3wVHtSdsDB1JHqmCwww4nwRpAzHaRPsC%2BkZ5C4ojJTsnYkRZwGgrEHsAKG0r3%2Bnwpib3hEe6nl4iomX50qqNxstBYfD9y23YyRy9IYtOglJg3mKKqffX3B75Rbe%2Fmyh3vhVHra8mpXREXEKP3SYe65vi0h6fIUhcaEJnMzXX1kglsuC06M0UQFD8r6K%2BM%2BE3glkgmbMF01HDhpwkm7pG5McwsVs51CCBEQAkRpl9Wu9%2BIxR%2BVNMQcIBMGRkBTCeKJd5%2BJotOWPRKhfm%2F5bJFQxkg9gPH%2F22hM6mvZo3YCY%2BmNpLDrbRoB4XVcVyCzSDwGfEfsWKKypCSC63WeDJERUrcx9mulhqZp8TuJ2k7MwO4nevM27w8VLzuyTC1mcfVBjqkATk0az%2BlVg4VRzUTtrH1XtbzhVEvQOJBiOErPxJgybz5A9akZ%2F3jACKtANqxOsuXDy3BnlRKyM5gsQ4x%2Bkr2J%2BQQUR%2Fn%2BYz34%2F4i6NqJ30eqw6H6l0silVQa7M%2B1%2Fp%2FOalDqL4lXj4IA7mshHF7wNEgyj1H%2BH6PWauLE%2Fbo58FRKh2c7Pxh0irWHxwzgCARBdzHUumkbN40F1dI%2BYGYRN7z%2FyTF8&X-Amz-Signature=fed092231263cf9e4f24fb9dd99522efeeba7289e0ca2ea3a21e6fbe6e8182aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWOBTBAO%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022252Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpf3%2FuhMwT%2F0QA9NNLuC0FdzeNyUIO9WU5iXNEQowA4gIhAKcVckz4obLdAP0qfPx%2F9XdwKMHzHkLC%2FrGGCpmzr2phKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz2lFpEZOcThV6isrgq3AMy78vOhD4OFL4OMkj%2FGmJc5NTF34FzmxPa2aztWiI94w1QhDDNNcQ8BtenMMf%2BMiUc0BfiMis74sSnZ8a3lp3ViQ%2Bfl4eQSCOBAWJZRhqSO8vnNZZOex%2Bk%2FagBKvJ3k%2BdS7Y6D6UXkJe1Q0Gn%2F0Smjf3b%2FCCC6%2F37oWfsi1%2F8cPpDjom0uWaBZPicSLSh7LQYyGYRuc4xuBCX81pgTCBI07srHuWU6yC1KZYEtWGkoy4YglX3fjuX3a%2FIRyd4LS9oc3wVHtSdsDB1JHqmCwww4nwRpAzHaRPsC%2BkZ5C4ojJTsnYkRZwGgrEHsAKG0r3%2Bnwpib3hEe6nl4iomX50qqNxstBYfD9y23YyRy9IYtOglJg3mKKqffX3B75Rbe%2Fmyh3vhVHra8mpXREXEKP3SYe65vi0h6fIUhcaEJnMzXX1kglsuC06M0UQFD8r6K%2BM%2BE3glkgmbMF01HDhpwkm7pG5McwsVs51CCBEQAkRpl9Wu9%2BIxR%2BVNMQcIBMGRkBTCeKJd5%2BJotOWPRKhfm%2F5bJFQxkg9gPH%2F22hM6mvZo3YCY%2BmNpLDrbRoB4XVcVyCzSDwGfEfsWKKypCSC63WeDJERUrcx9mulhqZp8TuJ2k7MwO4nevM27w8VLzuyTC1mcfVBjqkATk0az%2BlVg4VRzUTtrH1XtbzhVEvQOJBiOErPxJgybz5A9akZ%2F3jACKtANqxOsuXDy3BnlRKyM5gsQ4x%2Bkr2J%2BQQUR%2Fn%2BYz34%2F4i6NqJ30eqw6H6l0silVQa7M%2B1%2Fp%2FOalDqL4lXj4IA7mshHF7wNEgyj1H%2BH6PWauLE%2Fbo58FRKh2c7Pxh0irWHxwzgCARBdzHUumkbN40F1dI%2BYGYRN7z%2FyTF8&X-Amz-Signature=052b9bfe5cd6106c66c2f042cfb0fbdddb9a1bbd322fa3e181428b27d14c116c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWOBTBAO%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022251Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpf3%2FuhMwT%2F0QA9NNLuC0FdzeNyUIO9WU5iXNEQowA4gIhAKcVckz4obLdAP0qfPx%2F9XdwKMHzHkLC%2FrGGCpmzr2phKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz2lFpEZOcThV6isrgq3AMy78vOhD4OFL4OMkj%2FGmJc5NTF34FzmxPa2aztWiI94w1QhDDNNcQ8BtenMMf%2BMiUc0BfiMis74sSnZ8a3lp3ViQ%2Bfl4eQSCOBAWJZRhqSO8vnNZZOex%2Bk%2FagBKvJ3k%2BdS7Y6D6UXkJe1Q0Gn%2F0Smjf3b%2FCCC6%2F37oWfsi1%2F8cPpDjom0uWaBZPicSLSh7LQYyGYRuc4xuBCX81pgTCBI07srHuWU6yC1KZYEtWGkoy4YglX3fjuX3a%2FIRyd4LS9oc3wVHtSdsDB1JHqmCwww4nwRpAzHaRPsC%2BkZ5C4ojJTsnYkRZwGgrEHsAKG0r3%2Bnwpib3hEe6nl4iomX50qqNxstBYfD9y23YyRy9IYtOglJg3mKKqffX3B75Rbe%2Fmyh3vhVHra8mpXREXEKP3SYe65vi0h6fIUhcaEJnMzXX1kglsuC06M0UQFD8r6K%2BM%2BE3glkgmbMF01HDhpwkm7pG5McwsVs51CCBEQAkRpl9Wu9%2BIxR%2BVNMQcIBMGRkBTCeKJd5%2BJotOWPRKhfm%2F5bJFQxkg9gPH%2F22hM6mvZo3YCY%2BmNpLDrbRoB4XVcVyCzSDwGfEfsWKKypCSC63WeDJERUrcx9mulhqZp8TuJ2k7MwO4nevM27w8VLzuyTC1mcfVBjqkATk0az%2BlVg4VRzUTtrH1XtbzhVEvQOJBiOErPxJgybz5A9akZ%2F3jACKtANqxOsuXDy3BnlRKyM5gsQ4x%2Bkr2J%2BQQUR%2Fn%2BYz34%2F4i6NqJ30eqw6H6l0silVQa7M%2B1%2Fp%2FOalDqL4lXj4IA7mshHF7wNEgyj1H%2BH6PWauLE%2Fbo58FRKh2c7Pxh0irWHxwzgCARBdzHUumkbN40F1dI%2BYGYRN7z%2FyTF8&X-Amz-Signature=dacb86c17253da525c68bd07659b78397f68ace218c93c5b0cdc399690c81056&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWOBTBAO%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022252Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpf3%2FuhMwT%2F0QA9NNLuC0FdzeNyUIO9WU5iXNEQowA4gIhAKcVckz4obLdAP0qfPx%2F9XdwKMHzHkLC%2FrGGCpmzr2phKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz2lFpEZOcThV6isrgq3AMy78vOhD4OFL4OMkj%2FGmJc5NTF34FzmxPa2aztWiI94w1QhDDNNcQ8BtenMMf%2BMiUc0BfiMis74sSnZ8a3lp3ViQ%2Bfl4eQSCOBAWJZRhqSO8vnNZZOex%2Bk%2FagBKvJ3k%2BdS7Y6D6UXkJe1Q0Gn%2F0Smjf3b%2FCCC6%2F37oWfsi1%2F8cPpDjom0uWaBZPicSLSh7LQYyGYRuc4xuBCX81pgTCBI07srHuWU6yC1KZYEtWGkoy4YglX3fjuX3a%2FIRyd4LS9oc3wVHtSdsDB1JHqmCwww4nwRpAzHaRPsC%2BkZ5C4ojJTsnYkRZwGgrEHsAKG0r3%2Bnwpib3hEe6nl4iomX50qqNxstBYfD9y23YyRy9IYtOglJg3mKKqffX3B75Rbe%2Fmyh3vhVHra8mpXREXEKP3SYe65vi0h6fIUhcaEJnMzXX1kglsuC06M0UQFD8r6K%2BM%2BE3glkgmbMF01HDhpwkm7pG5McwsVs51CCBEQAkRpl9Wu9%2BIxR%2BVNMQcIBMGRkBTCeKJd5%2BJotOWPRKhfm%2F5bJFQxkg9gPH%2F22hM6mvZo3YCY%2BmNpLDrbRoB4XVcVyCzSDwGfEfsWKKypCSC63WeDJERUrcx9mulhqZp8TuJ2k7MwO4nevM27w8VLzuyTC1mcfVBjqkATk0az%2BlVg4VRzUTtrH1XtbzhVEvQOJBiOErPxJgybz5A9akZ%2F3jACKtANqxOsuXDy3BnlRKyM5gsQ4x%2Bkr2J%2BQQUR%2Fn%2BYz34%2F4i6NqJ30eqw6H6l0silVQa7M%2B1%2Fp%2FOalDqL4lXj4IA7mshHF7wNEgyj1H%2BH6PWauLE%2Fbo58FRKh2c7Pxh0irWHxwzgCARBdzHUumkbN40F1dI%2BYGYRN7z%2FyTF8&X-Amz-Signature=01745bca917c395268416abf874f4023818fbbf45b2ca350c4426a58d4e87e12&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWOBTBAO%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022251Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpf3%2FuhMwT%2F0QA9NNLuC0FdzeNyUIO9WU5iXNEQowA4gIhAKcVckz4obLdAP0qfPx%2F9XdwKMHzHkLC%2FrGGCpmzr2phKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz2lFpEZOcThV6isrgq3AMy78vOhD4OFL4OMkj%2FGmJc5NTF34FzmxPa2aztWiI94w1QhDDNNcQ8BtenMMf%2BMiUc0BfiMis74sSnZ8a3lp3ViQ%2Bfl4eQSCOBAWJZRhqSO8vnNZZOex%2Bk%2FagBKvJ3k%2BdS7Y6D6UXkJe1Q0Gn%2F0Smjf3b%2FCCC6%2F37oWfsi1%2F8cPpDjom0uWaBZPicSLSh7LQYyGYRuc4xuBCX81pgTCBI07srHuWU6yC1KZYEtWGkoy4YglX3fjuX3a%2FIRyd4LS9oc3wVHtSdsDB1JHqmCwww4nwRpAzHaRPsC%2BkZ5C4ojJTsnYkRZwGgrEHsAKG0r3%2Bnwpib3hEe6nl4iomX50qqNxstBYfD9y23YyRy9IYtOglJg3mKKqffX3B75Rbe%2Fmyh3vhVHra8mpXREXEKP3SYe65vi0h6fIUhcaEJnMzXX1kglsuC06M0UQFD8r6K%2BM%2BE3glkgmbMF01HDhpwkm7pG5McwsVs51CCBEQAkRpl9Wu9%2BIxR%2BVNMQcIBMGRkBTCeKJd5%2BJotOWPRKhfm%2F5bJFQxkg9gPH%2F22hM6mvZo3YCY%2BmNpLDrbRoB4XVcVyCzSDwGfEfsWKKypCSC63WeDJERUrcx9mulhqZp8TuJ2k7MwO4nevM27w8VLzuyTC1mcfVBjqkATk0az%2BlVg4VRzUTtrH1XtbzhVEvQOJBiOErPxJgybz5A9akZ%2F3jACKtANqxOsuXDy3BnlRKyM5gsQ4x%2Bkr2J%2BQQUR%2Fn%2BYz34%2F4i6NqJ30eqw6H6l0silVQa7M%2B1%2Fp%2FOalDqL4lXj4IA7mshHF7wNEgyj1H%2BH6PWauLE%2Fbo58FRKh2c7Pxh0irWHxwzgCARBdzHUumkbN40F1dI%2BYGYRN7z%2FyTF8&X-Amz-Signature=ed0b5cd67a1d90ad3bfbd19a5a45e67c9942b003c92cbd0caa74e28f41862638&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWOBTBAO%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022252Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpf3%2FuhMwT%2F0QA9NNLuC0FdzeNyUIO9WU5iXNEQowA4gIhAKcVckz4obLdAP0qfPx%2F9XdwKMHzHkLC%2FrGGCpmzr2phKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz2lFpEZOcThV6isrgq3AMy78vOhD4OFL4OMkj%2FGmJc5NTF34FzmxPa2aztWiI94w1QhDDNNcQ8BtenMMf%2BMiUc0BfiMis74sSnZ8a3lp3ViQ%2Bfl4eQSCOBAWJZRhqSO8vnNZZOex%2Bk%2FagBKvJ3k%2BdS7Y6D6UXkJe1Q0Gn%2F0Smjf3b%2FCCC6%2F37oWfsi1%2F8cPpDjom0uWaBZPicSLSh7LQYyGYRuc4xuBCX81pgTCBI07srHuWU6yC1KZYEtWGkoy4YglX3fjuX3a%2FIRyd4LS9oc3wVHtSdsDB1JHqmCwww4nwRpAzHaRPsC%2BkZ5C4ojJTsnYkRZwGgrEHsAKG0r3%2Bnwpib3hEe6nl4iomX50qqNxstBYfD9y23YyRy9IYtOglJg3mKKqffX3B75Rbe%2Fmyh3vhVHra8mpXREXEKP3SYe65vi0h6fIUhcaEJnMzXX1kglsuC06M0UQFD8r6K%2BM%2BE3glkgmbMF01HDhpwkm7pG5McwsVs51CCBEQAkRpl9Wu9%2BIxR%2BVNMQcIBMGRkBTCeKJd5%2BJotOWPRKhfm%2F5bJFQxkg9gPH%2F22hM6mvZo3YCY%2BmNpLDrbRoB4XVcVyCzSDwGfEfsWKKypCSC63WeDJERUrcx9mulhqZp8TuJ2k7MwO4nevM27w8VLzuyTC1mcfVBjqkATk0az%2BlVg4VRzUTtrH1XtbzhVEvQOJBiOErPxJgybz5A9akZ%2F3jACKtANqxOsuXDy3BnlRKyM5gsQ4x%2Bkr2J%2BQQUR%2Fn%2BYz34%2F4i6NqJ30eqw6H6l0silVQa7M%2B1%2Fp%2FOalDqL4lXj4IA7mshHF7wNEgyj1H%2BH6PWauLE%2Fbo58FRKh2c7Pxh0irWHxwzgCARBdzHUumkbN40F1dI%2BYGYRN7z%2FyTF8&X-Amz-Signature=37601351bdcbc662365384cfac1487602e8f843252464fff91435b85ce646282&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWOBTBAO%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022252Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpf3%2FuhMwT%2F0QA9NNLuC0FdzeNyUIO9WU5iXNEQowA4gIhAKcVckz4obLdAP0qfPx%2F9XdwKMHzHkLC%2FrGGCpmzr2phKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz2lFpEZOcThV6isrgq3AMy78vOhD4OFL4OMkj%2FGmJc5NTF34FzmxPa2aztWiI94w1QhDDNNcQ8BtenMMf%2BMiUc0BfiMis74sSnZ8a3lp3ViQ%2Bfl4eQSCOBAWJZRhqSO8vnNZZOex%2Bk%2FagBKvJ3k%2BdS7Y6D6UXkJe1Q0Gn%2F0Smjf3b%2FCCC6%2F37oWfsi1%2F8cPpDjom0uWaBZPicSLSh7LQYyGYRuc4xuBCX81pgTCBI07srHuWU6yC1KZYEtWGkoy4YglX3fjuX3a%2FIRyd4LS9oc3wVHtSdsDB1JHqmCwww4nwRpAzHaRPsC%2BkZ5C4ojJTsnYkRZwGgrEHsAKG0r3%2Bnwpib3hEe6nl4iomX50qqNxstBYfD9y23YyRy9IYtOglJg3mKKqffX3B75Rbe%2Fmyh3vhVHra8mpXREXEKP3SYe65vi0h6fIUhcaEJnMzXX1kglsuC06M0UQFD8r6K%2BM%2BE3glkgmbMF01HDhpwkm7pG5McwsVs51CCBEQAkRpl9Wu9%2BIxR%2BVNMQcIBMGRkBTCeKJd5%2BJotOWPRKhfm%2F5bJFQxkg9gPH%2F22hM6mvZo3YCY%2BmNpLDrbRoB4XVcVyCzSDwGfEfsWKKypCSC63WeDJERUrcx9mulhqZp8TuJ2k7MwO4nevM27w8VLzuyTC1mcfVBjqkATk0az%2BlVg4VRzUTtrH1XtbzhVEvQOJBiOErPxJgybz5A9akZ%2F3jACKtANqxOsuXDy3BnlRKyM5gsQ4x%2Bkr2J%2BQQUR%2Fn%2BYz34%2F4i6NqJ30eqw6H6l0silVQa7M%2B1%2Fp%2FOalDqL4lXj4IA7mshHF7wNEgyj1H%2BH6PWauLE%2Fbo58FRKh2c7Pxh0irWHxwzgCARBdzHUumkbN40F1dI%2BYGYRN7z%2FyTF8&X-Amz-Signature=5d2de2eef2cafb064727a335497c32a148f34e78c27cd5de682db23c1b0031f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWOBTBAO%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022251Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpf3%2FuhMwT%2F0QA9NNLuC0FdzeNyUIO9WU5iXNEQowA4gIhAKcVckz4obLdAP0qfPx%2F9XdwKMHzHkLC%2FrGGCpmzr2phKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz2lFpEZOcThV6isrgq3AMy78vOhD4OFL4OMkj%2FGmJc5NTF34FzmxPa2aztWiI94w1QhDDNNcQ8BtenMMf%2BMiUc0BfiMis74sSnZ8a3lp3ViQ%2Bfl4eQSCOBAWJZRhqSO8vnNZZOex%2Bk%2FagBKvJ3k%2BdS7Y6D6UXkJe1Q0Gn%2F0Smjf3b%2FCCC6%2F37oWfsi1%2F8cPpDjom0uWaBZPicSLSh7LQYyGYRuc4xuBCX81pgTCBI07srHuWU6yC1KZYEtWGkoy4YglX3fjuX3a%2FIRyd4LS9oc3wVHtSdsDB1JHqmCwww4nwRpAzHaRPsC%2BkZ5C4ojJTsnYkRZwGgrEHsAKG0r3%2Bnwpib3hEe6nl4iomX50qqNxstBYfD9y23YyRy9IYtOglJg3mKKqffX3B75Rbe%2Fmyh3vhVHra8mpXREXEKP3SYe65vi0h6fIUhcaEJnMzXX1kglsuC06M0UQFD8r6K%2BM%2BE3glkgmbMF01HDhpwkm7pG5McwsVs51CCBEQAkRpl9Wu9%2BIxR%2BVNMQcIBMGRkBTCeKJd5%2BJotOWPRKhfm%2F5bJFQxkg9gPH%2F22hM6mvZo3YCY%2BmNpLDrbRoB4XVcVyCzSDwGfEfsWKKypCSC63WeDJERUrcx9mulhqZp8TuJ2k7MwO4nevM27w8VLzuyTC1mcfVBjqkATk0az%2BlVg4VRzUTtrH1XtbzhVEvQOJBiOErPxJgybz5A9akZ%2F3jACKtANqxOsuXDy3BnlRKyM5gsQ4x%2Bkr2J%2BQQUR%2Fn%2BYz34%2F4i6NqJ30eqw6H6l0silVQa7M%2B1%2Fp%2FOalDqL4lXj4IA7mshHF7wNEgyj1H%2BH6PWauLE%2Fbo58FRKh2c7Pxh0irWHxwzgCARBdzHUumkbN40F1dI%2BYGYRN7z%2FyTF8&X-Amz-Signature=0f05f6f006f5fadd4dcae209ee4f88a439539a85a3e4caad3b89f576bbdeb794&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWOBTBAO%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022252Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpf3%2FuhMwT%2F0QA9NNLuC0FdzeNyUIO9WU5iXNEQowA4gIhAKcVckz4obLdAP0qfPx%2F9XdwKMHzHkLC%2FrGGCpmzr2phKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz2lFpEZOcThV6isrgq3AMy78vOhD4OFL4OMkj%2FGmJc5NTF34FzmxPa2aztWiI94w1QhDDNNcQ8BtenMMf%2BMiUc0BfiMis74sSnZ8a3lp3ViQ%2Bfl4eQSCOBAWJZRhqSO8vnNZZOex%2Bk%2FagBKvJ3k%2BdS7Y6D6UXkJe1Q0Gn%2F0Smjf3b%2FCCC6%2F37oWfsi1%2F8cPpDjom0uWaBZPicSLSh7LQYyGYRuc4xuBCX81pgTCBI07srHuWU6yC1KZYEtWGkoy4YglX3fjuX3a%2FIRyd4LS9oc3wVHtSdsDB1JHqmCwww4nwRpAzHaRPsC%2BkZ5C4ojJTsnYkRZwGgrEHsAKG0r3%2Bnwpib3hEe6nl4iomX50qqNxstBYfD9y23YyRy9IYtOglJg3mKKqffX3B75Rbe%2Fmyh3vhVHra8mpXREXEKP3SYe65vi0h6fIUhcaEJnMzXX1kglsuC06M0UQFD8r6K%2BM%2BE3glkgmbMF01HDhpwkm7pG5McwsVs51CCBEQAkRpl9Wu9%2BIxR%2BVNMQcIBMGRkBTCeKJd5%2BJotOWPRKhfm%2F5bJFQxkg9gPH%2F22hM6mvZo3YCY%2BmNpLDrbRoB4XVcVyCzSDwGfEfsWKKypCSC63WeDJERUrcx9mulhqZp8TuJ2k7MwO4nevM27w8VLzuyTC1mcfVBjqkATk0az%2BlVg4VRzUTtrH1XtbzhVEvQOJBiOErPxJgybz5A9akZ%2F3jACKtANqxOsuXDy3BnlRKyM5gsQ4x%2Bkr2J%2BQQUR%2Fn%2BYz34%2F4i6NqJ30eqw6H6l0silVQa7M%2B1%2Fp%2FOalDqL4lXj4IA7mshHF7wNEgyj1H%2BH6PWauLE%2Fbo58FRKh2c7Pxh0irWHxwzgCARBdzHUumkbN40F1dI%2BYGYRN7z%2FyTF8&X-Amz-Signature=307f372c9227d2a7ac40118a1b9c4e8f512d05c2c58c57d3b48df4449e2af047&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
