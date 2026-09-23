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
fetched_at: '2026-09-23T02:23:06.663Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664D37TV7D%2F20260923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260923T022300Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICtQsHZPJWldjuJF7CSishdFqif5ZBJwh%2F6IPQpbong%2BAiEA52fkpFGNkL0RsDwmhOTYpgs5G%2FEswPm3cZ9DndHALAAqiAQIs%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAwTfV%2B4DJ2HcaVPQyrcA1YaNG3jZ3ZwKByV0R95rfkQeYaWrDmNNk4VUz%2FA9J8uinVX9aPZPNwn5H7C%2FdQZeKXzHl%2FkRzDdogQBbRFbFHPIBFBj2nmJchx9yHcChBZRKL%2Fne0ceeTErPymo3c8ATWEFhwyUbjmb7pRIvLYFveHaMt8j8Huwe%2BQ4BWvGRDfX9oshCT%2FexiM7s0QOh%2FVulZEDKlI2UVWd8KkPaQij9XerZkPEatzYbjnvubcIOHWp0bRKZqExV7dgyvnyxN7cMRasldeVtqK5a1tf2c4QnOBwEiMiXIH9M81q%2BFMjVtNlWdPlhsVRf6IPZIPfo423Uv3yfKdhxLiSb06xHlQjfIy%2BS5%2B6NCgSlbvN7bFRVZIUj3LqpuvBy5ZgxVi182auzx48LlP22s3TUDDLiIY8jtVajTz%2B1kIqqeOmT%2Fl%2F0EYRN6CFoEC2eUfrqszGcqDTQ%2FNuX3%2BsGopA3HGQE6nXm6y%2FBnxfrJrq4e4n7seXXPRvfoCek17c4Zs5xmxduDOqcuiQKmegr1U0H6KY5bCUgTs4XL7jNV%2BXm9SVifm4txrb%2FrWDHB5VE9Z0Fv80H5wLGQ3WCVeoMivSn3oX7OCU%2FFDbIwSUvq7v7w1MGVLGPvKX5KB0pdA%2BPxEu2vgzMI7YzNUGOqUBzU0uCT5mE4I2hTS%2BuF6I5Uzot8JxeegpwXEP2qoZr754tS3YAgr%2FPzGGlTDklvvEmW42k6Cr696RMOcJiqlPHNprumPMmyQ2v%2FUjW3WBmdMPCWcb5imo6RTRqca13Z2vqV%2F8Se4gc7ITGsSSsFkxhCxoMGQsYkr610WFTLX2pBshhV8mOFpH3lfPDjGYKesB1PDtTtzgywAPfJwVaZmJ4Xfp4sRK&X-Amz-Signature=7558dfb405b716fe25de33420abe9bf8a86c4ebd0390dfc7349d935835e62295&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667TWOZUDC%2F20260923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260923T022300Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBWCBVDA7Vvzqbby0NKSVrHPhYlZAeUoycp3O3ygDtQSAiEA1a3spWl556QVzEx%2FFfBGVj0oj%2B2TXilk%2FHYx3lm31kwqiAQIs%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB9clTer0LcrRoCVlyrcA0DjfbZE9o8PeeZNXNQ7jCWrhgFxX%2BwcHwwyWcJ9SLuNFrGx3op2tBdMruVyccR8n5JQwVMdD0aDBOOUezlr5pBczn9PzyXo4YwqKbPXsUHzJ0FdkgMli5a9lOma%2BwAVmj2btrNk4cprAdwzZK%2FScQ77bfdEYStVmgURKjvXE7QDO8AOdlb%2FceWUQcXDri7Pj%2FaGkrquQ8S3W4tH4H1Rq29%2B441bVzskeZ2MEX8PT38i8Bu2miNLo%2FFyqpJC%2FrtN63F1tT1Q0KWKDffApm4P8oq6oLLv7gcz%2B3iPOrHnn606nRq1DPukhYdS9PABTVlSk0GtmyX2JtQjYoegxtqHpLcv%2B1sHyTCOyuA%2BJ6BfjfSxhwHF9ffnlWsrMIerErMcjCzdL4TCIY%2B6exjDTeeUggSrx6yMZCTRtpGt22nntBiBISQUVqHkMVheFn7u4D6ePU0u84yWsJd54zpkgYEtPvvdO6Kpyt51GqWSZB5aL%2F%2FkLFWHc%2BdDX2NRM3te1yAmbJ%2FfymdjfIryCwwZ6rTM%2FZtH0BPE%2FK33sNSyUm80dATnR5vBlyhyaQnBjoRLIiN%2FMq0LvWZ5xzazK1BGznt41OCdL6%2FD9XYLRS18ND5%2BnrlfQ78TponltclK1w4nMI7YzNUGOqUBJWpgQXWTqIk6ZoabWeA0GwXyfXrAZYur4%2F2IwaNIWLztP9cgiemGt1T95FSDWnAb%2Bu4QGRCXkLxMl%2B%2FCUmxvU8dwW16V%2FBiWiYA%2BmBrIhPZM1zAOfZARHkjR3bOHwJonY94vvrVadynZwFc2Jmnp6GtRuT14WRwki6%2BUngd8Alf8402edXxJgbqscl1Z3jGFP7mfQjIG9%2FsyeH%2F%2BkMj%2BWgHNx0bE&X-Amz-Signature=97be051f39692dbb5bc696aaa04d89561a960bb0f9878fd498a523dcf2b18469&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XYMXX5BK%2F20260923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260923T022301Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICUnyvZ2cHXtih873UStbMQ6i%2Ffeaz8D17QVmpMprFKtAiBQ6Z16Mt2FB3Sd%2BTCnee4gK0KuNtk%2BKMF3Rh9UctIAFSqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhBmGVq1yRFtdA06IKtwDnUsIjJEpuNq3RfzHKIchYveuTs2wXcUUg8QI6t4AVbPzlz05HIV22ENgq1hkXy%2FrkgoR1kn7kYyr0NlF1q5nfDeUoAEHYu6BGbgUsK0xcepWi5F89fQJ%2FQYPnbOtBmO7lIfPuU1b8yx7G5zOhCR2a%2FOtED4qw7erdrD83edgOQ63R5NtxDatWc3Q87IffOrmUDfXpbJxrJ4keo9wfdDENzkstfesU9MiMmakkeIHK6lPku21681wavknSF0JjpuX4eAgLgcde7YzDnZQ11UrXgV1sy%2FnlAY8QQ6TtYAJl77thWCmW9kpIRVc2cyaZ194mXRGgIBwpLZeFAu0amNv9oB93Mgytby9O978L5kopc%2BKgVbOR6hkNSxozyGzsYrm8WFcgf6lwR7wWMtMKa%2BMOe4WZQ%2FAygSHkJbiasvJi6Fwz9YxNn0XJN%2BSVVsUmh%2BDRNmOCKgvJI9hy6KEqtz3xWNtQHm1d4sn84zg%2B0s2kHdup6sAuz6X8Py%2BYf0A9vQ8sv7KUDNhgL0Jbtnbl%2FNO%2FUuaHU43vCfnEa%2BEydN81S7XwvRE8IuzXTWbXwD0bp2fGDYTMzZzW1x8hBRiUeNcki0Nd6BTEBGPALBMdzlvyTZJgpULI1%2BwFjbB1PUwwdfM1QY6pgEw1JNP19uZOV41v%2FDPLKisBoyIzEkqmYgOGeDJJSGbnQn190ML2i0jAaHG3Fm%2BUJyh0qrf8qf2vwSpDI6MkawoQyirkNoM4vXXmTaueT0dcS%2FmvsoZEiBWZqUDpLGQZrMCD%2FkQXB%2Bl23iGx%2FTuoLjwX7%2FPtVwRF8URsTfzWeazkZKtqoykQmVsBIDg6PmpYL7NvLmExt8ltqLk%2Fm4vzvvpDtFPn3JI&X-Amz-Signature=79b1af46aafb9107c81bdd45b4bb82e21ed927551ea7806e19aba885056fdcd8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
