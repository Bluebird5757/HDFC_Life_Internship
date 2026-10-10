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
fetched_at: '2026-10-10T03:18:42.940Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PFJNHWH%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031839Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE41z0CIhYhY74h%2FC174e0paz1K%2FeoueCpyL4OSIbr5vAiAw2nS1TvHeJUJYIwz6GkAURBhF%2Ft1eW4SFKFii6Ekbyyr%2FAwhLEAAaDDYzNzQyMzE4MzgwNSIMAFDQ%2Bi%2Bk7jPLXrVwKtwD64lMm6iwPvbTlbPDHUpY3Qk8jPiUwoi1%2BmiT%2FjaJd6lcgdhSCcgDlFQo0l%2FilfH1vJUnbUCN5LtHTfjq8D2i90Vkba8STgtTjfCLF1KTBwBJtzOIqOVwENiX2jLdmMLTVQhENI%2Bdp3HcAyaPGtTuSuMmnsBfiCncMzPaoA9mx9j1Y2toeZgXV3FASsF%2FXFgQ15imolvUJpOdbHevof4b%2BJg0hqRxj63HnzNdhds3FDWS4F7T4X8EXwyXvPKN%2F0BXN4hdi6Bo3lQGBVhi2qelq%2FMqANMhrro9l1PtitqEf%2B2A332Ei0zz0eTSuacqbUsoyIFOe0EwuYXY9p%2FahQLBqz6L8NKImNzRFUotsdAH6Xkyba1kM9BJ%2B8dC8tsT7JsrpbUFUrUJl8RaJDympJ%2FkptWfOT3dEjjc6jnCeMg1wDTiUvURzHRMUs4NiZVV1YSY8Yc%2B69QQPbwIUMCyRLTWy8YkMCWOLKJg51JqqY6oxEOe15oScLrYYlcgyJQbNz39pYtxixlay%2BV7GwPsGdL0gDDscofsW7c3MRIzxA9rp2cnteO20qjqj4xs013FUh6ljGNXo5vh3nvEb4ueQF0mMf8po%2FuC2URKNMNstaTFhAiICSnN2DCsrQMIo0ww9sam1gY6pgGpFtKKjnqEFwEyKuQiw%2BT8prvGMIxYwMVSoFl8QJK48s0h4VaBZ426au2YaGJiN82TJ0QlVbXCjjEohcHp1gRX4gxwAOmkaYQ2IATwNKHZiWyVWQ7R%2FgBaofxx7z3Ac8yrLYvNlsJOMfXKP%2BoCd%2FfOtq9Y%2BF6q%2FZEYhfVoUwJfiU0eRhvcFAnHgdaOzPELjo6jht2BqmuIJG0y3aPp5xaqj7d%2BtpAV&X-Amz-Signature=6ec283eaff13d8e92f41a55675291e00811a9249132c127cbdedc3cd690ef680&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQ6ZVJ5P%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031839Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCAQwM61PX%2Bflg4GQoxD0CHBKD4QgKKtS481plRIzIz3gIhAMlo9l8a1k1ZUKx%2Fj%2F%2FBpvMYkwc6211BbkJRefeDPE5QKv8DCEwQABoMNjM3NDIzMTgzODA1Igw6EEwgQAn%2BTq5yXJ0q3ANa%2B%2FQc7UOjNcrYopIMymR%2BJ8mGC%2F%2BrHK2wLKe4icsvlkdwvwHYcUeyf8UVBA%2FZJ%2BEE3mIzsTGv%2FOcJ%2BxF6TZnC%2FlQkLm988%2BN61CHg7RfyKx386HK7NhUZa5Vv1hEX8uxUqdfEvsP0Y2sUTNCDmwIS88gPTGVqK9zCglhWEL3gcjG%2BKDyk1vgbZpl8rPJTgt4M%2Bk0lzFnLhZdoz1oEFbrW18hTsxpdzkQQr4dcf3gdj9okOMN73IkM%2FT2dlPaRkaugsgGDlt0MsY9vHljwtTV1JcbIWn%2Bt4ISqPHS2LqTxcTTA%2FDxr4qUc2hY%2FXaW%2FWZRB8LaYpu9WHmQZ%2FnupCi7Dg7o41mxxCXAOooTMNIwaW7%2B4mWwQd%2FE3nh2cPS0Gv3ag6zU6X3x5RnXDDaFlD8941YX5z6LXjE9r%2FQq9%2F7NzBYR%2FffOUYG6Rvg8n92e0TPLNssiB1tTWtknUPef4%2FttzUthDdY2FZMZ7HcaaIkcVPMm4R48dI40Jy%2B%2B93I9M9nzjhXRyEDM%2FXB3n6tmLivjGRx8N1jEXuNNZj7RFuZtbVZ1tyCkJH2bEvhNR50RgPRbthHM1z7d9Z3eDaeOEA3YJu6g0xmHwbLWAHMnPjWYbbmjGWbvdu%2FuDmkSUyjCjxabWBjqkAdWN9HHpaT3KXmpG4oTR%2BNhKBxGRHpp8qGI8I1uefEcgm%2B94mZq0B%2F4Qy37lPm%2FSXJCiEz0gLhrJi7R5tcPXjkcqG7tJXGnez84URfm2PaTi0UXg13CH7vdg0yGFxNTchLmJuUovtE9p403R0w3Q343QWHOZKuCqzZ3hkdYvNr8HEByQxBcTMXNT3Peoxz7Rwu1oFNKedo%2BfiPchdMVpwEbnmQEa&X-Amz-Signature=224d87d17d9d8799497b32bae67e0b1dd500710ba2a360ac2fa90676d506cd49&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663MFU4MZT%2F20261010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261010T031840Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0E5CJHvLKHATO0xIZ7WYc%2FbsOhWieZOfv8MMSr0OzrwIhAN1wDKDrMXK8Yy%2FvzCPAClSumNk%2Ft63W1EIEdBswnElMKv8DCEsQABoMNjM3NDIzMTgzODA1IgwuVlSstJSKIa6luNgq3AM19MYl%2FsYzZJXlhRSUc6fr6TidKQ%2ByX5gbgZ3lEAF%2FPEAZmhNebb%2F8MXlV43yEOiUXpZLtjzTGSNqe4yLQ%2BcbHHYx0v5EFhsp%2By7xIjzhksgGhK4uAUthDVT6dM39p3NHOAVMtiZV%2BV5dHkPe%2B77lk%2FzOc5Xzgbq7qZwWpkeG7sATL4DsL%2BS4kkkC69l2m%2B3iCXSSq1X1VPOwln92AGhX5N4loPP8mKSNS%2BL%2FW5ehvOfebDHQSX9dOlQ0XedmTr4NHVSe6EOlH1E6e7rEDIJ%2FzkQOV2JtwKs4d5Xsq%2Bnc369ekA4VnGuCFU2oTP0%2FCzfP2tks0jMO8anDHzLGasQQpFMHI9R1Y%2FfuA8QdrB33eY7wTkhDFuBQZkwjn%2BTi2TfyJIhPLMTbVcbE2CnqLKBojpUnygufcHY%2Bnt3AQ%2F%2F9VSBtwHb%2Fab%2BSILlXTc4ZSNut5WGn3MUx1bFRKTZQIwZ6laVBaaZUE%2F%2FOWYb61RmyCU2R%2BjAr%2FILYyZkFaS%2FdGdeFudtmFBcz%2FGfYBN98oOtz5NeJmvHB%2FFyqXuwN4fBLaUqvfYoGO4GyOEARVRgtBJGyueiMrIWEUlw2Mon%2BXSnXwf%2BVkAKnvkS917Es%2BNQJZJEH9T4YeD3LEq0Z3zDCJx6bWBjqkAXhcq%2FQJZDiI0%2FrYcWS%2BjCIt4zUofBjjC4XrIsyMlOupJjGZUOlWSPy7sjFpBUUd%2BS60NRXPxBRehdj0MUIIddA0LB1WbrUuHEWr2c%2FFkNn8zz4K7G0n%2FsrYx11BEeNTXPz2q2mPrJUrvepHlNf%2F2jqit1k15ZbKZeTeJZL76lRVIXE61OLxGDWjg%2BPGB4XIE5rvgZaJtqqXH53crHLLs5HzfG9v&X-Amz-Signature=80b0d5070d164ce6883520b01375cbc669533429f2b107ed3a3beb51cfa1086b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
