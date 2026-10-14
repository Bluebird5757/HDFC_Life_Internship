---
notion_id: 3c24fa76-9938-8029-85bd-c56966a1b3ac
notion_url: https://app.notion.com/p/Langchain-3c24fa769938802985bdc56966a1b3ac
title: Langchain
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-22T11:37:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-10-14T02:40:02.151Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

it is an open source framework that helps in building llm based applications. it provides modular components and end to end tools that help developers build complex ai applications, such as chatbots, question-answering system, rag, autonomous agents and more.

- supports all major LLMs
- simplifies developing LLM based applications
- integrations available for all major tools
- open source/free/actively developed
- supports all major GenAI use cases

## LLM API


in early 2015 there was a problem of semantic search which included NLU and Context Awareness Generation, what is semantic search and how does is it work:-

- its used to extract the output from a pdf or any file corresponding to the  user query
- first vectors are created for every chunk and they are stored in databases
- then a vector is created for the user query and a similarity score is generated between the user query and all the vectors and supposedly top five vector are chosen
- they are then combined with the user query and the whole combination is called system query which is then passed onto the brain(LLM API) which will give us the output

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XWL7MXJU%2F20261014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261014T023957Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDbz7vmVVdRt%2F4UNRm3knC7UaG9NdE2YLVFVHgrtwko1AiEAh7KbxSejzqdV1dtxC9HQvJFA4iCYgIytU4R2W%2B8F1PwqiAQIq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAbH9zAP1ToQluQE%2FyrcAzitkkU%2FwAimyn3k%2Bj%2FI%2BCDLEshfQtkTJ6OAzpUE%2B1oMOAAfsEgmzcbDXHLAcF8%2BRhsnITvwDneSd0jUNWelf68vtsV0Xx3kUlbz4MRG%2BTMzyHnf4OO9vbYnrH%2BSiID27OgHVG8F8u8bSkVdONMo88b6mazcx%2FFUqaWdB%2FaaMnD6BAaiv4%2BOojauych3ZTLZ0GUrKT%2FheMn5IPPkcakpBInduCONKY2LG5Jr6zmnd8Zx4Z5TjqpeGDX68nZgYZ%2F7mc7lsI0ypgntReDbJs8dCEQi0RN67d1acJRZCkxO25DVsYSp7i99Ywl0GbOAnyrQ9D2LzeFW0cU8rDeQarlMwkyYJikrT9qrk0WXa8G%2F9zw0yGgvmX7fEM7pNLR9vDnmDUJfMz4HwLQa4sD312qmSre6ORfs4Av4JFbj7SxEMfGvXxxmRqQ2ZZDnmSY8fdGfWUHTNXCWHI6%2FvEuuPvo0YzggBkGSBOU%2FGY56t%2BLlTLqOPAgIz%2B61Vo9Mi6lKpYg22AxwmW9gXAZdvqPR3LeZfMle%2B%2BXL0ngOOACkbr7jjxrBQ56VOwqhMzHglb64szgRn33jevGDeZjHditQhsQYcnP6bAEXv3C5Y70kWHB1vw1TZNuhDzXkIp6rYGA2MOm2u9YGOqUB%2B3VVmaUznQuntKGJvDr9%2B16MlWAX8g43HJ7nJO0J1sCNkLTt0bI8P5VtAVR1ouwvcwlL68Qikqx7wTivWSX6bbjrmbTVo3mXC6ufgnvMQtgBKb2Vyh5r9HUncNUoECT5%2FfEIDskTXNeI26qr9r1%2BOlUDYgJu5iRDCB4%2BQOspYsm0RZuMbHC3tAkPrcB3hb9tUijOyWt3NWCHBWmz%2FbMFPX3mLbxS&X-Amz-Signature=52a2a525fa23c8a28c7c586c2fcf4a06f3f8f68a6fb29741761a392e3d20dff9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


so first challenge was of creating the brain but it was solved when the Transformers paper was established or understanding the query and relevant text generation(BERT and GPT)  , 


second was of using those LLMs (either use open source or create your own foundational models) which is again a tedious task on the servers either from the engineer point of view or computational point of view so LLM APIs came which are LLMs hosted on the companies servers like google, openAI, anthropic and the users pay to use those apis, 


third was the orchestration meaning it was difficult to move so many components together like AWS and then text splitter and so on, so LangChain comes in to picture that handles all the orchestration and the interfacing


Langchain benefits:-

- Concept of chains
- Model Agnostic Development
- Complete ecosystem
- Memory and state handling

What can you build:-

- Conversational Chatbots
- AI knowledge Assitants
- AI Agents
- Workflow Automation
- Summarization/Research Helpers

## Components

- Models
    - They are the core interfaces through which you interact with AI models(any company) to standardize the code

    ```python
    #openai
    
    from langchain_openai import ChatOpenAI
    from dotenv import load_dotenv
    load_dotenv()
    
    model=ChatOpenAI(model="gpt-4",temperature=0)
    result=model.invoke("hi how are you")
    print(result.content)
    
    
    #anthropic
    
    from langchain_anthropic import ChatAnthropic
    from dotenv import load_dotenv
    load_dotenv()
    
    model=ChatAnthropic(model="claude-3-opus-20240229")
    result=model.invoke("hi how are you")
    print(result.invoke)
    ```

    - there are two types of models in langchain →language models(LLMs → text in text out) and embedding models(text in vector out (semantic search))
- Prompts
    - They are inputs provided to LLMs
    - They are of types
        - Dynamic and Reusable Prompts

        ```python
        from langchain_core.prompts import PromptTemplate
        prompt=PrompTemplate.from_tempplate('Summarize {topic} in {motion} tone')
        print(prompt.format(topic='Cricket',length='fun')
        ```

        - Role-Based Prompts

        ```python
        chat_prompt=ChartPromptTemplate.from_template([
        	('system',"Hi you are a experienced {professiom}"),
        	('user',"Tell me about {topic}"),
        ])
        
        formatted_messages=chat_prompt.format_messages(profession="doctor",topic="Viral Fever")
        ```

        - Few Shot Prompting

        ```python
        #step 1 example prompts
        examples=[
        	{"input":"I was charged twice for my subscription this month.","output":"Billing Issue"},
        	{"input":"The app crashes every time I try to log in.","output":"Technical Problem"}
        ]
        
        #step 2 create an example template
        example_template="""
        Ticket:{input}
        Category:{output}
        """
        
        #step 3 Build the few shot prompts template
        few_shot_template=FewShotPromptTemplate(
        	examples=examples,
        	example_prompt=PromptTemplate(input_variables=["input","output"],
        	template=example_template), 
        	prefix="Classify the following customer support tickets into one of the 
        	categories: 'Billing Issue','Technical Problem',or'General Inquiry'.\n\n",
        	suffix="\nTicket:{user_input}\nCategory:",
        	input_variables=["user_input"],
        )
        ```

- Chains
- Memory
- Indexes
    - Doc loader
    - Text Splitter
    - Vector store
    - Retrievers
- Agents
