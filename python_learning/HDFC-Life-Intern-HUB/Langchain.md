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
fetched_at: '2026-10-03T02:52:51.164Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646YQWO22%2F20261003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261003T025244Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDW8IrgMQCDe85y2MyxEA7y93xZsSXod2dIRIKiQdGUBgIgaAMFEKVJZL02aH1KyIpVnR7O9sZrQMumUeGgGX1ZnyIqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMr782fdSZSOCFEtACrcA2ORvbng9fE3MQD2ikqpqCPLTRB1dTs3i67EAPnhgWJe%2FKxBhKYEURMfWuuFqRuFtdTEYrpoFKu2MRgWGIBSlYnRg2C7OGJFIlZc2PtAuxqMGFbjxBcz%2FLm4iSfTeg6V%2F608RobCWO7hxqliEpyRnxgWlEYxLFodvnxuw8o9sCFlfSuTZWusLb8ucIddN7%2FiTBx%2FaNL9ruaFraJQez3vS8OgsOoVKIJ20C2c3BlZzF9P0DmZ6pr%2BqZ3QGaqudFvAtZhFcckSGZVoK%2FopsgdMzTgQHAMQtq022bSbtGCUnDj98x2b08p72yy%2Bo5vnFY%2Bl9f6fc1x3XLLTljDUEXbQkzwr3ByIJBMrnzTzxiOxkbR74ca5A3N61M3Z6NO6fahh4%2BdmpcnY3yjYIBMtBX9MtJxK2bUWKDikLZv55zhlMsJYyqW9mw2MGgoQlenPkYpqw4cgHHms43Vw0LFNLC4%2FwQEWkpDs9cWWsItdsG4Klj45ZcF2gb6VOfk%2BBZlJvwzefPcFHWCQfY2Yb%2F3ojAFtKpB4YAtTrVt6OwrVICK6Ii35hS6CLlnPWukWTT%2FBob76eUDheWBdu%2FtEzqV1bcm4ADc1gikQqgPOrHWTciVqu9aQD2kAcQ3gB9w4qexkMM7PgdYGOqUBFwWMbSv%2BRl07YZDO%2FEM3l2OhMCtR2%2Fr8QEVobxagZwlPgti%2B4IO8z%2BXxUQ9isLzhXIh1EUALnGiLKhh4aLo%2FE5II84tq%2F6LGVuAHYcBV%2BZ7FjfD6qVzuMSLAVkhWt8xXDgImZkmAuFlJo%2BLmc%2BMD9wfI7rwp2W4wPdg2Bh9M%2Frr8MwLcsHciBTuRj9qrysWK%2FWuKn%2FlgUannp3%2B%2BrMYgfdPlBe7k&X-Amz-Signature=1e165b7cfa698827cb92fbbc91d045f157534259a1ccf9a9baa4f2f5a180f48d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
