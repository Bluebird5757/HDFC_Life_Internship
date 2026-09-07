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
fetched_at: '2026-09-07T01:50:19.945Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTO4M7AZ%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015015Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQCPjf%2BknbW3XmJsRYbjhyKdeH%2BII3adGQ1VKzmPW1i%2FmwIgWPHHvslYz3yUqRVSqBWb0Q0u1K2ELg%2FSANjRT%2BrfpGYq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDNgOXNWT1FzidBuYACrcA%2BAnGlPH5qwPCFlh%2FQ1eGC9FDxM2JoI7bHgIwjT82RrOIwXinjAlCJ0RwNeOtYpr%2FQYnFX6TKneTsZv%2FJBUSXGJhAZxbZf%2FDuGfHbvvFxWhQtad6xO37w8DoHwZoYBejbaFqRbLyN98zSoxlQ7ZP4fvJCzSk%2BLUx96es2cSNM0qbSMrQjS5v1weEo4i6J4UCU6ZdnjgBFEs59vUJT53sI%2FPI0sbOqSGJ34qDvBjeCAEKlWibSSQbDnylhP6CoHu%2FIMbnQvh3wsA7G5K8Z6Fm9XN%2FciHSYPbST3vl7feJnBCYATqg4azsNWFklt3cAhaOI4xHjtle3uYPDd9HmT5cEq%2Bv2twgS2ilUyAKPpS9BywdtdNYaSUHLnOsy8137KOPl8emsuGmUpofvYu6r%2B98Y%2FMHbaPNtxLwfdzqcMa%2B5A7TSJGIlWYx5p0x0BHzM27YJbD96H50MMpoi74a99sDneis5jecuK3B69CZYSVRco1lHe27HlDq50Ah9umSohxif4Ei65u3WF%2Bg9HPqWA1dCt0bbdCPXWj1uanaYL4AeZ%2FpCp04dI%2Fe7GnT07sxaU06Gvl%2BGpv93oN1eDhEfmjkHjeejwWcI8vFMnIuZSMzbap2SPgoUpy9BlZz5eWnMLKn%2BNQGOqUBucLPRuJLqs1L2YYMubM565ooucOnR7mKohtf3qZ3qUWu2AidDwwcwkQl03y8wsrD72f9SlDkj7z033KESNRLtdnSaL%2BpQ2z1P7FLPFlswYPjXstvdgWDFADshD1yJpjScTtRhsZV4ki6tzVMI4Mn5MHUBhkhLyoufrQXyLYtLJ7Yv7AcrvlP%2FduYOi4fc%2BykJIUUeIqXJxKJnVhcjdaPIRj1tMrg&X-Amz-Signature=fd8eaa61cd957d8d7797de6a9133771604d852c9335c2314e7cd8c5e2b1ff76e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666EG5MYOH%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015014Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIFLKm5qp2PuEUfvGH5SNLyEnZzBwVS4JfW1sr8Qpi7UlAiEAvU3CeHcxUkXDRYfHk96LvVoA5uq8HIsFnr8J1aw2U6wq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDPptP2qnM017FvssqSrcAweHZwt6Oxv5uA1SocBg1TsfXHA0qvs5Rpeq91xXyySJNRlDrj1B1sf3qGyLPS9YNuKxjIsfSf7YzELCoEEDO0Lhx%2B4As84fChSTocJBWjTRBx3UB1IXLha%2Fhemj7O6LIqLCZ1MWv2bELxtVpI9BZYXU4oxed5RRGcu6lGCexmHaW5uXt9dfES9Hjf0evEPDYrGmWsPkKXZwOQyKxWtadanZg8Kif%2BknW%2B33jglwNjMTOTez2G%2FoDBMwL7fLgW2lo3hsTAOAVQuxdIm1Kq68F7hyQB24LDBsxJr%2BlsKfy9xf2Qo%2BDO%2Fv%2FDKU76X6kxz9Y%2BeLOZ7GCvwLZlGpNjl8r7P8tSQ1my8a6GFx%2BE5qoY0qrACOJLexIdSyASQkjS4X0okaL2WfkJeLZwm6GMrk11S1TYsiG7p6TPGyq47%2BijeVq9iiOXPL%2FWNGx3PBrtf%2BemyFXEJXaAz%2BZRfN%2FZsFfeWOU4%2FXVfMF23l%2FA8yN%2FeDMKTEPjcPoIUHo9xlMm36cyseFsCqKeG60GOxVjzWEmWKfz4u%2FftIHinICfiY1baSkVAXtPOEZXhl4VNqc3aQrJKvkMylwdRFz8u6XRmaFk4Qo2PADe4NZ63JOmbHB%2FjcHSxmLYvl6IP4twunTMLmn%2BNQGOqUBc2vyt6dvZnGyaiommSpgQ0zCBvDx24nKB9W%2BAFIzHNQzZxTNBi%2BHVkpWYJ%2BwTdwFrPLVM2BYIGtcoYUIHE6K%2Bzqg8QniivlwsQzaGGjXRQKEyU4BU%2BwKTSgPQonbTfht3iLCZO3MBGnsEjbHJhSvGpeLfbq6Vf68Ltj45u1tx8JIxxkIbHP2aC8DbLWg%2FOIIZjQZ1rqV3ZzqteNqx8lN4aNYUAq0&X-Amz-Signature=013c6885f4c0a046af6f74d7940ef4c243f343126636a4076a86868d060e7f3c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UT6IKB6R%2F20260907%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260907T015016Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJGMEQCIEWwT9QxpTeOkQHJWBbBmvtSoxHJ5RkHNqon9qnD7BGsAiBuRugTYGMhhl%2FH9JQVx%2BYUseaOCXMbQEGnTWPHGIpEpCr%2FAwgzEAAaDDYzNzQyMzE4MzgwNSIM3nfQa77VLujwx01mKtwDyjuy%2B4Yr5mAXazX8xPplIGmnliQPMP8mjmLH0GznjV%2BNoduKvlMYfGsSTgOcb3ZHizZ2GCLr1H9mHTyMjgyG0DKNJRwVbCzNh%2FIzc8xXHU7ppw6x1Vn2uPtqtszmebd%2Bg0jqKrBiKjGwzLu%2B73CRu4o8PNxcfp9F0N%2F9ASYFxByya%2Fy%2BoioHgdDjgBWqZrKD%2FxCT7nNZxlHpVEgZIH1aj1SY%2F1RvoxczLovvUSOO0vqJOn9cpx6dDVY3F46WQDujElQ6z4hb4RuxyO4xYBD4dnT4dAGaLz1Oqgv%2F5HFvu2TAoh2wAJVwTiDh7HW3VUZgj5LhL96QNepcjUPlffoaYTECgLaWv1%2FcRlcYfI4trnyGoHOqG7q%2BgRS6IO2qjTdTiuzoqfXaLGgTOJBhTpCgYDOK%2BaaHejM7c2covRzXMnTH%2FQ7Cb8SP6VRBrt%2FGTM%2B39UcGHslpVouQW2wv7FQQtw7%2B%2BX7mLHLqbj3bZAm84gWBLH1t6GO1Tm4uHGJFrPWdTw3%2BFx8mgMA6yQ%2BeA%2FusW75FFG6doj733nHyxAeH5vRNNAGnMmwdISOManclYKkNsh2%2BNhH7MIiYf5fn2hnChkrYMQTMBlDEBpSx6NiZhiOQ9zwFTjenhrlvRuAw8aX41AY6pgEZmQm2y5eKZJSz08z23EMfI%2BX8Blgiz%2BmhTzD2kPlPFSl9dFTf5XMW6HaxetPZdPuAcG4Br%2BD9Phx%2FfW7k5yAVqDs9ItAY%2BmLR2vUfedZ84HSa267Jqnl2pnvG1%2Byb4MZOQFiUB4SaIpzsFows3lWecGpDM1RJjB39n%2Fbbt6u0MyzlLgRFjXRSe13ffVmTrS1IqtKj%2B8rrg8IM6d5f2rbg6wejTjTb&X-Amz-Signature=1d297d96a007c84534c817cdf5d49af3a39c21a143aff38a3189131acd081b6b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
