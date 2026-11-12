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
fetched_at: '2026-11-12T04:52:17.960Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XM3RYGLG%2F20261112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261112T045212Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFQOXXCHPbZYy47Lwu4Yi1806UWkAvu%2FGru4%2FIjyJZQjAiB7Js0Ny6nEbcijICVwB7S%2Fof8E7k%2F6wq3OaOT7E%2FJdnir%2FAwhjEAAaDDYzNzQyMzE4MzgwNSIM5X3%2BvA5NrKrncPw6KtwDfyrdKM0SPAZ%2BoFoFkQDp3FS6PTR8gCGmD0U0paSPcvE3gxYa%2FfFqvT3MEByDKySmP6ufDNqeoO%2FnqU3s%2BTBDoSNdJMfpF%2FNgykpXtaj52SNriorzByO%2F6lJBd3lGxP7hkxrgyKDvwIt9hdC1v2OovYoTJyH03m%2BW1qc3OOE5K4YXMTEMgt%2BFN1xHYOHFqDYU2MBpvZij2zky2GIsd%2BNiEaEMCWd1wgM97o7qxmkPw7uHg5gBiUWmfglyktlHIEjjNOlcu%2Bdb3QdC3fy4RpT5lSVJxXWJHcv%2Bu2n1OJkn9B05XQOvMPy%2FLJ7D%2Fd%2BlsDN0uiFYJmQh99SOsGZR9L5MrS14F1nAt6W1e4JSRdXdhK52%2B8ShQ5uSA6InC0l79Z3yPuyiW%2FwTBSK0A40BebLYlPFRaIn%2F7euKYL6EAit9WqySezoLCgF58VizakRj67xMyZTp%2BHsEkjV7OIqE2ZxmHwFLKCaQDknGW70bz2zvuyzLagFgp06gimkXLlB3A2nAmPGdI1bydKP7fMKk5T900UYOSzTgoCU3GsWaiP04UkJQAysamuLuQ7d%2BT17QPfkR7RcZGYf7Gis1IBbGEkYLtX%2BPGpCPIVTbnVumtav4%2FJYcJd8Dc7tjuE9EYZkwib%2FU1wY6pgE0fGy%2BkDSHsmoqr2WbpQ3HhjOEXXlqYGZl0JBWQiLaL7Mdjb9HVoJC1I3y6ASSjuSOObHn94%2B4ulVprb4PgrzFsb5ovFOspxfuW94QyJ%2B1ZkuloaaQpglD9hTVyTYhj72%2BpxQOVuyaFVWT%2FHRyFlRbAaKjhp4qMQV6ZdF6LfFiwbXbUHaRtQedYAM%2FG1HNZpt%2BIxPewZDHVTDvO9es8afSPmUAS2Gq&X-Amz-Signature=baa352430b66d79feee9bac04fbd9552ac496cbcd68fb088bef018e6a82b1419&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UUHGU6G4%2F20261112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261112T045211Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCiGFUkiRsbbknXzRLM7%2BCe8OJIUIjuO0DHwtcgS0gFSAIhAPbx6UeKerZfkcmX79T2B5GMOGjt3rWNS%2BE1n9xLHKHXKv8DCGMQABoMNjM3NDIzMTgzODA1Igz8uPIbEs0FEK3MZo0q3AOX%2B23KFKwPB5fGsZwwxeuPeTeuswavh2mXcqfvw1g16RuNs0q9e%2B32T%2BG%2BlRAaR%2Bh27EY2ER1RkDsnN%2Fm%2FNPslpGstYpk7W16iZZkHw4jZvIb5xA7aZvGD8zRB8eJJBK8BlA9pA9ZdZAOblooKo2PHolsenjqzrQqNBkoGaqcyWVgfNpGOecfahA7myphGUcq8snZVOz9nEUi1DfbgRvpz7h%2BtLk2uK2jM5CzkxmbGlzq2Y2WvIgEnVl2D8JzgHyT22kIjgvqBVa%2ByiTbsZGw7NhKm9gaNYE08bjyS5E8eBFDHlu%2FtcYnVYghSojCTYkmWEN56nhWkzgUyaN60nSAm%2BNvxyjX9lzejA6tokU8nhARPI43JjhfO878Vx5NsVbvu%2Bh6KoYXm7G2gpvQw5RyBbWG5w9hLGsk38QQ8KJJUTMtjE1NuyxoD74I5l54KhmfO0VSpP%2FoJDcSoz9d%2FGF3O8xqSIpOYxpVvKI%2Bda9MDcLND1Gq8SzWAZ32wkWkx8KcngxEtd7eZT%2FSg09NGbsoQ1OD7ydYL1QnxtBzzq0jYnH%2FqXdydZuVzxVCH9D0FInTkBX2%2FA4HoirZ50csbzb%2FjuL2cF9KchYktP4YWtfehFzKWQpsqYxOt3b47TjCIwdTXBjqkAZpwRN9cECfMzVtYNz1K2GMPCE%2FQ1L22M2aJ03aiw3WTGR2Xj72kS5YfLNkRTAvi%2Bijxs9fz3NaJ8rH2pkgl1KgPDWHohbzLXdfOfFj1BQAZwiPhEKYmgY2flnBegyntBgseNrEUcNDScwmVVSkCRBsgjIECA5N%2Ft5vGYZfxSHUguQU%2F6nnMvwNZ9B0D23IR2pPEHAJ7uLX9wpKyjcAhjb7M51FF&X-Amz-Signature=a7a9aab9571080cd2ca18b464a6f86e8b1efd5f70c1d52828b9a8e4226e7fddf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667KOBZXE%2F20261112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261112T045213Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDgxNLA8fJsFkbqLldzGoJ06A5sTNN65MB%2FsvFgRHpLqwIhAO6nujMp75z5cvHCy613923bOphsiuNyu%2F27rVmDkdYzKv8DCGMQABoMNjM3NDIzMTgzODA1IgxNrqQmLMSME%2B8A2Doq3AOwLkQh%2BLZobSxV%2Fbl59wKVQg2l%2BadCeT4%2FEdzoPnNjHtxlwlZFD40Huasmmn3nY%2BQ615p%2FNE01vxc%2Fi1lcJVtpsV3qIqk7vaNQAqLGZMFZoneZiuRP68MlKEaHGGha0KZz%2Bpxo%2BCnDY3hI6FaCNjtpkPhbvncOX4s55H9cUJw1yfMV16yTTjbhq16yFwrGIrSv23QaSq9e0DVZhsr9Py7a2XVcBtoFpuGe5skQuz99DwLMhn4GGd5GhBhk17wPXLmGyFiqK44egaTSPreh%2B7pdIoAMED8jBZkzlicamI0rzD0utd2%2FX1hm7tHnPT5Yo0xu4J%2BqBL96jbnTElcFJxuChI2On4oU0i4l2b2I9yWTenmtdc7ane8WxgMIS8r%2BEY5ktn9OzojLKCjqRsXIXi%2FRRHbkrpp4FzMhd6AEY89Jq%2F0nPSKhIo0aQnnjqrZD03u%2Fa0FWq0dKitpFHit2H11UdUEVK7LOY8W0BsvCVFXslROKSumAWbwfb%2FjuRDJfDNIaBIeOtbx0Fo%2BJffwbQhlxcVApKIwQqzkRnW3dQqS8ZfRGbN4S6NuhQSBWjNT29BJ67LuRA6vWARypoAjmd97lnoBOjASqaS9kVR2McLXFQBzu89%2FcGMQPDbwIcDCCv9TXBjqkAULMDs%2FygQOUbTNqJjEOiLpZwYZ2kH4rveS8ruE9LKSayJYQhyTZIRXfs7BFEvyh2kvwq66oBAhrV%2BawLjcd2yW%2FKhAAq%2F0UkyqYUe2Rh%2BgS591XLFQNnaBJ8a29XIGCKw2lNY0cLx5gaaxuWk7bEThrSjYlmTQ%2Bi2VYGSghKsQaaKUcHokVXOqzt91gCwmm3Kau%2F3jal0sa8iwhUiQyMWDIPH%2BP&X-Amz-Signature=c906379147959a2137184064d7df2adc8b2033376b4c136e26f4c1d15c6b4aad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
