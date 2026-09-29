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
fetched_at: '2026-09-29T03:15:08.993Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RX2R4A6%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031459Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJIMEYCIQCBCgZ9O9WEQhCgYLkQOAI8H4dYWTrFagtzxE0wrWjwBQIhALcthuq8FPseofAlyEOa4ADp4ib7iVVNvI3pP8YYEBGtKv8DCEIQABoMNjM3NDIzMTgzODA1IgwYbpe%2BTnkGJsG5RvUq3ANa5k5jL3kIpaeGA%2Bdp9mX%2F7szF2H1wUIFmvBf1B77C4koV%2BXPrKH5RHahrVpJRIcxtK%2FY1a9kE3RiMrNH7hfk121dOdXp8lhUMgmW0Oo%2BT86oQtoF5Spyfu5DtaiBAgq42fMTKGku%2FVf9Le3iF%2FAX3MVEE8lUDcRSHyxIg5wqkEm7rWBe96dHBfhOkTBNY05N4F9awTK8utiY7d89suHamZRMB0uafwgrLZVrh%2BBP01qMFpTLlBJEzMUyLcGre%2Fglqy8j7sZrfNiGZVEQaUJfVln6WCf8mq2TuXPAYy0fOudXdcklmmPY2fa5ox11hphacO6I4sq6sBx2gPhyzlFPOsXYQGxXW1lOKKsi1zUKw5oIDzGrTHQaBwrK6J9tMMIdPzE3KAg7S06JhN%2FGB3KuvEYFrjImqOyDXm6LL2z27JbcVcw2Q5AQudO3xQv%2Bi7J6JPLDohUu6Xeef0ecINVlOhbU3rj4qt7j77fzdKUdAYVZKAIvecOEpFjFoZzio%2Br9kC6neQWuiCpjrbKJ1ru%2B4H0ljay4oEjw2OZyhGTg%2FV7aBOKpgSr6SnACuHWdk5NpGds5RNqioRtMfgxxY8b2kjgbddARQOW41tCVNEXZ251pEb9WXg9AgAAomTzCsm%2BzVBjqkAY3Nv33dd7mfC54IBgfIqm4ID4GPm%2FI6QgwPE5LgMALTVSy8F0lCsk4rrNdf1CLQwu%2FUbHZtnDBg%2FTVbgS1oBUROHSYz1qJ7F0jDVhs2E4sGCCarmo0ZwVkJhhy%2F35shfVzc91GtZtNG1%2FLH374vRZtVfdCfUfrKxXeeg1T46XQEVbF3orwG%2BVfrfAw6il6cE7K2NJzIMQ3VxlGChYUoWRmnzEVO&X-Amz-Signature=c97db4d25e33ccaf796bb0ab00d6ba94d88a0655cccc43fbf3a319659335dc9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
