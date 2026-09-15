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
fetched_at: '2026-09-15T02:25:23.064Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FAF6PHP%2F20260915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260915T022519Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIBd81wHOp%2FUg%2ByGdZ1YMaJJYcTJpD0HKc3gQaFoIWwjVAiEAtf00OXggoqB%2BymoEtL%2BokOQJzFrLef%2BTUF4KP3sqANQqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG8tjwvJapqr0%2BpsmircA30OAUlMTXvHVmR3Gz%2FjiGbmVZ5Wcyq87KVe%2F5eOUzcDTpJs5ZWAc2cDKOMqtesHfpEKIxsvnzZsiYMX0ZQHfhXTGetUI%2ByrH3eFs1bRKvCDjobaeR6AoyrY2jTFs2%2FBaheuBWhRzgV11CEWDL0H29lc%2BVvdDAQ1gQiS6ubzk%2F1SV9PUVUoLWbPd8YjbaxgalxsWwfoBDT43fSdeJ2BiNqzoiC7NweEjYHpSqcSJS83zUqM3ZyQog4Xs8JEcwsAm%2BAfptF%2FG1rK4okNzeN6d1iPpF91smbiYdn6kZ7yfmou%2BU7M09zsMyx11vZTnR6GiDxRsLonwBAGvE5AfgaYaZD89BQmUkYm2ZS4EPRmxBBbRzQYCKbn8VjhXlpvgOB3sOyCKaUG7JUC66hhmNu5Tonx83aMmXD7B0kLRA5Hhzsvaq%2B%2FTv%2BJd7YWAQsgxsOPQZke1wQB5zdrWxmRyAAZnzYplh%2F%2FgrLEJ4FoiUqcO6t%2BhDwy23KPoLbaww7MX80zXXx0dJAwHLgjdyr94%2BGsfJm8SjvhuaJkf%2FiqcnMyE5RpQh30GUoMd6%2Buqh3vdT2%2FHgUR13JP%2BUg0R3%2B3GJ9epDnXNKRqLfT49tajnyFHn0N6hIvY9tN1HGR2SqYzjMJeiotUGOqUBIoLOoz267VTeLsYmlBgIBJ35mwIedfAnTa2cTEfY%2Bw0%2BvDqfksnHURn5IshzRZHOUDp1wm7QczmV4dL78M1kYXHuPpnQ9Nm7vb%2BcwdqqJP3T0Wjs7N97fhA9KEoVTEsOiZuTT7lb5ih2eU1lpXr4lZ5adosVs3gGYplihg6IeS8SCXLGH8tnRT98Ok6jpYd08D736k1k1RvJJFQLgRIohmiI71ED&X-Amz-Signature=89b51c5a451401e04b8583ef73e5c82851a15bd9bb1c04f61e007f585da727e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YV3J5HFB%2F20260915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260915T022519Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIH1nLfXm30Y%2FoN7Uz6xJIZts8p3zKrvpUO9UvRSfP%2FIiAiEArf43yM2uxls1Ll%2FUiNUg6OLGVjXQSPsL5wveodvkhSUqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLIT%2FQV2L6RUi%2BfGDSrcAxfBGKf61DfpPM5XFOXrTMfp81CMariEhVDBwSgKHrCHBgQmPKy0Cf28LZ9toB18QUIA9U6cz1E%2BZ9I06CJX0YNwwn4SeQia2DPF7sI2pVy78Mfh9wO0KCOVm%2Bj9byg1esJ39AkI6MpW0p7PsyipBPdtIJaH6Dan7LFfzzcdGIgTXKMXmLyOo%2BMiqfjF0iAqNFHyE06HwHMgd0o9IyV6tDkzbRKVB6OhIjeJCHfNDhoCUn6nMIdh3rmDfL1xYhyZAKAfdeubvEdgbh75AT%2FeWKYT0jlVu3iU%2FXzkC3u1kHWx00so2FRNimqJoe3PIoNWiaV8amx3ldI2RSWMm3PLMkxprDzUhErpfJwIQQ9rq09ppRfmoI5vwTanWdytPXlsiHu9uJIjGAChbZo8hgz8qbTtCqa9duRb4Slg1S1UQYLDBWpEoyM05lLnukhN4yHcXEGGMwzVZsIN9D0e3hDERv6iMV3L7n9CUdhj6ztWuxzHtCDVn2LzdSJjBEg1QGcXRZkF%2B5bIMXr0K419Dfm4J2mYb0jvoX6e1cbqaxhICRwwcmgdUYvzvFFqWqEScyWVyS13vo0OmXbJEFdQnjGm9NEx5PaVPHCDrCCUBbYqVnjqj5c9FieuWXwo0A7bMJaiotUGOqUBzqZIGfWFVfG%2Br4B9wsOKuKiZ9%2BQkoAOTPT7tfTZqCovb3eSys%2FzEjMF4LlZZBf341L8PPY4HpeG4rzGExEUkHqFolNOg0B6eyHhz%2Bn%2BeKPKGg9U260qEeBK42F1S0cxow1b%2Bjhv%2BZLxlG6d44idgxbjRF%2FUuvxYHAqR9BXTuMoRs3CpVTpxwNVvXPHtPXPIoQ7SaTTBa%2BbgZkoSI4ZBBh6wZR7Bi&X-Amz-Signature=1dce50bb3e8bb6c4c718e6b4c47675201420315171576557baf9923ef21bb00c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZLKOLJ33%2F20260915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260915T022520Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIQD9yxnWkJgx9QSWB%2Fmpj0QPJzpReAI0oCw8aaIS2jDG1AIgDeoQHdkZ2srrKuEH6o6RrD0lHX1AjeqO9af90fNv5F4qiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHHWh3cs4Z4%2FiiNbWircA800vSS0VobE%2FzDYSIJBx4bUJqzvdA5BaR4eFBgiPicu8OWoN6mGGxtX7ic0T6MVjOiM0NvJX%2FER5Gjru6rdgMUa%2FCQCqZwj0g2pwOFXEgyTCnZVDCG4CNlD6s967IPUqcePotRQKU%2FuST49wgTl6VvvfEQLJvC33JozwhwWigYCqbelMrjbnjJglQuqwrrv4VhqWI7Q6wf1XhfLVSgtQe2ooqUYGYdAPHp8xG0nkw2vzWlDkx8F0YONXztfJOCe1HCTICFGzKYMxNS8gGchNcqbYI%2BJ5P0hDE8Fd5cuQwbnFmGdynEDT7DurrqkYnrBBxqzWiDckyi%2FHc%2FA25vcynht39pzMS8ZqK8%2F2GRd662G%2BoEUZ%2Fh3FDZcJO97e0meEqrdCi3LMQ4rSmJRQjmcykitRY7MAnHsWzB5Q4jD95BPXwaV%2BbRO6EvhJeqBXICbp9GTWPyU6RvwCiC%2Fo%2Bv4fuaovmiyBZn18e%2FZSYBSOc8g6I5VH3Ct9Yvy%2FAiTXuCd3TnnYlMSZbASg8mgd2427%2FDxefLahyNKf203iSTerrk%2BZTRAC0rxOGwLqfeyN%2B2743cC0RcqFOG9J01XQI4aATApCJ0HcaDPMQP1IwYL2%2BCUKHpqqfVs%2Bye%2FtlxHMIaiotUGOqUBrpCa7Gb9U5apssILegXHRrTIn3StDhehnpeZjLnvns60gAOhthZmp7N%2BrDRgC1xifCzLy759mDvJtknFSrp3UyzXePS0%2FZm%2BmxvBtE00qKnGa9icolyRCn8coBJDucz9LxpoD5WRO%2FyCYQH5osd6DWuv3mgkLG2FfUIrpk7WUa1vTAki7dToVyTQWIRP4ar8kp2Ku4I0v3me4pv8uefuO4yvF1Ly&X-Amz-Signature=c0b3b829dc50ebb7cffc5bac374021384d4796f1634a2a3e833b2e9e598782c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
