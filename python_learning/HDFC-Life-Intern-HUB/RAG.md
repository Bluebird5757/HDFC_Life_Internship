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
fetched_at: '2026-10-01T03:03:44.794Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UBPLW3IV%2F20261001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261001T030340Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDq4l23NdSgdXcCPiLRmRLsjNkXhTPa4QSEUXMECqM7MwIhAM2JI7Uqx6TO6vTw22xO2RlriDAuiWRTSp%2B7B%2FjoRLV4Kv8DCHIQABoMNjM3NDIzMTgzODA1IgyUw2BomXwgWeG9ZyAq3APCubkeUG%2FaW6A5nlbxRa5sqrzf5bnHXHPT8y0NT9CY7FfKx7hmL2hPu%2BNdsTFACMF5NtcCPSHMVsz7FTt7GhNRgotFtDx4ao5iZkVBdgrYTCmvl9Lxd2SA0JnBFhYhKs3vM5daRpRjdN8MrMYEMkp%2FG%2FG02x3SAJJNNfscCHTVohPEJrmv6TmEspDFLc4FG%2FMA%2FQa5MvaNxHKB6kXNmKKFzX%2FhsJUp3IeqBO6ejRgcwAlZPpaifkAljbNcfgHBZm2RDCe03ub0LoqjvWQOMRSOIKjGSxv0XU3j0reiSzFaHgdaQMHCyFwSBWK0d3F4D7leaFAwXbc%2BlhBjD81wcIFLEs8HQ4yrbB%2FyLVRRxT7M5Q2M4D3vi%2B9NbG1MO%2BaQcspIFcuXQjcjvqsNM9zgbMwxhviIX2t8QSMj3RJdIIp9oC9XP7KR06IDA5x8jIXREiTZBBpC4%2Bl4kVS5a2ml0TUXrROItlnqU8sE7tHS1Ad0aSG7L%2Bbt13PXHYtKhI6h9QZ9FPB87Hc%2FRZYXplwUm%2BoWDwnPHyYHHQ2ab%2B6kkKgjkip1iz9whqvSAAqPTVMvQMT2SY1FL%2FZ12E0gLU63KeGmkUTlW%2FK9klL42um6iA4olswEsC3Bea0G7jPqFTCX5%2FbVBjqkAWSGN%2F97mQ0E8iof17U3l22%2F4Ac5ulob3j%2BwXYIb%2BHyFewpCV40A0bJvbM9%2BILj9zu5ejxgbkvqySDYvkCiIYK7lsPgoLafZIV6L%2BjtJcAt3LpanRDebyAn5Rl7uWphzp1Ro2rhFghh1z9OUJJgNlJN8UCPc%2FzWT3EW0c%2FrOXCvMY4n94G8iK8aic%2BWsiOLPyAsSuoHmE1zPZDTD4LhuIv9MaLgQ&X-Amz-Signature=a89995a3209b2e32a655c3b081eb23dfb1fb153b06ac6ab9f5b32e8d3975703c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665SMW474Z%2F20261001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261001T030338Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDqR4JxG7tWl%2BPpLmLBPMgjV9TWw4hGAgduODbwTysw9wIgFnOh7nIzri0lAYO0%2FABu9LbZU9Isaf3PL7o%2FJV5oATAq%2FwMIchAAGgw2Mzc0MjMxODM4MDUiDLHFFuYBdAIjnCUQtircA5b9I0yT8aC69oyGuWCuo%2BMGE6esGG4Dbu6xfBhiq%2ByHXs5VyMeUQrQonBUETVk4VswqQDZ5BRhj9hZHPVUKJUtZ12gRktsPvcFNe5dtR%2BDn2HXZZFb7%2BvNGvJ5wvS4m9NMt%2F%2F2EbGJkAl72ACCB2nNBxX6CJsEtIn7DDKnYJST7i4YYIsRLq9MIMBVL3YEs1l3coeRfZg%2BMKygOsXYRgnmN3Ae1eOkpmZtQfMyPCWs0QrtgvExl85DiBF0GMoa%2FscSN9UvAvFIb1bYhOysuMI9JLNb4hv6NR9a8UR%2FXJwqCh1oBLUOvr8fDKT96V4aHmG5R%2BJmrzJArS018X9WY24ODIcS1v9Gr%2BQPWbQ61awxHVepTGuNx8QNGIRsuxDsPpkBdwRCAeQys8STGNSnPFsCIfEKEA4TIlSzjig5DxukoyecNDVP1WnzMATib2NVsq5AaMYpBUgTXOH5GoHutJLo15lM8xNMMCg2NPrqw%2Fa0ctxgccpVv2j5qbn4s%2BBz9sKGj59w%2FwCaK%2Fq3ct89TjRoetcqVdPuxn9xYVBQvDHWMx4yG9vCOR5H3GRnNE1NJO5V1zROOTp0EmGPDBH8gYkL60ucP%2BVv8wonNpNDvE1L5jn2vAtRcPM3pt2IsMOPm9tUGOqUBd8DCerZ0OzyWN5HOFWgcDiWxjT60S6JCa3%2Flzxu8yYcBkvMbKUOAPLZh5gaXM4QMGz3FinjM5yfFG7BS3keLWJv%2BDqd%2Fm6fOeu4fvmsUzMwJSJFj%2FwbMhjl8R6DCojt%2Frpy0jGK8FJxwUGlZ11rsX7x4yZlrKzXkWtZI6WDUPhidTqnsB49kRxgtxSi50TTVm%2FcUJqbojyHPk46%2FTMgtCKUM5oHU&X-Amz-Signature=c9e746c425eab55f23698a5cd48936dfa2a2dd1fb2c5f98e01177f992bcbdedf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466326P7ALY%2F20261001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261001T030342Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC8cCyaZnlmIyjtB2HbR%2F7YKRoPvztx8PaDAk9WSVOvyQIgahS9PhTB4wtJvpZ4JmmF3jc0dYxtu5lvaPJP0BW7exAq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDLSOtDnF7Rj59jUsfircA7oZLrc1XJxSW9OTzgdtRS7IdxoMEXC6aeYuzUQ4dodUrRGVBO6gUiKag6ejmOLeQKdYR9mj5u46%2BQ0i5pd97tNfIwf2KlgGsHjPjjQiapUSajQhfJ4zLyU7UP9F299QPNb%2BmDGlXc9EHyqiMrkQqxT7%2B86YtGsXqsCq6J19p34yhaaeSqHgE2rjtkYv3aC1nO6GfNsOigXHm%2Bs5tf1sCU11U3OyIxLx9lJ9WmihdALQ2cqGuRf%2BGiruHTOdhvYMdG2f7DRg50VSmhI9pFA2E83%2F6gEeVfNjjoDzNwGGHvnqYyHsaWSRd8fuENi08H3dIGoB1%2Flih21edTM9IFPZe4sjGMznbNHus0mrL%2BgNIU0ntT9rxsgi23FwS2aBUGFrqZagic%2FH2OMJgx%2FgiRv9X2HQFXdhLZE%2FcpyZAGBC9p%2F9TwwCtIajukFpfzgt4SQZNYKazZfuAPyjUy0UI5dhRi%2B%2Fo1n5ZrdV9k0J%2FI8EjM4aCw1gFz9l896g53EWq0G0fG0mYDNZWDuVx8yPBw3dt9U1XiGg3ZxeV%2Fn%2BLjl%2F%2BVfjHHxhq9ODJDVHZUt9tsl3sFtuGw0cnxH%2BZ3Jij%2FDDUtQa51e4eXnlXRKV7gplCpMcp45zwP0VsqZeXTUOMP%2Fk9tUGOqUBH5R4oZYaTRhtGX7lPcpUHS4wmGWG2woDDpKBQGHfkcy5SgVtKhsTSu8wTPDlgRjp4xVx0HyKACI8%2BEfo4XPdpd9b%2BawspIxqwwr4qY%2B9klHMOXQjT8poEQ585PEpD1ixBI4Y3xYwNMTJCdBJYjUeWDA1jWneoS3oRLWfZBLR0fvV3BWMCsQ77e2d2qM%2FtADjlOBSNZSRmaJ1tWt5gmG1BftT1unL&X-Amz-Signature=f1da35498e5fe59a2b027c638796b7103c8d550d05071fa2393eb20f907b271c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
