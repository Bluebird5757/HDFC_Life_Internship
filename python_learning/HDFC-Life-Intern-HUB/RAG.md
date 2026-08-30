---
notion_id: 3c84fa76-9938-8047-b507-c8020ca82aed
notion_url: https://app.notion.com/p/RAG-3c84fa7699388047b507c8020ca82aed
title: RAG
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-29T12:04:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-08-30T02:22:26.010Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T4ZXYLUO%2F20260830%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260830T022222Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFJI8Elv2d%2BMtowTbeVXGtxhJ3fBivaiwMvIJd3REyKiAiAf0o8JoROIHx5CeU2NZz0019SM6SKW5BVxtM7lZ8d7CSr%2FAwhyEAAaDDYzNzQyMzE4MzgwNSIMsEe16EEyq%2FjJr7cJKtwDSIeVaowg2U0ZJVySbxFW2KxpM1Gh2KH%2BtNyAOJJblBQ1BWlXBpP0eNd2%2BJZ5v%2Bzv9zH0XFaLJLcTQ%2FDFXtTXskDDL9hCkurVs%2Fi75kfiUuO5RDxzyGbK0NBlr5raciXA2v4OT0Q6BeexG%2Bs4rGiqCJfrAu3YLgXvMhvdFA2yXIxdqAzLGybsNDs4f7VFt%2BK1wuZfefaG2qvMC91gWGO5EgMAoNK6iWQhHLtge2co4jPL6x3k1fq2eK8cQ%2F8RoTSFOx1eaUn%2Fmbv%2B1alUgZuvttnlMEUSW8v6gTHXyk4pjxtQvl9R9QbXG9m6Vcbj1BrTgFWeKZguS%2F%2FDjZjCFuybEN3HKCb5V3%2B1dXuunpWs6PrYzyV%2BK%2Fca%2FVf%2FxYiCvZV5hUgqUJmibcVd%2BiaefzQBX6R%2Fmzd6aVjEeez%2B9QoAXfUS2JKJg%2BchTQgHJvGOXgcGZhbHfttAa1ysD%2FaVerq1fnMB8WVeFGcU3999UyaNtnTIEWxrAkGVF4w%2FZ279jwyaCNfIHb5dMrIJRoHaLTMAoIMAjcWzrB5w%2FZMCNqs7xQWCjYVEs9k3bt1J4AjQjcLMh8XtFZKZj%2F3IZrgYmVcD9PsTbHJFAdNYkpxoutM6hzeyVnIsGOljX0qGt5swhofO1AY6pgHTzh9bYXRBt6V7lhomd7Z0mfudzriFhc9N%2FDNJ4j48FADWzk61utjUMqaszRI5Px8wRrYJxs8p%2Fkzjk5F%2BGvIspxmcl09BeO1kUc1Vc2nMTmZxDR2w5JxvfOiGGxNkpWTMWmTxbY2fTZd11r%2BcStJK7BZh6A%2FSCWnHpvble6%2BQhj%2BfIA5TK0DhVKIV2sJGS7JMhoBraiWtwY7XmS2pkItElvJ79hYA&X-Amz-Signature=8d054610ca4eb27246c3e01913c7da323f649622f752585092bd94a6dd5a6374&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBMJLIZS%2F20260830%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260830T022222Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCiLx58HQwbmBJjzzaUqNSyTjB6tv0iXApFHQx9%2Fv6%2BrQIgdhx7qz%2FfQQ8e6o457EWdEvDOBiIOiBNs3qaUP2heeGkq%2FwMIchAAGgw2Mzc0MjMxODM4MDUiDMA8QRgyQftsFSrFaSrcAzxPTZfkCp5DAW%2F4l7Rcs%2Bz8xSUTKAzscid4AN3dGQrN%2BrUWEbwjyacLeB07zg8Ssz9Gmrim4N5nxXMqt3J8YEjFwTDv2igtX7F09LlKAjoqzWUZhHmmzP9%2F%2BHR%2BS8pJlcRhFeAUreMW%2BFcAw9LnxCuzySY09l%2F55oYfjyXtpVxwO1VUOHo2mAC6DUO6ky42fuqh%2B7IOR6snE3RZWkHJ2TGDij%2BzlODCCpWjosWAxJgESYVZuwFUZLgH3O0a5NlaX28JYnNxsffnMkVBIOv25KcPVpwkNZZPz0krBOLnyb4ZGVtQ4Y1gYTKC9FZQn%2Fhnj7S8fWsH8vw5m4oSZn3mjwX9szLjR29p3MT0mZDf9WApideJUlFHh6xt6NcsEc55a5A6DS0nMc5khJdEK2K2jbp%2BJE1BBO%2FzXECQ8IAO%2BAjyRW2QrUJ6SwlEkekH1WehsWw7DkqwOH7XHG%2B8sEmgMpwIIWDfabiRUeg8g2tQWa%2Bjo7OoeBhVpWyOacLhfW2Bv%2BdnzQ5EavLck7bObx8oi%2Bk8cwnW%2Bu16Vw0BewgGSb4p0Dc9sv6%2FBCXUUCHX%2B%2FfhIyjmogGsoZfnpm3H0ySCEviz7Ja8I9KmjJxtyfWhjSKkD9zH7x5XsLIVvBLXMIiHztQGOqUBAPgOR2JTiOoRoAGQPdkJoxLNrcnuqcQtDQkUM6XA0qUjzBaiO8wFN%2Be1qu1h92DpTz0TuO49ZOsP2vSTjzCNsUeuMS7JRs0lhLB0dMSkxduI9nEhpzHE5plzhF2ZgmXUGDDTYyD%2BHbo3u0ttPF1PpeonaYWvvkE3D4k%2FcAoN5c%2BT5bwtDwKq63P86COXnt2wPauNUaoGtYPI1mODalio7j7pvnkE&X-Amz-Signature=839f7376bc6a9c9c2b514a79353758fb70da3812385efcde050ab02409c70e2d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHTB2ZSU%2F20260830%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260830T022223Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICGwTC%2FrDg3RNix5T5KXXgSILZApxD6E30uGr7IcPvv9AiB0MFTbqHtjkcgdh6S9pPOz2GYFIT9rRu0g8jgqu7hWxyr%2FAwhyEAAaDDYzNzQyMzE4MzgwNSIMHfnaU%2F1w9KvRov%2FMKtwDbIwLaNvJ5uvvFjHuvBBh0KliIVDqOT3xK9qeuz1xxnc%2FrqGKjUaLy3NdKJV4r1vWvq%2BFKZxxdy5cDp8xwrysu8oqxs2CWlsBNcYbB8x0TMUZjrZfsJmgZCuj1qTUmcdIyBIMQre%2BGsiCNHIFweXCHTlaqJ04ht9HaExhWbD1MpjoY7Dl7wNdiVofEh8pCbsrBqLNy%2FxnMXRT%2BDMNjfHNoCbWkU1QJNji21JXVLxZI2PDwlCN%2BqF0DMahfQmITChIaGQzCL1rETRFILGZM5WumCrUYXxtVAJUETi9GGZ3vCSe0lWjIKD9UUqs2%2FLuSt4PUzgLsRczDVwRDDtJ5V5H%2BsGi6vMRx9kK5nYffd1mKSkGeHLFc72nO8xsT9kUfra3M1qwYbL4s0a9Pb2RtuQ6iDagq16TucMcU8hF2%2Bi65wOfSbdLKoaru6BH83AFs1d22W7sNndo9%2FAeiO2Hj%2Fa7LrqaTTPUmcAi6FaIXr0acfvEITXlaloh4930ED8Ai7uoPX9SB2s4pFt%2BZlvgPB5itVrHqsATsRlFtvNy7FXMCGLqxiKlsqMHbiXLxiBRQu3so9A9G8oODnJZp2jVVF7A98dt8MHHpLtJ9I%2BHprT7gkJmzyUkHpn6lXuAQmcwiYfO1AY6pgFYyT0zRNCqESIIMBt%2B32EGJDS84nAbJdp49zAumqG19gaVfHrp5iixHCt0QYRcQM75mtbLuYg4U%2BCJCBQyGEHTIkvRiqJ2E%2BH1eANBX5b1GMfEuJDHTwmyMHFmCagppZOkNod%2FVdZBXC6LqWDpbnYNO75AzCwqIwIqw2ezvsCW9W3%2FoMk3FgPIIxFh0XzynIWhAuvwiXnNb2uDbVjjl5hdYfmYwwHj&X-Amz-Signature=76ceb0c00b800135a479e0dce6157c30494717951ee51bfdafcf668b487c1d00&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
