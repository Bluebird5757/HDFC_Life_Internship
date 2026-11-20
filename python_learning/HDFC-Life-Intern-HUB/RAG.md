---
notion_id: 3c84fa76-9938-8047-b507-c8020ca82aed
notion_url: https://app.notion.com/p/RAG-3c84fa7699388047b507c8020ca82aed
title: RAG
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-09-13T11:34:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-11-20T04:51:00.580Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

Its a technique that combines information retrieval with language generation where a model retrieves relevant documents from a knowledge base and then uses them as context to generate accurate and grounded responses.


Benefits of using RAG


    Use of up-to-date information


    Better privacy


    No limit of document size


So the components are:- 

- Document Loaders

    Main Concept:- used to load data to standard format called Document Objects which can then be used for chunking, embedding, retrieval, and generation. The main thing in them are the page_content and the metadata.


    there are many in LangChain but the main four are:-

    - TextLoader
        - .txt files read and convert into LangChain Document objects
    - PyPDFLoader
        - Loads data from pdf and for each page create a document object each with its page_content and metadata
    - WebBasedLoader

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X56NKNUH%2F20261120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261120T045056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCkhvt%2BCh2LBSwDsYk1NQGA5eG%2BPEGSJSuyZ5lsybELBwIhALEznB9upRiUmLZhw13C%2FnB5J9pj5s43GqTOn6tzZ3azKv8DCCMQABoMNjM3NDIzMTgzODA1IgwuZ0DELmnHoT1T5NMq3AMMT2XzX5CqG%2FOHyphyKmBVkYW1DRRx2NqQVBPwGBoACsEd5gVEnLNbHL9QT4zKar2cWstVccojw71jK79MopACRSLmFHgRjRs%2F048VpvaF9lkZ8bltvUe3bMwIWnmql2W9HoFhaas6OW2fXaNVSqJkAgtk1e0DnE82BbGxT4aBlSZPjXaZF4CtGOyna6FMH3S5LkaROSp0D1E5AjOX6quSq9TH5F3pYavVaiYnfjBWkOddslixAT3EXq9cmnMl7nzEdg9NniOMbqGEz0EBha6rCIFRnrZrzdD1ZioltI85ZPVtcN796xb0V4%2BTQCp5uN2HUu7vSXLYXzdlCbpiqFaGjOYHqqIt%2FP3Jnr97YzIrTYV7PTAoE%2B0QZ3kkhVWJ7qrl9MFnCJSP5xGtQZeg1lu%2FKmqLy6q%2Bw9wY1ps58i6AhbjR2z4UVi84xO12YGE7wH3FfFibRbLANtMR0us2R8GmFJ7a%2Bmt1VxaB59SwCP4oRWtSZzm7ZWfgLGgaViuqOBInJgcFdgfiwLpXCLeWvVYuxBiGJoTrb58WP6uxT3UWiNbRjBQjI56aN%2BeeDVrmcLrB5MQA%2FpnWPfKKZT82XBTAITtXWLYNOJvDqR3NZDCvRG6hlKGXM1HqcQzibjCg4%2F7XBjqkAaQC7jAmNv7U03d20yBjCurlenp47korAlT76F2RObA2JtRrK0ZCnqMqbGT0eQTdR4%2BvPkw6n4IVVFU1hpssWrQA%2FMC625LSyA1YWRIBzcU%2BY%2FxnMIZgDO0YH9c2oV7x8ljgO218KwH2bN08ESiQHnMPsUoe09PQDKzEKd8Yz9EPstd85kxtjToGCV82AmXo3teYue48%2FIwI5mfK3Bw37NHSW0Fa&X-Amz-Signature=978e117cb8eb286ee07d990bd51c95c1bfd7e31c112ee2cef09c6eb3879061a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RGWIZ2QA%2F20261120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261120T045056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQDEavc8t%2BOcRNmSO73gOmw8ZCB2IRvOLXSEe5wFmvYMOwIhAPgbFeNlyUzicca4lpKen%2Fm4V1bYuxvN9EanY9mjJ4yVKv8DCCMQABoMNjM3NDIzMTgzODA1IgxGuLnp0Qx0%2FmsjI4cq3AP%2BmZ6zGsTP%2BcTKeqq2dSFHSVHpv7%2BOE43jPk2KBmlXA5AfH0QD73IUxrXE%2FnaNcmnHmth7gF%2F8BS42DrRzNPDHtHYDaAgPQTjbEGNfJ%2F4p4jAdsIHn6ZqZryvPamEbYwu3uXCSFn%2BzGLArwArv4%2FKtR7PVtDrcOCeGa8WdTn9RbMzpdZlf%2FchlKPVYL8UPkCtt0EfcWrVhhLJOkISXssft6%2Bq1tiu%2FuWInLq7DN3OXvIjKerv7sesfycbhIgJSI442sw3sw6QHKlRq%2BfVWKskhPoo3%2BOnOYscCEBe0X8MR5f5C9Vcf34d%2FoPvo96gcM23FVc4e7%2FHKTQwDD6LOpfgNzLKEi3T3vlcHwnYtRoaHCYafb08FzkruZSFEN4xcncfNpr7ZG0OLBxt5merYlG1cZdRX381g4hfCiccN3MqJWSL1joff9IBfYokWytJcsf1CNZNG7QHEdvyiLJuJuCjWoR3JQJJyGHkr8j0KVUVUNaPIlT%2BC6rFtwWys3yGPZElyvc%2BEjRo4ATpBMD1XH7D0CZts3WzCKTOQGHLTrKakG4u89jvBKF5Pi0mIExP5q7THRwVH4rDzK0rJ%2FUwBflcVz3VrZvHNGFoZgO6Qugf8egfpV2fM2I0i7EXiEjCt4f7XBjqkAaEnRwvjMD5Hbx1Q5CJvP%2BM9eixgywVj%2F0IYPTdAGB1dpbF2mnNlegMCTQj4GwzEOSHmcjBM2HcnDNFR7waVZ9ZNDP92lvsiv%2FdBPj3rmIIImWMLJD7Y%2BIxYXBeTlXTbI5%2BWX16MGTAJnlwl%2Fo%2B8L0qjNP9gbZhiAPif0kd36DPx7ppTwGzv%2BIUy4EBnmml6ZyWhgYJjyhw5XhYA%2BtZiBHvxdEKh&X-Amz-Signature=ecaadd55f5c8b828d17854302bbb9b3fc205aeb31a0d897d6af2320cb95f5deb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

- Text Splitters
- Vector Databases
    - Challenge 1 :- generating embeddings
    - Challenge 2:- Storing these embeddings as they cant be stored in relational databses
    - Challenge 3:- Semantic Search

        So we need Vector Stores in which they are key features such as storage, Similarity Search, Indexing(Such as clustering which gives centroid for the cluster and we can compare to that which will give us the rough idea to discard other clusters), CRUD Operations


        Use Cases:-

            - Semantic Search
            - RAG
            - Recommender Sytems
            - Image/Multimedia search

        ### Vector Store vs Database


        Store:- A system where we can store and retrieve EG:- FAISS(Facebook Library)


        Database:- To the store if we want to add features such as Distributed Architecture, ACID Transactions, Concurrency etc (eg:- Milvus, Qdrant, Weaviate, Pinecone)


        Every Database is a store but every store is not database


        ### Vector Stores in LangChain

        - Supported Stores:- Integration can be done with many stores such as FAISS, Pinecone, Chroma
        - Common Interface:- A uniform Vector Store API lets you swap out backend

        ### Chroma Vector Store


        It is lightweight, open source, for local dev and small to medium scale production needs


        Chroma Hierarchy


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ASP2PI2%2F20261120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261120T045057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQDLVxQEoBMlc9YHje%2BUyzuv7FbGda0EPkPOVqodjoHMRAIhAL4A2TxBeLLmqufc5mDCfpjgtn4%2BnUEIUpmGtvb%2FoOoUKv8DCCMQABoMNjM3NDIzMTgzODA1Igz%2FOrN%2FY70reX9L5wsq3AN5Vwee6okzeE9z8fpP9CtC3tfRvnBvArGbP6Fzf%2F6sVv02Gvg6pSznMX1lH9JU6u99%2BdvNh9O1bZgoABb7ATTQPHCPXki6CG1P8DWIn%2F6%2Fg%2F6Q7jwNtirziou1hd92i0P2v2vzMKMSqAvVfgmEPG%2BghW8o8b7cEAJqRmp4SzK7z3kcnX6noRIuiiynfr%2Btb3BA373oz2y8nIYxM3GfDaYatlJIEhjSNvLLAJKvsi844SHJivIDdvGsnxgtlGnytTNgTglthc5sa%2BNCkeW9xYQyQ9k3UyGIzAzz0V92awtG73LHr2hhEGPAEgi136wU21MhxU%2F4Ubb%2BcREE4V95%2BA%2FJV%2Fr3jp0nI1mXg7dSuPVSDvu49DhrkHIDL6uYSzUFSYkavBU84ifUdbrtS%2FqDmfD4BE0KJTf8QuMtjRTyZXPOtG0Y7bSkuqWNPW6c9%2FqVuinTE8sznMHov7Okm1JCfx2b9TA0Ny59d8c46wF%2BIBxMP6IwASDDjzvJyidrrAeFwSeAxqP1FihgAZiAadLDc01axCfW%2F%2Fklo%2BHI%2FG8XhYmNMOWgGV4crlqlMuf41h73EPC%2BthEesUyRECgFAI6wqMy08VopZ8esshd1ZWovXePT5Hr4eBbk3jj5o2VfCjDQ4v7XBjqkAZT16efYn%2FbWAKK9qlVhtxaLj22VA7Pgx%2BGlw%2BcHVjVAEVmX5pPrQaKKQj1AtBDZ9HvPEs95%2BnJPJSS%2Bd7JKTb8IGmIKQW6tqchLLsb4wcl8bmtuPVP%2FEY8MBQk8cvkxa5BR8rZ2xkTkjqFQYi0eFM6fWATapcDLMwyL32owGtD85TXx6mfob1B2ta4DeNSvziqITap%2B9SldCsiNLxP5uUyEHirl&X-Amz-Signature=767e1e8cbafee6c83784d978cba4682d7cfd645259d736e236bbc798d919724b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


        link to the code:- [https://colab.research.google.com/drive/1rnMQ0VOcyak2KpNYcmrb30s8wf-ysMGJ?usp=sharing](https://colab.research.google.com/drive/1rnMQ0VOcyak2KpNYcmrb30s8wf-ysMGJ?usp=sharing)

- Retrievers :- a component langchain that fetches relevant documents from a data source in response to a user’s query. There are multiple types of retrievers. All retrievers in LangChain are runnables

    Based on these two things you can create different type of retrievers:-

    - Based on Data Source
        - Wiki retriever :- from wiki
        - Vector Store:- from vector store for embeddings
        - Archive Ret(website research paper ret)
    - Search Strategy
        - MMR:- Reduce Redundancy in the retrieved results while maintaining high relevance to the query
        - Multi Query:- For some ambiguous query given by the user the retriever generates its own sub queries and then according to those sub queries retrieves the data and then combines the output
        - Contextual:- Compresses the results after retrieval keeping only the relevant content based on the user’s query

        link for the code:- [https://www.youtube.com/redirect?event=video_description&redir_token=QUM4Zm9rUlVNVmVJQTRDVzczTmJkeUtoSkp1eXxBR3JiS2FtVkRJSUJJLTIzcUpaVmg1ZlpWcGxtSVRuTXMwZHpZX19xZzduMXRodGN2VXp4MmRwODhPd2JlYUc0WG1iekFtOERCRDRQVk9FOERUTmVsYnZLdThiWHl5ZGVMYWgw&q=https%3A%2F%2Fcolab.research.google.com%2Fdrive%2F1vuuIYmJeiRgFHsH-ibH_NUFjtdc5D9P6%3Fusp%3Dsharing&v=pJdMxwXBsk0](https://www.youtube.com/redirect?event=video_description&redir_token=QUM4Zm9rUlVNVmVJQTRDVzczTmJkeUtoSkp1eXxBR3JiS2FtVkRJSUJJLTIzcUpaVmg1ZlpWcGxtSVRuTXMwZHpZX19xZzduMXRodGN2VXp4MmRwODhPd2JlYUc0WG1iekFtOERCRDRQVk9FOERUTmVsYnZLdThiWHl5ZGVMYWgw&q=https%3A%2F%2Fcolab.research.google.com%2Fdrive%2F1vuuIYmJeiRgFHsH-ibH_NUFjtdc5D9P6%3Fusp%3Dsharing&v=pJdMxwXBsk0)


## WHY RAG:-


there are some problems with open source models or models on hugging face they are not trained on our dataset, private you may say and not on recent data and sometimes they hallucinate, you can solve this problem somewhat with the help of fine tuning where you train a pre trained on your specific dataset.


There are many types of fine tuning:- supervised, continued pretraining(unsupervised) and RLHF, LORA, QLORA where some are full parameter and some freeze the base weights and update only a small subset(LORA)


But:-

- LLM Training is expensive
- technical expertise
- again and again when updating the data we will have to perform fine tuning

To solve this we have **In-Context learning**;- which is the core capability of LLMs like gpt to learn task by seeing example in the prompt without updating its weights


There are four parts in RAG:-

- Indexing:- which is the concept of making an external knowledge base, which is basically the concept, (this is the vector store/database)
- Retrieval
- Augmentation which is the creation of the prompt based on retrieval and indexing
- Generation:- after the prompt reaches the LLM it gives a generated output

Youtube_chatbot = [https://colab.research.google.com/drive/1aaHDTp7KMqLCdnpnC7aPHwLiRIy0Xd06?usp=sharing](https://colab.research.google.com/drive/1aaHDTp7KMqLCdnpnC7aPHwLiRIy0Xd06?usp=sharing)


we maintain a index which is built when we first run the code and this index (containing the embeddings) is stored in and is used for the retrieval, this reduces the latency needed to load, split, embed again and again


but the indices will be created again in following cases:- 

- PDF content changes
- PDF file metadata(eg: -size) changes
- chunking params change → chunk size or overlap
- Embedding model name changes
