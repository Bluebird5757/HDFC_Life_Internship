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
fetched_at: '2026-10-21T02:29:28.468Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2MOWZX7%2F20261021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261021T022923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICR9YGqyHP1G0rNW9qpnG6NE%2BQWectQKVlWIgMBpXp4wAiEAzPsk5CHQtUcjTQQmiHin3o4NkMEhKD%2BPgDyifhvDQvEq%2FwMIUxAAGgw2Mzc0MjMxODM4MDUiDMVz94zL90YgBW%2BC%2FCrcA50thV%2F1RXqTupNUR99KxuwhYERihdnRRQj8ksmqTWvsbAKjEzzS0rbzh%2Bhu2cKOJGKDgXFhWZXsiXMMv7HRhPfmwAX6SOX2jM1KHEl33N9%2FDfcmEC9obtx26OxpXpl4HN3ud6c7o30gJRVWvvzUOT9q9kPtKLUziRZ%2BzxeEXNhUFCpocdZVkxndkW5PUWPoODRrXP0Ec0pKp7rg2tu%2F50%2FSajuyxOifCXlEHw5j9rLJ6dPo2q5WymvhcYZ7RRMKPreEWYAMMp1R2T7pfiZaCTexP4YJBsNVfWWWZwu8ERw3M1M%2F7eRR01Yi5xjgKRJKXqU9XEDHb8INcyDbnYbu23Bo677Di6KLoMRBOsElYPtejdGaWLwRV3THty%2B6SlLAn5g%2BiUyDZFIhUAGfaYEW9BU8WNRLEjaSwvcJ2ai6noyaFtt7OXGaFTpdd7oUV5ATs0xJVsPbF1aSvJakvv2YlrmTL5MRICM0pRUrdcFXKH7w9Eu3NAMRpBt8np7RqWsT8MOrdX%2BgY1V6o8lFwZuTLSDEJDsy9utP4ee5%2B6E%2FcoXtvOQRAJcyZul16MubgQ8x0oVvKmB4%2Bm0kPg%2BWCHcHCSuWFnljXgKW5RfCqkQ0tyW%2B8DuUHoB0CmfAE9ExMLrA4NYGOqUB9hohSTfdxywxKQ6XqYiAgYHyoDxkT%2BwwlzO7xtoFN9glpuqH0UmGOZtA8rdsBTq7rVLrcuqT5wUrhLgkBDVCtA0bAVWOCRlia3D56YAE%2Br5UaAUb%2FYSdk%2BZNKRcoKSRShW1QV4g14w7yhQe42wON5%2BWE8ShEbvKxIzHETEpvCYqk%2FqTieqe5FusoJjX%2BJbIkaqRuyO24mbp7%2BgLaaxIdp5oGZ3HO&X-Amz-Signature=0de38147055394e737a8db409ef133e095a9d44cb0684d2ffa1ddb0a0d291e17&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWOJRKY7%2F20261021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261021T022923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRKIOOVbNaGDMPjSux7qbjAv9fIYeyQDmtJscFhO%2BUEgIhANeoNxKJoYw5YqD5451yDD4Yfs6x83Ko1hSD0eC3sAxsKv8DCFMQABoMNjM3NDIzMTgzODA1Igz3SNHJmVPMZt84ERQq3AO6lAXisJUhHldHH71oidOV55qmSj1epwLxN3fWhvOQyHMW8uJSKw6hc4B7W%2FM39KR2UoJlpZiPBr1xOU%2BBkLXyGK5o%2BcX87ckLZiWC9RcW2W3HF8umcaTZ2ox%2F3%2F39IMLG%2BUC78oO3XOMdhd1D1Ya8WMm2j9fJwMARn529qg1Vf%2B%2BKApQ68nGM6opgKL54P5%2BCbmfSPhit%2BsT%2BUbdf42%2FuCnIIq0VWk3luKxaNTFfVNAaTqFXi%2FmKLlduJvErAT%2F%2F4w%2B%2FDfYIFEq9RricMlTlZKEMYbqt0B5B7jTgrrQfOOzUiDjExpYC8z9FUdq2DvzM8VCodTgj3qAiiZ3ecItSTS3lEB6eicseR21Cj5%2FZYfzmQelYsnfAlpLXjyPGHNdUcLH0RG9kikBKbXL5ifQRZ0sulvv%2FGEFNYW1Ds%2BJrOLx2GlX%2Fei7%2FrH7n5JmK3WAIJavC4z3cXFI6bSzLkXvUbRyLL%2FdtgQ24kHmZNEt5nBbnQf1lh4NTe63JMA1xMfh5FkPJOrJtNen%2FUFFReMrgWUD3mTYH2sJnR3lXRX%2BsrYwkLqbnIqe%2Bry3i31dBTSetkdKqxQq5HZRSBBSWu79IVc7tM97YHvnpBXfqjvuSbkZ6iDyVBAEn5HHFyJzCMweDWBjqkAQtd8Bk0hJ1545g7mLDUJJUOR7m8C%2BfPgdDmXbJc8QXapDkSOhTP%2FZ3YSkfNpmPnEtG4JrjvD2FivVywpmq6iJnPX4lgfUFE2NxpXrkKhYU6M54SPPUUFVaUr8ZKuKHoT%2FmUxRzQV38Lx5bkj%2Fk9cjXt2FmkVbQbeD546pfm9mD%2F9UmuIXZ4TccCZa7neZt0lsKMYsUrmkGkyb28mXijT6LmI3%2FR&X-Amz-Signature=0647c7f57cbc30c3a94a9e9b4c1796477848991ed1b5e3a7a18493b90ee818be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCF57MGW%2F20261021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261021T022924Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE%2FFRrVtvLmBcSo12M88HwYoG7J0f7dJTNtJ6fxWjDdcAiBCp38UGYWtWRBJPaMUFnOYp0IJv5XNPqkg1vvBUfjVkSr%2FAwhTEAAaDDYzNzQyMzE4MzgwNSIMkjG3krwvTJ1pDpIqKtwDKcndxopWAzRqALfFJMNeU51l2bCjcQXNlEZ%2Fh9MM1VvAHva9n7uIp5kPYEO019K0zXjsdL9H1FA4RPrBIIi2HTXnWJUB3RhTAiMk1xkBx6QEcMT1McjMZti9j2nF8jZg%2FemyWaJT5xEjvGt6PS7t8qDnRIvjPkqQbGWMlKD3jbLXmOCBYSGgAF7NwfLgfb9BpxCoXOlTVxn6Le%2BAmnZs52CFDGMVR%2BttimMLJo8qRvR1dmowirTsG6YWANRUA8pFnm3MPc9RpWbhKNPfNsWByppBOSEC2kdJsvSA1Yrr7hOXS81d7k95YeX2t40Uqp7tDGcpy79fBvN6sv1zOci2t5HJAv8SglI3QB4bEexx2sNO10H4vZ1b4jIcuLmIruJST5TCaeG4nr8s8nzaujyPZ5C8z7eo8Kvau%2BdZBoUI8scwepVldlOa0k8S3QRJDZ4Yt5TEwyC6F9YiwloU9LCLqidCV4fp6HTkozMOtHPuj%2BP8LgJew2DFFHIYRgaXHOsud7yNRZgbilnAnaiM4Garb66fi0y8ABCcqOL3C%2FcsThT11LOP%2F8%2BG2kVSJwvtM998B%2Fl5Cqkxtr0qFyKQ90EiBFiIJNPmIlFaaw8%2FJyhfqvRN2PHiURAjTcL4z2Qwkb%2Fg1gY6pgHIpp1R%2Fa66oVWvwrGVWCv5nn8OnfiwWyiAMGqWC%2F9JPecmKethYNTFySmC1VYB9wG0NaGc7GTi%2B1q%2FTiBteNF2B4gft8Os1lxlK%2F4Y4gE%2Bs21q95GyMOFs9r7eW8A5Zyh86SsZMvLE2DVM6cAJRnOQyF48hWpbA1QH1B7521LztZjdpeGvIJYE5ASmwgNUmYTYyPhtGckDeqHkOf0BeoziQSTzz6qS&X-Amz-Signature=c55b3c53a727c1de3a744561c4b29db2a759a6d9edc8c6b33160a3ef781028f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
