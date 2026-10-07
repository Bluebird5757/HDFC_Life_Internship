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
fetched_at: '2026-10-07T03:16:42.240Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TPHDFPFG%2F20261007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261007T031631Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJGMEQCIF10vo1zcCirm5n60zL8BxrApNniC8i2wQR8xdjMjLEiAiAv%2BQMXQCJrVi5m6YctImgPRFI6XCHPmK1Ov4Yv1mp5Oyr%2FAwgEEAAaDDYzNzQyMzE4MzgwNSIM08l4ikpBQKhtkNpdKtwDXMzUECIqJzwSX2pWtWvdZ9LsTArZ4Wq5HukHbPUSBzNGmAUNhywZFmkuis8xmD3RRCNZrTsgwTrR9oHKyugpSe4YUwYbgeyDT%2BxpZj8D97QT1ZdEXQANRjCol2jd4mR%2Bzy21LfByxO3nHVgzdIg95lUBqJ38pAfuc3p4Dpdrx5QnoKK%2F1CcndQ9Gl1QC9b%2BV3MlufXoXsoLlPKfRHdmXYlkmCFXJ1VkyocW5kShBtvT043DG2OBHbE%2BtcxqEsUddlpDUw28Z32oKNREny1I1lCrrJnfPV5DxIGBizNt7lsfa%2B0%2Flc%2B6hzam0iLkdmDszn2JOuUT7UlqVfA1A1kKTBDMsnBP4izQHGhPX9%2F3ZDaPAF5KTZid9E44pwtuguW%2BwbMQk6klQmHdDHIu21%2Fa2A38EawiTXk1OQLC36z%2FRXSDJNj%2BUoOznD1OuFX3RU0nptFXvGkhBoH%2B9twkEHe4IpSX09nIuKKDZBVh%2FHOH%2BAVzZ7s6CaZLojsNa6Ua%2BRvMeOY%2FKRSCy1awO5MhGPf3os6xaAFSNFcs22IpcqDFKnkgpMkm8OHUaQ1XwUompwMM631MvjFW4nQWdj4VETP0DA1r3p1EC76gyHa3BWXmXCdI%2BMXQAXpgZ8n7fAv0w3tqW1gY6pgGTGYnEPuYEYW6awKKwml0B9A9h0j%2F3NTZK7kKjQBZhJyol7h6UNocMW2gzGsDBbEUDuqfQCcV94Xfvcf%2Bl7WoGOMK6x%2FpZKk2UZJo0rUgAr6NRrMaeFadEdFYqL9ZaNgOHIBIQ%2BnWZZfigUWYNS5tQyeIAMJ0vDGCgK00ZUNXVNjHkUtyn2%2F%2FzcKc5%2B29T24u3PjRbfOb4%2FjJRzYqhb57Z0xeb1AyM&X-Amz-Signature=fcf023446ccc434e1740d2809b9eb19f8dad3569d22ce24cd24dd2b022eb612b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
