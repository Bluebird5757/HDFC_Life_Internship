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
fetched_at: '2026-11-08T03:18:56.577Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YC43NVOY%2F20261108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261108T031854Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJIMEYCIQDqjEOUdV%2Bw4ho2SPHLyO4ZO%2F3d5gSzIe9E%2Facneh6CqAIhAP7RrydQsozWSUJdo3odJLaXnVVRvt9RG1D38eXw7VxOKv8DCAIQABoMNjM3NDIzMTgzODA1IgygxHmMnuAtwgyZ20Iq3AOjNwjMmJwbosU1j9ezsY6deyDro8nFtIUH8nKHYtOLZ1ZWY5%2FxpFcl4v44lsHnIEeB4NUWecCClXyxxj1zQUOfuayQkqwG9XYaz9RON6XPt%2B6OwF%2BxnduBJW8prg9JaKggXiBS0x%2FuNE7gYRDr9nzp2mzHNcARipHPxG9wYGLnAFIbeXJqt3%2F2c4YFSpwmF7kgJfQXQijeqBsU5wTBTHqX9am0psjhx9FUk8apAiOadtg%2Fm3ehHoEtlLsmzIRr5erzzc0WZHwYbOPsK6OvCzzWPy%2F3egu9Os9ryQtoB2auXIJwA9q0faSPqncInPoNa5vDxm6kUDHLCm%2FcWYHub77UbFYkflgwBXwKKkdEnd6gKaY5jRzg%2B%2FPxg7ZX85neGMPPwtA0asAGDpveSESnwPIfwaxRSh8ukQgga%2FWlt%2FFxcR0pjoyOWthX2TYqT14xKEibP%2BXsJXCBSu7fX%2FOeqVOc73xIEWBfP3ou4E9KAEGqqMqd%2Fjukyz3e%2F4jh7s8a1xkT5HO7lWpdGmvg4d%2BFnXcl%2BSf2ddZD%2BO7pEVpclt5lpUhDdsS9nW5gYWP2SHGnHZpbms%2BGhAS2v8Ov9eHg%2FQttLzELHLJwxezhPV5vXgpoM1Jr43GMaqUtO%2Fl6CjDIs7%2FXBjqkARuyhwkuSDyjyQ34k6F4MYnwQJTQoPtp38mP8NfcBLFgQqm92pxwuBwbNelYxUrkeeE7%2FVydp2psb1bk6WNi7TxD6B%2F0Qpy1qo72zVAWhObzuXycdnLV%2B0PTcWpO10qKj8N7gKBJwMu1c%2FyvS7yGEGMXPGzMcW7FYP55ptuj6cUOLdTzbCawxyzDGjul2W78%2BWbyWQC8dB6oeFUBOPFdXKDTGV8j&X-Amz-Signature=8cdeaca20a5cada475ef98140550791b0c55a8de162b4fdf1c5ca4665ea4646d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYK6CXDP%2F20261108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261108T031853Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIC92XwyfLfj%2FVRAUOlH6rDFAP%2FK9hH8zliZLXRZqu9MAAiEAkUi%2BkYZOtdOoeWjnhSLbAg59zoG2M49%2BpRRXiWkc380q%2FwMIAxAAGgw2Mzc0MjMxODM4MDUiDEegF%2F4fa7p8lGsYuircAz3IdwThf5pI3rO1ZiiGtXS0gVqV7EeobntKBKO9QK5OMnezOxShoYPVGYMYvJrntk1NCa%2FdDnaB5nEJdkPYanAhIEeZFx3KcUvqXN%2BYhkm37K2PoW8Bg6iHov1gOKB211U9kQN8bXsYfLUXyCtMDQJho6HvLcdUKNT02oUY8bWW4h%2BeWcaovLelFshz5cR2zdpEFu3qktgEz4v0HHRI5r5s49gu33Laj8VbbsWM9n9KQuwQXcaF%2FABSdvRMHegjtYlN9QKuIlvl7It5ZKsjuutBtjtrHqSjiomfn9nkykBoOX5VH65CqvLVYxV53OlWVL0%2FDt%2B5mZAF840sRExuAaS8F5gXYtjbCPPxSOSqReDgeDKsZFrJpkX2BffxhVWCFa%2FeBQLwy4OeTl4qv6uhMk3ZkJnjfrihmDRtyID5fGjCEiiMCRPS1Z2%2Fvr2KGYHUc2f13kVn5m1gQhk6adZd0Gcocb1lwGTYddjvgF08%2FHqrvVwL%2Ft%2Bafd5Pa5ZO2wmvDFhSccra2WmHkPyx%2B7UyvmMVS2KH3aGhCQY4B%2By49AQC0JxYyhA30zagqoa5xs0wkac%2BqrcgUj%2B000aBHjvkpnmYYo3X0%2BJdWamHqq66XGr0XzktwtE7m7X258DkMN6zv9cGOqUBvlZ0OdQNL97FglXs8czsyFnSdbJKzWI9mhcrbWLazsEPWpStfIt1bdg4BRyUNeQcV4zpaZjhmDpyO730haYk4TPstRckxIdM8cCp%2B1bk33tLbxUIOCVKco0dfbh5FpZaE30W59SYN2IeuJGZ04LfguZFJyuHlqwItP2vARqtl%2FyIxGwJEh5cXF6%2B38Dejz3GJTZeQYM4o%2Bk21SlPyh2HGcCcpNMW&X-Amz-Signature=34070a02b058953df91e5579132e2e084c5a7cbc42283abad7cf06b21bf9f690&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466752LERHT%2F20261108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261108T031854Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJGMEQCHw8xOnaRQ%2B1mfY6QjV%2BJWf1ekpwjLwZ01mtMHxUkL44CIQDDgSwP0PNxPT4x4SDxNBDkZsbRXc29wVgNHZB0dPhPqSr%2FAwgCEAAaDDYzNzQyMzE4MzgwNSIMBHp68ciA6Pbplg5EKtwDMbDkTLuec6z7EDS%2BVjZnWFe5G5veh7H2ZvWJapAZZf8xHYD0ZLdEK6j4Pa1tZxg9ipB4xfa1gwg9E%2B1LvLs0cjFv5WVAySvn43jwQhILItJqKmBl8Hxfs%2FJ1x5aTvuS%2FKc76gk0OSa6YuFdDT9VaMXAwOE2Ug8V6bvcy9Gw6Gwu5ePK0fzDq33sKDzSqXBH0ZyUAi53%2BbVi55LPHcWnEMhXgvfy1StabqcHp%2Bmo%2FMw0fjTTTgeNIvX%2FHK12cxGhr1WCeuv5kx3CYXmp90gK7Z1Qj5VVmYMz5jfMeLI2PGLxO%2FMBSB8Dp2PpHOPixxyZk2OhZXD%2FZP%2BGBom8NufmT7H7qHhp7hlOBUjbAt%2BPB0HoSqxSOCiXKBSclI2uWaP5scGlO%2Fb9KzUGp9xQRaR0f8VLZFzVM52DjixNTMXLB2774pV5MuKMyWxAnezIWIVlahYM4pY1ocTRPSTV9DXt8ul%2F0XLHE06cZsepVoQJGstKqyifYoe6x8EHtoRomcj9MOkKIPQbuLdRknbJhgM4N5hvkakyN2GLblukmDcsIT2efDqzLPcGiSma3jKYz7t8oTUHoMPAJ760AiifHKTgCIwIXOdZtWbvm1NurJXMgYegtbaK5fpNzwVxKrJIw1LO%2F1wY6pgGUZi9H7V4dstvTaRs1TZ4GulMjApxRyVZtXxrfemaT3mK7MHGMn%2For811mp7AJ7XG98wWvIGBvkHvxQBu9QJAw7YF2xii1sUqvAmAuC%2B8JWIDgLw13NV3xx%2FgUwbf7PYxBvL%2Fgt4eYaIPjq9CQhrT8%2BJO02LcTo%2FmPK6MpRD%2BdRa1eKM3csVqU8leuhLJmLxlfCUHrh7m3g0d0at8XdAonp5GWSmUK&X-Amz-Signature=9950a01d284dda02ba3bfa6753edd7cf7986e74cfe98b4b4e93515f9421f0397&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
