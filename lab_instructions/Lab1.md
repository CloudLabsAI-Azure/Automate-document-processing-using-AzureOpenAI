# Lab 01: Automate Document Processing using Azure AI Document Intelligence

## Estimated Duration: 3 Hours

Processing of forms and documents is part of several scenarios both in business and in everyday life. Manual data extraction from documents, either in electronic or printed format, is time-consuming, costly, and error-prone. Document processing using Azure involves leveraging Azure services and tools to analyze, extract information from, and manage various types of documents, such as text files, images, PDFs, and more. This process typically includes tasks like text extraction, data extraction, sentiment analysis, language detection, optical character recognition (OCR), and document classification. In this lab, you will learn how to train documents via the Document Intelligence resource. We will be processing the documents via Azure functions and <code style="color : red">**Azure AI Document Intelligence**</code>.

## Architecture Diagram

   ![Name](images/archi1.PNG)

## Lab Objectives

In this lab, you will perform:

- Task 1: Creating a Document Intelligence Resource
- Task 2: Train and Label data
- Task 3: Creation of Function App
- Task 4: Run the Function App
- Task 5: Working with AI Search

## Task 1: Creating a Document Intelligence Resource

1. Search for **Document intelligences** and select it.

   ![Alt text](images/1-9.png)

1. Navigate to **document-intelligence-<inject key="Deployment ID" enableCopy="false"/>** by selecting it.

   ![Alt text](images/100725(01).png)

1. In the **Overview (1)** pane, scroll down to the **Get Started** tab and click on **Go to Document Intelligence Studio (2)** and sign in with the same user details used to log in to Azure.

   ![Alt text](images/100725(02).png)

1. In **Document Intelligence Studio** page, scroll down to **Custom models** and click on **Get started** for Custom extraction model.

   ![Alt text](images/1-4.png)

   > **Note:** If prompted to log in, use the credentials provided in the Environment tab.

1. Click on **+ Create a project**. Enter the following details and click on **Continue**  **(3)**.

   ![](images/100725(03).png)
    
   - Project name: **testproject** **(1)**.
   - Description: **Custom model project** **(2)**.

     ![Alt text](images/new-automate-lab1-1.png)

1. Enter the following details for **Configure service resource** and click on **Continue** **(5)**.

   - Subscription: Select your **Default Subscription** **(1)**.
   - Resource group: **OpenAI-<inject key="Deployment ID" enableCopy="false"/>** **(2)**.
   - Document Intelligence or Cognitive Service Resource: Select **document-intelligence-<inject key="Deployment ID" enableCopy="false"/>** **(3)**.
   - API version: Select **2023-07-31(3.1 General Availability)** **(4)**.

     ![configuring service resource](images/imag2-upd.png)

1. Enter the following details for **Connect training data source** and click on **Continue** **(5)**.

   - Subscription: Select your **Default Subscription** **(1)**.
   - Resource group: **OpenAI-<inject key="Deployment ID" enableCopy="false"/>** **(2)**.
   - Storage account: Select **storage<inject key="Deployment ID" enableCopy="false"/>** **(3)**.
   - Blob container: **analysis** **(4)**.
   
        ![storage account](images/doc1.png)

1. Review the configuration and select **Create project**.

     ![Alt text](images/doc2.png)

## Task 2: Train and Label data

In this step, you will upload 6 training documents to train the model.

1. Click on **Browse for files**.

     ![Browse for files](images/doc39.png)

1. On the file explorer, enter the following path `C:\LabFiles\Train`, hit **enter**, select all train PDF files present inside **Train** folder, i.e **Invoice_1 to Invoice_5** **(2)**, and hit **Open**.

   ![Alt text](images/doc69upd1.png)

1. Once uploaded, choose **Run now** in the pop-up window under Run Layout.

     ![train-upload](images/doc31.png)

1. Click on **+ Add a field** **(1)**, select **Field** **(2)**, enter the field name as **Organization** **(3)** and hit **enter**.

     ![run-now](images/doc3.png)

     ![run-now](images/doc14.png)

1. Ensure you have selected **Invoice_1** and label the new field added by selecting **Contoso (1)** in the top left of each document uploaded. Do this for all five documents wherever there is an organization mentioned.

     ![train-module](images/doc15.png)

1. Click on **+ Add a field** **(1)**, select **Field** **(2)**, enter the field name as **Address** **(3)** and hit **enter**.

   ![Alt text](images/doc3.png)

   ![Alt text](images/imag1.png)

1. Label the new field added by **selecting the address (2)** as shown in the image below, and do this for all the **five documents**.

   ![train-module](images/doc16.png)
   
1. Once all the documents are labeled, click on **Train** in the top right corner.

     ![Train](images/doc17.png)

1. Specify the model ID as **model (1)**, Model Description as **custom model (2)** , from the drop-down select **Template (3)** as Build Mode and click on **Train (4)**.

     ![Name](images/doc68.png)

1. Click on **Go to Models**. 

   ![Name](images/doc32.png)
   
1. Wait till the model status shows **Succeeded**. Once the status has succeeded, Select the model **model**  **(1)** you created and choose **Test** **(2)**.

     ![select-models](images/doc19.png)

1. On the Test model window, click on **Browse for files**. 

     ![select-models](images/test-upload.png)

1. On the file explorer, enter the following `C:\LabFiles\Test` **(1)** path hit **enter**, select all test PDF files **Invoice6 and Invoice7** **(2)**, and hit **Open** **(3)**.

      ![select-models](images/stu6upd.png)

1. Once uploaded, select one test model and click on **Run analysis**. Now you can see on the right-hand side that the model was able to detect the field **Organization** and **Address** we created in the last step, along with its confidence score.

   ![Name](images/stu7-upd.png)
   
## Task 3: Creation of Function App

You will be using Azure Functions to process documents that are uploaded to an Azure blob storage container. This workflow extracts table data from stored documents using the Document Intelligence layout model and saves the data in a JSON file in Azure.
   
1. Open **Visual Studio Code** from the Lab VM desktop by double-clicking on it. 

   ![select-models](images/vs-code-1.png)

1. Once inside VS-Code, click on **File -> Open Folder**.

   ![select-models](images/vs-code-2.png)

3. Now, navigate to `C:/Labfiles` and select **function-app (2)** folder and then select **Select Folder (3)**.

   ![select-models](images/doc8upd.png)

1. On the **Do you trust the authors of the files in this folder?** tab, select **Yes, I trust the authors**.

      ![select-models](images/yesauth.png)

1. Click on the **Azure symbol (1)** , select **function-app icon (2)** and then **Create Function.. (3)**

   ![select-models](images/doc9.png)

1. You'll be prompted to configure several settings:

   - Select the folder → choose **function-app**.
     
   - Select a language → choose **Python**.

   - Select a Python interpreter to create a virtual environment → select **Python 3.11.9**.

   - Select a template → choose **Blob trigger** and give the trigger a name or accept the default name. Press **Enter** to confirm.

   - The path within your storage container that the trigger will monitor → **input**.

   - Select setting → choose **+ Create new local app setting** from the dropdown menu.

   - Click on **Sign in to Azure** and click on **Allow** if prompted. This will navigate to the Azure Portal and select your Azure Account.

        - Email: <inject key="AzureAdUserEmail" enableCopy="true"></inject>

        - Password: <inject key="AzureAdUserPassword"></inject>

   - In the pop-up click on **No,this app only**.

      ![select-models](images/pop-upupd.png)

   - Select subscription → choose the **Default Subscription**.

   - Select a storage account type for development → choose **Use Azure Storage for remote storage** and select **storage<inject key="Deployment ID" enableCopy="false"/>** → then select the name of the storage **input** container. Press **Enter** to confirm.

   - Select how you would like to open your project → choose **Open the project in the current window** from the dropdown menu.

1. In VS Code, navigate to the function's **requirements.txt** file. This file defines the dependencies for your script. Add the following Python packages to the file and click on ` Ctrl + S ` :
   
      ```
      cryptography
      azure-functions
      azure-storage-blob
      azure-identity
      requests
      pandas
      numpy
      ```

1. From the terminal, please run `pip install -r requirements.txt` to install all the requirements.

      ![select-models](images/doc10.png)

1. Click on the **local.settings.json** file 
   - Replace **AzureWebJobsStorage** value with the storage account connection string **<inject key="connectionString" enableCopy="false"/></inject>**
   -  Update the configuration by adding **`"AzureWebJobsSecretStorageType": "Files"`** if it isn’t already included.
   - Replace **storageaccount-name** in storageaccount-name_STORAGE with **storage<inject key="Deployment ID" enableCopy="false"/>** and its value with the storage account connection string **<inject key="connectionString" enableCopy="false"/></inject>**

      ```json
         {
        "IsEncrypted": false,
        "Values": {
          "AzureWebJobsStorage": "<Connection-string>",
          "FUNCTIONS_WORKER_RUNTIME": "python",
          "AzureWebJobsFeatureFlags": "EnableWorkerIndexing",
          "storageaccount-name_STORAGE": "<inject key="connectionString" enableCopy="false"/></inject>",
          "AzureWebJobsSecretStorageType": "Files"
        }
      }
      ```

1. Open the **function-app.py** file and add the following import statements by replacing the existing ones:

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

      ![select-models](images/change_code-11upd.png)
   
1.  Modify the main function, where replace:

    - path = **input**
    
    - connection = **storage<inject key="Deployment ID" enableCopy="false"/>_STORAGE**
      ```
      app = func.FunctionApp()
   
      @app.blob_trigger(arg_name="myblob", path="<container-name>", connection="<storage-account-name>_STORAGE")
   
      def blob_trigger(myblob: func.InputStream):
          logging.info(f"Python blob trigger function processed blob"
                      f"Name: {myblob.name}" 
                      f"Blob Size: {myblob.length} bytes")
      ```

1. Add the following code block that calls the **Document Intelligence Analyze Layout API** on the uploaded document.
   - Replace r**Your Document Intelligence Endpoint** : **<inject key="documentIntelligenceEndpoint"></inject>**.
   - Replace **Your Document Intelligence Key** : **<inject key="documentIntelligenceKey"></inject>**.
   - Replace `<model-name>` with **model**.

      ```
         # This is the call to the Document Intelligence endpoint
         endpoint = r"Your Document Intelligence Endpoint"
         apim_key = "Your Document Intelligence Key"
         post_url = endpoint + "/formrecognizer/documentModels/<MODEL-NAME>:analyze?api-version=2023-07-31"
         source = myblob.read()
      
         headers = {
         # Request headers
         'Content-Type': 'application/pdf',
         'Ocp-Apim-Subscription-Key': apim_key,
            }
      ```

      ![select-models](images/stu1.png)
   
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

1. Add the following code to connect to the Azure Storage output container.
   
   - Replace {storage-connection-string} : **<inject key="connectionString" enableCopy="false"/></inject>**

      ```
         # This is the connection to the blob storage, with the Azure Python SDK
         blob_service_client = BlobServiceClient.from_connection_string("{storage-connection-string}")
         container_client=blob_service_client.get_container_client("output")
      
         # Assuming `results` is your JSON data
         data = json.dumps(results)

         # Create a new blob and upload the data
         blob_name = myblob.name + ".json"
         blob_client = container_client.get_blob_client(blob_name)
         blob_client.upload_blob(data, overwrite=True)
      ```

1. Please verify to ensure that the final code matches as below.

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
      
      app = func.FunctionApp()
      
      @app.blob_trigger(arg_name="myblob", path="<input container", connection="storage<DID>_STORAGE") 
      
      def blob_trigger(myblob: func.InputStream):
         logging.info(f"Python blob trigger function processed blob"
                     f"Name: {myblob.name}"
                     f"Blob Size: {myblob.length} bytes")
      
         # This is the call to the Document Intelligence endpoint
         endpoint = r"<document-intelligence-endpoint>"
         apim_key = "<document-intelligence-key>"
         post_url = endpoint + "/formrecognizer/documentModels/<model-name>:analyze?api-version=2023-07-31"
         source = myblob.read()
         
         headers = {
            # Request headers
            'Content-Type': 'application/pdf',
            'Ocp-Apim-Subscription-Key': apim_key,
                  }
         
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
      
         # This is the connection to the blob storage, with the Azure Python SDK
         blob_service_client = BlobServiceClient.from_connection_string("<storage account connection string")
         container_client=blob_service_client.get_container_client("output")
      
         # Assuming `results` is your JSON data
         data = json.dumps(results)
      
         # Create a new blob and upload the data
         blob_name = myblob.name + ".json"
         blob_client = container_client.get_blob_client(blob_name)
         blob_client.upload_blob(data, overwrite=True)
      ```
   > **Note**: Please make sure the indentation of the code remains unchanged and proper to run the code successfully

1. Open the **launch.json (1)** under `.vscode` **(2)** folder and replace the code with the below code

      ```
      {
         "version": "0.2.0",
         "configurations": [
            {
                  "name": "Python Debugger: Current File",
                  "type": "debugpy",
                  "request": "launch",
                  "program": "${file}",
                  "console": "integratedTerminal"
            },
            {
                  "name": "Attach to Python Functions",
                  "type": "debugpy",
                  "request": "attach",
                  "connect": {
                     "host": "localhost",
                     "port": 9091
                  },
                  "preLaunchTask": "func: host start"
            }
         ]
      }
      ```

      ![select-models](images/stu1aupd1.png)

## Task 4: Run the Function App

1. In Visual Studio Code, click the **ellipsis (**...**)** in the top menu, then expand **Terminal (1)** and select **New Terminal (2)** from the dropdown. 

   ![select-models](images/stu10upd.png)

1. Press **Ctrl + F5** to run the function.

   > **Note:** If you see any errors related to the debugger, then go to extensions from the left pane and install the **Python debugger extension**.

   > **Note:** Install the Python packages if required.

   > **Note**: If any pop-up occurs, close it.

      ![select-models](images/error.png)

1. Once the function has been run successfully, navigate to `portal.azure.com` when it triggers to add an input file as shown below.

   ![select-models](images/doc90.png)

1. In the search bar, search and select **Storage Account**.

1. Select storage account **storage<inject key="Deployment ID" enableCopy="false"/>**.

1. In the storage account **storage<inject key="Deployment ID" enableCopy="false"/>**, navigate to **Containers** under the **Data Storage** tab and select **input** container.

   ![select-models](images/new-automate-lab1-8upd.png)
   
1. In the input container, click on **Upload (1)** button, in the **Upload blob** pop-up window click on **Browse for files (2)**.

   ![select-models](images/stu3.png)

1. Navigate to `C:\LabFiles\test`, select **Invoice_6 and Invoice_7**, and click on **Open**.

   ![select-models](images/doc76(1).png)

1. In the **Upload blob** pop-up window, click on **Upload** button.

1. Navigate back to the **VS code** and verify the **logs**.

1. Once the function app is triggered successfully, navigate back to the **storage account**.

1. In the storage account, click on **Containers** under Data Storage tab and select **Output** container.

1. In the **Output** container, click on the **input** folder and verify the **json** analysing the document has been generated successfully.

   ![train-module](images/output.png)

>**Congratulations** on completing the Task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully validated the lab. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com.

<validation step="44d3193c-9401-4326-a2f5-067cf63f0c54" />

## Task 5: Working with AI Search

1. In the search bar, search for **AI Search** and select it.

      ![train-module](images/doc21.png)
   
1. In **AI foundry | AI Search** tab, select **search-<inject key="Deployment ID" enableCopy="false"/>**.

      ![train-module](images/doc22upd.png)
   
1. In the Overview page of **search-<inject key="Deployment ID" enableCopy="false"/>**, click on **Import data**.

      ![train-module](images/doc23.png)

1. Provide the following values:

      - Data Source: **Azure Blob Storage (1)**
      - Data Source Name: **data-source-<inject key="Deployment ID" enableCopy="false"/> (2)**
      - Parsing Mode: **JSON (3)**
      - Subscription: **Select the default subscription (4)**
      - Connection string: Click on **Choose an existing Connection** then Select **storage<inject key="Deployment ID" enableCopy="false"/>** and then select **output** container **(5)**.
      - Container Name: **output (6)**
      - Blob Folder: **input (7)**
      - Click on **Next: Add cognitive skills (Optional) (8)**

           ![select-models](images/stu5(1).png)
     
           ![train-module](images/doc24.png)

1. On the Add **cognitive skills (Optional)**, click on **Skip to : Customize target index**.

1. On the **Customize target index**, enter Index name as **azureblob-index** **(1)**, make all fields **Retrievable** **(2)**, and **Searchable** **(3)**.

      ![](images/retrievable-searchable.png)

1. Expand the **AnalyzeResult** **(1)** > **documents** **(2)** > **fields** **(3)** , expand **Organization** and **Address** and make the two fields Facetable **(type, valueString & content)** **(6)** and click on **Next: Create an indexer**.

      ![](images/doc92.png)
      
1. On the **Create an indexer** page, enter the name as **azureblob-indexer** **(1)** and click on **Submit** **(2)**.
   
      ![Create an indexer](images/create-an-indexer.png)

1. Select **Indexes** under the **Search management** tab and click on **azureblob-index**.

      ![Create an indexer](images/doc95upd.png)

1. In the **azureblob-index**, click on **Search** button.

      ![Create an indexer](images/doc99.png)

1. Verify the document that has been analysed

      ![Create an indexer](images/doc96.png)

1. Search for `fields` and verify the fields given while training the document has been analyzed.

   ![Create an indexer](images/doc97.png)

   ![Create an indexer](images/doc98.png)

## Summary

In this lab, you used Azure services to automate document processing by creating a Document Intelligence resource and training a custom model for data extraction. You then developed an Azure Function App to process documents from Blob Storage, analyze them via the Document Intelligence API, and store results as JSON files. Lastly, you set up Azure AI Search to index and search the analyzed documents, integrating these components for efficient document management.

### Click on Next >> to proceed with the next Lab.
