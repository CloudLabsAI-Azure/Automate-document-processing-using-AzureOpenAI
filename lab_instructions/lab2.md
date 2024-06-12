# Utilize your Data Set using OpenAI

### Architecture Diagram

![Name](images/archi2.PNG)

### Task 1: Navigate to Azure OpenAI Playground

1. In `portal.azure.com`, search for openai and select **Azure OpenAI**.

   ![OpenAI](images/doc35.png)

2. Click on **OpenAI-<inject key="Deployment ID" enableCopy="false"/>**.

      ![OpenAI](images/doc36.png)

3. On the **Azure OpenAI** page, click on **Go to Azure OpenAI Studio**.

      ![OpenAI Studio](images/launch-openaist.png)

4. On the **Azure OpenAI Studio**, scroll down click on **Bring your own data**.

   ![Azure OpenAI Studio](images/bring-data.png)

### Task 2: Upload your own data

In this step, we will be using Porche's owner manual for Taycan, Panamera, and Cayenne models.

1. Fill the following details in **Select or add data source** and click on **Next** **(6)**.
    
    - Select data source: **Upload files (preview)** **(1)**

    - Subscription: Select your subscription from the drop-down section **(2)**

    - Select Azure Blob storage resource: Choose the already created storage account **storage<inject key="Deployment ID">** **(3)**. 
      
      - **Note**: If asked for turn on CORS, click on **Turn on CORS**.

         ![](images/data-source.png)

    - Select Azure Cognitive Search resource: Select the search service **search-<inject key="Deployment ID">** **(4)**.

    - Enter the index name: Give an index name as **aoaiworkshop** **(5)**.

      ![add-data](images/uploadfiles.png) 

2. On the **Data Management**, click on **Browse for a file** **(1)** enter the following `C:\LabFiles\Data\Lab 2` **(2)** path and hit enter, select the **Panamera-from-2021-Porsche-Connect-Good-to-know-Owner-s-Manual** **(3)** pdf  file and click on **Open** **(4)** files.

   ![data-management](images/labfiles.png)

3. It will redirect to **Data management**, click on **Upload files** **(1)**, and click on **Next** **(2)**.

   ![data-management](images/data-management-upload.png)

4. On the **Data Management** page, from the drop-down select **keyword (1)** as Search type and click on **Next (2)**.

   ![keyword](images/uploadfiles1.png)

5. On the **Data Connection page**, select **API Key**.

   ![keyword](images/api.png)

6. On the **Review and finish** page, click on **Save and close**.

   ![Save and close](images/save-and-close.png)

### Task 3: Interact with Azure OpenAI ChatGPT LLM using your own data

1. Under the **Assistant Setup** pane, wait until your data upload is finished.

   ![upload-data](images/upload-data.png)

2. Under the **Chat Session** pane, you can start testing out your prompts by entering the query like this.

    ```
    how to operate Android Auto in Porche Taycan? give step-by-step instructions
    ```

      ![chat-session-one](images/screen.png)

3. You can also configure the responses of your bot by selecting the system message under **Assistance Setup**, and click on **System message** **(1)** to replace the value under the system message with `Your name is Alice. You are an AI assistant that helps people find information about Porche cars. Your responses should not contain any harmful information` **(2)** and click on **Save changes** **(3)**. Here we have edited the default system message.

   ![assistant-setup-system-message](images/applychnages.png)

4. On **Update system message?** pop-up, click on **Continue**.

   ![Alt text](images/continue.png)

5. Under the **Chat Session** pane, you can start testing out your prompts by entering the query like this.

    ```
    What is your name
    ```
   
   ![chat-session-two](images/recogniserlab1-2.png)

6. In the **Configuration** pane, click on **Parameters**. You can try and experiment with different parameter configurations to see how they change the behavior of the model.

    ![Alt text](images/parameters.png)

7. Click on **Deploy to (1)** on the top right and click on **a new webapp (2)**.

8. Add the following details and click on **Deploy**:

   - Name: **webapp-<inject key="Deployment ID" enableCopy="false"/> (1)**
   - Subscription: **Select the default subscription (2)**
   - Resource Group: Select **OpenAI-<inject key="Deployment ID" enableCopy="false"/>** **(3)**
   - Location: **Select East US (4)**
   - Pricing Plan: **Choose Basic (B1) (5)**
   - **Enable** chat history in the web app **(6)**

9. Navigate to App Services and verify **webapp-<inject key="Deployment ID" enableCopy="false"/> (1)** has been created.

      > **Note:** In cases of permissions asked, click on **Accept**.

      ![Alt text](images/doc50.png)
      
10. Click on **Browse** and the web app is up and running.

    ![Alt text](images/doc51.png)

12. Chat with the bot and check its working state.
## Review

In this lab, you have accomplished the following:

* How to leverage the ChatGPT LLM to extract a concise summary from your own document repository using OpenAI.
