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
fetched_at: '2026-11-18T05:05:49.317Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PSJTSRM%2F20261118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261118T050541Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJGMEQCIFTrwBb41J8OqGYeHjD7bFvFWLmNjbrBpVM0ti1M6aRPAiANm54lqLlcnaOpHQ9ajl7XyCd3czQC%2FF4OdDqwMOYk4iqIBAj2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMceEcUfnX8BOvDLXrKtwD4D85LIubdDzngHWhPg2c6VeEbWjEgtHpetY2cgpe%2FXw0TRXNPjQ3adyoKk1QWruoFHySRyMKSV8vmkpESADoUY%2BKy7Yg88vxHziuPIFe7%2BzuU50JREIGFPW%2BFZcadt3HeiaE8K4cIw3eML5AD%2F3In9gwNHI8wkL4l06v%2Bqq3I3x6kyn6eNb4RbyioQTuDWyd%2FfQUkrEVdo74gG7Klkr9YMYM%2Fm51IZVhLxAlIzT%2BCuFHhvM1TDiQL9ii3E9hbX%2B19A%2BAA4rojGyJCwGfPFZckm6n9B%2F16b24cMdA%2FLqR5Zg9lRsDJHiaUYzHcdWKUiH4QMxbAVJ9BbEcQWvYLRGojFFv3ZwXmwneqjF3BSqSxOHklbxiwrVwz1qeu7Z5Wu%2BqGY19hGlhZRkQ92F6t42Eolfa8WoZM3h%2BbrUzwgeN5AfuIeSD7G9NPkQGvvyoq7bFaRtam%2FuQyLCTzlgqpe55J9hEnTTJKXJbKwtvnRrhhmfk1c82FT5uB6BK0fUBcB4Sc1ORzrLFYh1mm%2BdKEoj%2BRgDUDtkQa1kBH1xYTeyZgEnetfhFDM%2FsUcnQB3HlL2EY6MqOm8fq46dYp%2FNTbkIYEuLL2LeHGgA8PO7U35%2B2zrw0t5fAnQV6IPeUT8Mw1d%2F01wY6pgFStx9ZQzOOLP3WyEB8tmdJWaerrPfExtAGQmmbMkbycWRJmsy1JY96O9mF3vCIoL%2FhGOJWKJb2K%2BCSpYAXBRFUvYfAlRm%2BGYQ%2FLnPBI3Ax2uGBMqrXT1Y33K3Hgat9QJY9SG43JAtruLmzZnBS4kRgq02ALyQE1dur98qvS7OsAe%2FWtzojYoXOeRj0cbeUxvHrovTtMQjlsTV370Zcj7Qj5fhzKuZ3&X-Amz-Signature=f2b235a1f4a2c1fd6486fdc341a57b39560c571333b8d0f5478ae9335876dff4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
