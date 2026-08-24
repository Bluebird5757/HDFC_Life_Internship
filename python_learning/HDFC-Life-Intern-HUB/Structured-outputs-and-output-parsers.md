---
notion_id: 3c54fa76-9938-8030-91a9-f07271d3b76e
notion_url: https://app.notion.com/p/Structured-outputs-and-output-parsers-3c54fa769938803091a9f07271d3b76e
title: Structured outputs and output parsers
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-23T19:29:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-08-24T00:40:02.243Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

The output we get from an llm can be structured in such a way that we can use it to further communicate with other llms or apis or tools like agents.


so there are models like openai that can return structured outputs with the help of with_structured_output which is built in langchain and we just have to specify the format of the output that we want for which we have three ways:-

- Pydantic
- TypeDict
- json_schema

but many of these models are closed source, for open source models on huggingface who cant return a structured outputs we have output parsers which help us turn a raw output from a LLM into structured formats like JSON, CSV, Pydantic models, and more. One thing to note is that output parsers can be used with both LLMs who can and cant return a structured outputs


so there are four output parsers:-

- StrOutputParser:- it is the simpliest output parser in langchain used to parse the output of a language model and return it as a plain string
- Json output parsers:-
- structured output parsers
- Pydantic output parsers
