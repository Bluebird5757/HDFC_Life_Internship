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
fetched_at: '2026-11-09T03:38:48.253Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ULF3GGOZ%2F20261109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261109T033846Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJIMEYCIQCQhja5%2BsZ0pYdlGpWHBhqflUlbkfwX8Hk7l7GS9BpvhQIhAL1T3RqLozedpE7fGB7EzGLcGv6BUjOl5B2ph%2FhEFbSkKv8DCBwQABoMNjM3NDIzMTgzODA1IgzXIEmwWIYMhCXcTicq3AOe8VKkZuF9xMyuT09UcUf%2FByuta%2FYcfBzZ%2BAJV%2Blqc5WDYFrgTzqh7x0AukmzvZmOO6W%2F4q7yAAr0OFIpMbdIHBaJHiz4Ht3maVq7UlJgshBQ8oqCRmWd%2FFVSDX%2FuA%2FgbjXiH2ZmklVlKYf0ivIdghNOg1JUxWOw7%2B8NOHIxiEPm8NChbrbVNUzZzMd%2BIkZDal13j7uzakgrn7Khcd0%2B%2BRR3RLBrwRpYBrcLE1j9%2BFl%2FgmVdPk%2B0WGcIRotKqqHyCCMFzH5KvctFJcpR8xpkpd1i2skZIY7AB71CBPSEZuoLhiUJvnzwN%2BhKflPwNY0J4Rq5QoG0jzEMVw0YG%2BW9FQyxp3DNQDFpbMKoro6JqqrOjg6aHVXlXY9T98fZyRF2451pRLGwjlW8BvuygwF4fk4%2BZ6ySSIWi96xk2UF68dQ7jPoFWKDcArSufcqA3GBVy4kjdVylUoPTd46Wzpls7NTnPk9EmwujK2ZheRRMVRDlRvoOggBQmpEy4W59dm%2F4le2J9Jt8fUCSveDZ8cVs6BT%2BNOaRde2Fb8GIh6pa17vxP1y9Hel8mtG33bDeyQDN1fbdj01v3hbRvQxpabdXfLQIA4iYuDKolOrKFqzCqOAUEVFXx0wGY1dqTh6zC59MTXBjqkAdB88gP4wKDJctn83InQwk%2BwUuJjCpfL%2Bc%2FUPQ%2BDkxSTAwdQIDSugwYOjiFaDyfPpUHuoXrjkIUg1%2BAQs87KndoNJBGSi9IVBQHqdnvenkS4c9EY5RQlOQZZqF79O9WPaDFafaGSOI85EmR3TyNu43ScNSkur0cZylvk7Ed%2FhaXYqNPhZ2B8GxXnTt2BDU8BKfCJ9%2FVA9l4czAfxAaDzmkJsUUzi&X-Amz-Signature=1add8e20aecd53f4f20f1c38fe063f4111cb31ce3ab60d77f3c07d072e9b5b88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466435BST6Z%2F20261109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261109T033846Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIGcAR0SnNLohN%2FgCzzp8bUxLj7ow1Qei9S83wZm3eQCPAiEAmyEgrhW9Dfo%2FkYtoPgHJzacL05ojAK1AoL4%2FYbpRUgcq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDO9K%2Fm9Gf1XjynBQHyrcA2sw%2FTiyEFpy0d4t1TL5dHyrp%2FKzv1%2BY%2Fig7lhZXMqctCogSGQq7Tm7LpIi7Mv3Qr3PcytHiEd02exp0bjGoeWJDLvCmTitTGzj8Fi1jcLw7OIkfzlzMVZKByl2eT4qbVy981G%2FIOzqFAn9FngZESgRMdzUsm1NyoyHD3iWdIVGGnq2a77XZo7jiqBM%2FYz6F1SFANe%2BSaGWlMUu6CKPWI7BJsCdRtuxFvcfhq5kOsmfhCX5HgRvkt7x4sYXwYS5lowJkPfMk8eYi2xF6K25WY1TWElph1FkxM9jlXo%2F0xxPEIHWCDUq2jhZbq%2FqcfL6wLgA7LMGj%2BSCPcEHraGvJBUrqMX%2Bz8hM1iZJsmGI3tSUIKsub8%2BU1imtIffgvALlf%2F03yNRKnepxn8o5MUMANtIC2eYBR6izdtqirMGOtX%2BDsOlNKXjdI7A3XfpOU4YVbtgjL8W5NAWg%2FHB5Ngzg78uG7bHo5QGoqcF4%2FX7zPkrcjCNNrrbqrdzSW9tHINCs2AYjv1g321te9g2cLOCNQMSJzd%2BfVp5lna%2BeKUrplDWW1eLYA2U3%2Fe2kcMsgw9C6WwyRy6GG8MmlDfU90IbvOuqYA9avASyNbvmDD%2FQ2LKuyp9LiNcxpl%2FVnsmhu5MKT2xNcGOqUB6IlJVYp97tO3E8zaTV%2BhjcPTwauFlKYCq%2FIEd3bQmk%2FJ3BxbYRB9Bz4FOvk5Vp1kftaaQQXuz54SJFphgeDGP1KfnuyT3z2Zsdo2VBHSPK0SgIB5CWU9Qoh32Xan%2FacP8gdvKc8GcEl6N315W8MjqHLqq6jykii%2BCZj9Xf8ASsQuq3IT%2BwGSbmHqouZcHz83XsvvL3fDbeRUS3eSXUivqJrFo8Ns&X-Amz-Signature=8bd8c811def1a2f717d577c9ff170ad65dc1985e77e4fa4292d3c0c24e66bb50&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZSI6PLI5%2F20261109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261109T033846Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJGMEQCIHBVyQSlg0QXDyAWAya8QYwkseE33ryIbX6z7mBq7HjLAiBrGY%2BdM8VRcacd9qsU6ZBmRKCvhAhUIHhUJqrW7WiE4ir%2FAwgcEAAaDDYzNzQyMzE4MzgwNSIM01HGUvlwKH%2BrcFKbKtwDo4UE0LZb5jWNZdskSiVwB%2FFx5we%2FVR6cn6ccI%2BGd8p2S5HSUOu4Q2SctZRgpwzRPRxkVAB9Qk987eh5%2B1KAW4c%2Bif357JDILTGLrsJWdkfJ4tuUj9wAS4iEeRCll%2FBSbPqCCo1FByI6C7p51Fa%2FQE1zb2T65gwC21nTdnsnwr1th322W2hWkRGId%2B7aeTMZcVnyO8%2BHgP5BdP5kcZfB9x%2F16iTqY8snj9cvK1ZKK7uFYRG7tL1cxnmQuAhyDYMk2zyM4Yw3BLuSet7Bhc9l1vhXpOhYsux8svZUM1Xl1oDVKwqeqh%2FzKdhXCgUhazUBIvWajw5tKhDy8g3LNBZ7IzjVK6Z4AxjlXbZ2eK97D3EhZXzkjkT1QuAtvTKYDZSsJPyUj0cgxDi1Pzl2uQv9BMtAd%2Bf6PS92DHuTRsdQ87GLiEA%2FsAC9cRUrnHVq0iuPzBWOl8G7A0xzPdAWaQV%2FTf6BMhDokp4JmwZTRYp1GXKzjZRWKusfNE%2F5IsVx%2F29xLTBeC%2F1n6%2FHSZKboyXleH0QyzBuCtV3GCE5M6l7AcjQJFQE4dBQL2pRU0AnaP3oEXIc0rZX6t3i8wzCTm9l05NaIDiRV7Zbjq964uAGv%2B409cb62EPG4Xufop9H4wuvTE1wY6pgEvPikKtyR1owuKzTGJNrroqO2zXpzdRUaubt75qwjEoakzh1epEwmP4sFFrBGyJvbwJ98RJydtpUdQhdFEd3hTN%2F1iF30517qzy7FpL9%2Fefeh8OpUbSIIiFid27HClzrijgyt%2F0uT3t70r68uXh9cvwRsKlMw5DNJ%2FMtES%2F6G93YrgUWOemYs82EfB9IZJrCHm%2FvliRaVPJHWo1T3%2BgtQZybsCcyiP&X-Amz-Signature=3960969ae905c27664591394ccd726218456a114cf891345dce77a1db2856e83&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
