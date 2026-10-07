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
fetched_at: '2026-10-07T03:16:42.995Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WU3HHRN%2F20261007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261007T031637Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJIMEYCIQCgZBukUFrYfUWgPlVWT5cjZbeKqbEQnFXGEsmoxO6zaAIhANl8Fd7ErSUgiTfncwmzAsTTou1ETorQ2v9hJoxKNy%2BZKv8DCAMQABoMNjM3NDIzMTgzODA1Igz8ljUQ9mGhIvGH6woq3AMjkOlBzbGf9IupDn7l6babNFjpeMfjX848dP1zcQWwk8lHsITHsaFVRtja1jGTU48JANFnanwtavBG7jlbJ57gwXrH%2FN0u3jZV%2FJOCz0AfJ29XkaDwujh4jBuwQ9ByIwxRsrPmqSbcr3e9B%2FkyTNFqvlmao9RYdCjDuXlC0%2BGp4dRkEF%2FlI3I9TOpP%2BpXKqdHqXOzRgVOSNVWPTNt39L2Pw4kqhoywceQp0UoO%2Fs2Ut2VmHQzDArIs3ysjFvbMiOGEwPqKHqG%2BdM%2FcoSTyT5TfGXdsZROs2bld3sFcAVnHYDgsmt6SlatOnrTJvV7Zv0aDk4s3HXO8Eu8vmvNLRlVehLuRF72WrRkJczkm%2Fs4XI8Tw6lWKnO%2F3JrbCpKpACWUVTItUDmoEcSbKRTGZslNnMBSYymoV53GARogrDLhvEljHYUzUcw6IE6AA9n2arwsuyIJfrjdinvipsEYl%2FmBEnbmUHWn4t1Lo64nbZ9vhsQciat%2FKZ2Se26xSW%2FSQc1Adk5iU6ozQ2XpoER1%2B4V2a%2FjyN%2F3VAvpzXtvtnhk2lR6ZsULgTBc0h1%2B7v9W8EjchHFbOvACgoG%2FYm%2Bt%2BQNjHIPeF%2FBq5w2mU2c0LRvEs8qDgrIvyBmS%2Bj0Yxo7DDl2pbWBjqkAbNiI6VlKQxD0lTztSRNAUa7qynFdFs98o6Vy88ywA2FKm0gSSK3%2FNcF9EQq7GvvM%2FPKlc6irwQDpnlTT8HqbDsDaClyVXuzpeSsWdUniMHsQ%2BP3ziy30at258vBchiM%2FoKs0p%2BMFBA9unEYDdOjma2ECJzo0%2BZOfgPoUl0EjBhfqnLhwg1wuytaLomZmP3W2DEQnqWkCl7dKdMuRQ%2FIFv2GGEZK&X-Amz-Signature=3c9e7b3f0e8804d012df698de9f7b0e3f98b0352d76ba418064c2252b0e2787e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TG5DR5VE%2F20261007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261007T031636Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJIMEYCIQDA7JzUv9dpjRO3WllRu5NVa46gxvPky86ArcIhcZe4QgIhAOBcWNQnI2fss1B%2FKWcJ4ccfZKo0Ku3rRCTIwZI8KKS2Kv8DCAIQABoMNjM3NDIzMTgzODA1IgxXJTphgEZ%2Ftd7Yt9Mq3APpJYSaDc%2Bs%2F1VaeLs%2Fain3sM8DWqkUd1tb64cstR9Kn4RCsUy4%2F9b%2Fp%2BvKqFZyM0Cg3yG9I0YMv31ga%2Bt81bw8j1cn%2FN0TiiLcJAt5nXtHktsKERspidtQ3Yq9mG9ZPKlPIMBr0A1W4upGpdb3dzaxw16qoqUuV6res0MJgYXYZ%2BPfcK9uq%2B3seONh32E1gEZdFWnf9BCbnh6RZQJuwLA%2Bl0BV976fRtFlOEPcVyN%2F5sc%2Bg0ENVj7ZMbgNhKRSMxkmlR9TicSKX7TjURS7LHp1PghHLUuOQu8LwumOVRdcI3fJRADyBBCNvpBl4IWIyx6op2d1%2FllB5RlIePP69z7X4fn6%2FYyjrXww3ZuKRfkiJ7F8lb42r4mnAqnriGE31nudQ55VIXcpmkN5VpJ5t9DH2rcGBW2Q4faGMwXEImIOmfMp4%2FCofBGBO0IGS2e4B%2Bl4lwp1Lc9kuessS%2FbCr5vYN%2F9YfT7aCJPgJON6ehQR6AT2udGgYXBGP3j1j9YSslQbXyyak4%2Bp%2Be53eEs8jnqDRwz4XGQTKKnL6roN8uaMuQZkxthxBJCR4M%2FMiklaQyszm5tshPDkedDSmmMaEHlcx%2Fu1p20xn0UO3wKAteZfk834Sakg349s136KCDCu2JbWBjqkATc%2FglmCpS7pIWLGKWilw%2F%2BCmhfezmTgjz1KFd%2BAfnIaC%2BjKunYHsp6toKHk4pTQotWIILl7G7XUBOPAshcaFn7jtn%2FKUUVyTMLFZ2ftfvaqxbjd4XzYAKnouGte1lDu6oiNDYN4hhXUBMrrgO9%2BRyqbTEdfvf0ocU4KGRapF2jO%2FabOfdWgOTxuboqesJUxRL%2FuR9Ed6to55GSBifUWQ1Tnc3Ml&X-Amz-Signature=b89db540510095b38bfa666886f99ccf763cc3513f7d17d19f651aa650a00e88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TITSQBRP%2F20261007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261007T031638Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJIMEYCIQCFtYiXsgWNfRx6BC3r6p6q1jWXER9HYe9izD5ggp7JxgIhALAXwjNy4ONB2Be2vFchH2fu9suPJCTXgTSSfq4Sx4y8Kv8DCAMQABoMNjM3NDIzMTgzODA1IgyPubsE0eUSZuX0Cfsq3AMhvC0TReGA8zNBbmZdKKiZ3HPwGDLOwsk6eUN5MjepvDAmo6n5fOuspCKd2qxPLdECWzFwQyafZ9TT3ppUtzhHMtAPpJLuA%2FO%2FCR%2BYjwzqpCw6aMIzD%2BGRk0d3V%2B43zA4VdQedJmJp2paRH8KuAYD3jxMhH47FUrpkDIkOe26ORXVQeJH0GErwuLFje%2BhS3P4BI2gpC0CVL5bsGFNwTUv7bDBP%2BqKZAZet9fQTMRrZnDwrz%2BBoTeu3b8JKmaL8B7O%2BPwzliT2NxLP2L8xYb8sjVdEcOA7qgL95qldNri5zSRwn8h0H6Pud9WdvglyOrHoprdr37zcNNMToWEz1FkGTfMflRkZcqMih%2BOlrlZCP0%2BEwYMqvYW193v7C4k54wObYnT%2FQuNwgYZklz8VfA6%2Ba0UsXD9wcLADAsy2%2BHJp0v8uXxrT0%2BZZFR9TR5RoILe68NbaU%2Fyqb71NWU%2B85hJ9rdFCPc2C8cet9hpk0OHADvPrbz3abRzfEpohc3PlFxHWUlIy8qOkF96BvETZQgODDl380vwFzvNgTUlgGSphP8CwZBLu63xZckga3bEvjEc8dj1g0f298lWf6BM%2BwX6OpanHpkjTytmohEzbVdGDuB18cIfy81dJSUQI4EjDs2pbWBjqkAeSAy%2FbFsuElfUX%2Bh%2FHyK98Q6AQnnd9GIVGEibUyEY82aZTf%2FWl8ETl7Vc4upv%2F6buBeJpKWdVwB51wm0fKowfatEBW5PpOFBACleI3nEvNLWm2w02BPnjBrKUBqwY45qyBr0Pmh6hzCFzXGxSm8rXs%2FOwzChaaO%2F4FUQcx8ZWO7ZDZmi6p0jqyOl90TUM9Yx7N4UqSidA%2Fgp4yV6DkaN8ufZ512&X-Amz-Signature=66c5769ce16e94233ecad28b5b038e35647556cd635629d0e88a90d7aeb975b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
