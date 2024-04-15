# Provision of Oracle Digital Assistant & Visual Builder Instance

## Introduction

This lab will take you thru the step needed to provision Oracle Digital Assistant & Visual Builder Cloud Service

Estimated Time: 120 minutes

### About Oracle Digital Assistant (Optional)
Oracle Digital Assistant delivers a complete AI platform to create conversational experiences for business applications through text, chat, and voice interfaces

### Objectives

Provisioning of ODA 

In this lab, you will:
* **Provision ODA Instance**
    * Follow Task 1 to Task 5 to set-up ODA Instance
* **Provision VBCS Instance**
    * Follow Task 6 



### Prerequisites 

This lab assumes you have:
* An Oracle Cloud account

## Task 1: Provision Oracle Digital Assistant

This task will help you to create Oracle Digital Assistant under your choosen compartment.

1. Step 1 : Locate Digital Assistant under AI Services

	![Navigate to Digital Assistant](images/oda_provision_1.png)

	> **Note:** You can find Digital Assistant under the AI Services.

2. Step 2 : Provide the information for **Compartment**, **Name** , **Description** (optional) & **Shape**. Click **Create**

    ![Create ODA](images/oda_provision_3.png)

3. In few minutes the status of recently created Digital Assistant will change from **Provisioning** to **Active**

    ![Active ODA Instance](images/oda_provision_4.png) 


## Task 2: Dynamic Group & Policy creation for Oracle Digital Assistant


This task will help you to create desired dynamic group & necessary policy for the Oracle Digital Assistant 


1. Step 1: Attach the policy at the root compartment level


    ```
    Allow any-user to use ai-service-generative-ai-family in tenancy where request.principal.id='ocid1.odainstance.oc1.us-chicago-1.XXXXXXXXXXXXXXXXXXXXXXXXXX'
    Allow any-user to use generative-ai-family in tenancy where request.principal.id='ocid1.odainstance.oc1.us-chicago-1.XXXXXXXXXXXXXXXXXXXXXX'
    Allow any-user to use fn-invocation in tenancy where request.principal.id='ocid1.odainstance.oc1.us-chicago-1.XXXXXXXXXXXXXXXXXXXX'
    ```
    
    **Note:** 
    * Please make sure that the compartmentId should be the one under which the resource is  created.
   

## Task 3: Create REST Service for the OCI Generative AI Service 

This task involves creating REST service which will be used by ODA to connect to OCI Generative AI Service. The REST Service will be created for the ODA created in **Task 1**.

1. Step 1: Locate the ODA created in **Task 1**

    ![ODA List](images/oda_list.png) 


2. Step 2: Select the earlier created ODA Instance and click on **Service Console**

    ![ODA Service Console](images/oda_provision_4.png) 


3. Step 3: Click on hamburger menu and locate & click **API Services**

    ![API Services](images/oda_api_service.png) 

4. Step 4: Click on **Add REST Service**. Provide the following details:
    * **Name** : <Suitable Name>
    * **Endpoint** : https://inference.generativeai.us-chicago-1.oci.oraclecloud.com/20231130/actions/generateText
    * **Description (Optional)** : <Description>
    * **Authentication Type** : OCI Resource Principal
    * **Method** : POST
    * **Request**
    * **Body**
    ```
    {
        "compartmentId": "ocid1.compartment.oc1..XXXXXXXXXXX",
        "servingMode": {
            "modelId": "ocid1.generativeaimodel.oc1.us-chicago-1.XXXXXXXX",
            "servingType": "ON_DEMAND"
        },
        "inferenceRequest": {
            "prompt": "What is OCAF?",
            "maxTokens": 600,
            "temperature": 1,
            "frequencyPenalty": 0,
            "presencePenalty": 0,
            "topP": 0.75,
            "topK": 0,
            "returnLikelihoods": "GENERATION",
            "isStream": true,
            "stopSequences": [],
            "runtimeType": "COHERE"
        }
    }
    ```
5. Step 5: Click **Test Request** to make sure the connection is successful

   ![API Services](images/oda_api_service_4.png) 
    
    **Note**
    Retrieve the modelId (OCID) from OCI Gen AI Services Playground and use a compartmentId where the ODA is hosted inside
    
## Task 4: Import Skill (Provided)

1. Step 1: Download the skill from the url provided below

2. Step 2: Import the skill (downloaded). Click on **Import Skill** & select the zip file to import

   ![Import Skill](images/import_skill.png) 

## Task 5: Create Channel to embed ODA in Visual Builder Application (provided) or in any custom Web App.

1. Step 1: Click on hamburger menu and select Development > Channels

    ![Create Channel](images/create_channel.png)

2. Step 2: Select the following option on the form:

    * **Channel Type** = Oracle Web 
    * **Allowed Domain** = *

    ![Create Channel](images/create_channel_1.png)

3. Step 3: After channel creation, enable the Channel by using the toggle button (screenshot) and route it to skill imported in Task 4 (Take note of channelId for **Task 6** in later step)

    ![Create Channel](images/route_skill.png)

    

## Task 6: Create VBCS Instance & embed ODA skill in VBCS Application (Please directly move to Step 5 incase you already have a VBCS instance provisioned)

1. Step 1: Click on main hamburger menu on OCI cloud console and navigate Developer Services > Visual Builder

    ![Create Channel](images/visual_builder.png)

2. Step 2: Create Visual Builder Instance by providing the details and click **Create Visual Builder Instance**:
    * **Name** = <suitable_name>
    * **Compartment** = <same_compartment_as_oda>
    * **Node** = <as_per_need>

    ![Create Channel](images/create_vbcs.png)

3. Step 3: Wait for the instance to come to **Active** (green color) status

4. Step 4: Download the VB application from the provided link

5. Step 5: Import the application in provisioned instance as per the screenshots

    * Click on Import from Visual Builder Instance

        ![Create Channel](images/import_vbapp.png)

    * Choose the option as below

        ![Create Channel](images/import_vbapp_1.png)

    * Provide the App Name with other details and select the provided application zip file

        ![Create Channel](images/import_vbapp_2.png)
    
6. Step 6: Once import is completed, open the index.html file in the VB Instance and update the details as follows:

    * **URI** = 'https://oda-XXXXXXXXXXXXXXXXXXXXXX.data.digitalassistant.oci.oraclecloud.com/'
    * **channelId** = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXX' 

    ![Create Channel](images/vbapp_setup.png)

    **Note**
    * URI is the hostname of ODA instance provisioned in **Task 1**
    * channelId is created during **Task 5**

## Acknowledgements
**Author** - * **Nitin Jain**, Master Principal Cloud Architect, NACIE
             * **Abhinav Jain**, Senior Cloud Engineer, NACIE

