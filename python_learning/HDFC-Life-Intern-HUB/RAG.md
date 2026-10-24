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
fetched_at: '2026-10-24T02:28:45.120Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666HSYV2RL%2F20261024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261024T022841Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFWwuE35BLga2xdFUUlnheghSLtDZTPJobFZT6PSVa9QAiEAsiJMO4OhhOrCqYbrombSxEMOparOdjCQa7YJBN%2F4QF8qiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEphiVn1HXQJfjpjnSrcA9cBK9YtddbHyECaRxux28o1jRtrAabViluOAz%2BDwfIvP0xtTeIdNHQgFIeJJhCXNoxfwUeqarpHLRfgi1I32gNjxTlPeYQa5HyBTW7zoVF9xa900UnKoMs9b7FYNDVuxDBp9FROjPzThSk%2BXHhWuFHtJ6PkBalVFRabrqLCRvEdgvMUNrHBujte2OSMpBsu0R2u%2F8x97C%2FPMvY7hJZN2KpIE3oOSdYC3y%2BKKIwWg%2FmL1nxdN63e8c0u%2FvT9xRLGZPlyYJnGJibpPVCxoLPcFdQjsUIvhbJnULBkUsi0A5HOjbYwnYZQjIel%2BxduMjF3pJXaz4pKjPF%2BKTYQMOs31dwaXUXYcNjC7TgOAgZY9iXGeFN6hf2XLnWhICcZXubNRxQkOBRHOTj3yS4LSjPja5BiLc9xPRCJYWdRN2nZ%2B7Fdb%2BLutTFrboTlfxES%2BngYNzMaetwrAuyEX%2BWUl8ikSTcz1QkEBEBc5t1kUWaT5qDWxZnCcFoqDy%2FYd4srluE4TMO0GSNqijsvkD7wuJrW3VThC46D0Bkpk9jWyPYEBecYko2qqyiZtdJl5I9qseZuBGoX3DeLjwd9tubCWmompZ7Zt7t4pbnckBzxZdkd1aQXdtKqd%2Bj4dNKofsgZMIet8NYGOqUBEnT5GWxQjtpZnAtu16AMb1zqwv1AhtVLdE6cbGysTinWqezN3dW1k41WcTekRciIf38u5pGhIm%2FTQWCuSmyvfF8cjhojE2a9ufijq%2FNiJubLAgj7G6NntQHqguRrbUMUAy%2Fzvl1E%2FuZsRdxevDV3hvter5ct%2FzkP2iTNRAPRrIcMIvrIw1ZaR%2BqO5g34fR6gTI19eDDJk3g5vb9mEJ%2B%2FnLzYTHfF&X-Amz-Signature=94801f3265503604083d6cd36654ece84330e076a9610136362768467164a2c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWCVT5L3%2F20261024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261024T022841Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFawfZeBl3B3%2BO0xUd6akv2DfgjnUrWETZNVeZp6wZ%2BWAiEAnh6%2B8z8tiCqu%2BZ52oqESFiBYSlFIT2qblO3c%2Be78iygqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLaXg4%2F9vEJPinTiTircA%2FrE9rg73R3fEayX1XrWmgzxdO8GHmcGb5D%2F%2FJ2kUA%2F%2FOF03G4%2Bi4yYh8iCNzgEzYZxGRaJjLG6mImSB6L1gG%2FwVbSl%2FqCBfyzgcSHfqK3UXhd%2BEjgRbychq0S8QkNVfO97HGHNvzW8ZNac3qeKJgE8koDXIJuX%2FaIBjPSrE%2BdKES4SpbwSAy0YDFR6Wpl7rQ42mx%2FAsYwuMtif1nRNt5bD0b%2BoeV59E0hWUTm%2BDS6xPMLn%2F8Z7jrIhdRsOJFaWNbAAyR7I8GvYwGXa9Ksdf4LR2FCBS7%2FTCtpt%2F%2BiJYVIYQ4L3dfuY%2BdR5Wo6LpQZPUdQDj%2F9rTy5e9GgMUJLI3gFIkDLDMIUwxszZaU9oYXm5jkXdenARuMNwlUjl3NRC6CplxB%2FQRKT0fkdYggvSEL0WqYhzfvd9BlIHs68ull8Sg6zxyl22aC71fH5BC%2FHQ8BlhhVmCb9dK0N%2BHpXBwsRso4Az6a7Rxlc%2BQOHT0ErRZ00ZhEmk9DgjqFo7KnwF4WCxMYtAMIzlV%2FJ8f%2FdxVPOjzG%2B%2BbwgcS2a8%2BxNr%2F6UtTxT7FDHzr%2BvMbWtHlmtRK4lVKUy40z8Je9Er7C0EhOBdiA6VBq%2F6TZO3t2ClsA1062%2Bp0XYL8OO4Pntv50MPWu8NYGOqUBLFP6%2BGMpjYQVBKqsHfIAPdu4ofe%2FN43eqdRr7cKekEYJRSQCq90rRqUWIuXQK%2BtrIceMpHPpZSS5XhAMmXSUzHS7%2BFMyW%2FgRKXsc%2Fgic3vvUgczqyhQjAcRtr9qXKGNcoixqRlUcqOoopm5mX2gIYWxXoZvbnBV6SL5S1TlhjVHcHyB9pwej1yKf8OFMo7XuBwzVrXlF%2F0A4g27auLPCpDt8oDHj&X-Amz-Signature=b819f11572a58dd8336250fd7ec5b68c580ed225f24e20f9131cecab1f410205&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666II2RJUX%2F20261024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261024T022842Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB3mA0K2SApz129km1Tz258Xu3a4XJegNN9y5wkwm%2FWsAiEAhCuCYxgt6CuFw6%2FnQ1QPnexi8%2F7aNo51fSBfeXx%2F2ggqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAwldA4CNWS%2FmL%2BGVCrcA6vhRharGEWv2Z73laYgb%2FwAhjzzflwGiRJvi7b8L48he4Th2Q%2FyL4AjTWAMJ3nVgMpJ3RIc%2FUzuLVYoX0Dtho8c3SLWYwdBczqMeJu9JeA9OaAqFB4V1MtEflBi%2FWsFxk%2BwmM9Y5qH9q%2FUr6YDyEdimkT1Uw%2B4ovUdR%2Fx771xY22yIPT6Zna9HsWZNZzksxiWln9rZmjDuskZdcduSuCvcmi7oJarpC50kgI0K%2B4fjRlSut3aD1PdzM4gKI%2FjD5PrGZPSctnMP5g3Tz4ZMEcyqpf%2BoCbYcMpd4Yzi4HtwqO6dteRHzFoaytd00QJBSyHosaLRQ3clzATHZ0qPkE2V%2FkatUp0purhn%2Fa05yTGnd56CWGHEFOevzJHpqGXHThRsuw2xa6WrZ4h%2Fkm1md2daqiBltHKRYLShCxKfq1%2B2OCikYB7zb6AA3%2FhlZGSeikldiToD0XpgFoQHHMY4enwFd00ND0I6JJsj%2FHjxCGEJ0I7IP3d1m9Zr07gBf4IKc5Sek4JaW5H2iimZgc17ZQo%2BRJR9JidpN7vCcyngyKBcfO0eVuMvfGVjDF637em582k7yfYq8k5LsBdZDABGm%2Bps1IGdSoBdAc97g5piT5rI8dlfLCLJg7ZU5ua9haMIGv8NYGOqUBVDZkQK5xP0XSAypjskV9OldsYFnIHR3dPYKKd8RqJ2GK7sWxcjfwolotJ7BF%2BayGO3l7Ztki5xO%2FURVeC2%2FSRjVcwAtEXmt2DeYpNrEu5mqXa49SjV%2F1bq7ldK57PgtTUlPoHk5%2Fv3DS0raImOhe3Qphf9iXKgI3YWbAiov5Lw%2FgcJWunjsFGAYY29QTTk9WEvgjOf%2BHsIC7W%2FRqCY3ndAGt3bdt&X-Amz-Signature=c15a03df8d575d4d14e0d8d15d8ad0c475d3897ed2de99986b4b8c6b3ea93abe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
