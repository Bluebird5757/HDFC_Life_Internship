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
fetched_at: '2026-10-16T02:12:15.201Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667WPM6IEL%2F20261016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261016T021211Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJIMEYCIQD4E6%2B%2BlvY4vVBvsl8EQzMkek8AX16YzOis%2FAmxxNKXkAIhAMzulbQNdt9lBBmn9avcPCFY2kpQUht6QenanYGZiweDKogECNr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzgyPp4eCjZxh1ApyMq3AO6qPdHzuT4%2B9yCK866Cs5Hpb1TbGK8FpNMRki9ZDGy9E5xuCqnYqASpfn6dSrfh3osI%2FacD2g47MDL6x1Ss4mNUkv%2Bb6aqawfSv52nMrFrLMVL%2Fi5x1aiyLZgcsvfQcnNfqbJkDEgoCBPpdj%2FbYODjqFg7UHw%2ByDp5twJRJJqID7b4Y2usKOOc55uoZeY8CqNcmgDH9zz4MiHv4aAD08ERiRHdZNc7PVFPl6%2B1Ta3xKqag2HxG9k4oDjSXD8os3kCOw2CQAGSfJy4Oy%2Bx2D1XNKbDR%2FBviZwQXHlU0pzjkWdrrMAucdMUOoUSnVW19mjAKDcrUyLkoRNAjawmN78UOZe3Im8Pgf8IpfljMAJ85rZfh0RZi9BLs1nunXQ0n00c7RCaBJZmqBQmghp%2BZaRhxJHctINnz45oFJksTqaTTmCl6teY0TE%2FSAcbbX2S%2BfCym9%2B%2FDbIf9%2BT%2BzdxPF0Ar%2FZQ1So1ONw8RuFf67qhZsWStP%2FGVlI0CBZPolT696XoIVGs%2FjDSLZYUXWipHsWR7vn%2BxM5kApv5LJjZt6SCHv5hd%2B2ZC3iIMTOjECinYkerRCRFE4uhy5YcVTOcO69P7xP2KWkjxI3azBAXqIzmRbVJucEJM%2Fa1JXhfFekzD168XWBjqkAcxuwrTwKfCbdG%2BBNJiAYR%2F9H4yQynk%2F%2Ft%2FQoYQvNN1bBp4%2BKSy%2B5RfLLtSqAb%2FtvpwXP%2Bt3MKMZPl9uoKZECCjEIadlhhQIc%2BWA4m9JHfgJLKjMsn6ShTvCla01vVL4iBLPj17Rvv4Bp2KS4KHk3OIMOsZzlXjYye%2BYMwzW400O5XJu7ouXenVwZw8hKu3SUA%2BkUJ%2BS1vt8hBESuEXy5FV0%2BG1l&X-Amz-Signature=cca2efe7a121d737a5996b28dbe48eaf6c1a30ec9e75923b31671ab84efc91bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WVQMGFDU%2F20261016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261016T021210Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIEOEo%2BgbqloDlmyraTfcsdoer8Si9g7d9kswYm8M%2BkzZAiBVZYZHDDj0B%2F5wC6O8DlejSxgfbd%2BXKZQZElGLAhxSSSqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMwGyu%2FC1x0Gfu6GW5KtwDMlYPAoMPiISKrUQBhl%2B%2FG1GW86y%2BUVBjm7PEFzzCSLmIGwiGMkTWFFgT%2FL%2FJmJt7SR1dNWSLCbh2ZZfs28Xy1gnzX%2FoCcV%2Bxl30JyHjoV2X%2FlT4t8cfMkxuMVroUzFdpJKmIC0Ac3jGQjN1LKx3Eiv9Ku15GUnt8Duffwku6NXqhGTQDriFongChvHyikumRsrcWsiLrEOUxt2w%2FTF5yqjIw4VVq09hk8Qfr691cUsghsXMLTLD5mFtTikSiASZwG3MLV9YAp4DXGdwZCRP0YCkolV%2FhH%2FNAjn9jlCNnxbjsU2WTeaYZWXswVBGpD88Dt%2BAUppVKWtQuOZGBJHaZR%2BQtv0oSCXFUGEL8bDrJbQ67Sxk6h0EaasfuuOEYJy6SQ3O3BhT%2FMmyVUtOpSfGNMRGCR%2BnAi4GheVPxEwohOjbewn%2Fxttrgxk2BpQzjA8URbRUF5N4qjIP7%2B26nJk5R2TFdAQBCbGapn9pI2QTizaKvvJIqUwpJTeeWASHFhIfjvcMmoMHzxiYqRTXcBkbIfw7XcOxCAqdFDrqrDHoGc%2FXefPeDZliDlPSBCUZX8QsjMAiqzX2UbGJ7eY3YUGoivg%2F0PSTi42yT5Xe%2F9SDCFPnar1xm9cFtjEpW0esw%2BevF1gY6pgEE%2BFPr%2BMXX0W050dedaJrdYhf2aNejBJQeOuX1FVyChaps6vV9U5mjesSGDOtJ91kwtpTAv%2F8rxG4fxUEec3PPNklQbkUEQGjtnoh9endvPuCMZkGP%2FvGUgKOiueNSQKnXFf9yU5yl%2FPFtIVDIuhb1dYq9GrRacg6jUzCbU%2BsLJI24d3%2FIA6ZrqHHSrJATOUR0zpXJoI0jMJ4WNKdzr3LCw1Ws%2B7EB&X-Amz-Signature=dd579132dfeaf01a4f2cc329e40b999f45e36995eb6e5f2b77a1a99835c6f4f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5NNZL7Z%2F20261016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261016T021212Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJIMEYCIQCPfV3TtxBpN8DPJkyxp6otc05tGq9lbrO2NmAaMBwcgAIhANT710o6xCICB6uVdl4zWdRUnsYpVCrm15t1ftNE7httKogECNn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxagQVf%2Fk%2ByiqJAmPsq3AMCZ%2BccjHUxpaGPk6ulMXuEA7QHtwWVJIS7Rkuyq7IY1%2F7ZYQ02jdriymKytTuwPE1hZ2NYLk2f0rhIKOvdgZ1KqZwzAjWvqyQWKUF1VwutLcOA5DFvwUV%2FWTksOhU%2FKDFUSRyGPuU%2FpN564npjc2g5nWWzP3uDXsoFj%2BH%2F0fUoH5hj1R8vEcyM%2BJAAjLwCFRu116k9G42xLmB0bWF535Z6ldbEuSi9aDCtTxcWGWb0Ix2f6%2FPQJ%2BhhMGgd8GkzXf0d1dtcxmXASpg%2B82ByXdyc9ftp%2FrVSSu0mvzCwdvpISf6w2nz6DKvOzGcBuqh8mJowZGSLu8C%2Fhn6DZcGUeE9RZTc6DZCkrixh6dApYDEKGK5K0bZxH0BJcSJ2ChDjGtCFrCvHmODmHuaFXagL2RRYZoRvMTb6YIjRvmwkare01Op1bxSFvMzrf1c8%2FLdTDquLPSvTiLLx1kcUe%2BxOwvP3g69a1afIG9x9fyXdhbsq653T3ZTwDjfqeiOQ6JDfYfcJsGQLqbysDdg2tILfTc1a9dSmOWRhXuP0BXCauMFdz4f8XDDSdMRV7Ro9jncGVJp9ZIm39SdatePeny2dsHV5MZtRMu8PzEHRDSwaqNTk40RfBfLAm0nDftq%2FHzDr68XWBjqkAevXbEvdaW9%2BKzH9X8uYF0xAZNqDigaDLTAm%2BBpc7zaCTViBySsjREWpyd7N1Vroc6UJ0uwsswWEcid6Me1YdFBL1isADKwuvPp%2BmfoPXsnrpXf75kspV9HDVrVvnoVpcEJURAZXkGf%2FBGOLCGq1%2FCdIEsc35lnc0%2B%2F5QBoBnxefvhwaGIAGKqgjUmWZl3gk0GCC7EDIQx6aKOa7Rf%2Bi%2BP3A2fMJ&X-Amz-Signature=47d0e697299b1918dd5150497127d72ca1a6927634ac7735ebac8275e330d067&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
