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
fetched_at: '2026-11-14T04:00:32.730Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TKHV76D%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040029Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAEgvUGHG6Q507Y4xhbNYOCk%2FGiO%2BwmY1W%2BXloym9JFUAiAP8cGqdFkUXROHLvX9qQ8dPEyCjVFTpMTQwqhsUxEonCqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM5WhlG4o3cHNQcwMiKtwD9TLW0Bpw79wvG8mSZsAk%2BOn6sRiqKuN0Dpet1Uo0ljysqB%2FHhtyUVkLEIiv33l4KP598InH2%2FVdK6QgPk67jCA9zNzixb2jsEowGtZ2ILbeSpyKHQ3FMHIOzhyoAz6VmMXUXsOvvoyIf9XtDiDbVJkrAK7EPmloGxzEdMIdExhUoEJq5rx%2FX9f7A5I7GsmxiVOqkkg4Z1QVREx0kGXWWzh3YcJFu8kuLTlJFq8yYX4x0R4r9Vcydr8s7%2BX%2B1AwChr36R345xYncgo9U9K2hTxIYKeFIViL2xd%2B2Jp0yQ0Xs0zCPRPEdaqaYA5euB9w7awM8smsLaryFBr1SsuewnHlUQjKrtMNmCp%2BxbUzpxTgl26cc9p2YqaZXcJLen6b5ccexZ%2FUGzXneImHI2GQCfqic4%2F%2FEQm2nMDe5%2FeyYIRF7mtwCMYWpOMVBkiQ0zhEdKAGLtBN%2Buc3YYq5X5wMj%2BZL98bwZKtHa5EkyO5rqU81OwOkxEteZ6lnBojF4cFzYx9SWMrLXADF3hUZOE1XbhMlsn38s9wpzY6fMNet%2FKADgdm3GYfeK7gcbfsVAp3kDIPw7GMBJfMiNiSbzVRlLwwiHPBqDtsVRZOEJTb3TQXf3WXLQ1GtyG%2F22UbBgwydze1wY6pgGRBp8Uhui8%2BXjrzt29P91iMmkGi6HeLRYN7FfJSlrPQpfnyt6PVFTJQA6rvNZpCBi3ZC3B4r1akUGiQDtSVNFN1IZsz2NJFbVrZCYHaueVMAhya5TrviaJNyySWQP1%2FXTWuzGMDW60gK9NJtZbPz6M%2FQaH0xYfPyBmFY1QDk8QIQgN64inINZbEOfn%2F%2BGLky7LYMn8feMYcGElloHwH8tKBOxxb5Pa&X-Amz-Signature=6633581395a35dbdccba4d8e00f90ec200fbdcc9a6ce2d9d4d5ad67d02cb7bf3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VPKC2UG6%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040028Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDQP3rFAKKRTtYKi%2FEE%2BZqyJ0dmnEmD5jYX%2BF4jr5%2F%2FIwIhAMO1HL3%2BBlB%2FbOZWbgTJlI5Xb19KTp%2FfKN8LUF1eBVw%2FKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxoAuv1hW91UYgz4Pwq3APhrfwZ%2B7M5uKsvZW2tCKgu%2Bwrox%2Ba%2BtuOY%2FyAJgExmiyEHK0z401pZvspuBHFQfMWvhygJZXYOfjF8TowRsgW2w0xX0WKoSEin9XEug7iAzv8kazx3rFF8pRiSnzzt7Sr0O%2B3H5levfWqysc8uOeLRqWlDxvO3%2BXdnsVmUBToZe1zdjLnEir%2BciL%2FHQLTHDeJER24yuX4MaP9PZqW0tvqZlP5tX7x1%2BqI3nmcN3FmDEOhKlAW21cmjd5NXiBtTA%2Fc3dhn4ReBAprYdx%2Fsy2Y2f5wom6k0YPq%2FYLuULRAgujxuzN%2BVAS%2FFmpZD97zXvEUzOOJNdWZwrtWsX45F30J2G9cXJifqbnoIsVSX3HwavL%2FuOOzP4ABWCSoKNSZGxXLtSL24rTXKF9FZBYDVEJQW7JxoogdwdCyvYcktS6NUJ7ZNfj0sSoXlDwYlQW8yUp7Nvunqkw8t%2B8yRVmHHyUx3cQ61kridr21YvxFKMxkcKXkySK8RePJ%2BCI90gcNReLsf4naPO7384ylTmoI1jtMemn4P4nx7%2BrV66jONaKcNNEVAPYyRQS8veE84xRVQJAxxjz1%2BwRiTbkuKKx1C90Pp3382ZgmHgZ8ADYUPXC2PV6zgUPQqRL6SQMhdgNzDG3N7XBjqkAb7knnfas%2B3ggMIT8nI7WNkSF%2FMCj5XOEz%2FcqLkC7t4uD9WK9grh5Hw5znpXXHOb8B1dkBhv5kf%2F%2BLwgZ%2BIlWxgFDPJwLNbqdZG9U76vlI%2Fk6PW2e2fqI5f4nRBlnoDbNPn8Xao7O123jPPOZ1zcuvDPz728nM%2F8yvZ%2FW2k7Kh31XClizq4lHriazNbl%2BE19zF%2FcLJU3D33jZZ%2FL%2BMlnGue2T8fu&X-Amz-Signature=bc4550475b8faaad2a46196eed6ee41f36b8efeff01c1eaf33cc5eba3d17c1d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XYFYMOWX%2F20261114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261114T040030Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRVufeFG78zsezS8i8y7kgUV5ja9L2M037rSsI6YT9SgIhALoqEOBCgv147wyDh8wOrkGOXrtZmtl96GVJZvgPdAycKogECJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwAzNFV%2Fkmg7odOc3Iq3AOEXJfH%2FOrH5I1RCMvoqwEoYdVBx%2B%2Fdn4SfUOYlJlhBiawQWQxuJsibW9V7542jYNCs4xbx1GHCYq8nL5lv0Z1UuP5g902YoWiprOnEbq%2FK%2BrDLrICic6nA7rbc7vdhvL4s7yMHIi83bqiu%2B7cgpbZdbWA7WXuMzbPbDA2uAWn43prReh6zoqKheH5bw%2BjniskAxnj1FMJMqIYMx9VTq%2Bzq7aK%2FHcVQlOBSaNs6wSDxalVaJpjCNBrmO2c5YosLBBYdI%2BxhCjIJ4vJDaSCfj%2FCOWbzDKQOlqkrVl0BWocGkd%2B%2BvzckbofQvGXtxfyzGNBumC7MjrROl5jxVjJM%2FLiQbgYUxfHA1fSKjJ76%2FmTxw70LrxvAD0eA7iU6ggcX6DZCWzMlVBKROy%2FK5rpWXGGFiatNmuYdSWT9AYKzMzBpN745vRLe%2BO15W4SA8%2FJif3OCp%2FYhhzTVMAQB5SvxEEK1dRyrHJZTYnh0BrwQh%2F6uQzZSY%2F1JHFVe%2FQgNQPaDN4dgUYX43Y%2BRlG0XFZie4Y0l%2FU%2FF%2BVr65hK1gQSPnSvMl%2Fy46KoCu9mLRef0v7FW3yUh%2FF3GbLJV61Q4Ap2IfLYnEyzdrmeZD4X8cqgnrx2Jzsac%2B%2BMSfsxBc6mPVjDDc3t7XBjqkAbQRhYgKRTXP04XQTxgvW1E%2Fs7AoqYH4mSCKl7GR8kDbhGtTgmGpOmkxtKYQmfPxvpWmSpUQkvlMBWziH2GBaV%2Bn5j6KfrivpoP1M%2FO35ywW30dzPj35r1cGdAbwTQjT0IBaBZrJnYzNysgA5wDwr2JNkwBzsSGSItRMg1RfFScJMkWikTHPGg5hHGQD7fHd9jODOlwQC9XY%2BpoSJH%2B5cxp%2F5Tb5&X-Amz-Signature=2aa56803f4b2b32d67126a5df6b4b88c0821a4bca1d927ab82fe5f09799d5736&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
