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
fetched_at: '2026-11-21T04:45:31.850Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662FCVLRZH%2F20261121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261121T044527Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJGMEQCIGvdWHJRPnfPOP%2BEhRfCvWovzu9%2BAfif2Ogls8ICuBBMAiB2HJq%2F97rrbvnUh7QV87GRgq5OhumPQLr15efmyHOGdCr%2FAwg7EAAaDDYzNzQyMzE4MzgwNSIMn47Ywk9zWXfQlNo7KtwDHehmOO%2BwX3NyM0Ivepz4uILVc4Pb9r8vLyesC9tWYxPw588AzosTwww9MKjZITM48cFDmlf6T7yCWSU5i1YCMJNe%2BZAk5nNm0UQA1tKYFExsP3C067R3SJ%2B12SJP%2BhTtjncJBFiAwOVwe%2FVGitirSpUGNF%2Ff7rCnbkwBY%2F3kiRAfWBbQUzqMupHi5Q3vNi%2F9fkRf0CFdeNElK988W1ZioxSicmqmeygnAdNIOdMgj15SMiH4gkIzqfvto0%2F%2FGyQq4%2B8kzQrmm0OyzayjpzlF6MfjDoOiiR5YRtonGvv8YLqd2ItzcPr9qOErWUIlRX8pr9xPaoOnA46R1E4lHZoreNniboAd%2F6K8wvbQtmBptDqzcDEZecojYnNwgUJs%2B%2FYJ%2B6oUgaRk8ssC4xxqSFZMjP24hleJlr8twXIOO99XHEgItxC6ezwwqrDD%2F7aAzJfCnSMDX%2Fqd1me3R7Nyq4zlOPEFzFz4g1T0xxiLxqE4ZaqWVOQPHhYokq%2FXgk6LPVj7OekeqsO%2BS%2B3VfyP3tl7VZT3oXPXsjDrkfHUuJuhMgAySX%2FwsXsRXEgRBM6QX1vmcF9dGKlQicS%2BB49ZO6kLJsM6h6h3pzEOIYnjOdtx%2BPhbv%2BPx61Bb%2BD0HgS4cwmI6E2AY6pgEksAJTwK4Tdy3sS0yhPAZxtN6J7OZfQWQPyP4WvjTEZxrYhIsosKErZ6UxiTLcV5xlqqTJo%2BP5K80CQRz11q1Kv%2F0BpHr4nH4ZoZesQCmIJzY8IWMqZbvkMM8guCmM6qMKCL8nYERXKFuDWKf9Ka1cNC%2BgVYJOh1RmVCQmR5UHUhV%2F%2FGO6KVVdIVaM%2FbNiBRrP8lm06RqGoyD05pgR2dGEcKvRRj4T&X-Amz-Signature=945f363d344a3bc73e4e310cd3514a9f9ad7c2b282cb6639c909797fe5d7328f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664GVJ4V7E%2F20261121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261121T044527Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJIMEYCIQD1u%2B7bsttde2ZcpWGiT03EZ9XK67nek7sEqVvP%2F9zdzAIhAPvP5P%2BgbiIvTLdfx2%2BAWy6oSbRO9oLeDZ1LPEogs5Z%2BKv8DCDwQABoMNjM3NDIzMTgzODA1IgykBMzlMP47ZrmTi5wq3AMXETOxoAYpnRkE%2F2e%2BUy4ZjwZ0sKRdGz2izhoAVjNOgtkUQns7rJqH4SHNUXuF%2BktUuaa4D%2FBt5sfQT18WeQVYXQKauPsXxzCXPTEXupBCM9S2QBUU0qxBg7fJBx3w7POhSCyERR5vnlEXixhcobu%2FYOwo27KFEtri0KuYGRmMJRZIujDPz2ez1Z45P8uLcCH2T532fsZyVMMXy0CSOhaIVc8sCKvqmo5jO2JgSTBoIy9FyhroTOLhpRAQld4g5OF8eZOFQAcBRPvyIugapoieicTR2pQIVjECgktyQ6cboTuGHqVzulPxNnW2KVQvvQpWDMj8%2Bs%2BMPYnGOzRUf8PhGYTRTMJVxUvr4nND7LdXBiWpVlF5qeLAFFEKJVseeSGOi8ZotJagQj4t8qs%2BiHAkQsj8qH2COuF%2B8h3a0FBomXeqOkMUcDE2n6Mc7YDnjey6hkd1RoAWHnTdu647KBY20mO0s66ENaK5jQFZD83LkliQ%2BQ98vWqnCBdy%2FGuIQwtfv74Il3fI75pkv8vRMCn%2B1INx2cuiTMEgazIMb7SS9Pyw%2Bta2UZ%2BXqCLMa4oUrzUy7Urq1okxXVVzWe3Vj6rrJhe%2Fjr2Ds5vKbnKZYaHz5jdeAv0d72IVNH44wjCDkITYBjqkAYiqzIUlg7%2B88L1H0OBJQ8woldH%2Fi7KBWhdKa6%2F85xJ5wXGJMAfGQhhrqbkVrfi139s7tGAUInm7vOG%2FqOZfjmDo7DojRvQy3DpJGj64MhUQeEagAAmTW1ExcEZyzwnWKq8YsyX3iAzPCS7t95sKHELH5OBuH6mNwERDq7pzKtiONyP8OY81FcB4YnoKjXRBZ0g09G1ovDlv%2BpsgDQ6l0z%2BIGCDT&X-Amz-Signature=8c0438a55ded734b38dfac14c42d6a089015bd696aa6e3f5baa99ab8598da038&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7H7SOHR%2F20261121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261121T044528Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJIMEYCIQDI5yG9%2BYU22RhN6V2pfxjTzN4a07yIo6gO0NVJhU0jywIhAPPVWR4gr8dS%2FZpzeTPFWO%2Fh1DWTkr1OIN9ECIG8LpRVKv8DCDsQABoMNjM3NDIzMTgzODA1IgynU3BljVKZHhJhI24q3APq%2BZcSHZHgHihe0dHeULT9caWYEOBMalG%2Fzl1yfLF7zS4OmjokZcvuZSKYlYrZo%2FR%2Fb1J1x9eSBxjgLmb5o7Q25r8qhKfhxDzqhJxFBLWi4Ci4Qiu689Lois8g0tkbJ3%2B%2BBxDmQJokPK5mL0p7euerhbC26U%2FMEm60cSia88ivRrmKKHVn%2FuxIZqw%2FNat51egucB5lVbdz6oWHvTpre64cPDpzXZjVNxiyDK9XgztDpF4m635VnrgLTFPJALzxsiylEl5CLl3pueQjowKy31iwHb66MX8R4dBctR2YOYTjOtJBKhtq5eeAALEr0ICgwZrVIbkqlB55qMyf3PhpEcMMggw1S5fXodDlO3jXWADE4RJmt%2BE16ga9wEwjRzpcmjnKGfE4JUy3xVb2gtS3oqW33z5w5lkzMpg9w15SR890LSyYB1F%2FEFKlwZ0c50HbdUyir9cYHMlyBXioKqa3xXf2nXk2bQqiy48DJ8VeTWEchgqxi8PEW5E0T1ZEViIr3Fbgly3WXQNTb%2BfOHzNPLbVTbySQADRRh%2F2wkjbUzBz7Llp1EtDULX4pGqEVtQTf7ItsOcoWWEUr50aphSewjF%2BU3FEUUfuMrA%2B3iRzdEckhhejXBtOpCEZeFLNySzD1j4TYBjqkAVDGtkSqqa6pedPsKgIhZzBm8azl6W9CXaYr9qCc%2Bb9Gwp%2Fs5lX1UtyuQKtwkJFiguWRWy%2FydqInhex6CDtS1t4Oe5sLXeyNpiIoq8P0VbA%2Bb5yzKr6%2BqNroZMMEwI52HCwVdxRR00rQDH8xtozk97a1YOcL5oo%2BtfWlLdMsafFDEltt4iWQNZl%2FaRWxYkSnwQz7oJ3RR7ZlYo6fUH0xR4uQNo8F&X-Amz-Signature=2152785e263c5e5b71baa9c80b0a36b6ce832fbfdbfa5d80d2833097b0b443b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
