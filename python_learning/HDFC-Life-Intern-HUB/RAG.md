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
fetched_at: '2026-10-28T03:00:15.323Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ULRBQOW2%2F20261028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261028T030010Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJHMEUCIFHrYR%2B1uEoL4h9VjxQSPI9tQCmi7ANhJ7CT%2FoaixJ9wAiEAxVjyT1ZQKv4k2p%2FWeNCwRXqYymVEhftIfD3aaBJM%2BWkqiAQI%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB%2BF7tUNecpWPl51MSrcA2Xv%2FJqMW8%2FHDedjZ0zqQsklwpbcVxZ%2FO3vfCV2yw6E6I27%2FSYzVnEGcJlMGU09xm0Rj%2BogBrjI9U6RohkJLBcpf6tqwZhS85mgP5fDPNRAfOISxIX%2BinB8LeBiCJgpsPZYyDsv5wcYnZGp3HBbD4Zh7oobr7a8kWgRnrLctBqGcGN2f1%2F6DGt3rOw%2BvvYhGvMRYKhIjN0wMS3tbR%2B8hT%2FxuD%2FRey%2F1hyttUzedb8MruEzPYnGFM1MkUnkUMY6YCLNTGn5hkrTxdQhtdjnz7Sz6l9dd7vkst5j1nTfE0C4Pv1LNlEHHWf0ZxnVUilpWMyk5uLiT0RjXqZn1eCrpS4zRP9iELVfmF1hl8HkAM4TCNPm6Kzz3snnZBzVlIrpqw8WdznR1rzrzU%2F3hgTX%2B0lbVJImpRlfkGzCXgE1mVVrK%2FeQoTFC9JDwVn2lq5iQ4JzF8V155hm4euoZHfspt5LLQ3jMGAd%2FDKZLU0c%2BjgPJY%2BJC6MhhP3Or4SBqRnpSx2BwlftMMGUQ8PogtrbR%2FARcWB3P9SGq8orZbn%2BKVaQpTCFtlohzSAvbV49bxXX6sujDZsmvmFwXxeIWc7CJbEs6d8cNk5O2kcJnmHi4P8uO8jCx%2FK%2Bdem%2FM5fAQ4CMMCzhdcGOqUBS%2FBLxr2DU6w%2BhYGPH1k2pyNYpbocAOsjfXvvDcLILly2pTWfcLoa4P29lCaC0nDC8%2Fa4wF1UTqThyAn0BxE6kXQn8S0%2FPYaqx2O5LsZMIo5vTQm4kC8xNgjUB3%2BslrJzhEVelL3gd9JUiMhcodllIh9ECS%2FExr4EvARkQFIhcSrGhhgQj7qBzh%2Bb2wrClZyDQe0IPm%2FJFfsYAkbv1X%2BcQwo8aFM2&X-Amz-Signature=cbfb0d7431a9880fc1e0235985625cc76133a28fbc53fc48b64160334c9fcbc6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RM57HPMR%2F20261028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261028T030010Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJHMEUCIEJi1D1Jj%2Fv9iXJiA%2F53ii8ElzTbqSAuLTEBuLgeWEAGAiEAhF6fFuDIJ90lvuPiiXIFnyNrCBvO0VYynVRVq6DiCsUqiAQI%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGKzdwrCOPXUtKBNHSrcA3gSGwnGeBVuvILgsYkZ9k4fC3ePke%2B8Gl6zjd%2Be%2BIlDPPKtz6IVHaLasfG6gvIamvmhZ2T4vEpFAjde2Rp1VKdPa9L6hxABqQeES8NFmqQi9cH3srSOdWqS2w2sjhi3ijOa%2FVVIr%2BW%2BBVkbjxEfCYmB83ZWsYhZOjREmS5hBv%2BRy2ibZdoSz3CjJiQzLTqkoPxUCp7ryu97anYd41yVHzTNw6vvbnNSqQr60q3Mvpi%2BU82ekp%2FMJtqIC5KT3h%2FICfhlnTteTDtjfbjyY%2FGR8oaJ9UjlgDucyjFFe%2FoJNaaQm0cIpqm1st3LrXr3gU0gu1y6CjKGYWgNxG4YIH06krSaI7OfQv34YUvK7dtVxulfSEtxZnkRE2%2B96GR5kojp4pgUxWODteDvJy8PHjBOS7y2Ygyp04OYVPLdJESTQcuPn1wu0U0Z7CS7hpMVtjj9R9g0Rv1iLo3gX9ZUH2DuIYYB77bWfBlRqYFSVh1%2BIh%2FIFaq1EwY6LTvhTWZNomjh0wHIZXyO%2F53lhyn%2BQQezZblgA5krz%2BHCjOVPam65UZ%2Frhu0ClBAntdKkXkOR%2FldmOEnvurZwIHdzMR9OhTpc9HLPHB6o9dHWJy8%2Fhd%2FWiZX6bywsTI7fsHao%2BeCTMMuyhdcGOqUBVBkRZGoODJ5DTiLrw9qxx5GWi2qjAHEy3Ux%2FRdgXCkS314SkBm%2BdTtJ2klQMqcOrIYyVvA%2FKwHqsJ60IQguazQ1aRtxhTbaBt80SNxvedGqSV4opyRMdYgNfdWojyEUuqX%2B5crDpmRhNo9tgOacDNggZ6p3rrcRO6yKkGBPwuAmsEfh5CNWPZxSmVS3AT2gPAP%2Fh2xlo%2FRhMYydhflf14EbtP35r&X-Amz-Signature=4c8ee69ff08d2cd5de402991c8226dc5ee0254f5177ca2dfe17b1511d261c190&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TIEOTD46%2F20261028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261028T030011Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJHMEUCIAedo%2FJfbY9zwIK4%2Bw773leDsdQIp6DHsVv4dmgD1uTPAiEAr7kk4bYgbd55JzuVfi76Hn7D7z6mZv07WDr4fzVHBqsqiAQI%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHfUhY4l3XTLG2KegCrcA%2FfQoL9r8FT7dkgUzjOy0WQ0oG6ezIF4WCJHjyhBmuHuqtfti2ISdGJnKz3bkLYSZNMfSbLEBUjAxi5QU%2FPcRePt7nArMz%2B%2B%2BLsmLsmqVyHEsnG65unWkyCfmKv1m6%2B09MT9AG2ocIV0QDY%2Boi5eraUWJPvLDpi4w1pwDtFnBfawkIBnNP%2F0pO6tlpmuSr5BTVWCoPu0zJNJ34Tc8LD6nIzcFl7FMq3p9bq%2BQSnoF5M8cUCMB%2F7DYWAmc4hJ1PtX5tz%2BQUF3Lxc6aM7xmwYXXWntGN6sVsDZOJE5Ps2buZqr4WE9ECP%2FeJCN5DPOdQYwBlA4BI%2F6h3wkoCxSKqgEN8PrVFW76rI0YGR23RtoVu1g%2BXvmScv4HqNWjd%2BQZ6p2oSuguqAANYOpqXhSfEh3rQeeBuwsVIMZ2anMSh%2Bj%2FJArdYJBYZX3xrqrfPULIVxghhXxAOm5ZLjFASmZelGnXceu%2FfqVaNCtW2tScJVIdzuzGDveYbHrE4M%2FGR7IVpU7yb%2F6MXaQT6iLspAAUEWysbLpYRmbxI3OxUrnFm3ua6MMKK9xu5JnX288FjSKASkJLVKRe0fWVdW56DkSrjDoPTaRGEMAHYMg218oQI%2BcUeJ4%2FwO%2FjdGeyWS%2F78wEMJCyhdcGOqUBBCi0eyWr6nXVfvMdxJC5NM%2FE73HpaPVN8OLB9uqtU0oyEO686UTVXx3HEKCB7yIQ9ngPSmv37IKP7%2FRvGLOlPRnZZJPdmOq9XGuYWHqR4jP%2BbiAYMfrVdv6GuQHdUxyMY4XJDxH0ec8dkI9w2BOgALF3eac9AMinK1SrACETIAnnybWoeSUsUA7b55gzkAY1dXf9UxDAvbU2EdfHVt%2BmNsdIhm7z&X-Amz-Signature=4226c8d988b6ce4bcc5df13b04cc6ac68455c4b3fe1dbce1d948cc287dd11e48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
