---
notion_id: 3c84fa76-9938-8047-b507-c8020ca82aed
notion_url: https://app.notion.com/p/RAG-3c84fa7699388047b507c8020ca82aed
title: RAG
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-30T08:00:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-09-02T01:56:38.867Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646URC37I%2F20260902%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260902T015635Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGBaBMGCYTGFrq9FA3%2BSmH5gW%2BdN%2BWs%2BQ18d8kcOoWOHAiEA9xMlGN%2Fa6OdFytZ5o1IhY%2F3NWZrGG2UHAEMvadKeR74qiAQIuv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJKdfHRFABnXDEINgCrcA%2F7GuZXosB9WBDeLs1w8FEqtBWUWC9vSIK37%2BKd%2BnUcvGqBzV7piLiO7Ei69sdjl%2F6QNJhv0rXYQbgKVUGFaUNwc%2BzAs9o0kSni%2FUnHHERqSD0lmqyZy6zBpCOtmVMWyp4XDNq2gHOzVzmRuGgNWH4gARvYrf2rIJaPXhmvtlDcsGs4Be4sdqkwg7vtiG2JE%2FXDSGsrAfsHQCXllrudduF4HdRmEb%2FTS1973omoy1bU0tk9rjF8StqzPbXrY1hwcGIJw3dxVKOnCzH45TBlCCssGSkOzT6iU4%2Buw%2FYwR01aTtPCy6TvJSeO2%2B%2BQHe68LPBT7IDpi2zCdqX%2BuUPo98KXy3AGKQ57urHKGHQkCy%2FXuDS4pwEVyNdjsmZnuqhFPki%2BM1MJsMPxbjLQqLM3gF2ddDUtRLmbb1RkwdOl%2BiirMinvNgpeeLvAV%2FFy%2Fqx8YXx3vdCoGK%2FIKfKLW%2Bu3Sxbs28zsHYRI7iq%2BvqQGkJ%2Bh1pjJZYTjurCvri1FbaO6ljFG83up4BcNKd%2BploIjjRsfm780NJHVJkKEMiVQJwPPVciP9oGgxV0ck4T%2FGYExSUAUOWzXxsPI2sACorQ2yS5RJyTodV19VaiLfWRoO2BSgbeFTHmvbokV04kjYMM7k3dQGOqUBSY1hTcjuuQUyNzhXBzTaExwXS2Mqq6JBJyOVZDBXrm%2FXF35vaJmdbEp851t3XqvhOQmFKA5BrSwTLLf%2BU9%2BbpCFX31lnZAeecoINJLrPCMXndLp8ApV%2FuPzmbE6CSQb3UvwG9%2Ba%2BeSPbodcp1gi2vAy0rIR9AxqeW4eemp%2BOsDdTpciEawUzIycfF69xcgcrxkKwroTu7yWFpvel6ROkBC9j5A6o&X-Amz-Signature=5b93b8d389dc6d8bc00a685dfcc8866029775badd6f0cc3b879b8fe20a3b1d0b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TUR2AP6S%2F20260902%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260902T015635Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC5MBQSlVDnSr0SfGGx6hGxB4aZ4NQO3eBw8IM8%2F5ilVAIga0fhMZ7FvEe3DMpntuZ9Q8E8V2yabwwCkg0bYA33Wp8qiAQIuv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC7P4nTOdURU69USbyrcAw3Tpj5a8jyiwqPhWiX6pb%2FfEjNMTgRYWmfDbrK1m2IIiP%2FuKnxZwDLCXC1cUGrdYt9G8mBiJnI28m6TXMeQGR9vbhzuLEh2WfZZuK8G4VYguzzQQNQOBBHuZXjCT8YZcsiG8O6jPwwuiWGWISYYNbscG4JxnagczF6bp3PUnX0EH7%2BBr15lup1A7WorJyJbjNXOq6VU2kUt4l7fgX2WjPe3dLTjNZ7bVChtt1h%2BuheH0p%2F7Sw3QtQZpJ9jsrF1pd6fQ1Ckii%2FpJaOAV5mn1QEaoR7LQRZ%2B1F3qylC5iIFXcdGUEX0QD9bPS3ia3MzKcVRQ6t3fi3W7T1tHdRWRDVDvhp2stNsJKrJuIFe8ZPPDTR7AVHr0pPvGESFUuWGTAdgWFDM54Uf4XSYFvZNGZzfWjqwSL%2Fny3rEJGvnX1%2FGVslk7CwPUiL5KeJZT2ZNWxnTk%2FyHUUCEPx%2FgDwbB17Igr8EzOPRFDHlA1SbYshCs13hKMM13Yee4TYGcrpq2YOLFczIzfzc0uGW3MWqm1cWqo83SCCQgWW3bRFqD6kVLxaxtbv8Wr9KIM%2FRAmJe8LHcZv3GH1GCmne3Lf%2B9UCykSIjCGrpsIKpf%2Fx1WuyCWmJB2Mr%2B2uq3iYeSy44YMNTj3dQGOqUBasEUTlqlfgXOszHAuXoZQuiynPAl6hyNDa9BuBY9DJthDmG%2FqKCdI%2Fxx6xA%2FuS7c63MCb3AeSFa2M4q8WBF7Eb8TDXp6rIj6nTVWnIhyQKwYM6LCz9bMGvJP7vPoW6%2B54WjSd1SA6JF9pSPeuRP3PxTepOJViI7Ka9IDT9Fz1Awz37kyqdseiJU7xfl7Vy3vdKQiifYDhBvk%2BcS4K4QfZ9VBdQPs&X-Amz-Signature=b333a00fda8d41ee80f184028749372b3a276e980e752805d7127c3caa74734a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


        Database:- To the store if add features such as Distributed Architecture, ACID Transactions, Concurrency etc (eg:- Milvus, Qdrant, Weaviate, Pinecone)


        Every Database is a store but every store is not database


        ### Vector Stores in LangChain

        - Supported Stores:- Integration can be done with many stores such as FAISS, Pinecone, Chroma
        - Common Interface:- A uniform Vector Store API lets you swap out backend

        ### Chroma Vector Store


        It is lightweight, open source, for local dev and small to medium scale production needs


        Chroma Hierarchy


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662RHMQRVD%2F20260902%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260902T015636Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQChJfWEgaSM%2FMGDM3gJAH2C%2FtaxpvCP9ZzL90U%2Br9oKuQIgUCGJNhc%2FuhbDUZC1rx%2FZTfs2lcwhqiydn2mZPg1JpvAqiAQIuv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDdL5lNpq9KgZap9iCrcA4o3972I1ZaXZId%2Bh6bTk%2B%2FjPaKXN5aQDe1QIvQ63jVVmC%2F902mPExS2HkjZhNhHNPnJHLgo6pFWj4tTW5PbQ89vJXVNio4SpbiEVf%2BCyhJpUZp2EG5hzjIZpaaxGhpT5WHJj3zBhMwUZg%2FePtsEOpc9dhYKkgPfd4kY1nuAL2CWwmviVEbMCeKth3IUu7cbvAeK8Ovn0zThV07plulVqB3ZJPrJy0364%2BbvjJX%2BVu6dSQkk8Z0qfQoIcycDKsC22Vi8ipclU3PP5JYu0iVPYxdbE3I6JiU1OL0R%2BfbiDgem1QdXbIcpkHQPbS1S3uupJVYapoIDlLD%2Fn4f8p37ZcjIZimCvSU2k528x%2Buoccl4bcm58ltLYdAxVso5BjxV0LbuQ6nFS4xv0KXBqIhdNqWRdThBaap6bPxTwh4GFDeRxpHw4C5mgcIyjsyY4PCr8KQks2GCdNwK7J8Dev8WClWLuu%2F6aeHaXqAj%2BIYTBVD96ezmYNldcWOu2p5cyuv2%2FbxxKA3B3pl3pXKo01rMmMsN2lWXq0gCM%2F46%2B9dumXgC4UaDodaQ3u1HxlZjQy26z7xnOD%2B3hFs5yTL1mM0h1c7JSFGVUjicBGYLoqYOQ5gErr%2BcykvnRA9CfIylNMNPj3dQGOqUBpiEL4dZyP%2BhPV9wSuXjdhcJo4ekJ0p5BquvP2TPgRwJQGn%2BSz1t1lDcAekRktyJSxjlsBWAgs0y3Gh5Vtb516UzN%2B9jRZwJyrmaBP7eGkg0v6bJdsUWI03QDcU5Z6X%2F%2Bw9FrS1hXvHs66nGPAQbNdfCYajLIAJrJ02giQIItywg9ScI%2BhS%2F9j%2B9ZI%2FkSEX%2BLIS7i7sz3c7F4Oq79%2FkJLTkuw1Wx7&X-Amz-Signature=6d072a88712d7bc0f2be6a3e6b35dde423c2830becfa7605f363e63509e179be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
