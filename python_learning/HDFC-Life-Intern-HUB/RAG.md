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
fetched_at: '2026-10-29T03:27:07.333Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S5JU6ZBN%2F20261029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261029T032705Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJHMEUCIFh4BAPUaR1b6ImsFSuWtFn4QZPVfGFnau%2FSg6pRAvXHAiEA6yg8YNkFaZfrrSCUJj3GYEk85RytJJZA5Pa9ySq0RCIq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDK4CNLuPhya3dsXEEyrcA3bve%2FFqAjvEa%2FodTXXvp6SiLCZ1YAQHBc8wzsKgbnK9I1T2GwMoa6%2FFlyguTK548mayOh6KMkeQZXrg8rbuvFu7uF4pxGBL0hOTy4GtbOhbQ7S%2Bb%2BZo3rl1wI7O%2BAhu2FoDqCdJwLyBgvLj%2B74Xh0o3QgPHl%2FqN%2BS0BWlk88gJunABP5I%2BoKebuMAsEoDLSW35yGsHrFtQxyKfZOl54e5pS7yN8C1u5UTDITU5dIdjGRy5RjYycATU2k3NMl%2BPw9uoV5VDaUEzQdCbsYDb6ZqFfRkg66%2FJWcWkJTWsqTQBkEB32GRo39lL%2B%2Bs5ZPf13P0QN1ZNcrCcNS8mCLOUH9x2%2Bf9hqozNvX64cwqOtRXrVkz2oScNZ6CXd%2BayshFm0uwgdvTMgDGZL2R3rbICjn%2F4bT1kQW8U0k8wJqEzpE8gpnWnYi9uVz5v%2BcLMd0Inktej%2FLUvi6P6b%2BZKk3HN7LOVVXeGOQ73y5UY%2BGGnDUeplMQFFp3aigXGgJ68Ec%2Bjzl2Cc%2F71YdT8llIUkMTuldC44lrfdmAtz2vdUtFIQ9PRY73X%2BqkJiNYgV6%2FPulR4nb%2B1SHVcPq4hrnHpg2nY355mbtDcrbPNcAyyxYEVMyyGA7Cy6u6Gk%2BckyXMsUMPLoitcGOqUBOscuUSIHmp3GnsjtbSmwVZJrtjQrelldaTHx9j8gdUJbvgkwHV137yN4GslCongsxeWHiabzJ7TyJxKSCbAYwBKW21JnzOHKEGwlq8tpFAvJsydWp5bTKcm7owSt0zwTqO3MoHj7%2F%2FCjjAV%2FDEPQmZwG%2BkH0LMEeGFBXEirhXDIxLo3lgBakztO3QR2%2BAKeJ2euq%2FXFqpM6z3rkP%2FfoLO2YQ0FFk&X-Amz-Signature=8de69e78384ae183e72895b85ac00c4df00e8df2c60c0d1f5c192f2cd15e497a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVSIWUJA%2F20261029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261029T032704Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJGMEQCIGUL8HPKI%2Bu54ccxY79K2jWfyzJTCOoq%2FFaNgp6IzaYxAiBj6rZlZxncsSKoSkIVt3CGdKiar0HG5Hdc%2BJ%2F%2Fpnv1kyr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMEIGxu5ilWok1KwvZKtwDmXVTXaZ4rsG95uSN7757WJXLmTKImR3R3fKYP2%2Fi1G8308oYVMXquBIGHi5QOL79ltuHNNyLOdxDhGAMyBOPhH%2BCtPiNJdQu27eDXv9GSqkL2NGI1iO2jU05dIWqzT9gHvTNAmkpZUgOjXOC5UsAg%2Fbzlya%2BNJdi9cDoJIq02HxvEaJGIJbiGx5mJ9ZbRRbc82ymPbviZhVM2wJQYPmjhxDMaoB6XTRBCgQvBChANze6n0k5sSEdW%2FKXjW%2BFsi%2BhMs%2B%2B5ilE%2FlehS3CxBuAl6oA13pnOPwmRvko3mh54JRmUJC4BSvYdLg7SdcGDr2NINldWR1YuCH4U%2Bpw5haE2wj91DVH0DGbs9HnX0KD3z%2F1VX8gnHBqEhXmDFcyTeoz%2By040JfXehXvf7TQZKE54DPXI9%2Bj%2FLzJDcWWnJsRNRR8bik3HXU9CkTcX2BOgsXxLesLvBzhbzv2kjk9%2FRJGZGPVeRk6O%2FnTs8YCdezrwOXJHuzcspaIKi7jdlUlO2m%2FLAk2truZMZvanOH2YD9szCAt1Vm9UiL%2Fl%2FQsLadVQF%2FuAPgKYZIydfPiRjreH628LiDcFU5GP1KgnoaYnRR3qIrKqW8tNnvkodj5RxCm%2BqTfEqVC%2FWMtn2j9tzwEwgemK1wY6pgFRLQh8maxlo1asgWxdkvPk1jg82SuNK2ldKPjssx5eBRNeNASUwJ%2B%2BhY7cP0LXOERIcQyy6Zzpn2e8Y75sN91v%2Fo2Sv0SaLOWeiBvjDdVZVftS0RjPs0QYhDe6ao6lx7l6ZHjkr9S9X8bmcg8lnKkPdeBwwfsHfgVD8bQFwDkb%2Bzb6HRFWGl3neqzQpdPiLa2909qdBpAGvOlFWJ6aX5xDW8kHbMlp&X-Amz-Signature=727bbc79dcc47fdc68126f5a15bd1898c0c58d9f636316d02b4078ea5d7f3c54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ST5QJZNQ%2F20261029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261029T032705Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJGMEQCIEJXvSxG9bbE2VEUcszPyLAZ9w%2B%2FTE8gns4XwNjMkr9tAiAtS2zwvkMzF%2FWYXUeiGTGCnekNWJbQ4k16z0mQQegHhir%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMs4QcC0pesstTVjoNKtwDVAyh5ddBeQieMJ3NV2NauaVp7cX8%2FOyqAzzON1ns%2FY19G39t6tzs5B3K%2FXN7mrhJlB2XCOCaT45VsV0vNdDMtUEKSOSUpLbPwVQU86yZPPEOlvnc7j6LFgQC6h%2Fg9FUJ6fg57z0BlhbEs5YjedJjzI%2FI1Is8wE1TyywEfd7Z8AobSVyEi%2BDUvYsPgaXr%2FvUjrQK%2B2m7HQuKOZ0klBos1z7epyidWaIbHX1UOXkYRJrcZNXIVN3OhKPjX6DXPDA99Q8xkbcxJnUFSK3f3JBY6228%2FH79tZGUh3dNdhERlsDhIrIaKpLTtynfJFhAnkX7k9RrcqnYpSxoagZb%2BFyYZ5GAg%2FZvKTQO1yAvdZYpbbmlSBE%2FGUX0IzXv6OveO8yvtwsMYHE3cuJxARCJ7DoWixCALuaNCh%2FmooomisHVDFzj1fSiXo7tfbP1lSMEEpad4wdfv6C6ZOdAD%2Fyps0o4O%2BoTX5crskBrxbtIZZNnvlreyw61338nGT1UPUVNCkZ5y%2Be3QkAHz%2BIb%2BBVaFKxde9U9H2zC%2BTYc9x38HpewSLZmRsqKTJFbgzkdG2QOWlyMP8ualDLhgt1ObAnWhdLpYBMhWQ3EUXksih3YJPeiXFiCmPjZjPHowIFK3hGAwhemK1wY6pgGNNE04urdgicOou2JPEgFeMtzv6KK9SCKpwSwgezSqDPSGnS0cfeZ6MvFjTTxH7HhrKOMMuPYIplPgz%2BgC96P4ugqFm3punI2M7IayijOn5tb5WQqa6dWO0ZwUBAdg%2FwXZ%2FA6KGtt7T1dXrFy6pODCnJrsizuO8RAUll3Mq%2FH17bJcS6mJmjLllfEH8plyF4HYAYZIT7PzgLB32JmWN8U7hbmaVO6E&X-Amz-Signature=fdec16d6865df79ad1fecd52bb620c2da5c8d343a8be5cc46016c22519d0dfec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
