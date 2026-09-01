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
fetched_at: '2026-09-01T02:35:27.432Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665WO25K7D%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023521Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAW35L%2F2GXxcGMjZPDF%2BCf94zjsDj%2BHQBa2R4PheNjt1AiBSau0tDfjvzttG6pGc23cmbqdr%2FmoBduMJWLuW8Yb61iqIBAij%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FYG0FElcHud8F%2FxMKtwD%2Bw8ZIr%2FS6VS8vkXw5HeihY27vb3ZVbaeico%2BzvFFkacr48VdnZhoMJKdA44nxWpOfxn%2F8Kfa9YGTlEuRRly3%2Fv9%2FGkPFDcVevpVKZUJG5OnaLCSEYthVTHNbxQJ%2BiacOqINUNOyknkoHA0SFqJ1ZrMUKdANKbdtZOe5JMXRttBvR57LHH59LDZSYH9XwGWcs6iG8Jc2kTERTXv9oAQu6xQ%2By3yMJFvC2NICe51an2Gq2Ew%2FvK7YLctk7490VL5vcyLTU4h72Gq8qgugB3FPlehEG3g6vFluUKRJYiiEzTHa%2BuBNvK8oEJSsGGuYz0Jbuz3shUbgZ%2Bjkpl8eozq9fXPwZP10UZUHTfwHJc6wiWmbBgeNVMkrzvai8oZg%2BNcGx58ENP%2BTpuG6n3n6l%2FmjJPRFJ%2FQj7k%2Briq5ap4JV39VDeUJ4JzsJHGZDM6QbUb3EtbegD5hvUNYp%2BCs8IMQZgsWNdbIQwoQpLFoe9I2SHkV4ARmIveBFoPETgXh99brmF1OZ62BY1dN%2B6n9nJZipdPPO9wt2VRf4Uc7%2BTdoOLaAnrnlEURtceI2uQfdm6o3DS4GNf9WT21Jf4X7GnH5hAiZ%2Bxr6kAlVmLrELori7xxYAYBXTucuNkDDkxRaMwkd3Y1AY6pgEm9bkaYEcF%2BtFtE5By7HhM8pyTrb1xypypAji72hiZY0cQsCojkAU1r1iOTdoPRPybciCrCTVt7xoMbQk07luPXMSpwpPg4QdXK3%2F3Qly7qu4vXbgPoqOhpXXubP8iwpGEwxNcONcnhyyqk8mlA8juMgINdscyjb0k2NL5ht3JvYasTrKGqgPGR%2F%2Fr36MCbOUYnhH1JbDBj5CZX4FJMyxil8nYhsGw&X-Amz-Signature=3ea3c55dca356a90c3619d9cc0c18fcd598e318e0f882be81417c38d16a63d62&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662A7DJWI4%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023521Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFX8TIxTadClwvjx81AuoBgkFR8puSZ4h4a7sco6BSBsAiEA5kmX8Lz4PBBEKi4Q9iR5A4GIPHSJHW9Nj1lQQtgXtz0qiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDInRk%2FRl%2BlaxF7dUbyrcA8to%2BbLVRSCwd8vSglnHPsZSPO4YRbMDryNNiYUCGsCmCur1VLqcCZtOBn6lUvwp6gvfk%2B61rCwLVZM%2BrJ3SUocDmEnEl78F7jKTKkzImPKzSNojUXPXWfBZk5LMoSR%2BqHtcsgbM9YkNiINce4UlCa0SQ8GozLA5xRZVHpb62CftsYUv2v%2Bcs7Tjt7gto2lXDeaFO8bnijdZFcGdQYnlXF5%2BU27PvyUfHSBNcTKdZEow28ZGEBmcBxDPp5QiVoR3SIhq18OSzk37ZI9EfFti52kpB0JqhQsmpFzzCtKov30r%2F8Oo7c4dqRTG74Qf7GdL4GviyWyQomfCamuRdaSCE%2FonvCKT9Xr0AQFqUdjzDTKLrA4h%2Bys7ZAaccScpEu83os7BReQq93CjN6KRMhcp9azPA1c0xv1aIzqzcHS%2BVqkEtgPSLlaN9jJ7XajCr4xzBuK27eowAqFgaRyH8lk9W1LYWVbi5eZSM0Gr2YS0OWjhG7D42d%2FR%2FCguhsnc9lKGvBT8pPpjvebPVgGhEaN7USbEYM6t00YP9oIMaldKsrbIjTV%2Bu0DM6%2B%2FU8hHrJVYbgngRkLzVKM9E2JFPTCCYqVQVDaFdANaaH1YKXXR3dyGWigLD55KVtWZMRJKaMOzb2NQGOqUBUqZt22ZlLRWewuKZDq%2B40YgnXL8jui7sVw38awqL32jvJwq9R6RpEJNWMmrXxqzwALgng2trng%2BKmqsEUcdzKFXLkkJzfv%2Bb5Jqh7cPkgvNZ3laIQECueWqTqvK6A4kq1VHBzqWI%2BQu8WlpLp982419C7b2QDrGm0dOc0JjB2WvlzJlKduOfgk1hzse3ChoFkXRr9pfCeAllwEJFJ73EIwXivh17&X-Amz-Signature=b8bb9ff43449156cb5ea3c6aeb06978a9be61751f2497d0f8e7042ddf3c3c79a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TUXU6LNE%2F20260901%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260901T023522Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3alybAnnHoy5MiP4c1ixhjkycUY94dTISPhNQXcsJQwIgCBrizEn2TIdDKDnv8%2BvpJuCyu8A%2FpjqBUqcl4maBk%2FEqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDODetNtzFHhAju0tdircA59uTH9pomUqch5SWDNmj4aq2lhQEp0qrN2nPFTTsErij7YlS3ktOM%2FoYfRtK2qBx1kZDx1x0kK4zcTtInipuOb02dF75sBUsfNGO1fAK2ubsa4TqyjwPrKV9JIzJbZ7fQ5CBzmeKeHGS0OQWPyBRzD2WaQEtRg0j%2FDEmL4nduF%2FEDNnzcU%2B2oX4iVz5aMMWgf%2BcOLRrp%2BNDDNXVpzVE3xDwuGX%2BzNHfab108mhlTyhPkCeFQPF13R%2BYiosc0GphOVdM2Bpk5boId%2BJ7%2B6XK7JfKEN8IUaxvo9yVYxkzEfer48d%2BWdk8RNmTle%2FlNdERZMJUfdFAOxo3LJ1rJCpCNDpIu8TJBTFHv%2B6Im8%2BURaHdleSRgFYLJTXFbN3jTe1Mj1OwAdCEtr%2BCvdo1Fz198Dm9O3jU517RR%2FwOzj6bg7TPLz%2FmuH%2FRUJlYNQ%2FlrbH88BchgIjzyeA2xnvIj5BESuYTSi%2BOEDkvi5OCnZZSBwK4mD2%2BggEx2b37BGB8yQGaMyigpTOS5%2BwUmYNYvOVpFHHh1MSlqxJh3GRntoepY4t%2Bq6T%2FiMcJJ%2BCZ7fU5dTn2sw1hCRSeDTCFrwaRILxKbutatEFOmlzvfDdR09ewu5J0udFjOoFj965CC3XWMNHc2NQGOqUB4NiZHh%2Bqj7ijGzTrv1KD19HNmvo%2Fz8uZwTx7kgg%2B1yoU0DeJXo95xv8Pu%2BnQaLqlz4VbrPYA5JjfPSg5JS1rS6e4lNzZTzHTbPdks4RaKGAsKW3x5JfVbGRVxMlbNMRKXoMXpYzGSByfs4gv%2B8l7DOMr8tsU4Za1A7toAq7WRu30VLZHvcHgPin%2FGAuYEADPQ5gBj9HnSMfFX8jWKWq7KDlvIpwI&X-Amz-Signature=efe5f3c995f9be507df5fc159fa6d5f7efda86c37c5b035b16dd9b406c976054&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
