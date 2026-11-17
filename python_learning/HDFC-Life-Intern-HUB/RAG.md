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
fetched_at: '2026-11-17T05:41:18.326Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667HMAV5N%2F20261117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261117T054114Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJGMEQCID9o1yI9AkFLY5p7ojkHx0K1VyEHHs%2FDntqoqSDqdV%2BcAiBhJEouTP4jTbmcpxwk%2Bxt1Jlw7Mt3G5H3SuCgkx%2FrlAyqIBAje%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCAfJi7u7rNaFyQ5%2FKtwDoW5v%2FeBqtUO9cSQ4sWhwLWyooKYw3CG8EAJpBWcEc1%2BULZSsabgvXy1eDMv9B5V4d2NtMwPUhE24Z81pYTZqUeMR22A3qz%2F6hfStEsWBvjEYhjS9Bq8RT68OrxcVl0Fm%2FkmgqStQXU%2BbGejLGpbl4F5K%2FhYI%2Fjf3UIB7Xy4a%2BUlPoWsQHZtC%2BxXwgvPd5KTT%2B9unVS1Ef8XjnQqCSHbCIbtFybWJyY%2FBx1AYkYlJou%2BVgbu4Q%2BtkSQeV0%2FOs6B2USKMaypHtzSY9cdJq2i0qqllaMslGoME5%2BofQxKESN4tlfFHhYnLB0PF9Wun%2FZPSW%2FoOToRGk8D2cL0zNVaad0W8uHPCa05DBE8qpH5ELKvLv2EQ9eIs3VRak6pMueH%2FeAxxdMT9PpF9RH4EhVh1w%2BoT9LHtIadsjEJrQLj5bU1WapBMIxOHfdjuMtInOVnBERPRdJ3%2FqJI5LRpn98O1iDXxkyYSlsYiBR8w%2BzgaRl2hfZAQ1Hm2FCUHafJflM%2BbKow8E%2FSTOhwM%2F%2Fk%2ByaF1ovNSsXZMb%2Fw78jhsRFMcUmvDuett%2FvpNYJHbR8uy9pD5t%2FdgN%2Bs2KCJp5iRM%2FPLtXDkPHztMVgcMqQspchJZSXUvkyKNMtaE6SjbVHmMwp7jv1wY6pgF%2BFisFnkHZOA1Zv551U3YvNDD6Beh1MSxph9Al3oZRmQloDFeNbL6Ih1hBZIYjQmikVRjxK6oCMlW2YsPmsakinEq%2F%2BKKVZ3vwChT%2Br3Sjl8rymoc7GibOO%2BdfYJ1lH2xkuz9z2a3fNawrUn%2FmUCfLR2w6ncuNCIyj9fEnJP5277C0pVw7QiUyj%2BHtqFFghUec%2B3U%2F%2F1ocCR716WdfChjA%2FX876mKR&X-Amz-Signature=0ace60ab8a983381cc11d443cc6e99f8ff7d481ce22e99a9b60098625ba73df1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBEE26I7%2F20261117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261117T054113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJIMEYCIQCWr9icI3FMfWFVYzWm5%2FxK%2B5Bm4ef%2FP7z4t5S2CKMybQIhAPzNiDsHwgcecNqSaZhEPHCe%2Bk%2B8Nly62Ps%2FrJlSF3vEKogECN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz73mI2OyTaSbWCAUwq3APN5CqjUNVCCuU%2Fd6aOu2Qvgb1%2FmZLjw8Ao0tgmNwE8e%2F8PsPDsqzw%2BTguUqjThpeqPnhfoTs0Ymk5LnbHhsWvQIQHQcgUGFutrtXjc0HFBDqwNOd1%2FG5w3IA42GHIULoWB6jZqaIniM6v2cq0883gPEbcLKCnqxpXLDNSwk1Df964SLWP5LoEDCyevnIukgK4JezsJ49DKOurwyagk%2FL3T84Eqj9DZxXKCKITEaEySFssLVTpS9J62XJxsoDIfpiTw0pJOSJQfZwjNmNjFUX%2B4QTrg71LcNM6vHJKcRUO1l98eBON1kwGBCMZWLXluszzuCFqY%2FzhUZMNSyHymwI7qPnz7QKvfRcoCmIaEpNNa3fgzO%2FlJTbREo4H0Tp5wHj5NgaF582o8zrGVb1YqV3ABMCII0vxAAQDmQsuSeucgr8WE8wNg3lBNfefX6RxdIpwPZzhQeOzGtv6tvjBWejXy%2B%2FzR%2F09oE%2BAYLVA89FGuJAFfkxYOEpHvc9qA7RpGfyXEX33gSy152f%2BNisYodAMI9b%2BHRqGTzSJby5kza5RqNrd22PgoXSMu%2FbS%2BN%2FCjkDBGuToSEigy5oqTPsYkKgICSeWH6HQydDng2p4g7OA2wIulyavj6HFAM46DPjDht%2B%2FXBjqkAYhXrW2sxqdwZaiY9Y1CETZakI4c4coPK3H8GjSZ6MjOffgZ0YL2vdq8TMcVpxnz5eqlAuR7UH4AoYEuVxEdK8d%2FDR%2FYfgQxpRh7Skga%2F6Ty5YZLJzc7V3Mb%2F6Wd8JNm0LxEw9DOChM4R8BUEBmCdCGgS%2F8%2Bg1%2BJRsDEmi5kpjqcFcQplmYoSLQfi4aoOgPuZ170gi1ZROSWJypAz3wo73hPmeOj&X-Amz-Signature=43fcbe69a95b6e0dad19810be6df47d43cad1faf541431ad21f19aeb53b4aef7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667WPVXYGG%2F20261117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261117T054115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIBSQQC71uCGrAlKO1eWoX8GA%2F3%2FX1Yp2sS8ZCi%2BWlN1MAiEA1twBrIVF175%2Bu%2F1PiEevxCwB4bpAJJGQjREY%2BO%2FLpAQqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMMnjbmeCGpd73sj9SrcA1%2BTQP70J49HpV2mfhJqOzpudt27cMOUqFXDTWlxcHpa%2FSZjihGNQrtY1Sm7l8uT9czAsCdpSL6Egt8yy8K85FAGh1A6jU7NxWp2dYRaX49uH8OlYS68z59ve6ZH4JGiNU0K3XnhWYYoOT%2FJSDNumNSrI4%2FGFsnuyzK3E755DBb7uvEfsDlS6%2BEecFEGdSarK6Wq4v26c7XsV1on%2FqkhsORYLXeVnJWRmu0GtcBej9GnGIdjeJuGEEZUTgbbKN3iWrKN1fuKvWW6Yp2EeNw4QJrRnfTITWGoCxLUaD4iC6KZPeT1sC6%2FSFH%2ByGnvcWrCKxwQk%2B%2Fck4rb0GdGOk5Bi3DZd1hm2XV9G3gIwqZ6pYV9UUFFNBUE5pi8ehszPHdDmkDWWhnmf4ZoHQs%2BoAyi7KEgWaj3zeuwLgWMEThCppvRJPADAyxcKM%2FccygmaSy%2BSVLfKx6F8WTdYAvj81XnS1i7%2BmPt%2FVgZyVAvuSpILVqrBUA693cYFqbc1lGBPqq3TSkfvI8oczx%2FGxTh%2FvD4pZtZHNx%2B0%2BRgW4AnDBqXWqIJYUj0LXTUp4I3n%2Fui%2BQhIut2kJefa24l1JKsHynWHuEwFe0UM5itFu0BMnVOWCZmgxs2A5V7rrsh16nSrMM%2B579cGOqUBiUDvLex5WZpkXUwSfzd95VCCoTUmQnLQTds5tjUAMlVdqzOB6PRyfsofRUJLcTme%2B0w4wK6tT4oJVXwBpo%2BmCFFFMWCaM%2FFpQhGB6PG6iICZGYLYxqGPc2werhme0kmeCSaJl%2FrR4B9QAX3tODgV74WXYDq81frPa8u5KPKlQ92cdut3fYup0b%2BnlymnbapPBPlUlEoGnT2Wil0b7uCTJye6%2FayY&X-Amz-Signature=2cbad99d03acd7cbc9a0c0213296d64d2088e885f1abcb543422576959d01cc2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
