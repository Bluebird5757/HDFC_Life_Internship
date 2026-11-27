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
fetched_at: '2026-11-27T05:49:32.311Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BO2YSOG%2F20261127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261127T054927Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCIBPFVCmg2y17zhM%2BSNGeh6Z8ONpxGXS8ljZeeHKKPnL4AiAHtT5HEixAjWwo2ElXeJIWjQTo8zqbVpBofnRB5bPU8CqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWBxTaoETP4eCPDrkKtwDCKA31okXLBPz7z2Zo8G9BufNSKEnnDf43iqHGo42KdrMwi9TFfhzJRSP8y%2B%2FfreWYBEOkjjaELfkoySS3AUPwPcvzvTfqiJ%2FHmcYaxmMyWFwB6rSzoJeREuBJXStJcmnOj4q6Id%2F8XKrCM5CoGU%2FtNbWY%2B8FK4rcnP6mJIf2CSXdm2%2FhpFf0YzF7n2bynAk4ZEFZJIVR3DTGYmFgHN2Lo6HDGNCvRjy3EAtdok6yNvWnOO9CARGaAhbwjEvDKn6wXxzNq%2Fj9MNjo%2FYuT9P5Gwow6FHXruSItHuFmeVCoxML0ZUiAQaY89IByRTA86hX2rB38eSQ85khb67TX0hQfz92W0WTZJVlwqaSS7AF9M4pJDrAYEDIUMh6PpDVWbduvBv%2F6edqZ9LHasKC9HPgHDK9nBQcGQ8EfYkzajdnUlum6vMxuEdAxSYtSTBdP9wcwsFuaV8gy7aIzpOlhH9Cb8i96CPkg7pEQkdrAwW1squt81ZssB6crC4rMzFO5zpUkrfcehxb393C31MGRExGADA9v33WfD50WgFuHHWkvAUmCeXa7InLbJh0KyHTBA3fiNvZEdFpM4lWQ%2FNGgYeNydlgZCuD%2BA%2F9%2BhUYy1xhT6zEBWLfN5BnKilSsQ54w2bmj2AY6pgG25XKnyusQaG5evxGLAIyDc%2B7Tf0sxgjXceGYJNt%2FWwd9Yv9fa0sdjOcnajkWbcldGw4XL42B2RsOTxlX0GUZDpmg2G%2BQh2B1NJLKw2ft2n4mLC8kwVxt2wjJ35pKG6Ykai%2FOzf4OYXFLHGeNEu3ZM6WVwvpTDNU6l%2F6p3MD9hDsPIuII9vJfvUiXksyYku2mAVq3jHa%2BVFeoO9vQ5no07sWaR7XGS&X-Amz-Signature=4c23a4859eb74f1ac478322ab5d75997d67a0982ee480c48cb7165b91d37eb6e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNQYOOWP%2F20261127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261127T054926Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIGwPVCCzoKAzcHDWbuYccyTlknY3ai8B3yKBJcQxlZZzAiEAoBInSAW03o0zvNns2C2MpsRHYX7Y%2FMabUptj7kgrylAqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPYxqHJ90Pnmr97j2CrcA7Mlk3qtqPmkXRE5qvEVru43C%2Ff7MxlwOnQTrHsFopQOIfcOzQQDuLodPfJGm1xYZFmouS3LHD8f6iSrIrZ2vcZAI3fEwstGMfYy3DmuTPbHNSpsdWsQjgtpsSqM%2BsVqMcnLVjICXZYnBRsQHch1e9gk9pVOYY1b31ryyyUHmxVyuw%2B4gRng%2F29bD7PNavPctk57AClTAakfWbWIPeWj2tp2DVXLd3JT1p3pELY2y01JzeDTntRsT5lhhPiqQBC2O2%2FvuVcGzO3J3xG879fmWSF2dztEC8%2BhsaSToHMfEjBnYbwYunFSjP1q%2Bkhz5fteXrRLzKFxBxAKOGtvLAvV%2BBKnvcZmxBI1jwdknITsDTd3ptA04uKsTNc3fXlqEu9iPsUJvoOURkHKxVHuYGmujdz2VmXDBhCXeSK8X374cNarVCvhBcBv9ZhKi1LIKzYs6ri2RR%2BBtsds8q7Z%2FXeCcwBVA9XzFfveiOrCPsELo16jBgGqXwS9j8pQUm871CtkAor3UlvmwQDRAiX59BF9RAIz037A%2BnHXUn1AKBEMeeDJLWY49mfxaZeRPxIfyziQPvboRrh1ozUd%2BqT06N4KOWHI5s45rc1FgF%2BLV9JEXs01UzuCYDviCiLK%2BXXgMMa3o9gGOqUB6u6VVACk5TKYkOpCOs1VOk%2Fo%2B7yKk42BESJT2bvlX%2B0p15ya7inooMN2eY7SKM8WaaOTEUqHmS8guPzIoYv97U8Dxxbjdc1ZpHwPk3hKEJa%2FkEJCDXJv6NltURhUKuB%2F8a6noIVbCA8p4r63XD0tqBBJ2oanpa9%2FNLVw6kepJZUvsHpTd4dZ6rkF817HthPV9r2DOi7HT6ztWCnB0bSKpuf7RllO&X-Amz-Signature=c945ef4a0164193848787be5245f3fd2e162438edb593e4b2c7aa13df6158d0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665AJ6O6MQ%2F20261127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261127T054928Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIEj5u8k7kXbHl77WYKzoJQnUwQU0VOF%2FuWpMcZkVZ%2BsHAiEAn%2FMxmQXfwfDalVulRKJZ5Q%2FNXwHIMSnwe3VgFstS3q8qiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDttO5%2BUs8iC794ZfCrcA6pMMA4v6a0MOWSHu9UmetbnokFJfI3KrmNqBXr%2F%2BVqmIQu0bBz3Efwus2iyQTU2mxC0tjA4sHPaCaVIC6vkXaF%2FnhnYZH0REAacJyBM%2BbMMaMVCEgCaf8zR%2BKn5ulMurm%2Bg2ZFnapLVJVbDkJuyO1jLZ0c8DJpt4%2Ff5HBEYY1LfuoBaokdDqdPC2yyx5EYCUmzSUUFQT6Ve4p8F2%2FKZUaf1dTjP3ujAP%2FgtUTd61hDgYFhcoIl%2FrL5yEA70pSLu584zmqrv93xZyovtL310URF6hoOunPXDJPP%2FatgehFL7AhKgIPEuH%2Frvki3X56dWchK6XslY5uy%2B7%2FWUkiRpEfjdr%2BiOiZZ7pwJlbIR2%2FJMXKilY3dvm9UhSuo57vck%2BnX5EBkE6t%2FxqVSjUlTpc2ULn89EDAcQRbmxFZwTS376D7EpAjk9BgiddGXh79E25CRWJpLBxfGwracjYCeXq77ZdX6Iqql0t2JVgEnrx6Ik6nhqyokdli8rLyDRtDKg%2Br13ix8lmFN8K%2Fe5uEG6KYgq0wa%2FVRV2tBQmEueTwFP2jzCNAByPGP3k%2BVM8pOI5gIp4g3YBprBgRiNPxDzcEmGbpMxDg4qEOGxtgWXHjK9UnuHKGxzEwhNRKtfJNMPK5o9gGOqUBjXVK52F56UZ3cmig8bo%2ByOcrLRhO5tqOV%2Fkw0vdloc%2FGSm3A0DvgEn16plV9%2B%2FoEP4Ws2TQWmPpqMg2WZlyC47xiYttb0lUnXtZopD2ASFzMXr%2BaX3T484KXIG27p2Pi46KYbpezvqZpVqozcqWNRQZns794tzIDB2a%2FvQVDGYhVKcNY6XByK5vSvTbFzFAPlvo9YDQBy0wSTvYIBnEU69tSMvHF&X-Amz-Signature=e87f29b57ca3a817f53a41ea7e74238cdb3fbfb4b038c07b93a9ec362b72e24d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
