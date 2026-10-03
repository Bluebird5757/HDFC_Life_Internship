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
fetched_at: '2026-10-03T02:52:53.145Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665E42RSFR%2F20261003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261003T025248Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDkT09oSZDakPwuSEV8Bpl4V2ekLFLpyE7Td4dcM2z66gIhAOevKMUBJN%2BxZPRxKgrMtf%2FBjv7xShOvej9BDT3NsC5wKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxSrLswUoOuq6PDw1sq3ANkDQGFwUSG4Sezp3IfK4i9Vq4FhptuOz%2FE4%2FKFnKsDMOEJC0%2Fvd7MlyfBnuIVq5Z5FvLIMz9r9dahQyZjC4aUBhnuayfmOeRmv%2BCdJzLb34mHYJXg%2FcqPBegPxh2hMm9aiP5uWlZVcp38CXC7w2nphLBNcjtgKfdNuEeA%2Bh20sC2O%2BKnidcLrry1wcWpVijGi2Ii3ud1Yb9YYCRUZAgXaeP%2BfMp0ZI78UzheUmcuZ%2BGURh%2FjpMo8WxGdNgSx1pq4FmwwwagMJc6Toc7hlssXOI5THDsBiAT4rY%2FvS%2FBbT2Xzs3ieWMcW76rBImnFYvGdLiyd4qcKF%2FmCUfqSefq9rRrrLm5FnAtNGfFqQ1%2FNQRbPTPL5FmM7Cote6QTvZeHSI5Y%2FiHCiwbaGL4c3lBQcUYbvB%2BCKFALX%2FQ4eZSsg8IqIUfKmcjaDXZTos%2FXRGYfWEruwhwog0nwV4ZRGNHec2qicDsEDqPb8EycTNDiuz2YSZ%2BpRRkmnkF1cl%2B7lXXpLiBNkif1jP%2FY7U9yav53WE6nv3cAE5trEQqTUuvalU4YqRCZXu8iHzqkrBwFjeXNEF66KVWzRJBXlVYcxTMigsgGrO%2FfrFD5lc2thNYA1CHAM3GI88Yp4H5v6I1ATDLzYHWBjqkAXWXk6klwXRBVTnhBbrnQBwDRZD0w3ZWNL9%2B%2BRqOrtSQVPrL8apEF59nz3WqqNGBFaxq%2FADVRk7O4bzziY4Xr8%2BQSrZpKnx6RSu2p8fKM8ElDkXU2M6IDb5mvyg4AmtlqVw%2FwJDoVmcRhrANmKJsxKgkULmk%2F87lki%2B0CLvTWLUpgcTHni0On81uKUnE%2Fk2l5ic%2B4X8EQoYGHSDJBZE6Yx13gOXz&X-Amz-Signature=430ddf68b83a6e1834561fd23bd8ee3b7b8d2d8653964c3adbc7aa82b7bb5f94&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VK2EHI72%2F20261003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261003T025247Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDjDWDXnQ41Rp8VCP%2FjK5kovV2xL%2Bi1Cxcxmc5c%2F4zeHAIgVGmer1dbHiMa0mnM%2B9iQShpee6DZr1cejAoJQXJ5sPIqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJDo95Ydw5S6TZJIaircA%2Bwlf8cnsUuqka0WrFnzjRLlEu9l2YtWwsUmelYT8Zq61oFlE32eQaSBDKT7q%2B2PCC6%2BfA85o%2FwQWxJX04Eh3jYbGOya1gm6jzm3snlukr6qhdnxBhvHV1%2BXDAHuD1%2FOt5qsrRM5DtVxsqKOqXsiV3LR78o2N%2FeW5wYwq6jV03njbZ5A79o%2Fbx8UVBhq%2BtbJ%2BlpGbL6XwaW7THLwGTXeyyyHsCydu99GwVXSw%2F9wA7NT41BnCL2KxhP%2FtDcQGjeIv%2B0KszkaPjkMIhXehHRArjTXgSkFRsgwlQDkavDq8YW5cYCSc1e99aOGEBmWr%2FhPO%2BaMI1pLf7qMBszmlmT%2F6gNGlF%2FQyNdeZHesN3JuFrJUbZfG7AnngyvCmU%2BuGYIoPU8%2Bxd%2BTPlJnNd%2Fr6ZIS98u%2Bpnp5D1JfOuV7bJKktcCMr0NjgPDMi37VzHUD1GM7fdm9AZ5xdKZTNDYLPXN375UGwO%2FzDgTh%2F%2BSdvI5goKd%2FB8NJQxa5UQ3ZpjRNRuTaONIoZk9Nujrm9RLcK%2BKtzMor3goNjVjHzXRoYLftUyHn2r1meoOS%2F4J0NqIJiNDFqo0YVD8CueY3Aj52ojPBLCwTOYkkeulFtHM%2BrJI018FCEHqZmobNyTQS0jYlMNfPgdYGOqUBI6QqmKv3fpJ7YrM01Ahbu0cK7h%2FF9aIp0ifFGADAFA131JPHS%2F0xtJLUEjsWm1rd9%2BoP%2Bl9dmRBZJfcXcRVHxONF9Ov053giWtgmKfYIeZxLs11fA%2B9vZJ3AKg4BZFHl1%2FTTziklpxj%2F8XENxrmu1IzD1nQYah7ptsSL7VfvwmNRYef%2Bu7yFvhjXKJg1fSIdvebZxP60pq05ALFhFNZIiZORQ0C6&X-Amz-Signature=cddc7a0641eb8766d8aac5a16965662b7839a748f0e38b242f0bd01d6c86ec38&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMKHSCOL%2F20261003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261003T025249Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFejXR82sR1%2F8Kj6J1B5sKLPaMRDzGruG4k%2BamDvtmUgAiEA4HSpwyS%2B76OO66dMZC4iTasumI90MlcppNLdNBmeD5sqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK%2F7bgRN9yukzGtvJyrcAwQxTmrVjEZTd4YSCgBGZiIfIoGVcOeKnhpJzWvhAKL7FY9or7lWEtWjgVdnSmstmucM1yCPL5N%2B1%2FNCmXXwZl2rAauyDtxqwN%2BfYxtzsTjGlS7etDg3zucBcYxA60YUd%2BUoByzofmJQEt4EJojjdyy6M8v%2FbMmdPhAqreBhlVUdkMdxml1Ias6DGnB%2Fn1KPA7RV91SWrZp%2FYOr8h80miFgbBM0W87npe2Y3fSBpiypmusnSYt8GFpTZM0FezfAV8Dj7aBdGej%2Fabc9JyIp%2BUMqG6cQe26YvPx%2BP6a0cIBC6H7N9BuUayGEc0gaYeGw3Zkwyu9K95eBj7bi3VVjDs0eABEuYT37zRWun%2FWOojPwcgA2VagqD5hydWJ3n64F6zyWpoBQjeyzoFLkZcrgMEub5LNlIezIIp1pgqGhERl8K9GIklIrK4d3x8r2zOKc%2F6cAHIzaDUppx5d7mujEVhKGrsP5kOEJj9UQno0ZlIJYCqkH%2FvssMcD7N7kJ1n%2FhugV2IQ9dRkCoI2%2FyC74dum5oCKvHR7opqKKL0ZH%2B%2FhBVZ7Sq6pmxlHhdmJfZnpg%2B5mvRVKf%2FiOBODFWYj8QgY5BK3tirgduBKsLP3zE2yx3P6MOTpQhD6VJxJwL8mMMvNgdYGOqUBqeHJGO7CMm%2BMe281D0rzIzL%2FuRM6HS%2Bu0J12GlCwnBcniu1eGu140vPfw5BGOkLQRMkCBy1%2BvOoF6ecpjJFzKmZ8vNYT1pmeocfgt29qeeYCE42fGHGqIhLh0C0DaRXWC6gLOXl10iJmQl2YqkSaajsT6%2BfJ7kb%2FWhyqw8xtlTbJyVseSL2v4OdeCTJMuATS0kEfVEy48dF4PAcC9smx1DotCeRd&X-Amz-Signature=ae9d9acf393b1bb4ae4c80df1751e1bd52102d2293aedde69bfdc26778cdb9a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
