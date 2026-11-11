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
fetched_at: '2026-11-11T03:43:58.127Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663RGTCHD7%2F20261111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261111T034354Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCerwKImZoV4XlE2HHaH5fXZ4rnzAYfQZKpNclJoLz5pgIgZYIqA2rGXdTDXJDPmQQfk%2FAawxfoxM%2F0ILDUUr9y2wgq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDNA%2BStKXzMp2K5cAxyrcA%2FkN5zubOTjLnNC%2F6rgjRxFYG6TqjH5%2FOF8DDUig0VrMbiP92dz27Re17hgrAHS4s52qHRb8RfqEMxnxdGN0QbtyZUDw1ImVfeOP%2B35%2FZ%2BhXrCkTv6FyRruDzKqGPuAzo4BBqSBtNFghgb61t8m2N1Lx40TNHXlXvngDTXf5XnA4GZ2%2BQP4bUDf6yd51Z%2FlKft3AFBXy%2BxQSU7E7r1DRW5lRHbXYtUFIav9jQ4vC8u%2F5G%2FdACjLMcjqWm1fQJhcxl5J6XjSmxK0%2FGkNC8WPaf7qzYBNCB7j%2BDOUFQDpZMa2aXWKVJsrApsFF4eXwROKhN51HPm%2BYtDQPUqjx4Xgi9ttlr6Xyv1rgWddxjWtw%2B3J1PmsPeARGislKq7Em5rHMTJ72JC1H5VCnHeNopF7aqOuDZz7I0qK00ow7K8YO9W%2F5kCrRR5Q45%2BEGmaxrxw%2F6Pxnx2cCAkybpja41teshgdFyisZHWESBnDyeKZXF0VSKP61F5e8HJe%2F10Td7EgVf4PpUxTeCBGv3bwKPhsoPG7iuxLKLkc0tfczQcaWSkGNzS9CLr3wIHQD5VZWHfcG873M8tiBxdttWV9dJJnDw2%2BTz0X5gG0F3LlIIMPDD1XUKmRVzweY0imAiUSJWMNv%2BztcGOqUB%2BFSL%2F3XZeJ6tHHbg0c8d3NYHd3zT9Um4%2BZTuPfPvAJoLzet6qgJNzzBu%2BXnc7TFbF98hkW%2BiAzDXoTprPkEASUOx0XRVHOCrFUV78fv41XZ9w1I2Wo5XvSPyKNoBKk2Fpdj00Icd%2BkjfdS%2FBVkRyCEbGN0W7JmaNL1mIUNX0DKXX2B8m5%2BDBfW2dCPh%2Bz%2BCPSt6AO%2Bk3jtmUsIuO65GElwh5jdyh&X-Amz-Signature=618fb74125f18ae4050dcc1cf15a172e6c36b3fb21456eee51ae5129048e16b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKM3EWMU%2F20261111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261111T034354Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkBlqdUXsfcxyr15uTBlHXTlqgdyJVOz9dYE6jyun4WAIhAPR6peu%2FrPsE6WMURncTloqU%2F5tUesz7SA0srTC8HJKWKv8DCEoQABoMNjM3NDIzMTgzODA1IgyK%2FaOIvcamuNoxe%2Bsq3AOYqW%2BAGnODC9Eua8NzaBN4FcXadNdi8yDLki%2Bf9TEeGW3JloMAC18ql4RojezrAEFvzm59h2v2XXQhf8RXGYvwVfeMEUVQVn%2BK5CcZ80yqcqDMecv0UqX2q2tBqAKY1VtyoPrfWNrqXh4EIHuRwHqfejf1TfpYqv0ko6%2BzhO%2BbbfrsJFKkjfsxhH4bJPcqYqswtCVUUnKr2FZtWLWMW6Pgr42Gqy4sF8xjXxXn9tYdNX50w8bV4LcFxm%2FA2zqprKN3wkTZxdj0ngNAbbpKd5SxvMFSfkh%2BeLqd6xNiPbudBZwVStTCpemKmDBc0%2F5Kga9jB7k0oz087qreZDhu0l7T0LXPlpts8LBzqn6XOQWGVIdIkwdQzJ19QGFNLAHPO2Y95wlOPQ2%2B5BDkykp9Uab59r5vu9ORwsQ6F11P1te8mRLZs8K11imoh7b2T5p8TIa0CfZDicAp9c1Ia%2Bj5YoQH4NxLeozYCTRcL2UVY%2BZV9yKrUtH%2FejS%2BfQMYuAbmOP%2FgFNRera9Uay68lDCbiQYBNfEKn%2FfJcUuj0deG3trtQSzfW7YEdCykGlT%2BQ%2BcI3YAsWQ9WRkI3pYJMc50GBJvu6R72ltIA1xMxpm8oAYaxIen2crXyE3NSqBBYuTDV%2Fs7XBjqkARMFD%2FDjV6KPkLdL5hisJ4H4A5MvVziCz0pImDR1fLjeE9sWvX35CllpIT0haX7bkBtCuVDbCEf0ISQ%2FHSdRdWhujbXrySzGMI9kBTI%2FdbiA6GiQTGEWWzz2P7As7wcW1bT1nZsvC6897yZFiylXb9oHWt8bU4xx3Y9rgM42eLxPPNKvwHAJQSygHnotFR2r6ILPJr82g1ddDCsk3VTs0OlPReaA&X-Amz-Signature=c1edbc197704352ba6d1b301cca8e05b1ad21ee4baf9e227159edd95b12d951f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PG3TSJ7%2F20261111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261111T034355Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDOvt5DVg6xE3h3ipWimHIQHiFRVk1nqR5%2B0nb5ql3W2QIgD2w7aqUWC7NSgrqmAcS972PvOpm9Aysx%2F3C7X4JL5w8q%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDCeZid6MF8TRiq3JYircA4ORKTzqVqqK8ynaCy8hHPoTnAIYmeO%2BXSEUCKgenec84zNylZTTjnFUliN9Z4Dqtugk4y4k9MYme0qoXxnJpQWBC2XytZzEfFqJhuig6OBIAnJSad0YM2d2yiE3RFwjA09wa8KHg6DWG9SNJOIvRVMa3p8E6iMnISE5EE%2FaqXMFs9gaJ6urEXJS5fg12M1AnkXqE8Ys67OM%2BHBetYFLvhb%2BEhWyrnslMkvSP6Tn18HD673WeV%2BrZB%2FC7o3vIuX4EW2Lm05oc%2Fq%2FcRXuPI4WbuBJDjEcSGQymTgsq0KIRxSCPyiEhuBg%2FPjgvg1lrs8voj5DdzO7nZHmsqIAjLVohFInJ%2FcJ9B7okzi1VCpfljC%2Bk0qvpIDxYtXGqRyNfsjzWBaA2BSGlE6qwElVEclFAbwt8ZnA4pAanWul8hsUuhx1fuYSVb%2Fs5nhrpfQGXeGorctKiwi64jYSyYe28B%2Bt8auOl5hgFML2heEWkECnAG5xJp8S3%2BosP1K6iWbJKd3UZAQdGTTY4PBv%2FVSzhB6yBEw%2FWBfpVVfRpjHytCOUVdeFeZ5LZAXJTbswsc%2F6DbwJJ59ryerP2BXlOTOdHX%2FqFfVd5RgdevaWGQaqUp%2BpkJrjQHQOpOThPxtdzTQ2MNiAz9cGOqUBcGIgRBRcPfY275PqS7I%2FIcF8sa%2FDyilEdyZK8BzTR5HbcnqPm7tp16%2Bo%2F3UWAdQGWHM9l7YN6g2fX9s%2FgVKmbGQp4wQb3rdUadY7FdPQbbJ0al%2FF495YNdbK9h4%2B8nRxAM3vMBIn2mfkFPU8Y1xtIvajGUA%2BaJSGZfo%2FfZLZOnGv%2Bnidt4o4ayB5R9%2BTiOnploQA99VkrCbkp99n2VAXQR6xBXWI&X-Amz-Signature=ae1d46eb87cce70018e3f98c2f2f44fcc828d6142b434e814cb061d941c4e684&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
