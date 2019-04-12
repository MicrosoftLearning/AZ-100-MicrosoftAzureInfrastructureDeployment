**Upcoming changes!!!**

The AZ-100 and AZ-101 certifications are being replaced by a new AZ-103 Microsoft Azure Administrator exam! You can read more about this announcement on Liberty Munson’s blog at https://www.microsoft.com/en-us/learning/community-blog-post.aspx?BlogId=8&Id=375217 
 
To support the new exam there will be a new AZ-103 GitHub repository, available starting on May 3 2019. At that time, all the AZ-100 and AZ-101 labs in their respective repositories will be moved to the AZ-103 repository. Those labs are being reused in AZ-103 and we would like to maintain only one repository. The AZ-100 and AZ-101 lab numbering system will be retained, so if you are still teaching the AZ-100 or AZ-101  courses you will be able to easily identify the labs. You will also be able to get the latest version of the labs, and submit any issues you find.

**What are we doing?**

*	We are publishing the lab instructions and lab files on GitHub to allow for interaction between the course authors and MCTs. We hope this will help  keep the content current as the Azure platform changes.

*	This is a GitHub repository for the AZ-100, Microsoft Azure Infrastructure Deployment course.

*	You can access the repositories from https://github.com/orgs/MicrosoftLearning/dashboard

*	Within each repository there are lab guides in the Markdown format in the Instructions folder. The lab guides in the PDF format are available from the MCT Download Center, however they are not being regularly updated. If appropriate, there are also additional files that are needed to complete the lab within the Allfiles\Labfiles folder. Not every course has corresponding lab files. 

*	For each delivery, trainers should download the latest files from GitHub. Trainers should also check the Issues tab to see if other MCTs have reported any errors.  

*	Lab timing estimates are provided but trainers should check to ensure this is accurate based on the audience.

*	The lab content has been placed at the end of each course for consistency and convenience. However, as the instructor, you are the best judge to determine when the lab should be offered.

*	To conduct you will need an internet connection and an Azure subscription. Please read the Instructor Prep Guide for more information on using the Cloud Shell. 

*	It is recommended that you provide these materials directly to your students rather than point them to the GitHub repository. 

**How are we doing?**

*	If as you are teaching these courses, you identify areas for improvement, please use the Issues tab to provide feedback. We will periodically create new files to incorporate the changes. 

**General comments regarding the AZ-100 and AZ-101 courses**

* PowerShell scripts in all labs use the current version of Azure PowerShell Az module

* Although not required, it is a good idea to deprovision any existing resources when you have completed each lab. This will help mitigate the risk of exceeding the default vCPU quota limits and minimize usage charges.

* Availability of Azure regions and resources in these regions depends to some extent on the type of subscription you are using. To identify Azure regions available in your subscription, refer to https://azure.microsoft.com/en-us/regions/offers/ . To identify resources available in these regions, refer to https://azure.microsoft.com/en-us/global-infrastructure/services/ . These restrictions might result in failures during template validation or template deployment, in particular when provisioning Azure VMs. If this happens, review error messages and retry deployment with a different VM size or a different region.

* When launching Azure Cloud Shell for the first time, you will likely be prompted to create an Azure file share to persist Cloud Shell files. If so, you can typically accept the defaults, which will result in creation of a storage account in an automatically generated resource group. Note that this might happen again if you delete that storage account.

* Before you perform a template based deployments, you might need to register providers that handle provisioning of resource types referenced in the template. This is a one-time operation (per subscription) required when using Azure Resource Manager templates to deploy resources managed by these resource providers (if these resource providers have not been yet registered). You can perform registration from the subscription's Resource Providers blade in the Azure portal or by using Cloud Shell to run Register-AzResourceProvider PowerShell cmdlet or az provider Azure CLI command.

We hope using this GitHub repository brings a sense of collaboration to the labs and improves the overall quality of the lab experience. 

Regards,
Azure Administrator Courseware Team

