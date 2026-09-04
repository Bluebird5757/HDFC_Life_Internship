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
fetched_at: '2026-09-04T01:57:44.789Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYTIRGXX%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015738Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJIMEYCIQDDRvBwB6BlITsZvixxwDSlz6cZlBoWinx1nB52iIv6%2FQIhAIVZNvoXGmZ6Z9rEz%2FhLY%2FWhShS6DY8qT1PX0ZTs2BpXKogECOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxZb%2F%2Fh%2Bmo4Vkwot58q3APNOsYkJPGTbwfrR1SHXEuZpyuvDobdeCmDfiDpY5FktgqDTDjMp37ylgFKw7xBosFclWudHsQn62Zrr2ZJl4LPFxj5Gt9Hij1Lmdt7Bb7jQ3vKKBAB3r4HIKyu5EXFYNkPkUg5TqHRyrv47UQXRsgcDUIe6YFtxdpbwJ1SO6%2FiR3Grp0C6r3kfdkdjklXxsI%2FBx5skrUdYcLzvBB6HsY0vhwDf85yZrsLJOLJDzf4dnZaqx2dqEyuU0w2q%2BKcOFFx0p2tE7UyoF%2FjsMMhvRlKLQt0JStg3RYTRcCtlNMsz%2BfIfd2qc4GMFyy1INPP9ii1wPZkIbOHVYg3EIMYFJFIASHXP2A%2FMr0NvfOPfO%2FZUb1GpQeZbX5lPHWhgVCw5GemuPhVM4aToSnCktqoWG64rYpHfaYt9uQt2thL4ug2qlTQOvBR3d%2FDVGx0RYQDxp0BSC4hydcX1ABE4gRhtHfw69fSxlh9aDOmMlvT1%2Fep%2FduONcJ53nBYqU5LWXzFZioK1MLTlsIEGMasM7003%2BjcPyIXYqW2pIHBQq18BR%2FGm6TtcjzUllQzQAypSXcAA2zYgBBPWCx1ER0%2BGJrIFlWNcSC1uvV51npF%2Fjd0zh4shGNdnBVwPWulE6h1o3DDMjOjUBjqkAee8JFlOIHRoSmWts2Xnr9UJoo0LOtyjZEAJ3u57x0Zp8uWzBDOIpEfTQ0HGbNHFk9MynU4Jhvev1ZlqkqGDZKudIjUYf9xj3k8jDouI%2F74NxH82pF7ag3IosoO1CShNm4%2Fec6awiLT2JQFliSIecEaZxKKaRemRtaW4GxtuOkvEty%2Bncl9T66%2BKVv%2BgaDEem8guHz%2BzSD8tlMi02EdELsE0eNNS&X-Amz-Signature=6479e0e9addf1b403f5ed9f7d6a2261b2d0492905a6181ef7fbb2c3f29be0931&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
