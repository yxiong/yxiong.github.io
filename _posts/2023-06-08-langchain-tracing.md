---
title: Tracing
date: 2023-06-08
---

The [langchain tracing](https://python.langchain.com/en/latest/additional_resources/tracing.html) is a very useful tool for debugging.
To use it,
* Run `langchain-server` in the local environment
* Add the following at the top of the application code:
  ```python
  import os
  os.environ["LANGCHAIN_HANDLER"] = "langchain"
  ```

Unknown issues / future improvements:
* Currently I am only able to use tracing in my local machine
* The embeddings API calls do not show up in tracing
