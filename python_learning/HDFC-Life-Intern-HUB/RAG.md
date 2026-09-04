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
fetched_at: '2026-09-04T01:57:45.641Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646UKGSH6%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015741Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQC49dler81pmQBoVpzp7g7LDp6yx7K3h%2B3yMRAgbHWhxAIgWQg3KQ5B5gLr%2BE4kwDaDx8X9PYawoPe%2BU9rO2%2F3jaNkqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL4u5EhUUR7yNKgR3SrcAzzOqA%2FighaIdZkY8mMeQsNUet0%2B6l39ZlfqJ3BH%2FCMjdXn2S4%2FPLmvRh1KNuPoawZq%2FzEzPpAIxohT28XO07gxefqdUMRL9EyWNqxwsfAe78p5HXk23Tfyl4W9G0ljkatNGpIk0IZVyz9R%2F3NYHNe35C%2B8UWQKE02kKN96YgpxQDnldPym04sxOI8rY66S7NMT0re4h33x0sci456xRhHGnuTugtARyv6PEzCui1pZzYnOUiyRXhUWVvg1hcDsCk%2B0HrXutA7keN3eb4jKOlarGA8qmo5bSdu56G3RaXdyvvQ4aESNG8fIWjGut1NC6wPiF5C1iD3Gz2%2BdfKwJBbURVAZYDvrP8%2FPl1DJcziXAMUOVBAVNSBFZ7xYP9aiSEbtAVr6bpgfmnmPNPioRLbXsJF5lihrOzJU0l1QqTV7aYaqk313Z%2Fx72GBCeG%2B%2FKWDGFKzLgAOcAH20iTqmxQ87Bj7KI47P5n2ga%2BTmH5VPRyZsFQqu%2B39JMzaFdRoSERpdXTjhonyfrdQJJsOVDg7oCIchRPxic%2FJGek%2FvW%2B01epuIZFczxEkaXH1ZANSpO7hZqRlfgo%2B7NUYwxgSKZ7AQ5zStt82Xp5OEUMtVYQY%2FBTF%2BmXEBTAsDlo1L%2BUMOKL6NQGOqUBXbdYsLpEMfmJFluhnXYCs%2F521NVxJ%2BrKsravYuBI2bypFBOuCH3Lcijh6%2BPDF4zkETwceM2OXMVm7wNKc6Y%2BWYJ8h14PppkcYdc9dczlP82oaAChvNtHD27mz8UgjKkZGkl5klL%2FkjJlVjl43fiXfN4G%2Br6ezZNnEaVBzY2aZomggNBwu2FZdyp%2BHwfafje%2BxvajCJk2H8O3bLzLRLvSZz%2FVKb0g&X-Amz-Signature=24e770dce3a8429d715d2e4c22099361c2d1630b7f2189cd2568c447d52aad70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662RVZ5T4T%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015741Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCICHGZfn8MIPROKSVd%2FjeriWOeT8%2BKyTjqfYfClZjXNx%2BAiBAjutqsAtJatPZY4E%2FXDd8xRHbXn1PmIMj2lQQYNjB0SqIBAjq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMRYZsrufyTK2RmBwLKtwDxl%2BnJglk5MijYHpGFUPqLiaHpj8E%2BkoBqmYQE%2BZDU5C11traJSz7OHvNXYAl%2B9nC%2F%2FKXk00I4msM5g%2BCSAn5sAGtVMaBUUZmH0R8DJsB23K2uhUXXDmvS2AMPlZ5wBDmaFGRQqDoCpP9rA5w5MM8J5s58Zd6CKPni%2FdshDNxqs1TYcPdVPJpFW74%2F1UbpKtstptZD%2Ft6IEyvHQHk6B5vb6d4qrONTFVn%2BD5O%2FXySenwQ0EAGOzZOB2U3Sx%2F8uqXbppT3vCX%2FEmeerGKP7iKgWTot9VK0jAu%2FsKAEpMb5zLjgdviwHOTfHdMXmGlS89rd7x1s6HyIhjQHZcf%2FIAIH%2B2JyoAEzjRqkDRL9mM%2Fx1%2FtIcE9EO1JJDYKM%2Fv6mUctFueKUHa531NLIapQYLQtUTwJWXbbOKpw36LCfNEfLgind36whL8DGnDc05qlaVXh%2FPo4aeLuI2RPxY80IAbkeHLSELsj9%2FhomDzG4r5kikzGksr%2FkbDdQoMlVsFXDsWSBsR8%2FU3Y2k5hg1zd4JTAPkxpwjHS7gMFlJzEyJ5PEqtb%2FuSBi4pIithzIX20wpHHrzD5Tk8hvHTcY%2Bn00OmtAX0VNwd%2B%2FVXM%2B26p%2FEUsOD8lm%2BnxAhYDWRK9r0gMwubvo1AY6pgFoai2T0JEcyhz6ql0tx505bXmCqQW5EE%2F5qYuIEYLMoRiv19VlNvdRjnpKg0CFOZsv3LBq0rQqAXIlRqphaSSYMcHVPB5mxfZY8FiMJc%2FADKZfF7HASIxmA4FrGgW1F6tbFzddXxzh4EtT0IbwDDc17c1Ya7AuLvMZVjLaT0fN2dQ0Kp%2FdzZdCK9fSsss%2B0VP39eyehtpHhvwxf%2FJ3Qb8sXF767mxK&X-Amz-Signature=76e0578eaaf3755a22dcd53ebdce44a9fd72bc55d22c2ed7b36a08ccf64f35f2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZONQD6M5%2F20260904%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260904T015742Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQDH3tiuxQcpVTGOhVRvrBdxRFtd3wD88J9%2FyWGNQMVZPwIgX356Iy1%2B6IUrllR91xzqSrrX7p8PjwUrvQcjSmJIqysqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFg4B2Zv8SGAVZzBmyrcA%2BGjdIosG3T2xWdSuHTOqFjfWqm49uLtCRqC8qKHxU7l%2FcnnS4snoxuLOxX0u0TsP4kTGeIsEAo6el3RAI6iFQO2AVc0ZLSNEzQBJYR5O5pwni1gjmqtpt%2FfvBjx9Mdyu1LlYo7Lo3ZxbBn5%2FWGAFGtSmxPc13wZKx3TxBjbr7act3hJUxnXVPjG9ei0HPSEqtNgQWouDY67mjn6GmVCGqxefA%2BqBuNBZ9zBb%2Bc3Tyi5iIWOKt2qT1PzJOCtF%2Fh7had6PtHhKcX%2FvaKYWlmkxlg4bonYXcxwRn8QAKLvOEJWfDcU53WtddBg1DuAda9VZEQncVcu7DQ3XU6Vov2U8iofkgcxVJb%2B92T5Ml1lF93QVPQQ5zA3y3JrjY8J6q7ed2Hxao9WSee5S6cgFpBBJe4wj4f5WrN3zN%2BExPn9ahaitNxaQdeZON6qF3c74HUSP1bQMi5CXW%2FTDdgGgGMdOo4IWmFyjLh0jzi4YtPYFwGS3qBOK%2FXryrJybzyUTtJziOG0XdhEabRwVrx1LQwQsNAg8%2FZZiJflDbI75MvLJRM6HRdHIDhvBXBC5x3R9%2B6nUxsNImolYzmbvv2Tzk3LcdSB80rBdph65o5sK%2FwQcme7i1nakXOYt7DKQrQPMOCL6NQGOqUB9u8Shc%2FLX3mqfev5W4cEZd4hc3eJ0u49FYE5KIwBiynRDlMHciIP%2ByGUiT7YKk%2FyJqdVEpVKbT4J2OEQpP57sd1jlqqNYEF%2B4TYENvrr6UhnYVwPxR8yDE3k0i7cFgbXu41MTtvOvi9ABHQvQAEQaS%2BDT1m36%2B5cB2XcdNAlSnj%2Fu3xzxx11tDwqjD0dq0I9aZJ54U367%2Fi5V5poU%2Ff0QQKKHXce&X-Amz-Signature=20d21cc2e71b26eb83ffc1ed43f9eaef26cdca06eddeb919b93f8bf49179c9f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
