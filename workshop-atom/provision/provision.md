# Provision ODA 

## Introduction

This lab will take you thru the step needed to provision Oracle Digital Assistant & Visual Builder Cloud Service

Estimated Time: 15 minutes

### About Oracle Digital Assistant (Optional)
Oracle Digital Assistant delivers a complete AI platform to create conversational experiences for business applications through text, chat, and voice interfaces

### Objectives

Provisioning of ODA 

In this lab, you will:
* Provision ODA Instance



### Prerequisites 

This lab assumes you have:
* An Oracle Cloud account



*This is the "fold" - below items are collapsed by default*

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

1. Step 1: Create a dynamic group having the rules for fn invocation
    ```
    
    All {instance.id = 'ocid1.odainstance.oc1.us-chicago-1.XXXXXXXXXXXXXXXX'}
    All {resource.type='odainstance', resource.compartment.id='ocid1.compartment.oc1..XXXXXXXXXXXXX' }
    ALL {resource.type = 'fnfunc', resource.compartment.id = 'ocid1.compartment.oc1..XXXXXXXXXXXXX'}
    
    
    ```
2. Step 2: Attach the policy at the root compartment level


    ```
    
    Allow any-user to use ai-service-generative-ai-family in tenancy where request.principal.id='ocid1.odainstance.oc1.us-chicago-1.XXXXXXXXXXXXXXXXXXXXXXXXXX'
    Allow any-user to use generative-ai-family in tenancy where request.principal.id='ocid1.odainstance.oc1.us-chicago-1.XXXXXXXXXXXXXXXXXXXXXX'
    Allow any-user to use fn-invocation in tenancy where request.principal.id='ocid1.odainstance.oc1.us-chicago-1.XXXXXXXXXXXXXXXXXXXX'
    Allow dynamic-group XXXXXXXXX to use fn-invocation in tenancy
    
    ```


    **Note:** 
    Please make sure that the compartmentId should be the one under which the resource is  created.
    Dynamic group name in step 2 should be the one created in step 1
## Acknowledgements
* **Author** - **Nitin Jain**, Master Principal Cloud Architect, NACIE
* **Contributors** -  <Name, Group> -- optional
* **Last Updated By/Date** - <Name, Month Year>
