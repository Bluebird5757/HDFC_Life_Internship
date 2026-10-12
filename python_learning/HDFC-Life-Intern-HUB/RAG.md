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
fetched_at: '2026-10-12T03:15:01.342Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663G7VJLVS%2F20261012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261012T031458Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDkCAhalByKgx0PiT%2Fsnypncqe9yD3x1mKnccOb0bYQBAiEAsFUSDjtqpmG3bPRYNU3Nzsjee%2F9zi81KPwW1lR590HQq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDJJFMkLAaHID8Mxz2yrcA4x9Hc7jBRvZypFgrp%2FYWF3ZKvwXZP74%2BHc5qkZVy1C%2FJGG%2Buk8xiC4dW%2FI5AEiG7BjrYTnbBGi3GALkixgCEBzLjn0tlzYONMSq9LjlpgAUATmv5y2tSCEHIBiSABd9vVkBiGFsT67CdRFI1e2DrCa4pfwytfJiyYvHrFBSvhwpdTaLiwDm1lsKa945ep634QuXVg96jYS0JauId4gupvK8MwpK6ESP7uWsckWIx9jhTJvLcYypXgPBveKmoupEqHmfE%2FFl%2F0ExVCq4C1kAVJTxCij1oJuUlkjFMsXaxvhwxvo%2FBoOqb6JGyYV7iQldA84L%2BjuOz6%2F3D1j2sQ%2BhxY7xkRQ47rTWUbSbsVXA4JDHBFEEjL78vyNBBrmyUtuPW4ey%2BwE8snLJxADUBkkAdkCmoaf96fePR3jlA7ucqKBUcZVXe6OWrDTMlytIgXlRKxrxKMvzrDQicFU625sMZr5vAVStujaqav5oPhjqCyfbiCQYV8znFbfh7TIOVWOTA03G6uWeWG0kpr%2FEZZBAEhTNu1b9%2FLbITbmNjBkfu73Igy%2BvGZySn%2FgvC%2BgsfARmGTgO%2FM5ANepDTpPvJs8pf4O9Cmy2JoCLXRzJcTcvLPnmxIWDhtHsI1k7FrZRMNb1sNYGOqUBbbpJ%2F8TwmxSmBJt%2FPYdDg77XOZAdJVd1ykqfWfwp%2Bq6Paed0bVOOxVQNNTD%2B4dswz2j5yrsuE50rAxDH7%2BG%2FSSz4EI7yFxDECwvTx6S%2B7wxIyFniBn%2BrXTN6jRMeg43Sbl7Xsic8fxr61%2F4Cec8GBT7rapj%2B4Y%2FpxAC6h9pO%2BmRrV40rhuwmFnLhgm4O3pq4a4oQEHBmImZ1niP99V%2FfUvOLtQ%2Bx&X-Amz-Signature=dbc1122c4b1df9f44c7aefc3f55639b08a39cb099a23b5601d7f49c5b8bab6bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662YMRXK4O%2F20261012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261012T031458Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDi6FWzPoyuW0m9Hd%2F2Y3kpfbjJJNyN6bodZDeajxRLogIgbWTmt9%2Ba5FRob%2Bc9En%2F5wUU6gbql3s29DRDSZyudjgsq%2FwMIfBAAGgw2Mzc0MjMxODM4MDUiDMamiVT56AANBFW1qSrcAyFrMqtW71ZMfvUnUWb0m2qSL8gJHOfJ3%2FSodkocOy%2FHkHaPUWmv%2FBJWCBCA3ZKYLaK8PLehuDVsCvsw%2BPToCMfH7dhv3N0t06bilsBPHIFhxJA3UjrCfg1Cps0s%2BcMFrnoiK7YB3WGbHJ78PAXl0wKmjJHsXXIrYRBvns6i2NUXAUk584m3tchzonEpnlPgPBey4zeoHaz1IlZqyEqet4RczZ8vaBmK2Y008YX7gJTa%2BHWvS9HyAMvhlNUL1M0sZcf4PkmPz8k%2BL2qKzquzIxEwlqQVY%2FW6bQRPPzfow45nIt%2FwQUcFj3s0gUjuJsqf2QRq6dgkc9sgiL7adMPu9unKpifs9gJNWbkcVXCQ0WLBcZ0sa%2BJWFEr5F1%2FqBepzK2%2Bqkm59xSoeHXNCR0%2BWJi3sG7uUZrU%2FngW611%2F9DuuJ%2Bu%2BHVLp2BjgxT%2FFRC6hvHEQjzLRel8K%2By66gCeVTz8gB9L31OmYpvc6ARYqltWd11bDu0As4eMhx%2FDQhXLLkgGlQJUPGM3syeiN2UsIcLleMj%2BhCmngRG%2BzNjv%2FM3Rau%2F4cutScpCJ0kOj%2FsNdbx0RKVDYD4H%2FGbJQNamKFo3h1aY0kXxMK3VJDr77j%2FZ54PwUZ7r9NCg9XYhM3pMPqXsdYGOqUBqtEfzgFCdwXlAFGAobqGF8aD6KGUbu9q3V%2BZFz7y6IvUPV7aHSWvWblJ74lNAGCizGYOlPXLTu1z8mLcY4gM3zd0pZIRSE6XXNtDCaS50nY1uEDW8zOM8kT89teepL6RpyL4I%2Fiv1dQ8%2FB4HGp5ZPVaReUFR3GtOw3dMzc9%2ByncnzHNtJCTIxNNYxBgfjBnqX1ZOCQczU3WcQO3wCA8yIT4BZ02i&X-Amz-Signature=aa3b84983399aa370e647fc53857f7b635f73b53a55fcf35c04227e61dc53576&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZS3OZIZ%2F20261012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261012T031459Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB85h7B0Opms1LlIlHjAtw%2BU1xk1WEAHt28S%2BWHmLR%2FCAiEA3jONxuvlOBdB1ULYYkoAYrD%2FOxfQK8v5qCrzGedUB78q%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDDe3dqYpOak%2FgNNIqyrcA7tmgto%2BSn5vDFDcxS3vlevYsl8k9RlIQCBeyq98kg0KGLrLPmOd3yZDuK%2Fig%2B1RVARlS%2Bfg9t6FY5WkFYlw%2BtgRwb4UKGFUPdekzPAyE7s3hIuZ7gC%2FtM20W3V%2B6cwcLhgqNVijd6%2F1XqYvia3yA1Y2bfxSx%2FXKwevFZDW%2BSfmar7HTWNLUOROySDaYjjUxloAGFBTZS5JU%2B13DYAhzva4gfO76D%2FiUq%2BkZ8pryjAk0Hec5lMCCwXVZmKj2yCNgFbPsM3sPbpFxDPwz1I2ZtuA11iEuUIufVKPgCs2JvoLFYH4Eb1e1%2BDFj%2BhQ%2FexlamzrlNDn5zeYMWoEGmZumfir%2FWvlUWvmvIeGOXjjL66QR1TzN4ul6Fg356rNtH6%2FE9H7WyutF5OLFCebRke%2B411v8uL0JxcwE2oRV6m3Md4kPjEFz00gnuJF5p%2Fl8tJvl48P3zgYFGigr%2FgScolGZhObDVwkyyBnKExd%2Fm9CpkisE%2BCSl4ectroEufH%2BccvvKRHVnqjorfn4Fa7hA1hFGYKBQRL1RuGQLoTYbuvGmiJENEpor51cN%2Fz3atwkuqiTU0wFdzqkPiBsdCHw%2BCpKl7HzV8cJF%2BqCQz7HuRoRprQzw4ojeyT%2BofdKFlHYnMICCsdYGOqUBThCKn5HfWDUMkMUZyCdwRFkaE0FG1bXRReLXRaRNWPjYcxuzhgGBSPLfvx2xSCbFhszUUSOYnyY%2BY%2F8mcMyYPn%2FMVkh21DJECcACDcNbqO%2B9rVjt2fjyhxuLNGgS1A7PQOxMfhrTu%2BFniO9rhVJHQl6uccNzncbzxZg7I3rc4YsTntJsBbpn6Y9czrItE9nKDAzPo%2BmwM80AL1%2BV38dTenwUPA69&X-Amz-Signature=d1580a1fa32145eace16206770dd3c08c9e6d910da93d6ea0e543cfb92923b0e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
