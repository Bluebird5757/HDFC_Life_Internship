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
fetched_at: '2026-11-03T04:32:58.003Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JMK4QIG%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043254Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDhRiQps8ZajQViNg%2BSQsrVVMoOJ93xw1KrOszqx3VrlwIhAKS%2B3PPaiyxmIcmmBD146WJmvFjGVs4z9byhwcp0%2F7TzKogECIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwvYiM8Shj5gH1xkq8q3ANkN4%2Fdvm7u131Q6IbBsmtl59PwCtQ7awPhzwf5shv0jZzzNcZroWYUxMwgWU8JousYnYf9HUHdKSZ7eV9kSbhoqGzd21YHZ49QbjA4R1qOrLAlyCwZmYxRt2VKVGsaCTw0QlNT0jLhWjcyibOY0YqRGtOcp4kBgHTQI29o%2Bid%2Fl%2FeuWsrNiWwgxJgGAox8qYC72DaHFwgQhHtoXurlL16hI0%2F4BHglAvzFxtgT%2BCefsV90eIIHtKGjN7YhyOetgO6EG%2Ffam0dHWXT6ns5ivCb%2BatU3fG4kj8iNPDYwHBkeR5BX9RsMGy0%2F6x%2FcVuRD7wxAMUTlgs5v7bd%2BWB6qXGND6H8%2FO7jv%2FtfW237wZB1UttpdmUzY2H6EPJmQIlLxNKXrOMHBsUeMux4UCZlH%2FwpqfdcnwvLsRD3tuiD4DPpHtQQJIGgaCUNoUaMl83Tf1P6%2Bn53H3QtkBfblm5CmKyUyYyy8xFCBwIkIzsh9kWr%2BGQtHwmzx%2F4SBO6%2BbgCJYvC6PaQGHRBW4bp55ZOcaM5uI3pwXfNTX6KQ8oM262tcRiqpEyR7cEre5i8zDV6GbFCYY3%2BabDDis94MvAUnru4Rj7SLQXy0fICQ8mxWgDURZTzF1EkIiT%2BgnIrCXszDElaXXBjqkAbZtYhdkyjPH%2B4tkyjqHJqCwCvd%2FCLxxNjhFJnlr3%2FF4oDM4pI%2FbrpRCHOAMlzz3cxS61Sb8C9aE8AZ29FYwPypci67SrWmE3Dhjg2JhyGpkLDbUFrmEd3O4PqEtvw0hy%2B9dJ5A%2BQ8J7DHa3AoiR2asWS111JIuDJVV2a1fnBdpUMD3QfVvwbamPjjq9C2c%2FOswd016KgYoMD1SA9%2Fr22VR1zjAc&X-Amz-Signature=cbe8defd34187fbf3b670c352d49fb8010ac54a5afe4c9986c16a9bfcfd8749b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSULCAUL%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043254Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIQDTOJhSo7Nd7yLQngAL%2BABI1AL6fY7l6mouvYGiNZkuVgIfAV74AMtVQ%2BY4WWC6inb6DXaLig7TMDf592I5pppRKSqIBAiL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsKJFY%2FsQe6FVhRgZKtwDiBda1BVYfKbA5DyeMClWdKtfbgjUHd3rWagR8hFEFX2DM00eV7exhEJqMe7BO4xrRu4DvwA0NqGKpUZKHR8kdCsHY1anKVjqUomLJnht49WLy3XOhUL9nefEofn1ctQddDMQBLYhf5EEZJDI1w7cZ2gSS1xHvDIdgrz%2BkfeqBfrqdE91vpEnWO%2BVtYXHhNBCRMxmx%2FQPvH8JrxUjpm3x90kMq%2BX7eV8M%2F0QQPsk5%2BVaaXOJ3f%2F29Xm8nVgKKY1BdKUYEOQxytAlsIuIYX75aGnfx%2FpvgePbQgxbvkk6udeKBacGwbg38DSl6c5JIE2SPcObMx6zEdN42sNXCM9AXNOiwkKci58bR%2FyV0AtQdfo46JuEwqWDghT%2FV7kjZ%2B7Ze3CXXIlnh33ckFq8pMNWiE5rVT%2Fo7s3ANt4Kal%2FyOb0pX0xW2BMem%2FhqnC5VrxSfPjTtKRm0aA6ZecKHM6FVj0jQT%2BHr528SIarpoLf0O9Y9dpazIUPwUjpQQN2il9%2BMwQ%2BCY4G1mG4%2BJolf3RyB88XgGSjFaGqAhjtTKvKEcE8X0WFEO7GgrCHM%2Fj0Gc9fkjaotcokrFgjHkHMxVWAMl5IqhPq05Afmbvpw3ZnHQu61Ot3PW4nwCx1YMszoww5Wl1wY6pgHQp9%2BAo0oQsOxn6ijDy%2FsFPYeFNGBw3JtxqgBfgppXdAlF%2BeJ7YBR5nJJHixA%2F54j%2Bn%2B8u8l16%2F7s70pq%2BPS2yHbaX8TnYxRDCXNlphIQTqg1Fyh8AxEVKjZTxWYKhPHe59WAbv%2FiZYRzcvJyusxATmTme%2FsLXgIp7BPFtfpxTArIK43vORPfPEak7ZRWquCsLEBnHBpF5zVMa4urCL8ubAQ8%2FSo%2B8&X-Amz-Signature=ce7c243d50e1062ad617bf6adea80f78c9786e3905e163c2fb68d6a88cd1bf35&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMKIMU23%2F20261103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261103T043255Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCuLaUKBW6OQGaPHYSjAZgpD%2FH2JSOXnVsWJ3KGaWDh%2FgIhANB%2FXI0KpAA4ojsuN7wCHRnPPdmRQZuxTWQMPQizU0Z5KogECIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxR6M3IirsnHdLPLFgq3APrceC5xldB4ksz1Kin%2BjfgVNXxnyGpFaw4vRAoWRH1m4ysm47JgKkk4ThA0tDUyZNOZvptMlB%2B3vP8LzymqIFWE3RCgKNRJ8ztBvkQFO6BMIKQgdWno26KKOd2kCMClr74LECFC8sSIit%2Fa%2B9MOtzyaT5fEWc%2BwaWsFPeGdafOxLOqE2H8R2VcV%2FgvROXm%2Fk3mp%2FAHGVSRdr%2BFNi5bQnpikItPxJmOwKLyZhuD0ua5UB%2Fe36OC9iLgxK5u5NOznlgABAYLhjVNoppPUilmpRqKCZqYumQGyAL%2FcEBcPqBuR5tMaBbmJ3ruoipOprXM%2BT1nI0tBsrbLGUJB1P2Nw0AhUhayTzhehKM4P%2BM0hxI3ahoHFauX5c73zR8JF%2BB2TAWa6KfntDz0smywKGxPvLfxyq9a88Vm0gpbSyJ12MQww5elgpuIuFTNxzz18uCbg2sV9Nd7JUt4Uj2GV0yvX90kRXhk8FYBXGHH0%2Fsmx%2F6xpCSsNoBfpJCmXN1fFirNNPKujXjrn1SCe4Tm74ZhsGUhXexQc2IbVbRpOmNJ35axUuXwqtrSlEiXA9rGRObPzKlTqpyG08ZaOlurppJ3niuMkqu%2FqNow0Xp4D4TErodlcrFeAKuobLg1TOonIzDOlaXXBjqkATF3NE9YrhjOhLOfv%2BTa8DTJAJDSkffHqMW2hRdGH8%2BvA3mlhq9Vqvm%2BabGNsfQgkdeBYegt0PGMEvGgG8rrBNsq30boYmHNIGJgbgD2XNWcsM%2BS0oqDojDvaBz%2Bay5NzGtmkAgt35l10gMLb2z7SVUuc017MbC%2FcEDmrNUvJDAxd2pcXA8S4rI%2Bq00HRFhq5LDR6KO%2FoIYItJzZEtc7vpPnIgrc&X-Amz-Signature=fe45724f6ebd064f61074e419d5a279da8c3500eda5e4feef31b0c9c74561e48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
