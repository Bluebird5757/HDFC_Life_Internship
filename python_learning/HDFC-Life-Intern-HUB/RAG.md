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
fetched_at: '2026-09-14T02:19:32.404Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W2732QRB%2F20260914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260914T021927Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIQCwENVvTCiwBYw9wmN7%2BpI%2F8rzjIp1Ng7blIKC2fTO%2BhgIgSsRDBwZhaK8EiSnznCQJelfaMNx39Bu%2F3zyuJ8wJ%2Fp0qiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI5ejMU%2BDauepX5ASyrcA%2Bog84%2BXob3bNkzGS0i3uxRupPm2X94C59Z4ymCAViLVubrUoHJPsZTw82eV5IQhCqtS5XKVdj7iU8qQLukmSvUz5eAnrdETSZ6EIulX99aaI0ZxYhJV7%2BiwwrvuHat9VBFimDPDmmzcvMvDQM4A25pfPglfJCIiQp5VSqKxLFDOH7XNbNQezzxkpN0Cj7uBaNsOTJosb4WIjUN7tXh2rxYbbxvc6y%2BdFx2dP5oD2%2FEUJnrQ4V0yVjtmqR0i7Fwjz0Xt99SlyFkkhpL3FVlCo5saExaoNK%2Fd1c%2Bjq2xb1dtbq9PZeBZcwFbFb%2F35ZXwGthGuGnEqJJ9dGhrKvP0HJKoFuqVd5qAiq8C4tCbJc99yqmv4seQl5%2BFhM2OZe%2BiJfOul7hM6fBnm9MQbFimRFLF%2FdoCaLEII8s1CUDOJYrbEP41gyLRrHJhlAYnI%2BZzILtM0iKC6MZMN8qh3nW79RZ%2BhEv32JjBzpKctO6yZf6MPdkjomXZgdrwXI6ToSmzHys73S%2Bvg1wGRich1CEUqKl0OSj%2FVVUj%2B3YrNYT3RPAE91zmA%2Faws%2FCMUhwyfxJto2zBTsUjI2pBsqDHxrcRO32%2B1r5n6RpcrjxN9MkD852CQ3B7I1S2VnUGRC2BXMKiDndUGOqUB5%2BAN9i%2FsM3Fw22nId5ZiqmW%2BBraWJA6ce1k%2FO6oJMGb9omfN5V5za1%2FmYWvzUOpCLF%2Bu3ff%2B278%2F2%2F4Khfp9%2FxAaS%2Fh8F%2BZSXT6SnQBqGfltyhEnRKsAZROd36frJl0m9elf2FfER67ZyIE7FuLjZQnW54tKs%2BdkoqAKUERjpsBlwNZP0sqx8B7pzQe0SJJ5vg91od1tXg9AOnsTcAGNNPuy7arX&X-Amz-Signature=61dc4b2d4737a5a5b850bfc5b66f43f05fc2d084521df5eb3db382291582201a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJ6BKUJP%2F20260914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260914T021927Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIDyUV%2Fuhfa3spzr9RTReYRENIvmfD4q0Z7QQdXReTaHOAiB5dLCpoPBML%2BGaH98%2Fu7ZaW9lOPoxoK%2BTK4Vtq%2FGeQwyqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMPgEqNHGEdr7zrelXKtwDsK%2FcDRA%2B350wm1sbJ4K25SKUU78gJN2f1FcznJ8NftyPfCl6Gvh%2Fx5vidL1KwgXX6d7qtUWnPsgP3Z9V6c8wOs%2B6sSiFrrD8rejnhL%2BkF1o0Jw10pVuDXq5PQX2j31D92FYNC%2BcfAZZMDILRjVZ6gyvfURNBSq%2FSLJGQUe%2BTJJdZLd2MUBtstV1mNRkp3i7WjFYcrE9BwkUdZ1KGKvHz0%2BtebNGHl5PYcjwHI%2BiXKwwnXQSegVJBLf9OnxMBjOLnJLpBAlyH0W8feqs%2FxybFb6EwVEAWA3ylfMLvxRHdGtYnOpgzINLuZMY1wCC0GSDlwEwx3J%2FNvZlxaowSCslRZDZWKg8HGfBx%2BUi9jtQNQAuoGLkgdcmvVxDfRTZhlHsnAbBFaPcciIpcm7rkFTGGxiZe09ISy2xYc4D9inzXRap2jcAa8tjHXJqnnReUV99vJbm7cFFhqIrxb2UTI10%2B7Fo6PeZ7PwepAfeN0TM5BwpsgNtDe8Uit1VFaBIrW%2BYm6dffD0wf8B3FYBX%2FAHqQEcsIx2cTbS3b4L0jBLlDtkSftKlV%2BzNKE5mNJi%2FM6cesZCFH7C3lVcf6teaSlQf243MfpyUtHZeN5knX5SvIFLAmL8S5rHMzBccYvskwp4Od1QY6pgEQqqFX3lFcVWWLmP%2BKB43AF%2Fyvy8l2SNKyixYYMDCWsEG6w0SuJq7JqkM89FTM7F5TegM0%2FL%2B3XQ2RC0ISn1Bn12mPPfDdM1qqkAVTrrZbklcTAAVoG%2FO36I2p%2F7nuIEAhrqcmnl%2BL93fw8MkstY32kkTnDGpbvBUH9fNw66BUaTXjU8qLKGIIhOQu3ceDC%2B9TLZCM9Qw%2BrLXGFqCS9tmqLJJBhVNL&X-Amz-Signature=1f96e61bfdcaaa68795e4940d10033897e2666c42cab5b3f673da2b912c7f34d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRW5TABV%2F20260914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260914T021929Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIC1S0w1u17KFgNRIy9GuvARGJRN%2Bns49goANwoh1O3s0AiBxqNrLy55iwavALrz459VJJRi5FMzyzN6a%2FyjAWRS%2BKSqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMzSzkJtXlkkKyj8kuKtwDYWfV%2BnANqZ0QgeWakakuYNPxf1BKA%2F6kJsP3Tb4MxoYKrHE7GtzMR8meC9OYc05WVJSK2gu3rmhs2MPoFVjN2EzQVFGmYi3o%2BtuqM%2BmFx0zvcMRe9xzH5fTJXgwPpI7WuV4x2DsltQguzE4%2BOrGFdopkyKimPOvjMy69MqYPIBwZfSfregttIvMIXHMcgx2LhiFwFOXpbxydG9BaRN9ZTCxiUPgd8VcXlEMuj%2BXXqdk035VoF%2Bvb1DObvd%2B%2FFSHeFGOSkALUaZNBdDbAxwffhZSj64H8XmY0MHQQVFe%2FXTzME6d1x2UF8YPsHPeUBjwx1yCLGGn9wzb2AuEsmsLYaKc654PSKVf7ae0rXSdcH3Th7WGl6ARPjZ%2F%2FjBDJIPaEFaRZ6MVnpi3tHQXiYL5ISjiEHGKvPusxAHy556800XgnBT%2BB9BPUH21S59WSs%2FVuHG0uGjYolHO7ukuYfLHUidOLVBEqXx8Ye0VxlrEEAYU0hT8RvKXN168sRdwDorSw1%2FMFhE47IzaNJI1e2b2%2BGk3%2FNge0NkNNGjf9vcTIdiqAfA4SyqcGQhF90UCeVxS9uVQog%2FXbhWP1c%2BqpDGF%2FB3aDFQHdudThYRGI1TTpdzNTm6mrPGJ%2FDRN%2FKFcwyYOd1QY6pgFl9wxsmvYYGyPUlba9wJ%2FKcdCLzB7v1mEL%2BcDr2fCIeyBuyu0uh9gt8Ah%2Fa9eAGHa7%2Fas17alvMgQY9gwcnkA0dNjHBNE8j6ttg1hiIFyAdwb4IRMams7LZMKh1oSB%2BsjTLUxeIyuXsX0EmgMzqaHgglGs%2BNaBpEe2d5%2F1nMlg9F8E1iPXFc73O5VtwYfF0oqyulv%2BJuuuhqlG6nwYd23XxH5XVis7&X-Amz-Signature=e2cdbd8e62d5406bc216d55455934a0754116a2105e86e1d185bff64f7b12944&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
