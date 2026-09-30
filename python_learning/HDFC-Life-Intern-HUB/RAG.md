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
fetched_at: '2026-09-30T02:57:37.439Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WDFHBYOM%2F20260930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260930T025734Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCk35E6YajC1obS%2FBYK10JVQmkoHMBzd5B%2BVfmB2x%2BE%2FQIgI%2Bq8v535kxz0TY1h62lp%2FZm0p7dO0GRg1P2PR9yEby8q%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDNcL4G3ZVYI3ys71DSrcA1hoQozYT%2FtPnkGn7P3hVIihuFOakU8HmooXzxGF62d6mwNxOdetPAMABc6gVP8ZJXRQFHay9zomFqBg20pI4TOM8fT8MLEQcjAEruuC52aQuVG8fWQGw3wUk8Vt0PU6GARMsT%2Fwf5yWHbhk4is6XiWrns%2Bi1ZhY8jGg53ipb%2FJAQmSaOLi%2FN8Q%2BMQIUfSu4O2O27Ze1TkT1CfW6n9PZXiU4PPfwDubD9DIEVMS3%2FxkKBwaBFG%2FjrhBuIrqZ9NJpBqhD6sqy7MVevyB4zMmrJ8bsrcoP%2BIcT5Z1Tjgqvde9rrgN4HLl9Xiw5waHr0JJSj9l2t%2BTWE1sW87Ud1iAB6GAlefiswk0NumMdpFN8y2VMytBBaj%2Fr3ivH7yNO%2B3HgcZ37lMwQqFA7wrP1lb6beYLTqMaQCEtAcehfVvhgmF5anHh%2BSA8GZyoKdtSuOwUIWXx%2FrBaYpf7Z5Bln%2FYU21rJxJb0JRRSqdMdqylUxKN%2B6gb29wsPT5JK%2FKfuCWgvc9BHnrhkH8dsa4bCdttga1nET258027tmJbU7re734RZnNuZ6BGzMqE115GG44crDtks6Vhjpy7lz9hx3dXwMawNiqrZXZtOQOVJDVtI9Wmw57Va%2FMpxQhvbZE4vPML3S8dUGOqUB5GG8VceGuQ4RBSp341vQZ%2Fl3tWLMOOQtX%2BdGicBda2RqIAx10DvqqKVvjtrGb07iXAIASK9lKpL9vwKzue9Dpi8%2FYbzedC9A1hTxkYJsJAnCqEYwgaO3Ld6wwfO4spL8cXKh1Nv4lxQ3nc1VyLowPKrXanGbgBOmo8tPZ6AMmiO2AmWrfyKlhz5A2ntMh%2BY8mgIGw8LwwKZACu6RlENWaUh8vv4u&X-Amz-Signature=bbb5609588dfebe16d03fd607b2c21b298419c368b344e820933dd37930d0209&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667XNJHV5M%2F20260930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260930T025734Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFlP9FaTM%2Bno00W%2FUXuYvOKHNf2MaiSpZ4TBEVBA4HYUAiBD%2FfmLr9j3j8nZ%2FEuNsrQHOpgWukwngy737Uwia%2BM0zir%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMfwV1QXhd4GkYFZOAKtwDRvTOovw0zije4Rb7r5QrphOiPXBk21RUcZ7xJA28UojnprWI2CpLWpeQY8y07w4rNLR5mrzoLncyWBMjEVVuhSaSY3Cn%2BHxUycYdN2qu1NMQu4QWer8lDdizugG1eE52gLNUWTfe9tZt8HrSy7tDcbvy7p%2BYZ9%2BeYVmYJJtu9iML42JSGcnpKQQdU%2FgAhfIq70drgShVhk8rH%2B7%2BFLBfZdpz1lI5RNJ28HDZKMgBL1ysaM9a7gEADBcg9nuYya96XL0lmX6bl00KuZOFcmA%2Fgjf4m1C4DnKCM2Dlg2j1%2BzFCdjVt0MoFUM7yMPPuCh8zaF1OQwBJYEKvkh9Wv3gGQNWyvDAumtIu%2FyMoIAutb2udqiI4Zf3pTiY1uUHhl%2BuyQiX75AT0DoSt3pK9sOCxIMAPYVF%2BCPOVFGOKgB2TmF66A3FqXdUY6nRf197I9yfpH%2FP1XNWJCjJ9gubZ%2FxNmFDSWosQmRU3VS0EuABuVDA0n0ChNDrCc6a5db1qdf0xG86mU22HVoUoJYocxJqkLSHZTLoj3buhhP8GCQgpA9%2F8WC0pjke7t2wh9t6vEjb23W7mk0hfuRJ5GxGSyRtRhkL4FErL1xqhsRwf1KvFqX6Dpoa9zE6u17r0E2J4wj9rx1QY6pgFbOBVNLrh%2Baaqr4FqHvwWWjPB0m281P%2FSKIELV2%2BVcJmfjj3M%2FctIJpYjGY5JWRcz1T0aYM7pLSlhplslOpDC3XT%2Fs4he68nY%2FHTdtjGzb%2B4bV4jNbzKZw2%2F0U3RkBgNaCrcLW%2FirHrGG5j90jJwdhYd8986SgbtA%2FNG19S9MwrvtAquW%2B8J77draKqxVZ79%2B%2Bjkt4lcmuxDNR4gdflv4c4VSrc0k0&X-Amz-Signature=ee45aa09223222ba0aa401e4f5be3f50eb893a36ed4d725f46891909967179fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666MYBHHFN%2F20260930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260930T025735Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIACnN5fSPLG%2F7vHmMfIT9O4uF6A2eTUDvcuyJC8p5lTQAiEA07BsZb%2BM8MMN6WAlS%2BQR9KKQkxVxrx%2F0qYaJs8uTRAgq%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDBygFheez9Ls8OzqOircA5zGHXsmVPTT6X5Cesm3iL%2Bijpze1cMwdmAC6Fn2%2FmMYCHbg%2BBlClbTzwVp7GqvmOADnINSWBcRJG7gpzKr0wPa3VKdRecMq6mluGljAR4f44fQEb8mtPgHmdiphsyfWFs6NprVm%2FVONeGGGvZ7xg9lxujPkYG7brfWJc0E2tFfRhklTL3UtNhSHnGitBMp5%2BDdVqdeeg%2BQIPW%2FlTrjsP3LxAMeY4n4uNf1GUkK0ScIhS5JBdvVViPOxX%2BkzaAl4YAmPtGElZGFgCXdCtuWUWLAtniX3LvFw0ABEUpptLx80kncJd699oX5%2Fe3%2B28ho%2B7VPVhmuk4bWgVQGgOADFlPZA8VAg5Ex1%2B1FZk9%2BXt7rr1BnqTV67lNzE8P3%2Bnc6ZJKxabOWMFi%2BsJ%2BK63ssBMH20N%2BPpyYFl4SZGvW81bFb7ad1qH%2BpheFauGfJu3VXKiNvh16KzhuzxFKUHikIIEJkCarWK5WueIOzzhoH5vglWnLu4YzYA%2F4NRH47sxhfTg%2Bb8RH6jwfixrnaLbvDJG5TvUHM8y9DG1czG9wa%2BUxJXW5yG2HUd213oDB5z18qnigimTHZ%2BDjjJHZKkEJnM0xRT7NkvqBJiDdHFASNKSfKOCZbbAurRXMMDeR3tML7S8dUGOqUBK1Tfni3fPwb8aFuAA4fdR6Yb4sk0SnEkNpQJs9YUqkLx81cXIJ1WRIKpI%2F0jydzL6KWrm124QP%2FWp3n7V3MWwjXdfZ9wHmUVs26%2Bgkq435UXXF2RHYlbomaasm8qVyNZeMJykzNMkqR2bWVqCvSfs14Tl1Ct7ZX1Lf%2F7vPOVe8fplC%2BRL0wjmtie1q9ye5HIqo%2BdWlr2gibcQfU4Qxl4H%2BjBgxXO&X-Amz-Signature=3bac4118edef95e778bc1f99840a5604fe67fd53bd69d83de6b76d9bb26c8339&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
