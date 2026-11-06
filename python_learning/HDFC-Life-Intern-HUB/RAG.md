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
fetched_at: '2026-11-06T04:09:06.331Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UNF2AAEC%2F20261106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261106T040904Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJHMEUCIApc6nXoRwvKA%2BL4q0NJ%2FvHMGVrKmXE48sMWXYVV47tWAiEAiNCbMB6upK52xrxMwQQLltJHvrwI2ncG8CMeadEqzmIqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP9bhChvy7YKMNjyCircAzGhROYvB7FCCYd%2B3w6aderAmjSExTex5C%2FNWYfwyX6JvK8FDPPYymvUx1zVvKuVQnJLki8jm4Wmqh0WDeK%2F6RwMC9K%2Bf4a%2BYYQRsgz%2BGIhr%2FdSJsX8NVgwQgNmMc%2Fs42XijHpkOUjVvrJ1gWno8eW581A%2BSHqviqbG1OV5C%2BIs0cu2PFukWPMbhtotAKL9Z73dtkf8xGtF9gQA5xBmUs7FQ0YJ%2BYZ%2F%2B%2F3fwtTl7UCvyOmyHkTwb3qK3YVEd8dBQW53SB0Mz0sv3EW9wVbUU2zIHAUGEXAUOMuL8QTmyXytt5QPIgxhShrm5tjA%2B1o3P7uUWSS3De4mp87G%2Fzbk45FCkIUr1Ns9ABc2UJzlnF2q1ECz6SUJ%2B2tKZbS6xPRc0p%2FwmAvmewSwan7cuURv603MyetSyjJmPXFVklju0gZ67gxMl0UkjD8V%2BmrVnq2CDVooRXOg4T%2FpLD3sX4bibobrJr5qa7sN3w%2B0YmVEQFcJJkkkVKzDewdK1uWbFlOwAfvo4%2FmfNZbECKpN1eRmUfr3phAYEabW74r1BgWcKFMGTapoihVijOOGJnoZ%2FvPAoGZxLsSVPcrDlMwUo9d2jdRJUUaMu7vz%2B9kbeeGkTcN%2BSb453swb2LGRm0raMMJ6ItdcGOqUBlribzg15BaL4UH%2FETQ3D%2BohVDCUPR6WM%2BLq0o8QJner5FDalha3iULFtdKYY7DTIPq%2BxVVRaZJsEuwvJriTseUl2mG9UEQ4RCQiunfSdHeV39w8D2Vd0itCz%2BD7fiiG4jpaY8WM%2Bjhi6ia6pThsQ3AKrbPQZgO50VtVha4FX%2Bh1shVRoZPli2Tx1dPiDWK%2Feqo6mweSYKjHuN%2FkWhpSMzBePFwdp&X-Amz-Signature=f964ebcd4f3cf22b868c0f2e371b07df11dbfad177a27583919a0937af651c58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGG5CS5K%2F20261106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261106T040903Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJHMEUCIDc86t649tFdPAH%2FblSJRcmjIgi9EksW%2FXlyWVcMgfknAiEA3sZqj6Rz2eSTNFv84FpiWSRG6ibX3%2Fxh%2BqQYV82In24qiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOrfGrf1txQCsI5UYCrcA2IbWU%2B%2FpizbR%2BGLyYh8khsXI6p%2BN3HlcqD%2Bttu040pmGoPq%2BE2e7P66HRG%2BjFfmRvNOoCWP6ZA9Euq1HFsLY2QHIzIxIbn2Ibx5bGdh7wuQI%2B%2FtbkMkWxSRZ5wMUDrbI86G9IndOOC9rO07IfKGDvOUuVf8YpUtjfaSQ10gWoAP7%2FqxO06PC4rEqpzLJ1lLMgcDBG92ROCwcIGQ%2FkUN476FLA54Idol%2Fq%2F0%2BM5GLcuWedfJlOigSHstt8IZxRUSL%2F5etxd%2FF0BvqJasxvGAKl3eUkXGHTIYU8nuCxV2%2FYamiZS0HSrs4vLCVfSga8OJczX1glXQi4hpX%2Bx%2BpXCD7giktu2leJH3F1EQNI4LOnIrQauxQqFDvRt5DPEFdCNJUWpR5%2Fb4vxbBiSZwy%2BtX0lTtBzcLrps2IgsH3TOCDCOE90NYxwrWckv0fgNz%2BEaSAC9Vcq6r2FMJhB42sNg0QTWL%2FhWNMaIH2%2BCN80M9H8DaquQuvbedREZ%2BxTUekC8Mot00m2IJcjkEWlTDhGSyhu3MuTWUXGHVHHZEM4bR5PVH%2BOj1pYcU30a3wpkf%2F3wJf3B7exa8%2B3fxy%2BMfmJlnGJSNYpRaLhlnwPJn8%2BKZZtUv4nOcWosefneRmdPmMJ2ItdcGOqUBYZi3mdGTztWp3qiMrCaT7iZRmmW5GV%2FGwibXeA2TXVzr%2BVuSelxlGFVdrwSc3TeCJj1Uc1nU87aDLqxG22D5f498Xe%2BKws2Tx%2FY3kMGOyo0Qc0oWM9i68ycKdXfpQ8p6QVcHHdhgYB526UixY3eYktFtXLRzZRN0FckT9AyIoJhaTuVluZTtNZH%2Bj0D23mriACPxsbCcPpjVKRt99RVYV96eRgjf&X-Amz-Signature=1645b37ad4e71d10a78e73e7e61f49b10de7f1702333c29960a9f66e48cd14c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

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


            ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/929020ee-84f7-4fee-b918-deb0caf14b7a/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CVW4W75%2F20261106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20261106T040904Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJHMEUCIQC0XHHmpGg8vA9GCofQUPphdg5Tt84olCfazWyatS2ySwIgXxrcHXpDa3IBhLUQutnPjvv%2FoRjCTmu1T9n%2FbGXSgvoqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHrepS6JCoCM43FgaircA8um0YLjBRIxbBH4FrjiAtikXoVLRqh84rViSbSyPbE2OlPr0vPkrPszLNpIrtGeAWwJzcNm0CnrlvdrnsKyeqfaKWp%2Bxszs03HaGh%2B0IHF%2BrR%2BvUrbNzVeBauTWERfmStNWP%2BDpot1nY1B3YwrY0cmsG6148VMUZjzZ60oSa11UK%2Bu36GAV4rxWnJHKCrhLuwP6Ajd6e0nfdGEARv2WJpCUvOr4MnwvHTidW%2FqZoTEj%2FmiDycqJi9UKXMda7vDFOuYgWKoCZRyul4m0cVbijwAvCv5PnKK6fVWpK6%2FOkU%2BRYCROFwwi7knLtaYesYq53%2Fev4x9l%2BL%2B6l6aBMwwo93x09qfpxXZPiV17FaCAUCaMmn7XPxF3epEW63seeEiB25mhFbYeTb2DFzvZQtLrcEQhMvB1eYHpPzKxxEtAK9MgB1A4iUMir2rJ%2FgNDk6O0SI0ctbzDZ1Dxjut3h80NCoU5CHzIa1Pw9H35cKvpXQlOMvYO7cBFbm7OZXmvm3%2B74b8cRKI30zdOeVtTCmZnmhcKaWEex65Q7fgaVOcTzz912IL1aoJFDN6NyeY%2By5LzojQZReZ%2FPWikdK5KHeq8WLvuVMSzIYPR71r9FGTZSbkYovcM9GZqmNACPBV%2FMJ6ItdcGOqUB%2FHscnJAOxw3Iuz7%2B%2FJlFFdN1zyl6ED2iwesLfNIiXNpZaYy4xhmlXZc6vDgXM%2FcAodLoFvnAKGvr5NPvq8jD2t213yDfE%2B8f7zSYAK7kwG8fU49AGLbxn%2F2OY16TloaLZ3XaIkEXna9u1z502hDsNfiXcLX4zD0GkywkYtaQl5MyYyyONQUPj9m3KO08aDiIWtMnc0exFKPDZRlD%2FrhxpRmKSSAM&X-Amz-Signature=719a18b355472916dac479e779f4db6961eddc0b0d3650f4e4015d560576ba42&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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
