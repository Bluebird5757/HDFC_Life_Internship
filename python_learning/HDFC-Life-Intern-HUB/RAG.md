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
fetched_at: '2026-10-20T02:35:19.361Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VWEPJAIG%2F20261020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261020T023516Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIFQGoQB%2FoGNz3JSA3p9Sk9NXkjHqXf%2FPZ2615RhSNaB4AiEA62cIvEsTc81ZlM2hcBcJ3TBrn09cQ3yLnj7ebXW%2FH8Qq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDI7Sk8dYFBxv7hMZ1CrcA60d%2FAmvJ52zeXUj%2B5htUD9pzDp74RNt%2BlLXrRDwHlGQ4vc8avxGxs6i1H2%2FVKLDl8EpC%2BayOZtu3ciNiCCWH3U1y%2BSzZUL4iYcN1ftdvleBEmNqpvUgeiIoD6NX1Upn6CKsYJnL2w63PcNSyObTFV69fnMxhoL9YVhg3QFchIKRXmsrWw7r1TRFG%2FfjCQeWNCKNygaQm85x80RCjJ7cZgxaTeUipKW6sLBvDwL8mndchfvl5ZqrmY5O91TaSw%2BID0a3XV9v2AXRxjZb5YAfZJ7vpk44iva%2FnJM4GoF0I4QIDPl7%2BUmtProz8EB1W0xH%2BsrRJbFp38eNstnVNJ2uxYLUsVoqrjlyQjzMi5camTgH3Q2kdmwAeOiVH%2BffgaJDJjYH2UFMO62eBv%2BYXRXXeSWNLiSzJg1ER%2Bf84Ji2G10RLEqUxKbShV09LP85UZTsnREWLautGktzIvxQjo0jocVvNhzpe7fBcSXLYZhWEttuUvZnuTIW5asJfqGOwNbEolhZrUdlpfsSbjKT8SGBBSm5Mo7hBVvEyxeON8XxNhW71pNQ%2BTkimIo4KgNFc8fHOt3i8prIsHhYmsVLdS5XQNal2aDCmKWBC6gm%2FYSN8%2BSsPH1jyWT3UIdm9NLDMP6B29YGOqUBqQBo5SS0dGswoPCzThw4gs1QvDNULQbhFJxVOFe6xJtM7Kyu0qag9D9xYJU72%2Bsi58mBtAlILexI3PlxAs0OpVmzuEc%2BRStNadVg3txF7RZN52lS9TztnejiVY9Gp%2B%2F2%2FlYNimKZERIib%2B%2BGXCNVIHnU4BaHE8mRinv66gFdVbZEGoANzE%2F1uoEob%2BcVkQwoYQa6Pi9z2hPwenASLeWJFMbh3dyj&X-Amz-Signature=e1b5efa70528e0ffbb2a8f6bfe8a1d1e0a1b3d4db4e2b5073cc27fe99b203da2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKZXOEFN%2F20261020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261020T023515Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQCJlruPo3TfazXZc%2BUdWnJaKqmrgBMWSiduIQjGH5OabAIhAIAKz76nvXN9dLDtZ2ayBIK2fFUfL6UXAnJ1pMXS%2Fgu%2BKv8DCDoQABoMNjM3NDIzMTgzODA1Igyv7fzbk780KV5fya4q3AOaidHyF3upKx%2BRO2RHOft%2FKapkZLNgRsDfl1okcB6r2XQJAntsHnamoHcyCuUz7F74uNPwFUAG0Nsfk1U%2BOccCvH%2FVyOH5NpYXat7DmSCVoQEaNW8zSCr6uM2FduCpDnGXLIhUgQAOyyiy5SimjbCTniQnpUOHxLE2ccUnVco1shNutT5HKU5Vszg7JtNw7Uk7QDmzezQ6jJaOX581PVPAWueDr5MqTU4fBM0TlJyR81vwXr%2Frabml0wbyDSpQw6Ch3q4yfZtiMWBD8dsYA%2B4L6iUzLK9JQ3jddMvhyhgRunsmWgtDYZK3M%2FNSfKTjahzVzNleMTDJRaDS%2BvP7SNio7O97IEWZC0mbvOJdrm8%2Fg9dxypC3g9XjTR97p5LrakrXPjNj5IuIVVkBLYF5owVXrjj69%2FW6jCwMxVi%2B%2FyGoU7af5Glt%2B5z06qirJjuHZnYf3uPW9f5AqYqJznZLx%2Bii7x9YDkmBb6%2BRBFXF2eqpRniOM4rzjs4FDplwazLx2pEomqndR2RwhBvV4MC18V%2FFzXT6ZveemLpFPUTfhO6RtdV6660chMhzwfzIt66RW1%2BNPe1EmZys6X%2BC2oH3ac%2F65P6RUB34nXCKeiKjc94ISRjDsthvkjsFP%2FtbcjCKgNvWBjqkATWxHx27TN9JBOAuow9X2W1QVh9CvIaNKYi1BD6Fd4e2v3CMyv4tRO1jXAJ4a5YFxu2M%2BlgpoSdV2P8zc%2B%2FiTd6OQdroK%2B0bVxImOO0PVtRV%2FzuGPbY%2BRaQX2dIyFgYwi8Sbuw8X37NaBOX0qt0i2%2BjOh7qqz9VxxuItcT3wNP6eIOBaVzW%2F90%2BHrzM83hUejP%2FZxpQEuNcu6lfNETq%2BTp02WdJY&X-Amz-Signature=3a80382c64eca7f91c24280e14986a6e8dc4be995e0ff9e4d80da7ba7a1d0809&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QSFD765Y%2F20261020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261020T023516Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIE1vjLcUQsIOcWChEU8pvbQfZJobRGlDlmLInOIyjU8qAiEAt5YCasgLybUWt3hwJH477sSVKHsdUvPfjnvkIcav16Eq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDIWuOfpO6FI1FxzbkyrcAxO805fkAySnIsTYoXjU%2FdBOgPKy%2FUSH1mJAuSKJzjQiKvniJM5jgDSGapf6wRhEhgCWKi0aTWolnUoLLUcxx%2BJ1UWpThPhogXS6cqxpAyWUI9bpLX%2FJVeBpoviHnAY5MYL6CRELcNBXuwqm0dTcrle9XIHzJ3m2tVu2gFejAhvl9WcHJ5NC9SyonXpncdsTazebDINI5bYdP4I4QSfC4Tcgwr99Xu7eJrHFHx7UJVzh6fMpsh4qHLC5tU7nRgTq0zAvsDTYpulsnXDyO12MTutMi%2FaVblGedxd5tk9ElSUXmPRqa5LMTwx4LGyE%2FLlRJzkBduNHPTe62ssm2jyqsMS%2Fn5dkWCTYJVfQtzzmVgFlMm68nfR59bu9Je5kY8%2B7D05phi20PaSpN5WhqosptTwn6Qqcmqty%2F3habFzb9DKrkxWoCzv1X5ShBhWeQ30JBDnOwCVAz6h8XF6u3iJ%2BEHotHiUAHGcoigt2uyQME5eZTdiAYlbogpu%2Bmn7kVdRtHtCIV45viPWJ%2FAGTtydFk3G2h0gWnJ9XSW1EOKPG1scJQRp5gCPcvAGDM8pE9JkDN5Waqks1Agr96HJsjlfemXZ9rH887TOjRzeFvV%2B6lTmEkTF0nbYQfrRcqPYUMJKC29YGOqUBV7ERx8vyCfvnwkFaveBbZi93z8izaMoZ%2Fh1UySh%2FLruoV2aaMpH4%2B9PTxZl9mNw2BEMsUJQjxM%2FwwOMIeojFqULpv%2BPqMZLaw6Xt0oKbVQCZumSsCc1%2BOmsJONvI3k3VI5ScLOWpENtKSQqMpzYi0DFjnD7xW%2FEcpP5q%2FxrXtRRko5fFn5DQSKS2rxywVxjMe3b%2BF7yRDp3Z5MbgPYVNXumDUbRI&X-Amz-Signature=fff395aa8e36ac6df47c1ae027e7038f343f565e201b5c485a0e1ef756b5ca50&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
