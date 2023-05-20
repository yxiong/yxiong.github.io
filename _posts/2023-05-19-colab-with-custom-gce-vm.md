---
title: Using Colab with Custom GCE VM
date: 2023-05-19
---

## Instructions

Following the instructions in https://research.google.com/colaboratory/marketplace.html. Here are a few detailed steps and notes:

0. Set up GCP account, project, billing, etc.
1. Launch a pre-configured VM from the marketplace image
   https://console.cloud.google.com/marketplace/product/colab-marketplace-image-public/colab
   * Remember to select the right project
   * Deployment name: e.g. `colab-server-gpu`
   * Zone: need to find an availability zone to have enough resource. I currently do it through trial and error. The one I found this time was `us-west4-a`.
   * Machine type: I used `n1-highmem-8`
   * GPUs: I chose `NVIDIA T4` and number of GPUs is 1
2. Go to https://colab.research.google.com/ and "connect with a custom GCE VM".
   * Fill in `project`, `zone` and `instance`
   * Note that `instance` has a `-vm` suffix
3. Verify that we can ssh into the VM:
   * Use the SSH button from the console
   * Run `gcloud compute ssh colab-server-gpu-vm` in local machine

## Notes

### Pros

* The connection is stable. Without custom VM, the colab gets disconnected because of inactivity, even with with the paid Colab Pro version.
* The pip libraries and cache files are persistent from one run to another.

### Cons

* It costs money..

### Known Issues

* `google.colab.auth` is not supported, see https://github.com/googlecolab/colabtools/issues/2533
  * This means we cannot easily access Google Drive or Google Cloud Storage.
  * __[Workaround for GCS]__: Use a key file
    1. Console > IAM & Admin > Service Accounts: CREATE SERVICE ACCOUNT
    2. 3 Dots > Manage Keys: ADD KEY
    3. Cloud Storage > Bucket > Select Bucket > PERMISSIONS: Grant the following role:
       * Storage Legacy Bucket Writer
       * Storage Object Creator
    4. In colab, upload the key, e.g. `gcs-access-key.json`.
       ```
       from google.colab import files
       uploaded = files.upload()
       ```
    5. Set the environment variable.
       ```
       import os
       os.environ["GOOGLE_APPLICATION_CREDENTIALS"] = "gcs-access-key.json"
       ```
* After stopping and restarting the VM, I cannot connect to colab to it any more.
