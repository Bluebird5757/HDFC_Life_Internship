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
fetched_at: '2026-10-22T02:25:39.433Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDJLPS3N%2F20261022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261022T022535Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICGid7JzkYH6SbK716Zr%2FcPPCO3Zfj72F7I6JB%2BIdWBKAiEA5vhMM1UW1VTLUnYG65YoVPAw4VdbyUV3WgwkBScx%2Bpsq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDKajCzCQgxTy4drYBCrcA0jP0HM7eEYjzcbUCXqa%2BHuANO0eRMp%2FAlMMoTTemFNC5x%2Bem1Lv%2B%2FfxNtHEgpeUAVL5aIeSw7wa%2FXH%2BN5Q15kb4XMzUUbTYHCpnCxyppf%2FyG9T2d0xyLoWvzzjoIdPQXz23%2BuW0KWnmfQmii3ACmNMnbbkXljJBmR00staLKA%2FACmY4NWrbDnrjHwR0J2Idnp89q78Cwzxd1eAm9z1P%2FA3LAAS88RZRkOe9NQRdR5soVb4EO%2B5dBcwq%2FDNn5kPnqFglc7%2FjqubCyjDAPSb9fAtf8KPD288P2dgXameiO4joRKB%2FvO4wJgI3LsEiiW7Dpw6p7uEpMJWIabp2%2F%2FZvoD0Le0D8zK1UhHKyk0iC3CAP6E%2BrCkF8CrjtB62iBNrr0D%2B6Wzk8frRkf6MS8NlAkqfUaNvtNUMaCrUTo3dWwCf6CZ3LQO0O%2FotNdUZptbm%2BO3ZsupvPyLH8mvnfIqRK0juzPynrX8t1UEXo5m5ubRgdVNpgX6A3KLGNbEJftlBXUEo%2FdONOEf3jiBqwfwQ9WF7UUODeih5GDa18sveNQRwiwXdTBtWQtCe7RMBQ2W6EkiI0aGQn1qxfgUx4Q0Tz5H29A0pz6LJ%2B%2Fpieskze%2Fp4XBgntXPIYfK42A561MOmc5dYGOqUB%2FUcMlLnEyG3MyvsHGIx6nSIF%2BAMsQMrN%2FEqSYQQU%2F5TNgih2Fbw2vbgDA%2BCDTyWqJ5FKo%2Bx9PecyKNHk1xO4IS69lAHvm%2FRdmOaB7G7TNbI3w6l%2BdSWvgniC0WeM6EIyxiu51Do7FXcvXlYTGjLk2iUiLhcGXFZxJkdcAi2WakwFWiBPR7%2FUf%2Bwnq7BUhrF6PyOvqtsTbGmkbAhcEZpid8%2BUrEkk&X-Amz-Signature=688126fcad5906e1e1e50afcb48e762163ac47106f581d972fadf12ba692ae11&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDTMLDQC%2F20261022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261022T022535Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBFF4x8ajgfaAhUegEb%2FSaz%2FDdCFmrHjl3NifGtXhgkJAiBeY92HE%2BXzOQDIYo%2BDVje%2BFjoqndg9DFXTOTfwTo28fir%2FAwhoEAAaDDYzNzQyMzE4MzgwNSIMvxFHnAcmYq4nHkIKKtwDNId3WmUjl27lEL28N8Xka%2FMlC47os4CuFrrZyj2lVj0QaTeHqJEaPoG5ZsQmngL0Vee6Bdst%2B%2Bx17H7sEOsj7uSyE%2FaVQjumqVBtnfe%2FShkGcResjQl3irs1VyHCWU7U1oFwdST4H8d89l9l8%2BgaXMgWmHZuI3WYRje3ZvNVE1vCfId%2Bc1iO0qGdxJRDCHU4ui7MALBH3GBtnksZTpkrdmJZRFRZWtGnBSBgZZiASZHQj4by9ipqBk8PDhRlF9WBun9qCKBn5dh62VqjS%2BJVAUCQY8LaZwWPNaM4dp%2FffeGzLMoBYrBvEKqBZLSWcP3VeVQ4OXz2WCvJHbC2yd7xKxh5pUXRvjTp4YF2GeQSMCtXmMrdk9eXcCJOUDmBUXohIZahjQMkZbWtMbHk59aSImgo0pcpPAWrSrQztxKHcV0kkFTb4cmUo6LoPAe%2BrBL0BQc8sghSdjzdI064kiUUV4PvE2ytH%2FHS2wvAtIDp0U1Cf4DULoipU%2FOMn3Q5SdqCiGJFT9KWCSaEYb2O3RCTp1yJhqGS77ITVUaK0PxWFfuYgrm2GLEYZXnm3zozXl%2F6CPcFPZjicmjfKHIIn%2F%2FIhjvCK6t1oZE9%2BOUMKCDkBbMhKWUXmvE%2F7v3t3EAw75zl1gY6pgEmuFE9QNTCpXTuE7FLPKAoxdH%2B8%2FU%2BopkiPPbUPz2gPDRaLsDzdyAwI2l3HK9QVfyb1mskA4pu%2Fcp9FhqbY96rcAFfbO4pjNiJT1OR6g4J2wkDG6YLzvZ7nC9HZJjkvShVRr9GpOmnGz1wpUNUG0DWdTdp1%2B32uoBXBxMyECcuGw8PYW2QF4u83uEZFQGwG%2FGUjMfQggbTeXCgvpgV0XECizNa%2F%2BgD&X-Amz-Signature=b721e9672e134391c879725531c69a067832f7572f70f03e9cfb1179574a9c4f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IZTEVVF%2F20261022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261022T022536Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGbpIcd4UjK9tdWpLYB3m7IJziCSIxqQXpCRhgsuhem2AiEA8i7M3sbKhjtn5ZxhPUNJY%2BnaiurZrTX3xK6uAf4Hk1Aq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDFNBRrFkmQKOOWCtCCrcAzG0DIziNoI5iuPsZydfwTPMSJTd0TT5sqQlhiwU4pI2sTqMcLmwL85Vlp69X6Lsml32xlfzk2EBCQ3JKDrybMObxN45oXOPlZYqHPaccx0IOjPq6A6543gL2FkATut6sJh82c37KPNBc6VhDDJByGwUiSd96unDr3tBA%2BnmwqCrn9xFm5Cgq3MG9zVV9jo1ekWHWSnkseL2NF0TE1%2BMUEBuDYMVY5HYfFok9cgMDIaoscbk%2FCD6T3gAZtGncOyz%2FsZN%2BgriaRM0Vnf1QwfNWt3TpoIfl3ZusAn8p2Y3IawAUZVPpA%2FFh%2Bvp7J6PBOgUmuh1Rty18OctHNq9%2FZGPh%2FZ4KxCrjoKqyN9Fjc5wGvi9%2FkIXGqPjmJCNIisnjnt%2BiPs%2FybxKQthl3UO%2BsvgQA3drAu%2BiMkBJ5%2BRYKTebFIScgMpTyAV1lh8LvFwL8WvxTP0UB7AL5dOtFdN5bg%2BkkC7zSLtyqq7AniYl92tLNrBxStf5Joc%2FlemNu7%2BSp2UfDadV6HhTBqxIyKlqgwj9JgbVEsy44Sp0SWEkk4n6q1gtFM3MGftVjEfW0P6n60exDswp7pmP1G%2Fo3%2FBDTHWbPHX3MNUULnblzJ8jNvuq%2ByXYtTXH6Y%2B40iAe1AreMOSe5dYGOqUBNfieS7AHSso7KUG5ZCb7j2UYE9D9%2Fp2ulixrlxIx2DryeUIhhOBoj7EioDZLfP8xW4fdvIrgCMd%2B2cDxeLcPHEMR3wgABeSHbwm%2B1e5zksaoa7i%2BHcd%2F%2FB2SEuBT9V12KtxvJYy%2BeqNJZo5CaWkc1NlDerPnl2y0M80GajZOjvpmFaa4pchCrb8WQLgQ1QGZqyu6Jx%2BmjO%2B1d3Abd0qzXneNPgZQ&X-Amz-Signature=364f51f6d77b08f76de29bb7fe1b2a1e1480517c9ed7d958c2e0c70e309f3a4b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
