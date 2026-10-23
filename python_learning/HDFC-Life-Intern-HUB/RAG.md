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
fetched_at: '2026-10-23T02:30:47.687Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665R72DEHV%2F20261023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261023T023043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICdCFnEd7DKYcioYi2AMr0YFKYE074NLxReqmXwHVoCFAiA75cf2hgIbi8vvlNe8sJTkV53x2B9u8xvHy2M1tz6pwiqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMayI8x0qFjc9G3l4KtwDmknsPJDBiqus%2B9%2FSBPVVpeCCUPo4lt1xORU2BfUGaUwtAZ0kVhSHytttCYAQx0frUgLQ%2BHoq%2F%2F5SJbMZLFC1MP6d23YHt8qLOMbEx6PkK79%2BgImEIs%2Bjw2GPOZsrdnGZ0I5tf2wOT0I9Uh9zhY6amK%2Fe7PZz5HGOi5qk2mu5P23gkZKblAwFq9Uu0FsLqzDApLOJST1pXTcJN3k4n5I8%2BdpRpC9RJcXICJDupZ5F4ngtdUdrSQlpyyJpRjWs%2BVjcYedHK8byPahLUoqEhf0GeuhPwdkKl%2BnbhdjOaY4FCSpBCPj%2Bh6GUDnQvDv6S8qsyWCFFds6ZynOAdsNsqhJ6CDAobv3HB0KzmQpSFm9BXRwKBUlK2fwSb%2F44F%2BguYi0Xgf0uFFrQUrn%2FPg1p6uk0e5FK2720ZYcLnTY2uETm8f1tzzDHf8cEUXU3Ymbg3RRs0Wd0PkYTZR4kT4045OrzjGSrOrQ%2BJs42vBYy9YydR8iPr9JRM4Izpuq5De%2FA2uswRhpvPqh3zwrTr5t3hU0X%2FDh3yiwcsNI%2Bv8Ukvr4eN6H82QYjTdE%2BKsAHGnOhDYc%2BVCvAtMRSzARDW5Snvc%2FT9b87UXI8w%2BiNpfZN8WjfNT2t8%2FJ7vkSEJZM7rY8w6Ojq1gY6pgGq1tHStRmaanBebI%2FSZSof9VwxcojimJ7i7lMZMjq%2B2GEFPjY6ThsCt3u4B4TqXxNQmrYIkMNI53j7O9278nK2aq2nl4CRWGELeEfnb%2BjGMvf0iqgtFMRjmQ8L7Ys6o2cWdAYmy50QuTHi7HtQ4hs5j0KEKubGZlmw%2BKZmSXGP%2BeAlYOtjLKeY8zBZHN4c7rK1CBh%2FngvNje4o2rJ6I4R9U5aVduJc&X-Amz-Signature=d4db1294fc1b5bacf6ed4cd0cd1cc32e0b2b5d2e60b0520983503a10000f054b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLSISBWH%2F20261023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261023T023043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIA0jONsYc8hfSOqQV3Ql7uvjIOpPNxDGnWbLspmcizPbAiEA9GDqkPg46VEyKF5vnllHy43bdOXscLbZthGt785oH3sqiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIxWwm%2FAZdnYHXNw3ircA2rXE%2BK7EgkQOMuyXX7JtsYcdgzkBBC4uFU9XEcWcdn0DAyiFT4sXcbO%2Be1BpVl6TPk47cg5ynV3OuxHxGzOD79HVZTp0RX%2Fjdcmke5NA3uvJf4WPE3r7CKShSRukVqftqu%2BFGmM9x3aDWOGILLeVBoN%2B1GZTCfxmRYayyeoZLrijryumXuSq6vIgR6lK%2FMt4D34HkWYh34TQ7B17%2BIzmwNfU%2B2T6j0Aadp5K9DZl4ptMeUSOMO7s5kh3X687%2F7f2RRqPM0Ycv5udAaSeredWNWyv%2Bfzu1BItp5qL9VoAz59PQ%2BP0mQNHNoJGhfLz%2BbFWKjpG5Vsic3qss0KkKwZihIH3FPVQLfNiwMHtM%2FBwKzdHNy58kKNmBk7Kknr6Nj4nUbKDLn8eqNon7%2FiJ7rAnoqCx5J2CQLVQ106cxUgP1%2FgpJN7bJ%2BoVQX%2FlvZ8keRddXURP3kvZ72YNztKegl%2FlMeEt1HVcmjKILQQH%2BjbF7jAlbAaLbxLKE2IujvY8tBjQi8StkjkxX%2BteudDoP4iqeomENnJD4%2ByO9F6FEsD%2Fu9cIVvhPgXUE0jds%2BLXnqZ44p%2FJHjnj5s7IEfAkWU%2BPwhBx7kHHJ6tJJkrbUWpykFWpbafgce5RclI6erxLMPHn6tYGOqUBswd7Oirvn%2FtTCibW1a5Z5Leri34Ly6wuVnhyZPcweD74MKQTNG2O%2FpPbztrWOHoRTsyS7XNeUZLnQzCHZhjWrONBkxeZH0mNhpEEMCOAmVZFfXlLsDY7w8VokWEWjT%2B2fUZPJ5tGHJah3VavNCZCsJ7%2BaCIJWkmtVF%2FRk8vJT0pRWk5AGVAWq0VfglPCQCZfS8u95e7bu%2BU9zAzKvtDXoeSN6NOF&X-Amz-Signature=262b2506ac0a5031181717ad55cd13766143a3306294e57722330873ccbf5ed6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667WNMH6O%2F20261023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261023T023044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBv1iKScQCk7QLCp3V6oM5uaEeM%2FV8upQEvPqReWq1mWAiBpgM5xhyl1iRIPCYFFub6V4H6z%2Fl87AO96Czizh8N36yqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMEbVfOXST80O0ebRoKtwDtznap6yEtzDsLSEAXNdMbsaudUuxBV8vcLnxub%2FJGl6FfDUFmmT10Z4Dc7OatZ9NuZ%2ByaGXmEE%2BLBH3WCbvW%2FA2ENJ8d5PIK3KEFqpr%2FxqwXwcD501Mux9W0Hw6Wvm%2BMifans9ZjFD7xTUGICCGrg6JXGmk1Oo4hQulVHj8KcpBoNBeYuhX%2Bh85yssxQMrtNSsPeriKaqKOXJfA%2F0oGzMlKdTOyDdp%2B1gmnx0tyjhWQnWa8o3Zx5%2Fmn0qxJrNmUymtijiboIttzZEEI9lx%2BrAkcvCg9kHgujTA6YtmE4mkE8NI63OiwbqQIyL5vyThPv6c38iyb86amOgmS5Q%2BI%2Bmv4Z%2FAEBXaiiSCXuOU6AMJureYa0GFu%2F8EVwD5MwKlOWu2kiyoqneVPvq0wSYnuWASQgL6by%2BtQHbdticg3%2B406QJ7Mss3X%2Fk77vjtyZElaIcPFD7K1jPu2LxuRX5cnPuIc3F49wiykMYRerj%2FqgFW4BlQW6hbpKUvHPxMeTeVDdM2dGb%2FHMZHIaoxLgspqQWAbk%2BjhnwT1tdwG3yeHKi8KRaviL6cg6%2F8UPuafXB7r4tgJnuDi%2BND10LAbvYTChlh6RYHHjjE3Uz8B80IwXh4t%2BDc%2BAHlEObkBOgeAw%2Bejq1gY6pgGjCHG%2Bhw6GxvHIvXGVbmiVSmekxaDzxOD5e4vf3Tz7XR%2FewpsDrhxvQIgyuLmsp5PVYX3whF5lUK9HLOv%2BTtwYhneAqPQ5UECymV8J8cRBTeh5YvXmSMPMmsowBjMH0YteoNWoV6pZ7hzx6qTjmzFjhnYgN3RvwLJtNOVaceaS%2F8caqzL8V9nEbazk%2F138w3UfrrAunCK%2FAQAKJ2wKJkyiPsKIbaT%2B&X-Amz-Signature=c064c6e8e936b8103df56b1a752ebec1082ab13d8200314a442602112990d064&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
