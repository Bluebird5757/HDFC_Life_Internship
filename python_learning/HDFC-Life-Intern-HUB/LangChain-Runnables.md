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
fetched_at: '2026-10-22T02:25:39.065Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XQKH5ZU%2F20261022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261022T022534Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDnEY5lykVwRwwAv01enDXgZpmjcCifIybvsWroRPWvBAIgQF2yvk0kU2mQ%2BQA3U%2F0zzs%2FJTacfQ%2BTz8%2BCkz3s4A8kq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDFwstvpH%2FavuqTVbJSrcAyv0du7QMEDoJ5luqWKCm2pfouYgGaezfzNHDIcXDIh44PtGddOpoGY5fCQNoBSPMdTbonOHiqPzBTFkChVom8ET8n%2FdzDnYBPxhWyME76jUQB5q9HQNL9oPbKVVgD%2BBbeKyagzaQtd9Wak5dhv2pdi0Ari9RjvhWq6eUHNrk5XLrJfsoNX4fw3ZyNRRfMZCUjMTOt5bzAlRel1Q1Z%2Bqw3DEGeTKr5LNeoqSkOMDX8D5wLLTBy1axZeKQJFqBNkoM7yghum4y%2FbbEAjQTyl0X2KDV3qsyvR6F4bD5S95prTsyz8U5HAOQrXK1NAxV2wEQr109VjvWNsOHufG%2Fb5Nlp58pdReFjErp%2BwO2GDpA2%2FvCmBDLaiPP6ktKu548qmBZnlUbqtVh%2B02lS64R4SI38VzAgnlY14a953njV2mcV7MeosdE4M8Jbx%2F2LOBcPn30inlTXY0f0LbgztiE8yIEjggvuuEfSS776ZJb1OYt7AISf%2FbQmeRT8wMtDMCk9Ou3NK37zolf%2FZ65QSn%2BBk9%2Ft7IaIJ%2FoLmtJDE%2F6swX5F9QpCJ%2FezJwnYzJYW09sGU5rPzeXKCkQrtQupOHfD5m7JEVFJvYtTUhJCEbnMeIEgZDx8BTJQbI2yB6lw8pMNme5dYGOqUBiK6opqLm1uAKvnusUXRRyMXXDoHLqmWYWstqgo4zSJWtQ%2F5OLXT3P51j9MLPo8or0n8NGUjDPgB3mKvFEsyGDuTH050gS6aoOyrnK3Vh5ZzQ451Vj7I%2BvjAuUTm0fWljXuPvXHHJPkrbddeOlkT5diu1vt6JWwx561NvyjU8vQeGImwQjmoGv0uYjZeceT689KK3cb%2FBUHn7ivC0i8C%2Fz%2FGJ%2Fyhm&X-Amz-Signature=ac78b27595e5efad82130397588438c0b0f98b9bff023f0a7a2b452a055d8154&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XQKH5ZU%2F20261022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261022T022534Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDnEY5lykVwRwwAv01enDXgZpmjcCifIybvsWroRPWvBAIgQF2yvk0kU2mQ%2BQA3U%2F0zzs%2FJTacfQ%2BTz8%2BCkz3s4A8kq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDFwstvpH%2FavuqTVbJSrcAyv0du7QMEDoJ5luqWKCm2pfouYgGaezfzNHDIcXDIh44PtGddOpoGY5fCQNoBSPMdTbonOHiqPzBTFkChVom8ET8n%2FdzDnYBPxhWyME76jUQB5q9HQNL9oPbKVVgD%2BBbeKyagzaQtd9Wak5dhv2pdi0Ari9RjvhWq6eUHNrk5XLrJfsoNX4fw3ZyNRRfMZCUjMTOt5bzAlRel1Q1Z%2Bqw3DEGeTKr5LNeoqSkOMDX8D5wLLTBy1axZeKQJFqBNkoM7yghum4y%2FbbEAjQTyl0X2KDV3qsyvR6F4bD5S95prTsyz8U5HAOQrXK1NAxV2wEQr109VjvWNsOHufG%2Fb5Nlp58pdReFjErp%2BwO2GDpA2%2FvCmBDLaiPP6ktKu548qmBZnlUbqtVh%2B02lS64R4SI38VzAgnlY14a953njV2mcV7MeosdE4M8Jbx%2F2LOBcPn30inlTXY0f0LbgztiE8yIEjggvuuEfSS776ZJb1OYt7AISf%2FbQmeRT8wMtDMCk9Ou3NK37zolf%2FZ65QSn%2BBk9%2Ft7IaIJ%2FoLmtJDE%2F6swX5F9QpCJ%2FezJwnYzJYW09sGU5rPzeXKCkQrtQupOHfD5m7JEVFJvYtTUhJCEbnMeIEgZDx8BTJQbI2yB6lw8pMNme5dYGOqUBiK6opqLm1uAKvnusUXRRyMXXDoHLqmWYWstqgo4zSJWtQ%2F5OLXT3P51j9MLPo8or0n8NGUjDPgB3mKvFEsyGDuTH050gS6aoOyrnK3Vh5ZzQ451Vj7I%2BvjAuUTm0fWljXuPvXHHJPkrbddeOlkT5diu1vt6JWwx561NvyjU8vQeGImwQjmoGv0uYjZeceT689KK3cb%2FBUHn7ivC0i8C%2Fz%2FGJ%2Fyhm&X-Amz-Signature=f02a6b80ca0d24002347e4dac2c8fc47bde6a1b1159d4132fded0f92345e2500&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XQKH5ZU%2F20261022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261022T022534Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDnEY5lykVwRwwAv01enDXgZpmjcCifIybvsWroRPWvBAIgQF2yvk0kU2mQ%2BQA3U%2F0zzs%2FJTacfQ%2BTz8%2BCkz3s4A8kq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDFwstvpH%2FavuqTVbJSrcAyv0du7QMEDoJ5luqWKCm2pfouYgGaezfzNHDIcXDIh44PtGddOpoGY5fCQNoBSPMdTbonOHiqPzBTFkChVom8ET8n%2FdzDnYBPxhWyME76jUQB5q9HQNL9oPbKVVgD%2BBbeKyagzaQtd9Wak5dhv2pdi0Ari9RjvhWq6eUHNrk5XLrJfsoNX4fw3ZyNRRfMZCUjMTOt5bzAlRel1Q1Z%2Bqw3DEGeTKr5LNeoqSkOMDX8D5wLLTBy1axZeKQJFqBNkoM7yghum4y%2FbbEAjQTyl0X2KDV3qsyvR6F4bD5S95prTsyz8U5HAOQrXK1NAxV2wEQr109VjvWNsOHufG%2Fb5Nlp58pdReFjErp%2BwO2GDpA2%2FvCmBDLaiPP6ktKu548qmBZnlUbqtVh%2B02lS64R4SI38VzAgnlY14a953njV2mcV7MeosdE4M8Jbx%2F2LOBcPn30inlTXY0f0LbgztiE8yIEjggvuuEfSS776ZJb1OYt7AISf%2FbQmeRT8wMtDMCk9Ou3NK37zolf%2FZ65QSn%2BBk9%2Ft7IaIJ%2FoLmtJDE%2F6swX5F9QpCJ%2FezJwnYzJYW09sGU5rPzeXKCkQrtQupOHfD5m7JEVFJvYtTUhJCEbnMeIEgZDx8BTJQbI2yB6lw8pMNme5dYGOqUBiK6opqLm1uAKvnusUXRRyMXXDoHLqmWYWstqgo4zSJWtQ%2F5OLXT3P51j9MLPo8or0n8NGUjDPgB3mKvFEsyGDuTH050gS6aoOyrnK3Vh5ZzQ451Vj7I%2BvjAuUTm0fWljXuPvXHHJPkrbddeOlkT5diu1vt6JWwx561NvyjU8vQeGImwQjmoGv0uYjZeceT689KK3cb%2FBUHn7ivC0i8C%2Fz%2FGJ%2Fyhm&X-Amz-Signature=23b39d6f4969d66461df7cf8fe6f8036c9d1c3171f8c4b9dbbd8be7f9dcc3a62&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XQKH5ZU%2F20261022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261022T022534Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDnEY5lykVwRwwAv01enDXgZpmjcCifIybvsWroRPWvBAIgQF2yvk0kU2mQ%2BQA3U%2F0zzs%2FJTacfQ%2BTz8%2BCkz3s4A8kq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDFwstvpH%2FavuqTVbJSrcAyv0du7QMEDoJ5luqWKCm2pfouYgGaezfzNHDIcXDIh44PtGddOpoGY5fCQNoBSPMdTbonOHiqPzBTFkChVom8ET8n%2FdzDnYBPxhWyME76jUQB5q9HQNL9oPbKVVgD%2BBbeKyagzaQtd9Wak5dhv2pdi0Ari9RjvhWq6eUHNrk5XLrJfsoNX4fw3ZyNRRfMZCUjMTOt5bzAlRel1Q1Z%2Bqw3DEGeTKr5LNeoqSkOMDX8D5wLLTBy1axZeKQJFqBNkoM7yghum4y%2FbbEAjQTyl0X2KDV3qsyvR6F4bD5S95prTsyz8U5HAOQrXK1NAxV2wEQr109VjvWNsOHufG%2Fb5Nlp58pdReFjErp%2BwO2GDpA2%2FvCmBDLaiPP6ktKu548qmBZnlUbqtVh%2B02lS64R4SI38VzAgnlY14a953njV2mcV7MeosdE4M8Jbx%2F2LOBcPn30inlTXY0f0LbgztiE8yIEjggvuuEfSS776ZJb1OYt7AISf%2FbQmeRT8wMtDMCk9Ou3NK37zolf%2FZ65QSn%2BBk9%2Ft7IaIJ%2FoLmtJDE%2F6swX5F9QpCJ%2FezJwnYzJYW09sGU5rPzeXKCkQrtQupOHfD5m7JEVFJvYtTUhJCEbnMeIEgZDx8BTJQbI2yB6lw8pMNme5dYGOqUBiK6opqLm1uAKvnusUXRRyMXXDoHLqmWYWstqgo4zSJWtQ%2F5OLXT3P51j9MLPo8or0n8NGUjDPgB3mKvFEsyGDuTH050gS6aoOyrnK3Vh5ZzQ451Vj7I%2BvjAuUTm0fWljXuPvXHHJPkrbddeOlkT5diu1vt6JWwx561NvyjU8vQeGImwQjmoGv0uYjZeceT689KK3cb%2FBUHn7ivC0i8C%2Fz%2FGJ%2Fyhm&X-Amz-Signature=964b07ae281201846bf0095bed122b31ecd077f261bef713343770e7b0e457d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XQKH5ZU%2F20261022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261022T022534Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDnEY5lykVwRwwAv01enDXgZpmjcCifIybvsWroRPWvBAIgQF2yvk0kU2mQ%2BQA3U%2F0zzs%2FJTacfQ%2BTz8%2BCkz3s4A8kq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDFwstvpH%2FavuqTVbJSrcAyv0du7QMEDoJ5luqWKCm2pfouYgGaezfzNHDIcXDIh44PtGddOpoGY5fCQNoBSPMdTbonOHiqPzBTFkChVom8ET8n%2FdzDnYBPxhWyME76jUQB5q9HQNL9oPbKVVgD%2BBbeKyagzaQtd9Wak5dhv2pdi0Ari9RjvhWq6eUHNrk5XLrJfsoNX4fw3ZyNRRfMZCUjMTOt5bzAlRel1Q1Z%2Bqw3DEGeTKr5LNeoqSkOMDX8D5wLLTBy1axZeKQJFqBNkoM7yghum4y%2FbbEAjQTyl0X2KDV3qsyvR6F4bD5S95prTsyz8U5HAOQrXK1NAxV2wEQr109VjvWNsOHufG%2Fb5Nlp58pdReFjErp%2BwO2GDpA2%2FvCmBDLaiPP6ktKu548qmBZnlUbqtVh%2B02lS64R4SI38VzAgnlY14a953njV2mcV7MeosdE4M8Jbx%2F2LOBcPn30inlTXY0f0LbgztiE8yIEjggvuuEfSS776ZJb1OYt7AISf%2FbQmeRT8wMtDMCk9Ou3NK37zolf%2FZ65QSn%2BBk9%2Ft7IaIJ%2FoLmtJDE%2F6swX5F9QpCJ%2FezJwnYzJYW09sGU5rPzeXKCkQrtQupOHfD5m7JEVFJvYtTUhJCEbnMeIEgZDx8BTJQbI2yB6lw8pMNme5dYGOqUBiK6opqLm1uAKvnusUXRRyMXXDoHLqmWYWstqgo4zSJWtQ%2F5OLXT3P51j9MLPo8or0n8NGUjDPgB3mKvFEsyGDuTH050gS6aoOyrnK3Vh5ZzQ451Vj7I%2BvjAuUTm0fWljXuPvXHHJPkrbddeOlkT5diu1vt6JWwx561NvyjU8vQeGImwQjmoGv0uYjZeceT689KK3cb%2FBUHn7ivC0i8C%2Fz%2FGJ%2Fyhm&X-Amz-Signature=7ddecf25c2792ab413a2549f5ab51dd4480057bfd49b177f3f0a68620667e216&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XQKH5ZU%2F20261022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261022T022534Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDnEY5lykVwRwwAv01enDXgZpmjcCifIybvsWroRPWvBAIgQF2yvk0kU2mQ%2BQA3U%2F0zzs%2FJTacfQ%2BTz8%2BCkz3s4A8kq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDFwstvpH%2FavuqTVbJSrcAyv0du7QMEDoJ5luqWKCm2pfouYgGaezfzNHDIcXDIh44PtGddOpoGY5fCQNoBSPMdTbonOHiqPzBTFkChVom8ET8n%2FdzDnYBPxhWyME76jUQB5q9HQNL9oPbKVVgD%2BBbeKyagzaQtd9Wak5dhv2pdi0Ari9RjvhWq6eUHNrk5XLrJfsoNX4fw3ZyNRRfMZCUjMTOt5bzAlRel1Q1Z%2Bqw3DEGeTKr5LNeoqSkOMDX8D5wLLTBy1axZeKQJFqBNkoM7yghum4y%2FbbEAjQTyl0X2KDV3qsyvR6F4bD5S95prTsyz8U5HAOQrXK1NAxV2wEQr109VjvWNsOHufG%2Fb5Nlp58pdReFjErp%2BwO2GDpA2%2FvCmBDLaiPP6ktKu548qmBZnlUbqtVh%2B02lS64R4SI38VzAgnlY14a953njV2mcV7MeosdE4M8Jbx%2F2LOBcPn30inlTXY0f0LbgztiE8yIEjggvuuEfSS776ZJb1OYt7AISf%2FbQmeRT8wMtDMCk9Ou3NK37zolf%2FZ65QSn%2BBk9%2Ft7IaIJ%2FoLmtJDE%2F6swX5F9QpCJ%2FezJwnYzJYW09sGU5rPzeXKCkQrtQupOHfD5m7JEVFJvYtTUhJCEbnMeIEgZDx8BTJQbI2yB6lw8pMNme5dYGOqUBiK6opqLm1uAKvnusUXRRyMXXDoHLqmWYWstqgo4zSJWtQ%2F5OLXT3P51j9MLPo8or0n8NGUjDPgB3mKvFEsyGDuTH050gS6aoOyrnK3Vh5ZzQ451Vj7I%2BvjAuUTm0fWljXuPvXHHJPkrbddeOlkT5diu1vt6JWwx561NvyjU8vQeGImwQjmoGv0uYjZeceT689KK3cb%2FBUHn7ivC0i8C%2Fz%2FGJ%2Fyhm&X-Amz-Signature=a71cb5b3a18d36b0fed6fea6fe9941ed3de143912e7f081eb9a9e4367828537d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XQKH5ZU%2F20261022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261022T022534Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDnEY5lykVwRwwAv01enDXgZpmjcCifIybvsWroRPWvBAIgQF2yvk0kU2mQ%2BQA3U%2F0zzs%2FJTacfQ%2BTz8%2BCkz3s4A8kq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDFwstvpH%2FavuqTVbJSrcAyv0du7QMEDoJ5luqWKCm2pfouYgGaezfzNHDIcXDIh44PtGddOpoGY5fCQNoBSPMdTbonOHiqPzBTFkChVom8ET8n%2FdzDnYBPxhWyME76jUQB5q9HQNL9oPbKVVgD%2BBbeKyagzaQtd9Wak5dhv2pdi0Ari9RjvhWq6eUHNrk5XLrJfsoNX4fw3ZyNRRfMZCUjMTOt5bzAlRel1Q1Z%2Bqw3DEGeTKr5LNeoqSkOMDX8D5wLLTBy1axZeKQJFqBNkoM7yghum4y%2FbbEAjQTyl0X2KDV3qsyvR6F4bD5S95prTsyz8U5HAOQrXK1NAxV2wEQr109VjvWNsOHufG%2Fb5Nlp58pdReFjErp%2BwO2GDpA2%2FvCmBDLaiPP6ktKu548qmBZnlUbqtVh%2B02lS64R4SI38VzAgnlY14a953njV2mcV7MeosdE4M8Jbx%2F2LOBcPn30inlTXY0f0LbgztiE8yIEjggvuuEfSS776ZJb1OYt7AISf%2FbQmeRT8wMtDMCk9Ou3NK37zolf%2FZ65QSn%2BBk9%2Ft7IaIJ%2FoLmtJDE%2F6swX5F9QpCJ%2FezJwnYzJYW09sGU5rPzeXKCkQrtQupOHfD5m7JEVFJvYtTUhJCEbnMeIEgZDx8BTJQbI2yB6lw8pMNme5dYGOqUBiK6opqLm1uAKvnusUXRRyMXXDoHLqmWYWstqgo4zSJWtQ%2F5OLXT3P51j9MLPo8or0n8NGUjDPgB3mKvFEsyGDuTH050gS6aoOyrnK3Vh5ZzQ451Vj7I%2BvjAuUTm0fWljXuPvXHHJPkrbddeOlkT5diu1vt6JWwx561NvyjU8vQeGImwQjmoGv0uYjZeceT689KK3cb%2FBUHn7ivC0i8C%2Fz%2FGJ%2Fyhm&X-Amz-Signature=0d51d39cd52296c488591cca9c1e2a6a8042a97f20514fc47e028bf03456b791&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XQKH5ZU%2F20261022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261022T022534Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDnEY5lykVwRwwAv01enDXgZpmjcCifIybvsWroRPWvBAIgQF2yvk0kU2mQ%2BQA3U%2F0zzs%2FJTacfQ%2BTz8%2BCkz3s4A8kq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDFwstvpH%2FavuqTVbJSrcAyv0du7QMEDoJ5luqWKCm2pfouYgGaezfzNHDIcXDIh44PtGddOpoGY5fCQNoBSPMdTbonOHiqPzBTFkChVom8ET8n%2FdzDnYBPxhWyME76jUQB5q9HQNL9oPbKVVgD%2BBbeKyagzaQtd9Wak5dhv2pdi0Ari9RjvhWq6eUHNrk5XLrJfsoNX4fw3ZyNRRfMZCUjMTOt5bzAlRel1Q1Z%2Bqw3DEGeTKr5LNeoqSkOMDX8D5wLLTBy1axZeKQJFqBNkoM7yghum4y%2FbbEAjQTyl0X2KDV3qsyvR6F4bD5S95prTsyz8U5HAOQrXK1NAxV2wEQr109VjvWNsOHufG%2Fb5Nlp58pdReFjErp%2BwO2GDpA2%2FvCmBDLaiPP6ktKu548qmBZnlUbqtVh%2B02lS64R4SI38VzAgnlY14a953njV2mcV7MeosdE4M8Jbx%2F2LOBcPn30inlTXY0f0LbgztiE8yIEjggvuuEfSS776ZJb1OYt7AISf%2FbQmeRT8wMtDMCk9Ou3NK37zolf%2FZ65QSn%2BBk9%2Ft7IaIJ%2FoLmtJDE%2F6swX5F9QpCJ%2FezJwnYzJYW09sGU5rPzeXKCkQrtQupOHfD5m7JEVFJvYtTUhJCEbnMeIEgZDx8BTJQbI2yB6lw8pMNme5dYGOqUBiK6opqLm1uAKvnusUXRRyMXXDoHLqmWYWstqgo4zSJWtQ%2F5OLXT3P51j9MLPo8or0n8NGUjDPgB3mKvFEsyGDuTH050gS6aoOyrnK3Vh5ZzQ451Vj7I%2BvjAuUTm0fWljXuPvXHHJPkrbddeOlkT5diu1vt6JWwx561NvyjU8vQeGImwQjmoGv0uYjZeceT689KK3cb%2FBUHn7ivC0i8C%2Fz%2FGJ%2Fyhm&X-Amz-Signature=6a8bb9764891d5e5b1dfe0313addaaa71385a34a840b65dca4ae1fe540055ab9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XQKH5ZU%2F20261022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261022T022534Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDnEY5lykVwRwwAv01enDXgZpmjcCifIybvsWroRPWvBAIgQF2yvk0kU2mQ%2BQA3U%2F0zzs%2FJTacfQ%2BTz8%2BCkz3s4A8kq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDFwstvpH%2FavuqTVbJSrcAyv0du7QMEDoJ5luqWKCm2pfouYgGaezfzNHDIcXDIh44PtGddOpoGY5fCQNoBSPMdTbonOHiqPzBTFkChVom8ET8n%2FdzDnYBPxhWyME76jUQB5q9HQNL9oPbKVVgD%2BBbeKyagzaQtd9Wak5dhv2pdi0Ari9RjvhWq6eUHNrk5XLrJfsoNX4fw3ZyNRRfMZCUjMTOt5bzAlRel1Q1Z%2Bqw3DEGeTKr5LNeoqSkOMDX8D5wLLTBy1axZeKQJFqBNkoM7yghum4y%2FbbEAjQTyl0X2KDV3qsyvR6F4bD5S95prTsyz8U5HAOQrXK1NAxV2wEQr109VjvWNsOHufG%2Fb5Nlp58pdReFjErp%2BwO2GDpA2%2FvCmBDLaiPP6ktKu548qmBZnlUbqtVh%2B02lS64R4SI38VzAgnlY14a953njV2mcV7MeosdE4M8Jbx%2F2LOBcPn30inlTXY0f0LbgztiE8yIEjggvuuEfSS776ZJb1OYt7AISf%2FbQmeRT8wMtDMCk9Ou3NK37zolf%2FZ65QSn%2BBk9%2Ft7IaIJ%2FoLmtJDE%2F6swX5F9QpCJ%2FezJwnYzJYW09sGU5rPzeXKCkQrtQupOHfD5m7JEVFJvYtTUhJCEbnMeIEgZDx8BTJQbI2yB6lw8pMNme5dYGOqUBiK6opqLm1uAKvnusUXRRyMXXDoHLqmWYWstqgo4zSJWtQ%2F5OLXT3P51j9MLPo8or0n8NGUjDPgB3mKvFEsyGDuTH050gS6aoOyrnK3Vh5ZzQ451Vj7I%2BvjAuUTm0fWljXuPvXHHJPkrbddeOlkT5diu1vt6JWwx561NvyjU8vQeGImwQjmoGv0uYjZeceT689KK3cb%2FBUHn7ivC0i8C%2Fz%2FGJ%2Fyhm&X-Amz-Signature=610d9ba004532fbaa567bc39dc33eaa80225030b88388b43cb6ca0b22fc95210&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
