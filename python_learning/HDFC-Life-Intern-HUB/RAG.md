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
fetched_at: '2026-10-15T02:19:54.501Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667VXSQVKB%2F20261015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261015T021949Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCUPyuE9lcaZTH62zV1fZgGdHacXp5zypAUSiIM%2FSdKDQIhAOWDB7brYB69cW8vNaSGswOFNTrAPPeXJZf2AF42giYjKogECMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxz44QvbIX2FoixwfYq3AOSCO1RCs6lfPh07Ru2Y7EOCNKVsd7l5TP4NiVzVgXn5fxuJd0Ld7hmAGmZaa%2Bqkc1ysDhbrVj0%2BHdEAGGN%2BM4Lbd%2FBYPeusYGU2UqI0b911rU27WjZElyVj3HhVFV7d4ykpF6aKoBiwu5HMGuXx7Go2by1f6pNqx0m39j7K2P2iy2%2BEVd8ZsLSedevoeGJJExcfN%2FzmqNu2LQEMqxDffSIeKPEkR4hgEkvNFKsu2C4jl2IEzp6wS2Qp23LDcnfV4LojIBa8kOOECs6%2BKsonoLbr89Zcn808UASkJMV5sLIEJAhh0N2j%2BZqrIm%2FzUfy%2BCGewcQX827i4ls%2BhvqiYapgx2MydAhaOaFVMCCMZbShEWiOJzdHuS4GfD3ZIAK6VdunvxeZJW7Q5CXzvhyTwnQlPlELkb%2FE4kUz%2FjIH9bIQEHQGFqueSEZzMK52nIoLkHxEDKkWmnioyg%2FOt9Mf%2F0dvq4pqEOH72JMs51ikMZ%2B5IBHXXBgEPY9E0ik2bJ%2FafIskcYEWlcgqe1sHVQqsgoWSKMx87qeCEbXhchjmgf5i93jN8i9%2Bf8JDTdqMR3yaA8lFBqyXjSzrqgcZ9OFuDmT74SAOO0lg0csrJ2fhXwrr1tNZEyCdV%2Fz3houOKjCfvMDWBjqkAf0RHAwFPCGvMdx5JMeMF1nae1rXDssaYCp%2FgGBGlOkZbvqlx29UR0ChTJWlcB%2BkFqyI2Wjj00VEj0uRSyEJF9gpGIyiy0F0Px%2FqeEPOkhMahRexqijp0Rvs6wv3fYp%2FkFF%2BXw5NHQRFw8JIEYLeod55ZCZODCO8xav6y5JqzQcwUZZ8o30zQjuAkuIC7TZMZ0wohkowZABCgacu3Svpob3%2FMIqi&X-Amz-Signature=6bd8855c45f194cf1868d20ce088ccd7f0c5ec3e593756cb81f3e9d02bc84634&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNEUD6II%2F20261015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261015T021949Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCMwLBf97eI21X6INRJbzQ5So%2FAIOLbIUqBgG%2Fa0yDZDQIhAPGL2jBuQNtVHa4O4V0CbGSripM7RjasZgI7ITfHA%2F39KogECMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzTEciaY4kf%2FeQNYikq3APxAnYkwmtxjXl1A3N8gE7kAMdmnbzPqfH1drBcN559yNXgcVweuWsmSNbX9G7tdhcJdLS%2B2LtcIREMgteOYkd3q7uRFf5X91YlalD8Xtaq2uH%2F6ixD1XznL2oyypv33kxsJS1fjrMAuPo6JdryQR1Jr9q9HS8yoTS0p4gN8kYkj2JYjHEbEbK7yJvZWJqfATzvaoZyInwvH1QRYYEF9eycnBAGYPt%2BLc3FaFeb3Jj1s0f4n9%2F5cMfqWqbYbV%2FAIfOrByZvhjQcNRVLWAAKCDmkPcYOmuG7DNNZJEmZZW8eNFY7srHUFj3CGtWOtRYjbXZcDDxDuJ6Ls9aNX%2BzO5lGvEMImBmW8J9nDCjOoROnHqPyyhJpwQJRjRwerLLZzWy6OOVEjC1UgMNHwYwsiyKm1sZgfV0PZDHFGr6J%2FUI8XWzT3ou07kf2AeUEFhNcPX9wsiLI6s3IB5DaBKJltilo22hkfukl%2Fz7m1wR%2FMxTq945PgD9r9V6YDnZgtP1%2BbeRMPFT7v1aFGkiUXJ3V6%2BQj3D5gXOZ4JKwD8fjK%2BYCE6Q1%2FCRK6DVyuBcmxYBf344Qgl7crYbAm3A9MSrOWdpGlV4iCHSFoD%2BJSIdyxbTbdaOKWmfXmhORVZdg3FwDCcvsDWBjqkAWoQPjazcBtj79GCdKHbb8HJ8NxwKpbhaEwWniIpoHBUMnOLFOG4jE%2BI90UKRt3tY0BIThgsTT571pjpmYuHE1lgO5KdtztGvHxhFKfaraztHRhQR5dKbyVC50syRuZLjbBhbR81maTRNbB7nu%2FTteB23IvLt1AbXdaZcqRON2mRHNRLfjQHP8xL81JOxaVVdE3TgWt2dDEx79rIIccIK6Lqtdq4&X-Amz-Signature=1dc6aba2f953f362ec32095e1fbf495d936e7523250a042395644545fab0d9ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3RAUBSB%2F20261015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261015T021949Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID2neDsz07IxiSEWuOh7euqubJENHIuE%2Fkb0%2BlMqWPVTAiA3YVjnfAHgVWvhzT04gMTDUdBYxenspJj7EBRE6GHJyiqIBAjB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMmscW5XdOCT36vZNzKtwD96z6V0e0Pj1xHUgfhnP9yWpqbVxCiSVg9vWLV8cqWbS4z3u8FG33ZmqiUiyypzK1SnvR%2BnLZ8YPlMlkZ8U%2BbqnDZ1%2FJJibjivCrMlh2mPS5DFx2UuBLUzwjr%2FUKBfqFxFsre000qOdteEvBdw7Y3WprMo2AX1MYMgFzw6BCDrPkenWj1XthvOwEbaEqlOWXbnq0EhDhfYmVzR%2BJaLcmRLVnMmtyJcFTnoGWXhIrVFQn%2FycH5MQ9JcarWPjPxxqS07NSnApxYDCRJpRYrEYfBuHu1SC2VwSUMLVkzkG32art8n4DE%2Bo11g2vpYMAhMg%2F644tsFVmb2smY%2BqAqhtaUEOQ0PBWwdQp6DEAZPpfbKzcHHgDLNoGVT81Yw1QLja9%2F%2BjGyfpB0mXGH41kk0fJ4zuYosEfwgNXJgaTvfGjSOwdjdGHUxJG6K5Dw86P1GI%2ByCRG%2FNEvLeBNnKAIJHiDawlg0RLP0QJjdvoh5gIfnwkb2W7owy1guMNLSoOJaTvgOf9sQWSm0WKCgJcV7CGJSmwVYjQ%2BEbUmE2a2S%2FVDX0yCFj00yRbSPI7OqvH0OmZ7GILT5xH61O16TdI%2Fd347gznFvTmebRl7h2HCyFr0rDweVuR34kKBjY72QNTMwoLzA1gY6pgFjUrLWCuC3qfgsPeA%2FxSrY4Hxas0R54hmf81A1bJxD45EbkTYDsQSPxVNPRV4420V91gIhIiJa4JShWt%2BIH1P0%2FIeNqo%2Ffzs0WOWCNjC3gc1Dmkp%2BsbDNe%2FHPUk7JG8FSnGAPv0tjOV2XBgoaDTNCpi7qGN0aB7T86U90KdtpJRGDt2HdaJScTwzy27aqc8YCNVtOGvF03sHYGA7aSIvXcC9s82Vlf&X-Amz-Signature=019dffcdac3fc06faba5641bf19f1517afbf5a63b8c2098b59652be691dc1617&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
