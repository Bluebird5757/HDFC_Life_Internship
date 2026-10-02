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
fetched_at: '2026-10-02T03:06:17.796Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46623TVEZKE%2F20261002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261002T030613Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH6hoMKoSUOV4UtOdmmG9%2F1kyLK5VoqVa7IHlfhwKTWKAiANVSMxSMS5gVLwradrlVUq2Mcf%2FcXWHop61ApIcj4t4CqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhKicIhykpQ75izWmKtwDKwT7Oof8LkaIDtJst9pHbkMmeKE%2FHmy7hlLfatgOjmL7WRd2vviSgKB%2FSYLMsVMKL19xD%2FMCB0Hdr48ZvW3E1VqlYOR%2F7a7SwBxxBpAUg8HwQb%2BwkJAO1LCGn6cjDal0vKb9EvapDH7a34sWtJJZmTi631AMivb9xuN2H9F7Z09s4F7v%2Bd1NUL1gcmzg%2FNeywIvxUEHAplh%2FgSVmBeBTsCHf%2FPQuUX9v%2BeDLX2mHxHyC%2F69tc4wpEqlxmGD%2FeY0j93NeMuh5%2BHj5t%2BIMEEUq%2BgKq2TdgC48cZczZ6oYuISEu0GVI3bFjS7kre%2BMGJlSAevSK%2Fw77D3NGeLWvX63aV24E6U8WkuZJKcLnZVzqRGkvZfnstAuhR%2F8Y1%2BW4meso1eSQktX8zqKdXXiTtZ3oDNgDVAr19PNn5XtugfAdrFh6GYSQWpCRSAWJPLPucLY4cpQZqjVXpDqKFWZRiQPYIvarU6Q1UUPPFVHyiSAQ8V0gNmEVOYRzd9S1uoodpV0F7OzdwkmqW4h%2Bz%2B1YVZqzQ2lJRRbtTZVeoSM5ulUzAqR4C27oUKR2DUMR7yorb6ELjKzNakoMRUj4WpODYX9h0v9ioquu%2BJeW6E5BOY2bUnJJv2yWwGtZ8ZO%2FRMEwzZT81QY6pgGVeu4CSiU%2FhDfxIQGAVhqGED%2BiVPLJLaCJXLXBXx1bKHDE4UU47t%2FUoyUE1%2B6zXWmG%2F3%2FmswnliS8TJvdJFYsDQsuX50%2BE%2BqZ1aaYrwdmqrizF%2FOGz%2BoIr0eLK%2FPfKfQFMN1IEgRpB7KNOGixLFRRZyhmtFFErcjSVmSxni0oiYX8efKMr9XDEnF4oJAQV09Wh9Zd6osfFLm9pl6p2OVr0wPAp7v%2Bj&X-Amz-Signature=4f4e349942f740520ff0c38d11b3e376066dbbaf371957b4ddc75d4acb000b07&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXB7CY6A%2F20261002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261002T030613Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDAZtUwCknArL3qXSrYsIzeArt6e6xx0FkqzX599keMLAIhALiiTawvIdXosQXZrlWurJug%2FkD%2FZfI0cYFaKVdqFZ8nKogECIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwRfhZQWH7gV0ffHisq3ANj1uum2oVIPLUGTQEOJw90z6AzllaO6E%2BCplMXV4%2BXQ%2Bmhxo8Kcej5uUrIh%2BaiF9ftoFdYgsAwAOlwKB0Hz2ZIdtFD65TcPelTsw4GIkr7eedNex4iqjV%2FKtBvQLm%2FawYZYf7Vwounj1ZW2ornCMXFgvYKFP1eoOBweYGc4JYW7J9Rs%2BpYur%2BGkOUhk8FOxV1ODu8GR2oP7xuAUyycoAo8BAxlPf0PDulUttXveYc%2FqR7vm6aC9DKqGMOhCUukz8CB6KT7FTtEqjDqCax3hZvKxnlGe%2Fvi2is30QiGOHhJT0YVBYc%2FLSgn141jvZDxEMo19NGr91eNdJE0OO4N3%2FV0XfdWXVfdhCI4hUVxHZkKBVWMU6OjLp3u9RNlZk3m29ClQpeP0Jzj0cC07IE6KfERBwYWvcizVCtoeYC7jYQz%2FrdfbO1AjbD745HmYkaqFJiHMDVSU1sC7ULPdkzFKn8Ubu6lSvnJluo5Mi1PB49T%2F3zwNKK2SyCLA5s2G%2Fq%2BnDZfd0NIr5j5B59XwzsxBTiDfyvQbgIB%2FzYBDllcOGZIrhxVbMyvN4Cbu71zsOqUtAdEFOFVdV9QAK50vOoCyDsxiHnjqUY2XeOrh1ChSd433t6u1hJjccCQxmi01jDslPzVBjqkAR2DPBeZ4G6flDdnmIC0AFRGR%2FcFNLYFRqURfPOgKjMzjHZN1mfzcUAOpEN%2Fm51rcUXwzff7q7pOz6svExjEpFteTYIdUTEq6QaqDPsq44ueYPxzUbdoOFpXx%2BdAMpHCEivi3tnxkwodwBQhK6QdsObbzlQDbM69kDHxlJ8kxJeEAC%2F8HmkRSb9kGBmNTwibXV75RjOKLKQ34fxD9qvBL0M7Yi10&X-Amz-Signature=8ce4d201c2df57a0b14b27111ae055696b4c4541ed102fdf188d268a31740b07&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SL6JEEAG%2F20261002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261002T030614Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHQBdyE80DBOebICNBfdQmLrYC7LI6qI%2Br%2Fra1LPHuZxAiEAu%2FJ%2Boer77DGqxGzX%2Fr5y0jmckVj8bF0EtNGoUcv4dtIqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCD6OecHyr8J4kFpLyrcAyX3LR0mlYTnvYXpZubRcSwZav%2Ftq6GJ7r%2BSXCvifZEZX2XGky3oNeIgP0Ta5v4%2BJytPryOrBCBlENlakGzDS72wmAFlEeOLYPqAzbGAxnUwYFyILU%2F9V5uC8l%2BJH75JcHU%2BqnmSr4uzlDmhQ19BUShWnYc5dy4DUHIvjtVWJqkVMX3%2F3ruFlZctOuO8gN78RSmJZc6%2BQAhB2EvKpJ6WxrMZ84ekXJzPYBlbrv2YsOr2yicRr1zMTIjPl%2BlxHCBGRmUZYqI%2F8vpfD84DUXJdiVJGZ%2Fu28QU8cLxxyxB%2BIBOyRzZWSxAHFS9HnL669Moh3zIUvWBwWVkAETvXmgMX2vrSLFgPVvs5VUR5%2F0tlFXblyaHjZV%2FROlsMzqn7ja8u1RHxZhDRhH6YJn0rnkzY9h4tvHlxl8eY784bD10WscqCw1R%2BkDLDM17tMyoRZ6Czx5cNWLoAOKgyMuTfvTvqqzA3X6hlnYktlTKebjyrH9apiQ8iWMqIcRz4u4k2N878FZG2PaI3nXJz6lhLR1p%2B34c2AKVr6wRv3IQ9WRyAWpMH1U1JYoOu2c9LM5SZ5m1gASalFC4EzYGTEsPyd%2F0r%2FI1chZl5jEbovQ02D%2FD856iLPs5MJ%2FVWWX2vTm0lMJiW%2FNUGOqUBkjQpFwHXZVn0o%2FLlTp9n4sfecuch4yWgq5FJYFhl%2FV7%2Fxzl6OckAninpqrtoeG%2FCt9DxqM%2Br2GuNCJ658NNzr1DjZLaWZufIPGLNn0wjK600Zyu9MtvaelgcNLTQLrsRJN1XY4ITQafO7M8%2BGI2ReIMAD2VEFxEl8DW9WNoGRBG3q6CQ%2B9h0%2FUdUm9MEIMctw6iFJzPH4knYYRkGpZv7yLnCXEVE&X-Amz-Signature=47c907e4d3adb06c241c6d42fbdc9c59d190072d0e8e0a033e9539396b32c475&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
