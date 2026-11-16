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
fetched_at: '2026-11-16T03:47:12.349Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCRGDDAJ%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034709Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDqeJlGNwJXvqAjk%2FsFLzWKqm7egKoO5L%2Fp4qXTbNL6PQIgOObAXQdMDLef0wyTRvyMNZLnI1yPBgsEHliD0%2B7FTj4qiAQIw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDO3fzarxj7Ev4Td3qCrcA62NyGiWaHbUKcin8z1mcV4AuN6VhCYGK1QRdtXhUOQB34KDzoOLRyTe2fNAqNPxHldLsqAQrg4sic4IiGjKI4camnOrhro6CVFZ5s%2BV6ICGhUNj0G%2B21fJ88iEdl28B5jGr1Gf78gAKKw2xaCFFcaxTEQfE%2BOkMvLe29oyn7N%2FvmqjkzYTsX7gIDxRAcwcqSXzQEZFUdmEQJvmpz9eVIpIOqUKaSL1a92PnFMJeCj3dLS1EKdSEMbkdBg%2BHAoH9Efp3XnYSnriIdF5iKe4Si2QKLkUjeEh5pbl%2BuXbP6KqTh8w23rWN0eQJ%2BSPMp7OAVH6Ux4XI10S6eWDRpA%2BsrEWfHr4ZUbq20oBvqTtZmhXgqYO3eUZSvhrFWLCXA3JbsUDkGKAMqqOVXPtNpR3jsTciA6R2fvv5O%2FV4KTyK81yP3GNI9EKZfPtJIdkjCd9yCh3e06OublQhkxdm0%2BZH5IQ5cLbihCEQdqKYnavzE%2FsbhRsnbdFrNRChFEkiVuKTzSB%2BnpWU%2BvEZjpJN8Y%2BC8Iija0RRzyR%2FRVLG6OpvNzwPfcGZuK9OGypqn78XHSuLGhLGWNue4ibYVMIZTTnbWH0IcJLNQrfy2zbOWjbEUZdjg%2FmZ2seQWrgvj3NTMPjL6dcGOqUBKy563QzcMptSTjwpmpxYm%2BrVjN4ZvOUy0mVopGHzvfkjKn6H98OScF0DFP0YkvJrE%2FGcQjFZ6neWhEIXvkWPEBwkWjBqE9dzAmMlAc%2BS2a0FO1y%2BkUiA7eM%2BTxIznf6YLDwE%2F8WSjK6ygUFPGkOdObd%2F%2FFZvaXiKNkKkPJp8L6nlk6MIhdk52rCABj6gOAdtPsglgZmn%2F8W8LcMRFmbOuQGuRrIv&X-Amz-Signature=e1d0e54248a73591694453c317533487664433c132bbcd1357c4c2ee0b9baa76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFVCNDRV%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034708Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHkSKnIY2HdKgetBJ0O67z%2Bxf3nuDCuWRKq9o5MktxO9AiB91NjMkmdB8akj%2BrPtVFkvZpwJek2ff6GMTIKji9eFWiqIBAjD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMICCFLljrwuqYMisbKtwDjUx4uAVhDZLPLp9hBYh5iIOFRjPzFYgy%2Bio1zE9Nbq3f7uQLMzoDTAtFT6T%2FLSpthV0%2FN3cuthR%2F0LS3kPQ8%2FZVwLudO2JXEZX4gLWiadSfF36EvKBJMgkS%2F6je%2BwrUofwL0I14AF30sjB1BgsSqxCMJKEwLTYria3P4Pp6iBPsmGjH9u%2B3n3BlJhWa2JoM0FBOH%2BT9TxAmjbn4mfD%2BWt5XPMB1yzvcylfjqHafEf6VDHtbbr2gqVGwLaFk13lZypbEe6tQe2CjOXInqEAGDgoPC3tZYR1doKrKh2BygH8qoFr3LhBsxzsAzzyvj4EsTRBs4K5ssW%2F9sN90eYZRTVXZ5yfoenXgiadONFTBuzDeeIjUJQLb3lA%2F89RpiVHzpgLhRCllffAmFZNfbokXLM3L0i4yMw%2BLYR0XlSErCgVBu%2BL0%2FAI61DlCgi6iBVVM6Q9hQKgRrvC1fVKsDABchYZi0gSk05NAXv6CB6djuYWAhM8fPiLfEEmw6ZkbjSkq4BL3pRDrtVpDoA0z1cwK3K%2BrxXvOByL8NkmjAsOU4LKtjy982W5LcUc%2F2Jd%2FJ%2F7HoMeOXCH8ImpbGgsiIqXobgvu%2BRe2TggMJJ07eZpPd4vcfb9BD%2BifnTb9xKx8wjc7p1wY6pgHklpe3SE7YhQCc7IQaOs6V%2BrNc4ZzljrC90sTf39bEnKplHsf61Di9515a5KAOFokbYIY5gAcVOdrUOxJpwaOA3lTB7DxoEC5kRieEz%2BQbWUcRInefApwz%2FLyVkIW1pk0A3zYXS8Req8B3t1gEWGUHu6BoM82bGWW46kb78AdxkKZpbf7TnG%2BLCl2kY%2F%2FyCKD5QSdGTwaKo9wBfBnCh0RgDd7oON6x&X-Amz-Signature=cc0594e4abad2be99dc9bbd7890f6cbf4b3990bb3902eb9243d2255440eb596c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QPBHRTDZ%2F20261116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261116T034709Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCY%2BvZgQ5vh5%2BRL0Ct6VN8avRQBPOejGTIVsOJUNJs8CgIhALqS7K%2BRL5RkBiGCOcPypZsWnj7VbfwOfu7f91nROHGLKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzgLAJJIUOhzHEBHCkq3APuLJ21IQECXjx%2BV2E%2BEJ07U3Ec%2F2WrMZcGo2PqoeEnWViPScIE1X3lHCIjWpYKm3joBcEW4cUgylHQjeT7yMU4c%2BzjhVwMUQrcsTEAjwmJnV1vzpFJukCMVRl8Cig7RiajNkZKnP2GFMCvOvF5theALzTVI9UhBJk%2FgFYOp2%2FED5grxJHqzlobMFwZ3Bqi6%2BjOKAifUhAq4uy5rqak1GetHuALSP%2B6%2FWUgpK9MHi8A22lV8k4hHV%2BzSgdONJHE3xpsSGLbOmATDP3M0%2Br8aBPzmcICB5VWtBlKQWVg7to9v8fz10qfvujxoajpvVXOYD4w0w9BmJS%2BrslCMxVSXDSiit3xzpDENxLydQlego%2FZYBDHtOwVrjMKuMsGncusgX2%2FQsFTnZDrYjZSu%2FIxaUNu56nlcrDq0ygByCRXykKoh5oVoR0Ky4%2FLKb3qPOkKZC5niWrSxaMBOmoyZh8VmqJwgImowu%2Bi7t7v97NhcvB5mCHXzUuRG6euEcgTm%2BmYJntmgJkvn3fqgK%2ByS39%2F5AOF2h9U9zjkQK%2BPcFxT1V3kyzjsCIrw%2BSpmWZ0j971KPuzykf70EgCxaQqQ3wop6XQur5u05Xl4%2FxRj%2BgRf3SoUcLKCupzp4%2BaiBHvq0jDlyunXBjqkAbsa0HVs%2FC7m0BiEcsjum8EWuoNKzbig5Nlsu7SVW2kpd7A03xvsyk2GjQAN16dnasrj39O9xg1NKcB5cYu2EeCqkFbUfViini1lDYIksi9Xx6DdZ8dtRDw6uOWlwmjFHVorydUQX1wfUGlhbSb3HBUzaRMFq3kCdcQF%2FiAb2yaW9%2FoD%2BlgmrGOTBCtLlDiuqXtu2i%2FgNl8YLyRCtr8DC8yIqGFB&X-Amz-Signature=52a5af913d19d6ebce6263abf2b540bcbc1c6ac96b9d1f93b017f996096ee4ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
