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
fetched_at: '2026-09-17T02:23:14.691Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662H2QRLIY%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022308Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIFha7RDghEgNHhwK6Rf3D%2FNxCM6yUeAP1R3Q65ACXurlAiBPCQq2gD4gUvdmj3szY6S37AaMbhFGncIU04tDp2W0OSr%2FAwghEAAaDDYzNzQyMzE4MzgwNSIMuaXn4hdBmYFYnXZvKtwDSrL0UXCsPJvWbrTxj8BHtth7Wx66xPj8zraUBZoauBF18hs3jxlpU36RKJcvdEzjInW%2BUsw2wOv2Y7XzlmAgafPbc9JsWgF6NtLs0991qJdJJsw8nDHGG%2B%2BfdA899jTZm9Ny1kdGT2UXz5yWmz%2FOrbN%2FaDYna8fVq4eSoHfsyo9dkNiG%2FxY5o94V4nK9xXM8gzczxBnTuUWNkby6sxr%2Fa5S7hvNPqyKgfVPpitG4iqnsriP15EfvFex1RxSqtHgU3WVUFDM2Rwa2WI74oqdRn84kIJT5G%2FpmX9Ivxp%2BOC%2FzQnXaiy1h7Ds5v5XWQ2C3QQ8TIac1YRkx3dWGcRQhijypsZU%2F1Gt2pnbovZB%2F2d%2FLg3fDW0YGRuJHma5BuZ05ZJyiBoFRzrFqNtO69Bgk8TjcVyqIWmCIRtxN2QM0x5SHuMI0Rg0fWMoZeuH1bvoF47kjk3%2BE3IsPq2BXPXDWJcBnzSXnnfPbZc4D6zK24o89zXKbFKal1K9VurhQQcTp%2FMju9HqzpuySp4G33xIFvgyXijGcSzBBnVHUkbL4zHcvh1taef190WV6cXkV6z5MVkcxRV%2FZ9%2BG0g9ZSasfgH9%2Feke3vupXa8RGJzZQOF7p10TFQfur1IAhTBy0wwseKs1QY6pgEk5PlUHBbOoaCk%2FeoQIbu9Y1tYT3c9bvhN1z7ALDMStA%2BuBePTtpOaVUuyMjPbDFmEBEUpK5JvZ6%2B9Dan3dEx7A%2BacLabkod17xEkWYjnZgKV4xtAs7%2BG9EKe32eaPct4K2EjDptHIuDHEYFN1emD1y7j3m8boIT6Q32d5y5eAtnR8wL42WquYY3hmM7YKQNU8w2SCbcOTu10Ek03YgkxWbMGU0r7e&X-Amz-Signature=0281774356a9e29b3f89da4bd3af419fcb15134d3edd8dd01259eac7e74cf68b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YTXQUOXD%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022308Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQC%2Bp4h4hpGWXZ%2BLb0P9upRrChbvfxt11l0Efxn6mJMF9QIhAO0ewVdIudUnbrQTCdRlbJmopX6uXHAcwdHenh5dDeFKKv8DCCEQABoMNjM3NDIzMTgzODA1Igy69ls8pkvhCDXwa2wq3AMPY%2BUSN%2FbcoJqLMHCh%2FqgCRqGlr1%2BZGbXOrPFUoVwJl7r%2FJDfA1FonpycKuaT2C3my8nrAU1qWlljrPN0jZWizbq0UYHkOw%2FXV84EORg1h7%2FX%2FzBgH%2FR8W%2FOebgeTSNJfaf034r1oNAM0iHHgOJ4v%2B6wRpjH%2F%2BoFTmFyQpSJm0n3ixCtj9y3rIE%2B8B1wAT7Vv5Nj9WtssrTezqLBykqfAsQklgODRilke7fyqHy4phRY9H8XYJUR23C6JDEpkkTExXY5Ib31MtPq0Au%2FFZcTsKm9xvJZLxPXSXfC%2B3TgVnP4kKb96w7DBNVZ472efB1ZISX4qcsnds78IRwW7U7A8%2FbBecSGpiBuDPKV%2FHA4imcVzaWxciwFsze0MC28CzP3%2BQMZlcNld9h1c781PheJ%2FZKEhXPRAEjxmcnIenIuPPlvic%2FdMUm85Cj8GWzl99x9c1eHiWuCw55Fren0XunjdnS3jwLwPpyYlSE0EMqcqrxQw6FJk8BJUxnjPGXAV9aXR1B61czcCyK5krlLD9Ith6DbkeajvJyEfdquU6ReSAbWIQzrFBpDmxjpKGhK3OMsMNNoZGts01%2BT0rj2JakDuNRx9rZOeLmIxQauwyBGy45RpcVAkNpVMMVQUChDCz4qzVBjqkAfKqz8lHoykg%2B0Fsm32uPoelQX5wWaUSprrfNQi3rD%2FPgZ%2Fw%2BMX6BTCOjIcm9KqYR92dCWT3%2BzwuluunfIf4X9k%2FoK9S4dcQ6BHWvfntB%2FQofn4xZvM1M00lf4BiqDj2e%2FRrj7Hzk2a%2B6CoommiHc5eYgkXm9xokuLfODngBtKINNArVogPBKAYqZXD9NSc5BLD0ViGueJKRzR42UWOAKlRPReNf&X-Amz-Signature=fe5bc7bd4112e1747e516438f97b5fad440b6cb42f82b16032bfaf98a0fbb224&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664453RVPL%2F20260917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260917T022310Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJHMEUCIF69%2Bn%2BBmpX46RssiiJ8zyDiFYkZAC1nwm2SJleFy4JQAiEAxcY74eX9kyqrxJXiSgzn0WEqJQnnlYVPw%2BRFUc4QXp4q%2FwMIIRAAGgw2Mzc0MjMxODM4MDUiDFKCZt8JWEWRUHbQ8CrcA7ccQbUHCq6eKA3B8sLKaE8qHcXELcIYIp3Zp1i5PvCLac%2BLkBGDIfPRXGfVlb5DKjggskP6W31Uy0TBgjFj1Wi%2BaS9F7KQdGZZQHmKNvFfB6s%2FX1jZx1HIyrDApmdPNE0B1AJw6Yp8EgGNO%2Bd917TkmfkwALMWEWyxCVa7k7KCDYJz9cDnRVRhn4nyYsAcHaaDgeyV7CpuBz%2FfnQ9LDmKPabMwsRlxJ8muGHmz0snGx0tkasyX92C6382OpRAxK66wlsvRgUwR6vOsdZIfU%2FVS2gDNHMX%2B%2FnvlQ2jz7zc75Netd3s%2FnXq4OEX88atNNnMnKZGwqkFSWhcb96t69wGQZiIFsZCk%2BJQywwpcoAn2ou1IROOYmtmA1gOjVjbOKqlCs3XkFSUTi%2FSrdPmpvOgcgqI35Ckcod4OvRckLeWRwA9GDcdSkNRcr1%2Ff8pEQ1KTS5%2Fu2TZ3JnpIiliMD36BM70TpuIH8ro4gKfV5Uawz6plGo7Pa7jgxfri6RggQiGgJm6pSLrijM3ZAXYNytUdYZqIfInsUs30nyCSmXe4j1cSISUsRaEBjrhJEbBuKCvw%2FhSs774c%2B47de7Vy3DZe24e8F8uL1fbfSU94ft%2BPbcvLXrSJKVM1l4HPeBMLPirNUGOqUB4Iph7Mcnjz5wqNo%2BISqVn5IcSZoamu4Jw8pltfipb%2B2P33zxkr2MJSoO660qKJEaz5J1zKvQgkPlgVsTZ2yqGzdQzc%2Fw3cBF7XMJDKUbVvugAGlclMIhgs%2F%2FKTv6nEx6WQdcPb5vXbNq54Y%2FxrBU1rGw2A58u5C3H5ka%2B5cKA4K1mMH7PWx%2B%2F4oTEKom%2BoYOYHbvgrHHJGwgxJAZPI83NcDd%2BT6M&X-Amz-Signature=d7d277de0c7901d3791b52e5113d6cfce54ec0caa2a19fd2fb4a287a3bfc4da0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
