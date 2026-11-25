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
fetched_at: '2026-11-25T06:02:23.125Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZQHCJRQM%2F20261125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261125T060219Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSww3nar%2B25Q6Ylf6a30duUDR0FPlnij3nTubHWYMTxAIhAJbd96dhGdBaUJe3dcVyb0XKKQxFQ3MIqwpKpO8PSsBMKogECJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxIwMOlCA8ptY%2BPETIq3AMxUUYD%2FY5WfNEGfQR%2Bdq%2BHTnsXA0EbOKyx1hj9EGceZDPIe%2Bui36SPdzh1S6Hf9kMTGCO7eZek%2F3NgVzIpQSjxLM%2FJ2z090sHkTHXlzEamnUE%2BitNTecLeaieKaEif1AWmfR6ab%2BN3hLbgKZotwr%2Bn3HRkZovx7%2BuSb4dmSu%2FHCTGBuY20QHOrquQWhrg476J%2FiRH9jQCN6o%2F7m4uwyvtNhoJr1uZcC2qMfzl7w6Wo%2Fv%2BMtRhBZp9G1gB2RhXwqlVZosJtzwkKY4G9mJbU2kjCULJGkGX1PABWyZU3exmV1gMqI6DYkc%2F0%2Fzx7VCHkGnbFcKB%2BieGqqVKkDgEoInAa8rVOQSozAvjER5Nq0UEcaInfTnVoLYiN0EZ37pYPvW%2F5efHmhM0wzb4bvwYEZ81yzY8afB%2BNdMLGiT48M2oZCCVYhyXOoMkGyR%2FH0hPg6CaUGcZh01APITG%2BzXO%2BjThxch7bKdUfMbRB7vmtdSScT139X16nPc8ZPvIAPv55J9vvw%2FZdoiAj8b4%2BtsPi5JM2uHsoqyG1x4EH1XA7cU5IeH8Zyi7s2xXWR3Z%2FyRJvNuZlHEXhv8d%2FqB1aCPBqCent%2FuN6a1Jq%2B8Al2r6siq0nDIahTt5iJK5J226kgzDSpJnYBjqkAVVjeEuBdav4Eal68MoowiZ6n6PqL2iQYPf2nm4aITLZCdQeIWTfcpAIDeHMf7RQAGYMBUtqCHBmPgrQhWjXmdGJYDTRfjfVd7aeOSwwNj%2B2GJ3zpD0ClEY8Vvri6Ljd16qgqXs6gS9m6C%2B4Cugixa00rDTbmYcMOewL2zZdJNEU5geSFHcTQGdQ8fxtvueso8jnD5FTZbWf%2FCJYBpOz%2Bufre1mU&X-Amz-Signature=5de16cacf069c7a59c324208e6bb548c8662a8ff6e3bcc2625a2385078111c1e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W3HYXBRR%2F20261125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261125T060218Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF6Fd6J4%2FnRgUAhK8fIhUHwfEvEQBJQcafc7%2FmNM3X2TAiAfa2lshJTq5pP%2BoZK1RxlcJnGrA3513l%2BZm6w7RI8j4iqIBAic%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMkp1weQpshiYNJxmfKtwDV2v5nZlWYrHkMKJYsRh%2Fgt%2BgBQqnpb01uxPP07VJJIHHTjnWnwnU0DPhANOEPd%2BEWN8tzqxzeYjGuACNeS3DeJAjAuX1cobZRZSq%2Bm4Qs48WRujnePkH37vHJE57I5nUnxoeCGQpRT3ADqbXBwsqXj3QexXtH8iRbpBiCXnYjnhH8IVYRw%2Byabo4K6vUZKc7WQIpdxM3nMeMdp3rPeP1n2PBmAJeJzkhv80cNC%2BJfFov%2BFPSXdpMRoLNlPHJ0lkuEuZ1MUmHAxWlBgX1TMHP4kKj06pZYQLt3p9MTJrQ%2BqGqALXYEPpL7XjHCk0Qaph%2FxhXVl%2FPZ2JvVAdLL41PnXFLu2YlpiQJEA6U5Ex7PAttYTV9ZdvA2cZJNa1vpe%2Bv2a6k8E6n%2B6ujs%2F1HWGKKJbjhrTvEOmnfO04matKbJc7SCf95BfWcNniQVzsIQWIkYO8AS3XLWzdo17x5wnSziEM2Nj%2BPsgYCH1MXIVdKLsWLaZpcoNiG3LztqpYvXeh1TZZd7Cbx%2FsXRYLEo%2Bn0gGvi5FSBJ9h8usd17DNIXmD%2Bmi%2FDG0LjXTtp29P9viK90L8Kb2jqYMI2IJNzaRttrsYVM%2FULVQBfKdyXv9DmVbfJogic00V7bBm9xJXNkwz6aZ2AY6pgHCqFft7WWSlVADUUUBNLz5sVFBR%2FkFgfENhTdlREG9s9s%2FCXnAv1%2BANBw3DT7%2B50%2FTV%2Blj61AHgKxCTPfE3DUKIiympa9bnmQr7c0S0%2F2AOrf0CQUrirLZKNSZlNXcVrqFhtzp3W688TGvFbPyyjq64j4L%2F2ajYizqck7XXSwEonZQz0bikRopp7CZ%2FFdJDg6TG4a7DG4MQQ65XHMNYW0SVpCoIs0m&X-Amz-Signature=103a0f88008fb015008cffd23b836d4274531618dcfb350e6e3647972b9da38c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V4SPV4ZP%2F20261125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261125T060220Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCQtETERTsLew03Yk7NW9IO2%2FrVlBJGuBKngaQ%2BQYd7SgIgTeutbLDYoE10KCt5tq8S%2FcP6fwBVV40csiD5Hr4ZwqMqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDONFr1BhpahZzsE%2BDSrcA1YjfvT3kSgGJV7HL4Y9LTOUldNWoBuFv7vMBSFw3XCDzQxhk2JH%2Bo8DqlBBoqCHpGitJnynWY2se5q%2BTLioC5pIvqqsTQEG4SEalsNlM6JU3TTvUmB6zZ28hgYpzBuh6TRgPLdwcqam1cc34SVYdkQ%2F200IDsHfNz5IADnw5lEEo%2FyA2F%2FfcbtNpvOzaMpPVnBTQKxWMLEUv%2F8GDvoD5uGx48qQASSpcXPcawqL%2FfEO8ROJ7VpQY171I6pvoy%2BWyQ5Cc%2Fg9SJLaWidP66MbsSo8wu2LKudxLiJ%2B0RW0pFeKNxReDnSnnK%2B96ntuYpVvpeLe8qkycnXR6xc94%2BuDoozYNYSMfJa7MK6JqXWijZHGZjBBO%2B9EAHC4PyACELSTqYBVJpn9BUbFRF4353%2FMTbVAAgUcZ2ibwrvj%2F2D9Jy6xaiLOFpDLj9rncRh4QfsOUtUhyQclDK%2FIq7Ltr52R9ffnPSU194kmKgDSVS7Wuc1S4jr4dEsfPT2Gv%2FhpmpCSq8UcZoatibia9IWeU44pL4iUpJJkttBcDsiglFEU6MQU1Jr1iVa6nyA4tD6cx9n%2BJNF78MxIRZdIyhvdYWpCcy3G8utLiH3K5%2FEs4qX3%2FsfbA5%2FDf6BnamvkKwC%2BMNWkmdgGOqUBEoLwwUQYv1adoN8c15t8Xn2gPHN7RhmRpCz5bhyGPz3jAAFs3A5ONlsw6VSiN1ZIF%2BhYPXe0VjBL%2BkHR71tChQiqNoW49ohuOwunqfFCA8quwKyg7VbDZVnJS%2BeFubtGWYSdzSWMr%2Fms1kvOqeCRSNaEsy3u%2F1lECjDEhHhOsG4XmWlKUony5Ww3%2BUbS5igIpR3YbpC%2Fa5rBbWb%2B6VKYOWxIWY1C&X-Amz-Signature=5af3535aa5f2bf5df3632cfb35698ba3a2ea616be258baa6be0f43c887519697&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
