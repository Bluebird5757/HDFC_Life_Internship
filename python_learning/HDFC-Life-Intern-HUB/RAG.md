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
fetched_at: '2026-09-20T02:19:39.444Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UBVY3IBE%2F20260920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260920T021935Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCHQ9fMhQW98aiB4RRlCAsZ%2BIS63plU2a9rtYm%2FOrswHwIgDx2RdqyYbmUG2v0M9%2BUCCx9uYL3vbBHb3QakFdYOMHEq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDMwOLps%2FYip8l%2FdmgCrcA%2FppSEAd4m5DLLZ4bB56JA4bNeOaye69xZSZBW3w0Nh%2BZQ7C94K8j9Dbcpy3AIb47h%2B3q0ts%2F9aUqnbFbler5S7H%2FtYVHDbI%2F1mIvs5K2%2FWhlstKvlATLzsUThOpgOPib4mjiApLRlNE1R96awOkFwWLvMV4IIWJILglIosD8tcTFplibv4vGjRAzzje8iOLISgjcGwZWYjDgK3%2BtlMfbAivA4tW21qDXFN%2BP%2BsPLZFjf428X%2FfsfvCAYNu1lQfHTDNCfeLamKXo3%2F8e%2Fb%2BV89uChwkn1yyeeW8kBd7InDja7e00xdn1m2j8tyKrIxAlL6ncTJHJoMJt4VTikL3GT9tPfQWP8jINSLz7ggAZwNd7b%2Fm58EpgFHNdUUdOstvLdIn1XpdpOWPYgouiSjoGe%2F8jGaFowPSG9xaE55hkAd7DT25murwB2e31B0i2frZziKEXw8I1RffekhjpBGgn46HeDnZrgyIUl17pu0XJUiw6jMNnTl9L01kvnxAw0g59rD3zi%2FWoNq6gCM29aFVOOD%2F8k9AOa1zIJ8Y45vOiI6BbeJfBpRJKrbxV9qwqy4OPQJ69nWV1tAtqkf%2FHwtb6PS1qleXv0P7ydCtxH745QnkHkVj41%2FLr5fa1IuM9MM%2BxvNUGOqUB2WFZTrJVUOXTYvxkUUn4dQBzOt2LJd1ZbVQtCxjyNmEiEnvjJMMqZmQaHPXKVpTHcFCPZLF5bahnAoigqGQrobfTJZNRFhfN5xwx%2BOddcIGwGzt%2F986atMSiyIGCP9ocVRt0jlhKULi50SGCEGzUsH7fWeUKCHaV0uxgVKCd8njEUK6cnsDuYb39HNE8rzdqUbYFYKXTLHdIpIVg3uLTuTMo9zKb&X-Amz-Signature=2994252c75f74e3b7676b60e2779a60bf713610a8d58bb87cf718f3b247c33b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZAQHUARE%2F20260920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260920T021934Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDAoJrA2dpOu3pQtrsIHfyn%2BUo0vMVKlTqBo6Xh3Ec7pwIgIEF3nU3%2BTpcghywCUYFpQH1sU4IXa7UNPjjwW8pAzhMq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDKbcpJ15lmVhrBJfKyrcA1NVVnxSp51Zb030FkJi%2BbZoBOHaCqOrtM1mXIVwENv%2B%2FZfMHqkvN4pB4VeuV80T7TW7avWGHNzi1Y7WpQm9Az8afbVQ1IV5em%2BJ6wYRjS5gES5SCngkahnaatWIUgjntWyOahuT%2BhJEsBpdUF3bb3GzaGH7BCb7afFKAFR0noOv%2BywdFQgPm8uCA62b3wY6KXQHjsOV91PgXJOhJAM5ItXLMJFhDzLD0hRwUQupxRttMYaFvy1qwYwjdwXz3luvr%2FeYDDGxv3AxP8HEVkNMVF52eNdLi4p%2FFxFzi%2FD7nOCY9BnPo%2BF6hjmwZyyhqgbpPGbCJj3pv6nSaZ%2B7YKnkrYII9GrYPebDiqlulq4Govl28ie%2BmCR6S9aqOgTsi1UbvDZ9aJwZkaD4eof8NaCQi6vD9d%2FX3lMouzGkxCj9VXcUSeaVvoCqT7Lm0vyjCwwK0AOzDzTWMb7sg6O745sAifxRwhNS3W7iksf1H5IJHmicH50FEv%2BrzY4e%2Bi7OMAAk4eHiU34BK16fCnlW4KB8Jpue%2Bg36fGVT6E7X7mlmHm5nFqgjoTaPwNdW%2BrHAdlMYI%2FTopaHCeN45Iy7kavX%2BtbaPVJaHmN3FkcP8Gu2utdo%2FEvsYJcHC%2FJoCgVBLMJ%2BZvNUGOqUBe1dWtsyM0UzYu%2BY3zkU%2BGvj8uh%2FVXxjP%2FsPfAwLO71sgtb39hZsulQyYqriX%2FgmxuNNpCTm80gJQSag9km7UGK0Syvvk8ymiWPvbP8MxzX%2Ftvsm5oRcaMNuDFPlSBGrwCVmEOkzKjydnZOwBHIBhGtZSGOSfm5vQVv1JGJtpS4aRKRWRQqixN%2FWcgUtSWLO%2FMnk0xJOpVlJ8uTStXsM17w0R3jog&X-Amz-Signature=32366a036cdb9e787a5a5213ef0ebd51e384cf06f28d7c01105b0fe5ddb84da2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TI4W2WDL%2F20260920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260920T021936Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCJZsrXok9dqhJfNKPiuQ7v5o%2FDzIvGE98QUlKYykwzlAIgJ38gsjj0f9%2BLqfYbr9O4SlIExZorXOxCSrqEGgw%2B850q%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDJSW%2FemmQJDqQbjdhSrcAxW80hYqwg9YUStGsLFPFm%2B3UNq5%2FzFC6QbOyxWu4%2FX34WOA8cJNAGSRPsmldA5BmwPqwQfJ2k2ltmZNNZfJTfyFHb3M5OdXtuY%2Fc6tsfZxcwEEi%2F2ekBRvX9o29e14%2FQ1iIt7wqsA8HTdM5B9qTxXMTA0toAOhdbb1Z5WjKw97xYdQ3pQHml25qnlzHBRgBoBif88u6U0NBFKi95onYt0f4V08ElvqX8uMVDn6LKeFcTCBC1r6O0c0xodQiOxrVE14qTKfoI9e11exbjwI77ikOCJmo6xN1MZg9I1b%2FKgOpowW8BQvkTPA0rtIN39epvm5bbksuYaJC9E05%2B3k%2B2%2BLoGEb2oUc0Hw%2BdDSo7S%2BS2eIFjr%2Fm%2BsusjEXoKYS9lM17dQQOvfNF6dEUqxJO2xBG0vTsH1kUi%2F7Dr5RWqb7RoiTdxNYK7UASEXjnoU2mRX%2B91RMyBatnapcd%2F3sNNYUbgbwog6RfpE3u10RhHOMyEdRJhoovqZwxhaZf2vxlmxPYNGhgGFBNyhjCXqyop1LBTRgHUBYv8Jsj1PScJ8dpYHiny46c28nnhXzoxH52yGzYQSsXuwbxPlgHZEe0V4ZvAsZBjeZr2o7AkfwAfMaUsrZvQA5qFqQJcPv0LMOmivNUGOqUBjUD%2FS2NGe%2FwK0vWS6Hy2mU%2FiPXUmBJCUeCCxYxp9JanAiCHPehuhET0UVmHluzhoGTDXitGbHtQYzLi7B7Fyn4fmrKqPyjqWCNWH8fOAgmCalkEr9F5%2FVQqtonJwaSjcu8t9Lk3e2hqLwrEgQ2RHiQ8wwdP3cL%2B%2B9EA2qFyhjUEqYrfo5iUxcO5b%2FH7mPTVDkpPY3%2B6nPZ11j77C9KcOvAkjMzod&X-Amz-Signature=3c59875b19d31e48eda0a410c83b791de39a27d8c02588d4e4719c92836da69d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
