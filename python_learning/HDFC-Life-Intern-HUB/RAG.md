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
fetched_at: '2026-11-28T05:29:18.110Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666SZ6WKKH%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052913Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJIMEYCIQDrsXbMM%2FsLquXbWuuuiJtFkGkJR1pWq0myYQOF2992fgIhAJBsoy8foWxi5SAMQ3kdWuluX0hTCRG%2B15cLTx%2BEhVZnKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyyKOX5%2BQq8ArRQ9wAq3APvIa03Bi5NkhM0mb7dMgbqi4HNLOuHexUK%2FylWFlf2NH2N194dhSqAMwsvsrF6uzJvXu%2Bb4rrvIymrkgvkjScpPapLtWVp8uhFFGLFi3lYZ7QEO1NSGjwDVTKnXzLCtdwH%2FxzCwWMrL46gNAwWSkjzKuThM3Iyhof6HwUmHeMnhVxC6x5j3ObomfWgy8vmTu0DuNQzlyS90eX4ZHIUMaNaEoioGib7ZOGc7TVf0pBApOpHS9vT67djAFP5NIXPep1S0OhRozR1InDxMpcLRAz58vTjycecUUC%2BRMHfVQ7tmXTUcOckwO6ySbqhoDDWYHipsgIuSG5jgijzL9o56D1Y%2FVOLlRSCVUHuW2vFIAfGmAMc%2BU%2FQ3jooi0eCGlfVM6oPluBRysK22AmNJiH%2BgxWInu8U6Ou%2B3ecxVAgLzZkDZ45LiIRub9Km3TBe3AN3Rf9CEb7mZBqnyUlJ1GcRPIwkvW%2BIPzBTo8Epiwhr2s63du2HXi2SAp0VotAqqiyqEs%2FyuV3DW4xWeY5fIW2Q7e98sJpKX8Fbb%2BKMAhNUZXkS8U7eCBMylFN%2BM9SiLC7iwTQRy31p3WBk%2Brv4usCZ3XW9xGiHCUNgte2tTmxmkW7wEm4B9XypXpuVHOBrDjCj0KjYBjqkAQsYUrV2P5zrOymvfCHVu%2BsFruXeKXWgyU2fRrAv7vEIcJpQ2zbyUA0lVsGK0ATYoMZ8msX1f9qGItkqs9Qc5GxURRv%2BAJCskL8%2BzIHInbbkqJnbjnkMC951xCbeQ90pt35QGz7FnDq5Vs3SoD9L3Fz3sUnsuEB6dimuO4NtsMcucDlysPa0pv9D5f5xMsv3MqUPl6ni6nkHxBzBS4V9nh2%2FcOh3&X-Amz-Signature=57be371ffce4611275354eece3c7bf3294250114726c22687e05cb926ad1824e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RUFFCRYG%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052912Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIQDx2P7J0ipjQfKl%2FuYf7aYI6rpHAINVXl3WhpfK0%2FLd5QIgI5AEay%2B5wezxExEQM6F7ppfU7Q5blcy62Enub88JlY0qiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOAzkO15Uq339KMGISrcA873aPnH%2Ba%2FWWw0aHP4NJ9UQJIUsmbMyi6U5Lzqw40lHvnUtpyXj0b2RX%2BIaPqcNTdONiEzEZCSqKXI%2BU%2F0zG3NHrxsny5FfkPBwYatXVjPdIjOEkBH2RGsXXIHCbi9y4FY21gQLpjFj80PGkT%2BrlUWDL11XrQPFeWoOiYq1jK3HKWRHNQ7hWDNzwoBJg7dD%2F%2B0Ema331ACTqNeSYPPcgW2D92aC%2F7fVCaPt0I%2BDD1%2BB1q8KemAEW2JB2GsR9oH6yHEhYimVTNNKjIhS2GfQyWISCNFvFFsxY3CVgnvB%2BRFeR89Dy1%2B3YWFePyJF4jD5AnGTo%2BdzkTNGkXB7%2Fw0vTxsML9ydEBxYXIc9tDK1r4jNXuxxS77zH8b4Jg7uNxmmzlBKkc7bOYZZR3V9EoZAILCrWWZZ9WXRVtt1wU%2B9rFScw8MMYoupKOoOduLg6Ltqd5vCK%2FsfjiP4O6CqkPvf1oRa%2FKeLwlTuZ02KKqepEQsMLTVg4VKEzwKuKBQl%2FW93HOfQMy4f87IK83Xg4BywW%2Fd794cQiSai3M9oXY6cjOCnns1w%2BsFBOBG%2BA5xN4Ue%2FoMIUtDhktOhbD1P7A%2FuFDeXVRfC4W5vEjM7m%2B0Up%2BFcCaHHMN3UODT%2B9CTdvMJ%2FWqNgGOqUBq7%2Bv7d6T6bI8QF2vAFqF96GSxwjTtJhe%2BXLR%2BZjQwTo1zCeo8%2Fdk32OxomB%2FY1Zg7bW71wqC%2BjZ3bhaXp6Fbmz9rsidBP62IVvK1se1zI2TU5wSGh1cgJIY2pQEXCqORD4T9ebjglcilW6dzuhKL4PQo9MGeJtmPimhgwCmJfNQs498Yizp1CRegnZFkKmCP%2BXXwB2v8GF8DUaUyfkFKIQcplUi4&X-Amz-Signature=751e4c57fddb11b0e8cca8062e7de89b06798c67ce1c9a0152acf5fcd4a093ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XPCLN25Y%2F20261128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261128T052914Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJGMEQCIFj66L1arzk7xPWOjQ2nrOtOIJBipq8TeWGCN3sA2g7MAiBUWnv1fU2sQmHdnAxZnMxklhYRzEKjy73dhVU4sgqDCiqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMqyHDbb%2By%2FE90JR28KtwDKMbWOsFfu5fPlvBKb483F%2BPYSosRDLATkiuxxn2ou80HRL9S6gJIeJtIdo9aEXLcBVrjYATUuSU%2FGkrqCbb3r%2FpNUIq26zOXagVxxKoUvYS71WDaKEr0coHkxIdUcYvATibcvIyS6Hh8Gsy3abKXDXYN7zIo3Ez22VAkn6FSC%2F6BRnSTm3SFCcNGGpp4%2Bec%2BnjHbHAEt5m9kswWN07ZojBGk5WC2MGBoq6CmS%2B5FYJuISewXVHP58X%2Bsp4zFHFDsP0fkfsv7sZj7mq%2BZuHbMbYDNqiw6TmkutBuBLRBSjFpLb8u%2F3OQEPk9jVEjTrH5DpJd1XRdPt86X0GWiRnR2sbiMcH7evCymX91XQP7TrdrzeC%2FEP21FlyHb37YhOu2GErhch9rEJvHDiRxU3Z8vtXdnSK5v01nndYxFzLu%2F61DOS7ZNbHp8V0ZGsB9VugbzAPjkxcDjbAaZsYePZNaaezNNi2w7i8npBkIVU%2FauysHWdrXd7hBQSJ4yGWvSn8dPMHwSX5MI59b32gbqjqeYOjQoJEhNl9OczS6eHOWQfqx2lHOYcTmPx%2B7aDoFv9fzeAto4XeS%2FXfEg1yiNbprUl75jytGw1mZ8MveMl0yoVmAzPLLAsa5NzilJD7gwoNSo2AY6pgHquHvu6nrLY6PNkCZ8J891ch2Zuudkdfvk6gTo2pPwHoenJVy3gHRbmPB%2FhtYFfHGZodctqq34l0Tk3wHXICQ4L0ReNT1Vx75haVV7PPcZ1YleQcsiqHjgUh1jPK90r8rsZnaDKecxRCUGSGZ%2BM0nNv7onfU6aXE%2FFFS50e07tnM%2B50HPDXvqnw2MB1kj8wjJz%2FdjM0khZvLvujwjuHpyWOgottKSW&X-Amz-Signature=eb6c57eaa9dce9083d41100bfe391721d5bd5bf8ee2a06a8ec193e4bed0d82a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
