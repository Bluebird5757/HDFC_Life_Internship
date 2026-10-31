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
fetched_at: '2026-10-31T03:14:01.292Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PUW5N5O%2F20261031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261031T031357Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIAG2leo%2BJ%2BPfw9IuXQeApHEeX2qqp1f%2Bntma6SFaswikAiEAoHt7SYFUYK6nsbGB800u97nEOEAIAEf9HvF%2BqIT4Eo4q%2FwMIQxAAGgw2Mzc0MjMxODM4MDUiDOkj%2ByR62yyFbn8oDSrcA3zjzwwraXFU%2FP4SEaRQPhCDh7YkE%2Bv9bj0NPrBvvkngHASd1h0Hq8XjfTkMs42qkurk8uHHkEXoAq3XCampEJ4bcI6i6ZvLsJ1YD%2BjqCXotrPGX5uQHTPJ5f3GIuDZupCffawB4EieW%2FskB%2FBrWHboNQE9vGVC8KGoljRGR9ZSniqSpth28qJbJuFJKe5taq84tyMjPg8k1cgsbM%2BuhRrdYeoC14ABjrw4rgy%2BGLHdWqZHjZq2V%2BgoikLx41k0ITaVIgW984NSVPjgSNoryympFGQp3m3yGYjndS05VinYbn9Zmg%2BP6IGDa12FuuwLtmcLjdigCyF6Z9t6RlnfT0KvEBhQFq%2BRVSQenGnGOvOzECEhXI6miXGJKQxtk2SUP%2BJJ5lOU5kaeyNvRcehTH4D2d9m1hnz2W30pxhUaLY8wOH1gXXcwyZYFz%2FZxtXtclSiepkGMafWeTzWFndaPR9RguLvLgNoXKKDmCMa2qXJN4i7sFB8AFDcetXeJbYD7bMWUiWdtWmzyq6OBcDDLPj2TJsvqLx6TNTT6xMflI9jWrxjZowEvvmNZCxHb4vH6mGrT8ICFCdlC4LxVNStKn6C9U%2FFfTBSnK6vS%2FE7gMsEwCVddQ9QiC6Lo5%2Fvt%2FMMixldcGOqUBnf6c3aauNifFAJQuHfq5aPbAzn77dF8kWrIsIJCoKDkyStxHEyiM8qD%2BVXqEih8kMKj%2BsheOZ7AtzJQQvEdiGogPWvhUpcBE6HLMKgFM%2BsHQPrfz2mqFCqORnRxmQvFxEFZRK6p01XsG5FzHVzwwxOtLszJ5ARE4i18idpTx29bneNbpy8Vmm7TwdgEAJTeplRm2hxRf2Z%2Fxc4Vj7sKgxqsFdnDY&X-Amz-Signature=baf153df75738fb6c2111119c84d7ab8180efccdaa31450e0810e0657c27f213&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VQBTKWFF%2F20261031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261031T031357Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJHMEUCIHGs6nz%2FJeYRveR0LlI0AdDPgaKPecky8wpwk2i8OZZwAiEAiCthY3YeTJUfU4ZbybySWOBU3qC4KWE8DZVOPClEuzYq%2FwMIQxAAGgw2Mzc0MjMxODM4MDUiDMgWA%2Fxi%2FROYAdqDDSrcAzwcNXxlOsRYdLIUQjhzw2Pur89gcs%2F5l%2FzyiE8F0avWTveQUGrp9WuGjMcJXeqIy5ziRqTsVKk%2BNgR2VmW%2BXeFQBe8j0PP1mFiVndb560RUSwdOh036yJSVqNYvaktZ%2B%2Be9waeKWYrvUzOfKljoL6HGZVu0mCNnvS%2BvDvOZTSC44KOFHIiXwakmUgCLw6fG2ZsgVTA1ool1kGt8rfJvNGhhjnTGWXYYwyLp%2FCgkPVv%2BxkZiLKCuM5SwMvuwf0BV2U2JYxxh6yNJhXwv19DWGjohaz6M%2BNZpAoNaS%2BWGoSz%2F19FTC5evTc3n71TKRs8dPN7OF%2BljdA3it%2FByHNFsUlXBXWfmwho7B1Y8R9MxbQWbpodchZE2MFHksCdb1eGuBKB74ckzdr%2FqNeQEIp4WeDIfYQIOQSXi8IVMkhpOiXC%2Bn%2BXQXcoqqvpNuL83LBZ7Op1UWaTkSahSplI0ZHHIkV1TWR2IWsSEYWziSirMGGhOhCtT9PtUF5fFzwkK6D9HMuoFBLaez4xMbIQR0chnAwVIettmNCOIm1NjCE9vRGZa1O4sNownagW0k4udPCkyPj1%2FJvBGBvdePf6LFgl5%2Blqu8fVT52Q1cW8UvbzIMPT61sp6%2Br8CpPNFJpeXMKiUldcGOqUB7oJrjcwYfzlkh2QW5R3RCQFHE%2BnU8rmkKElwbIV825QTfDF6ouDuMo7i0FDvKHR29juQHc16jqHnLmxNNx5eW0hvC9mljBM2ENvi3M3LHvr8Egwg9s1L8%2FIsenLVmsngCAin3Zwok76grtswQ7GMV6v0B29MYnjxO66pDQPW12lFbxlulFtlGBeoXg3rWGlSJrUFmmBbHfW6Sw4bE5WZBhTVo3V%2B&X-Amz-Signature=6b9e09b80f53c79e43559b4a0fad94cc632dae777d8719cc740f4773df4260a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RXGSKHSH%2F20261031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261031T031358Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQDIgxFJPjGuz%2Bf4Ll9cEWA2M8M0uaN9P2HQhdw5VLjHyQIhANezw%2FO5KXu5dIZeH4rZRi2E1ahHII%2FE90Qp0wg3DC84Kv8DCEIQABoMNjM3NDIzMTgzODA1IgwSJXgDuddeNTdbwAAq3APZXEFelXWSUkEqxEZQzcisvDBu3tzL87wOqyxVCom07L5OjDjy0okKhXuPjnfUB19Mz3mMRyuuouw0WmD3VXne%2FQtNFiTvAnpAZAk%2B1iz7YOV6IO0N1ED83zB0VNGWSTEstqJyysBp2Dce3BhwKw6VqP3FQTKVyvRIyDjwniWlJNuvyLqSsk%2B%2BPSOLaIvVpdrafZfKVPxc9IiqcJKfQOQWY57aldJAEv5KWNB2oq6btQpHAanzbZpnROb3ECApxPpv8CDgnGJmdnhQcT3CAKI8DB9TXZ4km7bX93YJTADRlidH6rUikSBnrYnDOTSWaLn5nbtG7PYXuSP9Ihr6veAoXkvqNL7hDOG4ZCiQmJw%2F6bsjtF3oqOH0WMQ0dZQZHMGS6zQm6xILzDRwBGIVaokhdNn7WhP2uyhwS4h800RoFbz%2FFDw8W4SgTGAd4jzKtNUbT9g%2FDe2Kw259DmlIWvVXZRm7wrAKx8KhwDESHPuk3oRcZeuv5UkUKdZhzU8IMrASR0fMMCXi1saeLcwoRwdXFqxpdEYh7rjG3hHzQl5F1lbSqHrK0KDVehIQZXAjpvBfD5dMXXJFAuJ3YXMofc%2FIUZ3xmgUx%2Bl0jhGc0IzBja%2BKesN5Cn4065I6ZpzCykpXXBjqkAeEFN%2FvOBzZxDDctbVmySzjpvkoM8a%2FhNbUfT1EqRiNa5kpvOm70sV6Bax2dm6e9WD3OVwChnb52vHpnkNy6AMMbAX0wUW3XyHqdKYkFyBVkf%2BCQLiHoFxpAwBpSOp3yRxV6%2BmOjQ%2FjCeUKFigHihkcmcGL6U7%2F7GJ0ADczLiRlmTzDhhH%2BKUT2NgQVU1ElzxTHh0Lj49zIkzjNaHRVgI%2BUhaocT&X-Amz-Signature=22da807c8d280edc03cdcb628d67b16684982cc52282241bf51d7dd1ad7ab5a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
