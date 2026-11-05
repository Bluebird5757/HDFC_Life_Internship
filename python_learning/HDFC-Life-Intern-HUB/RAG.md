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
fetched_at: '2026-11-05T04:12:24.894Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIIMHENL%2F20261105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261105T041221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDeSUj8gBDamnpZ8wzktrGW6YfI5vpZqYBncPAJn3WHtAiEAqNRqSE9UcBMa4C38ZtK%2FP4%2FxDLdroOnfyPVn2fnhhckqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHy0botuCy3K5hUJHCrcA1ntFoWlY8VSGjZ0iXTskDz5Gj2YevOKq0ptNjoo7M%2FQhWLGUE0oWiQEBQG%2FzO0gSj6sDctnjgoNbR23o%2FY4iLLvpt9KL8pG8NJ1tsfNe0gp8AUD%2B7zEkuu7PY%2FfaFnHFh%2F4C22SL7V%2B3UavTCCE2P0ew%2BGefDCPxpnybRxb199kUq0ccZyAMEayDf5tUJJ2uA5EMTW51C8VA3fY%2BVkz5pCTTqO%2BUhJYswFmynGZFt%2ByZ7iE%2Bm5Smo4JGab15dGvAgUZaInewW8vzRFZKs3eyJkwNLV1ZHyxr41vN5388uh8IguAQygOWtWFf1xBIzIRPfhWBXC4xEuUS3WTPBoHeuriIqsEGugUduPkRAxceipn7dOAc%2FWoSfesl6pdeUWlBbY5kmX5dOhdGp6jOmuGdSFTYnkBs4DSpOAHeUeyeQ2N2vz0n%2FYDMMiv1Q0Anuc8Jbq6KYRs%2F1Zb5RU2syyshHNgI7rxvo7yRzOmwfFH3lQomvairI2gZKsS%2FJULQzWQk3ajk%2FW7WLEYwMgSUK1jrHr56rLnvSWyfeYrZnB4TDe6SOScZ0HVov8X06qpDeY7HlvNZzkbt%2BUw1jws9LMTYzeQFLtSGOPNCCdz1n18OVVdkx7fZL5Um4vJ8zDmMO3vr9cGOqUBc%2BqjaSw8BHHUXSTnv9qz80P49f04VY82xOP4%2F%2B3XRsZv4zNmCgrzYTfqrjvS2LbFPPm3Uz22xkoK%2FBMimQRF7wUd6D3Q7RCS%2FjXUpKmldkHygyzsR4tuhPDDOczen8yqF0S6j1q%2Fi2ThlqC0N5Lzw%2B%2BIjl%2BTvTSNE%2BU%2FIHTX%2BfLMvI%2FL7wn%2BgQxI97CIr6FdGSZ8lCcIMpo%2FpQZtmxofRIcgTebc&X-Amz-Signature=e9e07c83886a4a05d1adb110d2810bb25aad7e42cb6365909382cde0c508102f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TUBPDPNI%2F20261105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261105T041221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHA%2BpQGk5Jle02plNFa7HoLg3WdQloFlpQt05BpQZgagIgc%2BVzZfL8dCU%2Fh6AaxDx%2Fultjd3tkxbZF9vGkrGy4pr4qiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJzBH7HffanYt%2BUsoyrcA8QS6ABqlGLpkM06m77JWNeDcnlhy95UdFRKfqYneFYHHBByI6Zeh1ElswnGUPDOltHTKrw0XAmB2cZl7tfpTG9Pfy%2Fa3V6jErBFyjM6gjcX1t3RUkRb6EbxgMRiphLKrL2mAySZthSmyrH4BwdfxA4371JDKPCT%2BuovqEDTfqszHWzS2pNHyPcB2X2KkpQgrHTqKIdWITCNue4WGdU8b6xPIIu4Jwj6RtzyUtEMWp79tsNwq4VIJiDdkSUjXAKCJJV5kKAGr7kZrGe0URyDKCy42c0G0jTXtbxe37OoPukp6urNGiaxZqjisEQq%2FjJn9wHMGusUkGJLDxvmE%2BEvDJSssTFi8ygk6AU1k14FHGBmQ%2Bx9khBCiEGaFA6MqswRZQyNdTYIiCoHK%2FQszMhz2MZSORo06oM3ouyUM9yMmAaR22Mj1SflLC6%2FvhT0NTEgUejwRs4YqivDQ%2BSQKusjg4FeyNEfjZ0KIfsuDcBZFQ3q497QI0GZc6U6zEZFnIwG912qIVLG%2FxcujmARsxGSpeilszT2xUG2Er34UYogRdil2CYj%2Bwiqv%2FsUlOBeZI0Psb4aDcEobKG%2BT987frUclQTH3ggHnBQow%2FVoDCvPk4EdroLKfGO2AqVpRl%2FVMN%2Fvr9cGOqUB7bEqFO9Lp6BgtRDk2rVnWE%2FQ0QzDpMt07ZOZe9xOwvr67agvoumjlj4Zx3uzr%2Bhomm8zqevAJaIkO2xwA8z%2FqyYz%2Bw5Ntj1bkSK6R2z%2F0elErZR%2B9ViZzihr6n9ihM1IKWdb7Iufp%2FEBK8ODjCLjk2QwZgXm%2FraMWHy%2FbRWmtMK8555GUZcHE2%2Bo46UJdjmvpRjC1JKnVzBPPwqE1B8AShRZ%2BePw&X-Amz-Signature=365c2c83a2fab78d064097e0f84d35894f425f6579ec46945f3bf4aaa24ad00c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z5KB2GTS%2F20261105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261105T041222Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAMmNsYrQeVZirdVnjXAmxtxI2kD9NFrpcln5d8FssxAAiEAh28ZuTuS3r7qAMtkzgJdlVCXlb3l9MUoEWjA9JanJtMqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAg2a32vwBGuv8UigCrcA4pQhbc7lyI8P8JBBWbQp4j656nIXAvjS5wzWAxVB9lHEBscNaLcH9ZIUEj%2BVtStKnPlGl%2F9IC8iWN7qk2K%2BoIWxjyNJZbEqcvb82e%2BMvqr6zxTa8abYd8Yh2EF4Hq1fl3JCBYfX45X97MNUxxnn7iNxYW4rxxiBTpuzL9ORYNsEIaIsahQ16lWzFzYV9%2BvKNvJ6A6IH1Eyc89BhPFPO%2BYi%2BMifI8tEsoY6GcZA2TPOt%2F7q11pCBkcqAx2uw9kc%2FizYvCDcGyvYqXUHL3J5zJ5HyPPWRXlo3t6HYNGo%2BYZwMj5N6%2FVz9iWfQJjvS0XVnqDngI7R6mUWQtApLb3h8b8aEm4zUH%2BxBnGEobzd1ZWmLXdXUIsZktAMNDco6ec9vCl2TP2nA6QwuO8nAiX8v5%2FTB4o2jKInpLXs2qTgjfrMNJIGmz995PJjO3wdw2gqq%2FlseqcmxZBT7Z4SSbHr5k5ZjE197zrdxfl%2BiVjkHhKgRl%2FU2nXSP12zrZ2648BXIVT8TWxl%2BrN72vujQVCaJC0Krpk8073iGcFPRA%2B0mE5LVz3zIgiAz0jh%2BTrYQoxAzlSiqNo9CDDYktfvXJut%2F9jg%2FKngcUADT5gccskG1gTXOsc%2BmLLkm1BT7m2ekMNztr9cGOqUBJkDPGdLRVflTTApHQAaBUPelxI0CcXUPtDQss2p7I485S8%2FDEGTDTVecMYY0T94ffOA0m7gs1QflwJp5MLYYlX0NPA0RWBm4zoNM0yN1JYHLUf3ugOCmnqyIlDLB9zFRGeE0x2I00LkxFsbigBnnY1ksKOg0Y6LzeErkc89jew8iJ7X5Fm6g%2BgtsQ4K4khhce0ZZsIqKFW508%2Fm0jKN6U2P3xMIx&X-Amz-Signature=4a9e122b85a6b3193e6970b4c01868b8142105f94ded5ed42dca04856364c23f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
