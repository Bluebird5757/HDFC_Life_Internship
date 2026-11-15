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
fetched_at: '2026-11-15T03:46:24.802Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TPN3HNT4%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034620Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCmbOY5HG2%2FC93mKtBLqKCZ0z0GfJYPSuSLnORfy93uqgIgPkNFfm%2FhFzt9dyKtRkCCnCrMKov%2BJFb6vzIjreMUn6AqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDoLTfitaBVdp4Xb%2FyrcA80frOG9szwUWZL9Qir9EE4P03IQ3aXI2IsvTXmTbpsLw3ocasFMeCMezZxGwskpa95tXHgXncaXL1PS2J4OGEQhJthaVrSKvuYFH6%2BNc%2BIVJVjik9r6ShKgq%2F%2FuGxRTvHR1%2BMmejg7fbWP2OxGd7QwQh348Pj%2B2cQ5A2hYF6pwvla2HnzXz03vHHuDua8Wjm4BEW0xKNDxPfVMl007OxpOTkYEXbbkfLitoA%2B2RogIxRlR43FJxwpdjsQ2%2BSY2KvLMnyri3bGofdEhjm0Vkud25CA1aa1w1tPaV1nPc%2FvioC9KMLW4rU6PRn40IcOh36bnlyMI0BTtYhIStlXH7dVEZ%2BchYIbxNGHZ8XttLKIcHLAj1nhAjqSoRmesX0q8330Vh5Q07NaariQSdnXeamCLSMxSuMJ6thc4P2LPLSyf6oeTCz6T5WsT1L29PI0mhQyeCg9OVw3%2FkIOjEPGT3nCBcJQwEdyAJcp7GjUOR5EBjSQTu94gQmQGzIPWFPpqaNSvZTCP0WoFdsmxDIgsnUDYYitS5VJ4Nm9B02cq3QfdZpeWGB3DH%2BhEOfxKzLDTgxLO90IuaSB%2FSvWFHguQ%2Fr1fONOMBzvW9rq%2B0tEGdgLFbCtTEdhACl3Nlslr1MJrb49cGOqUBUgJbqKenR1bQkGwCdt%2FT2CVlcH%2FQosy8AFl1xlB2tJeJR4CwBxefFzPSWZ1vj78oUShsGwod%2FrzcUjyORDKkgfArXWsdRg3Vf5owvW46Rk3ZT6OlNmooUTVtSmQv4i5XoKMrixdPD3dKdt%2BWkhfkRzYDl6oh4BhqL5sdW55OpIhfoG%2FbK9N9zT0UHglBMTQwy2jsyTgURZ1iPbI%2BkI1Tqu2wi0gw&X-Amz-Signature=abf09666e32f9674563fe1cc8c703ea22ba30b3e41a356bf055b5717393f798b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKHC4DYK%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034620Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE3%2F0NqHHb1dSnDcJnLFDDFqhUH0kuLdxgQxJ1sncvktAiEA2pZ4GjM9jhb2Ey7jmz%2B2QXbfRhYQEFTvSBEM3UNugYgqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFz3%2Bz6OtDq7xavJxyrcAyLyFOxzkfslR7iKSgDGEvw00WgtAxF6hU58SGwIFOxkzGOQzhL5RBAz7WP%2F7HGfXKHRQ7dvRboX8RMbwzVbc%2BYuwEPREdWK5TiC02peETMXwBG1UdSWwJdoaZ4y%2FRxLO4Gjsbs4nOD07RvIKeh1unJU%2FNpO0Ok6T1ReQ5VR%2FueQSWjoQzgujpIjo18jVlPrLcf8KgazuNEQzBpejgOaEeEQdNIgFawAF9bcsq4Gocg1xSlkvyA8KBhnsTEmQ97UX%2Fiooo3XLUNXBUTp1W9iRlsQtVOGXwVykB2%2FVQPpHrfScyPpl3mE%2FTa4yeAejDeFk0qIYLssTYnjLG9sMPXEayM%2BBvUDkFOlwZP%2B5NqJxVMkoVCEFCueqpzAt5l%2BHi%2FH5ybE7cQH4pG2I2Dt3RyHt4Xm2OHp1z5PQ04njl2YaDx0zz00V4kf338UItSepC4SL%2FUodTHw9Tct0M7Si7aOlcpWMA%2BcfAJogQ9dhDHLGvlNDkKJiGTk6aWEYN%2BYqCx8YPPME8LfS82TMpMOfD4hqyAY0J9iiB%2F5QDn7zh%2F9olIvpAgca9R%2BcVP7iu2Vk8Y1U6hh3qjGTKz7QE15MPK9peunRa82RBHXEO3N%2FJGS9dasykq%2FjL%2FnhUYczJX2MOjZ49cGOqUBVWV4%2FL03ZaYDmmqvQI8l%2BMPkb6GoEd%2BpqOutztlQ1%2BBxF%2Fjs9JqxrpUpSrs2fZF%2FATUbcmM1VBvNrHpJ5q0RSrMwps2VS64m8eL6Brw1P4of0tE3%2BwFWA1d7O%2BdXQs8ZsP0fDgoXpn5eVBo%2BDUIgbOWoDkTeLvQPjoL5c5h0SW3zkpoVIWZGIModXn33PtPonIa2B9P3dnNqIibZlRJ%2Bf2FzCKwt&X-Amz-Signature=a8dd9429c4b14f967e35dec2c858708c35711059e71f0262dcc86fac878c3274&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664JI45EFK%2F20261115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261115T034621Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD2YBPVoTbOXPOaGWkIHgGG3X2ALro6bvus88gFCYcpyQIhAMouqQZ27SPu3VnOA8WfAV6XIoHuEPt8tXVUaAj5klBrKogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyJqmBsmN2rlrLQjvoq3ANA68tbxRPio5%2FyQGHHFdjoUlH%2F6ns8EItPWOnmugsS0Gz%2Flt7R7VMM%2FoutRrb8BGH%2BYIFwEaqQU7QD2WJwTPrjY4H1lVuZQBb8KG4Sabze6QiP5OTeTsaEWa7yKkoAjANMuQDCIjoynAke0Jka8G%2BEWz0%2F7rgpb6EMqNVaaY2Lyy6VuE8cq4TGHshp25ZQWMGak%2FzxhtCBLoOjtwymogvjtRdb0TmbjOKSob0dgq1Ivm%2F%2BKu6TEh9LV1fO1K4D9UQtbpSpLHhpVR3WEQxseruaO3Lsrg7h66Ca%2BN%2Bs2sv94LcEcFpPMEmRVWMPAMLlAiCIkpZ2O%2BJfDM9ce3dOomUvfVSM%2BdxwE1GoQjohqs8JZv3uux7W9I6nC7KsY7caB87%2FMiAm7KDScFUXaCWYVdBzk7V1NvrwcP%2B572zjvGMxegL0zboLJJoZtn83W3B9RjnigocVEwEXTob1V8pF4GR%2BT07p47Fmc5a0AuU1wczuINOMoNU8Yq8fUDiKo9ABNSGPYB0bKdXv9VdL7EzgGMoFX%2F6SNtjZl%2FOdYFb9BXeXSYPL80mK0UKNS5Pry4Ha1OQWrXiMGDjqwxXqb%2Bnc5%2BrgX91LdN1BUK2BioHy1HqzyZYAFSzGrmV58f7wxDCx2%2BPXBjqkATnf7XP5I8CJKCAvpBHsvU8ASBkY%2BhsjVhQY3I%2Fhct6u000wJvXuGLw13hVn%2BAjKhfXr8Rfygcs9XkCk9HehrWcOo8hQYcxGx9sYhzTyxQZQHKM708pXjtFqcMQT6AkRIgfmud63u1nDOUfidvWr%2BICE2BoJT5wKsnpZm56xX1f1qoFnvFEkzBGiugjCR04jTjz7kzR2YIWIGx5Jm7Jol3e4OuSQ&X-Amz-Signature=23b48c0238887adbd0e3b3a55e78c2f3cc9bc9dc8ccd6dbb02229bb294ff583d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
