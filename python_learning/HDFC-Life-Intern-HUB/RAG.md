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
fetched_at: '2026-09-06T01:53:03.807Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666GA5GQZA%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015300Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJGMEQCIFDtjn%2BwWwELh12bDRyErt0%2BgSX%2FT89OkKo01rrJ%2FWugAiAQsgCEixM0BrqxWOFbHo%2FzggR02pxQz%2FAXUsXueeZnKyr%2FAwgaEAAaDDYzNzQyMzE4MzgwNSIMNhWQ7GdvdjfMFp%2FoKtwD5LXGAb8T7%2FG9K4bEqd9za59lxqxLzshJzt%2FjNg7FenUvM677B5P3cSdvCEMkangy5Jiu4AAPYVW4N2We7r%2BH2w62KU2hbnTm%2FzVG2u78PxhO%2BrHjPsZe6dYtcYORRdyY9Rr45t8V5ailveUSBkCDnR7XjpRf423NNSCC6jbDlOCs2MLO86SNhJ3TB7SoW0CSsy6xXlBJX19JAtDVdxVeNZG2q0ixNVYKU3fXdvs0v5726YjE1cafxem0FxLOsRbkxIgMttc5koWcpEttXc%2BBVgnejWREu5gbozgo8q%2FY9zPyhc3UprWU7ENW4WS8zgOHMi9oAQp5wF9zGOR8RphsqZi%2Fty%2FVocvkmxQm3WT1fwTxey7On3Nl8voyi8qkpGptLE6vhHOkhwyROVwNBynSOZ%2FIvf%2Fi77kw2JiaFYAviQRPzuIEf15AOFT9ygQoRAF1jI2iv%2BHi4Ki1U4agt%2B2vEAqK8MrqzOerALKBRPCqijX%2Byzr6Bv7YQjrjM2AOLMdtGorHTaPMw67rmjBrbhtx5ikf1zchW9Bn8MjKZVLvYNhsOz%2F%2FR5KxVK12vp1FkJOETk%2FkFtzKvMJMAU4eo09O3Btn0GlmNNertrJQwCMcEK86phBw0ZlbkQkXVpww5eny1AY6pgEKLDK1g0szloOO9PnYT%2Fqj08fFY6EdHL61o6G%2FELvVDSioFe6yOK6SnhbkkhD0Lu9Hz%2BIfC83dDHf68r5L5P6tWQ99P2DyN4DGP6JH2WjYVpWNXNMwRZp7md2llkaR2IluF0SCgO%2BAHmFc13e4PwUVnlG1rjYUKjji7k6Aol9PoWpJCE%2BcHYnby4YmjcjFgfuCFCx%2F3p23AfBCdUpnHgM1TaXHlUtO&X-Amz-Signature=4c11e67701fb0b2b608852cb8169e1e56c84f2bbe93b0e6de0e888057f265ca9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SVCWD5RI%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015259Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJIMEYCIQC0vAuiOu3iEj8dal4mL9lx25mA7%2F8kytjrdOBe0A4yOAIhAPi3YYswdx1uY39lXLlpyRLkYXgS51SO9zcJ9jKUMUYAKv8DCBoQABoMNjM3NDIzMTgzODA1IgzsNroI2RYJy4IpCDAq3ANQb16XbmSRe8t9feSVcEsIr73kl1xn%2FkI4falaZk4KQCSy9dBalHoTvXM1pkxdL46Z5k8jf0YuwHfqK4b8lrbDtQO3X0SgX6HYslVHCQcMludS9R1Fut7MufbnQP1Dpgf6k%2BRkdpmspTr42l22ae1TvapiH3YzcytSl1%2FY7%2FSCGokJVFGp2iuI7W%2FkA%2FFM2LtXfimKby49xjMYwuWmvvF2mwAsynR49N3bf0QqdixLIl1y%2BL7qKfDhKD%2BOTfhoNoGVRp3bM9XOK7Pr1FvoOX4PXtyYqiIRmpV3%2FXyKnoowoFaA%2Bdvh5GMORl0AZQZ8Cx2zm2y2rBa7bSA7qTiy%2B%2BKHiqI02rBI%2F9ijLf8yhkR2MANcOlz%2BraWgClHPAlxAOFT8wPq47V6xFPejAk9BUI5Wrmzuy6oAWRJITCqppeMhQla1DPEFQcu7lF75CS2mU%2FRMssuNPx%2BTTIXthxbi9s2gb4cx5jMxqTe3VYfgYJQ%2BGbh7VgcyogLUun4uJm7HQHH5zhITsYYJ8N7OrJIlZSEzMmg8S6mkLlORec9ygCmy1JuKxLdzdAFp3GQdA5xr4Kfy9M3LcAxlhUi3gGaHiWNfRTvFCjqgLwkvHjV8ObfOtC%2BlMvjZ%2Bl1CnESmnTDi6fLUBjqkASEILHKIQXeqA%2BmoeiMiSzu8th2pNRZmTuDYxK2DcE62DM37On5f2VyuXUHMOi7ch%2FtPEBcPBBC1poWkFTAYhrZRL50nsp150TzbLstwDCOF66RyEuH6a5KzNhTBeqPZpK8jGYvDSiPpHjOgW5Dw%2Fh%2B%2BQ0tKdCXtTMDHl6dJ%2B4qKNZvKkVLAkRb7VzwqtTCDzLMusSzQ4IuV6dg2h2%2BXuuYm6hgP&X-Amz-Signature=32be4ac0e4d66ae09e4351907394d70b94cd242fd274e8bd9731f624c0ac6838&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667Z4KMZ7P%2F20260906%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260906T015300Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIFF%2Beb6WKD0sGNRdW60ujUmh%2FI0n62GKOPZZaqzFuOv9AiEAstBYv4Rhu9%2FJWXLKqkwMjx%2FkMIl82rJ1yo8PcdGS%2FzYq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDE7hEa9ZR2fBhy0yXircA2xvOrySTIEKxzRVbwxcqRoHiQCGL4B5hU%2FvkH%2FV%2Bl%2FXQzXo1zeNz2nhMoKrLV8a7iBFc6VTmX10PfLImDrEHsiaxxetGVw0vyEBemjM5mVNVfJOmoVUk8g9ESU2qEBfG77nY9E7m2J%2B6IMneMc07ZtpypM2N%2FQfpQpSK8LpKSmpF2Pj5ouAx3UcOeb8Sosd5%2Fv3XFynvea6rcZbjkKJ7Q0VuewtiNCwbPTv4GkDr2j9OZCR%2Bh8UF68rRTZkffxJMmZkKpt8b7zkLsQ11i%2Frt6JQsZNgo24XOTW6Kny%2BKyH1DQXXlzrR2uC7BDS0CKOj1qqInjx2gDLot9aTix1%2BTeUfCPekCd1S59i8o8LDoj8b8l2ATcFhhwLhxDdCfeLOOjzYHU1tTVu3NRy6LCA3Zq1xVzPIB725RjlP3eVxfTkIFRkCj%2FsTqKxlIiyUovxaFP4m0%2BGxK6u06uNVNikiSzC%2B%2F0FkShnIfP9yWKhoLjBLdYB4eYvvrqNew0McTofbdhKFdTVgZUsbvx4XoKEmij10S9amFyhKt7xSzl15L6CbyJoMH1G%2BBaauGjWJ%2FDqSP%2F%2FKA76Xk95hHVlE5%2BP2h65%2FXejUal7e75ZEooVt%2BckLfDMYQxdjduUyEsM2MM%2Fr8tQGOqUBt%2BN3Yn9Eiw02Hb5qZpWXfXUzZsTGbsJvJH%2BhEjH9AZIHlqpgRTC3ybIZB9UQ44C618MRRgnoMD7FWw8oB6aAozvxS5kjfA0SN3GnVx5MCGMljhInYvrvU8Mw9ZTN3oDdh%2FaHckdasggL%2FUGi4cMKe0pPoUdKsW3Yh9bgdz2cl%2F%2F5TYtkvwEc8ilIvmHf8%2BQUskwsCf4Fh8ON0%2BM%2BM6iCy2nphAkH&X-Amz-Signature=f6f2e9c58b5a9a22d56eee83c6c486258d3a218e042fa523bab36c28ff372c77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
