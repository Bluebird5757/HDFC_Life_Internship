---
notion_id: 3c84fa76-9938-8047-b507-c8020ca82aed
notion_url: https://app.notion.com/p/RAG-3c84fa7699388047b507c8020ca82aed
title: RAG
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-30T08:00:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-09-11T02:01:23.961Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWSNJZJR%2F20260911%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260911T020119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAe8DjEU07mftICnV3OHo3Yr4s508A%2FKWdXmQ%2BfYiEApAiEA9Q3kVk5x5xvVefp%2Bjv%2FJuShjPOjjBHKEN%2Fh1JOSP%2B5gqiAQIk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPQ8zmRgVCer6g4%2B3CrcA3Mdgac%2FGE8Lw%2FY1%2B4E4Wt3ATuzVnq7S4zoz3fyBEtir6z2dDrXaCRmEIOHqnQHDy7FqTGwik%2B9hGCwEaOYwAd1TUzyHNt0OTp7MgcIARP1ZLw73BYYYwMW%2BJ2sf8J1%2BZBK25bsXY8TBSZdIAapr%2BxEsH%2B7J%2BjIEJO7uFTaB70VkepjvywXdo%2BM03M3qcONYmARnUqfCY%2FBY%2FsAMXypP383dxZ2yquVm04WFXRQHn3SflSmPKMPoPHxXuR%2BmADEWsmhKmn5V3Xiwqwlt%2FzVL%2BEWD7gGsJ4%2BL7H5ms74Ey34hzY3dH86lC113H0jbv55Ku2PF511aW8hyK1PMl7gszI5%2F8jPHYZBHBHXPKFGHTYzNddP4O5Sh%2BSH5nJs6roZ36dLeEZDWGDDFiucI52Uy5P9300U8HGnmGjFH5n9gN2oPp%2BPaWtG0kCCBN69Okv7gA3qD6IBmgFxNBGxh8YtD8XajZanhJ3%2F0CKLVs1rs0FwTtUFfx0HQ348kwboCgDYRSrl4bHwGUBXqNDyPZXszhIYZBLQ4Ptx9eyWOxMIZ0KB5Y5h6%2B7JkLDJPSzZrFRBg9O02BItwYN1ZdgsH%2F7t22%2BJ9H%2B%2FdkPHXyTSyQvWWNWJD6NFZvevG4VELNx1aMKG8jdUGOqUB9KYiEMlTs92S5WPPZvE8a6raZKL3ryut%2Fx75qCUyT3pNOsnaYiBrrPwa9qlKI8MkuhQpCJpBcgCdTXCnnA9wBmPgb1LQoymYMVT4utdxUawK61eS%2BZzr6SXaGtM2T2XeNO82h1kTb2icSTyh0TOTKH%2FapcBHPb6u8XI0KTZ7Ft7pMeQHqB3R0uPgU%2Fik%2FbKYT9Vt85Y3dhTxWUsg3n8%2BqlNzYMBg&X-Amz-Signature=627f7c70c54fe956c9965625cfb24d08188e59982c79e19df5f9bfd447d47182&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662K62WGIT%2F20260911%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260911T020118Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC36UZlIQDO7G%2BvWyJuoZt9BSmkzAqVhfCaQkgZX0f3HAIhANow7ck0NsJaIeWfKliw5QHFC%2FzNcg%2BrG9prAQUOHGaiKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxKqwFWtmv0vH2zvc4q3APaO9Dm3g4VHeA1nah%2FUIqfjrcDn42RJI7BfWoPYgP1cmNuBOwCRvrn4cQbvhbo9geAxF8ASXjzgl723nscYiL9XbrJgEGFy%2B3Pv%2FhDtwlHa8LXU7fxhw%2FfibhAbdDd0JLrPBiXyI2QsUgjUl4lJPTX3JRhsyWgy6Vr1dnfSevUNEhdIa5BQJoFFoNFp53AbR9EBdCrrtvCw%2FdePlbHoIyGtKPEESlxnr%2BGynfsNvJUzl%2BGr92eWrt6jVOS%2BLi5GwGJerc3V%2BZ0z7hppH5W5JWPKIBUS9F%2FaiZdBibnjSYrUIaxJcAfPboKnkJCmCCtYYJ6tfno5eGE3hiwdS74GAbXexBDI3i241EnWa8piYpGFwzGBZBQlJGpPcu4aRf%2Fx%2BzBIWfz1NqoiI%2BrirZKCGc%2F8HbspMioHU8TCrIXpOZLPqIwvyJSgn8esZsTaPvGJWVGXYYSsqMB1ZBf2q%2FOOLG5QeH%2FAfOyDCRQa6eQe56r5o4KSG8WkBY7tZLZBlc2ocXc7dDo9uooMjTyQkNbU2ZpWTBCRIse4dABEfHoG4reL1c8z7b58O3zsK%2Brq6Whn4HQD0whA1v0My35CQXwj2zNGlnBLYyZjDD7qyx7CXIKO%2BEcTskbqowK7ggr8TDcrI3VBjqkAV88Y5p%2FPWR14hZ3meOcmgXhQSo2LYM8F5xc32r5vN5To%2BWUqVjjqTEFneDuQ6qcvs2PPDujvfthp2U4rj6UkMaZgN83D7yEMHVbajizRcLh7L%2F8XvCtuamvLCvlu83t0SYiQA9pnudzHKNvKlN1fRyUvXqtoP2uM9YJznZkCEfPaSugo6WXMhvGk71IqlNNPbb7mc6dLWMAscZpcFakE7HCT8pw&X-Amz-Signature=0d8b4e558a5a7feb6e8092143381d3a38432c81c7837a9679c11bff7f8509233&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


        Database:- To the store if add features such as Distributed Architecture, ACID Transactions, Concurrency etc (eg:- Milvus, Qdrant, Weaviate, Pinecone)


        Every Database is a store but every store is not database


        ### Vector Stores in LangChain

        - Supported Stores:- Integration can be done with many stores such as FAISS, Pinecone, Chroma
        - Common Interface:- A uniform Vector Store API lets you swap out backend

        ### Chroma Vector Store


        It is lightweight, open source, for local dev and small to medium scale production needs


        Chroma Hierarchy


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHSJ3KDT%2F20260911%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260911T020120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEE6vpfY7qrVs4oz8uC4iKxjnM8VBlhvPWxkiz8B7Z80AiBnmr7IC%2BrPerEI3IEjUnVlR4iXm%2BoTzaPDTf6hSAuDPyqIBAiS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMuh76qKZ9dGTOTVlZKtwDiD9z2du45AEFVDKbpgVVSGZbD7cAX96QOkYEOvSbyio8yXBy8TKmmNbSaX%2BxsioZWJEE0%2BZgafKvxE5aum1HZcvvNJKszHXDLyaabJB5FAWJ8fKxyRjOghP8HmKXqL4kM6Et5%2BiSsDMbtviEEsNzunKdzkhgcl0mGUecwM%2B%2FCBDsDIYCaqdh3nF5sWoS0TVB%2FCWr%2FIx1Sb0cCVBhv03htHcfWEJWLpekyMNAI8qp4P6i2D8RjmFFDyJ3kEAg2STXeQBTn5H6NJOs9LIgeI4uZIn4zYwLhOvGxtpRcmWiSdxd0tSV8m5xXn2%2Fl02vT3ZNaZCRTULhSIIJFCFLxjE%2BH%2FPVhEPwGx258AV1WroYAqTbRCuaMTtPJRYxXbLWJoLBBhRuiZaIc2kZL6DnParABXGO6x3SKaweXqmewfyD1EZU3TCL0V1cTkppg6FY1TZuiSPVL5X1lRtpCnw2TBxHMsm8HWRxVGJfkuh63qhzrdeFYOyc0GHmqXMjvTpdsnnu0lVfi83JRmrJT7KO%2FBbfDt2eLhJCgFMWwWIJgC7B96g6w6kGPyODJFr6ScZ%2F1ap3YJ%2FZ04axnE%2BuOCljrsruOOZw54Y6%2BU03EK%2Bb1O8EmGuJqG%2BDq7Cbc%2FgCM7Uwpq6N1QY6pgGpAlYnlnCczp6%2FjF0paUGUzsaPDFiLrchZfWiQCv9OUjmq2C5EnsDp6qcL3V%2BIx84egYhfPwPkXBfGEU%2FwxPf8aAN3RG8%2B7Yq%2F7jcPu3ys0UMIWuLZH%2FPCHajgCsQbG5xG4EY3BJsclrOT48DKUlBZAPHL5pKDJoUNFs6cLShAmfG%2FhnKOBCLJcCbKu%2FO6FpYAJyf%2Bc2f8eLGdS47JX3CG1Vl%2BIWT3&X-Amz-Signature=00ac7901926aad53bc75da3bf20f66faba87c423236694703caad9c6ced51642&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
