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
fetched_at: '2026-09-24T02:11:04.258Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W24EMWWL%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJGMEQCICU7n6adL%2FiI7gqnqQZzb8G6pbttQMZdTZtGILzJGBO7AiARN%2Fp8i1s8IGHkZ9ZVZpaG4fw3%2Btd7xkv8d2%2Bpblhg1iqIBAjJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaKWfLqvFf8yLNh7RKtwDX2KnoMHRNdKDHbUILny2CB7c%2FwZhlNf6BkNeaFs%2FPnWfiCPHlaRCQLMZf50cKWbgHdo26BlSLv6hXoqHW07oWP7l2t3ohi3DL3DS8LEO2JwereJ1Ncd%2B%2Bk7mcg3c79fRdB%2FgfZH6p6WHXSb7ucyxfoRlMNg0sz5Up22gUxnqRqqynkFxKmBt4PsC%2BUmpizTex27609Gx926bYjthzani8Uq2GKvWB0RVMGXqfIoTjxA3EHDUBF6BaeEQUEPyV89WI4ISs6evu%2BaBgFcYYQwRV2wv%2Fza33ErCK6zCtuYfQCQFpG9RryEpu2TlhQBuE%2BqqA48c3qdldjt%2BL4v4nG%2B6GWRZ8a4CRX5033y%2F1gyB6mWDYSkG%2FEuz2aUinWRlAib2g22teGzH58qVH%2FaD6Mxvp0bUCS3yipPdYhkvMK%2BMHGoJk1h8RQoygPbza1rURPgyEWqKofgK56ibMnCA9P8ATyWzbA6vNix30jkUzGlyoLX60bDpKkxDxJtk9VONRCT7%2BOkAL5ozUd7BNCw1d4akTLE%2F0SbDkS%2BBA9fSe%2F9fwNIGrtyjtZjXEhms9UKCmEVkBIbkL4MEtHUkjP5eDa1dZvIu%2FdRV2rjcYOeVRK8dza2ziviDKpYXzuZlKmAwu8%2FR1QY6pgHuc3hxsll6WE%2Bw%2FcHR3NpN0DdfHw61MoTmwTykeyFGOyyXDk7Mis2%2FrS1yOEa6hdrotFtAOxXSLEBW%2BmVK4ksr%2FJHHd%2BxAVl92tibYeZwowdmyV7akiwSuM4NxzfzF3GJ0BDV5IrOHv3ZBVkUuEuqDLAzKskfxJo6JxNk2QFKfumdk37Clx7D%2F57MN8BOChNStaxKidVi%2B0m2v%2FOEfsRvkrGl8nhks&X-Amz-Signature=6ec1e51c1d99575c2d8fc1838559f44149cddc50ef519b2a3b93f3a68a288890&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46633YQNLAR%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIDubUbJfc3vyujE%2BZmxXhCNQocXDQ92HjJ4oJpN%2BaXPeAiEA4rysoqBHo94j09P%2B2r3rrwh0LCJZ0hlkKrTuhaCSbE8qiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBvebdMrUlC8StBrUCrcA0gjk8FfkvU9eJqrB5V080qiIW0eX%2BQBzxFBQpHBNuQ%2BSy1OsK%2Ft%2FasnYtmmvY6G1oXZuwIHiQ26wrsE7tCCGXMYWweWPd8r8rWJLVDzYrVgY7xDJG0Gv4iEj%2Fll0KP41vTi8OouK1GSmITyJrCAsRRA0RXDNeFmlyQ8SOSNPIOrt7dq50ESPyJFO6Z8azgywWapOcWQLGpC6TJWrYcIRwJ0UHXmIvptb4Za4MCCGnsP4vN8Tcby3nBbqb0JyWsmvFqMmnwo4%2B2GCoIhzpOBWhkEUQ53YKB3Hm2EhYkM8alE%2BNK8oGFnN1KP7XnUEalEYLt1%2BHiyoBgCFni58edydjHE3zs78fKLtAChF3sIpWeDlf4Tifnyc4XLZFqr2qm1uAqiM%2F5vKLWrEdT0YJSKGt6LtvmD3OAfMV7x4c1jDzYpuKjQtqPLsSTTQ6DijKP3n4NAPjEAgVnkbuJS7H40q1uWRBa9QSjzGlHm0vFxZ33Lo0A6ImzegucGUXKXMm5pREQIGHdsIOzQUN%2FqENvXjmnKGqkjBfs7iYgpsdr%2FYHABk0bw0aS%2Fj9CwCschqSSxB73H27smO7Bn2uwBrbVJOpB9Zy5Lx9Jm6ZwBTIa5AID4JMlOAw4JMb%2F%2BbV3YMM7x0dUGOqUBFv7kVLtM3Tt%2By0fum4Akk0zJRbQuGc7tKENo5%2BopyGgKqF0xh3XlJaqkpK3xoWY8ZgkWLeNkmYlYMPCIgpZ4YtVfCU726mV8N8PU2%2FQpdV%2FkQVjM3uJkt9cN9lFvTHhoHp0KCbmluZGO3ifDeP1hkEGLv9LztrpFi0euQ3NUX1ZldKH3uVkikFTcGMpUaWKPb66Zu%2Bu9pXYrx%2F%2FQGCt05RNFHz1R&X-Amz-Signature=b350158bfd060a40bd05360a6765f2d317900c4eb8037dcd7f4eed7361decc2e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJRID4B5%2F20260924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260924T021100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDJ66Rbv%2F8CBKJERjPHG54TFZ%2F9XXbriqpSl%2B4cRNs2mgIhAJbVk7y3L2gSlRRHh%2F9MNpdnJ3kSC6bo1cMrxgbECrEWKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwhyKwb4Hu0V0b7sKIq3AOaPovSh4Noqa7FcXD0jEqS%2FndHv54RzjTmXykVm9%2BuRqdDDTfsHFley5mph1%2FDD9RZZ3herGLOe7MfribOayfAkXhuZrQtfQdji3ikZSNLC%2FHesOQ7tlRQwsTwXmGJpu7myDET%2B5r4utS5U7Cn8B8kCW7LXrGwB%2FDVqEA%2BFCi3HwE%2FHZBIjeWoiuwDljVCGRSvMocDt2tyKCwDqrSKHQaoNfhyAEuDqqRlZYUJzoO1S0sbsd0%2BoIVlCsIPPZ1Yf2JOJTzwmTsV4wUGqPdjYxTiv3xH9nguJTbseC%2FRYyq0jVW02cfouNasi3VeWKch8Ae9Kdsg%2BkShvIzFLOS9uxFle3bE%2FQgneRwaqQBLRFivxqoNTwxCPKLUkceMC6GwgGCrcAujkJNX6JtPjkW4AF7sWn%2B8jEQGOgecQ%2F50I9cMCmqwfJzmSH0D3ATWtMM6ptTrqk4CBdA%2Bvp5ZkS5MtGsf9owORUYLc0jJWW29D6ipuBiqBUJst4Ck6Vg5ixneQzctfsJGYZGf4NQO5bfMSeOzy0zEAdYRJmJc8oKRHAYsDyCc%2F6zRvRRBvXi9gTiBVETme4RNCnm5k8lP7ZMoZKGtnc8JG8b84qje4YDiAMnC%2BIdDUDhu3kHCKb5PfzC379HVBjqkAZtVixL4zONBNxw6G4Jo7elqsrcKlNHuYHSMhcaULkVfM3C6c4lz0Ahi1ovb62VDAm%2BKpcQjV6lR3WVRogzlckv%2F%2B8kvJQErqQ3HVfQY2xKlNoclK1UyHT%2FfKsz08l3R7A9a70yZuDlQa49gRxjMRB01khszDEkk4sBaTzYEER%2F4yCIuy1uiNgOMB1IEfm03QcPtoh%2B22MggO7LQ0nyLiykXvq3a&X-Amz-Signature=0556e744d77523003578ac8ca1607ff7c6a48a829fd0279ab1dbcdb901e41c48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
