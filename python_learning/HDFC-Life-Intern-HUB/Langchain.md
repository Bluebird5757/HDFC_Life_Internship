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
fetched_at: '2026-09-10T02:03:34.212Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46676MBQQTC%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020328Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHPJ%2FO1hDICRNJYqlwTKbwPcopFhrAldKfJFboFkpsB4AiAWVp6BWZyqfbXHxf8BqKgCzeVSK5cAGAfbQGy%2Bc40%2FdCr%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMGotQKQikQ0TgqY2rKtwDRXdCOUPB2Y3X6%2FPr3ADczYTCYApfQojMVJkfkTGmrWQpzRdDuQ%2FYA9VLmAVNcpUza3L4b7ZkkXioY14FZfyddWWPy6czTHAcQeX9uRY2YG0VdLy%2B0OOAySp7qT8cErc6x11KnX9Tbndjl0cJC%2FHTutix6Gd6pO%2FKnA4W97x5xNgWRH5fu2zHh%2FQ7%2BPhH9hpVM7kRcfMV2PRgP9ulKPTYK4%2BYKOtjkv8aK8LcA%2FulxgyRylGmu5QVo0lzVB7bnoHzWjD%2FVRGfyhJJU%2BJBqYdRt%2BIIALRUtRvNtB4Pr%2FKSR%2F14xdOpkhnGXv%2FehxBOj0eZ3DgQ%2BbBAPemjJ7d5l1Mqe%2BdpmyLo4c6ibkb0UxrBPf%2FQmtFCFOCbaDdHvioTAVwSWg99wBhUKVE1JUbukMuOrYk05qhk5xQvBe%2Bep%2FzEKADMR7uQmGnq8GM3qZs0WxdDaXOIbrVfQkeO6aNuQzqmsPL68g7Km%2BRpN5mlrF7ROyGqsAxWiRb9xgmSwoFcyLYd7ChdApgXBpQQW63VC3NxUVhFyvlc3RhFTa1DrhdA9wcDX86szK5%2FN5A3EEhmi5v2SPnm7fq5%2F2GSsLmlzl5w%2BroRSUvG4sezJG6jQYM19gQ%2Btb1H7hSFMdCZKTswwJCI1QY6pgF9Ygh0pcC6JMA0DDb6UdQ%2BbTAVmOPJSpcDyu161BZcsbGGKuMQdC5af385N8Fjc14VgJHzXlz64Uk%2BeybANASs8aq9OGOaZTWOwhhDr3o1VaZwGKMos32m25shn6gykCQTLPvZNZFKkXrwkghV1%2FgSulNaC5I065Bn2OHX%2FsduuCnQbdI0CmQ0bAxxw5FCvozADRw7rnI2FnHHjWfLlggbDqy9ly8x&X-Amz-Signature=008750be1d8ce73e417df935e869a5a8a5c3d9d584a533411b987109e614f9ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
