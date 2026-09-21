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
fetched_at: '2026-09-21T02:18:57.878Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YTSPWKEW%2F20260921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260921T021853Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCf5u9wU8ejTJd4NHQVJCgmy0v4ttj1hftlwytBOrFO%2BAIhAMH6HWd%2FiK%2FIoOffB78RCXg3s5XoYmxARhIy1czC44qFKogECID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxWQyjKDHCZ9koEkXIq3AMVUy5mvGe0QQtZsTcRGCWX%2BZc9Gi6BJXk2S1TkMMdQZR06N92vqMWePdbSuO9WsJ%2BwQAciCpzN3s4%2FEno2yXXH%2B4LrPZuWf%2FLvzX6fulnvW2kmgSBZl7wPqvQltoFD8%2F3KXdQvceexxc%2BT9FaRSK6F%2B42nzz1%2BHNUdcvNjT0xCcP%2BfsXv0qyHBznyo1rGugcuinZzs2dU2a%2Fd9Cwvkk2GCmVPC6nvYXowh8xPuswQ5gkS32d3v%2FqvVEl%2BOG7Nn%2FiKrXpl7j77q907SJjCmeu9LKNl6Bk9lMsO86MQ0KXkdLte5HeQki%2FE7aDXpCi6L4ZiQMvsk9l9opyRv1Pz%2FAyGmv0u1CTIdvYfi84jYr4s7625GewZnokYM%2FJ6tm1T7d2KkRgLLFY8t8%2FE%2FKjVltM7mp3mEu0TMDTV38nuN%2Bh4a9tVftfhojbpPIJ7vUS5LfLfmY6HER2rzHU6ZLzPYYHkzvqUCxKYqRTRgTBME6CcM34V7pByLfHE%2Fuh%2FbcK5C0MpqPPtEmHxgHTScqF%2BiEXEKr4a5SQ7621QjsE5uUth1HfAUwimpFvDLpwt8J%2FCZxSZ6BZ9xu4CzwyBwTp%2F0eo47Dw3AZENYSYosNum26S%2BGo0zfaZ1eLwJJsreIEzCeycHVBjqkAb1rDyjO98QXJtntEXmXSubCdkeCQY4z1Krty4X23k2fH5cnRPPag8N7BZpyDD%2By0cDs2UgcdfzthIQz0Xq4UnUUGuYb22f7e%2BaCLWqr0m7qLQyyXFbn9F28iRMrNX%2BhqTGE7n7YL%2BmbcHmJtkD1DQkmoHAQmHqE8HhO36gKCqI58Xq5%2BkqkQAQ99ZOpzHPk8p9v%2BOVCQNi3%2BsDHTpG%2FUnsT4CV6&X-Amz-Signature=b3d57ee1bdb4d0b4e27640cb63acd81446e575148ff02bab533b3f4f4ef67e5f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OGSMGMG%2F20260921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260921T021852Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB9WwPw05NpW3NdCDYdUkzHt3aA4Ua8Q9gsgxtYIFa3VAiASDxLXDAhxAtrwpoxPoWE%2BsyDNSiQCVD0Ri4DIxLAaQCr%2FAwh%2FEAAaDDYzNzQyMzE4MzgwNSIMpXpbIWDAMwcUDOeMKtwDq7He6ixceTa%2FKdrj07wl8RmS0d4NUBaiDHlI%2BPnfx5R0KgXOIaOYPunDRop7jXdczviNLVQd0zbhv3zRjyc95H5Qemcd4O18Oxqtp6fNn%2B%2FNfbHMDRC1SDb7siFaqwuI8HJO0jMczUNwlUb81SsT%2BxpbKdYiAL%2FjUQjPPtubIq4gER3BM8t6%2BgyJPo73lIDiD1CR0Kg%2B6ZIcCXpB5xN7jgKzOEPn0MOH1dUnBQ%2FjdN3mQEOPZzG9BzSuILM2yktjkiffSXBnMn2EVTDpz8ctwfldDfUXq%2F8akg5kuW%2FUh6nAQCTomLGs0qjH4uODOaOI84bFLUc2qBbitctZmVblx%2Fym40YdmqZ0jdEGmNAdRUfn7c5P2%2Bb6Men207Q9FXqtIeIp%2F3%2BMMCSkxUAmFnkfwkubCHW0eO2SV7uAb6qJTTkCLotC8j1WYpmT4Tswa6lh8IX0m8KHLATMI4njxdkup8ZIX0PaP3p0gRJRefG7aa2%2F%2BD2obABVBBTi2QsRw6YG5HWhk5IqprJRYECMMm5NFZG1XMEUK1oXo1r1jwPMM%2FiMAxBmStCxW0dWP0ZsZKDo%2FiOmyP7wtuHAkO3mKjOutbPdL3skoIWrPUBzplVzHymjLyo%2FhQO7IG8W2jowwarB1QY6pgHynXj9KqKVoXV%2B77oPOs6R1nPgtzPH06rOE%2Fh7LeNTuR5k%2F3aTbjhQwA2OzRpk12oR3eseVVS3yhb6svb6MTkwarrz4sW6aa9%2B0Lu2SzhFAyKTJlmCiUSEXbvZGULgRWqxGteKYCD8MOCqvx71Qg725zcxKO3%2FbsDHj4GOI7TASQnxQtgP5J%2FNsyqeU6zH3vLJyYHW6pd%2FLHSb8L3VZLN2nJHBVGWG&X-Amz-Signature=d426b255143e56a3556276f152aa44e8cf17917da6a19a05d8447ebde6811b55&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X5R6UDMJ%2F20260921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260921T021854Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICNX04hySoEWxQW2R%2Ffkg9EHAGivbeS8XPdkdNk2%2BiQ4AiA4tLnzsMffbi1TC2mx9FRINwOtlQuolKg%2Fnc72nUZ44yr%2FAwh%2FEAAaDDYzNzQyMzE4MzgwNSIMWlUid%2FQH4lrgYzP9KtwD0JET%2F8Skwat165JZhUlZ4POWw%2FfJXfMUoSaU%2FtwI0wB7RA17k7e18BonvNNDHbYLqMUNOPBHaL4sUt3cpaWWupvo7PEiDDnyXXVSWoVfwyYABd3quBm9hivQiUFFPPfBJIl3ByTpaW%2Bz%2FgFDK4NKJFm8ym0WWinqpUw80PfB2rPanWuf98sOrTYkl7nsUBCRXMEi03bNyLBSwHoef3nXui%2FMtiYqAA4laiJR71agz5JihNIgfaC5MMt7WvJv0pO%2BVYOF6csB%2B%2Fk3rVi45O%2FzY63g0SoB3f5illDayK04pbfkmr0oYtJhZS%2FmZlmwejuBIe%2Bsh8rP4zh%2FQCdl3SAWdUVrep0%2FjrwBzQ958wE2OkkQ6xl2AX2aFPH8i9DfH0phcZZqCWPWCoCAF1jAtlTreOiwnddFPDCczHCZgkl3g5p6jYSBCFsdBliWBWNxtxZJA2BDG4DqmFsgqXwT%2BRqkTFUCfRMYAm1YWfawExepbYHjmmTH%2B4rx2ujrpEn3eYx%2BO87a57ATMV6x9SaIJ6Ywxkm4pRUwvXsa%2BGXxDy02gPibEy3ABPDN7ic%2BnQK6p0IrGIu1SuTK6gmai7cAuKI6kD7J8K4BlUPxV3WllNrFdT4AZfyVp2LtK2xBVyAwtKnB1QY6pgFVkdQv6lJnECDpnDKUGK2IdcFfLX%2Fu3DAQ%2FDbvPNe0cTb%2BuiIjyNjlQsfoMfTCVYBIyAZiBI5hKp9chYSyT2zjXjgwGaOtJedren6BiFY3WFbnYA8glUes%2FBo2kY3GWJ%2Fj%2BPWSJEbZ%2FTd1erAhop2vhsjlZz%2BLhSSU6D87Ud%2Fea6JwcUYiT159j9L05Gu9qAFXWkMGGp7%2FYuXiCaL16rKMJZ164uI5&X-Amz-Signature=16605ee0fcba3cc7ebfd143df2d49192a2d392b7f40746e9992a1dd0ae2ca720&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
