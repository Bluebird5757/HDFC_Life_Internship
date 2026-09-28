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
fetched_at: '2026-09-28T02:32:24.359Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TIELOQOS%2F20260928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260928T023221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJHMEUCIGvTQjwNfT8mso1fB8Ii1tGURiLWg0Y3tG5rXqV%2Be4dmAiEA6lBlj%2Fx5JTskjg2WX1jx1FD3t8TflLXKf%2FC7ZQ%2FHMwcq%2FwMIKRAAGgw2Mzc0MjMxODM4MDUiDJpcfjVvFXKw93BFDyrcA82W0Xaa4BgQuu%2B2DErQSetuW%2F5aGES84Kb4mSjDsfCFPCwqvZMFC%2BwDXY6Dk2P%2FRET1d1B%2FJuKsnFfgxbdywJAe32A71ENlHGGh0stz6vi8730aViT5HCXGCwQ%2Fl%2BLxLMO%2FhVd18zCEwo1BDjFwiD1haDJr8dt%2Fevz%2FbX1kjSbFk8hC7st061LY3X2DtgrEHRHS%2FI7CiY6YdmVO8JcJ819MoBnhujeRICb%2BlBNJNcoKBjnR5hpvZuyTIy5GIaIBKJeusEj9DEQ6yxNpetSB0CDQuFfxG%2Fo%2Bljv54k8T4FvdydmV5w4B1KoOfnJhnXu%2FngDL9xpCzdJ18vPxm5hYYHMSowMWpP8pWngY93AMmf3pCLA4Ry1QEDxGmAaVHByBr%2BSwIpAbax8PlFiaBLl6E0eOZEhfV9KRPm6jGZmrDAW0j%2Fdpx4fZ2OQz3fsV1GndQDIDBxU1N%2FaMGQkoWfy8EhVafj%2FnppxcuS65aki9kER4yKGrTIMsAbCa5AaUeoNFSHc53Jjr0k9qb39ooPyMTONUNsLOQai9xSCGwUm1HMRFU785oxz4oXT2fBHKmuVK4QORsRWR9kxcOruKLL8%2BbBoYRfvFR1HF338oO8IYySjsmtlsdlNdD928Xq7vMMfj5tUGOqUBBJsJRTTEDtie%2FtkT%2FJe%2FKo%2B0%2B1T2vICBKAG7PZRik77fRsc9ABs7kuBIu2kjar%2B%2BNLTJYj4k%2Fb2XD9cf7VHFh%2B1a07%2FNjTGNEXqRJ54OvqGsyFNxmC1l4G1pcv%2FQfrkD8BnWeXhSLapeKH92M0NtAJEp5IclIlD2DH8zSxnxf8vvAjF90wmz8LYIbLz1Ceod3HIUVlcyUMsMNB5vmPm2ndxfCArz&X-Amz-Signature=5cd68a52d0829c33b80bae50fa6d133abb39574e1f46cc8f90235ceea3bc3b24&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6XWRPNN%2F20260928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260928T023220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJIMEYCIQDK27VUmJ1CqzhuzZPyU59lphq%2BlQioGCkEsOA8u8AKZQIhAJdKF0qBlZXgwRAZobChTG0NH6hIGH3onl7NKT73wlVMKv8DCCkQABoMNjM3NDIzMTgzODA1IgyxPql9JjokkwJzZwsq3APej9Wussp5xPhpB7mEoF7c7uSomhAR66%2FAV2SMC8bW%2BfoXUziCn54aeDFs7RpJ2jlIboADdgNO4Hb%2FvxBtio3Ba91K17FyNO3LezujYCOMOyA2oMdgB3f2W%2FqoqpJgeQITycs81Z6l0Bt21CZNSWYzeRoE%2FHk8%2F0DsZaYeFpuZVYobgVfmADNKEjm792ElwbtnUDsrXtTYcYDXyDBnBs2FcB9ZFVdLC1PsyDmZADOaRD%2BhklyieH8X%2BhP8N1Vj%2FOY7JPlv6CAAaA9uXpkCuXpkRhCXqiD5odE3uCa%2F069DGsk%2F33NOMpcHBtovelbA%2B3j%2FO1kWyyPpWP29ZeFJHtkPI4LcS35yQRADo0liqrOzBEXrKvrcxx9wE6nS5a%2Bct%2Bg1vagziITHD7JT8agEjSsEYiKKVzQDfQdgquLAesn3B5FzwW5%2B1QYpbGoMj3da6mYuLHJ28w5uW0J0Xkb2ue2ANGgBWAu3i118lHeoiY0kl6pP2uHxNdCPbYYdfIsml6E3IzF9txwgR5TtegG5QamD4k38PjC2wz%2BrG3S7RwbaFWp6t%2B%2BHw0PQPpH96WUzEs%2Bw1N%2BYeafB%2FLhOEpzQeC4E9Yk%2BO%2FF80Put1wvQOwpxDICWTp4wHs%2BL6NUzqzCf4%2BbVBjqkAa%2FY6B86uP98QxHqC6w%2F74bVFkRzdGcj5ZQQ5JhBdPEiBW8kCaW6g%2FjBre1qD050cRaKO0RDcxPXTWFfiPyPg%2B6WxjPlcrmAigQMYhHxyzvzw289D6QfbubNBFnTsWivRFy5CdHJtOMIsYG85L4Kynrmj5CHw7QiGjJcOx01VekJXWoaqwhZHCSpyuv3ku6OdjZ2dWjSR%2FUX3cH3hevP7r8%2F5mA0&X-Amz-Signature=6560b06875a708bcdfc551d39278243a1f7f41e84e26d11e06ebd05204aa1115&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZQGA5OQ5%2F20260928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260928T023221Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJIMEYCIQCHmo%2BckoeStf6Z9OIOgMoeAJqLANZLXAcxuTlY40aVdwIhALKS2q5Q5CXFJ0sAr2er2CxpCwFqy%2BBk%2FXdIeIDcnGykKv8DCCkQABoMNjM3NDIzMTgzODA1IgwSIud2gXlbicA%2BoAsq3AN1nQv0WQDRDa%2FhJcye1mbfevFnZNBhRVc0Lf8uykrK7poz20s8CGr40y1SofZc3z9vvCP3Vs98TqBEuhqvXlm54kukF1FWtxfgiH35EFU%2BdYR6QaOtgdmqOlyOwGC%2BQudPM5gqnTTdcyNp%2BF0jeb5agp9TFe0rNSkNRyDrsoXdsrmyKEq1DN%2BhmWg27XnIJe%2BeUQxWGirL5tqxNY3W3chsu1sZLtX0%2FqpxxJknUfO%2BPo8wCXf%2BJqOJDRdTVOIOHSmLM4pI%2FxHrsIcl2ATn8g%2BSxurtMTw3E%2BHCZTTcZAqf4ARNw1svQPCuzogiICC5TshiUEEUpHKdUl2BcYwB2OOi0ceZ1850Ja%2FewqhaJR5epwLcO2bcO6RpOn%2Fv7BEFILYjPu0HReTZg%2ByFn%2F6%2F7FkntRpeKkm8SAMvZZZqqYoaxBPjDTmE%2BLJJ93dT3YAgQyHywi4A1ymhZbUalKfS48mGQZSEbasmb9Y4Kj%2Bs3n5EHYOf6E8moe0wNSayXan2hA2g60oBk6rbq5RiakNU3HoQAahOd4rcbzGQYGUz28XkgmSHEe2GjB91XvK8oQqfGxV4BPO6zQy3yJU2%2Fesc99VaIgcs9Pf7k%2FgFBwTOPB5wxyfKHWf9UcIGEZ6WfzDC4ebVBjqkAZYSEabEzQY%2FMmtFBs3uG4eKEFQhI6xboXPPx%2FyvxvPkAjQoq0q83PhJyhh5AeaMc%2BzIPDZLTfgGtX71fgm%2BFUdg8k60OSs2ROQT5pvyblDK3gqx%2FjECeOmR3jH1MV4wkY%2FBhHW5WbAcOG%2Fc3peV2JRwHhbxwoBdXfXdPz1BAfhccvqNQWFzL%2BpyHee6VUcyob7%2F%2FI7TiRefmGSzL%2BKjvwKDwvmH&X-Amz-Signature=277497082c083e5403a5cb4f853e121ef57509d5b1ba75bde4baf0aeb8abe1ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
