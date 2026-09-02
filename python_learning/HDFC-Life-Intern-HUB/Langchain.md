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
fetched_at: '2026-09-02T01:56:38.353Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DBPYSML%2F20260902%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260902T015633Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAKd5oRK%2FEEpCth%2B2W6s5ARL7fuczYMDiA8PzVEtuVCaAiEA20Oxo9DkLHw5Bt69QTGV7YFCD4EPg5FNKyAbgJW5E2cqiAQIuv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMuvui17BOJ%2Bb5ppzCrcA4Ns7%2F9dMBRfNcTA1l98%2B5GyNK65riReRjE499CRVFxTPe2CR3ohH83MUNwR0qhEgUjaq0XhooZSkUbp8cMdk2CUnf92gvuLN5BvLtFlKNspRFOaFGkn23bLwwO5X7jTOrai2x4Xu%2BUYf9LlbFNUR4Ie9zq9hgke8NSgwBAOs6ou%2B5JvT3NLMknwv8C0t04KbfM2BXNwakKMqLZ0TxnmlX1eR1PRBbCoNY%2BvOfNNyYJC%2BjndGeqDFVxZTug9jOb%2FuupV%2FxhFshVBR%2BljApcLdh1RCIvF19uPwgHKFbJOXyucxTJM1xWhmiK2KOYRSOR2rj7wPzGXAybEf%2FfaUjY6JZL4XATTyn2xqebwvk9I%2F9X3GouoDUVRTJ2kDgWWFfL7%2BeB4Dn8AApyB5UBezcCDZulrES4yrxe%2BrvTOuus1cgKM4QpTwYPuRQhcRhCIUV7OrxLfnPZOPa%2FYzhPl9FXjhoXZngLSYAJf4nWJmKM1FRRqu%2Bd5zoKctQzRTcEQIs%2FmXU%2Fpil5LofEfcG7sWv%2BkDj608vDTmPGHVGLZKnkH0IrclyBWUS0iNE3R2jZWabub7QkBGLt%2Fajsz5ZPNkQpwpx8xThqO9v30E%2B8MXCkmLykyodF5xLXcDk8EnzkbMM3k3dQGOqUBoppQIEQD7KgHZnC7zp16js63lj7crogEd%2FJ4X6LcvsQ3Ur%2FNMxjt0EGP3gNVqY%2Fh3fTiain%2Fby499dXv3zId4zOD91%2F5vA%2BmjjsHJ47UxQyVLFCjGZl5UBjuwcwJl%2BIpeX5tBX8u%2BlFoTwf0fJX%2Fn5vOtCdCJ1Cp2c8DVsHD0AE4z6C376gZCFoShmaQ4PdPOwqUj8r5mZw537NoJx7BPXe8Hhui&X-Amz-Signature=1374f23aac29aa4e60ed4c30598375e3bec40ef6c391f21309c005506b633fa0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
