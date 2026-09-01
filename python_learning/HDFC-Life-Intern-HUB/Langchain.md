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
fetched_at: '2026-09-01T02:35:26.647Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZWNNIQKB%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023517Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGEU6i1z3%2Bcl6LcRn2Mo2ydcIoMuWuKMjhLpMcD7EcbyAiEA5tj71TVNJ%2FZ3PVCqS9Y%2BwBGRFJ3L5Y5YGPoDTSTHwA4qiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKrYgyf5qbIUEJ%2FNTyrcA%2BrFGPVbsUQ6Y3s5boWqTpk52PShASxklx4y4V9X05YlJI3kGPe2xzaYcJ6p9Ut0Rp0x4wjj%2BGqPS3GHXW0csRtpJgqkorRxDT%2F1ZI0FpLtzzlcsxKsEAYnBp3fEVTzEmjAGRB4LJuqKuCUUjmhqIYAgxQNEixg71Fxhq86760VyQ02kF6PwvwdBZomauqAX16KDOhO7ztRsuDdFQonoMEF%2FDptoFvIiImlcUo9wRbynGKbhLJ1mtvMbKfakN9bf9EVjvJJJ26iC65d%2BOkJwe2LFJWzSmHfDMLPQma8YAjGPK2MPrS64R0PdwS1AWzf7uzYXOLyWHxxbqwevml4jYmenkzIacW%2FIl1irj%2FHI2qzNoCJGooVEsLX2tKHHx4p6r04Sj857qEeio1KqyZp%2Fe0bQPVrUbEe1mrqCGJaER%2FjS0U%2BfQiwQCEU3Fq2gRHcSLmZUAbD9hjCTlCG%2Fajz%2BYRmxLh7o9CkgTrJ3YDH6%2B%2BAMBCfyw7AJQSrpmKKZPxRu5L86xjca7hD9jvxLVXS6Uj2K37LG2Ge3BiFn5MoIH%2BQ9It6yZ4Hfcqk%2BtSLzR1Kj%2FgWaztS7YalJhdEz7A0gKiIntrfbQv0ewNI6a0FPdLxa1urdn0%2BgFEaSgx5FMLvb2NQGOqUBBnaqfzY8kAgT0EIb5de4JemERM74fzahoTwNVsFUL%2FGzoDqOubQ%2FYF3Im407WzU7qHroyF0wv2ZEdIlosg1nTq9wXAbB0HsozGUH%2FWgGbhLslDnlqodsaKEzT8Tg2%2FLafQbdJQx1m%2B1focrcr%2BVIJxky0O3fl0LD4C%2FfFbjHE6XLBNgiXCb4kgryeDPMWOVmFLpnaudE%2Bt1homDxBVQv3jo%2FnD6T&X-Amz-Signature=78d7827a6e99c73b05a9672092319b6ccd41c5b30d475053b2e8d83507e934d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
