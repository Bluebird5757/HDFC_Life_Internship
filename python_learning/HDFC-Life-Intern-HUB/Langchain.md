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
fetched_at: '2026-10-13T04:30:10.657Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SUF6FJOG%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043003Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBMgaMl6hK0YeWK3KE9G1ixugZPXpC08BB1iyZzovs%2BUAiB4SqQ0OFOcuv4HFKY0EQRAffqYOaQ6RHj%2Bj065wa6p%2FyqIBAiV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMakC8nvliTioRvrdvKtwDLf0DRV6b%2BbWHsT8NaWIqcQxdYv%2B33O%2FBICzjSmWiciW5gFDcBQhl9iRyfxB6NdX764H4X4jagmxIrIGO%2FogqJ2yuKSmSc1spEyO%2FSBjhlt22N2gTaw4PU0olXhb84iAOEgzEFrbfnqmxJDn%2Fd3%2FNCH%2B7PrNTJG06K7JLlROXoNQHvt4A6%2BlDIT7EMw4aXpOLX1wArCQzd331JteCMzL2AGXKIdk1Bz3JYeR39cJuY1OiYUDj%2Fv3%2FY9N7BqpJ5dOEQCzxRl%2FmcOaGkBPO0tYbwV%2BvypbYjXLee868PgXwjGpTnMOeo5wIMt2J8LP9tKhaLukQ25U1rolU1lQLFue71XuguYld9IZWFpL28KkKLGAPcru0zQttC4hh%2BPF8OrYiNH50M8u%2BJnNMhFH62vL9%2FaHkfqwxex9pD1kjvn9QPzfFPUQot8Pxgu3UqweETtR3k5aUN9XdbxQqlZ9b16Ls70TL9e6%2BQngq3U6CB9tHbmJYyEA%2FjsPNWyufCzH6THxNnX1nXhHyeXQbG3eetFtpSjeCdgkT8T0VNNTREQGZoDe2JCgcR41vBpRf%2FUl8qpzF87QylzpKudxnMKQimVvC%2FRsfEuDWjKFsKZEeOk7kMPZ%2BiBuxiJnEnDuufngw5eK21gY6pgGxa6rxIRfiU6d9OehJO33vhXnk5lelefa3iCTTcXAv%2FjpE39In6BB8ZJ%2BlVOsqoH8hMWtVLv6Ndo3Tx6C0FN%2FKPAEBwvI510Wkq9kMNcF%2BimZgXkGbDJ5nebTDAG5BBxxl%2FL1zzLuqaXlYR35S5JbpBZs6xqNOdKJqeAOcBxP6zkhlFUX3QVs1iabpsP7gs3KDd1T%2FeoRdPgP904J5EEOFZ3dWgb5P&X-Amz-Signature=09fe8f037801a45d52db7e5f5a86e432fba5024910a4af0c1474f4d82e465b5b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
