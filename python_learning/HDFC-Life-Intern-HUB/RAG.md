---
notion_id: 3c84fa76-9938-8047-b507-c8020ca82aed
notion_url: https://app.notion.com/p/RAG-3c84fa7699388047b507c8020ca82aed
title: RAG
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-30T08:00:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-09-12T02:06:38.747Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYWKSKBR%2F20260912%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260912T020632Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD9ZqqCeA%2FWEAPgccCHh7sKUfSlUC81mb13IEBWy4lGvgIhAPmeUPVV9U5iGuD%2FhLoocDbVb0PHTqvfIdq%2BAfUeYTlRKogECKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxQVJKafg3y9Kowd8wq3ANYsEzDxqvYMVmaniCQgG3RrNLaVinCMqaWRJloud4SyeAgBO6H7GK8D%2Bm8hvQO9qfZEh7YU2foRkRXLfFJs%2FryU7DWjIjukfcykLRN%2FIj%2BUkfmG4Hn%2FEbqFt1Pi44jJgrvZ%2FwLRTJwVezFqy1CYDqcLE56oIWwktfmuekoSMkeNSg%2BE%2B3HoH4ciZM6ZTL4cB9MDiSCdi1U2zZYuHU7SOYNqBlLtK9i8aMESwvte6XrA8EMqdLN3u%2B1sP9AV1LA%2BAfkxXcGcaLmPYBibhrPS4MgO3QBBhyBtvFXKqm2gQZFbdxy%2BOwJXlI9HhePgi8rHP665nsPOmpPNeddjtlkgk%2FRySL6TnXFmaTt0bQKwHKmeg8B9wP6gHstKYhD0%2FHdp8ESG0sPz1u1i4sM7VTPses%2BOpThY%2FnbHUg85lBtq2ZjYN6iptDewcKQxOjomr8U86hpy3qDPXpdzWjSbkyX6LNiLnClliQadtRSPU5fVmXTUSDUaWJlLK%2BcF8EIYV74OiRH%2FJH%2BXYpL%2BmL6NNSmAbxB%2F5vtCVxyH00sVoUouR1YHlGWBjgsQGxiKhsduER168w9Nqk4gGLBoGDLEjY9WYSflfDkifwWN4qeR2cEYMo35vaGo7xVKh6UewWDrDD80pLVBjqkAeSt4Kp7J0N4VmU%2B21LhlCItP4PfyGTTT9H7P6%2FYx9lzyneks%2BKIm4mZzJL%2BBS1p8wfIdJMSe65E9KTasIsbmF1zH12BuSbFr01QfQEl1WrZaiavB8oJdEUpHHupinH%2BJe031tA4QgzSNmgkWUZsoXLdV3nSjc4kluFcYyr5J3t%2FL0niS8ZPruKsLuYy%2BM%2BKDJAD5afe4FX6igq%2Btt0wDEx4Fecw&X-Amz-Signature=97405e38bd3c30aceff49e6c6ae185ed26ff8e9fd814f6e242868307256da2d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QSHLZAUV%2F20260912%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260912T020631Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDZMJyg4qfgwtnwWAIpcI1Ot9x%2FkH1Tc8A0JWfuPOgnHgIgdZeW0rL7MtPpZhhseyJyL5eVyOr96fn9NUC%2Fi3vaeZsqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMZzxS%2BDUeWOBCQXoircAw0qiw9yBs9eQ0qo2TxdR6IqBhVVFevZZqu1aTCxlr7ALMj%2Fydow1RHw8MMP%2B2ZQYItPrORakkMbGuZqdK4bPy6D7oBRsIgf18wOHs4K3IKrgHlVK7k47q3%2F4NA2v421l62No6pOtXJTeZNJICH6gcxR8DoPNZbU671vrjcJRs6oNCQLVAuAcIZdzbr3xfvkNA1G3yLxX1RyxmUrcjT1LAKT3zaS0CsubYaYIg9YG2joiE2y6uP4da5T8Ome8CVErlkilqLrG47xUhpnaT8gNAdB5jL5LcFTffc72qHw%2FqL1WAfpzCchptKQty91eHVGEwCjYBC3l8ewYTXiP21rVLcHr6WdBU1cuRB6OYWqlvKpHai7jhSV%2B%2BpnC6PfJxnVRErVb2VsvT7U4QIUTrShSj1%2FI%2FFlRt5Z3YGQfXCCjMeBQ3XnsLaKwT2uRdxHdz97pUMrf05%2FlUzNEvohyYVn5cYi%2FDdlWntFhawsQ0tnZbcg0pDpHEEDpuyNVQklFX%2FRu5Xn4kz8N8L4bkZzedIcSLSk1yJdA7SzNvt%2BWr11sEheZU1YvGL3oL4MrBKyH4lQapXOxMsaLf%2FjH0bx2XjgboZF1Cfw6Bc%2BTOnU7KQOrVaavZSQhMRFIYnAQ88jMIfSktUGOqUBN8JdFhN%2FwKp1TT2T2SYeq9oq2FcVtbwK7K1lfYrDiLZkLLdO5K8nsL2dsYDQ4Dshk5PZM6xtpZteL7wnDuI4ujZte5yx8DhF2PvgG391Gb3xnSNuftBvm7v50rEEM9sMvCS1xLjwxoBpFOTSN4bxPeBEiYYH8Pvg4swXvPxk0irKX68nAs513CZs46xRvQHUWtge0C70gqmjgMyqXZTzMnat%2F8iv&X-Amz-Signature=970d3b6961169416ddd91900e1b45f95d7ba35462798427485c17da3fd334bb1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


        Database:- To the store if add features such as Distributed Architecture, ACID Transactions, Concurrency etc (eg:- Milvus, Qdrant, Weaviate, Pinecone)


        Every Database is a store but every store is not database


        ### Vector Stores in LangChain

        - Supported Stores:- Integration can be done with many stores such as FAISS, Pinecone, Chroma
        - Common Interface:- A uniform Vector Store API lets you swap out backend

        ### Chroma Vector Store


        It is lightweight, open source, for local dev and small to medium scale production needs


        Chroma Hierarchy


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662P6ZEAMI%2F20260912%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260912T020632Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGQlJkGSrGwhr7CidKUE8o6F2lsWLH6QQUyZ%2FyEiGdUbAiEA7USLNCoWhkcnFzSCtWsjLcpvvcmk%2FtpoOBXXUtZaT%2BgqiAQIq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFWL3rf%2BbCZOkSW3KCrcA%2FOLxpajbtAsANJ9MwMgnmKGNvrmDHyWV6hZKoATebMOXLTy5h9nM0AvI77jGHCOnsmJ0oF1Dwl8e0llW5VxtAYZZKwLNFv27i6k6WAiZvz%2F7h6lKCi4hnktcyDumXTx4IRJxL6g4wsjoh7RP6U%2F5yFctTVUg3pGW%2FfbTZuialqSK2aCoAm%2BBzhFKuJ8XFUyoIziU0uaKcN0gK%2FPN1kzlJrQgD3fw4Op4qHC8fzglF%2Bd9YAuKyTmIWu4T4ZIe95XzZ35LWNqG7HXD2kBhM1oUHAk4eCctdSLDX5ChM8cNVxxn0f8HI9qjJZzTinOq4uGsNIbUtkKJ52nQWrXuytef2zY90q1iX4d23v3QoYAZQ1%2BbNiP%2FCgOqexlSwoArTodGGaHV%2FgVMYITvilW%2FKxe3wgTzsILYUv7eLiwopsQRVo6rRNYREwHjrIQvnKB%2BApA6ew%2BRUKaMw4x07n60rkSGsvMxx%2BGcqRS08K67SAXbJLjlH441%2Bu3%2B2PqihyTqo9qz4uvHNhF%2FWqg1XVPByZZKImeWmx43j8gwou6K9Xhpuosg9HFn1rA1Slwya%2FqLbC38tIwCNEbxcBXWKwiaqAhNmZxq70eOIGLRGc6RtdFGXwi4jz63ennOAwI8sozMPTTktUGOqUB9ce%2ByipSsu%2BuBR%2FDv3UMIlxBfu1dG6X8iD%2FJClHwXWTxuVWuk%2FjnikcI88uo4fS5B8aQj0GeZli5c64MbaeVyUp2xwyTsGRiV0O%2BFvR7TTE6PWPEk7vAjEbhC%2F2%2Fq0FGFKhey%2BHvAjFHftWrHicscfzOA46%2B0noPuo7zKGD4KXrQ5H%2B1i5PK70Ltyy77UF8JctFJRqYRqlKdhALqbdvwg9lCU1SG&X-Amz-Signature=8f19503599edf60ceb947ed019a0c6f993cc62fdaa3e1d632cbd67ee4c5ad4b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
