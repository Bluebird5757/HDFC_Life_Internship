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
fetched_at: '2026-10-04T03:22:38.184Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665YTJ2XT6%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032234Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEWeaaI5iv2V1LEeq4iG9y%2FTKUfGMwzWPrv1LniYE02dAiATaKFIR7T6qVdbC9L%2FiWGNT3zKSBufTXAquGMv%2FKgZ%2BCqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpMVNjAUBb10rDznBKtwDRF%2BqFR9BsN6ZP5A5ILa7VfCjbROncTF9aXDX6ajGmLLj%2FmhvgjWjFIX%2F3NkNhEk8zSWpAzlvah4u0iIKlojLUs8EWPoiFfQ9pumZLnbfgGV59LwsMgvvgiqMXFaXXl0LBUyodU%2B7eBOZhOBwv8C2OEPIZU2h%2FkgmdvIhY27Arxk6tK7zm%2BFwxYKWGlCaY8hjswTfjoJytimEYW4O8MVypao5yro8L8p6jiBSbSlZDAVucRLwYNmHINFymV1vIgRmHihVP1zec5FFp3QzIqzf%2BQCJM3isIq1maG0cFyp5EbYovBkTAbAwPqRj01ZHXIuj4Je9Bl4H945zR3IIDOqHdLSB%2FOEUosbQL8qjJHPiXz9LJTKJLh6mzPjvIHR5Yzl%2FmIHC%2FGZyPsghHTzpeT7q9Dru%2BDj9qCLQotAl1vUBHSTzTJ5Ynol6qZX1K3ALAixFYSlkdEmwQVqYrAL2IV671N5vc0394sSwHJ%2BCSYNMlsKFDA4BMS8UlL%2BELAZUetEMeTWL6X1L2xA%2FXTPK5SNVsTogSj%2F%2BUg4eTf5HvlarTa%2BbhlBFvGQ8DOGGGjOsxmmepBE2GVWxu6%2Fioqxadd1MsAgRRlvCshb7crViw5zgRsQHUD5NmpVVhwjuFIIwvoKH1gY6pgGZIN9FrQpDx%2Fk9itFap76dBt4BJpLhoxaPYVVoit4Ii13LpDSXxelQSyhiYfS2vLNyk5l7WQSD3NwMNavL26gCl5Zt4SYg90eg9g0AHW74osibu4XMT3OyS9%2BFTOD6DD6p32GLMpPmPS006LuhA7P0zvtXOsQHEEOhofw54Fa%2FGoAk6%2BmpFMFDHPVMaeqBEzEANfm%2FN4ZqUoovifcZ7vjt%2Fj72SpTo&X-Amz-Signature=7612536e02082a7510f5799713d62d0e07f1e40438af9fd0cc9bcfb2c5a68487&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UUGVHL4S%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032234Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDxj1gfgYAc0%2BJEyk0hiwijbpSv2xW2H5ORl3f%2BaPE0cAiBjNkJtZ1F4RCciI7MQdSXQhB4rDLV4vZ8twyd8Bht3zSqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMemkFFQxPQynrvwKcKtwDyTTo2zN3LzIqoZcGtAfSALpXK3MfhguQXRUC2rZFLZVBgDsR3M%2BBJi2xvX%2FXbYA7DtBahK66jarb0n9EN31cS%2BrzXHnhwYLpvSmfFdeDihV7ZBnOmn5mIehABg%2BIjFFVSYbom2sIvZVO%2FeUHXoWimHmL2oKjYRanpCl5kxaNcH%2Ft1%2FyNbEddDauUClGtToAoiJzv5Q3FzI5pv1wRXgW6zXhBnx%2BM4RMkTIwYHbjz10di6Q5yHfPTqsfX%2BcJZrcl%2BpkBAnPnMfBRMTAGqDuLmDfMGJ%2FtPSG8sJ4LC9VsMJxjXwVw%2Be8OPQHrK%2FBjdGEh3S9boRym8hbOVHJUSlo7tUB8IpWwW4w7xHotAsY93gK08k11b6YtEJ7GsYEVzTGmDwP3DyodMWs8MoNOWfrpSDciQ4UWnx9l8gTcKaoFnySJOKS0BEi2hL1GA07T2%2FGRSYHzp8WiCyOhbnUNb74d3CwFmlpWzlyu5BcPcREhR2nlTfpOEFzynmxYycM3V0oGBUInXVzuMZLrrfQUEiTKwyEb8g7h2pnUVMc9WVl7%2F%2FZGzpQJm37GxTIg%2BRT1TVfCz8iaqTXJs9YdDnUCACT861IoRVjS21hBDh8Z9lhSQwb%2BJEHikNJXo3oIj95wwwISH1gY6pgHsoLNaXOEWWyqecsz9HwUCi3M51W669wwNT%2B%2B3DcZDRZQt07k8cIkMYnTTNzZZKWW6YVaIdb3FIfVjUpkonaE8wxskNGFJsnjB2pr4VpxJe5vOZO9i7K4Q2sfq0U3TedOXq3VB2VgCobOKX7smK04IvcATMMSiJl75bwk%2Bn2%2B38uMWn60%2FEOrLXFXoB1%2FlZOgHrVflHiwfvLth4xDG2yZzz9b7SlF1&X-Amz-Signature=791e08cc7b17e6522723cd118d752eb736c2a89b92e3066dacb15c92bfb952cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMASSFUO%2F20261004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261004T032235Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDymMoAWPxe7OylWppv%2BgfhAmEsjAUsh1WAnWqhL%2BxYzQIhALz1d8To44O5BPEk9B1tcgHGrc7waYqD%2BMtedcGJkVgIKogECLv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxb8EuIXbPeFrRHsVMq3APrp4GMTvVkOfgFmiJJZVOqxjFDe3kBiKNY8UyKGFWL5SOJBxsR%2Bt9HuArghAyjhvl9o195A7EBDZfX%2FWaZr7rdX4%2FHiqKuFXB%2FN02cqT%2BBKoMvuXTG3UbtaNTFNleo8zb5fjcC%2FeMEnERXF%2BUIhqmuqrWGQnluKfzAf%2FePFDJ4zzkxT1cHo5p9p9qv%2FnyookvjGgmhh33ADmgXexSm%2BrIRF%2BVnrQduC6S1w%2BuDl7ig2h4inK0sYu2NTf48jIy2NUG6phVxu0Jgg1khyE6Erd4iialxXpTJ3eo1RG848LTV8Cdxc3OCAXgo%2FwF2fLvLKLPAs11O1%2BS0wcdOK%2FY1OGxc05Fb7nWAzlH1wZMPwMDqYu2PDwCnkXDnfwP2HuHVdhP0PjW8XfEuYCAJEtcjD1gkl6kyy5z1gIvukqT%2Fm0AcXAhOQEdm0gxTJwzIFwYhYJnYBkHp7i6PMFSoMZRV8PWGYZSUqDq3WurKpvhKBVOq26xYejm0BI%2FvGQGRwj%2F8dqPZtydVUCu8vpu88z3BbkK7g9BDR4xMvp2z%2FMc%2FGtTNKF%2FdV0SQAoqP30M9EmoS8%2BSflORNwaztKUB48JhwkyEOajgHsZMb6ZJNaCnL7T3pKkMhNBqO4IYoTorMuTD5gofWBjqkAbu0fFF8%2BuSIfOZpE4OXBeY3MSagvoGq%2FoWnvWMs2r7Dc8UEQPQmzlnidmsQpOuOh15fNU8mLBjLnoi%2F7E8RacaGmZLS%2Bx%2B4qcNh4Pkocjs91Q%2B5yuGqdrDNDoNALNpiR7FcEdIh9hIeURXCPgkLrL49yJx5lSpMwvNp8clqjQkL64NrpAxPmAjY9nhs%2B%2BBy%2BvH5rTOcJlFY2BrhvdAc5E19Fa8U&X-Amz-Signature=c1960300581b48ef1acbc6ecc67c46df62890b994afe366e71290b8029117e5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
