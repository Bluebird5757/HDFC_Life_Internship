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
fetched_at: '2026-10-29T03:27:06.826Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ZX5XAZS%2F20261029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261029T032702Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJHMEUCIDMAWZ8SlEQ7zPijfCImQTrTkgjkVC7brMhWfJk3EY%2F0AiEA2FGf0gM%2FuqGLegjVWUEhyzCi%2FFuHPjmWPwSdY9TPZt8q%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDOA%2FQxHANgsaF2fjsCrcA2xzM3us0DvmpPcuTZwvctUvXOsavLD%2FJ1uI1bajjE2RGPXy4TGd1XmqVCredPXL8jCGMlWARuKTP3J0tOiQqjw5cPUx7HK5NiXk%2BXn0XTlo7cMpO%2Fd00478%2Bb90xp%2Bn%2F01Bma0xxC0VBvM3T5Zss2wl80XjBLMBGyR0ZTXtwsU02oCBlLiprzWbGOs%2B%2Bn0Knq77lxjZmnyKqQ%2BUovXj%2BSfDRy955vlWxRb4TIEky4FfykZjVY0V%2BcWKrMVgNsi3OlJ%2FuqPgEmWuRXwDFH%2BnRFNObP3W8L3WKGAXpwlMjJS%2Fy5LVku8WMsXz5eczZXx8%2ByHStvTgYDLJPHv3o912M0FcfoUu1%2Bq4g1nMpGkEWToipdxDIEA%2Fg7yuLNUVw%2FuhmQL%2FwLDB4qRwg0sifty9aYLjL9eoX4msvd1K0FYkQAMyU9trcF0ceUnXHFZAFBxObqeKW%2BT2PBmMO77cfc1qJS2sYtgfSlt0dYnYQd2TzfaAhhL9Tu5XxosHDNXDKDeUb3TQTNejHvhufObVyiHT9mUuByeAptjpX7XQoMmrzPTLXT3s4C7yLyxEYOFhLqmPorD9Tdvje3V0UHQyhLqNoxwUrClVri5Eq2vSCzraq3gj8Od8P%2FXywWfotIRuMLfqitcGOqUBz7SMxOGa3LErhRThVnBwS3ooD4qIFkbxTYVLmJ5LYlJPVlhhNmZtxsKCibKfLZRIcpnhmUk41ULH4CkQZnSEps%2Fzdi96nfarFtbQofvCX8dHNVioaYbKU6LLb07O%2FScoyg1adouyPcP8W2xMfEi05JIebVDFPFq9bLYdNvgjmULzFTXlfRAvvUnZT6b87S6MJYLybtAnS6EPlu47BvjOd9lp9%2Bt%2F&X-Amz-Signature=fc261923936602f5372799b0fae1df9a0dcda3393bafdf9cc7c24bbc086d6c60&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
