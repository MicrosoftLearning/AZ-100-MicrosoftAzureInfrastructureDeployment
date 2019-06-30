# AZ 100 Module 5 - Implement and Manage Hybrid Identities
# Lab: Implement Directory Synchronization
  
Estimated Time: 60 minutes

All tasks in this lab are performed from the Azure portal (including a PowerShell Cloud Shell session) except for Excercise 3 Task 1, Exercise 3 Task 2, and Exercise 3 Task 3, which include steps performed from a Remote Desktop session to an Azure VM

   > **Note**: When not using Cloud Shell, the lab virtual machine must have the Azure PowerShell 1.2.0 module (or newer) installed [https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-1.2.0](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-1.2.0)

Lab files: none

### Scenario
  
Adatum Corporation wants to integrate its Active Directory with Azure Active Directory


### Objectives
  
 After completing this lab, you will be able to:

- Deploy an Azure VM hosting an Active Directory domain controller

- Create and configure an Azure Active Directory tenant

- Synchronize Active Directory forest with an Azure Active Directory tenant


## Exercise 1: Deploy an Azure VM hosting an Active Directory domain controller

Estimated Time: 25 minutes
  
The main tasks for this exercise are as follows:

1. Identify an available DNS name for an Azure VM deployment

1. Deploy an Azure VM hosting an Active Directory domain controller by using an Azure Resource Manager template


#### Task 1: Identify an available DNS name for an Azure VM deployment

1. From the lab virtual machine, start Microsoft Edge, browse to the Azure portal at [**http://portal.azure.com**](http://portal.azure.com) and sign in by using a Microsoft account that has the Owner role in the Azure subscription you intend to use in this lab and is a Global Administrator of the Azure AD tenant associated with that subscription.

1. From the Azure Portal, start a PowerShell session in the Cloud Shell pane. 

   > **Note**: If this is the first time you are launching the Cloud Shell in the current Azure subscription, you will be asked to create an Azure file share to persist Cloud Shell files. If so, accept the defaults, which will result in creation of a storage account in an automatically generated resource group.

1. In the Cloud Shell pane, run the following command, substituting the placeholder `<custom-label>` with any string which is likely to be unique and the placeholder `<location>` with the name of the Azure region into which you want to deploy the Azure VM that will host an Active Directory domain controller.

   > **Note**: To identify Azure regions where you can provision Azure VMs, refer to [**https://azure.microsoft.com/en-us/regions/offers/**](https://azure.microsoft.com/en-us/regions/offers/)

   ```pwsh
   Test-AzDnsAvailability -DomainNameLabel <custom-label> -Location '<location>'
   ```

1. Verify that the command returned **True**. If not, rerun the same command with a different value of the `<custom-label>` until the command returns **True**. 

1. Note the value of the `<custom-label>` that resulted in the successful outcome. You will need it in the next task


#### Task 2: Deploy an Azure VM hosting an Active Directory domain controller by using an Azure Resource Manager template

1. In the Azure portal, navigate to the **Create a resource** blade. 

1. From the **Create a resource** blade, search Azure Marketplace for **Template deployment**.

1. Use the list of search results to navigate to the **Deploy a custom template** blade.

1. On the **Custom deployment** blade, in the **Load a GitHub quickstart template** drop-down list and select the **active-directory-new-domain** entry.

1. On the **Create an Azure VM with a new AD Forest** blade, initiate a template deployment with the following settings:

    - Subscription: the name of the subscription you are using in this lab

    - Resource group: the name of a new resource group **az1000501-RG**

    - Location: the name of the Azure region which you used in the previous task

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

    - Domain Name: **adatum.com**

    - Dns Prefix: the `<custom-label>` you identifed in the previous task

    - _artifacts Location: accept the default value

    - _artifacts Location Sas Token: leave blank

    - Location: accept the default value

   > **Note**: Do not wait for the deployment to complete but proceed to the next exercise. You will use the virtual machine deployed in this task in the third exercise of this lab.

> **Result**: After you completed this exercise, you have initiated deployment of an Azure VM that will host an Active Directory domain controller by using an Azure Resource Manager template


## Exercise 2: Create and configure an Azure Active Directory tenant

Estimated Time: 10 minutes
  
The main tasks for this exercise are as follows:

1. Create an Azure Active Directory (AD) tenant

1. Add a custom DNS name to the new Azure AD tenant

1. Create an Azure AD user with the Global Administrator role


#### Task 1: Create an Azure Active Directory (AD) tenant

1. In the Azure portal, navigate to the **Create a resource** blade. 

1. From the **Create a resource** blade, search Azure Marketplace for **Azure Active Directory**.

1. Use the list of search results to navigate to the **Create directory** blade.

1. From the **Create directory** blade, create a new Azure AD tenant with the following settings: 

  - Organization name: **AdatumSync**

  - Initial domain name: a unique name consisting of a combination of letters and digits. 

  - Country or region: **United States**

   > **Note**: The green check mark in the **Initial domain name** text box will indicate whether the domain name you typed in is valid and unique. 


#### Task 2: Add a custom DNS name to the new Azure AD tenant
  
1. In the Azure portal, set the **Directory + subscription** filter to the newly created Azure AD tenant.

   > **Note**: The **Directory + subscription** filter appears to the left of the notification icon in the toolbar of the Azure portal 

   > **Note**: You might need to refresh the browser window if the **AdatumSync** entry does not appear in the **Directory + subscription** filter list.

1. In the Azure portal, navigate to the **AdatumSync - Overview** blade.

1. From the **AdatumSync - Overview** blade, display the **AdatumSync - Custom domain names** blade. 

1. On the **AdatumSync - Custom domain names** blade, identify the primary, default DNS domain name associated with the Azure AD tenant. Note its value - you will need it in the next task.

1. From the **AdatumSync - Custom domain names** blade, add the **adatum.com** custom domain.

1. On the **adatum.com** blade, review the information necessary to perform verification of the Azure AD domain name.

   > **Note**: You will not be able to complete the validation process because you do not own the **adatum.com** DNS domain name. This will not prevent you from synchronizing the **adatum.com** Active Directory domain with the Azure AD tenant. You will use for this purpose the default primary DNS name of the Azure AD tenant (the name ending with the **onmicrosoft.com** suffix), which you identified earlier in this task. However, keep in mind that, as a result, the DNS domain name of the Active Directory domain and the DNS name of the Azure AD tenant will differ. This means that Adatum users will need to use different names when signing in to the Active Directory domain and when signing in to Azure AD tenant.


#### Task 3: Create an Azure AD user with the Global Administrator role

1. In the Azure portal, navigate to the **Users - All users** blade of the **AdatumSync** Azure AD tenant.

1. From the **Users - All users** blade, create a new user with the following settings:

    - Name: **syncadmin**

    - User name: **syncadmin@***<DNS-domain-name>* where *<DNS-domain-name>* represents the default primary DNS domain name you identified in the previous task. Take a note of this user name. You will need it later in this lab.

    - Profile: **Not configured**

    - Properties: **Default**

    - Groups: **0 groups selected**

    - Directory role: **Global administrator**

    - Password: select the checkbox **Show Password** and note the string appearing in the **Password** text box. You will need it later in this task.

   > **Note**: An Azure AD user with the Global Administrator role is required in order to implement Azure AD Connect.

1. Open an InPrivate Microsoft Edge window.

1. In the new browser window, navigate to the Azure portal and sign in using the **syncadmin** user account. When prompted, change the password to a new value.

   > **Note**: You will need to provide the fully qualified name of the **syncadmin** user account, including the Azure AD tenant DNS domain name. 

1. Sign out as **syncadmin** and close the InPrivate browser window.

> **Result**: After you completed this exercise, you have created an Azure AD tenant, added a custom DNS name to the new Azure AD tenant, and created an Azure AD user with the Global Administrator role.


## Exercise 3: Synchronize Active Directory forest with an Azure Active Directory tenant

Estimated Time: 35 minutes
  
The main tasks for this exercise are as follows:

1. Configure Active Directory in preparation for directory synchronization

1. Install Azure AD Connect

1. Verify directory synchronization


#### Task 1: Configure Active Directory in preparation for directory synchronization

   > **Note**: Before you start this task, ensure that the template deployment you started in Exercise 1 has completed. 
  
1. In the Azure portal, set the **Directory + subscription** filter back to the Azure AD tenant associated with the Azure subscription you used in the first exercise of this lab.

   > **Note**: The **Directory + subscription** filter appears to the left of the notification icon in the toolbar of the Azure portal 

1. In the Azure portal, navigate to the **adVM** blade, displaying the properties of the Azure VM hosting an Active Directory domain controller that you deployed in the first exercise of this lab.

1. From the **Overview** pane of the **adVM** blade, generate an RDP file and use it to connect to **adVM**.

1. When prompted, authenticate by specifying the following credentials:

    - User name: **Student**

    - Password: **Pa55w.rd1234**

1. Within the Remote Desktop session to **adVM**, open the **Active Directory Administrative Center**.

1. From **Active Directory Administrative Center**, create a root level organizational unit named **ToSync**.


1. From **Active Directory Administrative Center**, in the organizational unit **ToSync**, create a new user account with the following settings:

    - Full name: **aduser1**

    - User UPN logon: **aduser1@adatum.com**

    - User SamAccountName logon: **adatum\aduser1**

    - Password: **Pa55w.rd1234**

    - Other password options: **Password never expires**


#### Task 2: Install Azure AD Connect

1. Within the RDP session to **adVM**, from Server Manager, disable temporarily **IE Enhanced Security Configuration**.

1. Within the RDP session to **adVM**, start Internet Explorer and download **Azure AD Connect** from [**https://www.microsoft.com/en-us/download/details.aspx?id=47594**](https://www.microsoft.com/en-us/download/details.aspx?id=47594) 

1. Start **Microsoft Azure Active Directory Connect** wizard, accept the licensing terms, and, on the **Express Settings** page, select the **Customize** option.

1. On the **Install required components** page, leave all optional configuration options deselected and start the installation.

1. On the **User sign-in** page, ensure that only the **Password Hash Synchronization** is enabled.

1. When prompted to connect to Azure AD, authenticate by using the credentials of the **syncadmin** account you created in the previous exercise.

1. When prompted to connect your directories, add the **adatum.com** forest, choose the option to **Create new AD account**, and authenticate by using the following credentials:

    - User name: **ADATUM\Student**

    - Password: **Pa55w.rd1234**

1. On the **Azure AD sign-in configuration** page, note the warning stating **Users will not be able to sign-in to Azure AD with on-premises credentials if the UPN suffix does not match a verified domain name** and enable the checkbox **Continue without matching all UPN suffixes to verified domain**.

   > **Note**: As explained earlier, this is expected, since you could not verify the custom Azure AD DNS domain **adatum.com**.

1. On the **Domain and OU filtering** page, ensure that only the **ToSync** OU is selected.

1. On the **Uniquely identifying your users** page, accept the default settings.

1. On the **Filter users and devices** page, accept the default settings.

1. On the **Optional features** page, accept the default settings.

1. On the **Ready to configure** page, ensure that the **Start the synchronization process when configuration completes** checkbox is selected and continue with the installation process. 

   > **Note**: Installation should take about 2 minutes.

1. Close the Microsoft Azure Active Directory Connect window once the configuration is completed.


#### Task 3: Verify directory synchronization

1. Within the RDP session to **adVM**, start Internet Explorer, browse to the Azure portal at [**http://portal.azure.com**](http://portal.azure.com) and sign in by using the **syncadmin** account that you created in the previous exercise. 

1. In the Azure portal, navigate to the **AdatumSync - Overview** blade.

1. From the **AdatumSync - Overview** blade, display the **Users - All users** blade of the AdatumSync Azure AD tenant.

1. On the **Users - All users** blade, note that the list of user objects includes the **aduser1** account, with the **Windows Server AD** appearing in the **SOURCE** column.

1. From the **Users - All users** blade, display the **aduser1 - Profile** blade. Note that the **Department** attribute is not set.

1. Within the RDP session to **adVM**, switch to the **Active Directory Administrative Center**, open the window displaying properties of the **aduser1** user account, and set the value of its **Department** attribute to **Sales**.

1. Within the RDP session to **adVM**, start **Windows PowerShell** as Administrator.

1. From the Windows PowerShell prompt, start Azure AD Connect delta synchronization by running the following:

   ```pwsh
   Start-ADSyncSyncCycle -PolicyType Delta
   ```

1. Within the RDP session to **adVM**, switch to the Internet Explorer window displaying the Azure portal. 

1. In the Azure portal, navigate back to the **Users - All users** blade and refresh the page. 

1. From the **Users - All users** blade, display the **aduser1 - Profile** blade. Note that the **Department** attribute is now set to **Sales**.

   > **Note**: You might need to wait for another minute and refresh the page again if the **Department** attribute remains not set.

> **Result**: After you completed this exercise, you have configured Active Directory in preparation for directory synchronization, installed Azure AD Connect, and verified directory synchronization.
