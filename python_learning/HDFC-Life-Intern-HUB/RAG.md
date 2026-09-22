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
fetched_at: '2026-09-22T02:22:57.416Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VTLZ45M%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIChiVCzzJL7zYSL2ryItjmLeAbv5OivuLM0ELaaGlelwAiBXvmJVCyjL3mFiR84dvd2dW9TJuqBAyynVHhG1FDHMOiqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDysyddRKmX6apjl7KtwD4qy3D0vN%2BvCuZDCSMSSWAeUFcYyC593Y67YXIUNzfrXk2IGHXyVMuaI4VAYl98LqiszeQhM8I%2BlLfCQ1SwCuCGBzCxE2%2FHpRI4pXKPppXASYWZrwI6egGAg2AKjQY6qoAslA7K4nZHv1iUjee2s1E4UnxO4MYgt0NbJyi9onLVdOWogRv9hXHimNxzlnNzPmoG6Bygl%2B3QLgJ9eyU0ElzRaA9R8eUhNiOMoV0jdPIw2lMYG%2BZZWqobov2xHiSkSerV1p8pusezbDVSQiZqySY3%2BCpkMX0fJGOO5WyJ5Xk2ZWv0Nu2t9Vu0NAx5qf67IyH3hP7TnaMHSuhWj0fcpainoRU48CkjkF0phww1Jt3C0ecD4l9dE7CKaPgyRWfjpX7NW7SJZYzYBrpZAlmCAyTyu9i5XpDMwcukyH451YX6XeiffFDnr7QKOg20BAHV0s5AH8YaS5E61AnKNydRRzYyvrGpBZoMymNC80wYLrVsASknM3WmTNN9f0bUxQ6GVM4LMlAfXKKt8%2FziBkv%2BF0TjNWSayMteYLX3CT3dOV3SdIQVpC3egJJ8wlga%2ByYHlIDEqA%2BgElMDRzrmXsxJb1s8apb%2FqtRw%2FXrGx9MH2yHSiCbPX4DWyuLbgCeSIw6JrH1QY6pgFvfk6tEXI44p5ipkr2nDcebPx2ozACR5lq4%2F6epYjt8ziBz7Yt13E7oDdylwNJaRWe%2FM4FHTMdvYeg3vyY7K6BEgSJiq%2FEavad6BLgp7cLr14T5pw6DJXH1k%2BRtjVe3CA%2Beebl2L2mgAQ9%2FTZYAjMP2HDiveQ9VxJ4cgVM6lSWXDpmsL%2FvsARQectjkORxt7KzFoqOkOH5V%2FKsTyBzLw33mxvUe9IY&X-Amz-Signature=c7f7ca01655a2730c1e9a36d404aabaea8b23299f1745ef1a62e27769535e436&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YXBNBBN%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022252Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFBJFxOT5pw0F%2BF59oRaL6Gry6R3A4dosPS4sNegkKlPAiAbVE5DgydK3mkDhTArp0OcjcP56oI1ZV72TuyxvewhXCqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMc%2B1NYxVQeImNrSETKtwDmVIs%2FWs1pv%2FW7yDZ2P7oJ9p%2B67IxB6XOg5aMGG83umlFV90n%2BhbersdBs2kGrWduSG8nGVNRi2yjuOtCO%2B2fGUZ0h5OIv0Y2ertPpq0acH5FxgUNZl1MKASDflRnPhIieggi50bWzbNRHDQj%2Bc3ZJ%2F7dNrb22cXZG7U7XvZH3EGIwdJ221WtQFKzka5fUBP685X6F8M0kR7tpihlG7rAlFxuFXiWAUQ%2Fr7snp4CAcqe0EjhX4alZvqspHuYt7VSQzYBKoNu9x6aTd3NHgYI1bWYVXRogXja24uPSEBKUaS2WLdyPARQsJQ9f%2FWWx9oIh0WjslnzhYe%2BKGatjwJprGdiK13GJOaBQjA3%2FuGg0bGgCQ24oZpriQuBjEgdbeurjtPzAnBODAmJ1xtfIcmvzFsq2K0DZXJ%2FWioV5xPlz%2Bs8qTsPsOTfl5RqIgTYjwk2TOdJCjU4vSFJbV%2BrGFQuutvfZ26VrvZvm2eGEkhn5pwg4U5MMy33F6jmQMlT98xtjfTxvtVrsuYrRj6ryeTy9lE7y6q8D5KHGcRBK5rFB4jBtTWAJ1gozqnRhZRxY4w3bmzTcrjT9jjmG%2FZaxLR4XvQnV3yRQxIAV9nChQiL4y90CzSPYKmW%2F6ebZzPEwl5nH1QY6pgEwbd%2FDazlAivOLYzeOnhL10gj7m%2BMrbu6iIWatP6tblQdGfpvIlTVKeCXZCkZaTZ0L%2FC8sFfte6gPJIT0wr7QH4MKgjdl6uy22LqagNsPMrqAOhtXaDWlU1FkSuFCnbb4GTv29oxLCdF%2B3lzuUBYpnUVsw1hwt84mDB8rBVzZpcKYQkF2%2F5JixHj4%2FX5RVE53G%2Bqv0PjTeH86eoUMTSoOLZk7RkiTr&X-Amz-Signature=cc45e308585bc0f5f1317d413bbff07789da9f813d3a43f28161d81965f35181&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667346MR3K%2F20260922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260922T022254Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH%2Fy9xXQHmHrxbUphf7ejuurJJcTHhGtF%2Bjtsv5nu6%2BYAiAd6lW8mYRNQhCUlz8L96%2FhqMgrSRSGWcmrBH6FlpBDQSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMzevkwS%2FZXPCrMZxTKtwDOdXx%2FcEG6Yz9YcKybcfW2kOiW45lqpe4SXf2k3TweixUC3qvye06XSgGMRIM5KvbJRmgpZ7OjksHeXsqoyMuMsE2Yenxsgu9kRCnpxQ6%2FgAx7ahGHlO9HHh8se3G5WUuGt8WjnbfAmaIpqJ87OUCDQ24yJ4UW03eNVwkp1zAjMFDNOZwK1Hc3EGP18g0U28s3%2FAVZzzRTdp%2FqhvGlCWnktsLjbGWeDyCl%2FUWiIW7BRvXlAVLxExfklqa%2Fa%2FkKmXsirdI5b7OD02PbRXlb49B9Vpf1z6h%2BKMK7aNWaMdLaZEhTSG8nARGtPSW6NRAg%2FXuCIggD9Szns8a1fSleZYE6f5YYqjyv5gkK%2FQc7u3tbKdhIvp7%2FithsLhLS4nturHEKEQNMu%2B76upUQ95eYVwmm7qnYhBvSrFCcGqpCF2Jtqi8L6tWSj4%2BCH%2BbOOoaIQiv4U65ZGC84XLKYMpzKY7Lbd2oLcVMgziW7rttcyYClU9jcE%2BjYlDeKvcZbxYUvAY6tW%2Bd6%2BanjDU0sijkP922Xc1VDatAHvIj7ssHmkx5rARokE7oYu%2FEJIvTPoC3F%2BU7X3fu%2B9680Ww3EzwnRL68VvnqZYuraKvc1a4OYw%2FBUupQbohAntygXujP6hUw6JrH1QY6pgHPGAzAuWAV%2BPiQ2VbYcHfC6K4xn5E1P4OsSgipvsiDzfsSprYXhEXJ41hKSHmNwcHDMtPuK85sGP2vSAR93HYX2q9Fx571CQ%2FAm366m9ZU1d9yCR7V1QwlsZBFK58Vvz%2BA%2BgyffxYL0K0%2BOfV0%2FINpkHKKcdmQTBKX0Q%2Baaf9Z%2BcZN3JWnEcsRY4ddHBP6cmREO41dkXYdfqpTAuL6Ta46SvaBm6nN&X-Amz-Signature=e2022d3fd0740ad843bf3b1a90a2809e440e33614fd84306fc162198338731c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
