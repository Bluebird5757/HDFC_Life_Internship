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
fetched_at: '2026-11-03T04:32:57.224Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666WDFXZGD%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043251Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDV5Ny7OhUcmr6e53o0sl%2ByjJly%2BO1O1OE45UYE2A8%2BdAiEAwWqJiLFgfWcos7FOc8e2Feyh9QMccWKkxkQ%2FSG8B94wqiAQIjP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNyPpp5V3zQkopXnmCrcA45wGVwb1%2BWIXBPHJbT3F79AnHxPp0qBhL7bnC%2B%2BObK0Fh5vlBP%2FovS6QRQQAwGdRHQDDqUxOAwMsuaYP0wQ%2Fec4LewHLY5WrCcPRwPsFVNkJ2rrqKnX1vS23EPC0J4aJ4bqxGWh2W%2BpVMWN63WMW%2FrK64EFcm0L6KGuxQbSlJYUMAXtkpQtioWhU9CS8pPP2IMdZ6FhU1hDrb5SXqGKedHkZNMqO55Ekdp3FjMcbS6%2FzTtvnC3ZQfMCq1MV1DwfmAilLHrSsSfUbMXYtxY8Pvi30EX5px5dIkmelniURrhbA5TyR6U6gjYEUCol9fDwEaT5%2BQrwcMrFzOWa4t54ztyk%2B8V%2FE2NytFw9smMfKtC%2BYv9tLi9JsGGNb3ljCzMZ0mXYXX5hBeXykUoX4%2FAyB6mYql4lP7048tEUh6uFvPfOqVp5wjH3gubQ2OLrniRHiPA0kaSAV5V9mWNFU%2BI9PHTewx8a0gSOI7UygZn8b%2F4e0l%2B0PuMf5T79iDgdeK4riMM70hyYIe7QEuDnP6zjzg4Lc17kD21QXf3QPWvCsGjaHHIaaD8hc9tw6uJp4zYwFLER0yRSWd9YxCxN42YOP%2F6dBEde1ZRrI67RnN7cGloUvEIreQ8KlQ81s1yjMK2XpdcGOqUBH1pBC2g7jccx2DEz%2FkqyQiuCt7LdEWC2iBvn%2FF%2BUrbKkRxeMQWeKQnBT8FB2NUihyjHAUQ%2Bojy3ntzzuWD%2BcSSwA6r5Qh%2FlbUu5zYhpgOn8N4ELjgfxpqiRC9TofqgS57ExqJXpWSuWV%2BpbrAXgTr1F4pENL1Cls9NEw62ZxczRnL6T1DUNMwiHsAlLAA8zhawitYEmPsvh2sBBVA7Ht3IjR%2Fk4b&X-Amz-Signature=65b202d893b4f3a7cdfa7aea74f524c5fb399359c885fa5c66cb308b55912604&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
