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
fetched_at: '2026-11-27T05:49:31.517Z'
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

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/76214e1a-ac00-421d-8344-60afac4e32ba/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666LHEWYV%2F20261127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261127T054924Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIQDnWt6eZ%2Bla9WbW06tJ987sNHAmRksXkEev8bWL8w26%2FwIgZ4zpD9dRLEOX%2FptEruR4Eqeo0Dlfi6CkBUtukOCoz%2FoqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHnrP24Xva1COKPCnyrcA9OVUZ5TKWAGjzvs0FbFskndJP%2BybYa1JmYMYYGFK%2B%2FHQVY5JvOSvNlEGMqKGOcj1kLWJ%2FyEwuAFfvSY3Fu0W2YSSH6nePp%2Bq6al%2FwjcoyFhVAhXm5mlY%2FPLMfGeKOCvvBoR%2ByKMMxdBYMaHamaxKJg0uafifIYcjN4NytDOAohGKu0txRI5tRszRiu1gOHDhuIAfS5z5TWz9vqbC2Zghx2mHb5r3oCyvDTAnQanbviOAqJljM5hEEtSg97qB9y%2FYebQJBsuiDlg5cF%2BK6pAC4E%2B9x6klhzE%2FCM8jH25Ztrlz96cJHoYDPAN9XrWHiY2aEcpdCO7NXsD4rr7aCgTOz9FOHgIZhoBsC4WVj4r0Lap7RQ4ir3ihlyh6eD89%2F15sA0PpIAlKNeVqUgzU3FqqVtAZYL2YZYIdXvUftHBBH8RMyl54tsm0w2liN%2FbuK7JVk7jnkWO%2BmyrNoIhp3onuN4GG3HJzuSDJenXX7zlqebWMFFcK%2FTnbVVi6n3X7wg5qcNoM6bZD0F33Q6NckcWgk6WVtfU1LUd2Dwbpj2fejMw7ThZZ45Olx%2B5CCH%2B%2F94H8T4v0uM6TGZrI1FjRjaZiU8vsyVfyZCfYRLIFR2u6NGjfSGLoLEKvTHNEeryMNe5o9gGOqUB7d4k5g9XMce1RWrZQRgAP5DPBEW8Kk3eeq6IYInEsTH6RZlDMxmeGF3koFMFs3Yukj8N76KeMwn8mnzsMFd%2BiHPjyweMKOAXaauZeGb8I1CHLRCXs32OFRjdwIbt8T5TQv4jiaWeoDNQxAWzpuIHRvVJky1MF5BIrfCThqWnNkLYXm4CyOly0wWEhwZWW2R%2FmxTkIyztZMpQsmHF1caYuFnR5ARo&X-Amz-Signature=eaa59bb49fbeb5a3fdfae8144770449db5cf2c32b745af9d35371ff1359742c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
