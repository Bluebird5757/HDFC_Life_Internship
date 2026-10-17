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
fetched_at: '2026-10-17T02:05:45.745Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S2LZI2RR%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020540Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJGMEQCIG9OdVcdHxmhKZDGnAaPE%2BDio3NmA%2BkQkRo6swopY5lSAiA1SbCl8L1L2qKEItPTDZSdHETiBeO2RYD%2Fj%2B4ctWJkvCqIBAjw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHnNUmxqyXi46N%2FnzKtwDDcwSDk1XUmJBbpjY09rg%2FZV6BtRJ%2BmaI4g2kgbuml9SCsQY2NI0TiJemPWRROdYaEoCVi%2FUhVpXifav32eHJ0pQLJaHjEJr87QsSxDverCRrfnwScSeUjhI57iVXlQhTpp94k%2B9UG54soB1dG4TJIesdNghWLHpswQpMyORantFdeXg4zocP2SNzSyh2Ui2ui3Wn8BZzc5hsGlzGVEbOkYZAicsktRD%2F278NY9O8QqmOgr07iotRlXJrIXxM1sKJUcGFR9fMjZaZPSIoagglYChzMO0QzjOjMLH61SyRyxrj%2BUpP0AE8t7CaqbV7thr7XWADTtadLOz7Kx1BEV9ECI%2BL0S8p5D84Di%2FAkFh%2BJ9ghfL1jNgW7suGT7cR5CrrwL%2BjR%2FXYoQ97J%2BY%2BYDUxhEUn%2FJCAUd9I47VQjegoWWYdrpg8sr%2Bfw31BbF8Iquw9mI9nhr97trg3lHcIM%2FmDWCe4dchJUrc5yK3opMfBRV5RpYTSdtQq9x1NeXOT3bKmsidDSMyqmNeH0FWFIjVy44EQ6yIbbQLecchcGADFm4zf1iuXRiKeUsMcrXkdGJYRKusGc%2FtQ7pT%2BwClBDIaFb5csihYgiuaRDqorTncUbMsMt8uw8WhJFFpquCiYwzeLK1gY6pgFSOFSj0zusxa7OwxbviGACP4nf2Xi02XukqdVbs7rnnlW6VUQCCwcC7%2FdRtFczosObRJLe7Z1MZT8FVJpla3l3DGwGCiwLwJZ1QiXZS1C9cCWxz3ZRDSGHgvx%2BzZZMPoYVszZNCkuBRRwsF1bW3K9Sf8YFe9PyH58PcZQnhlmv92zTaAWMvEUvM5cpQ8511YszFndHUKrqpeS018BTylPshaCNObBW&X-Amz-Signature=ed40fe7bf9b9f4ef5cac191f759b3e38d35b10c586bbc2a48dcfbb286f6c8300&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46662X53D6B%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJHMEUCIFtNyf5nncmvaBFNZNkH9iy%2BvnpYl6hBFfL88IooRoXRAiEA1xTihXkUX4cKYaQdca7gfc6ZN2vPWkN1LyPvgBVjYKwqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOIPUjUTugJJpkSVbCrcA84S5YSO3KVopoGBsijl%2BTAVTu5zQr39VXoo6%2F2X9M067URfrvpHUDXxxvE6Iugy7NQlcI8TGJkJSSgONvNCW1fnDtPrXZlnB6brlVl2ee7GHog4NVdYN0Vv9lmFSefJ8MxycbmW%2Bgdaatf1Vc9cLLmaFdDZXtzEQuI8nrPyletKtseJpTUBe03vgDeT6wBtkLSQ3QHBB2n4Rkin3IDgIxjCld0KsM%2BI3DYRcOqu%2BV%2Bycd0VqNEGtBpuC76qb1JaMMJtmX%2BHdYmgHV%2BupcOq0B7%2FJHqqFQ9YzYXDsCbw68m9SGVL5MraqL%2F6Lm07SLQsw4CViWMjz6DzG4JGyTE2mOxLbBJcN0q%2FxXkDLMoWcP7BToT5IoKr5P5q56JoDVHeFznqDe%2BwYP97JPecNqCTsH9nCKXwOatwgg3jcfAa0qjz4ZYc9PIO5LBfyTk%2BRNIi8gOSzFXNQySmtit0LV2IKJWwyN4oWNKCyjYUNo1IyplZ2Appg1peosimdC%2BXLofbyFJ7JKiNCS%2BIbg6il4cYX5g6C3GE6tc%2FDiUzMbjf6XGuxJm1ykzalMWeaC0BuCTzccKpqt9xT5x8cLYmTT4Gbz43t8Oc7oAnXej%2BHSaD%2F9aiKyMo5tsckcnLwLjnMOLiytYGOqUB5%2B5dQGajVsMXiKKgmvK1u%2FEzlmmw5%2Fb2TBLCWYIHTVd7jNxPzgDzWUhisDNy8sGm56bzogn702xDMqGVkltvj%2BWp5K3GWHCRxekMFb8f2Dfm9R910RHqpIoILQcnjSGkGo5vjWij%2FTAGo9%2BOVZhYXWX%2F0QSRO6au9qR45T297XgM9zf8osjkZaQOc3sDIJVGOK9ZcHw3kI9xX%2FJrSQtqjyhI6Frw&X-Amz-Signature=b87ee291d811a73f4b69f9303fa0c4258e9009471157adf05eb9ed9efd720c19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663O6OGYF3%2F20261017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261017T020542Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIEWVf%2BkxH2VYcc%2Bto2y%2BkjvFqMEDlb6mOsIin%2B8mDwPqAiEAhRBU4K6BCrMkjGms5xjVU2yeA9IIo4OP%2Fs7XhlYdcFIqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNvipHtVNDUMqeZY4yrcA9l8w0dJddYNRTpTj%2BU1PBcrIY6mv75oj2AKlGnV%2BxvSROQQZQwcfZmaqo862Q%2BjpQkq%2FC6fZ%2FVceHLDBjK4i7Oh4FAifkgJqiMYLQ2mBg1JRoGjMIAf3cIwv%2Fr%2FPHH6kOuNBS1rS3tp2Bq6I%2B%2BHJXswR%2BpbC27CskPg%2FaTOY7s3E0KAzHm%2F%2FAGX13emFPNEaPaMP5Dq9MGlQt%2FPb93jruBhlEPRuGRzONtpzTOx5TzV9yL0ARDkwvP%2BdtvpWmPm0mYPuSdbOAEX2l7C6I%2B6QK9hrBuogt%2FWt1kyTQWETND4YevRoie0g4LaGFqnZgTc2hOOGCU0Tl%2F4QYUO81mGDljxeK8mjRo4mWIr8zli9PsLyJoTqUKyLFNFsOpEhhBaEiYSthj2Kbi3qvpnMRk4lNVuN%2FKs4vJb0cNuhqtyyJFYIzMMk2zCs87VsNYYNs8bltehIMEOi%2FHfLhmRyucRLKap95cjRM63zH2fPM9%2B75FK9mTX9v9eOCC%2B8t7luXErsON%2B2k0cX9I1cg49ellYqxaWpICsDxqSmiddL3ibbiVJqCqOj0qGjDrZrvbAUYcJXS3w95jyBtxjwZ%2Be61LWzTo4t6exy22EvDGJSGVx1K8r5HTF3EKZ2PdrrgU%2BMLbgytYGOqUBnUjRqFgLu%2BVMVoVfE%2BDZIvVni7jP%2BdIAbNnaQZKzOg%2BHAClgcmd6LIl8xhPZGTSNOYsXTNKCXQpEGYupgsetimplBoKjp0s%2B8Zn5Z6nU5%2ByRHNy1Rr87%2Bv7W1y7wZNvK0Gqsbj91lh3EQCe4iSbNgRBtQcZ8dOaaYK97uIGIqV3PNLbKJnSonmqmN2xpAHZed7iEvnekZ1s5Qr9yGHu2Bk%2FntAKw&X-Amz-Signature=6da0175f7b8165af5494f0644457f349b9862c06f9375a813dfcdeca8b860637&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
