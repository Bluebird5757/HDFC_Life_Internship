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
fetched_at: '2026-11-10T05:10:56.420Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UTJUF3TE%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJHMEUCIH2q4WzLOL47LzUG6BTvxTUPeB94CoONDJLKfOiXpsihAiEA5k2tXVbKurAk0kc6aM8FavHK8V%2Fz8I%2Fbv4RWyG%2FVHjsq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDMWeacW6mHXdg7406yrcA4iVOEwNaZvG2s3jdsfpRI4y7j%2Bvr3iv7nWujQx1d9GfHygb%2FfuQqADrcSmy0GnvOtRFcrCtLUkRFjARJZUkgvMP51NUWFkeyN8dVVLBS%2BUqTzqSUxEMFfNkD0RCyFziIJ4XGVCUbLzDi83oRoShnuYgcXUGVwmcn67HLkXH73TNHJAoq7p1Fu6DrhqK9RvX8icgO11oTvBBUYHULT0sLAkOoMQeTa%2BFCE9UjxtRiuI0HHrPdocNGgF94nkCyeM4rMxAvkToNK8cs7grHrNUQj9LsFAQiUyotCB7pya2pYolX48B05aLtE8U5tP2jOSRRiIoOhFX1SmErEJFtZlXufyyeoNx7Q4YlOolGd5BPmBp03Th6SG%2FmpgWSI2Z2QySIshv0jbK0%2Fdvuo9TRjyx3q%2FPmog9cCGKVLidJUJDkcTBq1kgOdtVYQmhuJSIz6ysSs%2FoSlPB1auLeUaqSK2GfdzXea8vCa5w1TfusWkOxX39PFp%2BHQMdxLZO3V4QWGzyPVDqTJnywIc5mWVgDixbY7%2FfEX4XGqjK%2FGjTRDz0KX1CLdF1q0FfeM2oNJLRwtLVanPhtzFBz%2B2vSM00v3CcPWW4AIcdCmab6bCeKu1kWHiyjdb%2Bh2S98LjYfXX3MIGIytcGOqUB45WelUDN97Ta5921aRWcZsDgNXychE5fQVTpOYYVegZzylryLED7mxL6uP%2BVLF2DXJcrD%2FHefCBvK1F0PzY9mB4pLYrsxHlHBAF7tNNSbpU6Gd%2B2xnnmdE77fF7Ir4lE3svCJdluAtn1JgR%2B4nFjEBUrtoHjXHxoT5RF6Eydt%2FELbN6jPFjebzHvHniqnpmRd0jm%2FdLE9TinLxaJ%2FOg2bJ9omz0R&X-Amz-Signature=95aff10d7e2bc30da7fc6d5828d7677c59945db8600be6e92261f3c5b0032b6a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677KMMUBW%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQDgPoLaAIOrY0cy8A8VTjcpXMVtqBoFqLOnJLoN%2F1k52gIgMLYK0Y%2Fl%2FMTnbkv8GDsLo2onjMpa17jRPV6nhGCLsUIq%2FwMIMBAAGgw2Mzc0MjMxODM4MDUiDNSBCJeFLKhXiDmkYCrcA5roYDkia%2BVCnOKf0U4yF4%2FJPoM61dnHfT7gtpWOqZlNPgw7kyeN%2FLEY2eVcS1MqdKAdY%2FRgFy7Ix93BJpUk0fQk0ma0mD9v5yMgKdboheZEI9pDj2nr6bdEgzgSfGvbl3OGUgAViioxSkvpvBTDzmqpiLwjMNUWJw5wrqrBKIegbP1pdnboGfgOigHLuAHbPWXr%2BIRpefRNHHWCaDC0Og%2FytBPJJB9K0ILcSOoQMJ%2ByMQj0%2FgM%2FTdkcBCwks%2BwzFSil%2FuH83TgF2piPuDQM7JdH2PlERP1cwi9LchpQJ3brK2F30CmZl2jUKpU863uec29kkzLTjjdC%2Blu76bFJiVl6wzeLpjTyyz0plD%2FIR16uGhJvwScEXQPhYxK8tg2SlyVW17xynyx2JeX8G8Dc%2Fdo4%2BJl7%2FUagsd7QgO2zn2cY4OhRdgtUZyvvTm5WdBuewRjdz5JgAAFSg4kaCnlFh5qdFjbReG%2B73jz0iXBBaK5nRE1Jz07JwJoXzOdTACC2h2ncZuRyQgxv4L7chB3dlVpGyOuSf7qohydOzQaY6nQ5Bf9p3NLfHBYPHj2SkMez6eVCUATo90pVdilOEvw15U6AFbgc%2BWNgRoZRa63RFkB2bAi%2BbTDCsYcZGA6SMMi5ydcGOqUB6PmIZKenySSzGA5%2BkRCkBEffBNAH3qFGDJHzpE2DaxQdoiMnpMLmiXuX1klZQZpL9zLHUWLrt1AkuwLLnNC5%2F6ijr8rUlk61bCORQ89d%2FN0nyiiF%2BitjyIQ%2B53vEpiDNZBkR0IWECJpa0LlVkU7Xj0dyEQGgiU0pUdD%2BvclmzYCdU0VSa6o60DPzvEKlbKEilqJhOdxXz686Rc8w3h2lTh3VeTOG&X-Amz-Signature=5d8e79187d53aa8538922b5b836c286854823a485724bc19865c9532b788d112&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666C5RJ7VX%2F20261110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261110T051054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJGMEQCIEo%2BUq%2BO49Ufw8yBLqg1xidFw0CJFYGKNd8h2d4WePb5AiBxejsyXrhWHkndcoJ9kvs9mOk1y26x%2FmIy9gLUOQRdRir%2FAwgwEAAaDDYzNzQyMzE4MzgwNSIMveUqyrEv2fFimkiwKtwDtzXrEISGN%2BRZksQi%2Fk2edipLQEqIQF4whN6%2FqoupiwS0TfNbo2uMkr%2B8mgvHmzlEh2j01s2FBXiP110NDzPKfdUsd4jc2LUDoIwlgNs2DtG9JTdOUGdk76hd4soryBrbqSjQzPfJNaDpmjeCS0rL2IpzsP83yvPNymTxQ3yxAPJPtOcKkGZ0qhZFt07OGdVxls0Zsytc%2B0i%2BFFqjzyakOuwtLjsaE%2FzqDWG8oiebCMRQVFY5t87MD44manxQzbjCdg2WaTfe6b1A7fhpI2%2FIEw9i4U9aQayvbOrrBD76x9kjhkj57zz%2Fx917HDha33m8r%2BsKV0WxArj1vcH%2Bf%2B27QXx5lBITs6MkomTn2cZhBocjuRj4XMx3UaW3tKu2WmXYzFtFEAuFyvh%2FZLqz6wdjoT2cypm5OvsqCpkX9wyy9MnKH43HXJrraYIX%2BH1TidVbseGYzz2srUwVda2JzAsyJYmrm6X9rNqvQAXGJXkxwsUovqD909jWOKRnqpxEYsVPAw6oslcBctnuzVtzTHWtdkE5T7yp5dOuLuwZ%2Fd0PE3PSJltEvWN2Ivd0iZw%2FEQpg7nMzHsbEWDEO2%2FRy5SdS%2B67aPl0EcXG4QSONUhVSusS0GzR1OEb4MlGcKj8wpLvJ1wY6pgG2xSgRAee0pvTrOhpIbNkQCmab1HqPvZ7a8LVlVCkJ%2BBm2pAAv%2BzjWg%2BwfRTs09JNp9L6o94Sigf6Zf17ZXKQzFrGaGgEhUSzoSfZYlevJyNPqQzqgGaOJ3uySyxNJZMtvBT7Guxg%2B3R5lXOQAvPN5VnDt6GbeOxZV5cIEPrHQvi7ANuH6WcFgpv4SQpLX0CqcXHCNo4tOTzq6X0o%2Ftll%2BYRedIj%2BJ&X-Amz-Signature=f398c78568771bc0372b0f3627baa2b7719c462465c7a2452ba45a08683f69df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
