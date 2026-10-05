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
fetched_at: '2026-10-05T02:59:35.121Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YZEGE3LW%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025932Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJGMEQCIH2kWHpmAYIkOEufULvWkfDUG4qYiJT405hPcwZjCJSdAiAbtd4ykc72bunOGkAJQRAJ3u%2Blf0QU91%2BQF7ZjWHaNRiqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMN2SjT4ocIN518huoKtwDdzo65VNwqbz0SDc1bB0H9L43yK1iXWOGCgbH9uWepYqdc0%2Bo7QS8cLVAAIPNRn772h1cS7MkPW3Ey9zj9ME15xqm%2B59ORiElwZMkWXhxwJ9yax3dD6J%2F44hZ%2BeE72pzhXg%2Blu3vESks8cS2jC%2BsUVrzekz2aGZheJCF%2BSac2YHVjQzKuaL1L9saoBgj%2FsunPWPWHF2nJ1lvMZFX6W3NFWzbzfFpTaPZ2B%2FRcG7g2ztEcmFBSc4jmcAyBxo5VRHQgyGz7m6WzbGvrKZbUOrXaJVVYdqOCYswlGCS21fuJfuzXiHXlwEZ760JEF64VrsZ%2Fa00YJvKC%2FBmGyFNfE1MHXwRv%2BfEt72FH7lQYry%2FfTnf6yzNrcYWr%2B6yFsnCynCvozlYtV2vrnuJPJxM6rsiZ3jw74RXRcyz6zMTRqlDjZIZa74%2FkB1mwlW%2BX%2BYDhfmDlvr2EtEHiwuazNxuVjYFXh4OE0vNjjmPBg1qcZwuK08xtFAy2DDldMK0NxUT5sJ4Q%2FCHXy5zjpcG1z%2ByAfeo9mPke1yTE7pNGjnG2rfIPs%2BDT3ghDuxYCug1ixgp9Bh%2FQndFKNEXtEZKtUueygKvfcSzWSmsnO%2Ba3tJSMGDjz%2FCR%2FiPD%2FRdd6XRkhT04w6f6L1gY6pgEM8JXezVce8xd0tOSLQT2v%2FdwKO8OhZEj0QssLnQ3NLdfLbQXRtiH9RloPtOcriAFRxBORwlMQTkj4EnVfbkRghU4aZJvshw9zeOV8CRPaItZ5JVFZCxII1NZLiYAUMdvJlg%2Fnxt7M0PaHJ3%2FXoCEEayFsGE0dGL50TtCpZQyyxh7b%2BtSVryywY9921Mj%2B6W0NEH9O5CJa5Lzszw6GNkVAxfq4x%2FDR&X-Amz-Signature=62a3967fa80c690484573cb10c47b2dd78184c7356a4ddbf1a17f7eb1101b230&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665NZZXLAD%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025931Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIAD4cdU08JSWs1tPjc08hqWGF8RzrWRQS36aB%2BPnge8TAiEAobTnuxKMJqNyVXfwZjnEhrtwDtfkiOtU9agHQwPZyL8qiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN6LGZrBW16BMC9M7SrcA7nH%2BeSWxB2iZAJ3QUuXnS3K0pNRrPow1%2F2h13GdTQHhZgnp1qHJ%2FCEFdN%2BabkeCK%2FfBBtL7pbCAdJRTd%2FSAr%2B%2F2pfrbobZTpRv4kwEZLnS4mHJxEgF%2F%2Fw6IiqGsiUJDt9PoBWfTXxKhXMvf9AGjBuItYsSQvu1vKkrD%2FE3G5WCbbKRRUt0INKk2LeD%2FEufU8qxJDn4m%2F2ajiJmv7a3%2Fm214SusRZc7LC10LmUMlRUU4cLF%2BV0qBYAtKNGx36Y%2FRXSu3wETNCaz%2BBCAeCn%2FOfQbGTaAj%2BX7H7JLhALdd2P9wkheNELcwIyvmiw6uAtx2FYzv5pmAqaDoJ4teRnNp07Ns1kBcirP5TBerIRoUJpSAR38ovtxOrm44O91rwkV%2FTbpm3B963GQGMbjviB4IA4tJJIIuQFeU2oCCeoVFApQDj3dFNeSubuWWRlTudB0ZrZ7Q1F207iAPhqS4jq6PmBVhWpkmAlh8E%2FWIDWRJJRIOlOW9JYk3G77yWApASeXnKNV0tXh5pEI6huNS4B2fzH2GQvsWtUJdY1O8gS286kjf7sNu%2FfbWuW0fjZyRpK1hbb5LrpZuQHqgVzXDdLRn1lrVUa4OpXBq0tlWzNTOsiW5qSxv0kf9cfGAC2mCMPyAjNYGOqUBpxmEIYciGzVB8iH%2BZjLRu3G9rw%2BIXvyhxi2cOWzjeOCpNbG4VUZ6S9SRQ080wkuHw9dxKXdi1Py5dluQL6NRZo0CEBlsnBl8QOHPvdBC7pJ2bK8B47%2B%2F%2FO%2FFVgXdgysINLcaAfBMWmmbwOvRle4p6qdvCBpKetGE7cugU%2Bu17MhQIletxs6DdBAyt38vjAJfI2BOQuOXNCdYsAkHBvzp1xA6wOCB&X-Amz-Signature=d72f9ddb37e6cfba380a8920cd58279a5f77d71237727e0700d226bb83dd24d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JF5352B%2F20261005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261005T025932Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIGdtmOkrFUkIDR491IwECajCxBtv%2FEQzJXXJq3RDXPlCAiEAtjmVfbVUZACZtQG5Ct9jQf7MPD7br3LGQHBg82VcBRIqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM4gTH81M3P8YTAPryrcA8ELI812ezQHkup4NHx7nOFf3JSE9lgCQEPuhDCPBVmhLsI%2BxOnAamCrWEBtxH3JerDplJCvRS8%2ForemGubBTAnKG%2F9L%2BMMwBYG4fKKbU3tbPq9fQUKJfNJIiPdUZr24xvdb9l2YfgiJEiiqbIkfFExjbz7dAx03Mou1rhl97IJsvebFQKoqQYL9S5E36ojWI8x4O4LK7KNaWWYhV%2FVXofymHpb6KcETU31fM6Ne6XNXATlebbFsHpDvO9JRV%2BTHP5Kq01pVEF1%2FFnV2Q5J6gCoQk09PJS%2FmkoJJwaF7i%2Fmh8l9VxM1KB3%2Fvycr7Q3GFeXx4Cb7EWIcMgxG9vU956ZWc7JA85Csn%2BGJ6S%2FE5%2Fdfqd%2BY%2BP2TdvMppFMRat%2FcapRu%2FJbe4uCfwI4BJ31ZUES%2FHxMNU3XIHNCGuX%2B4x7Z5%2BL4y98PnJmO0Km4G%2FbMn6fm0F%2BWOSL52YKEJRPgTBZbl5bPCTGk%2FM%2FzrvO9fRfFBOJ8R5JObiXn2vXUFub%2FkPOFYFYQWGSgF1qHuesPPbkgvUYXAONq8S3n3ytF5mT59iWbEbe8FTuZ9C3EiWPjuVSUsESx8ND9SQ4VKtiVmbWgZdmmZGe3Paewv9QchncM1MD7WVfpT48KXKlEj4MO7%2Bi9YGOqUBA6NbgqaxYA1BNrh%2FTmES1AG2eWfTc5iavMCsNodZmMoMCFGwAJx8Az%2BJsLsFX96TEKNQOb96m2fVGkMqWFcP2tyOwHlDScU8i9gtdW06ji1Hkeyd9FX7mdXT9a50cC7kBXrTmKVQnsHrnwC5jda0zqnIc0oYeZRa3IM6dz%2FxNuUGqcNb3Kfihg2hGb%2BV6hbLD4LAZR8NvDPzwfA6DeOmhu0w0Nln&X-Amz-Signature=9d73fcabb400738c567667b9cd03ed8f0684ea6ac25bea2909ec598f618b8960&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
