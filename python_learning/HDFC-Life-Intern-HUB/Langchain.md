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
fetched_at: '2026-11-14T04:00:31.943Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662YA2RMQC%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040025Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCBshl3bqJoT1JO3FNLrd2bv%2BOX0NDV5n%2FWOGf2sA8vNAIhANUEJkIG2Bj%2Bf3Y1eO%2BhtFIxtt3GY6%2FYteCjdWPV41xoKogECJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwwvlOaCXPflIBj66oq3AMsiJjSTQarrtivmSUI8PBp%2BvIsxbvPa%2FdabGvBewqxCAw85ViJ9ZW1lGqO0h%2FlHku6ONxFy5lo2aXuKtPzOfhtUG6DPmUJ%2FsLh5ebe9ynssqpDzvWEXsqFWjKqAuEJv2yGoOcrURr7KOH5q1%2FXR6a8JBKi9qi13NjSoSgYOJvuzczgPof2s8PD84ow8VfEN84xS0gBAxuBXhnZ16K7DjdKzvblCEQFXVM1siOTxGqzTUQjHsOrSLm3qQsECq2NKlqFa9UTg6IZfO4dDSDprtelHivf4qhAIgxRpvIlHwQ0H0TWWKbli25SWFFm%2Fg70MAvQ7vr%2FyWoiIhX3MYuWi1wPKIrpsNqdXDTwff5RybEgYhK8UvOvspgbbke2QYUWmTJfxJ2kGTBLr1HU0ZPc5oQeiQG5wEAKOLxVGHW3pMVzpZyHtbTJdVAqnC%2BBcdENWgYbvG6WPccrmFNv%2BV7z8Bb50lnZMX6vKh6tyqe6iHLdEbaxGiob3oGZbFv5t2JNB2gAVvM%2F3Ho%2FwBJ%2FTPVVMYXj8RvPo42W0sShaTMgI0Uvwbq6tkznHwLazGkRUxZv36BYFo1iMknPU7Qfc360pIhYYbVGAcj7H%2Fr%2BhIacqA4uYlUCFoRXyz5CYp%2FZzTDK3N7XBjqkARdgWYArGYkiD207qiR9ClRMZnUqRUTAGx%2Fcj37C%2FpUfnu6gkBw6rahxh7vCAwY7KFrYdBkPLY%2FNgP0iZ2F5KXz4eCgnaWJgVxuH4G%2B2S0QhMo7VITxjmBd5cDx5adrH7C8dIQGk3Qvi51Vi7GIPWjb8uY1%2BDREt04JYag%2BSrgoLb2CSFUmG75smHOaKZroyKoaY34JO1iVCI4pobVUVnLJ99Ids&X-Amz-Signature=e7a1de7a42a0a2227ba0ceb4442e40a6de8c381a642efef42fea2cecd94c7935&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
