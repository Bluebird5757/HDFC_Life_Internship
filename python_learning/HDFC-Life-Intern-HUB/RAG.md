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
fetched_at: '2026-10-13T04:30:11.367Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWY3IE5D%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043006Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAn9pOJ3E62qfEVwQ3TXHF5ROnw9HN%2BjUPlp61%2BGPMjkAiAY10opOAuqddAiJPvt5EWfPokyXUWCDD9sVl7zclwgByqIBAiU%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMvyBBiU72gpVDpp9XKtwDnZEBecAlKmGsnK%2BhL%2FMroXl%2F7eObmbucmRj54tl5ZSy%2B%2Bp87uThnY46b78E5m24%2BqYx72QWvIsRZ9Rlj8imitqsyxB6L4gfer%2Fh1n%2FrTSCOQzb09nh%2BudieYTe0ZcnmmO%2B0LIvt4Yi0JKamWY%2B6ywMscZQFPQJjZFODxXI5G1tSlGQ0AcMFcaTEMbvZSck7A4m%2FlToD2L9WGv0BNdAVX%2BxBOyJdDOoPs0lCLUQtUTBTaJcWgbTwLQ9OnxIjzGabvyhN6vg4R%2BpOvmTm%2BrYGdvKDNYifW69LbAwdw8NyON%2Bap7o2yF4UZTnRDe18sEzSH34HohLSCKeo7k6Peu6MLDqQAtNknlTt9%2F%2BN4ozLcaJ70CJE8TDC%2FJW1iB8t94Ha2KN0hW3cL%2FWketLKncqzr1dXA42U1PWWaiIyULH%2Fw%2Bwqlij6SK56paMx21uG7PHptKWSLhObZXpPKI37oKOEg37%2Ferb06w0rrncZdlpXSVHN38qMdf8s2ZhzWDD01xixiDnV4IJE8oeno5ZPW9abk%2Fop87iyRaJq7hp7ZObMhLs6r%2F6GA6sx3c6hcHov8gxJnXSM%2FPdNa3pps4SLkiKkJVunxnauD%2FrOhb9XHLxMd3uFUtbqJjEFuVVAwJxsw2sW21gY6pgFZUS94fjiyZ%2F3PoRn36vhRXnP12EH%2BUUYOyPqabtLs%2BLtEG1tjmyS2pR2h70aTlAr3ye47n7VUuUrWvzKvo7GJASA11txC6bO2d%2BYV%2BRGwJDz1wBDP84RCtasRfZcLugE6JeUtFBHzRPVuoRdwgHK9xVlLi55O1otp8wmuSpGa66uKn7pu%2By6fykRN%2Bnm78XB%2BkSdktsSmGlJ%2FWZfHPvvgNWCt3s1s&X-Amz-Signature=3d93a936b16396acaaf7087afa714d418327a6c8e11a75647b4cfe85ceb5b2ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632U2RXWE%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043005Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDo4g9BsJqOafak3xfnocCkN2rZczO9QpUuL6K08mDdNwIgH2sF1jqQTTtJE8oGjqHvkKRIiopnbPbh768jJ4ZJx9oqiAQIlf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPfu4bpN3tAFQFRFayrcAx2mjeWRGcKtJN3AY3bVzeXCyfeCbeeOUzK3hDUNjykJ0wTGdYDkxgVnys7D3FoZ4HqYeWPItlKCEuMPyn69LUU4UiB6D0vCJf77BYZjQJy0dnCDnTgX5WQZ2a0bIVY98Fa%2FD8hUTep1%2Fao4eLdzmTY6difdMGD8xkol3AIRIxn7QGbLW354YdsArg9YZiEwGfHcw8lMxfpXecY4jtPaxdDPB9eWZY8KSasO%2F%2BGXlCd9rjqrr9e5Bd4FNot59QfD38aZzzOaaohas2V1qwKsgNAswWJX5o3FpiC3dZ9LZN9dZ5uK%2F%2Bud%2FU8ZkEtHZCVDUwTNCAjJNZv36I8qSaMXah47z%2FUqVupUIUbLhTCVqsg1m%2Bewl%2FUoVKHwvaBaTTlLvuZKD4CEkehp3RSVa42rqmKxN6X7wl%2FdPLfGDGBINLYJ3Xd32W87LKixhSutY3g%2BJSK3Ur6cNFPbo3NdGtKS9eOzCLJY8o4gjDaMwHycObtr3RwNFzuMq8jdlgHk0CrGf3DI2VwF2EscMb3DL9lnTAHis779jl75xNIxZGneL%2FZBSjxWdZaZva6pn5Hdt%2FGQM19AlsG5Uf8Sh22q9%2BGSV25KQw0k9fvWUjJrOEhBCi4HnVji1J5zQlbnlYMdMIPettYGOqUBz83Wf85e3or1o68zogprLfwFfeK6DJeGI0mD6m5fFBubC76Rgq9ZHNyLfoAzW7CGz1VU4HHfZ%2BV%2BuDkHWdTypzdSpzzvPDTY3sBnhezDcQniJBwsR61Gpptw9xnODlmdR3loFqjQz06XH7a4uc1lyLgImYRN2bVx7%2FuVg5av5MOlJ9utNEzA5sB3gDDXQxjdfwXUF2Ydm4jEwNKBw3wnwRBSK37U&X-Amz-Signature=cbceec1377b369145e7ce08da2d7797873c18da9a67d6096107201c55f8774ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TUPXSUO6%2F20261013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261013T043007Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCWcXRwuivt4IPgn5%2BbrMGpMaH1hwCckWq2Gwqr1n8ExgIgDUQAYqM55Vj2ept93nKiQFOmhOTckU1wOOJvLm0033cqiAQIk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPAUvugqjxdlbawJFCrcA8K0UavwTY1d5wVnQboK4DX42dEieFg2%2BFn7Y0oZJNr7OIQksUQ%2Fdbu9ad%2F5K8Mgt%2BV9jP%2BZpbS7MhG02c6rRqoOtSMmYgp1vEOPuvmESVFR%2BMGs%2B6A1CmWDTs5%2FPEJ5ArtLEkz8WmDGXZw0Yd0kxghK8jJAhBh2YaF6ANLAmQ877B2wSzVT36FICbONuohzzDERcN6QQ%2FPE3iAe1gDngMzInSAOaBO10n4Tm%2BYq5MEIgKg%2BfcFM6Jyp5j39OLf4oAhQ7VBHaKOfoKZ1tvlHkrMnElqCMKsTLd6ZEIjERh61p3gmwF%2FxmSqEv5nAwj3yFEpTPaVx1E7iUIpPqY6B5CNs95I82UYxyVPknKvmTvjPn%2BMeRuQW1Qv%2FBVJqOqO0adVStZ4jNy87cnpTUtqFORn6R5hYHZek7zaZarFSHU%2B1sUZflH9Hzz4OQQ%2BDOYA7UThZPLRgTblsi1jW4XpOfFpmoBY5yfndDn3x46oBc8jVDL1ZfIIyWMBXPUOYfFE9Q90jayS%2FPsXCMXbsR1EBglarSJbCzB8upsItBkYnXdJhIYIGu56Xlp4R741oMCXELKVGMzBbDdGVZLUDaByiv6avwRYEPAFTEkbEu3tGsncRNiJdF1iMoyXcG3XCMKHDttYGOqUBvLv2Z5dPuDozpOnJ3ekE33OFGMHbwkQlfwodiB1N4I4kW5sErECAHTmsXGVOzCh9gBVPJbs8RFDbcAwgvJsI6foVWiTjMqvBlnlZafELK%2BVbrwdlomc4LSXTfUU9nMiiGwW%2FekmD3cMXZSvbIbS%2FfomeOB6aOsGflTNO3IanNWW55yf%2FXhkQ0HU%2FtbdaeIdLwYEUyi0mYAKRn8gG48hRC4VJmtqw&X-Amz-Signature=d5b14611eacfbbbcabf8df907a1ae1fbee0b827ccaff5ae6a4a97cdbaaa31ce7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
