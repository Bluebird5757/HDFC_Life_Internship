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
fetched_at: '2026-11-07T03:42:26.035Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WF6MR2L5%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034222Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJIMEYCIQDKcfFWHDwWGg%2B0jxEGsJNli2A4tL6WN44B6gliBAMPSgIhANwPbf10T6oXLwfT18c7QrTNJue%2FhzMlGmtBy1Y20zxSKogECOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzAEh7wQpSGz%2FhuunUq3AOAG3LfP%2B6CoSuwKaH1OHpAYCxv%2Fzbtebpscktmrq%2FKlG9yW0ukvp9NGBtj8Lqj2efwns6DUciPxgCNvP0HBYwPWvEYXi0nXOHAv4JzAmhoz%2B3cZWL64IQM8os12ins1Bhx0ocfZMUygQba1thxlxPhYNxOmnlyzzayLYc0gIPVkX64ggv8pUnpIxnS%2BGr3%2Bq0KuJrO8JEbVpYediA37TvWeqlh%2B2sR5%2FqleePWqIFddpyjzjUdvPShOQ3uAhDM4KPuB6De6JAtQ9uhDSWCbg%2B87g7nomPTlc%2FFMq1nFQyjbA46bSMZPyAs%2FQGGZ5auEzKbfMffEfMNQun0IbX6VuL5%2FwgktA%2BRDYFDpSj9yOeFtFRFQwEK2JEOruUHGbGGxbQS9E39WV1CNhzKGUJEXXcYJMiiVOZZgffuhDdyeAaxbraOR67ndLtdCf%2Fp7pzX9RC269gSHWIJQhwazIWXPsudKkWviBF21%2BXAHoqNIhJbMqQeKjHYqHBxsJtJUbEAbXwPo7BiGcqJ%2BWZeXtDCk%2F3r%2BJKKC99AfI%2Bliw%2FoXHyVpN7%2BhHceKFDjvMnofP1ZIsD6Xyy1GXOB%2Fe7uFe5x60zddUZh0eIL7ayrPbcu79RYRJkJrcphQRE08ZhNADCBlrrXBjqkARvZuELUxz1tXIGxqCUkCi3FtPuQipIljgoVgWDdc9dpRNL7nUIc8P6vIHbMRB0OZlOyVnbzM%2BI4%2FTewZIV1WYhQYBmwopx%2Bbp1CVgcAkU0QJavVR7I3EaTHMsUtlhf3q7ZRj3ESSgnYfQ%2FH1h6Te7h7I41YWEZPooir81aVM%2BjUpmhcm09YVVr4fYNdrsHCmcUhpAeLVqlGA7y%2BkTZd4vdY2uEx&X-Amz-Signature=4b3ec1e0910dd1a90873f2149245bdf22fc5ebad36e887100b8f620980a16cb6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWBNJ2HY%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034222Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIEIMowZlK6SqsDUa%2BCBIZMZL2mHnhL9DPwfPyDO16Ns8AiEAxfO1sNX1HySLvg52NsjIMe6lTI%2BPt1hMELZL1vWhJEUqiAQI6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIqS%2BMNLD2AJhvKMjyrcA%2FEvRgPlH2J4FSMCZR2u3m89ryEFVEpy1i6xCr9gyuFen9rAB4Tn8Dyrpax3npZGFRJ7Por%2Fh7fSs%2F0Gw8qZT499E04aML6DJoC9uuaqRdyqRmc5V4EqxKJcqpFK7Nkjg0dvzPAeKaf8y4IpraqyHBRKd3%2Bc%2FIdi1FC84CPrbNohVWqo9K0K5r0eZCb5UMqB9tpMectvTa38N34HFT1MpGwdGBmf2izb1GWYzkmZLeh1Ure6f9uadiXj%2Fh9%2FqEYgclcWVVF2akXHTiP4Wk%2F9s5poOhlcji5ZCyVj%2BwxGxKKMl23CrT1vvGZ28H56CXK%2BJi5%2FlZGK%2FRjhUyYCW7zdbHAYMIJ9CINuOdzSUTi4RTPi%2BQcW30Y49Rn9CMpMm9VJaFvw6IkdpgIS2VaVSYLEaURqNPvlOGZ9iB7DoqZ4S3nR6WiX8%2BVIy5LX6a40QxWT8g2nRbUazABkih%2BOxQWmS4sBmv%2Bm3TLujZPWY2SYX%2BvDWy0XZwoVZuetiCHNHvX1Kp46Ue5VnFAU9AP0sMiocdxTHlFO0gP1eGtSOn9GlpZQDmaisT9KNMeOsxkvGgV%2Br4JZ6QXLdG9i%2Bm9rOVM8mX2InghtwzKJCfm5uCwUZwBSFXS%2FsCHDS5XxTNUHMIKWutcGOqUBxzjcMfF8mjLN%2F9qJF3aoR04uK2QM6yc72IS%2FATh27GqWk%2FndDtRe7%2BY6yIvvHNm2iGGmxqHvGCEo6k3etR4cMOWNTFi%2FT0K5krSu0SPqActEoEWHDzpMu4MwWGSgXYOHJyH3rmhmo%2BF8TX2Mxpsu%2BDJ5r2O1IgzD4W6BLWRh5Tww%2BtuLhe%2BGojJ%2BY6G2R8AvvhnxQ1EP15Sxnb40AxJmQ%2FW9lEtV&X-Amz-Signature=6deb1f50de485a56615ed08dad462ad3f36a7c8bc16ac045b4ad0d9873d2ab22&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662FXUX32F%2F20261107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261107T034223Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIH90mxBGNUBySz4LmNNm5c18tcWfSVu3gQGW9ajYZQHYAiBbmtbGBgz3E8jTrUU0MVPfpw%2FTba5lbEnTY0NIsFU7JCqIBAjr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMBMp1jjRJWVP6QuMJKtwDb%2Bq5Olh0BZlCaLKkG2PnbjTKmdLHDPbS7b6LWkLQuCgnIYekFl%2F6if%2BHvizMx0MpeiMeJMXV8agb62W9SHwefu4Wwqrbubn8x0G86U1nP2hFNguarLRV5UkPSanhk%2BI6Jo4A2Wquyu1bqkXiISBrnRIDnSHkVyI3PeuEqx7C6XoFAPYnLhdg3dTZVGorrBkLDLl1OMK8pWUhNnoh8l1keJXp1Rx04tGUMy8rBNRwP0wxHdhMS79k7SpdXvumWfuHiG2R6sHvWzxun2WBvNMtBpaJA%2B8HNryUnITd5Oj99aptuOwiTlTM8YUzdDYcsOj0nrzVUXTalFjGEIWUblY4exjJfPTPnHON5X9D%2FjimKJeR8uBVjtyjjgQ1swUlVsoIVytk6mBuGy9kkp3hnp2i3kDMMrjqCkyP8vAoQAGk3MTgv1GCEMWMY%2Bi2i6%2FkGu%2BxBKYz6GsTow18k2rq7Z6IcTkeVSG%2B1XhMCGkugPBOTU%2F7xtDt8I7da4KdyLuNRwNDwe3N%2FY7lZ8LxEaGnJPPXme4Wa7I%2B3YlLAwf8vGfwSvrzXbZ2uV%2BSD8WMt9K3D%2FeDC%2BDbvkdTYOZNrSkUVOFDlDIzQ%2BtbkHo3DeCtQsnwcy1%2FThQ8APJuy6zfE08wjJi61wY6pgHmo%2Fz%2BRpKKxOPCLTpeCYEPQEV2K0Adig2DLy%2BEac2%2BX2rPPCH06FahRUdVU0horl9TKvFEGxVUwML89eMgKruYwkzpRNkuAAG8yLph7IFEc2pbiWzc4Jd6FhY5nvPNo4BcHyMsQG0RRaUdqGxwYzwVUIOXehMEBxZvldsl8WzKRgtTWbnJmJTbJyWAhTcGUlcnML80SH%2F1kv%2BmOiroi3N1J2xdwsjy&X-Amz-Signature=69d6d637ecef37820cb2aa84d71d374120763bfcb83bc21684e8d60a82117c25&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
