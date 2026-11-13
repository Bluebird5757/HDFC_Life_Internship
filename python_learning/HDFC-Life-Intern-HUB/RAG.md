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
fetched_at: '2026-11-13T04:03:40.870Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SIBYLJQN%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040337Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDI6XKeTsgveH8WWNG1CLSMgBIWC3YBt%2FmIN%2FDiiQyRyQIgF1zDigysAiywJa6QwozwhO7Y9KthdCoskgfPohZQzlIq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDAf1OYgsYi9lscZORircA%2B35iW5vlKMeIJkk1mhOp%2BRZYLqCti7UuuFQyPPyMKdSkhAqVZJyoxPm9IvqQTzINJnukeFd1WJZKdRp6MOYLyVpvA8az9v%2BYuutDddjityaGikbBLrUAJ9NXoGEE%2FslNrpkb5828N1fdPMOzoCFxYxRv6tCt6y7QcosuiJmuftLWAkrgZoRbEMFNn4dK9%2F83%2BJJhjfRNCtO3TI2BNnmBtORKu7Tu0qsGVqcRjNxg0GOzooTLpsWTu4HlkI9hxudutEpXxxhW5QCWW7qOrFLFFxjwW1yXAmO%2BIS2jQZ0HVOmOmaOFwTUB8tHJrONSQjTkaDesylEvfsCHqO3dfrATpilVxCyqH1KsvOmG6BDAAHmwLY58F0FuO3%2F40dBwZjIUWfIstATQLNalS0vWxNEKI53gSa8tGLotjWKczgDoeNlwx3vdp%2FPxP73pJCEJ2MxTh%2B17NXLYsL0uPERkLhJYSVBrAB3xlIe1FSRR35UwYzlIOPzSmRF7MQ%2FYJtt5iuqHke9yGRitW%2BMkhddvW9nqyE4hkcYpGWJJCtPlUwrFsD0iLFiD%2BkTE3gqpYFN7gKdsxTFWxHGuLgcsbKBayGioIsVD7b3YFEuBw%2FnCPlNPRl%2BeliDdPVsxauQNLmBMJzZ2dcGOqUBzhU5WSMtks8dK6dxvzxKS%2BN5%2BNsjA6MbC1Nlk4Hr64vykzc%2FQnjJqJ%2BsAUagAtigV3DTHmc65hIC%2BMI1uesmjyoJyNZ8qU%2BSiwmKqSjdurP2FIFYpLB7XK%2FQoX3%2BJEvt2EgAaV%2Flw1MGi0YbSN9bR%2FFlLtGroY77qMjZPITnW3jpkfd8qGgfYZ65vEWc5RuI8i2Klt1ipqEkwJOZs8CCRQ3LBnPo&X-Amz-Signature=8197756ac3278cba6b0335a6d4e6105fd1cafd7f2fd5f036290e7131f8511c5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBAWAF32%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040336Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCxdDY2MXBvx909mC6WWxXhitsHUuuHl0FAZ%2FcMZhTY%2BgIgfpRLczF4FcXnRvD7F0wQTkJ6ervHxTb%2Bao60hmndu7Yq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDIlvAf1rn3tUwECKuyrcAwCjmOJ9MCI0Weos1i89howsFuf5A4xcQVysB4wWYyGQ8zJxCFOpJw3Hn6l%2F5iHcf1b2E2FX3RMA5qp%2FFwEp3J0apuPCgp66bPoxepD%2FTw%2FQPUsXWnm%2BsDELcm%2BUsScUC14wjcAIGKq6tJiIszUXYRTFFYjN4rV3KZkGWrHGbf2ZeCtVkEwn5gxw4AQRSqzUpW3szGmOCsweAwB%2BzyaWqFX7qgE0X6Zov%2F0CdnutSyazHnaBZ3gPEy%2FSxbu7HjEqevU36dbBdnNFt8elco%2FtRWC90iM1GznJbS9BLGxHqiLYWWoc%2FDgJvxHnAHiZo5oWr1QXNFaQIubesf832O%2F9B7ii5WiauswSXGt5sQJN2l8vZoYa2717nFow6wkDngZCwY3PACsQGUxX%2F1O%2F1RyLWKGBN6apBsjr7FkZxUm17sO%2FIPZETWBzX6bMnlOuxjVKHmaWHD92d%2F73KjpSpZdq%2FT5o7X30E9kDF%2Fgn%2B53MxuLNLQYA00RERZ2SYfI%2BtT35SCxx6zbhaUfhYifGMw0QCLWDltCBi92o%2BV01BIsp0W7jDSa95l99JQrQNbvTVBB%2B0B2PlxqsmyGgwGCXg%2FMiRSr6cuSUyVb4mmvjEu0UXoKEqzhKg5bczqFGoWqeMPvZ2dcGOqUB%2BdYFUhz5oI5XrLtdpdVQj4OALlzR1XDqx9cqvSERcCYeUWo%2BJdZD0HI4HxbCDbVHTsk43TIKjUh%2BkSzn9Ghb2mKhXIeLZ4EQ9Yy3R8cth2RlUwSbTJ54eNnU3w5iG7hBcmFY9dcFvSfYtID0AAXxWPia%2FsPl%2FEiL9fl7OuIctsNUl1zS8eYo2khi6F%2BOCLByvCWHH1sCQICZGjf0RYTOO1P1Ka6%2F&X-Amz-Signature=06e88b6101fcc7d94a37c72a3054f44117f6bbe69f227fa829ea3b1293712250&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZWWZSXX%2F20261113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261113T040337Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGmQgCDmHVk%2Bq7vcSO0KmN5wEAnAu2yX4iORgGvWJVydAiAXTiU5sfzOguyyvAEpVvZ%2B5gaagYACGLk%2FiFEZmWUz4Sr%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMqzaWAerHXUpVXXV9KtwDH9CEAwuJ8r%2BOg2PW1lY%2F4%2FN8wjM9Un%2FhmEOht7To2l3vJ1Iwpsl%2Bu%2FVR7GM6TKy1qXCkhjJ%2F0G4eSEgAAMulSl2Lhp8icqYFRuGp19eIjRuVhQDbSxeo%2BrYwmI37yE2s9QhMTeeIM7zosdQSjEHTlEmuvnDQjfYu%2BNSxttU7eKeJ9SevfWPcTPGKxYdwDhkbaZo7pAGy6PtvSZJRpj%2BZRr4Yh%2FUVTHSnfTyw12nL5NUie%2FewjU0%2FXVOxw0sl%2BdNFdY%2FvEY9otKm17t8N2XKFFVNNc4xMdXsk92SEZIidhcCMW%2B2WhssiFnyJsntbt2UtFrVk9wa1h7DqW8BjxCGXnr0i9q%2BQbtnTOtr3fAhx9QePzV6ouCdIBWSIOdmpRBlycI54u9Csy%2BOIlS%2FJ4fEAMJfoJC0e%2FEW6Xj82G3MKGP%2Bn85rofhWhHNT3LIVb1LxCW%2FFNb5CEZT17R9GifzW1xhLtXd1QDBd2PAPoKnaR44SobYIQ2CmaqybKoqykRg12QGAKsnRRK1HhqOHL0n4uGLoEC2C%2FAuBpe9CgSWvpB5nrz3ejAbIQYHD9IgwrqMlL746dej3u%2F2fB0aIxcsNDuZbYJPvqDmxrYlVDwBI50zv7UEhfGzsDQUNOCW0wndnZ1wY6pgGd906I49gvN9Jb6BhIfmBMiJWfqoB6p4%2B8Dpo0Q3ccPgwHCfz6EozkAhrOY94tr0jrF1unMPBrjWdm7OD%2BGv6SNMcS36vli1kWsNd%2Fh%2Bf3Flruw8C3H45LAPchYrFxM8cDzSPOD8VjdxodvKg0SAADmzdOFRSnqXKxjDUWzV8aYwFaspNnOvLWeHOUpbUZmWUFa1eJ8%2FcNkH35EJk8zD2NC0bkwUuc&X-Amz-Signature=ae1480348a94a37dbc642dca6e1b8d596b79a9a24ac1d5d0fd5f8e9d92964771&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
