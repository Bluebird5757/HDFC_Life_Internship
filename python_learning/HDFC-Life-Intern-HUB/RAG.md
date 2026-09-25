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
fetched_at: '2026-09-25T02:28:07.392Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666QL5B57L%2F20260925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260925T022803Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIBgeJB7cyKgmEqusg4i%2BLgHey2A3B6%2FKwynXfz8pVmT7AiEAwTG9SDU1UsA6tSNiRzjbfo33ncVpnHduNfRhzHBIjgYqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF3IbH8aQCAJBSH55SrcA%2F06wJJITodkrr8kGD8JSQCfXd70j1YlU9acGJ5n4UpchqZ2%2BPLi4IyIpTCNIrrrokFRpOuxjyOy90J8TZpjCPUuSxL7XRg5cswKsP4PLDu%2Bi7yflj3N%2Fwe1I7GIzM0cVW5AYK3J%2Bqh7S7lvo6Puc231%2F9Ec1TXEcOl0hS8LDr09N8VOuVtVBiEJZXBhBy5FM7tZnX42pO1ijuURwfz6cWx6C0OgzbvpwFNfgIMWIC69S8STpM9%2BbtUOUWp8h8OE4KK%2FbDmRjtytVA656PY%2BjSybtrGAPciqkMmr%2FvpgpsvdRInZmvPTDAoSqEbW3V9j0kNchkrBcS7tiy4Ash3ItB8mA6Sr0N1WV88L2SOeWPU02g4LdCBKWJvwLijpip4yEt8NWPKleh7lxoScGc7CqubfhRIq4Rz8ujyU5QBgHFXqqexwRGJ3%2Fee%2BS0YOx%2Fb4V6%2FtNxQx6QSob1yqacl4r12KCwvlpsel1c9Expw7sI2KAJsKC%2BVtivks404m8By4DQ8wOLy%2FSpFxOcB0DwKCOEP8oQ3fWOol3WeZlTm1l%2FPg8AquS3aH1GMHPjuH854xGJFU98OVgRr2hvL2CiNl1hKRCx6DlpsF8%2BwWJurBwLx09iEacdtqGOtLAVo%2FMND71tUGOqUBiPHbqWUVciDmZaG57x3Rfgn2FLbA16AKsZNbh59j3F9WGPlhlMAkKp2zEtf9UmW%2BJC4g2zw9egt9jouCkxzJQAEUe7qKjFWTPNaCrjGikIyTtIRoK6qAqui2BQfRW852%2BFUxhKNGpDhyG2lw5tFGkwbYeArpHViH9LyIddweYkiO6sxhOU16PKeCHJP8bwLOnWmNJzaC5wmCjPc5kfnAm%2BIPlBV7&X-Amz-Signature=de91135e067d9536c3a84ab313e95538442a277d53beffbc0761537c218488bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466732G4JZI%2F20260925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260925T022802Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIHTTtQ62UVDn3%2BtnTFNGgqquLR9r230jv03EQILd2%2FIMAiEAhRvjyKouSh3H%2FiO0qnQVAjpovElzH9ipmo9oh5BcL2kqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPshR%2Fj0ds56AuMg%2BircA%2FR8ugqjcPlaOiBfPGS3RmNTwpvvSW9sCPu%2FRC8njiuE7E6%2B0JjS3%2FEqpXA0YSanH%2BfX0WSnEgtMcStreK2mA5q%2F0Z6NEsT2kZK0sqGRzsG%2BAPRenAPzjdyO0Fr4L9CUA2AtoL26q8Q27bqXkVFY1Fkn6Kp4V%2FWLU1IEG0Nvm%2F0stKo4w5jrkEMP49fYhr1bu2hiLKMWqS4AI6A0OS1gPtNKFuZWAZV1JFmhq0bkXCLWyWLtOmpW8bAW98qPjsQDljj2kti0fslzSOthQHUeObJTGdEnInwTVTHBEW9a7RHgh1v5bVehwCv7h%2FwxgHfetnEk9H9xi0dJxEKC9b4YuApktO4MIwfs2d0BOutHaZXzMbJrJiBbmOFZGGBbqXvXvTmSCDq7T%2FrRkErEuTUsLMH%2FZuMWcOgLIl4al%2BNnnbmpvN1tGfGxJ7wIpV1BthdthRzrleVdg7qXm3eX3qfKbRurcclkx1tpsbbwaCRzksh5TNkzuZyzEMm77IqzKL4yYvQdJ08lNQN990tZQKuYqu6R3Be8XOugWuzSrD%2BVVID5jyETC9VikS%2FCCSQUA9GlFVG0BnazVcVXtjGp5VtMtO30WfzdkDNuTJuYoISWH3U8ODx9BQMDZULRINkJMK371tUGOqUBma9Nn9Mj6Zq4R3nfSuPhDEEKd67sv%2B19VcCqp8VVe4UZn1LSAaFX1lhWiCol1B2HDkaVqXqDLHCvC1z51QHlB%2FMVO78s4og0UgxzZqJ%2BAjUWScByGEyaWlfgRezmH4cwPoOqUZILVyBNvDhhZYuXHs1BO%2FVdU98HyFd1Tt1r%2B9Vg7pCcJ5DkCK5U%2FzwGNyt9A9qyi7I08wFkEVZuIAaKUcx2jyIB&X-Amz-Signature=93d84d2e0c42345eaa7e3415ff69972755b4690ba1a4cf3ee04f230cd92d268e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QGTYBAFU%2F20260925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260925T022804Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJIMEYCIQDrsX2u%2BF%2FxGUpLCySbqVApgn3I0RnjrL0iLafY9GpCPwIhAL1tRW%2FV5ADnf%2FgFjS2FNzu5HjnRdMuRFc5AYx7H1pAJKogECOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyhR7Ig%2Bh67g2yDi6Eq3AOABjIwIizQL6PTkK5f4uwHbPfcWcRVjkp5l6g3isTSS8H5fJcFno6BnfdI1tXU6pyTVlJZwBt%2BEpYG6ew7n0ygB11cZLLIiPR%2Ft6FpwNUeIGoM0xTEw7EqelbowYrb7c%2FtsGosVilUdg06cbSr9jlg1ITgM3Cj7DNrlvL%2FOfS7lWMRY6K4aG2UvTRI8TALfazY%2BwrPgI%2FnM8RIWMEn%2F%2Bnk6wA6lOAsm%2BDsRlCvJXPPqxvqJ3sm3LiLBKqV3p%2FAYnbdTw38PKejhC9gXneB3jk1Rbkp%2FSPj1GPqY%2FBlx08ImtS%2F%2FUMtdeUS3WqyVDnoFUcvPpKAln5ipxPnRj%2FK0PDHVoGex9LvTp5Ya0XImOR1klUrHJRPQ2R2JIhQqUFA4OFFrH3mBg9tnQfKNO039nMprSIuWyo2hEtQFZk4%2BzP8QSX%2FS3qEas5aGtR176Xg08Z2tSVwh1gUdEx8aWcvnOaM4SBpbKnmlDqC7daZyJOaVp0h3mx7Cz0cX6CdGfOQOqTcUDC5AWcbYUTkWn2BOL9aY9p7qiEmcSgVWyzb3Xl0ETkslALw78MGKI8FSiJ1tnpeJAEX9Ntr0w9r3YpWhKP%2BMP5VOkGRMSg6w3V%2BVGOhcgojp7GIcKWIhu919jCt%2BdbVBjqkAYxlE6FQGtT6SvXlERZof7ojfG1C4bdLf4PiuHiX%2B1bTzXhM4230x4lMVQGyShbs9FNjhqch0ZhiMfPWDwSaBJ%2BiwhNLhaCZuYKQiMQq3T9G2YTP9%2Bg8RLMwJJBRFqTCPnkvtqhnCis7OUo08LPLFyzdJ2ObgFwAZvKyk47YGjXS4KAhklrJXzMMTpQUgAu%2Bey7yD316D%2FZfaMAmMuRsH%2BbBYKsS&X-Amz-Signature=7429faa7e0e12f8a5eb0290cc6df9b79d8470a93d72933f1e27f1bd1dd8b721b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
