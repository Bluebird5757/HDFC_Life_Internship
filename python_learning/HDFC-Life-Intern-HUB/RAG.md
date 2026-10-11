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
fetched_at: '2026-10-11T02:51:42.499Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665GZBOMFM%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025137Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCm0xiryYBcpG9ZoBHgJ4udLuliCq5L2jc3Ir0pYtPOkAIhAPQ8J%2BRY50EbP9e1LqJv%2BuXK75OmQ%2FD9BzDPXQKfCmcGKv8DCF8QABoMNjM3NDIzMTgzODA1Igz8PxjmL5qbUSq4yyoq3AO1QWQ2ogLOUGD7%2F5u%2FawH0SpS4sQaff4Vjtt9N%2BWb9%2Fb1S8nZGlUJa2QgkzYD3Ve9De3vMX%2FjaMGAo4XMMH6wa22WqjqW1sJgQKNOsfM7Lzb9foCNAuX91Ycr75Ru6TaFM5VSdc%2Bnhkp1XLDyMX2n83MH3Lu6fB0RouxV0SSsCPU7ymuS2mcaRQPvdrcf9HTeF2hFFt3kbM8GeB4HZASc19uOaXhZ5Y5FF%2BNLWRx%2FS7T0MEmJPmlc5Zp2vYzPWxeBUgVjDIGIgPijg8LZjDQUFyklfiYb3Jcvr6SX20pGKwCi27%2F0lYUbEvepHk22QjI0H3Wo7RG%2F2iG%2Bn52Cnz%2FbcYQbymBs4KMfC%2FjdcOwa0DydZzgdxIgVuKr1iQsInQe8D4y6GFP0ISNVOracnTY59Zuy1xHYuvoO%2BJ2QPW4NkEuPEfwrgr8B9xorE1kda2Na25%2FzT5dGVJf44ZNiNSWG0uX20f5PRISooZpx%2BrsqWsqKMMXYBLHSNGGp4hkYpmPeBcJXdh%2B0984pPAcCALZIN7T2xJn%2BpoV9ovbZ4N3psFIaIMejqmmmCxEc0qtwnr%2FJnMiCmchxatnjs1cqiH%2BwG0ROfc%2FbcayqwdKmO0%2Bbfu%2F2HpWHflr6OwA9XKzCh7qrWBjqkAU6H%2BbqNZN%2FGCXsut%2Fj6GioUmhfa8S7T6Gu62UUkuzQSZfu6vTOyRLX6DA8iRjJD%2B45rByU0yDOsKIaPaM1MLUqNdkO02H3Zl5U6trBdNXr7rIrFA%2Fkglq3Y15Y0Hj8YSrvYCXEKZYGvUvp4aQ0fPVvlGgnzGUucwcwiGqJcYNpjS6v5plfaC%2BbO6WdHwsxXhcq%2F3f20Slhx1zAbLG8Z1gBAYOji&X-Amz-Signature=6b25e158a239b7f4e4b342501fcd12b68e2db41fbbd5b40d2bab1b9c79c76a30&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYRBUGOH%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDsjj9uw248WgEScZQmLYD5aFu8HF6M1V79%2Fhp1SnjWdwIgZczYs2jeVWGD5%2FJvQLqLltZD5ad3dwCjML1fikx0Bc0q%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDJMCLi3i6s6o3Ik5lCrcA3hZd93C5he%2B4xWpPNpVdEBCuq1miO7hypQTHsmtbZocaQiZyCBj8BrtgQhGd7ZZH6xjSifNZQgDqlLfgCSwrZgyEAi293SIT5mM53mZviDKZqwfHjmNyI0m8u9%2FuBbtfMNDM1%2FqU887S1js0%2B2ITSWjj34KiIJQXdT8rwNFgX97HNgAIEdXgcdm3p%2FnkQIBWfHH1YLZiXiT34yrcLQ1THafbckMBC3wjDxnUu4CEMd6ezbY8F3Pezjq9uUCXhWAsTHfcNIDDEM9%2FDleQkRIewCgu9mR8aLHVLJ4LwLIDA6A8IeVEZABjZ6W4GSURQRH4c%2FSbwlPJVhGDXJX1xzQxz8m15Pr2KdmQJ8fRYbztPLTqcbhqgRVPYozGzoHoUfCzoQQcU0UPflhtaUJY0%2B6rrUpeyTUFsts%2Ftkwz5MaI1x4%2BLnqTRV7Xdk8VkeiPwANXCq5XTuf2UEwaFl2NaGDTTUWamTi4lvz5hVxg3YrakYtUGegjXmMHperRT5K2aQrgfFUHBrImtSbuokY2blVAhqoStbD%2F00cPnB71Tv5QTS5LdT9e3fUEeNH4VIcm8HUwxiKPQIO%2F%2BkIOMJ9gWLcqFQlJOhixuseXa8V%2FaD2%2BDPZhoz7FAbonu9WvV7jMOKRq9YGOqUBwxoF6cbMExvI86jclcAvXp%2F%2FgdrG2ZLujHKyP8DWBnqVOIQe74Eed8eUC2Dzp15s5L63DyMBxsrI2Yt2d4g29wdgZcqYolQ7dOou06TeLehUkwAG3cEJc9XkdTmws1dadtPyJKr3wkv%2BfxMEqAps2YNfIjt3zPZ8LG6npwxUEzTIrzMdup9tYwg%2BDPYG9zL5STPyIEJ49Oo4Wm%2FV1cbVGt%2BdstkV&X-Amz-Signature=f39649fdb2cf78951978f2c698d652916282e085ea610a1036760ec9e41a9061&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667WQD4UIN%2F20261011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261011T025138Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD%2Fsp2SwcnlWcZ4oRO7N4fFcrcm4CFbZRec%2BEikM2AhFwIgbMy6q2nCt6wSZBa02SjN3IfW2kG%2B%2BBbEhlTLsgSu0sIq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDKPl9uUVgEacBzS%2B9SrcA1dAMmdXy1zkdi0glzOxxZiJ7KjI%2BQn7BNUqJDQS7UZHN0oOBma2gR3PxJs8bqIu9DCpO9toyHQXz30RuboLWYJoEZE19%2FbcRvgu37kpZVetKcMCmdJt2Y2GI2PcOVxTrkvdtwIPN3IU%2FGF2jy8mBrztChGaTr8GlIKs%2FUxkKAqEYhHQ%2FqaqvtLnS3krghIBFJVYye3cnN6iFx6gDGOmtE5mct%2BnixmAKVZwgKkwxch8xNHENtZhOL%2BpZGW8HI8vsEU73CwqnGMq1DezJIB8fzdR%2FOMtWlMYnjQFPAhepr%2BPpvo77jlv8XJbtT8QORYAI61bOZ2cUcH4NeCb7ylAaRIxKUeHwv7duqH6CLPGIrylIqlcs0SVVmbuKva0kKXLeCa31uJQ%2FekEuZ7dt1TW85MclOpUgfB2wcOLeGkEoc0MKp%2BDErb7ZeesQvRWGqsA8R3G7kjYw4XGRXiRNhkEWkIyP07f36aZv0zOCstD9N8DpvNRnBgFXce5ToWjA9dcSxSvBMFyyy87RhlU33t1C0pXg6wRPqtGoplqEK3lwXsePfRbY%2Bo3AFpIMoDELVGh%2BKZY6s2kFnffGTqPrGvaRf%2Fd%2Fv3VGn3LKYqKQzw3f5YBtF%2FeOmsO92Rl5jtbMNrfq9YGOqUB8HmMKOC%2FY74qBHiCraMvxb3Iq8IZDAUrI0AMXGDGME03cQTtjyA%2Brsu9l1RF1QgtJ50H1LFPdp%2Bj0vM7JykE%2BMINrD75k4egA0flPtCNXfr1VI2IOOmKIBrEAopkzYIzzIR0h0d7ci6CinOD2qwIb89MsvSErXFhVorjvyOM9nr2rVkNxoUiWJvbcrmd2DpxHINg%2FcFs4IjdiS7hZjS88DiULaLm&X-Amz-Signature=40df200f6e279da61db014750354112b67b631ce98140f48d49cff2993c26d5f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
