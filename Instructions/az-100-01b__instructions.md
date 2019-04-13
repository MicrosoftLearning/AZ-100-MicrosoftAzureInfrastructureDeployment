# AZ 100 Module 1 - Manage Azure Subscriptions and Resources
# Lab 2: Manage Azure Subscriptions and Resources - Implementing governance and compliance with Azure initiatives and resource locks

Estimated Time: 90 minutes

All tasks in this lab are performed from the Azure portal (including a PowerShell Cloud Shell session)  

   > **Note**: When not using Cloud Shell, the lab virtual machine must have the Azure PowerShell 1.2.0 module (or newer) installed [https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-1.2.0](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-1.2.0)

Lab files: 

-  **Allfiles/Labfiles/AZ-100.1/az-100-01b_azuredeploy.json**

-  **Allfiles/Labfiles/AZ-100.1/az-100-01b_azuredeploy.parameters.json**

### Scenario
  
Adatum Corporation wants to use Azure policies and initiatives in order to enforce resource tagging in its Azure subscription. Once the environment is compliant, Adatum wants to prevent unintended changes by implementing resource locks.

### Objectives
  
After completing this lab, you will be able to:

-  Implement Azure tags by using Azure policies and initiatives

-  Implement Azure resource locks


## Exercise 1: Implement Azure tags by using Azure policies and initiatives

Estimated Time: 45 minutes
  
The main tasks for this exercise are as follows:

1. Provision Azure resources by using an Azure Resource Manager template.

1. Implement an initiative and policy that evaluate resource tagging compliance.

1. Implement a policy that enforces resource tagging compliance.

1. Evaluate tagging enforcement and tagging compliance.

1. Implement remediation of resource tagging non-compliance.

1. Evaluate effects of the remediation task on compliance.


#### Task 1: Provision Azure resources by using an Azure Resource Manager template.

1. From the lab virtual machine, start Microsoft Edge, browse to the Azure portal at [**http://portal.azure.com**](http://portal.azure.com) and sign in by using a Microsoft account that has the Owner role in the Azure subscription you intend to use in this lab.

1. In the Azure portal, navigate to the **New** blade.

1. From the **New** blade, search Azure Marketplace for **Template deployment**.

1. Use the list of search results to navigate to the **Custom deployment** blade.

1. On the **Custom deployment** blade, select the **Build your own template in the editor**.

1. From the **Edit template** blade, load the template file **az-100-01b_azuredeploy.json**. 

   > **Note**: Review the content of the template and note that it defines deployment of an Azure VM hosting Windows Server 2016 Datacenter, including tags on some of its resources.

1. Save the template and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, navigate to the **Edit parameters** blade.

1. From the **Edit parameters** blade, load the parameters file **az-100-01b_azuredeploy.parameters.json**. 

1. Save the parameters and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, initiate a template deployment with the following settings:

    - Subscription: the name of the subscription you are using in this lab

    - Resource group: the name of a new resource group **az1000101b-RG**

    - Location: the name of the Azure region which is closest to the lab location and where you can provision Azure VMs

    - Vm Size: **Standard_DS1_v2**

    - Vm Name: **az1000101b-vm1**

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

    - Virtual Network Name: **az1000101b-vnet1**

    - Environment Name: **lab**

   > **Note**: To identify Azure regions where you can provision Azure VMs, refer to [**https://azure.microsoft.com/en-us/regions/offers/**](https://azure.microsoft.com/en-us/regions/offers/)

   > **Note**: Do not wait for the deployment to complete before you proceed to the next step.
   
1. In the Azure portal, navigate to the **Tags** blade.

1. From the **Tags** blade, display all resources with the **environment** tag set to the value **lab**. Note that only some of the resources deployed in the previous task have this tag assigned.

   > **Note**: At this point, only some of the resources have been provisioned, however, you should see at least a few without tags assigned to them.


#### Task 2: Implement a policy and an initiative that evaluate resource tagging compliance.

1. In the Azure portal, navigate to the **Policy** blade.

1. From the **Policy** blade, navigate to the **Policy - Definitions** blade.

1. From the **Policy Definitions** blade, display the **Enforce tag and its value** policy definition.

1. From the **Enforce tag and its default value** policy definition blade, use the duplicate the definition feature to create a new policy with the following settings:

    - Definition location: the name of the subscription you are using in this lab

    - Name: **az10001b - Audit tag and its value**

    - Description: **Audits a required tag and its value. Does not apply to resource groups.**

    - Category: the name of a new category **Lab**

    - Policy rule: the existing policy rule with the **effect** set to **audit**, such that the policy definition has the following content:

   ```
   {
     "mode": "indexed",
     "policyRule": {
       "if": {
         "not": {
           "field": "[concat('tags[', parameters('tagName'), ']')]",
           "equals": "[parameters('tagValue')]"
         }
       },
       "then": {
         "effect": "audit"
       }
     },
     "parameters": {
       "tagName": {
         "type": "String",
         "metadata": {
           "displayName": "Tag Name",
           "description": "Name of the tag, such as 'environment'"
         }
       },
       "tagValue": {
         "type": "String",
         "metadata": {
           "displayName": "Tag Value",
           "description": "Value of the tag, such as 'production'"
         }
       }
     }
   }
   ```

1. From the **Policy - Definitions** blade, navigate to the **New Initiative definition** blade.

1. From the **New Initiative definition** blade, create a new initiative definition with the following settings:

    - Definition location: the name of the subscription you are using in this lab

    - Name: **az10001b - Tagging initiative**

    - Description: **Collection of tag policies.**

    - Category: **Lab**

    - POLICIES AND PARAMETERS: **az10001b - Audit tag and its value**

        - Tag Name: **environment**

        - Tag Value: **lab**

1. Navigate to the **Policy - Assignments** blade.

1. From the **Policy - Assignments** blade, navigate to the **Assign initiative** blade and create a new initative assignment with the following settings:

    - Scope: the name of the subscription you are using in this lab

    - Exclusions: none 

    - Initiative definition: **az10001b - Tagging initiative**

    - Assignment name: **az10001b - Tagging initiative assignment**

    - Description: **Assignment of az10001b - Tagging initiative**

    - Assigned by: the default value

    - Create a Managed Identity: **unchecked**

1. Navigate to the **Policy - Compliance** blade. Note that **COMPLIANCE STATE** is set to either **Not registered** or **Not started**.

   > **Note**: On average, it takes about 10 minutes for a compliance scan to start. Rather than waiting for the compliance scan, proceed to the next task. You will review the compliance status later in this exercise.


#### Task 3: Implement a policy that enforces resource tagging compliance.

1. Navigate to the **Policy - Definitions** blade.

1. From the **Policy - Definitions** blade, navigate to the **az10001b - Tagging initiative** blade.

1. From the **az10001b - Tagging initiative** blade, navigate to its **Edit initiative definition** blade.

1. Add the built-in policy definition named **Enforce tag and its value** to the initiative and set its parameters to the following values:

    - Tag Name: **environment**

    - Tag Value: **lab**

   > **Note**: At this point, your initiative contains two policies. The first of them evaluates the compliance status and the second one enforces tagging during deployment. 


#### Task 4: Evaluate tagging enforcement and tagging compliance.

1. In the Azure portal, navigate to the **New** blade.

1. From the **New** blade, search Azure Marketplace for **Template deployment**.

1. Use the list of search results to navigate to the **Custom deployment** blade.

1. On the **Custom deployment** blade, select the **Build your own template in the editor**.

1. From the **Edit template** blade, load the template file **az-100-01b_azuredeploy.json**. 

   > **Note**: This is the same template that you used for deployment in the first task of this exercise. 

1. Save the template and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, navigate to the **Edit parameters** blade.

1. From the **Edit parameters** blade, load the parameters file **az-100-01b_azuredeploy.parameters.json**. 

1. Save the parameters and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, initiate a template deployment with the following settings:

    - Subscription: the name of the subscription you are using in this lab

    - Resource group: the name of a new resource group **az1000102b-RG**

    - Location: the name of the Azure region which you chose in the first task of this exercise

    - Vm Size: **Standard_DS1_v2**

    - Vm Name: **az1000102b-vm1**

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

    - Virtual Network Name: **az1000102b-vnet1**

    - Environment Name: **lab**

   > **Note**: The deployment will fail. This is expected.

1. You will be presented with the message indicating validation erors. Review the error details, indicating that deployment of resource **az1000102b-vnet1** was disallowed by the policy **Enforce tag and its value** which is included in the **az10001b - Tagging initiative assignment**.

1. Navigate to the **Policy - Compliance** blade. Identify the entry in the **COMPLIANCE STATE** column.

1. Navigate to the **az10001b - Tagging initiative assignment** blade and reviwew the summary of the compliance status.

1. Display the listing of resource compliance and note which resources have been identified as non-compliant. 

   > **Note**: You might need to click **Refresh** button on the **Policy - Compliance** blade in order to see the update to the compliance status. 


#### Task 5: Implement remediation of resource tagging non-compliance.

1. In the Azure portal, navigate to the **az10001b - Tagging initiative** blade.

1. From the **az10001b - Tagging initiative** blade, navigate to its **Edit initiative definition** blade.

1. Add the built-in policy definition named **Apply tag and its default value** to the initiative and set its parameters to the following values:

    - Tag Name: **environment**

    - Tag Value: **lab**

1. Delete the custom policy definition named **az10001b - Audit tag and its value** from the initiative.

1. Delete the built-in policy definition named **Enforce tag and its value** from the initiative and save the changes.

   > **Note**: At this point, your initiative contains a single policy that automatically remediates tagging non-compliance during deployment of new resources and provides evaluation of compliance status.

1. From the Azure Portal, start a PowerShell session in the Cloud Shell. 

   > **Note**: If this is the first time you are launching the Cloud Shell in the current Azure subscription, you will be asked to create an Azure file share to persist Cloud Shell files. If so, accept the defaults, which will result in creation of a storage account in an automatically generated resource group.

1. In the Cloud Shell pane, run the following commands.

   ```
   Get-AzResource -ResourceGroupName 'az1000101b-RG' | ForEach-Object {Set-AzResource -ResourceId $_.ResourceId -Tag @{environment="lab"} -Force }
   ```

   > **Note**: These commands assign the **environment** tag with the value **lab** to each resource in the resource group **az1000101b-RG**, overwriting any already assigned tags.
   
   > **Note**: Wait until the commands successfully complete.   

1. In the Azure portal, navigate to the **Tags** blade.

1. From the **Tags** blade, display all resources with the **environment** tag set to the value **lab**. Verify that all resources in the resource group **az1000101b-RG** are listed.


#### Task 6: Evaluate effects of the remediation task on compliance.

1. In the Azure portal, navigate to the **New** blade.

1. From the **New** blade, search Azure Marketplace for **Template deployment**.

1. Use the list of search results to navigate to the **Custom deployment** blade.

1. On the **Custom deployment** blade, select the **Build your own template in the editor**.

1. From the **Edit template** blade, load the template file **az-100-01b_azuredeploy.json**. 

   > **Note**: This is the same template that you used for deployment in the first task of this exercise. 

1. Save the template and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, navigate to the **Edit parameters** blade.

1. From the **Edit parameters** blade, load the parameters file **az-100-01b_azuredeploy.parameters.json**. 

1. Save the parameters and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, initiate a template deployment with the following settings:

    - Subscription: the name of the subscription you are using in this lab

    - Resource group: **az1000102b-RG**

    - Location: the name of the Azure region which you chose in the first task of this exercise

    - Vm Size: **Standard_DS1_v2**

    - Vm Name: **az1000102b-vm1**

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

    - Virtual Network Name: **az1000102b-vnet1**

    - Environment Name: **lab**

   > **Note**: The deployment will succeed this time. This is expected.

   > **Note**: Do not wait for the deployment to complete before you proceed to the next step. 

1. In the Azure portal, navigate to the **Tags** blade.

1. From the **Tags** blade, display all resources with the **environment** tag set to the value **lab**. Note that all the resources deployed to the resource group **az1000102b-RG** have this tag with the same value automatically assigned.

   > **Note**: At this point, only some of the resources have been provisioned, however, you should see that all of them have tags assigned to them.

1. Navigate to the **Policy - Compliance** blade. Identify the entry in the **COMPLIANCE STATE** column.

1. Navigate to the **az10001b - Tagging initiative assignment** blade. Identify the entry in the **COMPLIANCE STATE** column. If the column contains the **Not started** entry, wait until it the compliance scan runs. 

   > **Note**: You might need to wait for up to 10 minutes and click **Refresh** button on the **Policy - Compliance** blade in order to see the update to the compliance status. 

   > **Note**: Do not wait until the status is listed as compliant but instead proceed to the next exercise. 

> **Result**: After you completed this exercise, you have implemented an initiative and policies that evaluate, enforce, and remediate resource tagging compliance. You also evaluated the effects of policy assignment. 


## Exercise 2: Implement Azure resource locks
  
Estimated Time: 15 minutes

The main tasks for this exercise are as follows:

1. Create resource group-level locks to prevent accidental changes

1. Validate functionality of the resource group-level locks


#### Task 1: Create resource group-level locks to prevent accidental changes

1. In the Azure portal, navigate to the **az1000101b-RG** resource group blade.

1. From the **az1000101b-RG** resource group blade, display the **az1000101b-RG - Locks** blade.

1. From the **az1000101b-RG - Locks** blade, add a lock with the following settings:

    - Lock name: **az1000101b-roLock**

    - Lock type: **Read-only**


#### Task 2: Validate functionality of the resource group-level locks

1. In the Azure portal, navigate to the **az1000102b-vm1** virtual machine blade.

1. From the **az1000102b-vm1** virtual machine blade, navigate to the **az1000102b-vm1 - Tags** blade.

1. Try setting the value of the **environment** tag to **dev**. Note that the operation is successful. 

1. In the Azure portal, navigate to the **az1000101b-vm1** virtual machine blade.

1. From the **az1000101b-vm1** virtual machine blade, navigate to the **az1000101b-vm1 - Tags** blade.

1. Try setting the value of the **environment** tag to **dev**. Note that this time the operation fails. The resulting error message indicates that the resource refused tag assignment, with resource lock being the likely reason.

1. Navigate to the blade of the storage account created in the **az1000101b-RG - Locks** resource group. 

1. From the storage account blade, navigate to its **Access keys** blade. Note the resulting error message stating that you cannot access the data plane because a read lock on the resource or its parent.

1. In the Azure portal, navigate to the **az1000101b-RG** resource group blade.

1. From the **az1000101b-RG** resource group blade, navigate to its **Tags** blade.

1. From the **Tags** blade, attempt assigning the **environment** tag with the value **lab** to the resource group and note the error message.


> **Result**: After you completed this exercise, you have created a resource group-level lock to prevent accidental changes and validated its functionality. 
