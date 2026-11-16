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
fetched_at: '2026-11-16T03:47:11.681Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGG4JRXV%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034705Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGL0CSNM%2BrKIJUCMUB0MedSyTRhuCYKllMZrjYwdQufsAiEA0CjueakAQ0w7UPhVewznPp2Tc7WbxgmeWMZeaFNwD1QqiAQIw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHnY1GEBTQ4wrU9WdSrcA9EDXKnHK%2FKm7bBmw43uGnXoKX5Wu5ED0%2FauGu01LYZh4l7oMNsSsZH821ysfVmXdAQQeojFPZOu%2Bm3napqxvFFOcBSzQ4QQUTD9%2FXtg3kZjbwTjHUc6GIDQk3HfHKLG6N5BA54MR34vedQ73Av79BDd0OL6DQZTCTvN5u5FM1Nf0lJU%2FZaEMeAVovZ2XLhyeDWdfmY2ohm0JMTz8WU%2Bb%2BmvL7qqqiB92HfzAxZ%2BA7gQ3zMYQ8fWF19UXHIJC3M4HLSew7sU09V9Fm9vdPLHTUaGSND8270c1%2BPP%2Bn0bmbH0vq%2FRc89MB1xvLEVKW3JU4fCH%2FTRyXLvBldclwQB3xFGVciswXBydl07NQaJOu7RmbuhOPXyKCUsfljyQPqkXVSLpJ3JkzagZyoUVINDRhOZCQvC1oHdB1rNhotEmV3pBiZUnl24XlES5SxHxB6rFe31tGbElFSWRcP%2FUid2ze%2Bcsjz2TnFgrcNbsQpDGSlabz5JgJcoDbiYrcrE9SsdbvrD623d4DMatDoduMpm%2BS9M%2B9S3m2teZO5QuLYx41Jkb846TzgvWzhVnsYAxUGrUGgF9zyv0tFOy2Lyq0o%2BhhcRuzqqS7hQObdmijN2yty5LYoaMRv%2BdqPh3TkLUMO%2FJ6dcGOqUBUN7sU2shKt6YwEWH4znwGeJyjn1kTRDbmY57SboXxmmL%2BjlY4u0UHjAOtttZrnj3AfJOxYeh7DhFsMnRqGk1r2SJgLLjK8r8cmKYPNgNlOIVvHIkaNW%2FWCPHICqAW7Gjotz6CjoucFSmU90nnw1ag8kKQdKZJNEhMjmhnFbhLKDO4mkZni5BV7MH9E4CGwgR4AbqszQpZ4qWnBrNOnJFn3%2FbqkB0&X-Amz-Signature=d9ac80a7071e7592a8a6d920de1ac4d2593a3ba8341f1454aaf6ba515fd4d40a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
