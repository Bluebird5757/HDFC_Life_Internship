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
fetched_at: '2026-09-08T02:01:37.491Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664YW6VGNC%2F20260908%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260908T020134Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEzaEE%2F2yyGrBPeu1Yy6y5x9%2BGOddaa0WIpQpInAYXN%2BAiA89BIFO1HK4a1m%2F3m2dAHmHQA1wEWTK3opbfmA4eaP%2BSr%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMVr4%2F%2FnlwcmedXzViKtwDDlz4bzYOAl7Gcst0WqMfhxk3J%2BYzAtEPdyjd%2BMhvv0qxLes1RfUd5%2BpmfrF7vOWyYXPBcF%2BJO3J9sV3tU9ngXNgVdKV7JcimQT2SlaUkZgs49EIHZLs4HazhnmRjwDxNHqC2%2Fb8QXUZObZsPIbzoO86wh5UrnX7rgluDZDSD7adnjnrlzbt297NbKDmJt9p1wCOsoGtTrsS6lYIuheIvd%2FOWBxw5BVXImhjrWVOqp9qH3aYB4jsv7fgQh%2FFsqQjvx%2Bu44nkYemI4w38sPCVrBW%2BwDfKupJzXB16FOzjV%2F6qCoMRhCFcew%2BKBR6iBxqM1oYd1ybO%2FeY%2BEhw%2FVbIDdvKdH%2B3vN8Ijn9%2Fc7j%2ByTA%2BGLkQBUzfypFASVGSXGTDIzsHgk3LBJbrhF%2F8r6wdnerwqv2UIcUNOBadb%2FjSS1XLW%2B%2FBQ%2Ba7JmAQyvZ1v%2FY65cNBFIt%2FhW%2FBvx6fMCWAhtRAcsUWvXV3yqMaqZJ78EIaHFfU50vcW%2FnVS%2BTxPLjDgnrLwWRSuULir%2Bw1lEh1OOXObzSNJ9r3LnHdT4Amo7tMlO0hEobS5%2Bs7pgPr2bxMOmEi4jnAN5X8CVrAuym91JVMBm90bCPcro2XcMoNDoxkCnYFIuLbDbuWCIZjoww8X91AY6pgEprCL11ObOFyItSKX2NaGyoK59PJDV0da2c8%2BZe9i9yi0jcNVCZp1vJbi2n%2F%2FRjn0g%2FEd%2B%2BrQPvs9vX2SNIEaN%2Fn82lAy4QHW%2BN7dHVFcUdtNRCMPShk8%2BKtcQBsCMhxAZ3fWzpPxF6lcEFKU5naazyQM%2BZZ28TbAoExr6jMaGt6GcWlL3Lw2Wp%2FXEQZ0NpJbD1SZhmt0qGsVaR9krcSU6pfWkoQcO&X-Amz-Signature=9bba16a30994e4247d406877dc718006609810c40b1268ed7a64b3ccfba73506&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466REHCGVT6%2F20260908%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260908T020133Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDNtvEc4sguqfhUZoHzCYpNNEW5bsEM1SOscQ0YEqKQpAIgNdAIt86g3z%2BfLEVECYUdHNoD5J9DNLqdxKYLyKR7XIQq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDGMkGeaZ7y9YkV3NpCrcA8jPd4srFzVhrfAgXVCbWivR7Y6U1XKcb9WSb62YBQ2lmKiPe21rsc05M3tvPt1ygH5nDQTAIWmO0VLU%2BxDJCb7Cwr982hS12i469GStuTol7ODylwcx44FyE204rjIVeLQ0v0hoeQnC8J2y4BGTSZeaLVgRPEghyyf03Nx79MAz%2F1MinqtMiX108%2FF1hSB1nMPFG0Lcybirps8kt5H7uXZCD2EDKnt0VfmuEKfNGyMc%2BmNmIgCvzLGQ5nESkFjUCP9CgHbHs0XUEekcB%2BbNsjVCzMsoUPZGqDY5aVS4JrI%2FTiRDEnN7%2BF25V5wf2VcJXXx52xubjfup7TQOQh4yGKrTv4D4byAx%2B0b6xAd%2FVOCptw2gySdhJfNX%2BUHbmMEAZc%2BdiQfzY%2Bw3VkFLvyeVf2MgI6Z7GWjQ%2BeMzriVyz23OuFxQh8u8LIS4W%2FUgP1WmtcFz48lVEeiLcqfDx%2BJAMX7q5e5IGrxOZdkk6H%2FccnOIQyzXaj4pnj3Bt%2Fb%2FqLzXel0Yw%2BrRW5zIrwR7Pba%2BZw%2FzXnHzsdD3sNz59Fl7d5rwLMWw0i0WTUe6HyHxGP%2FwUeqI04xnb%2F6%2F9NTGqpi3ZywTWxElOAu%2F9unDQh5EMyYVqTHkQkv4658G3hxCMLbG%2FdQGOqUBb8FozWZaWeMui32gxw0qpDrje4xlLU%2BCcCBe8yUQf7qCPHHb2MO9F3I8Hzy19tRClhEpMmQjq8lcgEHaLPupFoegl6TpHRj5qCK9SnkhMAOnSbBw%2B2atvMIF7yFuqq3oOjTx%2B7HwCF4Ude264TGH2dKcyQX8beJF3G%2BP8Cf%2F5PZUj%2BfWiO6lGZCXUuDIr%2Fgg9emkyoLm8JxSeRglvI39sUFHn8xc&X-Amz-Signature=1cdf328ce3eaf3816501a73d51dd220dc8ae7693051509c255c60e32506e6f15&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632FFKZS2%2F20260908%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260908T020134Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDhNZ8Xk%2FESyxJVHaZqKngaAC0w0vZ89VV5RVfWju%2Fa5AiBx%2F9UTb%2FGLoI86Ume18uQNmUuo9aMXnVTXiM52lMqlKSr%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIM29p3pZpOhp9130HxKtwDt8m4unFNM5AjtBnxwqEcTlnj7dmEIwRLP8mJdqQmpBJ2UQQ1%2FfsgZULelHDPSF2e8FXo3dqzk3zud0g3vydv7dMPuqOgrW41pX6nUA8sHG9%2Bj7K%2FzH2lgMUs89pMBoz1aK6Tp%2FRlgiRR7%2FB3bRTW2NT9Wzqxoh1PKeYKjceA2iLrKPpGQ2%2F852KF5kTIZ5KBM09Fy%2BuXJJPCqJQAd5Wy9%2BXkEyKshCBISuWU1ztuEpYbWLJj5TwlSUwAgpcOQ5r5L6q6dfyFmnj5JJP6QcQq19Y54AUZWSrp4sbQwX35vyuJS86HIhTebyF9uJAz4K0cBwcExy%2BSpjEl3oGjPRCebSzRxmXdJzdTMk51UptgbUItgmSDhj6pmxZ4ISDTGcad0Z36HvQvizPmK0sZ3by9KgXy6m5wn%2FLSnU13wc5XL7c7Qs73K%2BxY8x7P6LAqVWJmfI9hwZLuPS%2BThdQBXBqQsf5wQJWl2rT6zP5ufmpMbfA5FTh7nstvNBYJ58uYcm2k3oB4%2Fkpzm7Vc3OZu5iclJTrOVIKvZWokAUM43p5TI%2BOGgeLan77xXqNFDRXJylIqbQwSK0D77IAG1AOdA3eW1snle%2FtifUJrNRXscLPob6V770UTWhX4Cx3GhZIw4cX91AY6pgGElqGAor0%2BG%2F9iHTYRezvb%2Fryb53s4rodIC4mur%2FZQkw2lc3p8eo8fdRbym%2FXP4c58AJ6qbCwaaLA02UlG%2FGOHRAoQZW2hb4Msk%2BmoyFzcnlIpafKBn%2F7mw%2BfcQlMkPA5JgCjZebvsp6YyIF%2Bt6XNBmlfQV4tjlQp6TEiFwLeSOTyEQgSJ4QIRXVF9nD8y3K6BiSJRdV46rk7KZLUsImDBDCNb3%2Fs7&X-Amz-Signature=c5f2ef6c9913bb642ed425f347b52903506292849ea76d8379849981f5c49037&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
