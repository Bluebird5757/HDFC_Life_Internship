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
fetched_at: '2026-08-31T02:17:40.541Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664FFV3O4J%2F20260831%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260831T021737Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDrvcmXlLzzrn%2BAAdFjpcs0COxCSG4UJh0gOcOGLZmNSQIhANibqt91AOuE5J%2FRZC2YZ1035F228VPR6wl5rePfjmH%2FKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwlgg0IEwFleU5Enjoq3AMuQ2T5Xpv5hPLvs0Hw9MTk4c6WBZtngMMjbaasVhisQAE2Gb0ycURq%2Bg5lDWEZnIE5oeBK2JT669QYBSwKtz%2BPgo2RQI0qtneGdYB21y2PGEdWtL2amNvk5KS24aOge%2F9O7M5TTgi2%2BMYkAVF5ju73LF3O6zVV6gHhTiSkEXHH69nBKPEtTRF4QMn9TzfeQOau4H%2BxwJWh%2B19P%2BxYI3KGLyup4GoMoavs087lYNED%2FsbxuAdFdXxG46KbE6ltzT%2BGZJ%2FIN22OpkhlL96Q8sh9vYLnAH7TO5ppQuhUDTdUBRFAWTidtdqqvNLZAIojxOHGTfXtmbe9BsqUr9qMqC6QnjPN%2F62CgyCeHv7Yt%2BQv2D6%2BvWoVrBYl7nDn0UxuftQ2uasbMxA0fGkjaQ8TdSAdoam%2B%2BLKfNGTK2LcIKnzjnT0C5SvG3N0VMJQyu0sSYqrbZn1Mb9X8VN1H3HH6pf7P9sXUttoj0UqqyORBaS9bWHPePVk0WIFw7q0M2r7OZN3lf1zzW0eK3C9xXi8%2BTgZQmjTC%2BBnj0Hd98s1ygxrbWzNUR6fc7OKAK53nBLm5U9T1fjcM2DjVAQ0JMvCvC1GMsqAsgjrEhQID1IH3CjiSvXuZ9SpLhr1cFLbAHnDChldPUBjqkAUkj2nkmW5q3WJhSURd16xHj0ceGkf0hEkqljfwIbL6hqpTZJD2tqpnGr6%2FI1p7QlntglYRV8IQ04kQnF3Rga9iqT6Z6%2FPjKXaXOrSZ1I8HdQGV36%2FrvjSIYIRWd%2Fsi2%2FgDqgoiWL0LoZuu0rc85cGxrCV8eJ3TrD17RFoXhIF3PR7iZFm1WbPRC8iVPMcJpVrIj3AD8F8cyfciiNMPiL5JxS%2B%2B%2B&X-Amz-Signature=45072ae483dbc5cca5d89f44173d80866f5581e6fabf4677db9ecbeef1ab81d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662RYJRECT%2F20260831%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260831T021736Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDMkKYRt3WgFvAYoLqdfjj8SDeLk6lV8dLAp12kur%2BPxAiEArG2zSUeNXtLl6aHfnzw6ZIB%2BfFT11FcvcQuYXYk8F4cqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIHbvbJruhcv2fxncircA%2Bh89AzotOCgi2h%2BLZK3bqUB7iGHFD1a1EfTWH6hbMjOFbLnXTgSbQGd%2F4SsDvZUSUb6y3n2wHtF%2FZ1KimTfzrYBlOdGHjt8ZKd1Hq9dpJtLE6K2OWld1OxuUs6tuoKjErw%2FvOtLsq2ge2DnxDormBs8naDGRCbfvcYLfMW7NvqnYOYVD9XO5inI8n3fH7XOXZM7QJyTkjOxSCFzKhagoFxY4YwTso9yGBZAvJh5EeYM1mnCqMDxA%2FuaF9hh8JbRl55s8bW64IZUdpLoSZ1pbm%2BF376KUVedDKdj2IpIxU3M8KPDs9B7Kwprz%2FgF%2FkVh1xVpyH7a02q8r%2FRUozAahCp7TE64%2FexmBDxElDR4FfynMUMyCGJ60MsD2KhE%2BsYzt3XAyE6StaOrFFhr8po25wCriaj%2F%2BOyUSfiVu499vElrRxtMP9ZZllvVzXoTcD%2FTdt8VHhxenEsmp2hW9u8RHL5YViFin2xqoHbOoqMcRVr3WezBIVd1vbPBXy%2B3mJ4%2BaVh%2FcqtSpjoqFQCtOSua1Lg6nl%2FawI8zFdmkuCpu0bucPKV3R1Nl3L%2FVr2RFUlNnET5YhGduKZ1fBPh5Yuqt4Pltn0ksZztW%2Fhl0ihLY4wZOUAwLYIyQKRvtKGEUMKCV09QGOqUBnzA3U7hbxiJNaqlOoIOH8A1TC5Qmp8M552S0zBZBCUKi6wbscPsYkuWntjpAwex7aikM39EjTPp1ZaCTBh0qXTK5mlE2vt9StA3BAUPxQWjApPiNosQ4mbuOHpIvklCxlug2cQ2kU%2BWZ%2FkSvEhqMYECbe9ggismD2XQJYZrY86JEjnk93afJY%2FVVk2RHbzCZ1rW3yAskj7upIKJcnPt1eM5xcH2y&X-Amz-Signature=c8c639748f9cbe80d38924992762a1a0ba584e521b6e2122c9e201f1bd270355&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRF57QP6%2F20260831%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260831T021738Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD41CmMGumy2SLfu8LYvru0Uz1jRdlkLFvfERlDPrwFAQIhAJfPS4IoH%2B4kZZnzofxlG%2BBFV3jiD3WYZy%2BKGG1nCuFkKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxEAvJ0xYhPDzgPfCIq3APHuE3a62qttfPByYMioeHRvx7RKppFJWFu2Ed81vwDrAVl1C9rsD7if%2BqoSRNYpH0F6PJhbk6jD2jKa4t0XEOteMK1hrlzEN2aLd50ALEsif7Jc2f6y%2BLf7fdcLqAFvecA%2Bi8xm9G%2BNNF6MNcs2sWF8LSzUdTYjRbiFBlC8g38BRIGaEge72W0kAuz41p5VyQq5AGVDL00Xbug5uh4nbjT4KlqxoePP4jBZuGpxACMxJhxhcD5Ad12Af%2Fek0TyqT00ncIRdKsC7apP%2FaWyvd7HWkWYolMyVtWU83mGdcJQA6Z7NvxmlL0GxziqbSmA7h6XDzWU6SxWYlUifrTMhWePyrlc2PfFr3NCRLWHmojuiMb%2F4sCKVn8NkQn6qostTRtHgf1D1%2BVZ0htV4mC5y%2BaFUwAHd2k6s40eHlAgLgkrUuf0vgq%2Fcc8gyeGHic0TB3iLiCkNi%2BqUZMowNuVBL1eid0dVdqbHBBWiBPYZOc763cw%2FybpeDKF8muVsOHHnuHzZgJ%2Ft4138Nsl5rARkURG%2BoOuX12P0SYpQm%2BvuNXV8SnZvhGjQxqROC8OQioXN%2BsDSjUPBQeM7ASQpYx%2BFDqNdNdQIseQUauKPCv9QBF%2BE6d9cq8mtOodyfm5daTChldPUBjqkAWqkd%2FBcjogVDg5szj0lLLTbRQADTaQeOcqHv0A4H761dmWu5bVs2AFcTlRMra%2Fam7gnJW5dVa1271r1hQR4FV0r8Gw6YVTmEkV%2BlbMJt5ahslvk9UrhGBGhPpfUAzqGbxCELxWf6xpFmYOodJvdnaasaYBMlnSCs9ctaYdXVGSLBsb%2BiNbQG4QYCuyXHfcjnAC5Bzrz0cUVpYXG2V%2FxcWouFmyZ&X-Amz-Signature=0fcbd3b46bf1c427f85a3db65c832324e7bcc6ba49b2a9a1b9b47418e194671c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
