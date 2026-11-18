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
fetched_at: '2026-11-18T05:05:50.457Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDJHY4PY%2F20261118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261118T050545Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCIBFDdIyfx48pIZVMx3GUi13fOvvinGPBJIF4F%2BC2SuVRAiEA5j4DkZwqKLv8yKRPeaXZiuYdD8ssRdy88sZT5HsD9CoqiAQI9v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAhcaJZ4pcgmjYDtAyrcA5CN9EvOmzSrj8Ydp91x%2FZZbJo%2Fs49Fe7%2BO%2Fvc4iNOrPjWoiRrpRWqHjeo1NN4J8ctedG9IrMZzygzT53s5hL1sZswPsCOMiaSzRFm%2F2B9KJ4%2FHUBGt9gtHugqulQQf%2Big0ufn33GjbF2ReQoX282WG4zxzadlVCudoXMtoCaWIdpIeUKmZiffc%2BpB1vvUpPdlaHriKUNXeTkKMQxlxE3ra9zdI4uKj1a3pAa%2FUKnfPO2FiOOXDEawQJ71SFg1bWmjIGMG9w9tSTW2FBBo6ABm0hb24PLJouwEH8r3PDigRFd4mKpGniDrm5rJm%2BGlV61GnPxVUBJ8cw1j%2BP2dYT9J9y07G4zyYWVg2I66TmZP8AYCFQPqwywa64RwrwZs%2FRzNt0%2BkceGsOT0NGQLxNq3Z2%2FkRh9NMhNtFb14ZzKmmO%2FJvGfh4zTAg0WXukgD%2FamTDiXHdjghAwZ3WSjn4MsQvsWzAWKxK8zbLF66N8wPmunilw%2BEwIwrOEdcFJMUS7xM7i0EGpSB6sslJ0wObMnoTlS%2BS5C6UGvRXHcq2AWpd8h49QvqHSo0JKnLJuQXBCh7WW8DBCuJ380A4Z%2FzgPk%2BWxY7Csd5NuOB5skm4kzRLlAPRXWbDt8p2P1Z18sMKbd9NcGOqUBaqJoDzDegmEXQznYkGSRu0L9CJ1jCnUcIoqN%2Bci%2B2UwbQmQ4%2Fnidhq1ck4eu3ALB56HjSkf6ffWWlt0ZwxPIZoj8st8%2BD%2BHefJHAPU8oBGzYzECwkbUgQgjG3QhPFl2GNr5kUUEjQjXZJiRwEH4016Pa35WrpRvdL74QZ%2F39Qeu69vV7LFmmcLBnAQQTMpUpDEgk7e0xbc8YKlnpcEdkHxjKBhWO&X-Amz-Signature=6a1a3f287754881c16a9cd80cab8e48de815c50bcdba806890cc7f47f3ca96db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W5MI5B7K%2F20261118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261118T050544Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCIG0jeluhVcIPp9F6ZX6UjB9%2BxlkHnYRN5kLNfVhbkB8nAiEAyYtR8RGHqbc4va9orLAgriSPtlCXiFHzIqI0mxgXSvsqiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJNLHirGiPH%2BQALCFSrcA5TtoR84%2FX7KMLUA%2B%2FZW%2B%2FFbSMA6%2B27S68xK%2BMcLKS8VfjEwyM%2FSAJmQCOKNce7ymNS4rFzMBODiZLVgJ%2FHJbNwSEyO3RGV3NtAVNDevz1%2BKnAQiy4g1RaJPqw%2FSnM1%2BB0Pl2b3O8k%2FIJNEHFXQQSMVeLj%2BdOEyR0FTtndp2zsuvtEBom8oyRUtMUTSaq567am9sSr%2Brdd8lkvt1OOca6i%2BxmrArsGlUZ%2FJ6LIbaj15ySV3M5BsXbGpuqfhW27dLjZvvwcair79y6hUU7OuFoXsHjwyWMjkV5YUukk261qCT1dCZYwhbonQrVJRontSyAC5NV46ecuHnOCNKWcq6vC%2BuFDRxn%2BFMI50ic5jzpTzUky2h5cF1sVLBzyuYOL7JFSWnLLnDv8b7V7%2BQCbXkkh2n6Rq6GZDl59oLAMM6dDO5ugWiCXvtaa6Y9Fl7BEKVLq3gDcfECGVKCaVQspZAxBG9aQ9KYH3UZpABLYXtGgAXfsN4HHZ0%2BNxk8kla8cMEnv2zDS3D39Gl4kNoy%2F06m4vIJc9wUrHRG7skQfJTm81GusCm%2B%2BVm02m1P4dvb29bea7AvC2j0XPzLmKYd3qQhE%2BR9BULR1WQROTuJn0PX1wvaQSCNYvRK1RGMguvMN3d9NcGOqUBS%2FQOSZTU%2BMeaBbcKZ4c0ZB5z8XfMVps97TTGfM2tkYM%2FeBiFcSawjtNiDt5Ou4fbCCUZ%2ByvHTu8DJSRaihVOJ21Boi6gUWgmKEW%2BA%2FNE0NrB0MyeajYZm3T3CZR5c6RnATgyE6Nr93kFGI5tSCo4ATvWwtZfYzO9UWWCEIecZCFkTxRoELtV5CrHTBx6L0aTdGKfWnTojPVx4PtV400LVRsLLzha&X-Amz-Signature=833db1c68342c7d00618469200c84c8bb92d467bc260df2897ab7e4d1b712829&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RHFIPPHE%2F20261118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261118T050546Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJGMEQCIDF9zkcPsytUsPOe%2Bg5nveuVC9IfK4I7CIPEn2U8EJz6AiBPXVm4LmvSqvMxsnaK3WcuxUqZAYqguWrJ9470E9cuMSqIBAj2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMv6e%2Bo2fkdFHeqkZFKtwDJJlAAGCyBHcml3hU18Pnq6raYT%2FXd0AQp28iN8FspbFY2G7jpc1gtXH3LOFbTc0K1fMeEx%2Bv9vxaqmR4%2F6pUWp3nLeOsoaRVirQf0dv0h%2FjwJCZdlSUFZIx13h1Piciuu5FUzbPMyN%2FP%2FpQXCsR2wa9OI3jzg%2FVCiH81vwlRJACOx3Fl1mwbPK45rwGzkkrq7J95rszA6FqPwebyHGMz4rkm%2BzFpzjfxNYAjdLJFQoDl2UctIkWjfEGnIfKI7pBSaQvPLeumKyfquY0N67lSAvOrIUOF513QTyEjHCaXE%2BWbekXAb3Qz6azk6m1EYMlwcb%2FLKEb6BOT%2BxvltIJ7VmTDu%2FwrqyO2q1%2BR7xd3G725SecFVWDkcs6X8EiK9TdxrRtSkfvXSMPujEK0NdQGdxBJEQRpIQ7K7GVNtWTSFt7khoUuMenyLEN2o2JLfx8w3i3a2IT6mUx6DkiIAFU72x0rcv8zPHiukT7kLBodD15cgkgr2h3h1oc0m2CCxMZwkKREIOCih0CwnZdTk3LMMAlcRwbSFieUhCHcnOEXVUu4ndVScz%2BVv0%2BB6dgMyZ%2FKasctp2rXH2dvkJMuvVJGqz21Xeo1v0Pz3QtEq93driBgYlwAMvveh2Phw5P4wsd301wY6pgFRm0SIB9oJpK0CR8Pv4Y8mmco%2BBhT8oSFhkFjtLmgGdtk0UCwSW5LeJ%2BuRrLqh97X9g1zaj52jtY%2Fvv8L344szL0b68z93Yqix9F%2FJhVNZrvdA64E7Z%2B8oGj%2BlhMHPCaWx%2F67rwiSjG3HnCRVJAP0NsW9zf0BJJKUFxy1yDpChQdF%2Fd2nlLqCKj5s0gHP4ArqWaNDtZcS0d8c7XRMncWAKAjQG1zVF&X-Amz-Signature=7a3fd456187c948db211f72a5019cbbaf43c5f527545d42cd567b3e8caace662&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
