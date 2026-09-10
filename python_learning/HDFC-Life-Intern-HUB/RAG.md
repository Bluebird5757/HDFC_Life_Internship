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
fetched_at: '2026-09-10T02:03:34.808Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7ESXGSZ%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020331Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCl0MU9vyyhAA0jIp2vMGU4UrEqKYzxE4xPz5F16UBfdwIhAJGhzC2exGL9GKKrt4Q%2B3WmXNzVe5nYjpVOc8HTKkTakKv8DCHsQABoMNjM3NDIzMTgzODA1IgwtWehsdb9ii%2Brfp00q3AP8V1AG6MslxWSlTADvg03bA1Pq51sNpgxK5oGWW9qKN0pbk%2FYb%2BiIR6NScd0D6HPejKS2TpJAviA8EinkTHSHG792fayRGrIYid4DSE1QiwwSM1JrgpwfKpxCojq%2B8C6rLj0strctt8J7bblnaOw30xtC35jd7ePS6slsW2QMcVdwdXA%2BV7QZHU9dZYKwjTWc9KOjwG4rqRJZb6%2FaXL0EUzK0g563nIQ7aeuLUSgUAJhzLlJtPft%2F5Ih9rU1Av5rq02pTFRJXLxkBdWRDXuiOoKh705U1e9yOAP2qeO9tn5iNC3ONilPBtaTNdM8lljPPtarQwO8gImfswQp%2F2Jo7speIP%2BMJEoRjYs6T0ndaLv5e02aCHrsFQ4j6PPwJJdnR16Saz7Rse43fQEXi2lCjRaOIUZuJoCf7M8wlFoF1HqHjJoDnruugDlWT9aSgxY914ccMNG1VNY2%2FXnmFa95%2BLQToApjnoHoXokSx53NhMd66gslSjLtF%2F1lhYEWOlD1mhygIoaSQWss13mT8g19gtYiyOpz5kh8q3rv6fiyTIK4uReJsKxZp8kT6JxK%2BKFuvOkbUrl26adHGY%2BIyyKmT8yS3oOuAwp8%2FMOzl7NaMfXJvsuOupglK8ZUhSRjC3kojVBjqkAdC%2FtbJRIi1%2BNNBZ%2B9zU6%2BcoJVTUhfaHOWMgzaI1IsK%2B5LcRs7WYq5A%2By8uiqXOpage3G66IItPcMMD0dEdf6KsZZBEWIjWNRzvkU8CmEFhX8gJ8CZvkwJuk7n4oBxj2ppu%2BbBTYyGLOWl0DzEw8y1vIjpn3iP4t83qOk4CJbHmPyxNfz4SDvCD%2BPYSFKiNMFPhx8sfmB9GmrXC5qMfp0z46nETd&X-Amz-Signature=8335fb1e69290096a048a3d6d825edead8c2489dc47fc61a87e5d4b95b6d6353&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XM4SOWF6%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020331Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCdjMcUMEtApu7e%2BC37t87RSeIJcIU3StL52BxIhJ%2BLMQIgKfoijd27Pwk9XCez8SZSZeU0GgU5kBWSf%2FrNui9SIKgq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDKycuFDcFhuvfibvsyrcAwYNt2BzfOfQt%2F6wbYfvFua%2FqrD%2FqrEGwQyTkYdsNRvLdGXLSeVT9Z90H5LVjrxMFmiA6Ezlgus0mb0gQn%2BrjC%2F3cCukp37eOs9A0rSLh0nepmJCNs4qPAmJNhvDublWBe3tTftjh%2BRD3fztQ%2FvVJgBui%2Fja2JtekJgChjnX4yzGvlVnlJIXqv3wiKmO5uJqWRdW24IohspVq2crXhz1xvzSfhwV03l%2FGSA%2Fi1sSm1mEAFaO57kDtryXj8CWrF5vSRgt3ISJneI%2BPcP5Da7ptSUqc9V%2BaIazwliJ2Haff%2F2GmlBEicgw3Gw5bVhV5ST19ZoZefnmFK4uRpn8yMb%2FhYO8G9ifnPqoBj6RMW7BeYXrkAupmPxphD1%2Fq4SZ7HuvqEtSXRc1j4wsrvMwWvpCoXK1gIOGkQ%2BT2Ana8PtKhzDAo0U16QLPbvdGeIjdju4%2BKwO9R5XdexFGVg3RufWZMBqeE14EKjfDxUYZUkA6NuplDf5W2hcQPDq9Y0T1QjzfP1G6GleGJjLUVWvBNTWn7hsC0h6uZeIyfq8k%2FpKC1aYvcjpURXAngh0W1E6VdPD%2BP4yPjBL06xU7%2Fzdot3Shfd5uECA%2BbyhlPWnROGydDC8ev6ERSZno5V3qX9cqMLuQiNUGOqUBbTUFo9n2xfIDC9UUVlhVqrVlvInWYXpEpnZKDINdMAzSymgqzIGucsSu35HlzD0tymyouaLZQHek4VhBedIAbZUfBLqB2pS5d10Th1VDtnpg6ZrmdhAV2w8Qf9u%2F2%2BhAvme6NiDahm4iUI2Y%2BCico8xduZbXlsPbQHMDCZsxRpQTCTdwN7Sp%2FJmcAqUEJOrmxaBRP7af0pimopsyi24dtQcSFABx&X-Amz-Signature=bd0e7575933d6eadc8562429158e849a17af6a0d638c783a8bc976129ed4a8a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662LBHWFJS%2F20260910%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260910T020332Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDMUbcFEotU5X8rPJ4MP%2BCjVOmagbQLszgY8gAGQRlijQIgJszNCAqhDv7lF44kIN6QD%2FYyJStNjsJtrFfQSqgiOgYq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDB3odoVN12Y0cCMYvCrcA2Y%2FHHDz97DXCZBop%2BFmy689nafMrhvIXAG3DeNqU%2Fukrjqvdd6SUIi%2FkSNDaHpxkWTkCsyonmFiyOAjXj3mnM8J%2FaReUQvZOOFTTt9iU%2FluejvGbZsUknzhJtRTURfqKx3DWFwQf6jPjieXSLVNbsx4vWn71tzNgxj96rHwar3VqF28xPnyqN%2Felgp5KqrW%2BHfVrTYxQcW5a%2BSv8ilwE9zwYIU1MTjriY0lUsTj35298cs2qCB67kGA4hhm1g7wawuiEkN62mJeeotJc6gmxszmS6mVxTrMsbH4F1%2FM1VulDvq4zcGy0z8UjWlaDdVBPaJlGWGxW2xYZNi%2Ft1XC%2B6lnn0WIrY0r6c535cGJ3Klk9KnttyJF%2FHy6TCU5bUAnOmVIcFOUA2OtbVgw1Qx3LqMJYe2EgJSXtu%2BJoyFvkjHI4eBIjzLI9aEe%2By2GUtYXoUqF%2BZcz5uI1e9bJO9EtvpRcfehQKK1Ua6bbq9URl9bDfze5G40akGzTvhMm6fMDeeLSQlbZcpsoqW49FW4RBQ7SFqB5YCnzogniV4Logse7AIJIhLhsVvPW%2FsmQ5BdSVSSBdTkz84hy6An6vSBVIfNV%2F%2Bexj7tSFVbNZAqLc9Kwu3mwF2kF8ZS0cR1NML%2BQiNUGOqUBuSiT15DHhLqNuHE94tk9Xo3Ut9xTxYUsDj1Y%2Fb8Iu%2FqS2HVhbD%2FPaVpxEWJ3qX4CnLBJ0DcPaOq2dpzKqKNYI5%2BHK8hhfgnQaPHRYNGChBN8pb79TRM3FvwvcSkFtAPJXENrChuOIG5Hpb7z7QAG9sh8vSfgcw9XAOam4ED%2FbENJgvdgzYfVlDAUQGMcfDnuW5MiJYFv31AQ16KZKHDWktPGE%2FJn&X-Amz-Signature=d57b197a4752e9d7427bccd8f5bd6a594173792fa603e55f4a1e7e7e23d62f0a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
