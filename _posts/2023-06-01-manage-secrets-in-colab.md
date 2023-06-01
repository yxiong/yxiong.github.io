---
title: Managing Secrets in Colab
date: 2023-06-01
---

To properly manage (hide) a secret, e.g. an API key, in colab, one can use
GCP's [Secret Manager](https://cloud.google.com/secret-manager). Here are some
instructions, borrowed from [this stackoverflow
answer](https://stackoverflow.com/a/64005794/4195568):

1. Enable "Secret Manager API" for the project on market place:
   https://console.cloud.google.com/marketplace/product/google/secretmanager.googleapis.com
2. Create a secret in "Secret Manager": https://console.cloud.google.com/security/secret-manager
3. Copy the resource name from the console
   ![copy-resource-name](/assets/images/20230601-copy-resource-name.png)
4. Access the resource key using the python script below
   ```python
   from google.cloud import secretmanager
   client = secretmanager.SecretManagerServiceClient()
   resource_name = "xxxxxxx"  # String copied from above
   response = client.access_secret_version(request={"name": resource_name})
   OPENAI_API_KEY = response.payload.data.decode('UTF-8')
   ```
