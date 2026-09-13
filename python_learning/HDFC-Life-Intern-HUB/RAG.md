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
fetched_at: '2026-09-13T02:01:38.384Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662SSIWV2M%2F20260913%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260913T020134Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHJuTFY6P7ICaH1GUR%2FCQexuCr7VWf0HzfqeV1QEm9XOAiAMrTdO6NDnEOFrco4pazeFz7zoQXDPzKsHHQ6S%2B9JXIyqIBAjD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJXE7eilTJnyEEDzQKtwDokIkXht2ecXmtUaHkViaSQOsYcx2MGVdpIs6kyn3O85zwZZsbFHCMGYGCh4dwolVI4I%2FCDCLNAxYoTy%2BKdGqPCulgvM5bqwZdF8KyWDfvIkOQc01QCz8%2BfRrCm6XSRal7QpNpphm3NMJ841ghwNJdLJ8%2FPvsNFDGYgYweLyiRozp2k%2FS%2BKzmm5KY1Q8bvbPxtsFRhke8ezr88FzEi7p9bCvgG%2FndVcfHckD9tbTASpJupfXkP%2Fv0wVcNftiQfiixJqa%2FcozdKvsuStAd0E%2BztYjfjXLKSVXGRh%2FA0HVQG7%2B9JQ4IGaDZsKU0%2FWF47PHcEoANDqldG%2BtQvSF9QBTrI2GcSQhlU%2BGpAGqprwgASVA3qVfMYO4yPwwXy4iqHdMRXyHkeD2VZdJhMt1tPPcZMrsFiRfJRuazW%2FEzoDH97YE7Li5YHR%2Fs7EJoSerH1e0oi51rWDZC5q6%2B9Rgp6shTrHgf3%2FBnNHjWoAKtazSh8lWY57EojlmvDbGXYldjsA6ktmfuETLOEqaSm%2FvVtjgDOlgRm3OYEQTh9C%2BaHC50bSX2toVPew70JvtLC5PNj4BDzyTsitbvLjhpHNrjamFB6j0Dgw77e7TgHYOWakM6S00WWQpAOvRE9%2FXqHuMwx%2F6X1QY6pgEuqnmq3LbVUuBOJhDale38U6b5DqnDS%2FEDfLYgYxurPkjVOWCPoyWb90UnUdC0q8bWdX81Nojj4%2B4jJYGyDjsGZZuBjWCZb6Kk7PK0HhycJM0JWTplMT2%2Bo4VnHBgcscPc6JeQc1drW6Y9oI9lCt7GFk3gUjBLYwOQQX7vDKP5HGBthbXl2sY1%2FjCL17vwfRP38LhbO5lmMPbVfc1RK0auEIo3ofZq&X-Amz-Signature=df741f3713f433969c08d7594202819f20ab1f712a300118334118b832977763&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YNP6P7IY%2F20260913%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260913T020133Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHQ78Tkt%2Be2CdK%2Fn5TLYKP8QFrM%2BYOQ1gVz0UXKlH1AAIgbv7Q6L2c2ZpTzUry22P2yquaj0ssSKPo9QSR0qOhXA0qiAQIw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHCrKEhIi7lnc%2Fr36ircA%2FpC4GFlb2ZmXesgoIX0%2FuuRhzhY7Sgf%2F9HfoVWOeozQuLB2sQkhHNm1CRKzR8Q4ZklkHOeiVmZ%2BcqRVpjSeSYIznrqo%2Fo6Ql0fRdLezpryBciJU3RX9gewt4lleWKzTZU8oEsewc8%2Fvu6ogfJ0qDOlLOmm2C4ZejNWUxnw4HxyBiYkJ%2FIXfrlBN95ku40D%2FCw4kjvD38fqJk%2BaxMjcFlsm%2F56rgBFTgKZGE6fG1W9ZSJ0Ky%2FP3jhN%2FYU9v%2BtFwTqorgWMl5LL1P7VAv47PuYff1iQAbrDwKNRvV1jidNaWlL86qCcAtNQNXZclfbIkF6NcNZNA5YMRGKu3D0PPco2fh0xH6rWbtDUNWY1RAxbPNMEsC%2B6qK%2F2q9J0j4Lw3KP9k5DGJq7%2FxUFl6d%2Fcx5W%2FHrlznVX3t84cpklUqvPkvY2bQtyfk2QTLLkIUjes8gcSiagSpMmk0rCOoSLYVthpIaGCYWC0K62J%2FtRcznbpYacHFmBVKnS1dMyu4orLMIbz1GOluDLQ0dW%2FfqVQZuI%2BnjmDDgtSTvIqnSNFPCSq7ignaHarne4z3qfSAem%2F41xQUfr1F2mBUXvieT2bGlQButhcZ7ahMBga4Js8ofD%2Bn%2FM8BoET8lzRHs8df%2BMNH%2Bl9UGOqUBiZFB2s4cTiwbisINCiSstgWdiA1dFMR4AHkdr%2Fxol0yws%2FvHYnChuND5JXq92mftFkGDnJcGWNUvZSGUZ%2BmBZ6IPIvSpiKbd6iOYbwYSPqWXA2DVSYVpnTJGfdcZwiyFQ9V%2BXFyFNUBpHYJHtem0%2BZS8KxXkovBFYu2b9GJq%2B7TZf4dZlh38z1Df9opTPUVRq1Mzm%2FcTW9xrX5F5bLK9M1h5ZMWd&X-Amz-Signature=ffa171aeca0bf5b318dd81c7b8a0813e92f764b7e70c3992e71d24c191768a45&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627YB54QF%2F20260913%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260913T020135Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIERi4yqPvI8TtGl8E8gpvZDlTSbAu60MKghKfLAWHwF1AiEAqI5FVBgq3Pb7PIioRnOSTRan%2FlAKQAcryv1k%2FcQgIbUqiAQIw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFukk159avzYiqq7circA6YK%2FJrC2OW87sxoURqCLz7gAazqncIV6MtIxK5aGdEz7J5njlJfI2LwJZezhQ09RXPEqZaDShwRtd%2FhQxdiupI%2B7UFIxBn65pEoQAi9A3DN1hRT6FhTNsF4hRmeGZT4%2FlpUmGWN3aJKO7YkXyz15YbIbm0suhuqa7tAHnYOMLEEEmJa8stpnQTlA354G%2BZDMGb2u%2F%2FikdsCFVUuNu%2FKRmnsH%2B4AhaIQXUbtuxjg1RPoeXL4UTCjNxOxMMetPpvlUh33aKuW%2BIV%2FkIQpTDwhGw0XKxxk9TLsn%2B8y%2BaOXTqMjSmlSUAJjrftdmAw48atNYWR8Bp8Re8J8JfEG1x3c1UbrAt9R5rgTJz3oPu0Twor9DvCss6MkCAy%2FLprrA9q1uPytdiRRklHB%2FdRJSgwrSO6DDwTchnjnqMq6NF7pRkPfwuiFBN%2BKGUQbtycKmKP%2BvTweyGFKs3TO1D14hz2aGnBVfWOwWntYnhw4ukjhSOMnXi3IGxj3thWfCI8Yp%2Bjc41EMsrmIKRH%2BxH5Obi1hU9%2BtdM7Bn83XbAzgvlN7Rap0zfrcOUFFo6BygYh2VP%2B8%2BMnaEB7u3uqn8Ss0CYC4Iw4fWlsA%2FvNlfY0NG25kLaL2X4GBk47JhReWdrXUMIj9l9UGOqUBMpYAaW6pPjaV%2Fbmtg%2BPSEH0WFwXy0nieKpWNUYg0jx%2ByD6Mee8wtMV%2FSGsxHAp7SMqlfCi0j2opxlxx3EF0EgFkJeEcH75ma3CCe%2Bn6ciTjf1hfAv9CKMb2uqRef7f7zgQ3HTnCJHzVrk8WRdeXUi5pieD5ZkGLR1Ame221wGwHO9ifKDw%2FaGz%2BR1%2FUP6yEksiA44S%2F1gNN0iuJoCKALjhUKqPgn&X-Amz-Signature=7fd68ad6a41dd9a8a2a3a9932bc8b3546cb16fdb06b00748c6d1fd53c88a32c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
