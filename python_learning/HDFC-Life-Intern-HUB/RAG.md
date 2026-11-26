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
fetched_at: '2026-11-26T05:55:48.245Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666QUZ2SPT%2F20261126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261126T055544Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAfpYeA5dWPZJTSY%2BieQ5pdtJmd75f5Y1m0ZPe4Ypk2UAiEA2puyyWyjX3n2saJduH2ZUNQP7MHmWuwm3FNR3r%2Fkiv8qiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLfpIgZypaMENU1QhCrcA71DlgyQsoRaN%2BFucgCGYc3B%2Bu4Jw3yH%2FnsdmC3h%2BFfb8XEUM8CUjJrcfcBX45YpYWNS0xOimx78fcIgwqEis%2FR%2BeHlmW56jmqG55LX%2FwWVtl5kihnYNPFT0ePvMn3mqSUNEZQ%2BJP1YZR3HH2c4Px57MDf9uWA0LGAYGO3cRPstCd8I9ULui%2FoXi0qO0P3FjD2w2Y38Tg1ahDoumF4mssmh2wywcMv3c1%2FFmDx4iolZbEEUOA21e66RcdYuxC8OZF2FIcmOMFvle2d1XSsy2gfszZIFxhi4bRuLGDyqkWz2vf%2Fp3ewbvZO2OvBqGEJL1A9jZ8bnASExy2HzVaocSmUpW%2Fr6z9CYYbgsPe5jCF4NXqH1u5u67gWpnqDJuipaaXuCG6OmIJD0n8JFZwns%2FPDHDdKSALkLAaSJR9FNj2wFr%2F%2FJM5YwMplRIUYC3iXhFeAy73WYx4s2aQnt%2BrxPlid9JNcX96LpWf9hpx44rWd3GyD4nZzdGop5%2FTwX7NyDcyOFUr925RrlV1kynJ%2ByzngfRbvSODPaMOiH5mukcvlqQP%2Fa3ZrGppp2lgp50w0ixc6wedyKq28mtBJtiy%2F3Q%2FRhIgkeoVaO8Z3AJ872j3nzC2JFv0RbmePIYFG78MK3IntgGOqUBy5ucyLo%2FDEvn29KsKZJ5WPvAOZNIqdCMJv3jTSQ3HVMi1UCdw%2BzXfhNl5n4wjw8aXa1gsvqj%2FzfqP%2FGCwP3jv%2FUOg8rCRbUOf05j2Ss5zzZOTYC5SxhEJV8%2BHCCJCG7T5uRvjIwwvmif7AxKrfLIVmNHtxcuQSaDIzjG5VYsSE1t1ZJtv0oNUK5L1nfVwQM7V1l43nuzEmrLvAJQqQQup8dcIADr&X-Amz-Signature=80f3a3cd4f8f37295146677ce496f4ddadb76caf61701961cc646b5f369d5757&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WEPQ4E5%2F20261126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261126T055544Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC8slWc%2B7TM5RFgbYYUojBayrbwpsoS2VbaNBkk1XZbgAiBUTYs1847S1Hx1vwRGe%2BJu9UdRkmxY4W1gvKbpVni%2B8CqIBAi0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMu2hrk4GfU4m4QDRqKtwDKbI03nblWeEjYJhxD9KJc4PNTI4kKpKmXK%2Bjmcc095FPVoGW1idT2oadNaAU7nO2hMUkIaNpv2dx6NbbrqZohpdAxaEaazYg3cD1zJUwE0NedqWU8UMtI9qmZYdxzChwaLHM6MdGdYZqwC0HxKy%2FNNqF4M05igRBWkzBI%2Fn6GkzDnMyGy8yS7NqFKyF1zguNKWPYW6BkehQwMahGoNVbXhZoOQeMB8wMIWfTh5%2BA6TNWqiYUuPfN%2F%2BUgUlRsWjomJwG%2BU%2BCywrDH2FNFdtHd0T1m%2BvGGH%2FNSqGc%2FoydZ1hMAo40ErgdGvYWaOjXQ0JMZKAXL9Ajm9c6EK0GbTgkPnUu%2Bk0ziaufYKCGJq7khBH9b%2BqNjyVGuns2bprbqMHf6lf%2BxGLQQYfbr5XPIhf%2FVVj4teoP5TYiicnrc00PExMF53QYj9IbDGykCFrHWTOSuS%2FKWrHlk2wr9lZUG3lg0XptBBdjOKk1bsF5FWlNtPPKOvudry%2B%2BPJFYlAEpc7I16224gkQ%2BtTKFMcEamoNKCpFSGJ%2Bf5zwIim4%2FHUE0HRgEgsW%2BoPvzCsGIEla4RPwHlAxZXwzDQuF%2B227S3VBqzreJKdiEgEg2NJtvgQsw55GRqP0c6TSoEZBZT6kkwvcae2AY6pgEo0XsVdGm1dMIhuhBb66yML9nxo3QnktZp58WmF7afsKpDnnRHwa0xuDmhpH32MD%2FvjQm6%2FBVB3IHQG3PBvXPLI2gr%2FysMF9I7q7L5%2BalQCd7lJ7UqeY5%2FRzBZSBBri9c6BhtDeqVe0BW2P3Ik%2FxbAsKt5oC3SadHZXxdVyye%2FwSAvrotrwKZNuce0OqY7eWlNXPMFBUJKVD3HMp9MDR76HmSI9HzH&X-Amz-Signature=8f8e7a49d636e762d2238b20e9453b1ea5fbb771e7566183add7fdada1d13a12&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VLK7NBI2%2F20261126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261126T055545Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCW0ced4pUQPLcgmt5vpBYLxzA3rP2XgLej9IUIs0GphwIhAIP7xNvWQ7v2zndt1V5IcNXBihqj1v8PCrs9vg5yC%2BHPKogECLT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwC5XJcQfsQyJk9iZQq3AMSVexWvHbkpNqPKeVXSW17%2BN%2BkzTFutPgjt%2F4wwKbdygb2TUPkAyR4Xs7t8JWMm%2BhVDDSUGtq0u%2BmhuHHSerMR%2FSHPGEKFaa%2FgDgZbWZXyQZXikVAqL4YTiDcQKl%2BSHeHi%2FeLFo5JrM1JVp93rimlkgWRPQD1tmf%2BYtIf6vs%2Bz2ZSpXtM4GSSTtvK8snSyrkChcPkZJFAyPxQgnaEHG3JSZByLHTD9RvWiw8uGe0Y0bOl78cpAjBjktfkEzEPjKUcNiJRPR8qwlrddJzwpYMfgnO7zkun9kwFBUOa78z5Vb%2BMaLSVIvt3FJ2adEyWqrl0D%2Fy2GNocLlJfkZXSjMxwJEBshC3%2FsfaMz27oeXpiXHU8UHdLZZAnYSMtP4Ykbg4e2GsE6lL5YexSCoIIY7VQc%2BTDBoSZhAI4RfZLxkIuV6HfqwJwSl57SOFLUsN2Fwjm0j6iVyBj%2B6QcrTll50FiWvn9ci08hv10QuoupJ6S91AkG9toQVvgPjmBcLLweOJeLA%2Fw0wHiKiEbwhPwlPkhWeFwm7eNC62Alm2dfESR10qxD6jfSK4md%2BuXaZSLN3qh2cxhbm2IhANaVOdPfIDuWVP0CSVMzz29niSedrdnsiGgFWegXNLXDdrD9XDC6xp7YBjqkAZjnV8pCe1VlNLYraffisAu8euXNLkkWeFhFxwoo6YrSU8JrKnJQpkPZZLe2fGGkqH%2FmTgLAV%2FU%2FHETJCEsCmNSgiMOLj7d49wjTlJXtCN7AwgZT6vY%2B4%2BdZMncIyOAzvfcmY9a3U%2FgP2XHZyCYZbMH5D43EDqYxOdQgvl6ezbw%2BIxbzREMhxN8Xp1fYRrDmHNvvT9m113braCuk6TI1BQzCwsfH&X-Amz-Signature=7d7fae20f9229572c2d28ec313c732c29737956d3b1ddd10222d9454afb4b8ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
