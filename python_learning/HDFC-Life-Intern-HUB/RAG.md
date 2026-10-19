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
fetched_at: '2026-10-19T02:22:00.313Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T5ZI5KWN%2F20261019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261019T022156Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIBgABU8aDos%2BaGJwxz0xHWZMssTy2d%2FBeAH9O2dB8pYWAiEA2S%2BacMn7gzZnVRyEkv8pDdvC6CboseRX0onP1MCSd08q%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDIEokCetlvxPvkd3jCrcA4vb02%2FK28cPegwtfZDCk48%2FyehSRXrP2Jl9Fvv%2FknjaE3vmOIm2movWtY4iixK3iQqkwqWV0%2FNwUO6UmCczzaINtvXwp9HgFPuxtaBxgwc2IjWKryu3tib6rl6aOPYA%2FdlTjIHGeMbfgmGI0SvZnH88eDi3b47XvIZvTizVjOxcdOddNcZUZswPfTKPHrp9rGuKrtzzUw7w0usRSyNmoXEGN6ObEtfpmIuj3kh99910CL55jDOVhpP0bxefVnGfrBEg4gredUmvtv5WrwIm3jZG9sgXKrshboUkj3s3VODdxGmvud04Ftqcf0%2F8aQgv8hakVvHLf8H0scmLWKWux4kWqT6iN0bI0HRSdJU8xqROGGpWOLxMb7kvBCB%2FhI4cb12rnGRBXTTnkIwekfV40OcDEj5Wh%2FUWYQgDTDgSCobHHDq%2F8YDGNHTe6PLuxtTalC7aX0JQcVjPFdLZgV1piZuzcRnzr679BVsqtirirr707SdqrGcLt%2FYASz1Vf7SMF2KddqkNQ8m%2BrmCtLtVO7WMLud%2F2sXma9OTnnjJBRlWPrjwaGxK0TwpW4bFkJq71uXMLOEP%2F3vkQ8U5eEkG8w6wilM6mYXLAM72C1ULdUKRZwHQZh8u5IGp3GJnZMIrt1dYGOqUBhkWTRxMttvcIAzge%2BKVbPTN8jMFRS1BO%2BKS9UGuNCx90%2FXp9IZ9zIDp4shZGjQOAT4ahGgUNiL3c7RK5xNQNbHUZAgs3c92bTQrhCHOVIOQ8kf%2F5ZbCHuv%2FWa6aSa2GUYTXfl24EujIL5HxygjORh23qPECzo6Eg7xFfxqAl1dP4NxsgoLsumtMA27%2FyxYKF%2BXW%2BqSvauxyR256RCg36eMNFtLlK&X-Amz-Signature=f079beac4d8006d495ea27085cc784f49727cd00cc0142e7ea4fc5afb8b2839c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZGNNLJBH%2F20261019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261019T022155Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJIMEYCIQDq5nojSXdWEe3FvYCERqdlG1ER1mZ67I%2Bo1QUJ8DkgHwIhAJ2FURQqylLXHw2vWZqF3fp2sxkkQSRSmrw6C8mptFeSKv8DCCIQABoMNjM3NDIzMTgzODA1IgzuSuIoTiRF%2FIIu8I4q3ANOcMBpTz5KRT4acsQS51c2MeSL2DhKgVR1ZVZvyGe%2BVVVgce7ap6xpQmUsVmKtGru7yQMEPA8vy4yGJqjT2BGHFY%2BR5Am1sB%2Bsnx0eA5Vpo%2BOXyzcWz8loeecbri%2B5ZNBP5awTUYh6Vxjt%2BTRH8%2B%2BRbqpTyyKX1PYjPP4QULyYGwaeODMOGMtHkbZomnA1tMm39%2FBdmU%2F2ABXelhXBmmYGXRRZfHLMtCn8VumDD7KbLYQR3WPHXC%2B7XEstX87o%2BtOD%2F9sI3Baln3FWdH6sNxdpCAzpg1LBYaqk5vPHKwWGe72xJ5jtqgt7EeMk0wr4mPpPMhWzd8HWUw1TPRXUqua%2BRJldQAXo10DZq2Ycojk43KKcPjJ6WeMrZVB%2FTzhYQwrr2p6Lox2pi2XBPEHc65dnswHRfGYbApQagvMAeoWnwhrVHQMqsSNQDZd%2Fe%2Fkb8Dp2ANa3z03fYsUU%2Fa78%2F3JA5SedkbvckdwQPDLuh%2BSDrU3W%2BYv38x4k5cWXRQzEPolcfT6Rz%2FXi7Hk6PZltsIKwAERG3XvKeSQmgHsBxnctVT4iqCX87U2DkgtNvaOL1f62h80SKSqy5MbeEEFHXNTPnwwc6Okg5P7IwuJU2qC2D33mvrdRBvb%2F%2Bmdz%2BzDH7dXWBjqkAWCmXPVBvWoOzM%2Fa6Wg7e25c%2BU6cMoJy1xLvEnMZDaWCUR8M8pmEoWw8hTunxz%2FPZReD2cKEcrwdrhcp2oewrAJJ8qCgaHMa6z7qjmvZ0xijAefJkqdo%2BmzMcRacw4GFzyrmA0oyu23FVkIEMuirR4KxN6lcYNrSyqQnYiiQmzxL%2BMXCj98kyyollPEfiHD0yl%2BQh28ztikwuLx7R7KHEt86xM5J&X-Amz-Signature=4d0cb6c859b2f859a0584d6b70c67baae7e1c6114b48e5670c4aeb586650b65f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z3AG4FLC%2F20261019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261019T022156Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIQCUDF3KF8krpPST4Iu749cKmOPZavJoIMG9HtIXS3PArgIgaCt8fDmaJlKhoiBhUN9RFg0HtQ1Puz%2FmmdQQTvU1csMq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDCXBvPQiC13Q3DlSbCrcA%2FgRaDrT%2Bgg6wpS9f7nk1x8sAJvCIx5MjVqRQUOZHqUNIV%2BkqudD6w0xuWuQEniu43pjkBJImF3%2BTBf0jCD0hP37ux1nv5McO3kEucO9dJtln%2Fx4lhUyEWfewkAlLia96MmFoFwjuuLdb37LIeJjwiu4Dhe%2FDxXg3xGi8Jgnt4qvOoELFFl%2FpJ4jp1JPFLQlHcMOL0K%2BURsm%2FscSnzpoSq1z%2BBhwXdNUqG3EPPNVmeD8%2FCC4Hi9ppU0TkGAGvX4CwHZQez%2BsJmV4SCtO3W0AtvqH727LWb%2F%2BmUy8teb%2Fnhtof2HE%2FHR6MJXY22w4QmT3yVY7qnNRz91WGc4EUS20Wf5hSsAnXB38WWXfCZ7wyygk7MEB9C9ADuRqcx%2BeM3fnfYxEhcYeaPzCD7eoFeeNnQotB7NFJAPSO3zC3Z2CdDyIYudaK3Q5WdmpDtkZtDU%2B4SY2zqHCAQ0azlxwxiHUy%2FRwiKVWJ1dNK%2BHv%2FaLt7xkb2otdKmVTWhwL4qGCGyNJw%2F4gYJVANYD%2BESYYe8Ogbc3HbIJuNGeqQm%2BPmt0%2FfXK4KMRyr0cYfj%2B3li%2BezOwDn1TCRGlIoid%2Ffgj6G%2F%2BApOn3qCDWV1DmYeU2B6nhj4l7JGgrES%2BTxKMFnd%2BcMNft1dYGOqUBuQrkCfyq4%2ByNYc16RpJxYYkTJV7G4NBN1U2pQDILpLqIdloYfIRj%2B1Z6bnkzLqJAfnEKtMAvSRWJIYWx%2B1cbK%2F8v1esHjNd3a0B2RUhJLXq1EWeY8EHLhQh9kDsQlcmuqcmuQm2vgztZkbIbrAa1k1uPEWZcWloHuKqDSw0O863lh8ZLupVYhBXZ%2FUjnKWhPH3RbnCA56QVzvx%2B4P%2FxLEPLX9wg%2B&X-Amz-Signature=ec74150a4e82fe0c6fa8b11f82ee6d33a21c50c7f6148710021b35dc2f73a7aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
