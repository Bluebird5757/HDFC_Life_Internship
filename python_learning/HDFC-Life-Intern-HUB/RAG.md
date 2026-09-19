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
fetched_at: '2026-09-19T02:12:20.042Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667VSMHSM3%2F20260919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260919T021214Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDLLxFDVYbFN%2BStcUYCis5BsUsH1%2F7bSRyPE3q6%2FazvRAIhAPhTolzLPhhYSjACeGC4UZUjYpCwe0fhxxxcy6M66txqKv8DCFIQABoMNjM3NDIzMTgzODA1IgyUIhRYeRUHR4WY8g4q3AODAf8tZSgsH8XE%2BWmn3Ou7goXXAJ2lrbmE4FcqUt92EMQbEH3Vxt66fsSrf2RrWpCJJa3nO0HEFmtvJm0cJIbXyuxqbLmestO6u%2BiuZZmy8e0u8QGMt4dHLKd5lWJLol3zNZLmHVW3PU9ikaBGQT%2Bnx64awNq3x4Do9HGG0XDkQ4JVpsXts7AvTSUzSHzI%2BXMU%2B2O58Fu2eKfKIhxlliCkzCFCg%2BjYcCzAbEv96X5rGqscbsj5gFBR0FzBScnCWa8rUeRhEkk6JpFYNfeDxyG05uroRxyfjChEwMePZLaYudrednQ9TVZVPpkFsWjnJlMuGyrZDsCS4cP74%2FYlw8%2Fmpg4pjzJsvUYVF2hLkZC2XWHJQcvGs7ODtjev3nOsTBNYr9j6JHRzRP%2BUE5R%2BUplzjxWqTdPabo%2BCG%2BeD1nGnv%2BvPrDcKQG%2Ffdbx1Lnl%2BlWwG%2BuglENRGYqJywzUrHRqmE4jVmzWk7d6qDs%2Fb%2BsLyuKa%2Fcl3Y9OY6brtyYj858Z1nOlVYSL9AfdYuC50yc6wWgSd0wrtyQjdih6SCN3d%2FM6kgKrJEz7nNIvck5IHwgaPDTiHfwD9JEvkuqDSNNJagA8fDl4Xb6u2tNcycRtsH0blkG2ApVhpWjggKGDCPrrfVBjqkAVAZMaDlMXDIaFLtU5eyx6uZ880mk9cRpLAD%2BNu3A7pccsx8hcQMxRlHX5gsOELrGKRJU96DZhFhhMJCQYfKP3DZxUftMZAoL7m9qkyfpmjjozJfWPE3Cfh6ZlCGjoao7Pd3O5mI0e1Zp1PsKLWFyG05QWM9XAFhwgpCH1vtt%2FaJIVaQX5Mxf2SNcjRutSGT6sfHs31vgf4UvDWMzSxnnkI32VZw&X-Amz-Signature=defe54b54917bb7c3e28d5dd7d098edee5e4919ea89d620cb956f3d7d2c26498&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WRCV4HZU%2F20260919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260919T021214Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCKZzjuANCEC6pcI677qD7MAS458hOx1imXXXkW6FytTgIhAI8HQcyTRJgAz8yQukivj0T3fdI%2FUyUq8iSDzFwJhCifKv8DCFIQABoMNjM3NDIzMTgzODA1IgzcBnFJkUoCRK1wkf8q3AMbRubSrhyZwD2AaLl3X9aqxhfm7Sma3bMTTlm0zJ4D8ACxiCkShKbB89vuONdzBA6u2RSd2jOSxmwt4pBgss6YPs42dgZ1WdZoeL4on4Z%2FiE7wwcYqYXtQWgYFnBogDlIYdrkpqQtKnkS1Jx8Z4oc%2FQK8LCgZfXnrh5fQhUUTbea2KNhokyOfs5quq2n07JGhPZUasHOXZbL2972VPaonrmf29oux0XQYGPI7lNHvkWOGNbGO3P9j2hgF%2F6YwgpZ8Y9WJDgOS%2Fo%2FkmaHHauN%2BI078cdAiR%2FSxpSKCc27ciQifW8nbpp1GyxHf5FwaqfvjD2prhtqN39JsydfwVKvVp7lcQUb5w37CeHhRmc4hpFXusqBvcM9gafdw3aDW6GaLePUBmDUTtRIaFK3NxEjLmBAh8IM1VEoDfs8EgdA%2F4ccRUH%2F3F1gyP%2BUEAcSeOAvmTVcRWhH7kAoIkV235w9rFWyc41JP1y43Ba1cKn6O6n04%2FIaTS%2B8xltKNU8lGlSD8Hjs2GH5VSbGIfYIl5e%2BA1KNvb2En%2F1zPF5LfMYmmJyGFaLj%2FJDUwhjKtJ%2FUrwMMT%2BE5LjhHBM0qzwcbiysCR%2FiE71CqlB2BHcFBw7aPPlTSfbW97U%2BuxQozgUOjCYsLfVBjqkAanPqhJWw98AYLIJKNtdroGpMB74h5ZMCpJutUw5ZT6Mv3Gkqp%2BATNtT5BDBm7%2F3aVxrRufgu41kuoZXp5NnrW7Rueazo00WLPeATFe4jFlHie1ME3dNnpVTGqgIcqlRi82pT0lB9Hh8GqWr3mtiXY4Q%2FGSY9lZV9IO6p6dmyt1H%2BxqSNPAPA03DWTCbzagO7d1Ya5p4TxRW7TlePdy4HIWhcEzH&X-Amz-Signature=29e6e97ce01b70e4c0b25264bf24b5efd189742ee558b67e39cf88bec127d612&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QR5CSOKP%2F20260919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260919T021215Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCcQQ5KbkpbPCZos%2Bo2fSNCjeY1T9xjM4wHh8qYe2ApWAIhAIsDCB1gz3MlxIUVQ4d8UcPU3%2Fj9FOIvnN%2F5Xzx4b94iKv8DCFIQABoMNjM3NDIzMTgzODA1IgwioffX1JIOKOAuF%2Foq3APsK4PKDbvwE0H4TjYWIMuhTYvxpluCwtOhhm7lOBoOzS3hitSGJxwbGuuEn%2BwyG1PKLKlhUjKwPUkOLueYlhnpvXoGym4H3WWm7Jc0ivm0FXxeIsCw%2FcqF3FkjyDcTg%2FmxowlxAG7pUq52sZHZd24Wqzux2USL4H5nTc3pv0q0CckSqxy4j0%2B0TyZRFs4w%2BxZe6R3vnXIICpjj6lnZGa0ntA9HbtNOYqH6aWX4CjgEISV86TSeVYx9psXeerfYeyP7LfMgYlwld3r4ZL%2BACLXWDAwaaijJWieRXbNYKC5%2FGJnzsLIXYIdMOmFqbkkma13VUbFl7eRYFDXYGhNgU43129Lq0rQ1i4wLDlMH5QDR8qaFT9HViZJFtGdxuAHL79Zm7lZ%2B1njht8jWy%2BRkmGETbCPIn0lo9g9i7pzKBhLmaaPyxcbQkk%2Fd1woUcgCyWCgILVwPVnqJ8GanrvjKkWYTVGlaT3FMS6uKPgZOgPZMNiQuUF40HBxxwohs7s774NJJWXRmqK5G%2B5lkWeLspyZmgOpw10%2BkXe%2B3jiiOwFhdjWHyUPv5PQHvtBg%2BDNKyxSUwGW72LFRvvm35KoklGcgnJVGuslNBdW7Ed3LaEaduLZ6akzbgiGtb9BBvJDCRsLfVBjqkAfV3H18MV8Pdu8RZO2mfWhklJLcv3BjkVJUn7UIekTGHEhW4xkO5No7p7FZhJLs4JO4a%2BDIX0BNbECCmxKRJZ2nIzlLWWOsfZQ110vyS%2B3OpLuyYXUx4AZnHfpulPc%2BnsbxzFQr749%2BctfF%2BhIss2hsEZo5rb7xdXac8zCilFigXs2ECqiK4B4i01wk%2BWB86yXr9inXPjmjHTH%2BQDhqXXf4L4bw%2F&X-Amz-Signature=60a80f16283d3941f8936330bbe268b73733c2569affde066bbf4ba5e1bfcc9e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
