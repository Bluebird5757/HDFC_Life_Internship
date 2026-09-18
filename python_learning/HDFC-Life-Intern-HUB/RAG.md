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
fetched_at: '2026-09-18T02:08:30.512Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VNU45PON%2F20260918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260918T020827Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIECD1Q9WLRodqvUzQeeEBnaDCTGFFl8UNy%2BhHRct%2FtqvAiEAryp8StLjnzGgsfewXDSCd6zctvV6Ugg8roZ%2BfxqBUcgq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDLlGPy0vYvxrKu0XVyrcA1Rlw4N964%2FXsyAZCA0ufg4njBbaLrjAksyzHmPyjdb2yN9ytkn0p5kaRktFM1Mf1Dwltv5Ywa5t%2Fa2tnEY5uKDgGVqBLfIE%2BkMNYcZbrqSuRnBziA4kHnY587lSX1v6%2FChVEPAI67z8TGefphNt%2ByMN4JKWhcCJo3talsYxjSp0h6xt%2Fby5zlXTJ%2BUYU13qHIFyk8riGV7IUGrdT8Vol7XYRHkWoJml0Vnmc93euBmC3uascJcYzN2nuVEF9JZ29dnPptbfCFviAIrAPnS4iAqQWnvzojNcUeO7%2BthssqsmwMFZCbMdwZUBhTfzpzjxZhmKuwyboYfO9xAMCY5FSGIok8WD71BlmxdxyDx8U3Znip1xyEdpwome%2BM4GeBnkYlSQJ9GwjjbwexSHxhM%2FB8Vl34kxXgXLS426CfF02jqrwbAaobicgL3TJFstuRCaiPtrFEx%2FSsk84O4auyTLURV%2BdjyIB50NYj%2FVLB1pfmkgS7XWMeH6C3UaLa2gGF9HA5P49XKGCudJ1608nE2qc4KnNmsFhZK%2FP1ssMM6Dl0c586PCbAMQg9hFERloCP2T6H1Q9oZrkZCkhWU5JTMXmdQP5MW%2BhPK3Hmi3vUW%2Ft9dlYatQFaxYoT0HoTMXMMGVstUGOqUBvvrFJWdVib5lJwzHcD%2B6FdU3egGJrBlgxdg0UJpn5l%2BFXSVAXZJ1Qbx9r5nxjzxs6a1QU9FOwQgOQk3KrANxl81U8%2BqpwIEXku%2BNaxvLJj7fEjhyGRU87YFQPFpzfdwt%2Fmg9Xc1QqV3sqXvIUSUBk8kmpS3HfRBoKiGpLjNMS7%2BW7ssh9Bm%2FcCJI7K1LVhXqO7nrHb2TX6r3RdYxlIy3ep60GZbO&X-Amz-Signature=4f0bc7bedf23b24eb2ad35c3aaebd0bfc19c8cc0583b6e1f05368d96d80378ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NEFSEQ7%2F20260918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260918T020826Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQDK7x%2FwBr0zQTpiA%2FnXv2PRrx9Td91qK5mHEHv64jeHnAIgW%2BG2JHOo3%2BBd87fZZzaD1E0ZjDI9XClcqXuDh0nWsewq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDG25JACArAJslpKXXyrcA7o8n6nbPmV6K7ghUlmrtuCYTPykC%2FeVo51ta%2BUbwlc5GQfxJyy%2B9MNMpea9VIEM9o0AKgCORXFvDpq8YZGtOdZS4PJCRscCgKv1HlM%2FHF2udUaSb4fbqA%2F2OeLsADb7TGKO5Q%2FajVvV6RhjUjT0Kas0W7KA32UOclHgG6SJS%2B2I9sprECN%2BhS1ubI6SMW%2BEsyJmXtzBW%2F9m80MsM%2BCKDVBbxXevd4h0Hb6L4YK5qpTTI1mDmpVGTrAh4eSK7fsIEiWV03OttVqbzomvTo3TS8paWcj4q80Y7XteKXsIM6VbxWsHLJwQ%2BsgeY1oSkWtbvVXmnw%2FTAKsMPubtfgUgBCkq6WVtWevmMSm1H5kmptcxRlCnGWpOfBIQEPD%2BPv%2FGujlhF%2Fqylj8693999YO8fqmf4dZP6%2B%2Bm%2Bc3g0atL3CsKPWOGqI2Kc9xaVUPpfXp8Y1GRh%2FCt2Uo9%2BMgEkKeT9UQexrRKLCjKzozzWo4Qwcbf2vTffRYwraptpCqr4S1NfMIxwpI%2BGdD59o8FAQPJz49My7d2SJVK9vfrKhW77iGRyHiDVddgaAoTXSGCnAiwJWKArWjlf%2B%2F0Btp%2FJfIosK3tqRQVBV8kKzmiRFPXMepraRXjftPUvJLhd3reMLaTstUGOqUBR%2BODgLVksCLo7hMUGOIBDlANhEw0oU8V0u3WzZ8ZiGoW3u8jiNeZ2npldM3nSsYkaF35JeB1BbA%2F2XR9ggd1pHo7GYvCA60%2Bj%2BIF1hzAIj%2Btpmj9kSyeh81JHX42rxHGxoB4SteRZpBkVT8J4TaqX78HEK80oSMT9yqNQbsB1CULInBRDkduNNSFUVIeoKbLEoX75YDZlLUbND%2BULpy6m8MgsuJT&X-Amz-Signature=c2e04a1eb4c0485de72168602bd27000c0e9cd2467724dc18ba658cafe53a5a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVYGOU6K%2F20260918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260918T020827Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQCeeWhGSCTAOBxp5L2tK5iaUArI6u9fMNhUp3yZ35Y5JgIhAITZBa8cDj%2FV%2B2fj4CvnrJuSrFCOXlI9CnZo89xWSSmqKv8DCDoQABoMNjM3NDIzMTgzODA1IgwAt3GHHxeuEuzUku0q3AMVjrrvTeiXD4x9PMuNuq6A9isXD7rfNhuiumXZV5MNJTArqBVgI0GVhJGW9com6SbSdbZkhZInx8heCamTAK2K%2Fc8KKgLwWq%2FLFPCdFv%2FjSFU7BWtr6MFsZJIpLach2Fe0jqG9Yagn9UaDfT8aKa%2FtJnOjSD1QL7JLBa%2BGvEqVD2pRLXF%2Fja4SajpN8F9yg%2Fcamw6E5D8y3CqGyNbBo7tjrDhDnrsH9%2FZL9Mdj4ec%2Bspl2fkTr18PCj3%2Bjvya9RKX%2BIYTyeldFx%2FgLc8DLe4vvDlfiLAbK9izS0q3wMS1kUiuZd%2BUbU8EeB0PC1TrXIXXYwqWOZWnR1d%2Bg5htwXpy%2FrjaiZeJKa8Io4M4d%2FL0QG4zyKDkQzLdEBLw1EL7Ig4121X%2FHq49E1bMe2%2FR3amNwLcgxU4FotXcwFNfFAdgQsWSBYkNNKtviSNJqqcNZxwxtDJlUS692V3g3bJXpMJsoZhY6KYXo2UE%2BKKEj8aKm%2F3ceLiEeNODRDlB%2B%2BEqrRvFuGmm%2BxHNfiBULYZ8K27EfcPhmuGTREgzKEMBwfXC7DHDKx4BwGiX7ovqOTwaKaZ1rO4djHnTvJRhG%2Fma6vZenxXNBMZo7aagwr%2Bwlg%2B2bs2x9kQVBwTcsR38cNjC%2BlbLVBjqkATfY6ILB8aH5algx0bZRu77%2Bsntu6ddt1yUX1htx2x1ypsxiL%2FFfxX8bHO%2F0%2FtIQGhE91B52lD7dZ4Jevgjjmoub8AjB6DwjfszH1tIzW%2FIKLuuWVwV5Pazxa6SVgN0y1slAWpbH8ptzESSgC3XdK0kQzicvSJkayipSumNRcfeMYF2QbRucepXguyXln11hHGFnvTP1YyNP0LuIYY24t5MtjNwi&X-Amz-Signature=9dfc66f0dbf74507ff04273d64802ffd7c3ef886e7319c76434e9e21eae45bb6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
