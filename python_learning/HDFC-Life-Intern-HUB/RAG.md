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
fetched_at: '2026-09-29T03:15:09.652Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666HY6RKSH%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031506Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJGMEQCIBLtSGHMsP9Nowq7QWZVb3JxloKoVun1txg%2BJnl0MIWBAiAuf3iu6QoWtygGcPb1oiHHpH%2F4KeiSeIrdmTpkoEVsayr%2FAwhCEAAaDDYzNzQyMzE4MzgwNSIM7bUVweujRYKLsTUiKtwDKWhTPshtZwuxiPh%2BXdFMbFhMhmTMSzxeWDsL1mec86nEzm5atAL9Xo830%2Fb8%2BiGf%2B0KmF178erEKR05yr8%2BY7TlRms0QJSl3x8x1lt8f5WHF3mbbdFQ1IV5fgvmuPeHQXf9qk3Z3SNpsuBmxX4oWFIWaruzGtdGAE5iMMjQyk7jPs4BzubBxqhUCUa1obqiNNrmuPIWx%2FqO29QGjDoVWvrshK9TpT0FIujTa5hyIBIyyVtjFSV5AHnzs790oDVnRfe9EC0CBBq4PuSgDlZKXcWYrIO3Mv91u9fs13hVc90da7v1oK4ubr4%2BrBfcPlI3sO4mg4j4PcwLWU4PTbEO%2FjrcN0GKcak7jgJGspOZhTotM%2FI2Kq%2FfrNyLsdJ%2BnspHRZTgzD6SD0glxbJaB04nJiIV%2FuibkhWavTQ8NWxptM5yGumesb%2FocoZEReJiXPcuKvkNrKZn4jAtKBOKxJArL16q9uo%2BuLlL8%2Bu3K4apYbnI83RglGch%2BBLfEiflffrdc5xS33t16ja%2FxuR5Ubs3lafHAUDtCwp5KYEy7jXXujS6SZmictPyorykI0gy6FfU%2BWYVMQUsmAACR583zN93EqxMD9KXMK2IOTdYZnUcewTZPqr78uMtLypdX1BcwpJzs1QY6pgEHrRcTl%2Bdbi2JGo9syMHteCSM3Vrd6%2FqPlIbcsbVesiiSASCCVXetzZS6kOf%2BRxnytSHMiCPBR1sroB5OgIQR9xsti6ouCgl%2BbVELZvNPB6GVMe8Ac4t4yRITTIt%2FrdqK6o9ufY11OdDBlKpCGinPgeHccFWxZmBuNteGuiTzyO0cKeeQ3HhXqOers7t3VkdIzD0nnFUn3fwssEKdLG8XUFulh7KL4&X-Amz-Signature=8ea2e0ddbbd51fb0970f51beba954ad7c2031857ef60157e963b0ed7b1808484&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U34ZTGZU%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031505Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJHMEUCIDMLeiWVR1SaJSYU0HNJzOTp%2FhN0uqyb%2Fg3qClhKQtmYAiEA9HvSKL7qRZLERr0qupj%2Fm6lP843bPLsmEWOTITcdK1Aq%2FwMIQhAAGgw2Mzc0MjMxODM4MDUiDJaS7H%2FSTEq%2B5yQL0CrcA8L1M6PIniyg99TzOl3W8HfNjmtOFovxwr9WtGIC%2FvZFN5TphLJ%2BNzAu9lFfl05gn8YGrNHDl1iEA4j%2BtWDIZlQ1L5zmCPky7zAGE54FuFgnjO1qEnNxQ%2FKFiXA8Dt5TwF7TDFefBWwbXXGAQl3lRyf3QoywfQcd9Ro0Gu3K1GG60sBXrg1HnkUlbNLWgKb%2FwmlthRfWbThPk%2BB6Hq0tLVSKwDVLy3cttW%2BOtdScybhrTAbvV%2BzHoF%2BGw6pAdKUmppmcz7wKAFDlu0taktbrqp%2FdKpfw3DxEZ4R2yjpfdSG2ygne6INf2TD%2FyGWg1Zqf1xg5FTsQLyo%2F90eIMTIoLeT5J79hZWt6rkqyaYBTduev8LlVGGK%2F0ESoGs3FxiqHod1Wsw%2B2Pem4YnRY6Yma7stO%2F3zP5axtA7M0GMbC2zwVW51UyZxvdchnwFIyIV9ft8JN11h%2BxIuJht5ZSS1J9KZMM6bawyXhJFntoyeulNDskdnY6nDv%2BJxb5yp9z949I1QOrrKjQXcKiyXkgZBR1SBXKQk%2BABB%2FUwMuHUK4BVpF8gNZA5%2BVpUdRSQLljmf6zwCu2wS5ewNheyCCjJH%2BMoEKPLnZGyQDaBvNok1XUs9gWJH35z4B%2FYGSeehjMMqd7NUGOqUBU6Dk5HeKoINX%2BymlAf02dXGnzoEGmSlvurfW7AiBWup6blUFFfR8bIxgemv75bQFIHsONNb3TW6fAPNjpVjDDZMCvdTTSnkemZDmiiR7%2FlhuuH0YUMnQ9htpVHbvuaqJW9d9Dg9b5jGvWeu5PkYpqtGdRLi3VJFKDQiEKt%2FUvRWQ%2Burk1Cx%2FWftf1kBzTdZU%2BUmkA0BWG8WjPJIlJCW%2BUm0cjEde&X-Amz-Signature=fe6855059ba914774a14a4f3311fc270c04bdd3e1afcec9be466226079cac258&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z6WZGFWB%2F20260929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260929T031506Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJHMEUCIQCglrNsm0uMpRhXAWwFdN8oDBXj9dtTjIqGp3A5SOkuvQIgY2q3iuJWFkJKM719CVAk1bdZ36GHX8j06DxBgs%2FcABgq%2FwMIQhAAGgw2Mzc0MjMxODM4MDUiDFXDCIG9RQE4v90pTircA%2Fwv%2FYIpziFIykLGZ30WmpKqUNFvtCgee2DWI0FRkKvY9DIrbW13gOHhlUxWi8yscmM8hcPQiuAlEGuyZ0DxROEd49UELrnGXalKFwUvUx5B8GzD2QAN5H5OI3uN3QTlW4Fogh6bwOvna5Joi5rl7con9wkc8NtcY34XtC50rpJoxdwgTKCcroKqy9fk0GwbBpDWgiKyLq9mCs7thehD3KX7nVg3GzZ7rJNeOlEVKbB14852%2Fzkm5wD%2FOgl2mZrEZ3JGvgPUr6vneI2qeev%2B5C3xBDf%2BHS3jNW26YC%2Fg5DplbYbDDz2Ou%2B34iXGBotl%2BmiGs1%2F%2BMHfIzHaUaVmVyvNYWWXfp9olF78V1hmrxEAfrtPUwhbEC8rAOwoDI1TLXFziMbsGOKCBAZi6Pp2bFe97ks5R0%2FzSYL%2BXXBY3ZJKboL6KEheeLijqc%2FJBEmb0YVnsEh94jIARttvrXUuBYNtmT1zL51G9ASkXXTqezPkUlHZ3S6McSnVxAuDPjPA7ini3EXsywAB8%2FWaKc6TmrexCwk33E8Gpt%2Bq9P4Jfz6D%2BLDynfc%2FA3yXADzp1vOvo4cr5d1%2F3awCCDBF8z02fGmxon5t66NVl4fDw7Hm1k12NvkO62v%2F1BOiqVyAc3MK2b7NUGOqUBZQ7IfEs%2F5UCpeGObf9HDKi%2BkzX5LWlzGV5hSo8Qjo7bc9PYOmwVQy%2BFLkoRYP9S8v%2FXNZnLQNN%2BYlQROmm864RX%2F07IvyDMevSeCiDRK1RsmgIp8D0tAjBCREb4XfCUFt99vU4KKmCAfSRXMPH9Na3YGCgEXyeNJIcGzSRwUT%2FnM6J%2BIIha00JOIY8PUc1Bwiu%2BGv%2BEUOk7UqRyXgZ6bFIB01oIh&X-Amz-Signature=b9d90fb135d3534503573da755c1aa60da14275dbd57008f2f133727eb06aeea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
