# Build Face Detection Website with Azure Serverless Service and Azure Cognitive Service

## Scenario
First, you will host a static website and create Table Storage in blob. Use Table Storage to save user information. Then you will build Azure Function to call Azure Cognitive Service (Face API) to analyze User's Image. You will also use Azure Logic Apps to design your service sequence.

## Prerequisites
>The workshop’s region will be in ‘East Asia’

> Download <code style="color: green">index.html face.html variable.js</code> in this repository


## Lab tutorial
### Create Storage account
1.1.  Search for **Storage accounts**, click **Add**.
![1-1_Create_Storage_account.png](./images/1-1_Create_Storage_account.png)
1.2. Select **Resource Group** to organize your resource.
![1-2_Create_Storage_account.png](./images/1-2_Create_Storage_account.png)

1.3. For Storage account name, type a **Unique Name**.

1.4. For Region, choose **(Asia Pacific) East Asia**.

1.5. Click **Review+create** button on lower left side, then click **Create**.

![1-3_Create_Storage_account.png](./images/1-3_Create_Storage_account.png)

### Create Table Storage in Storage Account

2.1. Back to Storage Account, and click  **Storage Account** you previously created.

![2-1_Create_Storage_account.png](./images/2-1_Create_Table_Storage_in_Storage_Account.png)

2.2. Click **Tables** in the **Overview pannel**.

![2-2_Create_Storage_account.png](./images/2-2_Create_Table_Storage_in_Storage_Account.png)

2.3. Click **Tables**, type **Table name** and click **OK**.

![2-3_Create_Storage_account.png](./images/2-3_Create_Table_Storage_in_Storage_Account.png)

### Create Face API from Cognitive Service

3.1. Search for **Face**, click **Create**.

![3-1_Create_Face_API_from_Cognitive_Service.png](./images/3-1_Create_Face_API_from_Cognitive_Service.png)

3.2. For Name, Type **Name** you prefer.

3.3. For Location, Choose **(US) East US**.

3.4. For Pricing tier, Choose **S0 (10 Calls per second)**.

3.5. Click **Create** to create resource.

![3-2_Create_Face_API_from_Cognitive_Service.png](./images/3-2_Create_Face_API_from_Cognitive_Service.png)

3.6. Go to Cognitive Service, and click  **Storage Account** you previously created.

![3-3_Create_Face_API_from_Cognitive_Service.png](./images/3-3_Create_Face_API_from_Cognitive_Service.png)

3.7. Go to **Quick Start** Tab, copy **Key1** & **Endpoint**

![3-4_Create_Face_API_from_Cognitive_Service.png](./images/3-4_Create_Face_API_from_Cognitive_Service.png)

3.8. Open <code style="color: red">variable.js</code> you download from GitLab with **text editor**.

3.9. For **faceapiURL**, paste **Endpoint** you copied at step **3.7**

3.10. For **SubscriptionKey**, paste **Key1** you copied at step **3.7** and **Save File**.

![3-5_Create_Face_API_from_Cognitive_Service.png](./images/3-5_Create_Face_API_from_Cognitive_Service.png)



### Create Logic App

4.1 Search for **Logic Apps**, click **Add**.

![4-1_Create_Logic_App.png](./images/4-1_Create_Logic_App.png)

4.2. For Name, Type **Name** you prefer.

4.3. Select **Resource Group** to organize your resource.

4.4. For Location, Choose **(US) East US**.

4.5. Click **Create** to create resource.

![4-2_Create_Logic_App.png](./images/4-2_Create_Logic_App.png)

4.6. Go to Logic App, and click resource you previously created.

![4-3_Create_Logic_App.png](./images/4-3_Create_Logic_App.png)

4.7. Choose **When a HTTP request is received**.

![4-4_Create_Logic_App.png](./images/4-4_Create_Logic_App.png)

4.8. Click **+ New Step**, search for **table storage**
and select.

![4-5_Create_Logic_App.png](./images/4-5_Create_Logic_App.png)

4.9. Select **Insert Entity**

![4-6_Create_Logic_App.png](./images/4-6_Create_Logic_App.png)

4.10. Click **Connection** in order to connect Logic App to **Table Storage** created at **step 1.5.**

![4-7_Create_Logic_App.png](./images/4-7_Create_Logic_App.png)

4.11. For **Table**, type **table name** created at **step 2.3.**.

4.12. For **Entity**, Copy below code and paste.

    {
    "PartitionKey": "user",
    "RowKey": @{triggerBody()?['user']},
    "age": @{triggerBody()?['age']},
    "faceId": @{triggerBody()?['faceId']},
    "gender": @{triggerBody()?['gender']},
    "glasses": @{triggerBody()?['glasses']},
    "smile": @{triggerBody()?['smile']}
    }

![4-8_Create_Logic_App.png](./images/4-8_Create_Logic_App.png)

4.13. Click **Save**.

![4-9_Create_Logic_App.png](./images/4-9_Create_Logic_App.png)

4.14. Open <code style="color: red">variable.js</code> you download from GitLab with **text editor**.

4.15. For **DetectFaceURL**, paste **HTTP POST URL** in **When a HTTP request is received** section.

![4-10_Create_Logic_App.png](./images/4-10_Create_Logic_App.png)

![4-11_Create_Logic_App.png](./images/4-11_Create_Logic_App.png)


### Host static website to blob

5.1. Back to Storage Account, and click  **Storage Account** you previously created.

![5-1_Host_static_website_to_blob.png](./images/5-1_Host_static_website_to_blob.png)

5.2. Click **Static website** in **Setting** tab.

![5-2_Host_static_website_to_blob.png](./images/5-2_Host_static_website_to_blob.png)

5.3. For **Static website**, select **Enabled**

5.4. For **Index document name**, type <code>index.html</code>.

5.5. Click **Save**.

![5-3_Host_static_website_to_blob.png](./images/5-3_Host_static_website_to_blob.png)

5.6. Click **$web** which locates in the middle of pannel.

![5-4_Host_static_website_to_blob.png](./images/5-4_Host_static_website_to_blob.png)

5.7. Click **Upload**.

5.8. Select and add the <code>index.html variable.js</code> which you previously download and modify.

5.9. Click Upload button on lower side without any setting.

![5-5_Host_static_website_to_blob.png](./images/5-5_Host_static_website_to_blob.png)

5.10. Back to the **Settings** tab, and click  **Static website**.

5.11. Type the **Primary Endpoint** and open with your **Mobile Browser**.

> :warning: **Mobile Browser or Web Insepector Only**: If you use computer browser, please open Web Insepector and switch to mobile mode.

![5-6_Host_static_website_to_blob.png](./images/5-6_Host_static_website_to_blob.png)

5.12. You will see the website as below:

![5-7_Host_static_website_to_blob.png](./images/5-7_Host_static_website_to_blob.png)

## Conclusion

Congratulations! You now have learned how to:
* Create a static website hosting through blob
* Create a Logic App
* Create Table Storage
* Use Face API