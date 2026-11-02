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
fetched_at: '2026-11-02T03:21:09.771Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSMJORW6%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBnKzccbheV%2FfwoJdqWFgTPKlUEbT%2BCxCJFPR9mn%2BCyoAiEAlltYlTLzVOaHeDxtl2AmshfGANhBiwSPxDO%2FM9Gj1YMq%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDMwmJS70eDP88ZNr5CrcA95dLp0BUJCa8K1n%2FbMruz6bvjtwTvb3oqFio8hf7d8cdcblRwi8WkbwAJRNvFbIshIMonLvRAam%2Fo6HdSTY%2B%2F1pxQoc88Pwj0sIdy5nFB1om9tHHBsB%2Bm78F72SG%2BXODG%2BKcwU2jRSrf%2Bb7cTFVI55vdHL0vomsO0po%2FoYa0daNFsz%2FMpHXgBCfcZhWCmuPlHxCf95fHocT55I%2Br7fF3pdU3mtep%2BLoVVO47DeKatMMQLHI4hQMhmuk3d86JNo6zafx6VUnE6QZ8znhV4o0gcfBDhTnzGlxvgYkEXnYsjy32BQ3N4iQtPBXlUTLnr3wTYXSk8je3UTgz99GL%2B0yvBy5QtwEOcVQAqy9YOYiLcYiU0x0HdLZI%2Fn%2FfKCcBW%2F%2F0gYWrBzR%2FQRmxzKRy0zHApKkIgyoEwmoYg9ovNZxSw4Zitt3mOLCWad2%2FpAiHZRr1gG6JUCARESu5cajBTSAXFC2nxLvvLvLb48923acYnMFH9i5swBDOrlUP0eYP1uWYdntcywwWwhXXstg4zqNC7HBZ8cN%2FZMt6mMX3wXgLXz55PA45HdPS%2F%2F5ZNlcd%2FwTheDWKgnqpqbmOO77QjDLOkUWwkTKcgoB%2BVx4NWo1LKbjmrCPSZCmU1Crp7%2BuMLX6n9cGOqUB7KGHRseOiV2K%2BEaX3aQl2a7uF2f7ac3e2PcCTU1REmXmwiAutM0oH6dxcwEygMZO06Pa9GtPbnijg2CwFQXCfuzHFpr98K6zw7gItv5R6z9poye2nH6xOn7Epo%2FixrXiKhnsx4%2B2jMBkpsEveWGpc6Bolo3bHs1xHmNtYb1ZIjOCrFIuQbTA73hc%2Fp6X1n5uvGwjTvdyza%2BNLM%2FOdLgwimyGlm44&X-Amz-Signature=30aa51f20053971a7240edf47756cc986d5274c092142bd09157f577f5a93693&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664P7MDUV5%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDmoFz29UmqlU9vKkjEVZTFCquM0sZbZp7nBpoT5TCc%2BAiEAstB0Plmemw9%2BFmTMq1fi9IeJ7mOF1yBYYuSBUg5h3Isq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDLKJheXDljqo4LHNZyrcA2Eub1oSfSJYTtZhFzbPNdVphhWIcVjJUka6tN%2FjkUWRUC8hhRSH9AlYyPFrT7klVAu6l1Qwm%2F6DMrP6CuybE2IfN5qXKYIjHTOH%2BKRsulC2m4O1PQvyya4FwfQIKNzVz45KrnjkiE5SEUdcxPKYVL1ht8DMPRYy7Zzlp8pd6sLMiNOevS5OVjG9jdHYz5KxcQciB%2Bf9EJ%2FDb9nn%2BI42mMYVOg4PCypZbVBBUJVRN%2FQAq%2BbViNX0ftIEnXP8V9GHKAQq1M6dufSo6m8PQsJbj9WkkjCXljitZ0%2Bqzh57QD1JPFrejr%2BwK2K9%2BzNCFpo91tt30aLydNgYyrenk6tKyHfqPdYyKuqfhyexnH3ttr%2Fd2jSUfSpa0dEML%2FRmqa%2FIA2M2d8lMF%2FtzqupQE78HePV%2BjOJ5%2BKEnmNUJEAnYZRjhnnGR%2BBwuoBAVR65%2F0OypGoAjGIAO2AP5M8OfTmzsxTBGlUN%2FAJDPvf8JR3VmyEasSjcyoIkGw4PqlxX0useURvADGJlGLonCixNXwB5hvKO811Azc8RlcIwDbJs1uael%2BA2Pn8wtKA%2FoxTZv1Vnfmi9dx3qwJty3FHjVhXHS%2FjidYMee2HiTd76OawPCGeyN7zShroRghIIHljtzMML4n9cGOqUBQh%2BSVP8QrbvAEYVmwibtilBrGSzhtEDGu5t3w3Q5FdOmy5hZG1PuYEhV7UEbxSej2%2B2cmjSLSzkKT%2BDGJrW0%2BzC%2BonSZj%2B5%2BknwwLSGvDC9nvftk2qeF1pOTBUDs%2FvM%2BGEVHcbOEPigLr1hA0U11S%2FVdE%2FmV9pcTRll3BlO7B0IaOrjWmO%2FkzLBJMU7t3vdcWX6ujOSB%2BCdUACZG1g7dH9TGdAo5&X-Amz-Signature=647aa8800d6821e13e777bf91159dd0678acb0139347e54e6e70a3db124c33f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XABBJUFJ%2F20261102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261102T032107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDq3Q5m6uZ%2BFCwIkKgEEzKjgmkyB2BYAakbNEtbJbl60AIgS0qYJlQjsLTG%2FZcbx9hADdu5fIjfz%2FTjE72tyxZ%2BFpIq%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDEO%2BLsBg%2F5QsVtOt2CrcA1qSq1z5EnqCU8sFkiOZEmtHuCku4uqC13hO6VSgs%2BE1jL33KHxc7Cjo5gXIhci68kESy9%2B%2B407HX%2BAXdTq7SOhmwikVKJl18FXNucR16JqwwseFjivTxdl0gZbz%2BDogFWEz2viL4%2BmQkQShSvAhhvMhkIu%2B%2Fr89fAqq%2FzmE65LEJJted6QFfD%2B9tFC4jYCGyebb0HU1KLzoEbTbLPxEs6RlFOSPj6lzeEgRDN7j6vkicX0lK9nh%2FyTaxZ%2FQGaa%2BLmMMHG%2FlGZKucbfOUqMsvFhgHC3nDbZh3eNKENHf8Qa%2FXogPVnhXgbWaDsxlFU38mvLtXwYDxWvT5u%2FJW9Dg01jwbBu%2BwldGGiFWWw4v3QesyrFTD3PZv%2BRac8kxePQOpQpryrEiLsOXQppqzMT2emDAfJg3%2BKLTUxPxEK6wT%2BJQXlmcHztpcQ49NKNtPfvKKXiqb3%2FAfb4wrMyEnY%2FFdyrF%2BsiY%2FMQPPeGY8XBbjCidqei%2Bx0j8IrHIhk5L4De2nP6wcxxU%2BD9H1W5POc%2Bn5dkR%2FgHZomlTOQ8b8V4iu0DAT%2BFL5q9zPLXFQIO5nC%2BqacswRxXNOMJDDl4H2ceJ9NaAz8JwkORLRrH3apiN%2BlPbONAqlSuAmvlhV4MXMKH6n9cGOqUBg2kHt0BAquZkBcuxvLMmDjGIN39foGutfSkZuoxYQ2CtPZveIcBAAzMgJGzgb1wAZUcoj%2Bte7gzGXpM8UC%2BLLB3OyJRklguF%2FqBJbAQdK2LK%2Bo37fDuJfzeOSDQjJ6Q0JJ%2F9Wl7AdcPUMrgxnV6k2R5PAfqgNYlERQ49pH6yAaep81Cc9osu7SZB0rgRdC8KmxfOtBAYBUjMNLnXdYM2Mpa3LEs6&X-Amz-Signature=db1fddcb3510d7d9769d8043ca71fbdd37c63c9a1146c8613c6c8a8205223b50&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
