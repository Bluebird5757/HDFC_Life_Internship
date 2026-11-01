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
fetched_at: '2026-11-01T03:11:25.281Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QMGOCZ65%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031121Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIA7WC8%2FT5I8Bo%2FJaF%2FUaSRLVDVvotzAbwb2OOlPPmbQPAiEA2oermdIYtpb0jh4CjtlPsrrxFmu42nsYllACw8jdBfkq%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDAqKZB30LXnyHuenFCrcA%2FKWKc14Lea4F6G7yWF5UA6SmTmGUJd5iIi0YRSJyr%2FhIox%2BsYOf6esGmtB1VFb2scchCUMer2LhwYUS3ib1XJkFAkjrQXR1lRYrHqXm8sssZHWciG9dXJO47yc3kSMDnC3Y6xRi04zdmwZnTpWzMGF8Npm%2BitV8U5S%2Bzgq8%2FeRCXyGdREa9AIF9hfthWzqTMIghGfxxel0zU5NPOI%2BbrnKWD54OjLYL81ssrAArJzUadTOq8LXasfCIn5MlB00uWFdZnLskzkNnKT76YgKkK6TqgBq%2Bt9kbLDPE50zmwKi2T0Fo24Oyu3Wz%2BSt9YhkDL7KG7LUuBrJbaot515yfr4nbeb9irr1Xg9vLvBmeOTcXX6ZOKWDNgA1CUd5tELwpxqfkfAT%2F16bpXi9MVbAaV9%2B0hg9dYGzd9Hf9paGKI48bQa2Y1Hpi2odh2CaYcNIREa7PZONpIhEgT7AY%2FAqHd8jykxUJAzWQJ%2Bg6lswfNn60sYlXNTndxQP3DtPPjfrcDr963PKUnQWu2zaCUv%2F63znGaVaV5GV9vAWkqriJMFk2Lw1bCCGQ9qEJRpsaWGSN0%2FVKll7X83EdEWUkofwYJbdMmPZfBU5%2FhEXdWbpT0LFvf37KtaGC4GBZ2%2BLOMPnZmtcGOqUB5RM5wx%2FtvLT2dth2pFmcxkq0tqrFC%2BDCAdiw6oeMFHD%2FEtBtWP78grOobmr6WQmmj6gRIRfq3zqGAjQkhVBIn56cglzfs%2B%2BHNChTBCuGTaZaFccN45Mr%2FSsW5ScR4AKvDUkkzo1lkbyFDiZ9OdvKby3%2FTiJaJxNEz8AqUkbiUS8jciNUPQHyzVIeWWquYeOnbOWDHsHMirEPYf6ZBCO%2BEpst72hJ&X-Amz-Signature=11268f553c822884951b82336fa75bd9846577f61a615b50aacafa1dc645982c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGGWSCS5%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031121Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAZ9nI1%2B4C4PRD%2B6Cwnba%2FMn1caT0VbqWL9LKoodQ8StAiBmhs9PNxT6cUIOhHQUskoyBVZRKwkqdDWk1moMCzyGYSr%2FAwhZEAAaDDYzNzQyMzE4MzgwNSIMmniF8fu9a3G9fJe%2FKtwDo7HjvBSlPMMdeQx%2FY0013oi23DJBzFixV5pvBMIJgicAwa3IAZLdACKpBLwQjc9rQFkuL9LiQxcewD616ZALjrt0bT1d%2B2fItKVbBbF%2BE5pHYfsTnSrJeMG9mUhrQJ0DelI%2FzciWjwgnQaZmtsKLoLYmoBeaBVPNXuy7Fk6UD7bdpa9WnofYkRH4Cx7obELx6B9K8xHVJBu%2FszVAvLFLK58Sw4Svy0Zent4qzR%2BfQ04Zno2D1792D0indJgOOxnIm97Y09eb9d7pTLQDjUcN5acX7W66XAaXeRX2%2FsvRtQgdfBak6zYb%2Fjc12JVJgBOsn9QxylLo1z6AkotKuMjgrlRStOS2G86Wyd498a0PI6StouC3878r748AsFCmRCtaeualftwqw%2BTRaOmQ14CvyMyS1WIk5ivuyi1wGrqr%2FAbSCocecPWDQKKtJ%2FUO4cUsK9kjfGJMUtX8SwJMcDN3adVpcZd1fFdQhuyrilHXVpk7ZaE%2FhJXuVJ3AxgHc%2FGCswvvyW3D1nSd7b5bffRdSQw74hWihqF8uEFghRsrC1DyO6jhANJ1BqajXAx3ga14M7Hb3sY2T25ABGA2iDhN7yOG7MddD%2BMPRKmoBgwHTtot3vwrVLe%2Fa%2FtfngpUwn%2F%2BZ1wY6pgFJsXrAowZrfczfq%2FhoLLJ%2BUtSsTJc0zuimULq%2BHuQ5%2Fo%2FX5KCM%2FTGDY2g3LmnF3yeXwoL56viyVSSt%2BWUXCn%2FLsbBlWF6tARktnU3BhfDvh%2Bq18bTv%2BqPsUFw1Oj1r270x0wtC%2B0728YMUPTEzBKpXrQsymT0rAIiUzr0BRYeSEp28DgpGaatN8LxXTfSQZR3Hfd5%2B7DPtIjkyEf%2FzEx0bozGyrL0c&X-Amz-Signature=145e3ff505c57ef554543848f49ae163565377f14c667ab7a3a75e569ad4d612&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJ6FJBAO%2F20261101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261101T031122Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEHF0oLkCoFxIwur8dfCuzIOZiYuzwSFyay5sOSqGVLdAiA6edhJOt02DT86kzf2Y9z0Na%2Fw7A2%2FdHSd3PknDPKmPCr%2FAwhcEAAaDDYzNzQyMzE4MzgwNSIMl63MutaywXPasUaPKtwDeQstMQrdnO11gyp4c7hNsMu3iB6IiXkAI97%2Fnio8dRm2VXXHSucV9O92RnfW0njGAmXCrgTezctZD4hVdFIwYu5%2F1s76w9xitmFSy5XDug86qlrYSRy5dqltdaE33KvbqD%2FkVeGCsnTHoPmhvP77MkbRJAVGbVP6Xij3ncerPwmeCH5fOyEPUZgI%2FM2m6xaFZ%2B5c5N349d%2Bwdq%2FbY%2F4I0SCWsJL8tibP1tDRJiYNRY809vzrhtmXuaI7eQjUOEE806jSoJMxTwxDcnmb66hw2KhL%2FE5sqjUyGsx9zZMKHVrIkVCPZM9QtGTMmnIeMexp4O7piitaeim31JuSNeqp7Qn0gj5dwd2sXE1aT7cwxBrxsCTKC4cL%2BZZvEam5elEoeeeFUQYmqMDKEnxIL%2BH1gQFr5ODHcSDCC%2BpEOf8Mz2pBoi3BP5RJWh1C9RWVzWQN221%2FMIVx4qM%2BsA4BPQ6pFdQKiCCImzzGfye8QTVks7bKuwdT6JepSsAxXn2X%2Fkc2Au2fv1kBUdw4nygGleeFe1qud2LBZ9zp3rhriou2L%2BmEHR6GBtxkmX87obq2Wr2JzBUSf%2BpGBWz4a0rEfXdeQLHV0Esr%2FSxXd375VxGGuecaowJOqIHmWo5SfhkwvNua1wY6pgEIbPzfwVDeGoTHc%2BkWWbo4ed2l2xaxFjITbl63tqcz0iBBikpaNyssBBLFSpCRnk6g6TV%2FOdPSpBU3vwbPpevMi4nifj8KlJeW07Uqb%2Fbq4am7rklZmDXix%2FTvuGKOUKS0uRh0asCQML7CxaKjynMxcuAZjFh2RyRJb7Hw%2BXVcSttvoXyqPldeedpbeLKu5ySZ8otG3R%2BagMh%2BpR0EqiJMEVnvtsn8&X-Amz-Signature=5b12f4ce11b85aa337ee48dfb8803b57b640a4b6aa2b550a5703ae6d7ff89e71&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
