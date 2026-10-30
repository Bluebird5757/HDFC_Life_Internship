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
fetched_at: '2026-10-30T03:29:26.824Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QFE73CL%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032922Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIDJtNFYyjBjnYY0G10cAQ8zZBQXj1sezIBEvm0I7itlGAiEAgSsVhzVpQYNYnrMXngDk1zMAO1PU1kTO9NLmax7hXZAq%2FwMILBAAGgw2Mzc0MjMxODM4MDUiDFVR%2BAouPjemhCPKBCrcA6bocz%2BhU%2BeBgop6yu92It4zxC0BTyEI65YJmkhiGeSyUWTp1eIHuQhXgjADlNCnDdJWhhojIydGNiVbxTij9Q%2FJmoO4d9%2FMb3P%2FHC85ccOuq%2FoFhXTJVyYnQjlttlVSSaUUnEeUK4c7E5FiuUI5ZmxbaKqTH1yS9sNCRwnr5S4JgyNBJdxgXZw%2BcUzwJEj2eo4q%2BZ9OGJVzLWIQLhAYKfFXAxVfr8KUECdCcO9kwbRG1Yph4pCthaSeQ1j8VwZgWxEzl8D2%2FG2BjEysaX6oACS8aSn9k0q0ZdW%2FICTmSSy16I4nTZPlCyAfB22rkUzNtBCcDvpOD%2FTx01A7UAQup%2Bx%2FnS4HaIEFzKu3iLBgtYhLN%2F0MnukIB6XQY%2B3j4gzVm9IorLFea0V31dgzR1ZbaVi1L3B5hP3IHYWnfdMTzL5UQgaReF%2F%2B38sRuSr0lDqvtAdqA4TDrWNMab40G07gSNej0ZCJ2f4m57YeqKT8S5bOvNObnTW%2F%2F9fYy5vwAlng2WkKuocYgHDJxXhjcLBWc8WB%2F8tBAwPylCKaIOtKlItUXk00kl3lKixKLbiPNO4fMK7FAbUDpoXJdo%2Bu8UyLUrv2wYlagz6SdAlOc%2BZsjtyUADNcODhiBD8ukYAPMNCckNcGOqUBSIqUB98qse1CSKbnTTJfRKx95OMy2%2BK4cCMP7g2xxSRYWnvB8yjs%2BKTYB12W4srHNA4bAUtvyqrLgwhExuOcVH3X%2Bh3htKkYUaebs5Ak2ujzjVRhOzCWSj3qs7DXgY0KeGEubygry9ZxVopQO0TLSmpd9j9SM7qJ3qfG%2BVFpfuDtBfNS61oCgWc8ViMX7N446hDeD7mwOjCLqvRr0TgpeS6pk8TP&X-Amz-Signature=b4b47cf4318fe8dd153159f92a62829f30402f79e0873bd18b84da9e8305f687&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTNGRCDC%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032922Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIBmicB4ODnc1Usdjqjfim5aJwjd%2FJx7fJ3%2BsdV5s8GmvAiEAuNlSfZz%2BT1lcLqMw1s223LFTu9jniKq%2BRdl%2Ftqii2B8q%2FwMILBAAGgw2Mzc0MjMxODM4MDUiDJREoZ7r7vtCkf5KnyrcA%2BtvRNGMf7r6%2FBYpBI%2BKMJuElVgyMTUIJyDITOJTRaLtHV2HWi8blkTt1y75mJ4nF%2FNm0ffow8uHXSIayQb5xI8XJNjqroQJR7jHvyAIPaFiI0%2BNkqeleqp4BgmazsEsJjnOP4tZ4JOU%2BpByQfOlSLx6Vt5VPa6uB33tQCL6sHcAIzIbdu32DR0mU7V%2Fv6xmq7h%2BLiFQJx8V47354OLrmZr2ja3dZkegA1yo9YRAvCKme3ZDtFvj9doHTMx2iyN9QGd6RjXR6yO1gmTS3HpP3y89nqLZipL0vYfZx9PpCTNpTRpjIEm1YB%2BGoxRp3B8SbUqp0YQ38QboU455hIUtOb6x5C2xsbAbMNbUWYD%2Bn4TFfpeV5YNyXabBnm%2Bnl%2Fi4tzsaLwBgAMtJyW3tPaDoZ9YAsg6sxFcNXb6IiSW0ppw74cqHjCd%2Fw6e%2FW8MqPCh02NVQvxwcwFu5GmLdf7tqHcFwd%2F2PlQ0kS8vDe6Kde7sbQ%2BVHzrplX9LY1C%2FrENTr0Br%2F7b7ditfxwMtKzDM%2F9XdstJDPftA590TlbbkN4po8FieO8tEfJ%2Fev5kV96LrYxsilviy8mYxkoafXA9nVfyzQXcVp49pmxqAC%2BEt5Pumn7j14GrTVTJaoqhflMM%2BckNcGOqUBq0NAvuYPtLhqG%2Fsb7RqLE%2BU8Gd7XBoCirm71%2FU2PcRlER22H4Jn46Y5Ff7zqXzI3o3gMX3qibotjRh0ivgpPTm24WWrERfAAK2GGZ5W68NsMlTXb9GE3r4gWxRL%2F2%2BLCTMlXqGybBKC%2FKYdeFSiTjzBdf48m5NL0OXLv6US0aSqwV2yOQZxb1Ige3KH0ufBydbojueodbPA1%2BjanVB5wXLj6uLKa&X-Amz-Signature=5a0889eb2befe1451d07c5c86a55dcd5765846767443058b618118a1c4f75253&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROZGNMVL%2F20261030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261030T032923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJGMEQCIGNdK98kj9rLpRpozdZX2V6hPA3Tvz7OaqTsfxnm4qnjAiBlwdCqh0uq6DdmgvnnVWSphNrNiL7IFJ6ius5K93Nj1yr%2FAwgpEAAaDDYzNzQyMzE4MzgwNSIMhw4Q22kbjJeLg%2BeiKtwDk6Nz6TDO5jHZFgFRIc4siQeb70PlgAd%2FrdOBOZRzQFd7qfJvx92hK0HAUDpLZaoTcqTeGLfsbcRAR%2FLRx2BwHxweZ7NrlIzUkNZo0nsSXBdZiVwYybP7gtTmPKIranO2%2BMPP9zDREl1KdguFEvBwknsweRVd88KQs%2BxYv6Rw0jX3uVX3eQfXxcjefZaFh78AveuJVni8F8Uzhga%2FlFlyceohn%2FXy0yLw1GjS4fE%2F1ca9jA6RjwCk0kw%2FMNav%2FhZs%2B4toRlUjHQssmyATP43isKhpf%2FDqt%2FvIUeATyrImWP81qIvmzqb3bxunTUqLx6CWehuC1shsB05112uVWH%2FxoaaAZbhxqBwBwf%2B4NcCvvb9J1m6jbpCgzERp9W4xhhjvGCLEEUQ4ge%2Bfxw2QW2GbW66iuOrXgrMGzUpnn6dNZUFOiBWVo69HnQABLNdWGQHCbiAmu7h3ZQFsWD%2Bn22YP%2BQpGv%2FmFOz%2FA9KXRO5X3HLtwKrej52941xOYnf%2BvF9vHGvYm8zh8JQXYrIpgT%2F3KyRTaLAS0rqvjIvv%2BVYFe2iY1qhdLEKK0mFuXNGhBLONJIF%2BDtZnfh57W78owKGeVbOO6oTMa3meziHxbNfwFSFSu0EKm%2FArbzpBKa48wwc6P1wY6pgGRS6id7Zix47E8zUssmiV%2Fd%2FNykrleLwaC3d4G8pI1GjvGyqCe1bgn%2FdxEWxo9fdxgr00%2FYIU16GeATmAuduJLruuF1tJx62%2Fpgk1p1PI9eGQzETo4PpEyd8tSDO7s9ybVDgX0hY2iJwzmacgtcTzJEsABZq%2FxFbEGmZYIJtSUpP5u0a%2B72eCo136fjllulBKKRqw%2FGtBst4YtBHh7U8G392clqy%2BA&X-Amz-Signature=a245a0ecf821feb75573364cc45099f59202dbe220584a34701112a847d6081c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
