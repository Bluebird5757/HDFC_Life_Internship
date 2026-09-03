---
notion_id: 3c84fa76-9938-8047-b507-c8020ca82aed
notion_url: https://app.notion.com/p/RAG-3c84fa7699388047b507c8020ca82aed
title: RAG
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-30T08:00:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-09-03T02:01:58.713Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5HXZVOB%2F20260903%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260903T020155Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIG%2BmAgnOi1FJ1LzFpVpMBxLQ6sXTdiEnLjhgTl3Hb5wVAiEAmOi9SYi7f52rj%2BCFpY%2FXwDtJIhsIe2Cex4NsjYSjWiYqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKBaTkSYIPrJkYxkeSrcAy2dAp9BuaBu66WRiEg0zHmKt0LFqr%2B26hJvgaQF%2F9znm2YDSCbCOuK47ngXOfYspHhL4sQ9ZmWlEuuthiPc%2Bxjx0lz%2BjABNafim8awKDZQlwzVs6%2B6a8CePcd4mX0A%2BnhV4npk8wZchfPhGnSgGN8ApI1V6lWZ3RSMAGePuzyq2bBdZGMa2gRnIT0b%2BZ%2Bu3gaYK%2B0DTzT0HzWqD7GUEKaACBtJVn4vawXt5v69CTeZxDGI%2FNCyKeDzazjMwU3diNMWuNFj8M5ba4kfKVFRANo9B3f6%2ByR0CMokFNW1UBvXm3fwUUjGKjgfa1ZXbJ6iGQnXzbIaFuXHc3ZFJOP34%2FJORIzbOAW7LRblrzV%2BReMU3BdulNJ5F4%2BfWdbDU0nuvmEM8sC2gdY296ySvjD8Texjhiftav0MypgA2l6Ktv3pKRzMUQ8zp7YnYBVmJS4pZre850LTXn2nGMJ8qmjPz8e18meofAWibVzIi8zphAgapEiDapRseLyF36uscJul9w0gysLYRjs0txgtNE0NGY2y89SIwnocorDs2HlcB4IW2O8Ygk%2FW5%2BIVoEK1hF%2FMbGzRFiXxMoRM7lW2oh0u0p1JS9GDFG%2FWxwXAKqDQfAcv%2F%2BiZV%2Fgf%2F0DLJJ3QqMLqe49QGOqUB6Cf6zD8BNIMb%2BM6VVttZwBSjiayXpYeP%2FIrriZz51rfOXhHXOFGeUMuWsJIj51JtaT89zpdhwR%2BO4Y8gHLx3h5Ln9Eub%2FNj0XRqU9MiNF2%2FW4uEkwTLI4nOEg2OHWrQ5NrKkdX%2FPg6ytV2nRVWeQqow16jrtf%2BCwDHDfmlWiDx5JtzA%2B8GZ76J8wadX9ZyNjAFmWhftUbfehGiwiDF4pP5rMDH%2Fv&X-Amz-Signature=c0b0ad7119e75ea885e13c97716d3859fa65d438c41276965ab882e0052fc88c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UTRRUNOS%2F20260903%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260903T020154Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJGMEQCIHZhuPdH1cQ0ech0f%2BJWZv3pPJp%2BFgooH36d%2FIw9ltAZAiBzGn0%2FH%2Faog39fh3f0%2FWPzMKMpkGl9pXqlgcGEiT1YBCqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0z8yCGGJwFStAdibKtwDE2t8oxd23%2FQgyFUdqfx07VDBqEz%2BuDdd30yGHQ78dgll%2FKRTjWE%2FVwM%2Ffk%2FkzQqD1mulrn5eFTzyWg3MGc%2FSAFq7l%2BDW4nsjjmuQr09zow1uqbr77MV8FnoMwhjyQe1EK8arwGkFjaMDE60VyLuFrMihIUoayHcoCgtbd5%2FDIdUT%2B0CoVtUcgwcnMxY8mUFMQWyp6ea1ou57AcivN8NgBY7AO3XmsmhKASqXdMcyonmX3O7gLelQdEHC%2FbJjydl%2BtEn0r7cz2s0gM%2FtO1%2BkyOkFpZSqCMdASHnKgdqJ3xT5JaKHZHqUxRzZMy%2BwPiQDqSfadtptxUqwe%2FNz%2FDmzyTfBT2m%2FlEirDW0cDqE0G6idks65BfKmR0252DEp55AAadVDRUZ3XaY1Ujf8%2FUjw0cC0G9crZNjA0xi%2FqHFz0PNZMnLO5fYtCdhSsXBrn9yG%2FCDUOyYSlLNFm1%2FXY2%2BSEeXjaeEILY0DTkvyWkPnfv0McfiUsgGSm4ny%2FaWOyj7b3Blu8DTHOO%2FNO0ztZXx48T6PJkAd2wJb4A0VfoJNq85Tss0TBVgdL96lH2IMLVpZPSBobr9KOWBZsmxIro6QIymskRTVpJwIf5s80d9l8mA1pjHZi0vhU4tloiJQw85%2Fj1AY6pgFr0ZfUisPBGLn%2FfAXTnfO9bXZczpFpySd%2FNbXHYDNGkCdpnxmnuXibkB%2BEA64KZ%2BeH5fOQaIL7O5i3VoCdKfvCeGPyuqp0gqno7H1PRbVTw3Zlw0P5expoMkperJ0V1fHDXnCajqK9zKc087kBCusIj%2F3WJtMR4WUyb06sVU57HFcELqcowRSDY5AgCX2byYkatx%2Blhg3xawjc08vS48q2MGkLwTFa&X-Amz-Signature=5e5274f8d9c69ea05cf990ecaa9b9fc4fbdbad9f9c15cd027e28ac84217dc58a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


        Database:- To the store if add features such as Distributed Architecture, ACID Transactions, Concurrency etc (eg:- Milvus, Qdrant, Weaviate, Pinecone)


        Every Database is a store but every store is not database


        ### Vector Stores in LangChain

        - Supported Stores:- Integration can be done with many stores such as FAISS, Pinecone, Chroma
        - Common Interface:- A uniform Vector Store API lets you swap out backend

        ### Chroma Vector Store


        It is lightweight, open source, for local dev and small to medium scale production needs


        Chroma Hierarchy


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T2SFNOCN%2F20260903%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260903T020156Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIBeIK2AIZtQxQ4P6BmLqzolAv%2F%2BtD5TbDY3jzFIX8TASAiEA0zm4gxEDG7z5YM36tvud5Akr%2BMfDtZMtVlZFFtv28eAqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNSJC6Xyjhe%2B%2FJDV1SrcAy1525i5j9ozSOsJZcJGsAuJkgulN7XHx%2FRiVsLjS9UxBRXe01n7jgrhWPseFLgXLY0Jp8dNll0qnEhKYWr5dcStmrxw7wgphVrA3iBNdzt7waqTBPbO1BdowgtEzN0aYhaDfefrKle%2BgcXeTKB3bpQvqPCXHtK%2B%2BfobB8qdacZgqwHk0Ih3h%2BOhTC0P0xOoHvKPOT3CsA%2BTgnhfd3GwKDFCTD1w6wSwsXqe483sN8sNVf6tXos%2Fk6Vz5bl3k%2F%2FLy7R2MoJMyHirkf4O0FOr7jCKDsGqDmSdfQE%2BY7qAPPE1xWN49XQouPUs7Ru%2BS5cqrt64VpFEXvz0pAH6SlI8gv3NK4KfznyNCLwx6o5YLRfywghr2crvMEm3x4pkj1BuA50GxedCAgAiYiYPPnr1nP0kb4AFi41g6XhBI36g6ECZZY1q5RdL5P4XVC3ipPXuy1tnfQKMMX3Pbl104ODLX0LbIMvIfSvEV1Sn8TBPNvmEEmPo8W95N2hqG2fOA3beBg4rwJ70%2BKnBAkqcUcOm61Ey18FnWI4ee3U7l%2ByoVQ0rQQLfBl%2BcVkpFlAVk4UICAyO2PSg6H2SmLL%2BG8ihi0ZJr5RuTFfMeFHQb50KgxNugJX7A1E8unvypaE1fMLKf49QGOqUB9Z6yfcBOZMMEs2%2BS3pe2ED1v1F1ShnyfopMGItvtF8C7JwFMkX3tMRpgbUAQ3%2FjQq65EBLGNIS%2FPi78OJiNt74fxth0c%2BtPVyic9Vo6Quv4QRzXnyL8ebY7NyY31YuEnZnHCZuPuyGvZFZVzZcJ9XNrcIMfsvVR5pXtJ8tFQP40W7JmBTxJbCDcmvMkXWTw0GyHlBmC6ZCEUBBTNodXtLehVIXkQ&X-Amz-Signature=ba815b4ac6d3d274506cc0f6e9b5990a7c467d6dedd9d73b2d028f292dc97088&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
