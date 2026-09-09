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
fetched_at: '2026-09-09T02:06:32.498Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSI5NRAR%2F20260909%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260909T020628Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDtqzaPiCZdz6YyeFDvTgljZuH68qzQKWUI9AzENxB%2BlwIgMUjb9Xsm08ClQiDgBgaslOf0ozaMeQ%2BKQC88PgFfTiIq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDEMAomkGweGjvIK65CrcAwMfyKCeemmWukPWxUtJSr2ZI691dYoKJviRTRJmsLpca4G7ZN6nD6vlynjUTsvNiIRxT1%2FKVozQJfV08S%2FrIoM0xkcc%2FOPgL8V9IpXBOtHASa3IoI4dg%2BwvWof9d6S50TKwEPSxcSho%2BOJOiZ5dmrEYjMgcY1Bty8hYdewzZI%2Fv8c8u65QMfUhXPlumdXRIU3skuGy9encptG3oMmLGQlY%2BYkPIxASOc87oPo7hfSubWQN5bqdWegKqq0S4%2F8kPUfNwQQD9XHMzAM9MmiiiLKAGHpXM8vvxx%2BwzYXxk3Io1GtB3wBy02C6%2B6kixvxFTJ6nT51gcEzkmS7mLNVq5Vwn3xgLhCKonwI%2By4vN793l%2BTluc6xFjtwi4EhqvbG8kmRWeMLPqXg4ztl83AD4r1rmvpnTCelj6g5hMONrPV1PY972vjyVlH3eEG%2B%2BjwdfgnoLQiI%2BBvoSQr3tFTwmLxK4GI6mg9oJRkxLltDqo3iIFcbBBNH4ZYrrK7wcYhtiqagN1W2KOyHUFGbd4XisAdDWD%2B98%2FXmL57tqW2ew6YHjGRSBnw5BXxdLDT7RnWyipEXeB4IQgfb9VhR3ZJXoOrGTBDRQDynRSrKhaW8bxeexdjFR57VyTe6yZssA1MN3YgtUGOqUBTewPHoFotbW6usRtwKE3BI%2B8xm5SGiCNgQkIWSttHjQZqT%2Fm0BAbDAG6h19qa6AoE7NWOFOHeXIHzJQnC%2FVnCRoBDr%2FC7FpmyGpRiZtajX4nyFE9pIpFm2TBjI9J6acmq1YDQEm%2FOSQZaMdhVv%2BoA72ccIlFiawQkKmuN7S20Xt2qR6vMK2kwaapXNH1ptrs1FdUmGFGZwYEiVY0XR%2BG1BZ8xcW2&X-Amz-Signature=c02f1186051faa9e827e235305a101400e18bc396d9749168f9d9100f3774a12&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZMCDRQDQ%2F20260909%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260909T020628Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBumpBeK5HXXbnHzb8nMgsOcJRbXPyvZWz9nIYmSi%2BiVAiEA%2B858p9305gSCgI6YOwgwgyoSD0eV2Zc2O43V38RRPI4q%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDLVc1jlzb5l62dNaWCrcA0Z%2Byp%2FglxdfDZTR9nvyMTTF4CJ5HChEHlY2m3YBf3DTIWY1wRjhqpYIMd%2BWL4Kyc7rdVUo6Qd5OhyIn6EHF2G9psgdnmHpvcT7dcb8hOKspZv0d9VngTWqkImJcKqlIEjmrF8Gdi%2FOG3YXKVwrK0CQ0XM%2BWwElBktyfHIgVQUiDvlQzAytvct%2BXmAPsZVUqwENjqR%2FOYzUkOrAvF1phhNtjtHn6Zf2C59f2aLCjVY2p%2BcTvq%2F7eAmpWwctaExakb6nJjy4x85Ob6fTB%2B5xWnxhRAJcjXclMacrMBGpDx30UaxPh8KAk1sTxhLcXuDka%2BY8n1mYbF6yQV1y8RhY995XN2%2Bwt0rThZkrVgW%2B85Pr8qO5bsEkO42XyZ0ZJOXtGLnUNUJEBPzOgcWD8I%2FFPBMnnFffEGr9c26JbYITTHIST9NaeQ%2FCbp0FWKXaX1%2Bw6ipTLRfwJAjqEFwz1oEdzxkpbFFQfHE%2Bax0EdNFjPFBUXZeRv43xQwXlrr0OTZrtHWsPU5Wrp%2FH2iXyjxy95gt9oltGYEBkI1YAyXwOJL6ClzItr6QIGQ1e67ZZ5yz4KxBVw8Xq%2Fx%2FSMG%2BqGQlwmAtTY7%2Fx4N4QtjNQ3Modrp0EnP0JIbQRWY64XC6AxbMNfYgtUGOqUBlfoM529zRwVYNsVNOtOQJGsieqgjIbEBMnfrVNuxCqUSJiN5UeaXBt5PN%2Fz0jcb9%2B8XWL5Pdf%2BiimDvUzL0u49s5R2mmvS0cxjBj%2FRYPtdE8PQavdti9axRIafNV5ZID%2BTe4PacxUMw7h7mOVpN39PF4BoNl%2Bx5xcaji2e8AR5mNznUs6rgXKF%2BY0Q0Qx2%2FUV%2BM4YOaWoatpH8jzws%2FBNKY2%2FS%2F%2B&X-Amz-Signature=02b390acacd743bf41b2dbd12471d8d62c5b708a68f0f50ceb8c541f1a984e1e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VX24KR7F%2F20260909%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260909T020629Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGtu1Wv4dL%2BL8m7zR7qD4rs1Cml46r9KV3gk%2FAT94encAiEAmZigzqckUhiWcjgwyrG5dDStVqAYNPZyyK%2FKsOq%2Br7Mq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDLaiHP%2BhwuNkCnvl5yrcA%2FgJl7EuMniJ29r1UpIk1Ax%2Fg7tIaUBd3tb8uIxf0BYvOyzEDJn%2FDC8TB6JxLJCHyEx1rdfnaeK18l33MrmM5fd2jEVx34ZV%2BB0HA0%2FCo04ZnP7JmkwoKXnUHV9edvUtIZlpu4JnXBkq%2BNMPDULqSJpgsv5W5EJhz4AvaAecWwEZU%2BCwwXCo%2BQwjzvQp3G%2FUyqil1yrL5wtkC1ZBtucvjvCaMmCki0DijDYN%2BJ7F8g2ge028xBKEw1uXeizIrY2%2ByJr2JLwNrLvjrQhf2q5FBLzMJr5PKA5uYCz4PvMLITAf3%2BvB3L%2BT70jf9JpBFMH38aURzfjcnb4lgY9RqVG7Y4GtNLQxiixwQFj75iLrinWRXVV14NXDpx8WA6VDsUgJ085FOEQOJcPw63vXIzjvCZLvbCPUTI2nLV8jicmXFqB04Q9xZXLb17AktVFJY7Tuf6Rm7ZRKUCCvQgYq5og6AAr%2BqDTmOadMuarRVP3k4JzW2jp0x8OVa7SCjTyxXUbcnr0BaWgYSu77ytQcLoCfKvRY6PFWwUh5TpfEvec3z3zc2%2F84yP31N%2FsOoY3Y1iitaOJTYhz0Qj7D3iecfp1OaEdgCw8a%2F4jJ1WjH7YoDE%2BmBzI1AbvwmGcuQmNqgMJTagtUGOqUBS1%2BDXoVpcSQbIQmgNIyW3cVJBgV67ejE3e6FEHNQLAVD9Avp%2FQMWMz%2Bg29TsZDMl9IQ5u%2B2qZAk0KT4EAp4OB8xwZkmTrUFUyyoXSTLGICvp4Jw%2FFVtxbrRAnhXXMm7jYFi5WMX7IK%2FhI%2BlawjGiSrbpsa4DyjguwiB8AumXbF8uEU7VlxtL99Kh71PFz%2B2dyb9t02CxnC%2BfKUamZBZ359osONv5&X-Amz-Signature=8e2299920c92966ffedb45a2c2c9c9e25025531b79486208e0e92a84570b3240&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
