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
fetched_at: '2026-10-26T02:16:20.106Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YZP3445F%2F20261026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261026T021616Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDQCcgdW3dqpzc8bzOY2DmuIvZzdLe5%2BXPDLC0LfcnQAgIhAOxyrjh26mkbfLi5d76WAknFrv8CQunm3lQvGwWKFbBxKogECMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwo3yUCO1JAJHKZwL0q3ANFEbnXeReSJb%2F6mtyZdV9TyZrMDBPeW%2BMkKol67YaS0Y485kTLBjpTAdwI1chKLOq0NTtvtzUfj%2B6b9vGx7hriVsFviIfvyLTdq4U6vlY7K%2F%2B8Q4Q23n31LXZNB4yS2vYKozWdvVFeBwn7rgA3y80lxDI7FwSBU7lXDgq%2Fyz%2Fjj2UmtJEeQqSeUyeFESGa5ru4hk1da1OSGds6HSj%2FM6%2F5w1i9AFCO%2FJO6vFLfh4kjf%2F8ffN3rqzsH4zPxNJfhakv3IcCYTw6ewyR8AtgcD1A927qUsASkzFu08priBbQCD4Lg6uWiT72Ws4OxO6Dg5Fkut9R%2BtPK0MP0MgCe%2FgXtcaS7m5SncniyKj461%2BKDREHafSfR4WF56dpuTI0FJYk2INLWWhcnJPWRZvHicQF46q9HAqIGfX75w3RCuaeHf1oQsq8%2BxBX%2F6lMmHLvrEb7CJG6Owkfje9kLwXlESu34pltQ7KIhTV8bftBmzjmPd8QBX4GAS9pPpQDWC%2Fi5KSBtp%2BEZBy0UcUROaUjedAuWUU0nxxwcxjW5czpeYysbYmt0yd4VxBtz10Xxhk9%2Ba2tgLQUdrcQP7qHmpujeuJWXUQGPqH%2FWDJuMniB5Lfrg7rbkH%2Fdx5JqG1H05CnDDQxPrWBjqkAVwpKsTBtJLduFoiofMMFG7%2F26%2FYuVpL%2BHRO6zcz2VQUT4bAg3iMwwk9Wxmncc5%2B19sqPpTlEJE1Ni%2BAUb41IAgUqygumkTwxAz4OVY7NNDhiv7s5rxaCxpIC4JBQTJ6uqc4%2B8lC8gn0kdY5DVIvaL7Y3mNXFgZ0X%2Fvq2xpUP%2FdnH0v05F8n56jGP1Ax%2F0KTW4PZliz1%2FLGqlT0VQK98ni6%2Fx9%2Bo&X-Amz-Signature=b26f485ee9137435550eb4f31be886427280c7a01ddcba29d6f6017419045401&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3R4ZUWQ%2F20261026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261026T021615Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIQDAqAY8%2FQ46oPw3uzRZ%2BD%2FnbOcNbxjhVQoRflAFhbbXQgIgdEz2tq7hrlZfEUaJF%2FYrqRu2n3d3mkGUBeriLtqixPEqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIHVjByJNz8YAo7dzSrcA68rsZZ%2Fx5PO4I4YCW4ld74EZjUft0SY0oLBodUNlEvpWp57E8gCiTKfOk0U3YvwQbfXtPFhlQQBJa63sisb%2BiQ%2Fr71JqTryeZx7Si0FQ5GPesDXcFEXHCNt2yXC2nQtTcJR5B5Hc0aamwhtU5YuqWPG9CzljaEWzsQnGvyUEOPcW5aEhpCrbq7Xq0gYh96rfF0UqeeR4XmSyXceX5Qb%2BMZSmctgpj6dxktoBw1nu0OGnjrYEe8NzhjeUtRSD6FoOwLMVXkOM9oEVmD9kn7MDHOvY7mTwM43t18MZSQ%2F9hqsTxdI7SUdD6yIPa%2FBY4o6m019DefdxRetF%2B13daQy4hPXO%2BtRobVEg%2BeqT1ZNEXiiHgFEf%2B%2BF4C2o0pukO7EnJB6vFULFfp2HnRhu%2Frvh8mWHz0ry19PjZ4hw3S02P3VsA%2BmdTfhleJV6KiyajHVRnP4wAcoaWmd4qBbtnqg%2FirbUOPICcVJvZdWVOZ1MEq48DMOgEc0yTWYo1cax9zyeI%2BSpjVM1fLbQqDhdSeZIk8TWjZgdXe%2B%2FTAcSSfjT6rlXt1iYByaMr9YzFWJ6FRmABbxIdPNy5SmmvotI05B0L5HHSICt6wWUuMHhLlA2mgKRRzlO1IgSqBvoTq1nMJ%2FE%2BtYGOqUBF%2B697f2JU08UBGAqPCQ3gnuxq2dXrwIYacZ1hLIysdOOqCKnNeHCCQ%2FolzNShv47SYJrPGdNHU85OoNoUQIs9wF2acoyf6oDbwiPo8glGefSp7ldAN8E2BCgn1IN9NNJl%2Fgke9sn5TJ5R8LoRHdRAR95OlV8UO4xL4Ft0WLcF2ykJAXg9CQUTyUC9%2BNEsYHyIaX14yapMK3J8hAcBc0g1SbEErPZ&X-Amz-Signature=29d798e5eacd93d17b98e8a486f3e5fbd5a7def24c4c1421b12f8c89b3430064&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YCH5THC6%2F20261026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261026T021617Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIQCNSrMvwpVPW2oqrA4NhGShSz5J0NVDhu8xqm2g7J5RLQIgR6whxgx%2FZzB3nt3FSBKoR4mNRS4mvNeAYGmGf%2BEids4qiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBAWZTkICp4xQmGTFyrcA55NDbKajLC%2F0i%2BCvNvC6%2B0VrWBDBa3ruRpjWFANKJ0r1K4K2PNHnpbH%2F%2F8KVGd657XYM%2Bwq%2BLSrYZyW6EzrDdRPi3AN8fdSBpiWzAGEL6Aq6oUeSBzDKwtICZY3HNts5ZKySBp6nDgUpQHpLWBbG%2FsdpUBF1AS3DxidtZ3i6q3qm%2FFD2J9LU6HiPinFu8tiRqWv3Lv4N8CIw%2FpL6VebcRzLb6KZhDEPdA9h3BS5powWzTAxc6TTWxFOW8dqMJdwsITu%2FJXxEY5q5zuLYknBQOHQyCV0pWvIuI3BhrYIJaMmwd%2BHs7GCtLNjUMwLENp3lx57woE6RfM4ndsE92y4wIvAJUXn2Jfxg7Z8fRYr5Z9hpbJ3fUyJrdGmGT%2Bgzl9SDo27Ma20%2F%2BD3ty9AVQsl2JjO3fqUVV6GJu%2FkKAOPyM%2FUppeUYjYchfOIeMTqCP2Wp4u%2BFi3q5UA99H1BTmF08Zqw7FJxIhDXdRtxij%2B5nyMhY1ZZYcM5ISLgiMrrp3N8Xw8D2UN8gTcBoJZQZj2zbL7oDGBLEb83BHnYam%2BTitdTd5Ug%2B9CfAwfjiD7kB6U%2Fl5GQDGJBGNhwTysBwmey%2FB%2BptXTkZuGfIhud0HNaytUCS%2FeAhkmePUOzMccqMKbE%2BtYGOqUBOHpaLPgMfZGzAkvLL3OT8NGD8Epx%2BT2yl37UhJDJCZPMgceRsGwGimgwi8b0QKyZgaPRFr6OhBmliJczI5zBjBehAfY9oyPixxuPkoiLMPRSEnVveMVEPyF6fcxXIPQ60mOuMb8ZFzsre1ix3tdF081PR1x0UdULvARGgqw4F8V7jSjnufTLJ4hY2BNYrqkBJ%2BR3mFJRA%2FHuWv0RdNJZKBfGEAOV&X-Amz-Signature=ab858f841f05df2741df9a0ec440aeb05f5363b2665d3d6bc2832eb7cde7efff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
