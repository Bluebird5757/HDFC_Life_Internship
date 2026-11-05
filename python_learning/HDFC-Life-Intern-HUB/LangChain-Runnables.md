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
fetched_at: '2026-11-05T04:12:24.773Z'
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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f594613c-b0e5-42cc-b2a7-c9612249366a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SS3VSGTU%2F20261105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261105T041220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHgmRThSuqM9ab6FhOaN9dOwrT1KUTM6UwKrCjS92MV%2FAiBPP5GpfB1TMDMCb9nXCHZS%2FqKlLXJan%2Fw%2BtowPRJmcmiqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcmZPwefLpEAmKfLMKtwD75p%2Fz%2BgS4USmKHdSkN6k761kph9L6Jz30z9%2FtNFM3HiFWe7omzJ9V61QlvdApPe8iSICHpN123SueBNPIcyQWRAMhcS6elXj%2F%2Bt7RX11InOB30vAXrr7d8qjm54zQzfbuOsz7s02BrCVacyPAe%2BU7G%2B6YaGjSg5%2BozY9EFHtznYZxdLEv5rXu2ymV9QjONN0Z8DgwG2NOFwPZtBsTdZCZ%2FU3ujCnW63%2BcsRad392HlL2sIjJaYNsA4xxRSIeYy7lNB5Ax70xYslIH5zRSHr0ItJznltV%2B6DWTKxaHx1%2BJeDX7iKWQclmY72hGp8DqNW6qsmmrQMf4lO5vUmGvRv05I3yqU%2FKXsVWnQzmc5R1sZk6P69KemSSzsR9z3Jst6Pw9qgwLsTrkLOL6wkAcX7o3i4g%2FS3PZ1IDW8%2BF9QKCAQdcM5P6kCs2qNHTMgeRDIwcxzYNvlrc3BYIzUvP5xLwYUoU2%2FUre286AAboPkKH0TBVDZZ8pKtEGIAh2jhAXyZGFgzIb08GcniV%2FXYjX8xMCa9ruKsC33hTEH3Ovc%2FymAi0Rq8WUuOO%2BbxhI0AnV3viqV15mh4D0DIJ6Ambc1nZ1nlRHRDlhR4Av5PRZ7ocPbJWDRhpKwsNdVJxzrww5O%2Bv1wY6pgGxG0T8juO%2FLdbeku%2FXrFptLf2fWFlufh%2B%2BejbREs3GNyzMI3mli%2FGPGnVh2XwS9yuoK3us9fLpq1zTRSAuKa1v0aJ9tFWMuQ138SEYypX6bj5BKFka9lTczqMLtizFfXTHJuE9gXu0SgDAxazIw4Nwglhnlfnd8ZCGAj1zv9zxFOZKIF7usXPcLFzUYSB6csPB%2BJBovYHLEfAZUaNquegHIyKQTRMG&X-Amz-Signature=fcb2945dc0015fce458301b7c322bac57bc6f6de2142223d4fbfafd4e981f7ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/8764313f-ce9d-4009-a291-9c2ad9ecc998/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SS3VSGTU%2F20261105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261105T041220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHgmRThSuqM9ab6FhOaN9dOwrT1KUTM6UwKrCjS92MV%2FAiBPP5GpfB1TMDMCb9nXCHZS%2FqKlLXJan%2Fw%2BtowPRJmcmiqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcmZPwefLpEAmKfLMKtwD75p%2Fz%2BgS4USmKHdSkN6k761kph9L6Jz30z9%2FtNFM3HiFWe7omzJ9V61QlvdApPe8iSICHpN123SueBNPIcyQWRAMhcS6elXj%2F%2Bt7RX11InOB30vAXrr7d8qjm54zQzfbuOsz7s02BrCVacyPAe%2BU7G%2B6YaGjSg5%2BozY9EFHtznYZxdLEv5rXu2ymV9QjONN0Z8DgwG2NOFwPZtBsTdZCZ%2FU3ujCnW63%2BcsRad392HlL2sIjJaYNsA4xxRSIeYy7lNB5Ax70xYslIH5zRSHr0ItJznltV%2B6DWTKxaHx1%2BJeDX7iKWQclmY72hGp8DqNW6qsmmrQMf4lO5vUmGvRv05I3yqU%2FKXsVWnQzmc5R1sZk6P69KemSSzsR9z3Jst6Pw9qgwLsTrkLOL6wkAcX7o3i4g%2FS3PZ1IDW8%2BF9QKCAQdcM5P6kCs2qNHTMgeRDIwcxzYNvlrc3BYIzUvP5xLwYUoU2%2FUre286AAboPkKH0TBVDZZ8pKtEGIAh2jhAXyZGFgzIb08GcniV%2FXYjX8xMCa9ruKsC33hTEH3Ovc%2FymAi0Rq8WUuOO%2BbxhI0AnV3viqV15mh4D0DIJ6Ambc1nZ1nlRHRDlhR4Av5PRZ7ocPbJWDRhpKwsNdVJxzrww5O%2Bv1wY6pgGxG0T8juO%2FLdbeku%2FXrFptLf2fWFlufh%2B%2BejbREs3GNyzMI3mli%2FGPGnVh2XwS9yuoK3us9fLpq1zTRSAuKa1v0aJ9tFWMuQ138SEYypX6bj5BKFka9lTczqMLtizFfXTHJuE9gXu0SgDAxazIw4Nwglhnlfnd8ZCGAj1zv9zxFOZKIF7usXPcLFzUYSB6csPB%2BJBovYHLEfAZUaNquegHIyKQTRMG&X-Amz-Signature=d9641065b4aced40c914c6f2c28a2081a896e13c31838aa0bc6e01271d8bcffe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/53498049-019f-4c90-ac60-75e3dee65ea0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SS3VSGTU%2F20261105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261105T041220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHgmRThSuqM9ab6FhOaN9dOwrT1KUTM6UwKrCjS92MV%2FAiBPP5GpfB1TMDMCb9nXCHZS%2FqKlLXJan%2Fw%2BtowPRJmcmiqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcmZPwefLpEAmKfLMKtwD75p%2Fz%2BgS4USmKHdSkN6k761kph9L6Jz30z9%2FtNFM3HiFWe7omzJ9V61QlvdApPe8iSICHpN123SueBNPIcyQWRAMhcS6elXj%2F%2Bt7RX11InOB30vAXrr7d8qjm54zQzfbuOsz7s02BrCVacyPAe%2BU7G%2B6YaGjSg5%2BozY9EFHtznYZxdLEv5rXu2ymV9QjONN0Z8DgwG2NOFwPZtBsTdZCZ%2FU3ujCnW63%2BcsRad392HlL2sIjJaYNsA4xxRSIeYy7lNB5Ax70xYslIH5zRSHr0ItJznltV%2B6DWTKxaHx1%2BJeDX7iKWQclmY72hGp8DqNW6qsmmrQMf4lO5vUmGvRv05I3yqU%2FKXsVWnQzmc5R1sZk6P69KemSSzsR9z3Jst6Pw9qgwLsTrkLOL6wkAcX7o3i4g%2FS3PZ1IDW8%2BF9QKCAQdcM5P6kCs2qNHTMgeRDIwcxzYNvlrc3BYIzUvP5xLwYUoU2%2FUre286AAboPkKH0TBVDZZ8pKtEGIAh2jhAXyZGFgzIb08GcniV%2FXYjX8xMCa9ruKsC33hTEH3Ovc%2FymAi0Rq8WUuOO%2BbxhI0AnV3viqV15mh4D0DIJ6Ambc1nZ1nlRHRDlhR4Av5PRZ7ocPbJWDRhpKwsNdVJxzrww5O%2Bv1wY6pgGxG0T8juO%2FLdbeku%2FXrFptLf2fWFlufh%2B%2BejbREs3GNyzMI3mli%2FGPGnVh2XwS9yuoK3us9fLpq1zTRSAuKa1v0aJ9tFWMuQ138SEYypX6bj5BKFka9lTczqMLtizFfXTHJuE9gXu0SgDAxazIw4Nwglhnlfnd8ZCGAj1zv9zxFOZKIF7usXPcLFzUYSB6csPB%2BJBovYHLEfAZUaNquegHIyKQTRMG&X-Amz-Signature=6133c9cf88e4a9cabbcb2f7370b248c18782042bbe77efcab26ce6c3e24a8de4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/f5d2802c-69e7-4aa3-8098-360e521dcfbf/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SS3VSGTU%2F20261105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261105T041220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHgmRThSuqM9ab6FhOaN9dOwrT1KUTM6UwKrCjS92MV%2FAiBPP5GpfB1TMDMCb9nXCHZS%2FqKlLXJan%2Fw%2BtowPRJmcmiqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcmZPwefLpEAmKfLMKtwD75p%2Fz%2BgS4USmKHdSkN6k761kph9L6Jz30z9%2FtNFM3HiFWe7omzJ9V61QlvdApPe8iSICHpN123SueBNPIcyQWRAMhcS6elXj%2F%2Bt7RX11InOB30vAXrr7d8qjm54zQzfbuOsz7s02BrCVacyPAe%2BU7G%2B6YaGjSg5%2BozY9EFHtznYZxdLEv5rXu2ymV9QjONN0Z8DgwG2NOFwPZtBsTdZCZ%2FU3ujCnW63%2BcsRad392HlL2sIjJaYNsA4xxRSIeYy7lNB5Ax70xYslIH5zRSHr0ItJznltV%2B6DWTKxaHx1%2BJeDX7iKWQclmY72hGp8DqNW6qsmmrQMf4lO5vUmGvRv05I3yqU%2FKXsVWnQzmc5R1sZk6P69KemSSzsR9z3Jst6Pw9qgwLsTrkLOL6wkAcX7o3i4g%2FS3PZ1IDW8%2BF9QKCAQdcM5P6kCs2qNHTMgeRDIwcxzYNvlrc3BYIzUvP5xLwYUoU2%2FUre286AAboPkKH0TBVDZZ8pKtEGIAh2jhAXyZGFgzIb08GcniV%2FXYjX8xMCa9ruKsC33hTEH3Ovc%2FymAi0Rq8WUuOO%2BbxhI0AnV3viqV15mh4D0DIJ6Ambc1nZ1nlRHRDlhR4Av5PRZ7ocPbJWDRhpKwsNdVJxzrww5O%2Bv1wY6pgGxG0T8juO%2FLdbeku%2FXrFptLf2fWFlufh%2B%2BejbREs3GNyzMI3mli%2FGPGnVh2XwS9yuoK3us9fLpq1zTRSAuKa1v0aJ9tFWMuQ138SEYypX6bj5BKFka9lTczqMLtizFfXTHJuE9gXu0SgDAxazIw4Nwglhnlfnd8ZCGAj1zv9zxFOZKIF7usXPcLFzUYSB6csPB%2BJBovYHLEfAZUaNquegHIyKQTRMG&X-Amz-Signature=fb02cf147a417cbf07df192b4f9c7100c9ed529c296f98b4ed6eb34f55b9a327&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/0937fb48-b760-420d-b65a-b46d547099f3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SS3VSGTU%2F20261105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261105T041220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHgmRThSuqM9ab6FhOaN9dOwrT1KUTM6UwKrCjS92MV%2FAiBPP5GpfB1TMDMCb9nXCHZS%2FqKlLXJan%2Fw%2BtowPRJmcmiqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcmZPwefLpEAmKfLMKtwD75p%2Fz%2BgS4USmKHdSkN6k761kph9L6Jz30z9%2FtNFM3HiFWe7omzJ9V61QlvdApPe8iSICHpN123SueBNPIcyQWRAMhcS6elXj%2F%2Bt7RX11InOB30vAXrr7d8qjm54zQzfbuOsz7s02BrCVacyPAe%2BU7G%2B6YaGjSg5%2BozY9EFHtznYZxdLEv5rXu2ymV9QjONN0Z8DgwG2NOFwPZtBsTdZCZ%2FU3ujCnW63%2BcsRad392HlL2sIjJaYNsA4xxRSIeYy7lNB5Ax70xYslIH5zRSHr0ItJznltV%2B6DWTKxaHx1%2BJeDX7iKWQclmY72hGp8DqNW6qsmmrQMf4lO5vUmGvRv05I3yqU%2FKXsVWnQzmc5R1sZk6P69KemSSzsR9z3Jst6Pw9qgwLsTrkLOL6wkAcX7o3i4g%2FS3PZ1IDW8%2BF9QKCAQdcM5P6kCs2qNHTMgeRDIwcxzYNvlrc3BYIzUvP5xLwYUoU2%2FUre286AAboPkKH0TBVDZZ8pKtEGIAh2jhAXyZGFgzIb08GcniV%2FXYjX8xMCa9ruKsC33hTEH3Ovc%2FymAi0Rq8WUuOO%2BbxhI0AnV3viqV15mh4D0DIJ6Ambc1nZ1nlRHRDlhR4Av5PRZ7ocPbJWDRhpKwsNdVJxzrww5O%2Bv1wY6pgGxG0T8juO%2FLdbeku%2FXrFptLf2fWFlufh%2B%2BejbREs3GNyzMI3mli%2FGPGnVh2XwS9yuoK3us9fLpq1zTRSAuKa1v0aJ9tFWMuQ138SEYypX6bj5BKFka9lTczqMLtizFfXTHJuE9gXu0SgDAxazIw4Nwglhnlfnd8ZCGAj1zv9zxFOZKIF7usXPcLFzUYSB6csPB%2BJBovYHLEfAZUaNquegHIyKQTRMG&X-Amz-Signature=2b44c8fe3b1d67548079871544c4c2afcde1ba01e9613919c69a89731ecd4df0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![Screenshot_2026-08-26_232834.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/069680cc-03b0-4eac-b9db-bcc4495e9b78/Screenshot_2026-08-26_232834.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SS3VSGTU%2F20261105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261105T041220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHgmRThSuqM9ab6FhOaN9dOwrT1KUTM6UwKrCjS92MV%2FAiBPP5GpfB1TMDMCb9nXCHZS%2FqKlLXJan%2Fw%2BtowPRJmcmiqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcmZPwefLpEAmKfLMKtwD75p%2Fz%2BgS4USmKHdSkN6k761kph9L6Jz30z9%2FtNFM3HiFWe7omzJ9V61QlvdApPe8iSICHpN123SueBNPIcyQWRAMhcS6elXj%2F%2Bt7RX11InOB30vAXrr7d8qjm54zQzfbuOsz7s02BrCVacyPAe%2BU7G%2B6YaGjSg5%2BozY9EFHtznYZxdLEv5rXu2ymV9QjONN0Z8DgwG2NOFwPZtBsTdZCZ%2FU3ujCnW63%2BcsRad392HlL2sIjJaYNsA4xxRSIeYy7lNB5Ax70xYslIH5zRSHr0ItJznltV%2B6DWTKxaHx1%2BJeDX7iKWQclmY72hGp8DqNW6qsmmrQMf4lO5vUmGvRv05I3yqU%2FKXsVWnQzmc5R1sZk6P69KemSSzsR9z3Jst6Pw9qgwLsTrkLOL6wkAcX7o3i4g%2FS3PZ1IDW8%2BF9QKCAQdcM5P6kCs2qNHTMgeRDIwcxzYNvlrc3BYIzUvP5xLwYUoU2%2FUre286AAboPkKH0TBVDZZ8pKtEGIAh2jhAXyZGFgzIb08GcniV%2FXYjX8xMCa9ruKsC33hTEH3Ovc%2FymAi0Rq8WUuOO%2BbxhI0AnV3viqV15mh4D0DIJ6Ambc1nZ1nlRHRDlhR4Av5PRZ7ocPbJWDRhpKwsNdVJxzrww5O%2Bv1wY6pgGxG0T8juO%2FLdbeku%2FXrFptLf2fWFlufh%2B%2BejbREs3GNyzMI3mli%2FGPGnVh2XwS9yuoK3us9fLpq1zTRSAuKa1v0aJ9tFWMuQ138SEYypX6bj5BKFka9lTczqMLtizFfXTHJuE9gXu0SgDAxazIw4Nwglhnlfnd8ZCGAj1zv9zxFOZKIF7usXPcLFzUYSB6csPB%2BJBovYHLEfAZUaNquegHIyKQTRMG&X-Amz-Signature=5cb0fc5466f0e71a599ef1094de6e95b20094b5e76e9418f6aa18e5aa51aa8d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/21da433f-e59b-4ef2-bd2e-5dcf5bf609fe/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SS3VSGTU%2F20261105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261105T041220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHgmRThSuqM9ab6FhOaN9dOwrT1KUTM6UwKrCjS92MV%2FAiBPP5GpfB1TMDMCb9nXCHZS%2FqKlLXJan%2Fw%2BtowPRJmcmiqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcmZPwefLpEAmKfLMKtwD75p%2Fz%2BgS4USmKHdSkN6k761kph9L6Jz30z9%2FtNFM3HiFWe7omzJ9V61QlvdApPe8iSICHpN123SueBNPIcyQWRAMhcS6elXj%2F%2Bt7RX11InOB30vAXrr7d8qjm54zQzfbuOsz7s02BrCVacyPAe%2BU7G%2B6YaGjSg5%2BozY9EFHtznYZxdLEv5rXu2ymV9QjONN0Z8DgwG2NOFwPZtBsTdZCZ%2FU3ujCnW63%2BcsRad392HlL2sIjJaYNsA4xxRSIeYy7lNB5Ax70xYslIH5zRSHr0ItJznltV%2B6DWTKxaHx1%2BJeDX7iKWQclmY72hGp8DqNW6qsmmrQMf4lO5vUmGvRv05I3yqU%2FKXsVWnQzmc5R1sZk6P69KemSSzsR9z3Jst6Pw9qgwLsTrkLOL6wkAcX7o3i4g%2FS3PZ1IDW8%2BF9QKCAQdcM5P6kCs2qNHTMgeRDIwcxzYNvlrc3BYIzUvP5xLwYUoU2%2FUre286AAboPkKH0TBVDZZ8pKtEGIAh2jhAXyZGFgzIb08GcniV%2FXYjX8xMCa9ruKsC33hTEH3Ovc%2FymAi0Rq8WUuOO%2BbxhI0AnV3viqV15mh4D0DIJ6Ambc1nZ1nlRHRDlhR4Av5PRZ7ocPbJWDRhpKwsNdVJxzrww5O%2Bv1wY6pgGxG0T8juO%2FLdbeku%2FXrFptLf2fWFlufh%2B%2BejbREs3GNyzMI3mli%2FGPGnVh2XwS9yuoK3us9fLpq1zTRSAuKa1v0aJ9tFWMuQ138SEYypX6bj5BKFka9lTczqMLtizFfXTHJuE9gXu0SgDAxazIw4Nwglhnlfnd8ZCGAj1zv9zxFOZKIF7usXPcLFzUYSB6csPB%2BJBovYHLEfAZUaNquegHIyKQTRMG&X-Amz-Signature=acefb687229444f200681be66522cc4e76202f8167f54591f5d363985355fcc1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/16d6bc10-e7f3-4733-9980-c9ddf44d2c18/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SS3VSGTU%2F20261105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261105T041220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHgmRThSuqM9ab6FhOaN9dOwrT1KUTM6UwKrCjS92MV%2FAiBPP5GpfB1TMDMCb9nXCHZS%2FqKlLXJan%2Fw%2BtowPRJmcmiqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcmZPwefLpEAmKfLMKtwD75p%2Fz%2BgS4USmKHdSkN6k761kph9L6Jz30z9%2FtNFM3HiFWe7omzJ9V61QlvdApPe8iSICHpN123SueBNPIcyQWRAMhcS6elXj%2F%2Bt7RX11InOB30vAXrr7d8qjm54zQzfbuOsz7s02BrCVacyPAe%2BU7G%2B6YaGjSg5%2BozY9EFHtznYZxdLEv5rXu2ymV9QjONN0Z8DgwG2NOFwPZtBsTdZCZ%2FU3ujCnW63%2BcsRad392HlL2sIjJaYNsA4xxRSIeYy7lNB5Ax70xYslIH5zRSHr0ItJznltV%2B6DWTKxaHx1%2BJeDX7iKWQclmY72hGp8DqNW6qsmmrQMf4lO5vUmGvRv05I3yqU%2FKXsVWnQzmc5R1sZk6P69KemSSzsR9z3Jst6Pw9qgwLsTrkLOL6wkAcX7o3i4g%2FS3PZ1IDW8%2BF9QKCAQdcM5P6kCs2qNHTMgeRDIwcxzYNvlrc3BYIzUvP5xLwYUoU2%2FUre286AAboPkKH0TBVDZZ8pKtEGIAh2jhAXyZGFgzIb08GcniV%2FXYjX8xMCa9ruKsC33hTEH3Ovc%2FymAi0Rq8WUuOO%2BbxhI0AnV3viqV15mh4D0DIJ6Ambc1nZ1nlRHRDlhR4Av5PRZ7ocPbJWDRhpKwsNdVJxzrww5O%2Bv1wY6pgGxG0T8juO%2FLdbeku%2FXrFptLf2fWFlufh%2B%2BejbREs3GNyzMI3mli%2FGPGnVh2XwS9yuoK3us9fLpq1zTRSAuKa1v0aJ9tFWMuQ138SEYypX6bj5BKFka9lTczqMLtizFfXTHJuE9gXu0SgDAxazIw4Nwglhnlfnd8ZCGAj1zv9zxFOZKIF7usXPcLFzUYSB6csPB%2BJBovYHLEfAZUaNquegHIyKQTRMG&X-Amz-Signature=abff181791f66a1d5a1647c9b33582e0da414b083d646e74949d8253bd69d445&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


here in runnable branch the syntax is:- 


```python
RunnableBranch(
	(condition,if true do this runnable),
	(condition,if true do this runnable),
	default behaviour
)
```


## LCEL (LangChain Expression Language)


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/aa47ea71-89c7-41e4-9ef6-507c88a11a48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SS3VSGTU%2F20261105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261105T041220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHgmRThSuqM9ab6FhOaN9dOwrT1KUTM6UwKrCjS92MV%2FAiBPP5GpfB1TMDMCb9nXCHZS%2FqKlLXJan%2Fw%2BtowPRJmcmiqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcmZPwefLpEAmKfLMKtwD75p%2Fz%2BgS4USmKHdSkN6k761kph9L6Jz30z9%2FtNFM3HiFWe7omzJ9V61QlvdApPe8iSICHpN123SueBNPIcyQWRAMhcS6elXj%2F%2Bt7RX11InOB30vAXrr7d8qjm54zQzfbuOsz7s02BrCVacyPAe%2BU7G%2B6YaGjSg5%2BozY9EFHtznYZxdLEv5rXu2ymV9QjONN0Z8DgwG2NOFwPZtBsTdZCZ%2FU3ujCnW63%2BcsRad392HlL2sIjJaYNsA4xxRSIeYy7lNB5Ax70xYslIH5zRSHr0ItJznltV%2B6DWTKxaHx1%2BJeDX7iKWQclmY72hGp8DqNW6qsmmrQMf4lO5vUmGvRv05I3yqU%2FKXsVWnQzmc5R1sZk6P69KemSSzsR9z3Jst6Pw9qgwLsTrkLOL6wkAcX7o3i4g%2FS3PZ1IDW8%2BF9QKCAQdcM5P6kCs2qNHTMgeRDIwcxzYNvlrc3BYIzUvP5xLwYUoU2%2FUre286AAboPkKH0TBVDZZ8pKtEGIAh2jhAXyZGFgzIb08GcniV%2FXYjX8xMCa9ruKsC33hTEH3Ovc%2FymAi0Rq8WUuOO%2BbxhI0AnV3viqV15mh4D0DIJ6Ambc1nZ1nlRHRDlhR4Av5PRZ7ocPbJWDRhpKwsNdVJxzrww5O%2Bv1wY6pgGxG0T8juO%2FLdbeku%2FXrFptLf2fWFlufh%2B%2BejbREs3GNyzMI3mli%2FGPGnVh2XwS9yuoK3us9fLpq1zTRSAuKa1v0aJ9tFWMuQ138SEYypX6bj5BKFka9lTczqMLtizFfXTHJuE9gXu0SgDAxazIw4Nwglhnlfnd8ZCGAj1zv9zxFOZKIF7usXPcLFzUYSB6csPB%2BJBovYHLEfAZUaNquegHIyKQTRMG&X-Amz-Signature=4493ac5b883e6e5787243aaa2e7f20c58fcb5cc9d675d9513e11ae065895b31e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
