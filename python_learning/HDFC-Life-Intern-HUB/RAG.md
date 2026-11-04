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
fetched_at: '2026-11-04T03:52:50.409Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SUURSENI%2F20261104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261104T035246Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCpHfKt6C4J9nY017TcK9k%2Fu6sDcsDfUwD76UgrGT1ufgIgYIDaXNxAQkYWdoPFlRbMl0iPhawNqtFrsj3dopQbSQsqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAMjguVXd73Gi%2Fw9VyrcA8FvBJIGBy9IcOl22M7qQ51nPrL1pNmgCnPCq8wUKLX49Oe7eIxWJR0f%2FGw5CsgMM1SfpaklRX8d2jsj3B9GXNLvCFI0UUnLQswnfAoaC6ruRiNk4d7yFYDVMpXDMu6ZqFEujLy9jj16%2BVxn9szasgRMI8S1BoSbU%2FGvGYY8rB0ni5cDZlSlEUHRlFdJBbTNh69tZpxVpFX5jvAo8AB7sdEEicmZtN6lD46%2Bd8gM90We7KDDEWafG0iT6j0bx2gqiRsGJUqE15XMFaEZcmIp9VaVvyUYkUwCP1Rg%2Fy14uVoy3CC0jn4UglSxTD%2FhcSDIVSOTilN7dIIBfPnp3cTOUZ1qhXtFozBIsaA7U0vOzCmlrf0%2Fe86tZCO7aPB7dj9J1D2dR592gxdsFZ1eGoOjQqFDyxWitHhhYTaHcXi5nni1uV4gpWKELV36aIXY9tb%2BoIixmifpoY971WaSadtw392cpa%2BnTnjgPeA%2FXIo9hQfG0H0MLgsTaZLy6c9nzBpKcWeysund1vtYs81ANCSdcKJjAWGgavJv4XHxQltLKCU0t6aRdt56KeO3wJ3mh0zsQHy07vGKsa97zQwzwTd%2FtmkQn9by1Uk0RnDPxUvYdR%2BlRdBe2ejshUlc9gx3MNytqtcGOqUBRKmNO%2FDOOmByc68tzxj%2BMjf4KQrPZVbAArAcuPai8kDjy3OWuqwpu0cnDz3GpsfVT6%2FqChhn69kktQ1fEb2WZyHvIVHjm8VXTVAOyFndantAjdJ7%2FHhLOur4XO6WNRVt2MygdFHzQuAvRt5xzLSeYJR0UT%2F00qe2TYpZwUQ8tg89dXMQNEKMdMgCcCs336wOew238BFBjuA1HkquJFrHRbY93EUP&X-Amz-Signature=abf8fbc6d6d4a27ace9227895699957ca557247f8eaadbd22ff200a72940be4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667JMLWEZ6%2F20261104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261104T035246Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDxHXY2jnQZXx7OKO68ujcGQwUoxrWjIHhPUDidSf%2BNNAiB1GMrIDJHT2DWA1qoZDpP2YhS1Le2SnpvtQcFjExBkgyqIBAij%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMR4LHioGDHn7%2FvtVzKtwDajM1v4Zjq%2Fqnsxd3YOekdMByUt%2Fbg5N2GJWAU1gr6GFjz7Q%2BgX45FQN%2FwxyCK6vsgS3c1Xmwxj8tzYD2m8uV028hQ6lPV6%2Box5KLVf5chmuxVCQTSukyp0VR0IiF8kHllnof8G5y12xThsVOlsrnbyaH9CjxwrWVqID5vC4t3%2Fzigb8AfvQoeFYYSsXSgHbjCpmmNalSUzARY9cEvDtdWmX22xtHpM1cOgSSUpMN31t%2B%2BFK%2FeHCpwD6fJq%2FVUntqfdmvLfVtaxMF0rYyMUlcTayqC34MZMjX4oSH%2F1Dfu3Qe88Z2pd%2B%2BtalA8kfY5LjtzwXhInX%2F%2FV2hf1HKua4KBBIKcGyY0aaltZfJALeqYuRBZEF5Heea3ddYYl679syEuwC1895aziCawWjYN6aJgKrMox6zpyv77uO7ePNeW%2F%2BXSHPpAzBLnluMw%2B9wNe6M9C6dW5I0hbh%2ButEQ69BbLZdrKWpCT3b29ztckvLR0FJX8xJEICXPZkHmJswzWYdXs0JIUYY%2FVhGo7494UCH7P6c%2FxTxoSMfpajcXv2LTRPzVyu5gqq366xHTNgNSq8F2JzAOWch%2FMTNr%2BcdscvLFtKBF9bQngkFd7JDoO2SiJ35oh9c1b%2B5ZMaXQ3www1q2q1wY6pgGFUVm5tvMqZO%2FnrOjhEZ0RPSdBhBQRq9pqbLAcXz5LoYOkUDvZcHFrU9wIZoY7zAP5wMwLVru8a%2BK2%2FLuHpSUQNtlzCF2cHp2%2BLIIbJ5bR3agMq3ZzvCU3FlX8%2FRsfGlptv0s0R3KROX36voYeFCLUQ3QOQAXVtqwQa8simC11WC%2FhygYnU%2BLM%2FrpRYoYTgjfKB8naJna%2BtREUYbtsK9XClw5hwezT&X-Amz-Signature=ac38a745c41cc86ca68a3d2773cbb0ab32e934aaa5b2daa6e07bfa4ebc5112ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THSO5NOQ%2F20261104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261104T035247Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBshQGqi6eJb4c5gi5Libm%2BAsGmb6YvX%2BXLQEdiCHrJkAiApi%2F4kD%2BTmY9ZHWvVbCnRMpBm%2BA4AZub9FaMgrDVa3UyqIBAij%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaj1hQjHA3DbAYvgEKtwDIlFQcsmuJ0P1EZZTillBGlJH3DierJcTJ091nMFUEtN1FKtrZXUX%2FQn6octVWrkMaffW8ZO%2FHbJSML85K6DKfamXhikwNTrz5i68shlmd2c0OKMo9EzAuoLNaTxtjxWOpe4O8OaKM9zBA%2F8m8bwYwDtViAeB25rqN9Ey0xJWpKVVTWdYqJLHfjEkmWGMwH5l4%2F0MXVGQMmhOmHcsMDxUvFW6%2BqxZg3KW8w1I8%2BaNgiG5H2nae%2BwSEJTYKduM08ByGRfuvbAMCNRoSoUUki47s1PvEPr1nnhlvsXahEPCj%2BpGNNstOsmcSqnqMTw4gv9cEfKpraIrBWN9bKv0Mpo6HhJ7n2zKFN3nHWOPGsn1fvXNf1dyi7xtf%2B81uSzJN5sgHAzrDQh%2BMWQ0%2BtRcwoRnpeh432jva9SEstejY5xTH8jiSQL05qivDRpy%2FMbgGNbU1yMOdoUCLh8OUXTeRAD9jYcUQGjJt3g4%2BgreY21HrrHmSH4awyxq%2F%2BOy7yAXr6v3Oa6AF4xpNIys0uLGChErQbtx%2B2W9EhQuZ3JKkmXx91oW6wjn83r6iUgW4VfAk9BK72x4yBjaTfxOw2viJXE4rqg%2FKeVWkssUE6H%2BCCOuO%2F7f2ad6QxsidDtZdy0w262q1wY6pgEbqpHIapqSLovkARqnGqspNfZo1Pb6glwb1%2B2bwfUJ3C6n1AcjoG7wqGzlpkwFql%2BkqXSHvYVGEhHY9FAd6l4KpygKWJqy8b%2BRGPGoA1qYsVlGB%2BysRLp8roX%2BCFxpdzXBcKk3xKBhuCdQg5BQNG3MfZm8F4eYcnwAqskmrJTOof8SGr0kS3HVCNh9HeN6wVluBkzc6vzZtXK9tkO6RF%2B14NUK85yy&X-Amz-Signature=c96559b31a98654ab7b2cf922b70a2469a4ce6cd325b4fca88e95971730b1844&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
