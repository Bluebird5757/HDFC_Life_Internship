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
fetched_at: '2026-08-28T07:51:44.096Z'
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

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/ab421347-0f53-47de-af9c-b98e350eef48/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YOXGMCOR%2F20260828%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260828T075140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJGMEQCIDUYXuY8vca4GWKq7T1UOT1gQafvGLB7TmhRxj5iswWtAiAMSi1ti62lf1RyD7ooyYZp5sGk6LTWJx%2Fxknicrm0qcSr%2FAwhIEAAaDDYzNzQyMzE4MzgwNSIMefHDOQSWavxdx97XKtwDqmA3F5osB7gCiLjcecM2zF7xSgWuGlTOgbfkvu42KGElCzRQQKgjVzLauu%2F%2BfdZ0L1Q2zZiNT0FFWUDFX43d2SLnD2dJbwQ%2Fx6qc%2BzHMMAFKeXqpSzGDvmxlW4t08b7pPKwcWaNI5oU6x%2BdzBmmK7AAvp9QGZFDCvM4I4Yc5gd1YdNsJvLtcqjctN5baUGLyWwHxuBr8RPUdsdmoaeflo0iYZ81u3KSvQFIjzQpI3g1DcnbAAkdyl%2BHQbtg%2BAdTaVN1tdM8M4pUVWHRe%2BZTFVmNqaBBGXuOD%2BVdsz3eKKFZ4%2FyM3o1vuc18EsEOX8xkzqtcnGtfBRjV58DXZfgfcsQPSMlAmoN551XEh2wgUsoCVf4lvPg56YXcFTLEZsFPGn4xrFIYmrYEmFvNJs%2F7eOypZfDh8t74JuVqk8xMU25qxxFyPN2FrDA%2BS8D%2F8SLzndQ719wa1A7RR0zcPq1kHeTKHfpCUgf1q76pmKdu5EA%2FyMMM0GVkpRs1OnpJiDuryt%2B8BZsyQxuoWIwTyWNqTwV3hVd73sskvuAb9lKqkawgNcYxcW0E5tSL5OxHBvn7pxlyFavFEyZyqsVbujlGPfuaiPNnl4bqEFscnk7y%2FPKJxLpuUuS5%2B%2Fr1oGfowq9XE1AY6pgHK1Aqt7HFUOzHx%2B6byvFEa7xDyoA7C4sBXcTaB3kSdb5WbIDQZ%2FSjTPD4BDmJg5eQIJj%2FqUAIjupfm4TX5yhRZOfV4PY9xB3rb8Xm%2BW2PiUVHl9tO6Qvovy0taoFKONgLUdhIraM7%2Faj%2BvBBYp0wgQe2ReQbXeouVDz1%2B5GnRgNlCYXmxKRsJ44iTUKmjvFsyRXl%2BSU5vzIPMCS0RFHcR7T1TQ2XH1&X-Amz-Signature=5f1c635cf92c379d58b62b9e8c5c47e6dfd04925c6cc2d4754f6465da3d54f2e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

    - CSVLoader

    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/4c6856da-5f7d-49ff-93af-fe22c177cff3/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EIOIFVX%2F20260828%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260828T075140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJIMEYCIQDvRqjzp4F4FinojyZouDCq0qgIKr9yOlb61r%2BP5wGRbAIhANKEZVxeZJoLAJeKT0rVCh2T5xVaNrHkwsKMe6KtmTT5Kv8DCEcQABoMNjM3NDIzMTgzODA1IgwZ5G9RshRiQAIIqaYq3AP58dupQeSA3zsLx%2FTeCSWcwaq3bh7smOyIhKmeH%2B%2F%2B0eSuupcCW%2BX1OhUnmsPfxHI%2BZ%2BQxSqh2gZrXE2hrmsuNsL2Gtp1MIfxfeHW4jWPQTRTDWd1jWeWyJrlRxkiUcwdIQhEijjqLYjZHYjHnwzXd9v4vWU7YHLPov1OOF698b0Hiy2I8RCbBUVOiO%2F9zWPrvP80%2F2aouD1WvtP4KCE3zLSmhToR%2FTksnacnimB6vm9ARIYpOMJDTntmZlRLcL%2BrVqdM4L59m5BSbO9IPNTADZ1fku%2FBh5VUGVVuNmadZc%2FV61aJh%2BPnVVku14lLhlUvSt5tGpclkIy3leI9t2RA6hPZiTS0Oh93c2lsGhm1WMYIxEIujfkHizsSMP8HH2PcvwHVcDMUWyWGnDl6jNPPoDzligLQCpcOxsne%2B5FxxUBmATOKkMsg6Am1LC6LaoKU%2BM%2FyRqo3M5brvWp0G7UuqdMo2Saw6dNUXHWwKRxKrAJZTfryrJIsZbCAe%2Bl%2Bp0JsxkS%2FAiEvG%2BbWIsLfvCr%2Bsg1TvtxZ4HFQuHHQ3fOxfs8%2Fb2hvVow546hz6%2F%2FWDpIo3y1jo27dsmLkT7eA36Upml9rJtzOrdvScFxBlvf2l%2FLGhQYhH9d6TLoXTNzDX08TUBjqkAQXTsn10nXCEuUV9dOEUt5DkJUmfWUda91CXseCpbqYBXjZxkTlePLtSUGk6XiW7ZKQYIJ5Mc%2BOGqFh0jSnUlLbqihmqImNCPQGHRnpyUWnJNuua48Rf%2BOKhviHstDeIkEblfpT2qDhT8FlgZCPpp9q5cQbh38iJJIkZYSfwIt02fMf%2BJNJ1AB8IWAy2nhkjlRQo0fTcqolFHnkWT8QKlliK%2FnM2&X-Amz-Signature=2dde5d23867972d42de0f6ceb5e81c195f9f4f03352a208106a0cae3d5e6df48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
