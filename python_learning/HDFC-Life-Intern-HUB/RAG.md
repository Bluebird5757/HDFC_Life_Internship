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
fetched_at: '2026-10-08T03:32:00.056Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WESKIVW6%2F20261008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261008T033156Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFIaCXVzLXdlc3QtMiJHMEUCIDivHd55pu3xVbr%2FmQdHyIZa41jBSLdy4U3kEupCSIZHAiEA9thxZiGxZ5PnTv7mBHXusU978xYdkCY9suCH6hXopSYq%2FwMIGxAAGgw2Mzc0MjMxODM4MDUiDDteYKW5y11ERoM3pyrcA%2B%2BOP3WwL1KagvRuiW6GnPFVKh1F3VCED36P6%2F4HSHeQ1HdMykYIIdHMGPFqFRiJCs%2BuV0HyltfX0Dw6HQ4nEtrUXP7AvbcCTdHMWv4Z25QB76BHSLs5sWaJA%2BQyhlocUhDgB3Bi1YAqnen5I1H9aqXyWAc07gkZcLHTq%2BitEGEr%2FbDwI%2FoVaRc17FbPGMP00W7%2BF18DaryPh21eWMwrJxNcbb9wzJIj%2Fgcop32tIITHY1qT0i43NfxdX%2BOri0q1eoeVWjQ7hXt9v9Sxr6pePB8aMDvnXk0vioYas66QB7NgWToQU3PHXFswsm3jCY9uzbv35kcU%2BTiEMO9n8hB6N5%2FzdAhW%2BRDPUWisk2OGglE45gojKV5l40SKCZWHljI6sPNduT7b7CmG0J4JNzU4L5fKI6uuD36nAHYkfmC3J%2F%2Bljg3C8WSN0DHhhpkqEJ0OVoDnibXnHTj8TWdxsJGoaPHf3Z513xJRLKjnuTv87TZ0EOplN0XfieKmHd7zc2fMS3%2FM%2BAzE%2B%2FHrTG3jW1ZK8Ley%2Fp8v8GcPQPk5%2BZeI9tQpwj7j97sjuuBOiJwSTn9cXg0mQrwDOczJ1BauK2tV0uNGLK7CS%2BkLO8IGqWlqHy77JmlLKi%2FKnaln%2BD3HMIDtm9YGOqUBJYq%2ByP3UGhqoV0xs2oGoTjsIRkZPAnjet4PM5YtuAeAaKpSZvAvSbU8yjRLoX1Tk1Z0Jjy0a3AzoX0LOKfaUhRyo%2FskdgMAPRTsF6ecdsH9tOaTnGT53Bjv9MzelykwrOv0n2T%2BzwcDNZX3tiu1RBoA8CKzuXACx7cIK2bvCaUUZ50S%2BCu%2BktPpFDAEG6uVXR6omwo8qNP%2FHpD8Cv4IapDPOWydf&X-Amz-Signature=30b4d3baae42ccd5df428d0452bb9ba70de1e88d471043d72e4c7ea425933cfc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHQTJTOX%2F20261008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261008T033156Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFIaCXVzLXdlc3QtMiJHMEUCIQDhyQhVkoOBtOLVJFtlEJ4iF%2B1xzlubZ7dWFB3%2BBvQmwgIgfFLGS6hJZL0Fjpi6i8K5%2B%2BZ39WwqoRxnbNHcheyQnC4q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDPgD7ZS%2BxSX2YuQyOSrcA5oDC12I4qU6xHV%2BF8jCX7pviA70WMJgdsQqG0QL2D0zKf7qTzGuuyvykB%2BxKU5bwr%2FOamExHel2GuX7RLTzvzAq8wOsq9GcZpwZvV8PwwOnLULIEUQdr4gxMkK4jPykiXhyUN1KbFk22cNNunE8uXKIOnC45k5%2Bj3E5CumA177pXRKDfw1qEdl3JQ4BJlbwh5kajahPcsXea5dalAr%2F7HrkeJqc4148CezwIUcas74I3lw1WQ6Mi%2FhyRTeMwXRIBmqVOAz80YjmMTH4Ld3E40QbbepC1EEmKwKUOyBW5Lwo9nKt4jCkOxYRLYSMB9QsW9EGkujGiu8DQD0zliYb3rpLRMIfh3dtRe2QIM86mZslvf4Jul9cgjBIoy4wp5adgkngi3HbtgCPB6B%2BbTFy35Rw8OelpsLcDSSRvswOa%2FHJUBqCV23JFeZyhCzzAuYYteChTiOpIjuTOkgT9CIFHLG97TA14aVxNf18VtizVitUOUQcZwSV3bSqJKfEuVKvvcvPmo4SuBQQZkS%2Fx9veV33lbD5yyN1%2BqImgH%2FuFL8t9Su9zN8jHwyp3IjDvBKouIifbS94wEZZqdjW3FqY4OfXwW%2FqoMvDtsaj%2FrBhOQAbsS3RResa4lkUOSfKqMPvsm9YGOqUBI38gM514V9ayU3Yezkb%2Bfpr5vYh1YtIH8DZseu2tsyK6LRaYjMuR1O3eJFgZ%2B1erxgZNmAvx7DmjNh6yn1fUHz7l%2FEZ%2BTnLIa7q%2FUzJpd80AJXgXZbtW8%2BkV8%2FJ7n8z44ZxzG1jjzDXqlGttDBppy5JertlyNereujlzcv8W66i0Jf0R%2F89xbnlgD9oRY3RMfHj1IcyNwk%2Fe17HLMerF%2FjJuPyeS&X-Amz-Signature=bf0523190446fd5a7650e7bcccd9257c288ecb4180e78e9a9a4da604f8203723&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXFE2R5S%2F20261008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261008T033157Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFIaCXVzLXdlc3QtMiJHMEUCIAPtPalIbARaaIcujWxHGZBAnj1Bk8l9BPiHygR9htdwAiEAjylm5wZOELPlst7AUB8OrW5RuFk%2FoUbV4WMH5zXdxTQq%2FwMIGxAAGgw2Mzc0MjMxODM4MDUiDNwhSiEAjYqyua8EdCrcA5Uy5QYRa28mdkTnvl%2BPT6RlhlXJCug8qYibxwobgeiOZ79vOk5UZjcLNHoOsaTS7%2FeQ%2FFG3jx8HZW7ql3GbL2HEZ1H9NiWwcXnah3g3wYwU6OHCTfRLAqKp%2FGQNcriSJi3PDzgxvmF30kykIzzq0J2ldJx7oZMtADWYJ7q6OXC5fy%2BqfGet2cgY03DqV0heQ0DuJo3%2FOGGlgooC6Nh4V3YA4LBLauO6VrO%2BqrcEnkvL6gMnoG9tdiauGbRJLuh1gtmf7wJkPFpgBXiktSg4TC%2FXagD%2F6GrD67dqUkeDKVP3EI6O%2Bpy2VMAXPWQr6171cXweUwfmTz7CdxVVQOwG5igrg5qFjY6aZxK2XUWC1om4mtLFYTHAFqf%2BI7nhC%2Baql4HQlYpfB1dM0kS2Wi%2FlwM2jHtAzZ6wNAFM53BshaUNODh94czefzmw8w1R%2BHXgF9bI7gSF3bEGKpIJyEBJZPfRh%2F9CrbtHPIQGP3zPNilEuSgPPjoD9YMwa4fJ0%2F0m5MN7%2FdBHXubEiy6bk5HO6ViSF3jcMSMQOmgRvT%2BgDH7n%2BEb6h2G81Rub6EmbBMX95MpSgWqnuonq8hf1Yhr5Dlw1ZrcB76JFn0MwfLI1Mm1QhyJHrelSvv91voGUZMKbvm9YGOqUBDbTycmKePe%2BUUUoPJjEgXCVsW88CofaUcUtxGwIKhJMSw4BXd2HbuuFa0FHObMwl%2BGkG9%2F3H%2FBOw1Q%2B79UqqkrU%2F1qcegygT5mL2NpSg2P%2Fw4xNoOs9SixiolE%2F%2BbQo0kzfGvM5R2g%2ButsClBxMTqNGUPKjn56qOa7Q3Fkx5U7YXMzPvucKMEeHOepGOX5xCn7H7mhG186RKS9TdsIEPTbKv1MyH&X-Amz-Signature=4f944611d77eae913567496dd1369ec9eee1ceb708548efc5d52b6aa52626d8b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
