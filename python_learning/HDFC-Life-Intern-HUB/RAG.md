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
fetched_at: '2026-11-23T04:43:07.015Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YFD2F4QM%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044303Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF2hVJR0dyAGK6YI2Z4TRIU9NKbPlM1oVSUl8b%2FaIM22AiEA9vINWs5ZbVoJ3s%2FkBz%2BDQZ%2FLjGovW7XWcHTL4798COkq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDPHIw4cuvJ3bSAMtQyrcA6w6u22IL2DHVzGdDvUr%2BGREPnXe09iu7KKX3D1sNJEVulYArUhr3sV5wzV%2FVs2nBE7FWX%2FX83iG8jlGv%2FlEhgtdacIjGnnOpQBnUbKW1C2q70d6EfMcbumGHvK%2BXj%2B%2BPe6WEGINz2yH8BVPenX93%2FzT%2FJ2hGukbxFqQMLCHmh%2FWSgjjjSQZbCD6p7R8fVUJJwH0YFBomsiaJPQIGBzTLCDSg7i4ahm%2FTRI%2BsyFZoaZCLwwkU8%2FkU%2FrFFe%2Fh2QRG%2FbCH%2B2OKDmxz66eL3wqUtaj1KWPMQh55WWZg8pcrx3sioqiWGoLxYSq40q8zYrK9fESawwLKY9%2FWRIZtPYo8u4zfLDTzSg2qjoI1EWdj%2FJivlL2valV34igV5N7N7ZmigfSkVMcwE0lZt9g2GHcREai%2Bi00tRfx4qj9QQZqaE%2BR3pZPQkY8jwembdYe29WAvWpl5CcCuX9u%2B0B2oYzNsUTnIDLHhCsABzNtO%2Fq0OQ%2BGvLXOjQI9Rq6pKAb3J0qjUY8s5aMdg%2FRThaH9pz5VMGwl0a80TPXnWSKPMReUjmi5UxTJeytw2LFeRHVLkNorhk5VeEWTcw2bMgE1uG%2BlOxwEmX%2FWbLyuGZZUxbghwi6UkAZa2L6IDJiYkvl1mMOX5jtgGOqUBpLo2Hs0CQaB3J%2FuweUI2o24bHZ6QP1YB%2B7jk3eLVF5AjAyJsI03w6Ukk3S5k8QWCwpn8mUnz5gYMscZ%2FAm1ej27D6c0vXWjI8iRF2Z5R4wcW5yDlDp76GnupgUCO7Qf65iSI%2BcB37pPKNkijUWWH6LWa7wOA4m%2BY9VCRde3s066fmepcQX8m13sBPy9w%2FB%2BXBnMjAtRREt7eEBuyrBSyQZLEH3b4&X-Amz-Signature=78be1b4d3d45c26558a563b96e7564ec58c294e85635168f5cf0712b60d500db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKSTF3BT%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044302Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDp3vY0Ju6ceCHeFWwNwyqnWKTixxQHKK%2FfncDAUOIOxwIgWt4lKvfi8tyYXwfoPLzO9aYpKZGjlwmI6a3hA8Nqykoq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDCCU%2F7RnrBhiVn%2BzdSrcA2ZaeX5Ke8r5p2aOn%2FL6vuOTkfL42BWZV%2FyaZNyJ594VHTbqId3p049oVORwfTgoEgtCizvnDU9WkRafeixP6jq1lAjUCmkfv7Vgl4z6mFjcXAsPvLbWGqzrzaPmREAszivaitRKxJGOQToTk9eLJO4Z27JygVcC4BPWYiU02FcIEWjNJHiiifgKUq66vvq3SRbq%2Fg0f2yT9w6r63WS%2BTFjJkndgOAJAaA8t61P8f3rKI%2Fan0aECUNgSs33aIyAYcKcN%2FsEJQ34qGuo%2BCaFGpTValRmJN899FLOiVAYsadTitq2z8pMCULjHnOsc038ywNT35PsCuyacbybazq%2F0uSzdPJjesF6PRrKDU5%2Buo2OGJBJMHlpWSnvvM0PrscsaRz0YburWjfZsfnEt7XWc1UwZy3JMeexVaRMIJxZhcQuvljA07gVNWslhTDWs5%2FRRv5jdlCrEJLudW8R2bwR9W6QGkH0R%2B757D%2FtgPyKT3NC49O38aG7vmqPnNV9A6QqmW9kGZB2tKIFeW67A7%2F1XaZbJlB46NoRLr062NvM2nx3zLkCtjXvYR0to2Qs52Iw2wN1dWuXgvmcIwIcfLVWhx8n%2BEUz%2FWacMSfmeSaPR%2Bs59%2BZ35OuSjs%2BvtqHybMOT5jtgGOqUB9HPJH61D19FbaoN0DBIVZDAVC87oJzZrkBd7jv23qGMHmWUYT3brxGCYi5M4h%2F2Iz908qyThYY64Rgdf6b%2BLacpys5vQVGv0%2BLhAfZ2LD%2BFY9RMRKVIIePWTVm42gcss8eCtnUCqncDLe00Ub71QZk6s8iIqixuoTZpkrldrDJclJx5%2Br%2Fxid4FjQNidIw0slmMfYJXeVcwVzek%2BEXvTRChiWDSh&X-Amz-Signature=6895ab6f2c051437a5e8ca1303388ba3774cc2c25cf5dc5e79b83d38cefc206c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YZTE4UYL%2F20261123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261123T044304Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICvbMeoM3JF7vlK1NDBnc0jxpItjkqqYHvOSmiq3g4JlAiBmHpbFJImqCuK%2BNurTu%2BL6DgEx3Bbm7m7ujy90iJyKwyr%2FAwhtEAAaDDYzNzQyMzE4MzgwNSIMlMzclXjmuAvSyDZ0KtwDfPmdEFZyFO0kq22J4nSz0A11flGqXhuK6eWHkoMjoz2D4NTCrKHmCib1fn5g3yq0iWG3dlMflQufCNxv7nYalptnowShAGi8xIPJH9r6jHDcwmkFd3YLMpVEz%2Bfa9m%2BaQhbSKnmiY%2Btn%2F%2F%2FfWGAzJPOxTRRp293pQdaeR9RSTNjVOGpCI5kvCAcL90AFjr9G3mIRRz5RubRYvhOukM57%2BEewq6BX2D6KDEpWuXLsdKSm%2B6d8o48v%2Bk3dwFThf9FkSzKJ%2FRCHclYnMy7pW19NMNAiCd42Dk1vXl4sPH8zH9%2BnBbZViwdVMQsr6XR8LQfWoTwS6MhmydFgaJ1nQZmbAqLWrqEohnBmWcbDOb0qlS1N69Kn4BwUlItCUYqTJmULfEQnQ4UlXb%2Fc1ODIH7Snl38uzFpTgzqBd1JZ0WH5HX4eW6oDWC6zf9MVXIR9%2BeV%2BwLZCdtaQs9ODG9bQuFW7zMgjsNcLD%2FoaM9n48Fs%2B%2Bk0mgFoijopgx0mwJFeFDL2iE2DggC1bmvMdJyAe%2Bi412PB5w%2BTmYV4%2FRX5bVOJ47JvWcX08CS3%2FEPzbrdTvh7tHn%2FoTNGV5j%2BE14uQcKQI4%2BIDNLYjinFZvfUrPHj54RjzPiJrCIF4yaWrkBu4w9fuO2AY6pgFDAVNE8gLzevYee0k1B6mmnTjulBOYUsxBTgpTVt1vjSoUvZyZ0eQ0Boxg%2BTlBUMJkDoRRcfz%2FbWnDy1ocayYRh6MFwL%2F04YjJXsBrRbSClI4NBJg%2B2pXtm7Gf5kTgQ6bOY%2FAcMfLCmcVtcHNVKOczJt7s1qqaP4qzUV9eVe4dqTBlbEM7YsBZCFrp8CXEkrp9MX%2BO4NLu%2BV8s3rGRTdKKEbNu789s&X-Amz-Signature=eb8fbb4a0d6142b3b922b2ca357f31871e8dc90a783f117dea63cb919a91e2fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
