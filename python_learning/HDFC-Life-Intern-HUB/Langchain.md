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
fetched_at: '2026-10-12T03:15:00.551Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46623ZKKLJA%2F20261012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261012T031456Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCuTKRReTzkuDdoWiydQ2lAEMXoG50gpwR1fEqyWTcNCwIgNyH8hkYebxl6K13AeQbhSabPLxWh5P4S0GiLbVNGfKQq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDJC05GZ5sxB4PfrSRyrcA5JIzRYwnpsERmzCvcdhg1F3iJUYKXL0%2FQgR08%2Fv1jPCOSRHIVTdXd0bBcLZOtEJZT5GYTDuLqjYUZvKaq5VVWFeSZT0Uz5qhxe7QN65PvuDpG3DTgmgRyDC8XkiWD4q2ch6%2BpJ9WIyWxUTBE5L3a%2FSlkvxs5nAoQU8mrRJLflkYCLWpYGhVupTUqleXm%2FRZzWx2pzTkC5LWy0lhyJIRisAynnZtGY45nx%2BePhty93rPG3Ek6rz1THJ8KFvJQSL2yFl79V8qjY1wP3ptx8J%2BhTC5Ss%2BTUx01uvrGpqNn%2BL6U7t3HqoKsFmDc5f3homFH7PPTiDZHiz9G9WgVM3WZrLgXZwKFNBUFavXKmj%2FTlUrrrFetT8Rz7wBu5%2BL0VgGIUaJwB%2Bn8pLz0zuwd4HGF%2BZnomUBOVwt2acy3XJnG0Sc4sEHjtD%2BwGx0vRejFw78y1%2BRemd8feAlTCn%2FobuIzMY5fZlDVaApw1%2Fk5vZcdPWZPP8SYNOzH3%2FgPmWdz7QqpK9LnLqrMbaDsE10zZvS8%2Fd5q8uf6XZnPcE47I3lwThHiRSdtV1v%2FXIoUYZYUzjkmXIUUdeMIU25d6UzU8gI7araRwaJzfhj6QzvplK5MJDWXj6sU3Hw4Jvir8KW%2FMJvzsNYGOqUBVvBY6EqaEBN4TrT5eHOpfGCWtt1HPQX9UqjEoWcStw5MjQBXmNB4OE040PtjCzlrAD4wPDm2dGT%2BpdWJN5RYP2cWT1Jqqy139j0VxXHtRqsxha3OGpBzlC1me6AGYnK8AuJEwWszSMTpSmxSDtgNgXwdoXaR7JWvyxpvKXOoUCpS%2FjOYtve2gTEBgtL9MZaN9F%2FUej1A84%2B7Slv1MV%2F2jbs33tY8&X-Amz-Signature=4b67f1416b00ce49866577465580b8369bda78f7f006b4e21201ab0880fdb156&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
