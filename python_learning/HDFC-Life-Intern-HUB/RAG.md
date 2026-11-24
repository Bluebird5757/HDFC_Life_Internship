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
fetched_at: '2026-11-24T07:33:55.572Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNV3656C%2F20261124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261124T073351Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCFougLIVN%2FjeOiHHhnT1N2CTr%2BXWequvYaQ07pXW7vNwIgBh1RI%2FsRBo3buk9qs5oFBTGkETzRHQdRqdkexUsIwQgqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLwZZ1NMhCLyM79UjCrcA1osweTcFIBQuOCDV%2BJFeIR5WHDAuCe9zcihCOtajMPWO127XMc4LG3PnOEwxFMpH3aiq0MjU8QdJiVA%2B7LYwAE1zpZhcZGz%2FKpEp3Ck6iE6%2FtqFC3U2hulIO5%2Bw9w2chGUBtyNT9%2FncFBhqoFYEluUEsLNH%2BORlYpTlmJp9eWme9lg8eF10KOnvJXTANYtp6n7VmdZG0QGMLJwZZZn3Ez1s59h7tOvBfOF4rMfzwyO%2BzKU5ZEhk9qQ%2B2qg05ZDmUSGwROEpU7BDjR5Rm8Rxpzmzh0ktbe1FTMEUrICiEg7qF%2B4N5TdfwpMexwgwXpUnaUfyBZf5OPch2jgYcaMQT3iwtN6jg%2BLLV%2FzprNoOoVmMVCHpZ8Sod3uT8vU6PsmrVndokPz07i6wnlKZNWaR4EuPHbAwz9bIEklmo%2BJYo3IOzSKP1BqQkTog%2FeRVatmGpAdZZkOYXkAFa7yN0K9Uo8m0LdbsdHnSsCUZ26Fc50T7%2BTVNIJLu0Nqah8zNLO3ijETGuy4IRDAXI%2BNhoGcwP4j7bZ3fTvWPWcXQEgpHXD25RGzbLjaB5XjkrGsik1nt4%2BZs4PHOpZIIRymQEZTja2amW1d7aUBd5Jv6%2B6%2FIS9hXEOiCSllaAcQUr52BMM3DlNgGOqUBJ53axNspXCxKA71ESU%2BL9aJp5dCQebcPl7E4Djog5TOVEr76HdXunHSbGHFJijpX1LRkWkSGaYnSqiSbfTFlXNq6Shpn%2FZ%2Bljxo25dbw5dcg%2BGQao1ufyTxF7RyNdNK%2FkR0FvjcVnyB8jVpm55G4pWeo3iBqkp0IdmjJKiT27TO%2BCme0ZsxRMAgryj9vCCoatgs7bD49pazCyzKoWd5DYxfBbC2%2B&X-Amz-Signature=1c5f7d49e9dfca3ccd8ec1b64db8976e652416bc01592bfe0178c7c2ac8c82c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIZUM7UI%2F20261124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261124T073351Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDcagNtD9FtC%2FgzBZIh9jKY8aaDEmCu4n8zIjdxEp3bRgIgO91fgQQUSeG7ja0BN0TaYklOXyIQHOBmZMns69iYaO8qiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP18HgPPWmodVER0zCrcAwlHzTi0csyiStCKzx4zvYx4QP5F5kZA6RFeNV1gkOYdPTLBwLwH97FsyWuZGiAeJIKb5NbzlkUuAisNjq4qTb12LtjUTYOOk95b3klqrK%2FfMY2EO7Mu5Mj180QDM1CMJGk2aP9ajpLQWp%2FYLFBDwRKlchL9KuJOJkYl8vuUn9uQCwx814FhXEBZdzCF2nzrz6WaMDlVxvc2GJbuw%2FKgZNVMLHm6XqmH7MGLvmLoiBMl6m7RQbQ3NxgiULt76dZSBMsr0UIjld1V%2BQqSxtkZcnVbhCpUXg%2FdSV3mpDraO2oVATMDklodEVaIAGpiBKW7iVuoY%2B5EAzKN7bEv1jkKmOsVlfB4FAucwKqYHVkCE%2BTBx2Gc0cXmjFUq%2FXecMUTCRZeG0w0RWUPtoPw8CPg7oYmPfILKBg5uO5CRJpu4mMFwYZg3v9j%2FTDGitpltseocZnl%2BbOOuJJZJpVkrcbRskRFDvqszXXDvokE%2Fnlq5dkdVhJyBGGhdj4X2Knu%2BcErvx7w8NztMILuCKdoRObLkhnWZQH3sbhovTkudlM68wdInxEffkGa4HIFw2C2PPR8nrLfZLyAaA5GBIlaEPesv0HXPBpjNZUE6uLfhv12k0dh7W1Nw%2FRCe2TT%2BDRwcMIPClNgGOqUB1%2BIVUFW8HI9c6an04RwcDwKSFY2Me0ppgwxqrAC1RlKkCBo%2BfwKjyQytTIbspyToz%2B4Z62q6a9pQ6Hwh5yJEX93BVe9d7Dz%2BmWi4BolEXE20wk%2BBASBBDwXTphkeECLZoj0UMOC0hS44FGhCzj3ehKFZAdx8ughJgbD5tR7mlSSxUoU50qfUsVe4WNWNRgy99TxJsjgYwksPr6oQhpvfurqPsBwm&X-Amz-Signature=6981c5429730f14d55d6e34f772f9d3af79a71bff416082b26763bc0bd7624bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRC6SLGI%2F20261124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261124T073352Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDyaJK%2FDB8AOmDMOOHJixSYduDp95oRFmWgtQj8erOBqAiAZcVtiMiJEZS2LKccW4cRq1lfw20QG%2FpTt5%2BbgupmPaCqIBAiG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYQpG5DEHvDhO7BOBKtwD0DWfxr55Uj8T5pyrOZG8qe6BoVZbh2eVr7%2B9YyjdR6de30cip98pIgdtDEBmhCQzOSSA%2BT27Lb4hZ3GzZ7sx9cukXdUAqsG3VmVS%2BGhqKiOSx8Rdd8pUlZABTe%2B5Pf5AkFuk5xosPIvBEOOpVwuqK568da%2FEU1pgn9i4lJd6IYxGFyX4Ah35aw0DF5JkFvvS7jJb35K1TidmwkLZFjfbZ3QVxFIBw9W7CTbyugnceGYNVaSp1PGP8gAL3WTfv%2F5AV13M%2FIOC3BNKj4ICkOOtXiMMHoNOqyQJ59rTygWvOO1vObsHxSIUmJwj3l2%2F%2B7%2BFzpTXaT8zfoO70JrGlkTMhj8cVeDeUpbXTFSfm7Rup69juqj1GGgNo6x5r8NeYLASFyq9qOmcW3W%2BvCCe0p93xWMH8iJxU5uDvpE4vMQpZVrnXSIwp1%2Bq39DVNzjZdE%2BamF8sEHnA4IFFogJrEtWgdbMHZNg3944nl2U%2F9nZTC7UscjUYf4jKD1f5iSBMzeqYXnScw1%2FVMTaMSKcs0%2F5CNsiaKqzLm%2BiCS0UQLyM59OmJa7t0%2F0AHZrGkrl5%2BdJLBMHSPxt1zHI5zEALLD6QMNWI%2FpiMTc2GcuZ3ybxOGaaNte2QffiOAGWzrl64wpcKU2AY6pgHMycEYD4Wzrq4O%2Bj5BHgy4%2Bt9FcxE3xi%2B2g4U4Oq2qT1DVKWt0OhUP4yqU1x2fF24HJhKfJ9r%2BGm9tgHQgOTFjDleqzXbHmDta%2F%2Bu1erXBgr1E%2BjpYhhWz%2FTgVx6xybedXQjW%2BWQOZCJInem7uaEryq3NXyTMiI1RDddj0NFtPtR5%2Fej1lKhKgvBzkhjVKBoeBgyWrE0Gi4CTOwrKRoF%2B%2ByCT2XGzp&X-Amz-Signature=977f079d2b534d0f1f317bf99f470336cd2f84433b74ee8450003948c26d74a2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
