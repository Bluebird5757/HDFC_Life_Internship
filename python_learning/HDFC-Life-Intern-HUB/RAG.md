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
fetched_at: '2026-10-18T02:05:01.952Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W6D7KRX2%2F20261018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261018T020457Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQC5MaD7SS9dBfMN90v2izx%2BsXiaM2Lcox221xq8FypzQAIgElQr3bKLDmzCPrpHwDjldb947IPhsxOoI%2FxQWqLUGnQq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDGssoYCzgtUfbw3brSrcA8WMyv5gSYWugqiC3IGrVC2zB8ybTfmRrj7S8s1bB7MwQfDOH67ooQingo3zfAzrY9zhWvdYdR%2BL1jayXYMisxtarw%2FrqGm2mh7xkUtCwp4t0xELfofPuT%2BtBOrcN%2Bo7zdYWC9XMT4O6ePaH9Voas1WcsYarV%2BcLPrZ48UVA0cXmdJD5wFESUR07s9keJgSXP4WuOIEb%2BNbVoQc55MjWN531zwUSwKYmr2KPJh4skN4pepOnKDioPv8GhBLb0DZTgTguab%2BhbjTvZGdo6sCie8xZIgNdqLSA12SFbrj%2BY%2BOmVFCNTg6opsZ9%2BKJfMrRNZ4imvuHlAO1ojExmeX442bP3FS1YvVgEYn7mSX%2B%2FcujKAd7sEKF5nWaGqaR9q%2FNvlQPMcY1ZqS4Ns83dHhR9%2BG16qrew2isqlvdvpMR22XkC6huhUhRJW5N2BVxqyMJjNwC9ZRncEzwihZ%2BtbQZMfQLyOzK1Sk2a0mSmS3vpUrW8tVyhD%2B1dka6w2uRlJYS0n466dQI4HhSKJK3PHWQviMfPzwN%2BnzpuTn%2FfhRtWPiQPRmKMH5wNLpJvIuIfwGyQo%2BIFvsKc29KFDwNaA6L1b8Gq3lGcJZKCwkjyN1cWlyHxeFjNeNzccvOfs%2F6DMPGh0NYGOqUBXi%2FK%2FbgYwOYm9qH2E517heRD4ycgKl9u6qR%2BEatir3qQzm46jjgrtQMCTZbTg711RWX7%2FfMRLK7%2BrJdBoNiH41zO7n83%2FjC1FeFekH8m5ytnI%2B8u2oW96QpqE6BxLW6IT1IBHcnTnPDDTOqJE%2BcF5rfHgfQrjghGjpHqSNbtH7DCzCXcydrQ%2FAni6kiJnRSh4XP9TrbTekwa1DyYaRVCtZ5F0gPM&X-Amz-Signature=32ce1e2391ec10bdc34b71f6916896df489df310ea4a830cb8f459fe840d2d75&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667FIAQWLB%2F20261018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261018T020456Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIFHS7k5%2BD0tBLy%2FPpCWYSW%2BdRQoW6xj3WvUqYTaIUJfiAiB55cpPSFFDpXRor4yuRpTqeRj7evnoVVB7CzFkPfbH3Sr%2FAwgJEAAaDDYzNzQyMzE4MzgwNSIMWMVASFr0Eh%2B0DJh9KtwD2JuRo3GdlIa3BMEyCJjx6QCTlOAwpIRhoCwKebmXZMPpPs1zPgV6fw4OFC0cSbl7aoEwM22qcKxWRLEgrwZlbu57KTDIl9UHmlZMI8j7mgKr1EeJFTsM844G8NlEthLyzT32SMyRtsKsPA%2FO%2B1hF48q8AXJegE5RiYMQ0wa1%2FGsBJ1tUd%2B9FB38AWQFnkqQ1f45u%2BkNbtQuXhDLMpMMAxHdLpoJVxyPFomFrmD3ZJ8NRL%2FJJT4VzcmnKgzAF99OyUWSr9MEInu87Px6I1hvDhX%2BUytpc%2BfgSXqazb80IaCo8Q0C9bMtLt2uwfOCACYNcMGVA183bVqFhQ3ut2V0elC4HJjxplprp7pVFiVhlPhHxm2YVFD6orr4Dv1w2sKlB%2BafTyJwSegmGv%2F969ZExIUQdQVat7TsKxm5WzGT0nBzi3NvVKON1BzLrG8oHf1zFTtV1E2gPtmtdqT1ZXTedd5l1NIzzlWU0syV%2FtyqcW%2BJDl17VuIeHsyPoaRdXWn87RRK4wCT7aSZCdunq9H2jjHQaYjiJpXIArqSTvFBNTJnFcSTMBA6Ib6%2F2e2UlSPbvlQFcW1tY%2BDiAm43Z3e56KFAJeGPpC%2Bklp%2FPdVu%2BvKQu2JIsN883c6vwkmF4wsqDQ1gY6pgGtmEQyqihM%2BAsv%2FYHfgGOmwTQ%2Fh6vP%2Bmrd85np7mI3NdGGvIZxkjqs3CIp1frW7TJhYZJqkzxItmKx7eHlV3AiH8CalyznW3DoA1CkzA0Aeqgmv%2FRqAZZ0a2P%2BZ6sEdqj3xYwbAyP8SFzesQywPmXi31PW9C9fVJu5eGZhBmXJTvGDWWZMw9Nm1tYtFNSgCJS5CEGEBnAOz4UOLqTl11P8QQJsnPzz&X-Amz-Signature=260c09938c79e8e9feaf95fa629a7178053c52d2b7d731a74c98c6467e598d32&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SHHN66C7%2F20261018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261018T020457Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJIMEYCIQD0NaZAJ9A5onIivUAi51F%2FFF66WeDpl0np%2F0Sc38OswwIhAId%2BnhKx99lyPmGiUn7yigxB0eshqJxXjN45XR4AdXWrKv8DCAkQABoMNjM3NDIzMTgzODA1IgwyZziIuNFzke5hckUq3APvcSKUz12YzO8jMUEgHJ%2Bdx2233InE6TyDLf%2FPYN2jd78CiBVWY%2BtVjtmwRZXuGBL6FsB0vCP0r1luZNM901haBZJsrbMo%2FaiYOqu140wkDoS0lkFpm%2BdmgV8AYbcbPEQ%2FGo8zR5afDHaYnunLQuKx7SXrRJOB7%2FXGrT5Go2KlFJ6FtA7bZO0W0%2FWfFU48kACzL98Uwg5WTsGk5T7fu8tiDvvjeg95dfFTUZuhnDgn7IiOCW5f2BOXRP5qGlEF%2B4S8xZIFvImFTjx0YcOyN70zhHSuQ%2F8oMoCUSOvjhZr1nrk%2BuSOQg%2Bm2GG1zU3e0Mm0BKb2nJ0uncleBU%2FG6MSaTC%2FLkrM%2F621WNeKHOYGm07xOtFMOSb8QZolm7QeaYKoEEWa5AmPv2GnNwSK3QLMtzCAaoJVkJpCksNngohlsTdwEz5icXYtKnUkwaHe0g1xByWFKpGINEQs61IY4mGHyanByqYyasTpFcLYb%2FZpSjexptsu9MUFhG7a6VGEM%2FLiADlcpEQxZKGid1AdM0opw1woOcpKXIOMpDaEPLVLHuLcj3LpQ3cUmwpoh2pL26YPzobXr8uYHX58bEz%2Fxc%2Bim3%2BBLYS6coPdubf%2F8EdO%2BHRtvTIeiei9eMdqmKWjDnodDWBjqkAbb2SXmCoLsmwRf3WyCXE%2BAW27tbRYR9WD%2By3MLZodS8uooFJGJj1LnYaK%2FnjDhQhZ1A0zcBZ52MdQKp1NaBeCoYONus4vGt9X5uFlKcqsrnjgN%2BxL%2FA4HzwoveM3BkoobnMvLNfFF07fnBmyRGodwvJr%2FYWUzRh%2BRjsPKXpPU2me9z%2BmLH5OJ3U4sPK%2FTH0YAeC7XA6KvRG0JtRBeeADClQjeX8&X-Amz-Signature=6d7e8653c54e10709b80012d803dcd6b10fa0fa287711e1a61d036162c68449f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
