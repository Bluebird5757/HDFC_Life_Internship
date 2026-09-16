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
fetched_at: '2026-09-16T02:18:57.803Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666LDGBHGW%2F20260916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260916T021855Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQCW%2FKzr1klCelFgWKQITZJPBtS0W8ESqXUA1klxE47xngIgA3WU5sOgkG8Bd7Zfdo%2B%2BAGMpfG%2F%2BvQAtcrBlkKA0OT0q%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDBr4Pu9Qi0%2FJRsIo2ircA4Jpok%2FNjzxfB4I8jOSgUphydefK8T%2BWEsrpYkG840GkzJPn%2FJXY9X47AinnVL1FaSXoL5fVM9Fe6q5r3grJt%2FSLt8HEvq1EZphb5%2FHvFKov%2BllBMAy0CQIZ8XZoIWOgfPZQSpvLY54ufJ%2FVarDJZYQxTv%2F2lp7aiU0vnCuDEIlUQnluc6PQ1M4c63kkrol34eJ4lSRmk2k2CIz1Asd7Yoz4VnqZ8npDzMDRHc%2Ftq89gw8bhXXQjlRyNoVPJ83RrV%2FTCFoHq309gOcBllAcsz1vdplXzr262ZeYCUgov6LlzR0hfe3ruhgAZtfxKi%2FdO3vEIcRZmcjXqf%2FDKd07WklwpZ0hLNR2aUsv3ZMGMbcJjiwRt0SY2EtXZmwXfRWBzEt38xjJ%2FtJtoQ8cKjHNNrEn2TkCnEX0yq6kSSA5wzToopYGSdnaaNfq9CZ4l5HRE2efR07yQiEo83qSGUen7exzOJMi5QbWVGvk8PbClI55U2tq8iqSHnQXy2AAaLtSdZ7u%2FtFy%2FCf8TW8Thga9FEilhaJuthjJIb5Ng1lu4fCT3P%2B5%2FX0rPJUAC%2FXg0nHRcMtVxTEYGQtyy%2B6nkch%2FBIvIi7e5Zxqf1Q9hHQGYowhzW%2FsnIwZ%2B0tMqffsNWMJSxp9UGOqUBFABKVseJEQBrE4WN2%2FE%2B6ivWP62UjabXixTEQL1GXaGSvbFHXR53SYyB%2FJutZy7b8hzI6yx44rGUt2SthXxMnG1eUMdYLENBxL%2BHShU%2FbB8ishQGfC0IQbPc0NLUmKwDxPbZ7o7tBqTfU25pnCXbGxZYdGZQBVeXNxq3R%2B4dn966VvYksHxZh322mAfAkaeNPJV2oIEfPkP4OkpjlFhzmaCvWOst&X-Amz-Signature=2ee483adf2f7fe38a5e60dfada2063027fd22fbebb7d34698224d236d0d038d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663HQPNDGA%2F20260916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260916T021854Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJGMEQCICxjY1h8VDpsMpS5QpM%2FC%2BezT3TWZ3%2BH9Sxpzsyu1Ka0AiAgOhl64iW2W2QSOb2kBj%2Ftz0rAa1uq%2F9GNHjBa%2F42PMSr%2FAwgJEAAaDDYzNzQyMzE4MzgwNSIMUD2EvHZCkvu2nhPsKtwDFeJRVshEhn6UYFhQgvZMJFJeCHUW%2B%2BUjkQzJc1shpnDmIJCS5EjRLrX0yKjDL47zd0jV8b3hJp7eG8ogonJBpehi6eg%2BTkg3tcwo8IS4FQ0Sp45Nlhh%2F5kc2U9JE0hC6%2BhPA8uFQy%2Bgx9LKK5t8mjTrj5XbUvr0sGegNx5Rwrn3cUx4Fkoc8vKjSpYCqQ6q0STqJpj4xFPHR6H2I685nhvZQ0MgE3JxOChsJvbCAYEywjNLL9slGpk4R10zoqAyG%2BJ3hHs0UbA%2Fk4v%2FTtSQXu6TCh6AxEHhOsd%2FbeudfuwjQbtpS%2BUkIXRLiNXUId0Wbu%2BEBotkpuFRb5BQEmSlxmXK4hNZFC9F1YaxC6JeDgI9dKR3LNF%2BKVqtVoycu0kKl7FIr0E4vSnVqevZA8Kdc9astXdrEfKVtg2hqk28yZdldAzqhnkHwVU0yrk6n5F6ox4l3uJN14cHMXRzdO4MCnjK%2FEIyn894Do26qAFgjjuwvDkBLAwZG40RiaI3BOvt2GhO7YW3GYQvvP7Feo83lUM%2FsiEKLsZO1EkWa6EG0bbUFwL3EfH2nb%2BGwUU4wjtLtsOUJHi1GzYD5d0DQmyJLdhym68vGi4zrtRgGSnnZX2dTdEZNRqEp2TrOUyMwgLCn1QY6pgGka2or91a7AUe4vrERyIGYXeWL8GCgY7jOJDNSs5FpyEh2PIlyrJizn1h%2FJb4IbzCkUDy3ou8dT%2FlHOIPUUEwuhEDt6TyUvhamCck68M6ofkyGYUV7ylNOaa1wkugAWNgFRZ1qA%2BJDAN7pLMCKKsqVjJ8MBsaOuUE430%2Bu2bqLodC6SjeETpMj11faPz7al4ERueEmVciXPZoIAfr3nHSWuYgUcHNK&X-Amz-Signature=ab05c28aa1feeff9f6888bc1cd579c6f82711c63728169b96766519d9fbfe46d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CUZ5MCP%2F20260916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260916T021855Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIA3OKNK1sb38f54aSF3M03EgCfSBlG9VNINvzsjo9NfWAiEAwabFj1INE5n9tGwZgIC6zf6DS%2Fj%2BEOWF%2B7kPaI0Y1toq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDEt54v8UCrmPtYcffircA%2FyIOGfFGALIFuhB0yfMSxhqEgw5bk5q96a9pjPbSFVGnpBhsNdaYTZXJFCcHfw2LUc%2BrIqvgf%2FtvDsESh%2B5cQv8jzx3A%2BZaAY0Th5SRZutRMyz5ARn2NnCymfMD%2FLwpCXgOKdCS9%2BgVvth0k025jDO8WU6d%2FdZaOGlgTMWLUgazFkn1LBM4zQBy4vbNek8972HUCD%2BMeFK1Rl4eYc0VCGnTrP93iqyf4QqIVfhwU7j8swYqoOZKoIZ7IISojM2JanLEdlc%2F2Fy6rX3VAsYgDMiXrphHODpm7DOFc9ymeTg8FCtaKs2ehxjFfaNgDY2N1xAopViLEymraJoeTz4mHAq3WHrBHXJqDMZqS0TArSRSLoDvAAKLKFNmbM35c1W0ohN%2BPgS6zLDUfMS8xGpsdskZPdXKuStmAyDcTPcyKY2CNkpmul2PiLzenWQHZa9qlF%2FQ5ezyrHQKgTAVDLqebiT9IbyXkv3AFbNgp1pqSjpQZAbmwoVjrwo1tXXQAXVvVpSSSxnHipLlunVo%2BqV5coSRpYge9KgD5PISrqXF2aygSzuPvlqF7NHGdAiSIdtLnGVhSmpFbXGbYOrlciyJ2Hu0rk%2FOZrKtLsXj%2B1qjP5QlFc60YIIQ2AHY6NYpMIKwp9UGOqUB71Ol8gSqfVHcv87x2e8hynbGtWizI%2BMF7t3lPA%2BltokxXjmTpfS53FtWnpFK1lOzB5fh1jlCT6SR1FLbfLf9qPxAmyzEak0jRqaHjTKi%2BZg1WHbIKMCvM9I0gTul2J6IElvyFnWqisugYtRrUVW%2Fixal0UfD1xMwU3POsiSWsHwrhAko1DSSZCuiUGXOovU%2FGUaNKpWsC9sfMMXm8ahZC08Gijxf&X-Amz-Signature=2b6dc0a482d1b2606c67dcb113051d636ca24f347cdbcff3cf87775fa79a6a97&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
