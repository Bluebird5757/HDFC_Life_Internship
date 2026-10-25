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
fetched_at: '2026-10-25T02:15:59.325Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QPQQTSJE%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021555Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEOfJnxllE8y4mWNozoZlC4lmjBrm3pSOM%2BFSNDJI%2FavAiBy1pTYR6FJfqNa0Y%2FHuhHlcY8MdtIXwFuEC1E6Ny7wTCqIBAiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM51pWQog9a65alBwvKtwDP7y4cQC%2FoYO4jw6njEdXzk1m2TIgEVm0iF1a5bj%2Bylyiq3ET59PRFp%2F6099clq1ddgZF0Sc%2BYMRULN3Y7dWsTXICmoIOpJTAnc58OP8DTVUfLbx4WJh%2FQObsob690O%2BrPiNTVSR34GupoMKYFJfrimqXVvzaMmw8VPolAriFD%2Bv%2FwzBtdqKhIxBhV%2FNvpdU4YKqleVxeQUS79b%2B93kUNf1oUSPsc86mUinkNMwDsN3SmmciaNFxU9KczOrV0cIM4g1wHdwTnXRhHzabKAfWBwTRmVJh%2FMEwgFGmLTMc22Hfkq3275KHvCha%2BNIcDhpeAf6uOhNAselBDrDStH5KYKgT8%2Bms%2FpWEXffDNt%2BP0Ck3E3u8%2FnR2At5Z4ynJbVYDMSPQP%2BbB9wHpu%2BcnmPxcMWS7SzFor1LLB3FjjTfxfvFznJd6clYb74K7INBJcbtAYAb6iCsyfRcG3JeGaq%2FSWPMheDM3z0ZyY5lDPEMa9XNJvmK3K5AhaeJtL4XcVtXFi7uSHeR69tRNQ%2BoiLunXSnWbNqm%2Bi275fYjR2NkW8THksSogbxhD8Axs%2Bih0G7aLLbcqHzRqaEF%2BILQ3xVLiIoiIdQSISVCJSzBLhwtPZCHWDxn3yZHe2OzOhoKkw2tX01gY6pgF8D3uEddEnhkxDtXndRunriAvS2b%2BpjxVtmTITkIx2xtgN%2BFDUiwqrRrcqqWe6sN5hFQgPyh%2B1XO%2BxaubYQ15ZRNAAgivxsHdBIlNrWagduTM0s7Gcy8IGsxEDIzmRbqipgLq2w9b7MUz0ogx1uiW%2FROd3EfhOrPcmVuHg%2F59aMSM%2FySCusaOuTxzaA%2BTcg2N9AhpW6sp0jbME9uz%2BtvcNiPNGeuD2&X-Amz-Signature=615bab6b74cef241e1354c8911ffaf7a3c0b790fbf0caf182b1d7bda0b193209&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
