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
fetched_at: '2026-09-27T02:28:52.949Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665GB6MLYC%2F20260927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260927T022848Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIQDOqzmH1YEPNmYabc5icJUg83aUS4YPGwBQ7enik%2F7OrgIgeBmuy%2ByeK%2Bdb0lE%2F6xXOv5gKb6BrCoojOyWIZYzzelQq%2FwMIEhAAGgw2Mzc0MjMxODM4MDUiDCwcbKdAlDc3hgrncyrcAw7vH%2F%2Frj4UNG08m6VBiU3a0CbFbYlDbwO5iGtk0wRmsoplfLLjkCJZ0uOe%2BBjlv%2BOQ0u12fiTWJ%2FPHYr%2Bz1ZxvOr8I7WMb2Y4Dt%2BcCO2yKyd2LzK03qB%2FYyZGqrj2pS26EjcnL8wkfx2f96fo3K6t8jbxrGbMGkihi0AdVALCeChzs0I3Ni6%2Be%2FmwkqsAwYNk3Amb2T%2FKj%2B8IUctV8EfCRnCCdpQDw3beB9nGXTLp3ZZGtNk5KOXiGWIHUt8gRtFkyLi3%2BntDm7wdsieYzE9PJ7q3cBRv6jIrKIgC3Q7Wa393htNtaHNEGpEnSnZP1T290NIh%2FEXqANncFevGuMs2%2FjinAECySYBxAA4HCAW4yuH7eZOe7v2yEh8ZIZ5wWD09viifd%2FN3ZfifXZu8LdhpbL9BGG0aVa1av0rcMy91RxTXg1%2F0zur1JCVapicQL%2FX0hX4OSIOsKh0pIVIdf2le7eBBzKElQW%2BbI%2ButjZunTJ%2B3Yy1YFljj1YMwzv3OjPNekIvjneBhybrnNg1ehMvTqfZMQx3SjfA7sCJI10jGLq0bh%2B%2BZWZkdpVI7ZRk3QTcraOK8S2vnO%2FzVmKxweb9TPLNsjNHk3bNlrBDzElysZFTCNkk98ZbwOIxCH6MNLe4dUGOqUBmVgS1xTCODlWCdGv8VrkbzDhByHIYCrwUVeWN7luMU0fRu%2Fah3H%2FC9CT2VgaufzpxATd8x6d6RgbTYW3cjUT7NQd6Y0tvrvL84ak0cvtegFEKevg8zE%2BJo1BcsmtTptwX79BWWLZBWpOZilSr4fxGWd4ILq974pSiaY9IFj8Fatx%2B%2FBedMK6fhHVWSNBU1XdJn1EKVrSgkoB6f6PPM9YWQFMjxCX&X-Amz-Signature=e03264db8ea277648b2b355a542942101a505a6fdcfd835dc15e32478b0d300a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDPGWTWZ%2F20260927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260927T022847Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIQDp30Ds4h4APLI9v32p83v9SRr%2BgXnVPar47gSB2tFp4wIgP8CIm9YQs%2FJ25UUXOBEXuPR%2BQ%2Fn0b2PoH%2Ba0CDkNQIgq%2FwMIEhAAGgw2Mzc0MjMxODM4MDUiDIyUqs6Me6FywW%2F3uSrcA47gqWbJxu0RQnl7TQS8seSBWEY1b5o1Rmme4Umws3yWoZ1YScY9N58dFdNZD9HviRR8QSpF3oJGINy0FRZKKarJu1H43lQSbbXsKiKevUvglHnvI5it7Ma1utNYI9aLyBi%2B0ZbrStah4uTHWjM7%2BiRUr6oNjokitQJV8tAAeZAN0CpZHdAOteRMEDX5gXEHkciz18znO7oAH6tTkFcbE8T5tXvqPx3MxEOyTdUuJPWMbEBXbnctv61P6yef08ZaKqshSvYvhhBlM4CKcMRszwsIZZebtWMLF3r6Tw8Zc1GEkB3bYw15c8I8aJ21GNyrY2BR0nGGjProwGpjSdj%2Bd417ygjpDv4PHj757t89BHqbBHnoYqBGEPc0%2FWUgClIY8OJwMSzlQ0zPEMwlKD%2Bz%2FET2%2FmnFGtOEBg7pqfZ1Fq%2BTpOMrlk36JZuUMz%2FZGzIh7Y6D5Y7fHaIIGB2sLC9NBUyTAwIVgDAWC7ByJeqHeXXyb5pW%2B4yhy44SZ6kTqSdX71h8%2BQ2ZxA4wn9axBxd1wklYua9l%2BUFl85akKdPwUtnnLCGKDB5a0JBJxHNPeD%2Fh6jpNR5Nm%2Fp%2FbFpV8bujv2cjcyOfMdGcZDFCV0PHuGIXw48U1apNl3%2FPRSKqAMMPf4dUGOqUBhiAlaLWq24Lnny4SSdjQ3fP%2ByuenUAMgVtllGV1g6TwPtP9bUQn4HKOX9OnRZ3FUHN9sOKOZ1ZkbijwPSPbgO%2B8nouiuAO31p0RRgBTRKieCN7qdXzTbo%2Bm8LtE5wfbwOtE%2B1J0T1%2BuCBZVirq97L%2F%2FnlE16%2BTYoNFki3iKuds0xQFg0%2B8COve2eynpPqRk%2FMatLItaO5cZDL2m2fO%2FY9yJLW5ud&X-Amz-Signature=35c60457278575e96a7df89f04801de804460d57a1e432ea27c29a82d1e84b7d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667TTKXWVO%2F20260927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260927T022848Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJIMEYCIQCh8hyxlwxdnxX6AZ4IQ8PWWYo936HmyLO6Zc81daId2gIhALxyHSoh0dNO0Xudz2VPaX4sGIG8Thgfyxe0DrYF%2BGhzKv8DCBIQABoMNjM3NDIzMTgzODA1Igwkiq0EkyfUNP5P448q3AOo%2BLwWj7p27%2BgnNx2cR%2BwBx4V4u5AAiwV6pwsZ9u71PoBlLSh15BYnqACJdxLqPz4J66xKHrhXjR9Iw4YlclYnZAVi9Dyzw89FdN1NwDX7oZWA4oUNxnzUeeJQlowJmjFDT%2BtmczDT5f8c4VfRwu2A39K654kDULtrC3icsjMkIYwZF0xEhSFBXtYCcBXs9erWbvxKyM7WS89Yld26sHSuPny6v9gfLiNSCu1ugTYDp6H6U5vqRNLLCmNCLa9ApOHj7b66nWStM9fjU2QyeBvlksXEpwytJjBiDFNKaBGTnqZVunZd5nHTcEwFuh%2F8T4ThApuZ8uu0z0zMDnAqSqE%2BNw5nDQWoTV7YZhfo6wiBGT00N0Kb3AeYZbYOGhP7AjiaYYQdkFwnytJms4wCxHS6uij7OB86FKjrMeLFEEAYA3vDep%2BIdbXefUs%2FADJQVaWPO7VSN%2ByICp0MHVh0hw7%2BXYmu%2FA%2FEbOBzfdN4ucLYM4yObcto0FiCaeqQfoMC6tLISCisrI94Ku%2Bd%2FdusHihbp5JcPIHhtmDPeHdCqlb78ztDae7dJkwXTKviRUSoqePz1PQ08CIEx9D0%2BGhXBrFa%2FutquXYJiXVWLuP4JBbBhfIyME6FCPkA9NyLQDDL3uHVBjqkAYPHZI9JzlSEFgd8fvvOIx9rCpF%2FsYGcIV29ufhhtTv3vAMBeHXpKfoYCFgR5b%2FGK%2FYV16tVWhzULzapBLc%2F1yi0G2LLWF1PZ0dAXdgLJOcAlIb%2FNmgtDzWY%2BcmEFKGEJgMlsEZR%2FN4FIkgxZQ8RpXp09JBqsI1%2F2R13J%2B3u4hk5UCnrHKg9HAObZJm%2BecIW8EVisUt5PM7gtJy9m%2BUPBxwuntNN&X-Amz-Signature=7920bb3df8ef089462e134045764fff53691f0ad925ec4b6ac227f3fa5379392&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
