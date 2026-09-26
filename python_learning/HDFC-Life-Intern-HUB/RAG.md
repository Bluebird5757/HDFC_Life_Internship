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
fetched_at: '2026-09-26T02:31:25.984Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XFWFCTA3%2F20260926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260926T023122Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCICtAVrDvRRULbrwewrtH4Ov3tBcsFICsoW%2FFpKbCYDq5AiAmM%2F4LmO6RE02D2%2BLCHyOU1MN2GHbpoDNGahSng0tHxiqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcOnANbMFO9cRsDEUKtwDdriVfocIutrLmeqzhn4KMs0OJpMGhJ05QxGdCOAyh%2B4wMiuE6iBfAOlZjdyNlP0kBsg7U2fJZY50C0BZpEpbl3G00mz0orBeNF%2F97gXJ5xcmKt23%2BDzPboTUIWSF5egsxXmF6ulgD0t2uAoPFSMUr%2Bz9%2FdXcoE%2F8qn0xEgH96mM3SJ0uDjBXC%2F0SCe%2FT8eTbuDEGME8CXZmblSSsUpv%2B6KDvYhaVRvpZ6wwU4YHytWbDyWuow5MH12d7jd0jVWsMxOYNDJObzXulLMpRa3cS%2F2OfHttq4Vsv%2Br5QXUiNssJG1EYv1i7Oa6oar24aPcaUatbLnn%2BbhXpAdcW57X78W7Cddvxc51F3L8Rr5MsWnaewpykY%2F7f%2FaRYjzu1%2FjnLGAzLByMdZHQYf8MpMedGAVmbXQIUNTJhxDWq4CnQaKUQeaLPAr25Tnp%2F2R0OASd2xpMlCGLE7EMCzZIYh7qudq9gEAxC1hEFznckioBmll7wwqflU4vJEeDP29YE1EF8zgSmjwBYzWkI3FWjHJbKiJyvyx%2Fje0OD%2F7vtdUjTHBtrM%2BMwi0IoPi7jZjVAou5pQBTa1GwxaDICS0w3Jsp4rKlrSPv50vXuldonQrnd1A9F42W4qFwy1QX3jvvgw%2FKnc1QY6pgFRstafFrJWVPo%2BZW%2BYD8m%2BN5Ujml5HbKKrQKKFp%2BJBvo5%2Fm7exEqzQXQhx49jUwB62c1wty3qELoYvClTEvYs3CaF4LGheT7NfV9GHI9n2xxSt%2BsYvA1OkhVMMQU7vr%2B3qmmN30XNA0spCk9NBtSvR3TmAEqyUoe8JeUwDxaq5Rq%2BJmA2ng7IKR%2F0Jl7PmJ%2FHSXfcWivFOt%2Fsia3u0z69ueFY6WC3L&X-Amz-Signature=e02e5af74b28f475a4afb97feb559f02e0711169ee15168c3de8bf9760a75ea2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662LELQ5S2%2F20260926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260926T023121Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQCQT%2FtqRoygHs4xxAcRQSgUKVOP7VtMcAAgpho045%2F9ugIhAMTiKdVIrCPyEPMdPcrIjkECb0YLo4whaPYaZhgCaor7KogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxY0T4vjaZLT5x%2F8ncq3AM9R5S7%2FqX9wae5Ufv2YV60JCwXFkmaq%2BlZ0HZjjuBlBCgAA7BSEKALM653fmma4Ktf%2BAuGo18LFG3zHgkXoQ9xNGadeW4TOMJq7L6oQjoDsVM2JnUAUWjmT5%2FxTN8Bae82URnUGTpy5WKyo2274p05bdYL41ybf1tGnR4hsmCGf%2FEW3%2FKlnZwaNu0gnwyo%2FzcL7xsR%2FdUd44SaYFAzqrYJH6x2IoJmiEMSRtPIx3mFHp%2FZfCWyp0PH7OF5%2FxI7yPHsGPoaj7QvKZAcWCoCzYTtJFAOSVqXFU2glW9mZRTspXSQwIK3Y5l1P5hJPz6xn6tp0QLJWB5Bha7JTcZtu3ZIfgAWpK%2B%2FQlujA8PmUB%2B2Bx1X2YsxF8E18cbZA%2FIQcawVoRn5o559eYl62SND1SL1LxWJfR7%2BZFCsNbSF5JFIh9eD6R8vy%2BtQYfiah8V93r%2F%2BdPXxV%2Fx4%2FhI%2F2NQ21J7fCzcbDb%2F2RXmky46%2FkoZz096m%2BrGuL%2FKn4ZSPYuMZHW9zb2GlN8LNpOMTp%2BUfGNFbh8l%2B2edkNF0einUfcr3vqMCZEUU4GQvC2tbNZk3OsIIZW3yRuWDsPBvVYUFdAVTivQ2FUgtZ59%2BevTrvCH9mRRUuLjOtO%2B35gGa73jCSqdzVBjqkAf7B3z93Zgm3o%2B%2B6XjbfXleCNsxhYWLELMWTNVe8w%2B7axZwzXOK%2FVa7eY9mq3ZVgwb1as%2FGEZN23Q7WrBJcjSGnPkWRWSg4g%2FG3o37qHAtfDKOnITGygTd0nXC6gQ39fDe%2FTJstWsytR1zvLt8zcQ9aPm1CGGlYN3NLxazpTPQH4F8EYoku2DOQP%2Fyxydle5hOISx4Yrhtn5Yx3nn8%2Br42gfFuUo&X-Amz-Signature=c4f258ddb2e01136dd3b8d629719be2fcb94ac4426bf98101d5ac55411e2f653&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PDFNDBP%2F20260926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260926T023123Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQCXGuJC74p0WxTjkHZzrZBzC5pkUcVuGdFRRaOya9GCFwIhAMfnjJYwgvKAUSgic4wKXhw9elML8DDqDtQlaKY7KqZrKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igze1xON4whORW8vxRgq3AO1S2sWltydeObTNX2pkHirakm3D%2B9E2jsNQkV0qNslKRTnzr40U3KMfle59lI8A5iywhy2VDYRdp5KulvblAwXdIzAKu5ayJ06GufvyQcL8as5tRBJYG8TI0eBV2wEauuN7M1MV5slpM2CcbscqiVw%2FPG%2BDUEryc3VPhG0%2BbMPeqqQZoM8eN08%2BHDiQStkWe%2B%2B8i9bXS%2BqmR%2FcID6D8AQqGPjO1LWZVZivAFbtTs1TP%2BKZ%2FVlisrGJ1g51r%2BkO1AhRIPqXiwAMV1JBmVLH8jsPJ4RB3eWIdwfzzNC886REstLmZCfETZJHtbFybut8gWMtTAtYAHjk2haXfOY6hbRTnZ0YCqb02vhpqd7E5mVOnTI4rXoi7uFzYa3fG8Oop%2B09Sj82KC3NjLl7V%2FkqqQcQ7VWCV64fjtFojhyRuw%2FZcrIxay11SGKNKoQaAcnJE1VHX%2BNUaEwSVHLaVnfm9FK%2Fnx1TBzRL8k1j73wgr2ZOMZLnwZIgKqPbicNXMuFsOG%2Fan7kPsClZqSVFlgSVaLePLUjG9Zh7UZbhqIyRs55sT6XA8UxQ1WnhlUcJv%2FXQyv%2FkumMDAFHPwBDyfQrD9rTvl42FVDl%2FitUVbQAXI9tvQE6i3gi7QwFu4fLyoDCJqtzVBjqkAX12Gl%2BDOM6dT8BCIrnHdKUoRYbIYnVSlxXOjClLPagY2i%2B1y4McvoiELurvNl6IaEeyvf82JSVMV5xLBG8hX5BMZXOZfmO2ghCIHvQg%2BtJ9QHDZOkccaYQF4dWreLpFLst%2F0pjKjoJuaZHaWcXuIwu1MearbLXjYQP4VKuhvxidfGsSjU8jOsyAY5WaCzThopjZo%2BuXzD6U4UdfiCikgRgujfmf&X-Amz-Signature=37c039ba5c587fbdbce3ec2224e433f064ed270084405847e427a8b1751980c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
