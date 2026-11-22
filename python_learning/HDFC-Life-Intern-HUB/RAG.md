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
fetched_at: '2026-11-22T03:59:59.694Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46642BLIXB7%2F20261122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261122T035955Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD%2FYhUCqi4V01UJs%2Bwm6MGuRFqj%2BfFbT9CoY2h9kkJgsgIhAIscd6qDXiBpWpPOJftOuWWb4SW9dUiyRiOh2rvLBwSkKv8DCFQQABoMNjM3NDIzMTgzODA1IgzuKJh7x2kXfsX2B5Uq3AOGrCnHYyZFf2KulZvJzK0VFMFJRZL7VnKF49VB3rkFwG4xdFMg9Lvphpy%2BI2NGRodmeymKqWmPXs1HV2ovD1zx9HiwiAlBwFpKNZYUNXAau%2FCc%2BfPvuK74AM%2B1ufmsMfbIQ8xuxCQXhA0A3JpWIDl6dM1Q%2BOVWu67krJKUGGvU3tWdx47aFByOLdswDCIoYI%2F%2FCJnLpM5e1RvxKVqZZO5514ZVpcM46lxMn2dJlG99J%2BqMz1wy5UoigPTMDsccg07Q9HhpuGMppGCc0DutNP0n5LPDzOo%2FpPxKhHXo1zX3Zu0UhFx11I%2BTaMTbG4k95tru8LjOnT%2F7WUjjiC4tXy4cyIpIRtvOJbKtyQGcEGVY1m0P3oRIR3qS2FdNPCXdGTi4cxG18gyu96Kk5pZzivZhavaq3rKsoQ4TvjmFzzmCuYcSgvaeEks5hBp0WinQq0hqtJvfpkk32Sjt95Cvo3PXQsP9VzAqgFG0On3IkRMwaKeOJyIlJv7XY1lyGIKYO3O03L%2B0wKoF2jRRw%2BlDWZFgi8tAwBKr1xJTsjwXjatOYGTgQClJ5otdU9AnKDs9z7xv3dAWxls8iMxNSQOpsRQhQHP%2BJsdZW4GWI7Dp4NBU%2BI6%2FIdxhNi76NXyhXTDep4nYBjqkAbSB7napAYJMPWshVKbrXqfKPQlG5ve18WstNxEXtoC7g%2BiiJSIc1%2Bfs01u7FgGa%2FVLfPlzOV5bT%2FTDbtp5q8Lz4BHawP4wa7sjV%2FLL6mKWU%2FR0DisYcXxKWirbMBVB51xNFyWINjCHzod0MP5c70qhWI7ye1HimqfYhnJIEJQO6Ke3L8kfDKbD3915oXHH4ZweLYjHTfDHc9X3SbFTadzJjsLqk&X-Amz-Signature=e2dc402b0142508c458fd32f4d7a1c8993279214ecb857643136bbde2501bc2f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMJ5JRSQ%2F20261122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261122T035955Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSEH5TAIA4D882Ltc4FrL6%2FTC%2FSD4vYmSAS6jw7qTRvAIhAOZ8c0buCRpzHv7fokaSyVksFSmiI2rd7ix81yxSPmTXKv8DCFMQABoMNjM3NDIzMTgzODA1IgwWco%2FqUo04ehXI6%2FMq3APgmmVaAkkQ0DKXRL9v44EngL4ql8pPrZG9YDYFsGJ6yxW65ZWy%2BzNorgrFt%2BIvI7Se21E3U6ESTRnf8r%2BjypgsOSH%2BbqI1OTAvYSgCy6xa21UbH8LShpfFqKXE%2FkTuMXBFp6wDBHpoiXsPxvouZByDfETf8PwLOV36PL6ujNGemfVYnPAraAbuWR0y3aRdglzUOvyKzw%2FPOdfaWZTJHeF4SIiArYJk4Gt%2BKyPL8pfDvaBkkxA5Zo7wZjlj8TO9UaR8RqxVNPQ1EHyaYpOSGIEBgcdM6uMPjqOV88oZOD%2F1YVe2dQHaKgF4GwmQmwo8cEztX9vB2zmspVl2HQt0OYVt9gH%2F3j8j8waaQspxdgUpOzrEUEviKs25R%2FKsdNw3Ry%2BKplifSruBDKa4Y%2FbeyUD2HHzhnaAJBeNCuLCLueGFXhEL4MhRQoxujsCVtuyfS193fYcA6MId9AAXdP76fmRXU355DY6ZMNXmBCVXyUK%2FjtuI0%2BwwEMRy%2BgJSm0FCm5mVVoJlrKIjIExhCDb3mfwGCyanhdFfvxkHgG7t6ZgrwOxZEd1twA7DdaD0k%2BjG4ECcaTQ9TnOMU9f%2Be9aGBtkvn4VaW%2FPJ8TIC%2FJmGgy9BzcjhZOQ4jkLtgBwt2jCjponYBjqkAYTau%2FD9dt8o7iPT243Fmg2nTzcl%2B1YR1O%2BW1yH7WWG7TM8KMngfstSYuJqgIovjepVHasbXQXplxe5JYBzY8XeYxf8XSY5RZoozMxVFuGYuC%2F2841N9CqcVEO9iZm3o%2FvW%2B3Pum6sV09A6Ovenhz9qwjgfEWHtxgaarV8NjiJcYIZRz47InhfzAksQuFkeey8MQ3tUs1z9olB0shlGTH5x33lay&X-Amz-Signature=dd8b9fe5f6607b7a4f4672089534e6be7f368ef22f69956c241843a9fef01f1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WE7QNPW2%2F20261122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261122T035956Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE%2BV0Z8Q9DvC%2Frvjafme5rjpwNPyf7Xycc1hSg6aD5%2BFAiEA9SORhy86Vq05D2l19EkxNXUXlPRqJeflXRY9Ofmazx0q%2FwMIUxAAGgw2Mzc0MjMxODM4MDUiDAzXspy68ll7WLnpBircA4xAsRu0wvZd8SMVnzn2fnkRoM1ugaC8Sy6Flux58rhEIq8w31YHQ9n71LbYgceBy3M6ZA8GDmdSkOWcguHdc%2BbNOYjTIwWU2AeTfCAsO34FHN9F9fXg20i3P6fCszva%2BIT%2B6H80H5WH6gvOYfIJmvNNaH1zeg%2F%2FDOr4xp8eDX9J%2FUW1K0E%2BqgB3cvQPY9H%2BNS9JVC53gtfmV5i4S7zFLQ22KnHPyv7KJ0mEm2GJKEmpNn7qBWq02dBUwhUPdnV%2BfeowMXdt16Bnqfebk9cvE3Qbw4%2BEBheFRsZggwaN9o6gIBXBr9ldoUuMiKvJ3VFKFhPHZfQ1qRBzp6Im%2BIEOuv0JBEJAfjLw97Ii6Zj6kT0yRfmpO07e%2BKK0%2B9QTQigld%2FpOEjGRHbewQTHmi1PQa13bMNrUswUsNlqJ1Ny2IRpphonp%2FjIJHpH7E9F39V2pGqErGZfpb42aqXfzgBnl9P3tTkGCwjOCshdGVUNHK2Oul5zmvSv%2BOKH%2BvBRh%2B41IbAaNgTOBj0fwojdpHyhu1BQQdBQTvZX%2Fy2sQlFKaK1famnI97TiwS8MvN9aXdsylyiwsOQiFeKec7MFg%2FD3eiKyg%2Ffk2Zb8uBq0vJbh0%2BJ5SHUWoG079f%2BMJWQm%2FMPqnidgGOqUBuJLChSruw843w%2Bxpagba6aHc7X9R%2BEPUNMW1c4QLHJAAcWmRCwsXm39cRLXTX%2B2kWM76CEQ5T%2FX6eM1tGnCA5FZZXkXw1aJUvIrA5YpPHcyk4wv%2FiBnSCKDFl1Rx1vOXvXIZoddayLwKdzzhu6S%2Bhp%2FUSEQ0YdT94GfM6oFpTWwGm9iQg%2BcrVtgnbX%2BK023D%2F3vvBaw98NLUvJ0ChJ3Jh%2BmSG4Cr&X-Amz-Signature=4d4b65a2009ba0612a5483a3a412d32fd512c0b4379bc75f0c602df9a67a5cdc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
