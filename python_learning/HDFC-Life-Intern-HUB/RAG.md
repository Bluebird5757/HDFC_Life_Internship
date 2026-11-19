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
fetched_at: '2026-11-19T04:59:39.595Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VAM43Y3I%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045935Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCICPTyCOeTDzab%2BZvJkk6DBr2nqAsJhQElmiLnVSEdQO7AiEA9NKAXl8KCP79FHfw2JCLT128EgKXwICdnfXe1nAG4rUq%2FwMIDBAAGgw2Mzc0MjMxODM4MDUiDDf7RMrlXxnJd2fY0CrcA0FylUeOvB0%2FDKoXohtxZR%2FUUZv%2FA5ZCzGtX1%2Fmv1eXonTYv6jbpnumFx4g5VTJKacokSy6IY3SH3aLdj9icqIZnxi1QlfDoh2T9%2BBmF3TkOpY19NTKmK2%2Bf3O6JEw1BQKZYOhxwXXDFNdt7NnH06r%2FgGqcpFOS1eIsPYUyR0Q9Sc%2B0sXwnLM9YfZgTAG6Ebi0UOOgcT0%2BR%2F68mLT0MeBNhFWIdJH%2BjPaiYhRETAeJ9TH1OzYNccPQ6xOxTTweClDj7WS2eF%2BWLKMvoDs52PDGrqmBFI8nC0bmDxnR7XB45jL5Cz6as3r%2FqdaPQESTj2ANFI%2FAW2Xtvx7%2Bii61RtVC2T444CE7jUqQ%2BMQGGeguYqU1kBKSdNOYj7FZyBkFZnKN5INJp0hXJJYuPjpqXVj9BvjLn2Jd50leY3Bjtbt3Hn%2Fu9ETRvvcsbx%2FS4He6VtSwjd9tKbBEBRAZ2UlA3b0rpBDYRbbEnvH7xvcVooH%2BYLtfH5kSXk%2FZhLMHkY5mHbR1AcWrk307NeBsWLadA9YfdGZFUvcEqPbdwZ6YRAf2XoGoanKUvIU%2B40pjOr7mmkbY%2BhGkH5J%2F8%2BuogHMhYt1su%2Biy%2BIVw8l03O1yhUw4KUlS1%2FF6yWtUuQkokw9MLTw%2BdcGOqUBebNVpRyVX2KrjzPfkvTYGeUsNQPfFpn%2FzIgITQM0fVeK0yNDorECdn26IBomNcycN9iFqFhASTzu9omEVUb7iy9%2Fc3w1L%2FPzkt%2BPdvhZWLVACxGBVZou6zD0%2B8wFRX5%2BxMttGvFS8CNK6QLFaTOR%2FtnTziF%2FH6vdJcE8KIOzYRlR4IByjDGbaECKe%2F33DdHrWFxF0XNxODYCtD4WdJ4CoKB%2Bgovx&X-Amz-Signature=207f863d0fdf44f97a19a7f9af4829bf1c2f584fda53ac3b4fc6ebdd1a2af976&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBPM4RXA%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045935Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJIMEYCIQCwRYxGNQHruY9radTVVc%2F4UIToZio5KjPsOneSsKFqhAIhAMor%2FhG3IxNvfymMODnKaq8ittTqeT0jgGxq%2BSQNojeIKv8DCAwQABoMNjM3NDIzMTgzODA1Igy2Ts%2BbdFH9RAw4Gcoq3AOO%2FPc05EqfUXXM1kDeszozUk2ewObj5jPYbotAwtTK4Zh%2BGhZtBAD3ouMilWNis0Fiy%2BjFcs2CQ%2FYrLy1wxy9oLNu1poxlwv8J3QXYx5kjBN4pRgzTOAgK4HCo9ZeunBBzTUoc0X%2FIQgH%2B6yr%2Bjt2Nf3Amwc2Qec5vGefYeu8amE1l%2FKDAzSI3I5SnWnrwcekn7Eovnzc5KOD1ZA1vRIOeharL%2FVDugWOnhFKGEkkUZEFNmNwsxSmMb%2F3b1%2FlPdA2vYrGsq8wOnWjbzjMeT3UP6yYyb%2Buh7L3vjr1tsW%2F%2BnEiW6ZjFp%2FqGHwUa5QIuxCI1gpJ8arT%2FUuCV%2BwCGs2rrKO3GiCGseg75NXyT1U7b4%2FFQ9wz0cyOTBv7pwkmlemkc4GlYKvzsI22YUOTX62pDMdQOMMr3xfTGezw4nNu%2Fk1EqPrMmmhrp1RJXRFAUCPCr4Bvz7vruFZMpRkxSIa%2BURgT5R9SubqvsUBhftYGs0NUIe3%2BD3lWR1qTfrG8o3yaJIxwLjOD1ZFEWCtldCONe3d7hvAIwamtH%2BGAi48JXpfyBjDBFXBHmJer%2Bw17xVfRPKxH7NfVmlAnULuYvaGsDT30k3pBMpKt3scP38iE4myiL1c%2FWnRqoUOzVajCJ7%2FnXBjqkAW8qWHFBhuMsZYyBcq5QXJ%2BIr9IhwYdyMNb6oHEqqvBbrtnTbbv1FKJ3gYv%2FEZs5RcVddA%2FeW5uwS7uSWP52Z7KUrF%2FzM9KV31kXo1vh5WrHMeDwJy7kLssjGqAUw8%2BvIzL20p%2Fy5F%2Bv2scUdnMkXCn7kRJDkq%2FZ9HnFs%2FpDY4dPbUG0s0Z%2Boz2k2IbRkB4qwJ3tRQc2Fld94uze2bg%2BBpB8Z0KU&X-Amz-Signature=0ed029fc6bf2ab91893acb15a72d05a1e8a04db51a07da299e01602d28d7408a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TLTBI2GL%2F20261119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261119T045936Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJIMEYCIQCP%2BS9sO9D5P731%2FCKcK0CF0ItgzaLEXNAnBWVfdAS9YwIhAJh%2Fx9YZ%2B4lzMn5B23655n9xitDOyPW9jLS6HwawnpkPKv8DCA0QABoMNjM3NDIzMTgzODA1Igy77QvOzDqYtKKEmIIq3AMLWbKXnqsVz%2BlpxvZkIoMelP0nRdl%2BsmogfurZUuz8%2BubeOudx1xik70YYHtRtBx9SLMsDYlie7D8g9XLqcKR%2BCDl2Y4ORzk1GU7lZ2Jh4nd2HK9R0oeBuz318t1W8ka62fX5eBFmfWGRyQ22OrNMaoIHpMwPCQiYBU8d3N5OcIRKLiWUXtFYYumwNMKM7f7cZhcublJMlLmVoJtuzBTUMKlyjG77BFER6o%2FzAOWHA1nCJqYkAawgYFOzSZc%2BtF%2BwlqC92TAPpgReeIfFrNqD1OELOwBKxryzXEYI5Pr716pAlpeopGKk8cOlt%2BkRHQWHvrnmS3bkkRODGy1d2Wy01539c34dRpuNgDnLP5MBkgGsJ9P83hIzyi9ODsQDdpzGzxxGqSDNxCnkkeseEa8SNoyCfLscDxJWUF%2FxnlhiEv0rOLrXrudHniO6iYJpG0zdw18CnWaXiYYj%2BuKLOWJSTjOM%2FyEtiyLHiakfEweGrA9UOl%2BkUzTQRw7DZsYzCYp%2BoDeWSN7Q9zRiEwxItFEBmxpKG%2FIOoutUEwLaXu6sibFcEg3fCMVt%2FSeVRP5zcxXvt25d3s19zMbaO2PYnGvhor3gGdSH27WKd5hEi8pd%2FZupZ%2BhjbrYynlq3UJzC47%2FnXBjqkAXN%2BBgfAZU71flYZNF2v5HuuUpz491nkXh%2FtaVREL1crYT5ZDJGGaG5n4oM7yRs5Sfofb63Rj11LHqfsL6svmzlbt53H90wclT8Nv6lDhB4boO3EzurDxQX%2FYdxMsKfTRIlxU9ORADwhNSxQt%2BDQXgntCWY8vzy8kUBmv5TLibmkHjR9lMJtOyJ2KY1U8xH0QiNPQyzxCYrufycQRQc635ZoVezc&X-Amz-Signature=c9d1a0528664d1867d35b8a58f5973bf9b0bae5770b47903495067043bf12358&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
