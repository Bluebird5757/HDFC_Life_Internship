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
fetched_at: '2026-10-09T03:37:30.454Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662DFYI2UI%2F20261009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261009T033727Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJGMEQCIEYCIDga5d1zSbTPC0j4LMvOBlJiiRmS6qRw%2FUP%2BzXwKAiB0kdftOlMlJKNmHPL2Ua6AM%2Fv8Xax9Gci%2BznqX4SUBpSr%2FAwg1EAAaDDYzNzQyMzE4MzgwNSIMG4tfPRik4DXGTew0KtwDhVNinWJZkQSizNH0F2lU3EDCYD2kgdCQ3g7nkITpx3EN8y6sb2ZhztLUIBLpfYFAw3o4YWb5GQQk43rwqyGro5AioOPya4ezvHMJ3FJmb28iGN%2BL4wYWx6%2F3wsutba6ImVmxad8qZ9%2FMk0GvP5RH%2Bmp5mObA2CLga91%2Bv8xulRAB%2F6g3knukSk90HhfiJ%2Fz8ayIQYfLxFTr5NK9cEt2DsFmUHpPhgN20wU4vVQEGW83NhOcdgc9KgNq6MWRzsjQSJJJ5Pz9%2Fa%2FeyWZ2y3TMu7pg%2BkfIMbASPmbhDqsQtxuuAGSjwvcVPxjc6rvtWzYv8UjerpqxD8WSxxJABnJli40anrRDLV85VkjtFVfQuvlaWdBvQBx191IS4wb8eZK5%2FVfDO1VdPGu89CTDhyizMplJ3dlMn%2FJx23Qc1SkFvxKru29RCUh0MhG%2FKfd3%2BwmNG9DyDHgD7u%2FKAjv9wxmVjLQzEojQchlswXxDJzZutWXpxODlKbsrv4JPFJf%2FBxA6w7MzdgPoappz4ZsVG%2B2hK621vnKA3hjzJIUk%2FmB10mTRW%2BnpbQCSFNnm35oaV%2FuX0u4TgRoKlVZdft4ONfwY4dq7WmsiIqQgN0U2CZs41j8RC%2BtXHEeJyYGdYs1Iwh7%2Bh1gY6pgGp1v%2BYIkboQPXUndF39l%2B1VmoSmLyKXx92IrN7mhX2C6sbEC63WYGCCdktcx6RnGGWdzGgxCqlQdD3WSgnKklSFqlwdIOveIPnM7ad9Al02ehtclljrnVf8nAyPPyUc2mVUJJYE%2FSrWbIo021osn4920Siq%2FLenm2OPbLu7VI%2BOpaUfaPsY0YNSsMgzIp%2BFjYZlsBrx0nnYLA5qMZWcl4xld2Fi3ih&X-Amz-Signature=794f4046e7aeca3066bcad0b2262303bc2c21b5749f55793b9d2c80eb8fbf395&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHB3J6CL%2F20261009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261009T033726Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIGV%2BzExuPVUw1nc2DLR7UQafvALK1cLAn2ZL4X1tO9eqAiEApL9WAa19ms24%2B7WMDCmmv4NdAUAV4TXIjkqTpK7Im%2Fwq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDEPd09aaugcyo7WezSrcA0%2FDL5%2FRGJadkBZ%2Folocc87TS0d5AZyyfURvqWsSrnsAXrY3nhTZtIVvkKlM89%2BqNIjRJkbceeGH0FZk24u4hbEfXPRDZ7X0Qh8IiAueA7GJkkeWfq4FdJBaSx%2Fbc5RVOANzFvzoWmqYqTOWGh4MWMYdpedRAi3t1G1vKY5EAbrQe4vse1lj1vLux4hDc6jPNypA%2FTYyqTGWTv7zFoebJeJtB9%2BY3GcvkdR961TqJWbr%2BuMYHcN8cQbpW4ngVumBsfvTIq%2FsVKjGXmd2tHfucVre6SjogRkUuOtPeC%2F6udN6nr39ImBdOhTGbqPCM7cwgvNaNluGCcoSTo4zXUXk8smlnv4WOd0Bcr%2BgmYGqRIC4zLqy68EeRt2spEXwgvG%2FK36KqBBeJdPWP4OIK1wSluiFVOdq5y5GTpj40G3Ck9morRSjJ4dYiKtIt3KbjQ4%2Bh9eDuOhnrp%2Fh2MJZa8E5OKvP8Ied7X99mJX3IrQSfR93jvxitbLRwTtxwzEy61Hh9F%2F5Ty2I8Cdt2o6YF9oWk4IFmi1d%2F383WFddrenF4cAVp3q8gqnQXj37gXE3wtiO58xJTqZrER%2FU2VfFt1tdaC2yEWyyXfCtERHgr6%2BP6AmgxZYkWh19z5ZXRKsPMIC9odYGOqUBtNr3IX1PAp0cZqTxWgKQm7cZ4U%2FaIUacu5%2BH0UleUgeQ8dS4dvblUPpNNQqOV5xxobazjhEbdf9demmByFlgQdmSlh0wbPbRN8CvqGJMtGU8TWwUtNvw49xNLUbFyYLu8odAE8hFffg8%2B3tcWkhqjmz6FeOHokLgJJXqY6jcGp6NLkX22VkksVUFSQQ6HpNc09MpWwzYUGN6ppv9DgB%2FLhWLcIlc&X-Amz-Signature=c7e6eea14add6546584b24c5e8c834cd700e68907695c0e64529b13d00cbd2ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEIZU25Z%2F20261009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261009T033727Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIBG3zc%2F6s5r57LQZr0izgxNN327xFo46pC6ymaJ0fh9rAiEA9ViPa80uyXGtt%2BkZgG3FKeA0zTbODPR9MGgunAc4IoAq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDNPV%2F3RlOeDoW4VKgircAzkvs4asVccXD5FP6Tr94kSM5Av0o6ZaOOUQWEvTcM3d1b%2B%2BATpFvBo8wefbX20hG7f6i6MSrEgCknWFvhDS4laD5QEC0zzRIx5A8B7kikTlhIUfgcrILZWCmSQg1wOIoR9U2cPt5TeUcyKPvGmWMssMhYy75gOqU8Up%2BTEATh43sJ%2Biz2iY%2BKPgciB8tud2qTC4jBX6gL4X0V3DpbkkLANfFqkiBGjR80gJMfeP5aNL2R3zMRglmCdYrvAspiE0JZKI69HOq387hfP5GZMziO98Jj4k%2FdP0qeADEuuxD0BDQIAtkq88e8Vo6PEbYU4sKR1QdQZsGVGugNTKYBemy2eq96ujqFGU6ub6fv6%2FqSTO3QgKxeRmnECaljkAa%2F5VXEbqe1bS1T02reG0We5MmY5vE6D4U3yA%2FmUg0eXsM%2B5rkcYug6xcIqieWKlTS8L9sCmVzrN%2FDYHhi1QdR7gjrH%2FkDCisWw4BKkDGrFojO2H3bOwrKXLPkGsVCuDeze5JT9bbqvD8v0FauBeJ7TR15YPSqoEe50gvTR0O%2BF8okVPdVNzg3P2x7NNHQYo9K78MBkv0Wt7CvA5on3uBzb%2Fs8rXDzdcrYnzDpaVRjwoeNcDAJGvDLc0Lgal%2F9m0dMIG9odYGOqUBJ2SzIKobJNpRHPvTwxzGe6A2uGg%2BRq66YKYHQaTmKN3CJyMTXRJAIH3mUcC8sZVtppqh7v4gskv1HOWkrAN5AIFBY7xE8HyuRSsumK5YwHRweO%2B1aH9cmxQ1KgqDmMth4JW9jenFqCnLqUezIfxnyTjAsCz7nfnPUvYZ2l0EyYZ8mbJ0DUEbhFM204259jBf2C4wKTGs4W%2FhhoO%2F4nAdVRrgDnDG&X-Amz-Signature=484ac308cb8790726af7aadd72ff8d28577396f7f454ecdb2311b323973b2063&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
