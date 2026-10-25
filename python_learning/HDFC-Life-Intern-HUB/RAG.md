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
fetched_at: '2026-10-25T02:15:59.769Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S7IEE542%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021557Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCour7DOOyTKpAPKqgHF5m71han2V58LZHKEMMcb1esVgIgDGxUl%2Fq6Yti%2BujIkdFeNc4of%2Bhcbkv32ReyKG0dbZhoqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCLK1AuxGjfdL%2FD8GSrcA6reNbtF9VdJi5BUfYnyOjtGMxJbvdDVSrsGm6p5WXEpZn%2B%2FvIrqLFKBWxEPLCnJst0o6RHVbXtuwnb8PzG%2Byie2kQcDUbAhD0%2FpqWdZND1H6Sco8kJDFgPEbrmAmImijIFt99A3llMNkLiNiDdzcW58%2B422PqZy%2FRXmblShkU8RxSTVaBPkl9%2Bn4dwMzsrqPzyAJMBt15zY%2BAvczAsu%2BVSGuwjToLusO%2FxXxC%2BWGhjBaV5FPPwzY5uOxV7bOqwDhr%2FurbMlnIbi94PRgDZrbYGX%2FgEDWyX%2Bo49sW6mcg8WJLH%2BJKMiYerCYAriYbK6IPUdmTe3zgMTY0%2FKaVfi7kPaA13EooH8Whe61Bv%2BAaKtFWbCVjNi6ta5JdqPPwyzJsF8fO8yvThMEk2YMbCNlv4ER19uG8m%2FwE0VMDnQtJIXiO7E4TDc7%2FA%2BmkTtkvKWg4P3F0Xvz7vaD62Erb4ivNSQEscxpHsdiUJJvcRzuO5VISx60BrDVXE%2FpgU770ruWllfsOz8fCHRkdze59JGLsg%2Fkm1p1Q7TmQt5VNKWYIjkg%2FlF9i%2FADbVjnLOTDphDqW%2BzigEgOcgeoz0KKzXAeEVJu8u6YyU%2FmzFNsWzEqjKcvFy2UoqB%2Fi%2BQQH3X6MNmv9dYGOqUBkLd5QZ9%2FF3bc7exjjxfMaLmPDRADrVMuxgRZs4K9bh9T1rC0FQwYv74x61xfmBhlzohp8kExIrUx5MsjVuSsko1KLnubhDof69mMlkijR8iavXJuWzTg8QwqxfzEpRHrtmLm4b6BdfkTuaA%2FVWbsgQwCMTUO0K1zmjizA%2F%2F7sI7PnITv5K4Z2WVsq%2Bcn%2Bk0kdopFJqqYXVmUSz5vW7AOHl0R5cmf&X-Amz-Signature=2a49214781fcd89948ac2bd60320aba2219887e4a088d3e4e4af9331df07fc30&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466STYNB6ZX%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021557Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDGAaW9NaRtufz1EroEQ4Ag9flFF%2BTu60sf45NfJYIo%2FAiEAlU9K4LpXex50U%2FrXHcU4De6jl09t5UBCV4s3CHp0Er8qiAQIrv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEmD%2F3tM%2Bh4MQz%2B9ZCrcA8Q68uHD0zUtA7huybo9dAV5X3OFc4ffRS5ttQGm7Z4XXVL5kGe8HYihjIuAs5zU95cjTWKeQJhWdbLvymCoNWhg3iry9i3JDika9%2FoBwhvWzaejiKSvJ3RDmIC%2Bb2IE0F5%2Bz86pb%2BHD7riaQd1DxAZX078Vcd737G%2BhANENsUggBDKbnp9lqaRret1hNnx1X3Ye2woG9U74gjFjy0V3NywGVGl36IQNE8oM9833fSJuNPp1AzAiD%2FEmwauIVTHcCXq2iF1mkKqo9esHC%2FQfqVmvhSOBNi5TzunDVvg80dCYpbdJH%2F1P6ER4Sjs%2BcEztFoL6mE49jzOfWN2L%2FpEnYSV%2FfcrPozWaRfY0h6cGis2naAPw%2B4jECP8aeNVFhlvCey17%2Bpo1AEhDmBoQvDMagYM0gNj8LdBQK0J3f%2F%2Bd0GB%2FI0RafEBTmfDpddXYo5dpqYeNPsqK5QqtYaIxbH%2FQ6pSvRYHMtaumjTphALH1VmeUystVFSGdNb%2BCeQFH2DUUeI1bWvz3Q5DmHTiK6fB1dW3t7f1ZqtMhXjf9Nk8zz0OXHHtlpK%2FcFGfpMGFM6koOZtFjjh6AGVO7YRb6wBpPrxqWBnDLNzcz62sNEO4QSIWzRE9RTMw0NE8oo0kEMIvV9NYGOqUBLP61l4lag%2BJSjfNyGGLeyIB9fHfQa2NVpAvexyW9H5tBtbjVfjmoeGIzFtxAdZYzSsGKDcV9miunlqFDpDik%2F52rWDP%2BidonaXHPeB%2BsVI%2BpHVWqrmsoMhP3pUyLHigNYv3v7CxU8d74pM6F%2Br7ihPerlVqz7iMYF79rTu7SzNe7BvKIV1RUnvmufqGjYQKqXU%2BYf8PrsGAmzka53Uq3131MeviE&X-Amz-Signature=7addf17526a099a8ee38ecdc585c4e065394c05ccd8fd2f39811b0a4ae2599d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664BMBSCKY%2F20261025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261025T021558Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF1JnJsE0FpgJ5rIkuK0e69vQJqG0Su6bcKNcGUsLOQbAiEA4JsrFBkTCVGn6m1poCdPZ9T2jEwQnlowle6wcQ33inEqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCaQ64uMoLH%2FrZrx%2BircA2Jeo2CeuH4m6hOL9%2B2093Yj02jffSEuFE2UJTC7tBmCDitXlWw4fgjAo3wwjaN%2FJ3K%2FCiKilOw7tcU9ff4KK2IwRQiqVrzgXOHrKp2CLYEdiv0CLTJ6nYUmTPdliOVg1y9JWsBJ0Bc4fO5hpmLwJ5E1icVUFumWJOe%2BV017Puf9QsBhyPzDs6QqE%2FUQp63gVJQ3o7u2%2FLla1C9oiBezd0lutrwSxxjhf%2BK1yKgSYtZVTo84wMwjlYi4I00pixy8%2BIreVVkDrWS7EjuRgspuYH%2B1PclMdBmQXVI7zonJOAPQzQVlfa4EWPSVLafeKJU9%2FcHN%2FPZU%2FrWakaJbSDiyLIyAWao4b9QOBnTx8%2BgSVoRKl4r7yvbarWmN70rVfhCdajRJn4qkuq8bj8iggrLAqOwQmIKuhYmLNj3kFjDkdcMBvcHZ6Xf2sbrgb8slA4c5I71p6dStrRh8FnNADLoc4QUPxRpBwnIIRDeXmwXSrYWMS85c2UuzzJRPtVwi%2FOHUFC%2Fc%2BjWC4cw5WYZ%2F9oPL1NY0tbvs%2Fh80uaBaBqVpJtpgJ7AktqrZMKhihIpdGYBsgp64Un6q7vWw1dY7NySlwvEGXPqm5SJ1chEIase4S6Rzss1NkVSRKHtnw%2F80MNLV9NYGOqUB07a2WWxIw8Kmj0dphBW5SN8G10vZAQp7ofXIVwE44n37zKdz90veSfEQUw2Z%2F4ohJ2S%2F0qVKuWm0z7AIDGvECUwBv1bOZrW7SfN6HPQwKhT%2BM3fx9wDG0C12jHrTPwY%2BLvCaUl1CWazNU7odw3d0rLQF7u%2FVf%2BOzhB8bEBu0fM5cXJ%2FC7m%2FBi7JFlgEXnuDqUwAuijrd2lTyy3hRO0Y7tbcSeBuu&X-Amz-Signature=1d6c91e2d8ce53763ebd9a3b8b763db8823faac549c814f85d48c8e3039b90f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
