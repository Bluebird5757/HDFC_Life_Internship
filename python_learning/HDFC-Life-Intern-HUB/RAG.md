---
notion_id: 3c84fa76-9938-8047-b507-c8020ca82aed
notion_url: https://app.notion.com/p/RAG-3c84fa7699388047b507c8020ca82aed
title: RAG
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-26T19:37:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-08-27T05:43:17.741Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RSJSSYW5%2F20260827%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260827T054314Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQC1%2F7J5z0a4VFpG%2Be0OfNkG0nrlcAx2V8kMz8rGEaJv%2BgIgFHRygU1muawVbn1hUduUMCpGrmGQ4FBgzkufnjfmhhEq%2FwMILBAAGgw2Mzc0MjMxODM4MDUiDMdTPyVuTHRzii8v9CrcA7nvXWfEk5dP6XLyba2fEESvJyKlXtw%2B71Z8drA%2BspIHjpGM5H06GpafAqCyy6TV5%2B2j6l7hQQYYnwMqofIs0PcLF1hqHHUs9y8pmBTz9Q8WDr5nmiozlziHdXdQTr1Tnl%2Bm%2Fif8WcJxYITx6DU%2BBWIkTEhY%2BUdArXrVTh4J0jrnnX%2FyV%2BepP2Zf9xFtRPTb75pNtzLRwaoXTYlObUf0pWQy1ii3YRDJ4TDlFOI5Ce7eqaRZE6EOySTqtAfNtPdhgloLS00M0nPJkOIlxf3rUr1i0SnKX3eaormUNNR0RRDhMxq5F8Da5GFERdfr38FGH0Svncu4wz6K%2FzKUVHm7iaPLEE9REmhRz%2FECF7Pi1HFK%2BipzmcGfnOsbLLrRVNcS5%2FM6OiX%2BiWr3uFSBqOHShArifh7cpVOmzxm4f8MO4G7%2FvQ%2Bf2vLFnC%2BVZEv4FDOX23cexran8UMgXCia5E3zcVnKGz8b7A8FeJ7%2BsIg6r3QhnOZtVwcw3OO1bo7ISB%2B2Xz3qaYsGEzyQ868AwAuOIeKHJgBlxRlN8IizTZw%2Bz0Qr5xU607zVQrv7DvojKfd7C3tYz3Dz2KmD7%2BM9CFytyEMtgnkshj1O9qeSnaXsBhetQc%2Br3xF3W6gHkr6tMIDbvtQGOqUBOUKWnSAy3lNau2WVP%2FKiBFSKiLaurcYn8NvMf5FxR3aeEASgQofhN4ybNSAr8wzVAwsC1XB8ft2MECXtILAYL5okO0gc5UVjWVDocieIi2X56JPlqVaet%2BmuAAWSqET79va5wiulozdL2OR8tI5eGcDRtY34R6lfRCD977BtRELcqSowB7eZbaO6PBaRobG64Lv0PWH5bstbwluCDNE6plT0IeLs&X-Amz-Signature=d278e7eb594d9891073f0e873a5f03a51bba0fe0be1cd7df254e7c88016723f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QMSPSVDO%2F20260827%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260827T054314Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIFXTbJIrE%2BVhiVqtHHqh7gTiHdC9liFfdVc3XY3bGK6LAiEAyoGo2X93%2FQcamN4hXbI9Ftx6qLi2yuZrRxeFSCM0nN0q%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDKU2%2F10ptG%2BNyx%2BCrCrcA809L25d%2Bf5uD%2FIk%2F0lx%2Fc1E0LKJ7%2F5E0XPjaCiGzNHR9BYAGKaUGaoNYUUaG80RigsRsvWN3aS%2F4liSX91NaEXsCgFcDuGPbLeAVxANathhctpvFX4SY80tKTiG%2FKSfErGDVCsPw7JVszN2fTUQo1A8LdTKfSuu1VtBdQcYoXFR00AP8D%2FN%2BWGWqLBXqoP5EjK2%2FxO2agjNwmL2T5%2FMo8OedKnznwKdaJKn9wzWaJmYfQN2z1MWJ%2BuddLu%2FKxyEBzR6vQdBL6Be3OOLVIbQerZLEguxiaVmctk6OvXYh7Sa%2BC%2BP9AzL%2BldvXN2xYFy0fEygefckZDiiNE%2FQKK1btr5Fvvqf34qC7EwiBgycIQWxZby4mZ21oOU5gCK%2FjqxImr0ILllsNokOKJY7raGrMIvs637Z2q5%2BkQ7lc%2BkkugqBtSmADs4R5mK6bPXB9jS2o%2BWZLDmRye1ezga0H92UIy9iYQzkA0oJBEsWUgJSrAJGdGNWGjL4BqO58Bxx6%2FPlkfxJGyUjM0YiwbT2xvDSm4F6Z%2BgqzgjBcpkksxHVO7ecD9mZw%2FRbOzacWj05IHY0XAvry4F%2FuWogtZ8RPtYcl3p%2BkUIP7CK%2F4lE9uTtHED%2BYObUtgVdNcyl7QdUXMOzbvtQGOqUBIwfN3YXw2kK1S7VlrYI3gxDVdk9ErrWR9KMklE76HrhMK1l2ilOosPtzw0pdni1bU0Gghk0FH9PaphMaC8AjTnyMJPFhq9mpeaLt5qvbQolI8pzuAPxnGjO%2Bny3dDGSoAABa5c3fBONj7wvKk3hRTDUN5Pn%2B6NMSiUCZlIYIrdE2YEsV7E4uLjcNZFRowz1%2BRlknCzxZZAMV8o0BSGGJCI7y%2FdV5&X-Amz-Signature=abc9c3d976387ac0cfebcd9163bd0d5b2d90e0491e3b23b0cddcc72f140ef0ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
