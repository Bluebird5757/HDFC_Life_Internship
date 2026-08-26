---
notion_id: 3c54fa76-9938-8030-91a9-f07271d3b76e
notion_url: https://app.notion.com/p/Structured-outputs-and-output-parsers-3c54fa769938803091a9f07271d3b76e
title: Structured outputs and output parsers
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-24T19:36:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-08-26T00:40:27.753Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

The output we get from an llm can be structured in such a way that we can use it to further communicate with other llms or apis or tools like agents.


so there are models like openai that can return structured outputs with the help of with_structured_output which is built in langchain and we just have to specify the format of the output that we want for which we have three ways:-

- Pydantic
- TypeDict
- json_schema

but many of these models are closed source, for open source models on huggingface who cant return a structured outputs we have output parsers which help us turn a raw output from a LLM into structured formats like JSON, CSV, Pydantic models, and more. One thing to note is that output parsers can be used with both LLMs who can and cant return a structured outputs


so there are four output parsers:-

- StrOutputParser:- it is the simpliest output parser in langchain used to parse the output of a language model and return it as a plain string

```python
# suppose you want to extract a five line summary on the text being provided and the 
# result is also coming from the model
# without parser the code is 
from langchain_google_genai import ChatGoogleGenerativeAI
from dotenv import load_dotenv
from langchain_core.prompts import PromptTemplate
# from langchain_core.output_parsers import StrOutputParser

load_dotenv()

model=ChatGoogleGenerativeAI(model='gemini-3.6-flash')
template1=PromptTemplate(
    template='Write a detailed report on {topic}',
    input_variables=['topic']
)

template2=PromptTemplate(
    template='Write a five line summary on the following text. /n {text}',
    input_variables=['text']
)

prompt1=template1.invoke('Black Hole')
result=model.invoke(prompt1)
prompt2=template2.invoke(result.content[0]['text'])
result1=model.invoke(prompt2)
print(result1.content[0]['text'])

# with parser the code is
from langchain_google_genai import ChatGoogleGenerativeAI
from dotenv import load_dotenv
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import StrOutputParser

load_dotenv()

model=ChatGoogleGenerativeAI(model='gemini-3.6-flash')
template1=PromptTemplate(
    template='Write a detailed report on {topic}',
    input_variables=['topic']
)

template2=PromptTemplate(
    template='Write a five line summary on the following text. /n {text}',
    input_variables=['text']
)

parser=StrOutputParser()

chain = template1 | model | parser | template2 | model | parser

result=chain.invoke({'topic':'Black Hole'})

print(result)

# as you can see there with the help of chains and parser the code has become easier to understand and write
```

- Json output parsers

```python
# without chain
from langchain_google_genai import ChatGoogleGenerativeAI
from dotenv import load_dotenv
from langchain_core.output_parsers import JsonOutputParser
from langchain_core.prompts import PromptTemplate

load_dotenv()

model=ChatGoogleGenerativeAI(model='gemini-3.6-flash')
parser=JsonOutputParser()

template1=PromptTemplate(
    template='Give me 5 facts about {topic} \n {format_instruction}',
    input_variables=['topic'],
    partial_variables={'format_instruction':parser.get_format_instructions()}
)

prompt1=template1.format(topic='black hole')
result=model.invoke(prompt1)
final_result=parser.parse(result.content[0]['text'])
print(final_result)

#with chain
chain = template1 | model | parse
result=chain.invoke({'topic':'black hole'}) # if there hadn't been an input variable you 
																						# would have written result=chain.invoke({})
print(result)
```


the issue with jsonoutput parser is that it is not a good schema enforcer

- structured output parsers:- it is an output parser in langchain that helps extract structured JSON data from LLM responses based on predefined field schemas.

    It works by defining a list of fields (ResponseSchema) that model should return, ensuring the output follows a structured format

- Pydantic output parsers:- It is a structured output parser in langchain that uses Pydantic models to enforce schema validation when processing LLM responses

```python
from langchain_google_genai import ChatGoogleGenerativeAI
from dotenv import load_dotenv
from langchain_core.output_parsers import PydanticOutputParser
from langchain_core.prompts import PromptTemplate
from pydantic import BaseModel,Field

load_dotenv()

model=ChatGoogleGenerativeAI(model='gemini-3.6-flash')

class Person(BaseModel):
    name:str=Field(description='The name of the person')
    age:int=Field(gt=18,description='The age of the person')
    city:str=Field(description='The name of the city the person belongs to')

parser=PydanticOutputParser(pydantic_object=Person)\

template=PromptTemplate(
    template='Generate the name, age and city of a fictional {place} person \n {format_instruction}',
    input_variables=['place'],
    partial_variables={'format_instruction':parser.get_format_instructions()}
)

# prompt=template.invoke({'place':'indian'})
# result=model.invoke(prompt)
# final_result=parser.parse(result.content[0]['text'])
chain = template | model | parser
final_result=chain.invoke({'place':'british'})
print(final_result)
```
