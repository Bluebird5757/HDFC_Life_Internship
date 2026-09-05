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
fetched_at: '2026-09-05T01:58:09.882Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EAS4KO5%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015805Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJIMEYCIQCpRlfWwFMZTtDjGERnIIi%2F70DGfQlaU4D6ZSc7VLCnMAIhAL8JlpfNTvrUaD3NAZ1SigcPeYJ5i1Fj%2F%2BxfXHp0IurFKv8DCAMQABoMNjM3NDIzMTgzODA1Igyo7WMXBYQYVuQ7Ny0q3APvXCgb1WjoMTlMRVIZt3nXbI1M88D1OP85xkryLAMgnQsn16lNNfZF2AdOxKDLUObQ659SWff9OiRBIz7kEe8RYhCSawFIh19lHGavhJ5S9mI6sadSj2FTiMTMmYLL4DVAkBuJoRLUtG9N8yvbtZSYNuSQVterMyxnNI57%2FTCMneMa8%2Bn%2BWH3UJBukeCeyylEKUAPPlBbILdcEhsGgzDNeKnspHTRIwuby1IcMlfGuogySSkXQerJ0DbNO0lqs1VE9HJWmDforuvixpLDFkczzY3EBSjUOwOuWn0f4AX%2BWAMHLi4962KGEKg9I6fKhUzCpTA30LaLGaJbEWq8muUCM6kdXsLpMr3SxtVg8fxZp0KWMrvu%2BLngZK2gGKcQ9BhYD78N2MZBGGsnWrcgV6c7K%2BmxGXuwPpEQaWMXoX7iDPbHM69m0IiZuB7XroqBxzNTPJ4UQAgQU5CIRkingwhdCidI8qBzfCG2QPurBvzTEgnf1CZJrPghSu9hGuwuIv7%2Fh1sBcfPYtcYp1j7ZkWr5RcGdn3hmWdRw8bc70DtTcALdqRXfGY53Psh01cUZmoVlGjPXqkhngJi8tBrIgAXL4FX8JWKzxIv%2B2nSm3ZIcoVYYQk42OnWA3NjNNCzC15u3UBjqkAePKo8010mzajK536iopVBJYC3Jba08cziyJa7%2Byy00cHODXHQiH%2FPtX8WnKbvs%2BPMftwlijw84cxXDwD6%2B242tvEPeg%2FwoFbhxDhZVXQzQdVNhwblJ4vZFqO0%2BVUGFaHsC0TO7b%2FFqrmWTIBU8FzwLbp7z3OLY0hS9Xap453k8%2BUyApSCZDRRhHpxe9L4Gu%2FdakXhNsMvAfSWgoBnXZ0txGx6Od&X-Amz-Signature=72b1186d57d0ef68116ef6823c466401086ac6c7477151a98cf4fa0829204205&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TJ2CMHYU%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015805Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJIMEYCIQDdC1JYje%2BkJRPolCy%2BSpXYBkjxcuwY%2BWIsl8Yxve1BzwIhAKRhsuSTjSkMZbJ9RprMgk%2Bzv76JRol3QlPH77p0ymfqKv8DCAMQABoMNjM3NDIzMTgzODA1Igx1I0fALtnlSUxkXyQq3AOCAwgNGJk8wrdj3JSyJgZSiGvG9iFfGeJ%2Bb2C4v18lqfz6FQBY4P6gzX4l%2Bb%2BruriJtrlLllP%2Fo89Ih%2FCwgJtiXIUk3v0K8bkqp42fNzUG48JPMNu%2FemKFuFBmKmM%2B0%2Bmgx5hHtDdB3zozyfbZNilsiaCxI5hGxgjOiGNMVQxZhFuoOX4P8BKKV7ZeWbg6OtJWJVqU9WaYCStMQ%2Fgx2ojO9yow9hSHkzqADD%2FOCWXuTfUjF9OLQnTYsfOLeAhdFu8pPZH0MJlxY51IbKNkeVTDnKbYa5IW3ig6XBngJVMMQ67UaTsBESmq5pAqX3N%2BlqYJ2fhId48VzhavFTZbfshxnUElb9BLCO%2FXi6jOagwbO2waTQceu1XXGcHt6XRVb7nfqogv17nzS5kM%2BvbO8OwkUU6zs9sHSC6c4%2BrfKnxUIDw9omiO0SDDuQaNx9JSTyJv%2BcBligVq%2FRAuyQJ5S3GnIMPunGZZTr0hF%2BwxNwS9uyRwygWgCPhmHYwYYoNFop%2BpC06ePsoTpDknfK%2FyVxDIAyI7IP4HgLp4GpVNr5DD%2BvFAr4T3%2Bu1aKT2gbW11ULUSXknu%2Bmbk7waQKa5FHQY4p4DglEm41c0zJ2f1l0aFfeAPtb%2Bt0h2PMOUHSzDp5%2B3UBjqkAULENs16A3WHjaiZdWvKs3UDAxV9AiFj3Lu%2Bo9nrbn5%2BndZfH%2F1aOhVe8nBGv3WdAWnll7R3Lv6FJrKftLn0r16iJ1k%2FKGeBrlcOJ0VjFlUUgQ0XxdJgbK2sURerw9%2FQGTnWcoY%2FaJHoQfQNsXtCm%2Fppn6tXJaX3bzsr8yESZwLl%2FRJTuWyhR0l%2BktJ6qxoBOaH6bZVv%2BEmSwiviSXXnR%2Bo245qm&X-Amz-Signature=ee49dd8efd4936047b5841c9df98aa52ace09609b410d842ca6bca88ede831e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YM3GRSXI%2F20260905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260905T015806Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIC0eEhwBW3MkYlVHm6wsFghtyCsWav7VV37Tw2uLKsvWAiEA%2BQqMujUM6z7744WIH0DjYsSNxZkn%2FpgKSCVwi%2Bs3TtIq%2FwMIAxAAGgw2Mzc0MjMxODM4MDUiDDh9Di4mwq7YjRz5ByrcA9hQ78VS9vZcOP28xlNV%2BMNWMp7hOrzGua8FqMumRNPdMqUIRZK5lSljP9MF7la8CGb6423DrhokJTHmxld9q6VfI18qewfWYeERjvIDSnMtZPPdk3eGJwlsNZfptirCmGDS3kxaYiRwEMttf%2Bfz9wMvqUjk%2BO3Peh44R2aIUkrUnsuir%2BZsK1u%2BuNR12u85ce3VMQD3xXUjl9KJmMliUNhCNVg8cU%2FwgPpeNJ0FMzFUnYjwm8Pb6cL2%2BZqjDeir2ofDC5%2B3TQD1%2BJcHrmDD5s%2Fdpzjdn4qaVFss8Ue5g7Ot3cBfUfxgneMN5fftABVPmaWzuB6XZAFQkQkZI2rx0IVhU3884c5jsBnQN4uBvGPHJZPWHZO4lTFSXb%2FKRgUUmbOmHFdOI0AhU60XBBbgom7cD5eIC9ygNPNF%2BKoWFjDeDQwxu1Vf3ukm9fYufEofAjohEB2legJ2QZ76S2vp5dqyaaTbv6JAoAPmNsqpADGQqlqvTlDUvHvFwa1ERPwWT%2BM3R1XvO7C66NwivsAkNdI4bAEspKRR6PbQfcHsro9MBKhQDiEGG1kyOWHRa2deEuVdmcrJm3cwLQ4wnESo8oem%2BPbacLfJce%2Ft5q9nWkTbj9NjSq3yJTGeKNn3MKTn7dQGOqUBFoDDQqCmNn1CfOhMnuSR2wy3x10FB9x3usmPn%2BuFA7V3B%2FbDlySmOl0TXl1YEDy%2BynT0GDZnZ3ChUVgTofjGqEs0qQmzaOz%2FcRolF%2BXr1tZq2cWtIEKFCOdW3nBxUTBzdRMk7DAb6yn1%2B7ov5iZM9yECjCuNYS5Gh3%2BFXeF%2FPLlz8rv5dFlZXy7%2Foq51YFtADZMM0SA2mGRPovt4jE92UvW8ZPya&X-Amz-Signature=45ad4f51b9005ec38401f1ef1ddba120bf080810deb7141e46b33371b416454f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
