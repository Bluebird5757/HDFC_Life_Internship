---
notion_id: 3c84fa76-9938-8047-b507-c8020ca82aed
notion_url: https://app.notion.com/p/RAG-3c84fa7699388047b507c8020ca82aed
title: RAG
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-28T22:56:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-08-29T04:45:32.176Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466365R4DYV%2F20260829%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260829T044529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCMD%2FrKcZl5gZ8iZtO66Pqxoj45deO8NRvfZAb17DLciAIgeHaeKdOJLKlBZV14gMW3grX8hIxLAOwmyIbrhJY6IQoq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDNoKZr3xLJwOTJ1fSCrcA6JfbDcdvChSbFDBLZambDqyyIhVdLuWsE9YHpWTHgZfPbBHCTeXx0ilQ0nNplfMVxlplcTxu5rjOiNvBnwqFBDEmpxP2oItPqI%2B8iRZS8HO%2Fy63YeCTcOq0RRhI11fHcWzvZ20LSlGjijNo51hVsXP5X57CJYfk4SbPaew6lAW3Syba0fdNJe7GCn1HXHBhbYvVGl%2FN8k9N8JCgd2p0KXVdtmk%2FHNfEMyKUFOSpWd%2Bp1fpEpenoXE3kz8thoUT0%2FMddC7TozPPsxcvsG4uCCtnDpefNytgnAPQecFQERYSx67KzdyVJl9cQiNwpJxHi7yLqaJ1uPKZ640Pu7lFrjN%2Fw4shwE2De2PE5aKPZAVxgLChl%2BwYGRXlvTKzOq8ola4q8DVszqnsjcNaVhjbPP1ey0gN6QzPGxOd219sE16Er2aQ4DCW1DOAPAMPY6zk8%2BM043%2FcrqujxYc8WVCYSiJU5XL%2FZwSBhnmRdxxkSrMROjMHZtJ5%2FgPMXqdJ4RZJLT19FOBn9ORl8UmqELizn%2FVdJlXjQhHXCYhoNIrIJ%2BVk4u97nIJQA%2BkQhrBaegIcBmOv9p5osOt8SF1lNmsKvEksY10vKt3NY8ZnZCvsIBsD4sqE3vuMCIO0ruXBNMPm7ydQGOqUB9dRPghuY4JTP9B%2F%2FNZ%2FkTVt4wGtIizRm74YJSiM%2Bafm%2FXueS1eiG3FkhtUdpNg75Z5Zgfljvn5Ht3ifG5yuF2qiW9AQyceWv4EETXQptRGgNpevVjtzmuZwGnhqcReFL8gVK28bEynr1jYM05x4m8b1OGts7atL%2B3T%2B7jKABZi6G6rjr54vg3J40AWzUjOroYLrB1NUvXPbQz4GVy9O3s%2BOOgMkg&X-Amz-Signature=f6311df1d21383b2785bf9c3dd47fbece706806a73cf6f6a3425ad8a80b7af66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667QIXJ24X%2F20260829%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260829T044528Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID2d34B8%2BxuooUC1r%2FFezdMsPiNlA5dVAq1l7pNDW148AiBh4hD6H21ApadB8bsvXEVdjWdKeqcyWG1T3o9lSYp2wCr%2FAwhdEAAaDDYzNzQyMzE4MzgwNSIM84yDnTmp2DYAU7RvKtwDwwZimVnvP11OFojuBSvDRSuphyX4h79wtHshorMC1xPHdK%2FkBn9R6SrErYuEFURqUMHZ4FAHEHdZZuzs0KeJQWq0yscoJdEDMl6sS8YvD5iwGmo0BapaTi6MkAKhiQ6IwH%2FBl2j%2BCt%2B07W%2FND9cxCbdpmVqG%2FzcGp1mvWppkS8bE6D1j%2FL0g2%2BE1IXNG%2FO35jEsHbkxlsDRh6KadMRxH73IHZ9IOn9dZ4KQ8jlKv6fkeHlOQAsJuIqJVK50vkfvAJwy803rKnZ4pZ6KDRk2%2BcM%2B0hKdGgjiRfqYvk8dg99xb%2BLSj4A9DQK6tJu963ZyNPzlGd%2BQlFChaLKguC2M6rA%2BYAvOYZ%2FaLBj87W3cQXPnpnD4uk%2BTJLvZ6dQNnU97DHkGHPpLW9V%2F6wl8UMRo7rg%2B9uHXOO6a9bvGvlM%2BjlG1fzYMkjBBrWIiFMFI%2FtgXe989n%2BiLyXY0ZSbsoh1EDKTCJD6iqvpLaImmH55%2Bl7l9uc2NeADO0nYyDXmUL2p73Z9px0lFPUBCbfMNiZ%2B53U40gm4eDH%2FbjSAu2B819yEgsA8jBjXIielu7t9YiDCITlv9eH7KmO2BKWWzmsGAKlT883qfnqCjEX2tFRDGJo4iYIUcX6l1uYVGVVtUwg7zJ1AY6pgFfUdUayhPI0%2BNz4pDSKfRuf29xNGlUxByVV9oeKVxMWW5BoeCTITUp5K80jBEUAWCEEvreGSTjtLuDRTxqwjGyW2kyL4%2BgWWHGBgFXhOu3TFRsw0t35KkHUY4cDKBcaw71cP%2FEIp1Dhg7grPE0KzCchTZmfTYX9SwMQVilUUA4%2F8bNba6BIndG9JwDIdS0tmrEaI0Xa7UXOt8ve0skf%2B3TwYNc1CBJ&X-Amz-Signature=4e13ee88741fd2730dfef62040cf3e4eac133e83d1ecf1e40b3aa85a097b01ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VPFKQX2N%2F20260829%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260829T044529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDLm%2FpR7gYvemSpGhrJwquHKgKGP8ZLOR67UIU2AK8vfAiAMqNIivPBFTbVA7%2B8ybBZb0UTXPNeAEccFK52FVaEqxCr%2FAwhdEAAaDDYzNzQyMzE4MzgwNSIMuhZMF3cKqNDqxk56KtwD1gQRgEyOLKqyR98lEv%2BbbMaYQ%2FtoyMB7s7bm%2FMxGo3s5orDH3uCyj8BtGHEt6k%2Bfh9uuB5f4xts5pWJl9WMuyhO0m5PjJuEtTPWJwvTQn2z7qgarpbMeE758HpjqmyQoALySpM8KrT9g%2FwG%2FP6nkMFxPI%2FrsGPKkqPiS2GpVaIYLFICVMB2PRgnmnowT1mrz3PjhkeicOczJw7jMSc9a98WGyvwBzNR5mdNHEyZ%2Bh7TkOsGmqfHplg%2FevIV6I6yszDbj6o4i6wzLsNbzlKlvlEhF9YseaXKKSFQBgRWThAss8FkHf%2B8yVaNHMaewniwOq1T2Yzg9TTtzG0PVDR4VXzjDs0xkNyg0Vjb%2F5sxTCSrckcBdOyhVQb6NdTkaDWTCVmDhQIZi9TZWZ2Uh8isbzLH0ooZqX8cAVa4zxWCpXRCjHuiNjDwzW0cgK%2BOiscYnv6zFOpmT%2FMtrKTsi6NtjmcgFxfC68SZ5CEGZqUHV9SN2CBNAJiaWG0eDRcu9wmfEw1asVsSJjszMnUHDySZ7D473DVsWYRjTEPYprEXnc%2BiG7iQhMrX%2FCeQDXOhGR%2BKXnEuh9hLr8rtkL8wJz%2FXvYAvoqyV7um5Dre%2B%2BG1kXZP%2FerOKr1FXAfQt5txEwtbzJ1AY6pgG28UqIaDFJ5tb7K54fwbS8GeYv4IvhtzSmCb0Ne7VDPS0%2B%2FtqEPo14eZsXe3xGJI8fGV7dGUq0Ng3kl%2FYsLJoaPI%2BaS9qlYk67stFZZ3qJc%2FTtQK4qP%2FLYFKmQZN5IUPqbhnJF14OOXJCSIHEM5q%2FW%2BWS5PsDu%2B1zfYwZb142WrHa0kTBnNnwsX0qlzZlfOf54oEUe9HGo7HDg1fm6meI5%2BIv72Fn9&X-Amz-Signature=50a772568d683906df20f8b3651a250c3b8be687df87d1a21a1a7ed841413bc7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


        link to the code:- [https://colab.research.google.com/drive/1rnMQ0VOcyak2KpNYcmrb30s8wf-ysMGJ?usp=sharing](https://colab.research.google.com/drive/1rnMQ0VOcyak2KpNYcmrb30s8wf-ysMGJ?usp=sharing)

- Retrievers :- a component langchain that fetches relevant documents from a data source in response to a user’s query. There are multiple types of retrievers. All retrievers in LangChain are runnables

    Based on these two things you can create different type of retrievers:-

    - Based on Data Source
        - Wiki retriever :- [https://colab.research.google.com/drive/1K6gqDRcruvp9ADfEFj3DfkUNUqU2ZdXE?usp=sharing](https://colab.research.google.com/drive/1K6gqDRcruvp9ADfEFj3DfkUNUqU2ZdXE?usp=sharing)
        - Vector Store
        - Archive Ret(website research paper ret)
    - Search Strategy
        - MMR:- Reduce Redundancy in the retrieved results while maintaining high relevance to the query
        - Multi Query
        - Contextual
