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
fetched_at: '2026-10-06T03:48:47.518Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ROE3UKB%2F20261006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261006T034843Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJHMEUCID%2BYRcHs5%2BU%2FBgcFfsV9qXwpVnUsZhjtKA2A0ReyuC8hAiEA%2BbRaEZKPsiwlYr%2BbolXXBnZ5xdQMvl%2F1UiiOodTnSpYqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFPOALNNoJvnl1%2FKvSrcAwsOnLlUo7oUSPFrKPWD6mY1W3H93pT7nEmte2%2BoN3BSatIUtL9jrg5lfdZwS91BxpuOAbAkgHcI%2FGmyltghKJMMAvArYkzLjU69JAlNxFtGPxbQzr66JpeOLEVa%2BsBLBJDHyNn5k%2FfEmQ7Z0w%2BUREI7UPaGWy8gWoyqeObe1BUq325utJxery0UOkIAcTHbjOQjh45s9hFCusjCUC6NX0foimdiwpFARNJ1Y6mJJ02v6wjtnlm11g2Vxnk2TVAlKLuAUlin0rebLlt87e8boN7Z7pe8O7nbS0yU0Zs%2FoniTfTuTsrMblcFK7tF8LrgJpH16U9qGa1OJdFvCXlnMkCB4KD2%2FsBPJRTiK8iKTsrJpog6khCT3CHmBuu27kKfMx4EcFx7KuHRgCeAtH10viudX5WkaRZZ1yjajuvsr7hanFOxVBCBGf%2FCpF6f98G4AHYJ3rANIg%2FkUF3fWGJnr9hL3ect5boWZbk2aYfKa4RPSfh%2Fpqjn7cPFEjraq4oYxA9hsmvGP4GfsEnuvxj9C08CTftgnqTAa2QgY%2BOfT6soAs6rdR513Dyz%2B5%2FAijFqnfZqFiYB3%2FHFMs%2BEA3ztmMiWNpphHor8qdoQHEQkncqgNQk6sGuogPoDJEcJ9MNnakdYGOqUBSvG8Cl5dYitxLtMdy4Fp2m0r8nF%2FMk5YJosocLMHu552r8tAtAFh6su8puG85OEn%2BiuSAJXhSJvt0qszGC7621oupTXVJ%2B18CkGdyqGzhAP3nCz6t7Ws%2BSlqI0uLIE24R%2BiLMV9iOrtd0ZmdiLDc%2Bx7k1svJ9HIPlDMkptbuu2j71yL9Ccm36jL8A%2Bn3VDZDiTjNe%2Bn69i52vmX6wcq0WtcGKOQC&X-Amz-Signature=91f9601bd649ecb0834eec5f7b51bfa7c56b2a6fe3d32a005b12a66e745b7725&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMSXTBWG%2F20261006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261006T034843Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJHMEUCIQCuKN3cQkY8K7ELeO%2FtboQAs6vSSSQpaMzIKrOjBcjgDAIgctWPNAsbpSTIyle7kWDFmXSEfwM4UfQ76cf6xjI4E1sqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFtYg4h2F2JBewjSjCrcA%2FoVV0ZQUpLop7jND8Lsmiib7gPdT5IMM%2BfM247N9AVUkF4dUHM%2BA9s2QWyTObqDHOlghVtuDmM6rr4trqov6Ts2Ctysin0mz%2F2FmfCbK%2B%2BLDMOTcNUFnYqOGtrm6n2sLPD2pZiKXSAWm6BEWJpqlrqwGAQgZ02JPVSKmY7tLfB6LmU%2Ba9%2BxSYhRQYXW2gbHs0WBEUlTBQmcr3wXOkOhbdjVTJcJQzfkmXaGbhQRVLbKfv7zTrwPOWeharjvRYS1wS1%2BhPQtswZEGMWTIO8MCrSwpFMS8Rt9Al%2B3oyRKKFyJh2tD9lH1wahtqbtNdOPIsbQ739zE3vkmA8yYhwqS8YeNKNRfI0WgisCzA7NzPJn59FaSOzxuMRc%2Bz3xuD%2FXhjLSRBlGiuBrUwQf2sPiUGRaXfUlB1Z%2FNpfw3vSWH8dVnSbrWXh4aqL165aVPkyC9rEifBUHFYkEGZT6FUCxggZGcsRZ1iITq%2FkVm6i8RY%2BjIDbdtDn0%2Fp%2B%2FEOM2NjsVQcvnyRebgxIRsITduDDlvQVtC1CxrfFY67wfcpSjc40anywp3ArHiEV8yf0Se8USqBPJgiPnc0hzz3X7mtcvSPQLw64%2BKP9gHnGKWXvwISLLCraPtQUAZUiXj%2FPEEMN3YkdYGOqUBXLHv8%2F0gViY7d51Uvvlx4b58LvpvaMXoStoYV0mfyI7ll1K0KySxutx8UEgWxq7R%2BTYcOsYO%2BPRyc0neuqhb%2FOCL%2Fmi0Cq7suu0dPWOUQnKT1B3hDao27f2DujSe%2Bkyr%2FZuFpCaVZiS7V7B4bzsGthBdAVQFkhk9k2B6zVHAaTJvYglKogTGH%2FdAALs1hnEvIIjyURwoTqERaLO6wATU0l5GIMWx&X-Amz-Signature=558d8ee9cf1917bfd63c07612801ce2ab476dff459818060f4e76d53367a0089&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBAWKLXS%2F20261006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261006T034844Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCIFydM%2FN7J6HSXgfP015d7ouJQBUmSvnN2MyUAkw4Cc4UAiAhP7crwqjy9wh97VLd%2FdXngDc1F3yY2D1d3Ps8kuiwoyqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMnJ0wOU1tF7Er%2FHnqKtwDhIQkGEzpRV6iESqa8UVI6IOMuJPAo%2FeDV79VuVHtR%2F6nOLD%2F9oK4gMhsdorNdmdNyLQsfcc5RfXT%2FQDyd8zovLY4dcAcLglD6z%2Fy%2BOABttYYtw9XBMzJb1PORnd5Hgh3%2ByYawrIN1H7VhHScg2PTxeEBxLiXqrIJTBJjvG0%2BuUQGgSTA4gNAWpWWSZ0SVI1ey7r5zBWC4ThLBCO60d2tggsDsitBvDtGrzDTWD7CU%2FnyLNY4xBZsyw2MBa%2Bhtok6F7pV6TdaMXRNikfb6POkujPGkPc%2BgCMsJO7v%2FRKxazXGNVmFy0qFmSJK5CgGKdOLcFcw%2FkzfcKYsvTBVhDGI2V3F7Y%2F5al13%2BKhzRPBMRh%2F4k4yTBGSOb5RCFK2AUYb8hkVp1OF8cAf%2FNJ7qY36cf%2B6SQvd26CIE%2FIipaFW7huYwFbGd%2FieA6yzqz1IUun8p4fWGXKQ0cSX5UuaPVn192npb0Qj8H1bEAEGfoMdSyi8nRYgef5OqVd0V2h1vLm30izskfMH%2FsXdWGq15KaSey6tycyoQLkc9rzrJZ5aJtgPMKxM5ty03x6cTazriEz9ffrHbOcWCci2SJJsXfIZlHNZhw1y%2FEgyyzemfLtLQqiu42n%2FFegstJfvA0SswgtuR1gY6pgF6sAorQDL1q2H0vULXnVUT6rrNbBSQZp4QzFo1%2FLUZR69RSruxPP9zTdGi%2FFD4NrPWGhlD2AA9HZUnvolTVqqyhoHR6ww8ZTMK3pE3WhGRttsTr84kKEXJ1zqQDRbV7Fibn2vgWKcJ0m60NPNPmoO%2BTFYyX4%2FVkpVODxBHDoiOfaAZ7sht7mHZlwTTQBhVxttcKZ%2FaTXVR7%2F3%2BAD%2BVCLjIj3Neiupp&X-Amz-Signature=fc5893463c3f06556af47aa0dce4740360dddf2805a9a789ad38945548bfcaec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
