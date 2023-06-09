---
title: Langchain Tracing
date: 2023-06-08
---

The [langchain tracing](https://python.langchain.com/en/latest/additional_resources/tracing.html) is a very useful tool for debugging.
To use it,
* Run `langchain-server` in the local environment, the UI is accessible at http://localhost:4173/
* Add the following at the top of the application code:
  ```python
  import os
  os.environ["LANGCHAIN_HANDLER"] = "langchain"
  ```
* (Optional) Create a new session from the UI, and then add `os.environ["LANGCHAIN_SESSION"] = "my_session"` to the application code

Unknown issues / future improvements:
* Currently I am only able to use tracing in my local machine
* The embeddings API calls do not show up in tracing
