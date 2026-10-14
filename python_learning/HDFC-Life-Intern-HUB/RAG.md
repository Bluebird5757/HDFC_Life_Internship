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
fetched_at: '2026-10-14T02:40:02.859Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EY6BPRI%2F20261014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261014T023959Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGEMuWVmWeYoy32S8GByw7ZFJZkRJk%2FUvcb1pdv%2Fwaj8AiEAxzKIoRINFf43QjD5w3YLyvqCCF%2F1W43yX3VHDoi4VbUqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLP0FYSCFS%2BYeWT%2BKircA4CO4eQwYFrQ45bpM0k34WkkF37woeP1trGvRQv7v%2F91VGOcbjCPc9N25Qjqnqn%2BvM1A0Klnc30hjK8Y%2FHowoVM26%2FYkAusjkeO%2FnzDXS3ybbXCdVhCLtxe3fPC%2BaPW79yWV2pIK3lUuydRWIBSQ34Qy%2FgAltpIKScWmZ5xO5QijdN%2BkkDGok4tzF4cau0MVQ%2FynJYjgB6%2FjUQ2eRa6%2FhuXIFuzpXwtJCT82J6dUDy1VxuMk7pLnE%2FUvJhumv%2F5p%2B4fEC%2FFMNUft1zaFZkJwKZMSKmFr5ffOgvLXOEtgCAOCKiVnrMYYxOi2OhU%2FMssvlYNNt%2BJKn1CJAAq42lxL0mk4Vt9oEYWX5oEsaWOCP3h6i2nLwWMxrW8pH35xaZmaR5xVNze81i9NRAbs8ZsffLb4%2B0MVUAbcipHaTRqVq6ZVj2y%2F%2FUmh45p8Z9i4vEQMNBZc5yAGFL7l9Bip9tRzJ6S4F3XNGrA6FrZyI3A5NXgAsQGGrOcJE8WIHDjhbkYXIM2SrGt8kJTHabRRHuB82H5DyqYgPANKLDLljvEbMeguIGdGoLgsqirQGhWaC%2Bt3T0yWUWeqC530Ze3erWiKjGMB7QACf6eIc%2BtUsoTAjvMNSc5qqoG4vp0EqGawMO%2Bzu9YGOqUBV0Dk1RisephG06CH%2Bf2WYkuPw33kk6DIab3cdWzgEEkE6LUEHksBWKtYM1uL754mkka4R7LSwtVHO2RyYOvWT7seYul5A%2Ba3JZrUqJwLPn8wSI2fHDQC%2BdqHiz4vVMleBEF66GfxuAkkXYz7c7e%2FvOFNeILWabEbl%2FHWps1OhfAp34OUEhDaV0LdEUNS0Vs4HZjxVRJ%2F%2FsZgl5MvHSxaJjde0mSs&X-Amz-Signature=494758c132a830a604eb6215dbc00a3c1eabd3bbdc51ec54bf781ba590c40370&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJGD6WXE%2F20261014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261014T023959Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvcoPZ0uOgiHmnn7YyaHsrzqN5kn28xluZhEzl8DHHPAIgBiwTKXiMC7WxxKm8os4zsTlplITav3NfnGmVaMEZQ%2F0qiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJRs34xXJuw%2FOpeH%2BCrcA6hEGeEndiiqreJX%2FvLsW3sfR6RFZzgui0Pz9jYtV%2BeW8gVFuJfMlm87aMgxJodjKz9WNKjTUSTzZC2Mq5oBV1x%2FyTw0%2BVBg9gM9naIoFkQM9GomtYASAM49%2B7iAXBTovw7Pc9HgRQAKqiM7KSB6WJPkh%2B2ZpNwCsSsWP5kbJna%2FlpNR%2FnAuUynj%2FzhMWIs7dTSrjswl%2F86L9dTS7xWYDCAZSeV7D7g5u%2BEp8m4PeG0Zv2A7G4n55mImZNL%2BwvL1G8Y%2BbcCvU1TfBjp6JhouA%2Fj978Ty5NsCeqVh1T%2BspiIi1yEEcYvTKfAettBW%2BShsGAwi92T4hX%2Bl6a0QqbcSf%2FSSJPZae1nK8O4LibbvRm03hTQZnQa9%2FgPbNJD0O849SpGXqAF1SE07BRrPvnF6QO6fBJp65Xk807%2BEDlZ9lT%2B0uB40uq42WWyowqZBX7UlUUev7cRCI3z%2FmulV1yrFZaql%2F9ZnEPdCr%2FKDIVAoocbiTK8zt6n%2BU1kcO0uxoLWekWNE9rm%2Biw7xaFtRIAlcPdoNKP62S2qyUWrNYFaiCe%2BfddbFIVqgS4TR37iVnUL%2BnDqgcU5uFBDfmKmoKHRJxYvZyWZdahkuY0%2FB6HYhZfwcPdBo2O4vUBjm49bNMPOzu9YGOqUBii13yQWyBhZcAbC6%2BZhS6vq73t09WE1qCKXPY13nHNUymw7FGXT5%2FkCZjmrFdgRDaN2gTtSQU8oaWuZ%2FJd5ovKrqqCQ4oGdSdf3VovDNVQJHiSmKvrn6viV%2Fg0oHGnrkEVOv5uQdM5eTV%2FkF5D0COcQPlfyE%2F92JS1T%2FnvhRsweSl9wfRYzciVihilJCqQhg8KetNyMIz22CC9NP5m63EMBxlTzp&X-Amz-Signature=d8e02392ebd5de633594a61dc2a77c7c366c9046b30a7121fd9c2622a5e98f2f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QOO5KVVD%2F20261014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261014T024000Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCtwLrPbssb%2BI6vf692WCgyG8%2BVCLdhTnvnU%2FAw7oKKGwIgIXKxLNQ5BsB6sqpb22Khq0qrEZx5a6NmF%2B%2FBV3eertUqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI1wK1D96d1EXsytGircA7hsLRHuUMF69UKTQa5j2A49%2FtZtus3gTecDGYnMapJQJz9DMT%2Fo5gUymQNKkFGKYbWg4JO0pAhZdLXpyHnTaMr1BefwBVS2fCk4iIAHpAIkDVVxbk4W3vYrlTQfQihKUMTGseQVowGJEGxDD1O0PtzxHXiRSsZ%2FysWX9tAXq1kGTNlJf1xDZ1Ail8yGQ1Y%2BHl7WZ%2F9fYWrJdYtO%2BFwaRabeUNm3xfeVQggykUPtLbtfC%2FQoxbg%2F1ten6H45brs1iLN4BZBVbgeLDEIKUjfEk72MRLJGqhNF9A%2BArN8aDwsYN3nH8r3Ya5MIEW7pGPKu4wmJFYoyh7fgxhaVFi7nxhPfAiXNILOKPb4C3rtcISpzXill9wCyTsLEqCsFGzDc%2FbiSvobrHYeOA3zZc38XsjHXa1aH6bjOFP5EmuVCeqtZ1jbVy2zzA7A0X3aLy%2B1aIK5iKgeTDUn%2B%2BimEdtHiF98qJU8muNDdyY2Oxo1A2%2F7YTcIlR6CgqlR79DEThtPPya4VG%2Bs6Yi0Optp2upf4uF%2B9ku7SvT3qG3%2F4g3FAYBWNR9Q00DAXdLky9Hdreg5htWC1Fn2xxiY%2FZJ0oToxumbK9MDGtFSUg0TMXdPo5lNrbQFlNteGg5fm4hS%2BeMO%2B1u9YGOqUBrLviJZzVGX5bSEE3xNqp96xRQ6Sa7V5Zyus8cojZZt1zKQQOZTaj7ZXp92hPrGPZP9sCE%2Bklws8qx%2BVp5nR0HOKwwvB8RAFG73MCy%2Bm%2BKtX91jiSzSON%2BLgnjgPCZb1AnzoqtmIi5wFAOEgczqK2rV7G5ouJVqLDRp73jxNZjDjj9Ud3uMseTXAqQ8V4yVNIXCchFABhCIOd6mbN%2FS08SFjGuzRr&X-Amz-Signature=511e99ce75d44b00ed640c08263161b219e4f6d1ca61cf7043a1fb5a1b7887a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
