# AZ 100 Module 4 - Configure and Manage Virtual Networks
# Lab 2: Configure Azure DNS
  
Estimated Time: 90 minutes

All tasks in this lab are performed from the Azure portal (including a PowerShell Cloud Shell session) 

   > **Note**: When not using Cloud Shell, the lab virtual machine must have the Azure PowerShell 1.2.0 module (or newer) installed [https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-1.2.0](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-1.2.0)

Lab files: 

-  **Allfiles/Labfiles/AZ-100.4/az-100-04b_01_azuredeploy.json**

-  **Allfiles/Labfiles/AZ-100.4/az-100-04b_02_azuredeploy.json**

-  **Allfiles/Labfiles/AZ-100.4/az-100-04_azuredeploy.parameters.json**


### Scenario
  
Adatum Corporation wants to implement public and private DNS service in Azure without having to deploy its own DNS servers. 


### Objectives
  
After completing this lab, you will be able to:

- Configure Azure DNS for public domains

- Configure Azure DNS for private domains


## Exercise 1: Configure Azure DNS for public domains

Estimated Time: 25 minutes
  
The main tasks for this exercise are as follows:

1. Create a public DNS zone

1. Create a DNS record in the public DNS zone

1. Validate Azure DNS-based name resolution for the public domain


#### Task 1: Create a public DNS zone
  
1. From the lab virtual machine, start Microsoft Edge, browse to the Azure portal at [**http://portal.azure.com**](http://portal.azure.com) and sign in by using a Microsoft account that has the Owner role in the Azure subscription you intend to use in this lab.

1. In the Azure portal, navigate to the **New** blade.

1. From the **New** blade, search Azure Marketplace for **DNS zone**.

1. Use the list of search results to navigate to the **Create DNS zone** blade.

1. From the to **Create DNS zone** blade, create a new DNS zone with the following settings: 

    - Name: any unique, valid DNS domain name in the **.com** namespace

    - Subscription: the name of the Azure subscription you are using in this lab

    - Resource group: the name of a new resource group **az1000401b-RG**

    - Resource group location: the name of the Azure region which is closest to the lab location and where you can provision Azure DNS zones


#### Task 2: Create a DNS record in the public DNS zone
  
1. From the Azure Portal, start a PowerShell session in the Cloud Shell. 

   > **Note**: If this is the first time you are launching the Cloud Shell in the current Azure subscription, you will be asked to create an Azure file share to persist Cloud Shell files. If so, accept the defaults, which will result in creation of a storage account in an automatically generated resource group.

1. In the Cloud Shell pane, run the following in order to identify the public IP address of your lab computer:

   ```pwsh
   Invoke-RestMethod http://ipinfo.io/json | Select-Object -ExpandProperty IP
   ```

   > **Note**: Take a note of this IP address. You will use it later in this task.

1. In the Cloud Shell pane, run the following in order to create a public IP address resource:

   ```pwsh
   $rg = Get-AzResourceGroup -Name az1000401b-RG

   New-AzPublicIpAddress -ResourceGroupName $rg.ResourceGroupName -Sku Basic -AllocationMethod Static -Name az1000401b-pip -Location $rg.Location
   ```

1. In the Azure portal, navigate to the **az1000401b-RG** resource group blade.

1. From the **az1000401b-RG** resource group blade, navigate to the blade displaying newly created public DNS zone.

1. From the DNS zone blade, navigate to the **Add record set** blade and create a DNS record with the following settings:

    - Name: **mylabvmpip**

    - Type: **A**

    - Alias record set: **No**

    - TTL: **1**

    - TTL unit: **Hours**

    - IP ADDRESS: the public IP address of your lab computer you identified earlier in this task

1. From the **Add record set** blade, create another record with the following settings:

    - Name: **myazurepip**

    - Type: **A**

    - Alias record set: **Yes**

    - Alias type: **Azure resource**

    - Choose a subscription: the name of the Azure subscription you are using in this lab

    - Azure resource: **az1000401b-pip**

    - TTL: **1**

    - TTL unit: **Hours**


#### Task 3: Validate Azure DNS-based name resolution for the public domain

1. On the DNS zone blade, note the list of the name servers that host the zone you created. You will use the first of them named in the next step.

1. From the lab virtual machine, start Command Prompt and run the following to validate the name resolution of the two newly created DNS records (where `<custom_DNS_domain>` represents the custom DNS domain you created in the first task of this exercise and `<name_server>` represents the name of the DNS name server you identified in the previous step): 

   ```
   nslookup mylabvmpip.<custom_DNS_domain> <name_server>
   
   nslookup myazurepip.<custom_DNS_domain> <name_server>
   ```

1. Verify that the IP addresses returned match those you identified earlier in this task.

> **Result**: After you completed this exercise, you have created a public DNS zone, created a DNS record in the public DNS zone, and validated Azure DNS-based name resolution for the public domain.


## Exercise 2: Configure Azure DNS for private domains
  
Estimated Time: 65 minutes

The main tasks for this exercise are as follows:

1. Provision a multi-virtual network environment

1. Create a private DNS zone

1. Deploy Azure VMs into virtual networks

1. Validate Azure DNS-based name reservation and resolution for the private domain


#### Task 1: Provision a multi-virtual network environment
  
1. From the Azure Portal, start a PowerShell session in the Cloud Shell. 

1. In the Cloud Shell pane, run the following in order to create a resource group:

   ```pwsh
   $rg1 = Get-AzResourceGroup -Name 'az1000401b-RG'

   $rg2 = New-AzResourceGroup -Name 'az1000402b-RG' -Location $rg1.Location
   ```

1. In the Cloud Shell pane, run the following in order to create two Azure virtual networks:

   ```pwsh
   $subnet1 = New-AzVirtualNetworkSubnetConfig -Name subnet1 -AddressPrefix '10.104.0.0/24'

   $vnet1 = New-AzVirtualNetwork -ResourceGroupName $rg2.ResourceGroupName -Location $rg2.Location -Name az1000402b-vnet1 -AddressPrefix 10.104.0.0/16 -Subnet $subnet1

   $subnet2 = New-AzVirtualNetworkSubnetConfig -Name subnet1 -AddressPrefix '10.204.0.0/24'

   $vnet2 = New-AzVirtualNetwork -ResourceGroupName $rg2.ResourceGroupName -Location $rg2.Location -Name az1000402b-vnet2 -AddressPrefix 10.204.0.0/16 -Subnet $subnet2
   ```

#### Task 2: Create a private DNS zone

1. In the Cloud Shell pane, run the following in order to create a private DNS zone with the first virtual network supporting registration and the second virtual network supporting resolution:

   ```pwsh
   New-AzDnsZone -Name adatum.local -ResourceGroupName $rg2.ResourceGroupName -ZoneType Private -RegistrationVirtualNetworkId @($vnet1.Id) -ResolutionVirtualNetworkId @($vnet2.Id)
   ```

   > **Note**: Virtual networks that you assign to an Azure DNS zone cannot contain any resources.

1. In the Cloud Shell pane, run the following in order to verify that the private DNS zone was successfully created:

   ```pwsh
   Get-AzDnsZone -ResourceGroupName $rg2.ResourceGroupName
   ```


#### Task 3: Deploy Azure VMs into virtual networks

1. In the Cloud Shell pane, upload **az-100-04b_01_azuredeploy.json**, **az-100-04b_02_azuredeploy.json**, and **az-100-04_azuredeploy.parameters.json** files.

1. In the Cloud Shell pane, run the following in order to deploy an Azure VM into the first virtual network:

   ```pwsh
   New-AzResourceGroupDeployment -ResourceGroupName $rg2.ResourceGroupName -TemplateFile "$home/az-100-04b_01_azuredeploy.json" -TemplateParameterFile "$home/az-100-04_azuredeploy.parameters.json" -AsJob
   ```

1. In the Cloud Shell pane, run the following in order to deploy an Azure VM into the second virtual network:

   ```pwsh
   New-AzResourceGroupDeployment -ResourceGroupName $rg2.ResourceGroupName -TemplateFile "$home/az-100-04b_02_azuredeploy.json" -TemplateParameterFile "$home/az-100-04_azuredeploy.parameters.json" -AsJob
   ```

   > **Note**: Wait for both deployments to complete before you proceed to the next task. You can identify the state of the jobs by running the `Get-Job` cmdlet in the Cloud Shell pane.


#### Task 4: Validate Azure DNS-based name reservation and resolution for the private domain

1. In the Azure portal, navigate to the blade of the **az1000402b-vm2** Azure VM. 

1. From the **Overview** pane of the **az1000402b-vm2** blade, generate an RDP file and use it to connect to **az1000402b-vm2**.

1. When prompted, authenticate by specifying the following credentials:

    - User name: **Student**

    - Password: **Pa55w.rd1234**

1. Within the Remote Desktop session to **az1000402b-vm2**, start a Command Prompt window and run the following: 

   ```
   nslookup az1000402b-vm1.adatum.local
   ```

1. Verify that the name is successfully resolved.

1. Switch back to the lab virtual machine and, in the Cloud Shell pane of the Azure portal window, run the following in order to create an additional DNS record in the private DNS zone:

   ```pwsh
   New-AzDnsRecordSet -ResourceGroupName $rg2.ResourceGroupName -Name www -RecordType A -ZoneName adatum.local -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "10.104.0.4")
   ```

1. Switch again to the Remote Desktop session to **az1000402b-vm2** and run the following from the Command Prompt window: 

   ```
   nslookup www.adatum.local
   ```

1. Verify that the name is successfully resolved.

> **Result**: After completing this exercise, you have provisioned a multi-virtual network environment, created a private DNS zone, deployed Azure VMs into virtual networks, and validated Azure DNS-based name reservation and resolution for the private domain
