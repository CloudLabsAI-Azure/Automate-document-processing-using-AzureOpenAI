# Automate document processing using AzureOpenAI

### Lab Overview

Processing of forms and documents is part of several scenarios both in business and in everyday life. Manual data extraction from documents, either in electronic or printed format, is time-consuming, costly, and error-prone.

Azure Form Recognizer is an Applied AI Service that enables you to extract text, table data, key-value pairs, and layout information from forms and documents. In this lab, you will learn how to use train documents via Document Intelligence resource. We will be processing the documents to Cosmos DB via Azure functions and further analysing the document in Power BI. 

### Lab Objectives

In this lab, you will perform:

- Provision a Form Recognizer resource.
- Train a custom model in Form Recognizer Studio.
- Create an Azure Functions project.
- Store the extracted information from the Form Recognizer’s model in Azure Cosmos DB.
- Use Power BI to visualize the insights from the analysis of the forms.

### Instructions

### Task 1: Creating a Document Intelligence Resource

1. Search for **Document Intelligence** and select it.

   ![Alt text](images/1-9.png)

1. Navigate to **Document-Intelligence-<inject key="Deployment ID" enableCopy="false"/>**
     

1. In the **Overview** pane, under **Document Intelligence studio** ,click on **Try it**.

   ![Alt text](images/1-3.png)

1. In **Document Intelligence studio**, scroll down to **Custom Models** and click on **Get Started**.

   ![Alt text](images/1-4.png)

1. Enter the following details and click on **Continue**  **(3)**.
    
   - Project name: **testproject** **(1)**.
   - Description: **Custom model project** **(2)**.

     ![Alt text](images/enter-project-details.png)

1. Enter the following details **Configure service resource** and click on **Continue** **(5)**.

   - Subscription: Select your **Default Subscription** **(1)**.
   - Resource group: **OpenAI-<inject key="Deployment ID" enableCopy="false"/>** **(2)**.
   - Document Intelligence or Cognitive Service Resource: Select **Document-Intelligence-<inject key="Deployment ID" enableCopy="false"/>** **(3)**.
   - API version: **Select the default version** **(4)**.

     ![configuring service resource](images/1-5.png)

1. Enter the following details **Connect training data source** and click on **Continue** **(5)**.

   - Subscription: Select your **Default Subscription** **(1)**.
   - Resource group: **Document-intelligence-<inject key="Deployment ID" enableCopy="false"/>** **(2)**.
   - Storage account name: Select **storage<inject key="Deployment ID" enableCopy="false"/>** **(3)**.
   - Blob container name: **analysis** **(4)**.
   
        ![storage account](images/doc1.png)

1. Validate the information and choose **Create project**.

     ![Alt text](images/doc2.png)

### Task 2: Train and Label data

In this step, you will upload 6 training documents to train the model.

1. Click on **Browse for files**.

     ![Browse for files](images/browse-for-files.png)

2.  On the file explorer, enter the following `C:\LabFiles` **(1)** path hit **enter**, select all train JPEG files **invoice_1 to invoice_5** **(2)**, and hit **Open** **(3)**.


3. Once uploaded, choose **Run now** in the pop-up window under Run Layout.

     ![train-upload](images/run-now.png)

4. Click on **+ Add a field** **(1)**, select **Field** **(2)**, enter the field name as **Organization_sample** **(3)** and hit **enter**.

     ![run-now](images/doc3.png)

     ![run-now](images/add-field-name.png)

5. Label the new field added by selecting **CONTOSO LTD** in the top left of each document uploaded. Do this for all six documents.

     ![train-module](images/train-module.png)

6. Once all the documents are labeled, click on **Train** in the top right corner.

     ![Train](images/train-module1.png)

7. Specify the model ID as **customfrs** **(1)**, Model Description as **custom model** **(2)**, from the drop-down select **Template** **(3)** as Build Mode and click on **Train** **(4)**.

     ![Name](images/train-a-new-model.png)

8. Click on **Go to Models**. 

   ![Alt text](images/training-in-progress.png)

9. Wait till the model status shows **succeeded** **(1)**. Once the status Select the model **customfrs** **(2)** you created and choose **Test** **(3)**.

     ![select-models](images/select-models1.png)

10. On the Test model window, click on **Browse for files**. 

     ![select-models](images/test-upload.png)

11. On the file explorer, enter the following `C:\LabFiles` **(1)** path hit **enter**, select all test JPEG files **test1 and test2** **(2)**, and hit **Open** **(3)**.

12. Once uploaded, select one test model, and click on **Run analysis** **(1)**, Now you can see on the right-hand side that the model was able to detect the field **Organization_sample** **(2)** we created in the last step along with its confidence score.

### Task 3: Deploy Function App

1. Navigate to the document intelligence resource you created earlier.

1. Navigate to **Keys and Endpoint (1)** under Resource Management pane, copy the **key (2)** and **endpoint (3)** in a notepad.

   ![select-models](images/doc4.png)
   
1. Navigate to **storage<inject key="Deployment ID" enableCopy="false"/>**

1. Navigate to **Access Keys** under Security + Networking pane and copy the **Connection String** in a notepad.

   ![select-models](images/doc5.png)

1. Click on **Containers** under the **Data Storage tab** in the left pane and select the **input** container.

   ![select-models](images/doc6.png)

1. Click on **Shared Access Tokens (1)** under the **Settings** tab from the left pane, provide **Read, Add, Create and Write (2)** permissions and click on **Generate SAS Token and URL (3)**.

   ![select-models](images/doc7.png)
   
1. Navigate to **Visual Studio Code**, click on **Open Folder (1)** , navigate to **C:/Labfiles (2)** and select **funtion-app (3)**.

   ![select-models](images/doc8.png)

1. Click on the **Azure symbol (1)** , select **Create Function (2)** by clicking on the **funtion-app icon (3)**

   ![select-models](images/doc9.png)

1. You'll be prompted to configure several settings:

   - Select the folder → choose **function-app**.
     
   - Select a language → choose **Python**.

   - Select a Python Programming Model → choose **Model V2**.

   - Select a Python interpreter to create a virtual environment → select **Python 3.11**.

   - Select a template → choose **Blob trigger** and give the trigger a name or accept the default name. Press **Enter** to confirm.

   - The path within your storage container that the trigger will monitor → **input**.

   - Select setting → choose ➕**Create new local app setting** from the dropdown menu.

   - Select subscription → choose your **Azure subscription** with the storage account you created → select your **storage account** → then select the name of the storage input container. Press **Enter** to confirm.

   - Select how your would like to open your project → choose **Open the project in the current window** from the dropdown menu.

1. In VS Code, navigate to the function's **requirements.txt** file. This file defines the dependencies for your script. Add the following Python packages to the file:
   
      ```
      cryptography
      azure-functions
      azure-storage-blob
      azure-identity
      requests
      pandas
      numpy
   
      ```

      ![select-models](images/doc10.png)

1. Open the **local.settings.json** and add the following statement and fill the respective values:

   ```
      {
     "IsEncrypted": false,
     "Values": {
       "AzureWebJobsStorage": "<Connection-string>",
       "FUNCTIONS_WORKER_RUNTIME": "python",
       "AzureWebJobsFeatureFlags": "EnableWorkerIndexing",
       "storageaccount-name_STORAGE": "<Connection-string>",
       "AzureWebJobsSecretStorageType": "Files"
     }
   }
   ```
   
1. Open the **function-app.py** file and add the following import statements:

      ```
   
      import logging
      from azure.storage.blob import BlobServiceClient
      import azure.functions as func
      import json
      import time
      from requests import get, post
      import os
      import requests
      from collections import OrderedDict
      import numpy as np
      import pandas as pd
   
      ```

      ![select-models](images/doc11.png)
   
1. Add the following code to trigger the script when a file is uploaded to the input storage container. Replace the values of **container-name** and **storage-account-name** with **input** and **storage<inject key="Deployment ID" enableCopy="false"/>** respectively.

      ```

      app = func.FunctionApp()
   
   @app.blob_trigger(arg_name="myblob", path="<container-name>", connection="<storage-account-name>_STORAGE")
   
   def blob_trigger(myblob: func.InputStream):
       logging.info(f"Python blob trigger function processed blob"
                   f"Name: {myblob.name}" 
                   f"Blob Size: {myblob.length} bytes")

      ```

1. Add the following code block that calls the **Document Intelligence Analyze Layout API** on the uploaded document. Replace **endpoint, key value and model name** with the ones we have copied in the notepad earlier.

   ```
   
   # This is the call to the Document Intelligence endpoint
       endpoint = r"Your Document Intelligence Endpoint"
       apim_key = "Your Document Intelligence Key"
       post_url = endpoint + "/formrecognizer/documentModels/**<MODEL-NAME>**:analyze?api-version=2023-02-28-preview"
       source = myblob.read()
   
       headers = {
       # Request headers
       'Content-Type': 'application/pdf',
       'Ocp-Apim-Subscription-Key': apim_key,
           }
   
   ```

1. Next, add code to query the service and get the returned data.

   ```
   
      resp = requests.post(url=post_url, data=source, headers=headers)
   
   if resp.status_code != 202:
       print("POST analyze failed:\n%s" % resp.text)
       quit()
   print("POST analyze succeeded:\n%s" % resp.headers)
   get_url = resp.headers["operation-location"]
   
   wait_sec = 25
   
   time.sleep(wait_sec)
   # The layout API is async therefore the wait statement
   
   resp = requests.get(url=get_url, headers={"Ocp-Apim-Subscription-Key": apim_key})
   
   resp_json = json.loads(resp.text)
   
   status = resp_json["status"]
   
   if status == "succeeded":
       print("POST Layout Analysis succeeded:\n%s")
       results = resp_json
   else:
       print("GET Layout results failed:\n%s")
       quit()
   
   results = resp_json
   
   ```

1. Add the following code to connect to the Azure Storage output container. Fill in the values for the **storage account name and key value**.

   ```
   
   # This is the connection to the blob storage, with the Azure Python SDK
       blob_service_client = BlobServiceClient.from_connection_string("DefaultEndpointsProtocol=https;AccountName="Storage Account Name";AccountKey="storage account key";EndpointSuffix=core.windows.net")
       container_client=blob_service_client.get_container_client("output")
   
   ```

### Task 4: Run the Function App

1. Press F5 to run the function

1. Once the funtion has been run successfully, navigate to `portal.azure.com`.

1. Navigate to storage account and click on the output container.

1. Click on the folder created as a result of the trigger and verify the json analysing the document has been generated.

### Task 5: Working with AI Search

1. Search for **AI Search** and select it.

1. Navigate to **search-<inject key="Deployment ID" enableCopy="false"/>**.

1. Click on **Import data**.

1. 
### Task 6: Utilize Cosmos DB to process the documents


### Task 7: Visulalize with PowerBI

## Review

In this lab, you have accomplished the following:
