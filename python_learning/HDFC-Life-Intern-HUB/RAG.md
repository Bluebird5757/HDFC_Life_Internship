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
fetched_at: '2026-10-27T02:52:40.471Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WKVGUXBR%2F20261027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261027T025236Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIAmI4ScoM0IAd8aVkfqHCbRDjPuH4Fphkcqr65gARNKOAiEAhBEMSmy7ai04d45n3gBs09hL1qTDWsd1erYlilE62AgqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ1nKSoh5NbZkReC5yrcAykLaCvVYNnWxVEOIRSSLmkoWZOY0KqfVljcX7oR5syLEWTdIH2ifRF3HelYlYOXTN5I1mYKSUa%2FR2O4U2BelxAaRGNDlFvYQYn83JeXKvXRROsocLkN9xGleSzk%2BA1ryyKgGTyDKTktGmfLqpbkgeXCqu77BvfLcBIOckW8smsl1OvTWmwPggveyF3d59rW9xnoOeh%2FMZmKGXSESzr0qGCnEXX4enYg8wuL7dXpFHG00n2Gl4St8ywWF29TdD4PurBJBVdl3R2sJ2%2BiJGTqaSBEoyZah7HyxfJcnBwOOC1oT1f8RjYKhlGD65XKfn1APhMjJIuZtaaEFqs1HjWnvIzyLWak0ATFoAIHTatBq1mW8exlaO88jXx0IrYJsLm3j%2Fx95TFTMIOpEpOg%2BP6wibkmUckgu%2B%2FwgYkACNIQ778DwGN%2FH4lJoB012O4IiocoCIjn9MLJOuUUcmzgJqnAp0AcyN4pZu45cmD8Gmpbzt57OaUGzjz9dgUxLmNsS2ILlAKAZD1Dj0fm43hwVPiJGzhhGDlmrXdSu83A3HGm4ek6VKe0K5Lp1i3MoZut2bciUGByN4eReKyaOfrULZH99NevG1k1%2BNZoO4fxcXLPDWW3l8CbYy2HYErWJwHRMJz5%2F9YGOqUBsLAma7uFddyaestP3ukvr%2BXhro7SRPLWcLx0JAS3uSv8C1q7kFSW27283CEWN5i7SaVBKm0Ad%2FkOGubuxLZzCVtFLNzWGmQ80GtlAThkWSSUDKkjWQAeV3foY8ddB8rCBkOLY5Nsw0p3bxpKc4Yo90j7k8JFSqam0lkrXNwMDkcHauZuLS1VPVpQPiDnc8wwFPhWZwAl0vZlEp%2B4%2Bi7eAX0G218%2F&X-Amz-Signature=6f7a30af47a18c2d6087a8dfd517289244720a1a2ae79c3cb4241ced2dc3860e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXG2NX3D%2F20261027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261027T025236Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIHrW%2B4u17TpwADKKGKYDRxED4O5YVe%2BTx62z%2FmzE2oRLAiEAop0Z4YoNfReD4QGvS4qYmZxq6SQIKpxE7mfwKo2ZK%2B4qiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBPttNn0k5ykk4uGKCrcA5EfZkiySd79wWRaAkyWD3Z2qtaCqNisZLjMAhVGmkEv2fvjbxpccbLs71mAc4OpUtk77wv4cjmgnky9ecp6IgfAGsZSOmnKvWA61rTOZ7FEu61Uj4uU9RHKyN%2B%2F3qsN%2Bved930RagIG8jA2XkZxIohCfzD%2FvOU3nTAgZHT7z4rOxrQU8SbTuM%2B8sAIHhTiW5dUv0m4zQI6h%2FkWJkP8tS%2B1VVCg85C5QrqBtwfLzIPihrmRYADlXxAIxjeSGSuPh5uofaIh1EUfqzWBtNPJCwiXMuGUB3XKwF8dHtityyz1h7xlG1DlfYVi4%2BDTKKnUkJfoeKn2Y6Mn2sf83cQL8ekMqa18Ct46NUc3ItoLnT40nucXMlMKXkzg1IJRN1gyxZ0a%2B%2FLfrz3Ew7mqTKokX3zl4ZrtU%2BPd3MGzXpWkz%2BIFtbtV%2FeSBOGPwTy8GL%2BWyGSXTNdU%2BOk2zLT11NqM0Lz8GHF%2BoXK4YMfb9AOVuPZXPlwI6Dg7HAr6Pr%2FJd%2FSGAhuv88k6UzZAVU%2BVIPqnL34vo4Vkz4Lfq12Ydd7XMR9g2JJn9fjhymnFBEgiZ1L%2FWazRPjhjzdQxrZ9Gs9RbnzQFV%2BX6dG4wm%2FqT2PHEKoT5sld%2B45nES840g8eCKFMKj3%2F9YGOqUBb%2FwtD1GAJ5boaUp5POXxbKFTvZhzPy9%2BlMnED%2B3z1VGKa5L3%2B1pzCt3NtUdcQlhXKeU3XMMS2%2BkOYDRB9Nvq39fUi67AHZNLG%2FCER4lwP%2FgH5mekejpRPhuBorum7FIsWYfvTbtioqdc2FhYBdnol8vfiMggoO9TYYpUlispVZSoF9JyMzvma14YTXzUhI32ImMUltT1ooMW39er7a6Hpzl0imiP&X-Amz-Signature=1a608f765a2869e37aa27c2ee4288532d8f85626ee2263a8d533dde54f6e5a7f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X566ZZ2S%2F20261027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261027T025237Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJGMEQCIHqdXU%2BpJg4nKO0bLNEph2P%2FsgpIS7DFWZMowTcj%2Bkh%2BAiBx3rbXODZL1XvEpf%2BFh%2FsZ8t8%2BRzbK8xZj%2FT5d866ZySqIBAji%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWa4RWri81ngJYTsDKtwDKluweRdm0vww96oqJj6wvB6d9zABXItfDaK2pwmgU5%2FFVzMnPBoEOyF8pJiJmQLywEmNFdXQchXX2X%2FwmPpPoruHG%2BnBBGenc32lRn0nX7GJL8VPoNVR31Rl0FkldN7Em9QpG9NHbWnC7NvRl5VCTw51T2SsC10WQees5BURUvYKuZU7NxZWkqpLIKdgf5NUOKuUvUh1htcOHgPz2fd8Rr0aXgaH7C6kA7Niwphl6LV6EUHWMthuvg2LaonLeP5Xzp%2Fj9VnMYRZGalMWNuZupwrhQfq5nyZHIDS%2B78vvjDtnxRwatttcsshrU0pWLFWi9FYwO93G5dXo6ebqcwLKzpXmK7sMdgQ5OeiQPKRMC4G5o22O39cSSk%2F3T3PUtKNgED%2Bo8oHSQmvaGq0E9exG5g1KtA%2Fgxfl2c7D6%2FmgVLP0Jj2J%2Bqf6pE77qz1vEqkPmgSYcBMZXLM5unQGFMqRAmaKkUQ5n4bNAjUgiX7BQPyC4IXcqAK88IpAX1cay%2FJ%2Fgptu9kydcLc2UfczTukatvm5kcacdlRZCgOzCj6EFrbNbkKz%2BiJ9mNaU3lTEVfcSG7keQSgHYiRn%2B0cTovx%2BSpMnsXAe%2F4smrxl7IpyY%2FPh4W8xsci4rkxTmQipUwnPf%2F1gY6pgFupwTNxpftcqk5dEpdaHlmmffZuHwQ1VM3ywBB9uuDWroiRjykTxReNMjBhBFAWyaKjyPbqubvFrIuIZOAwwWI6EYKkoJ%2FDwRPIv5X7g1qzEafVJT%2Fidx5ZB%2BrrWDahfjGiSOuZzplbOHcycB8Q88YMP7yGbUEouyKkpbuQBe7nF0G8nmkAFXZUDyGT3D%2BvjSkD%2FQSilmYCbS5QL7Esgz2a5Em2LbS&X-Amz-Signature=4e620bc631548ddfba8fa84de7c0c98c119fc73c167cc4a8b871b1f84d3380b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
