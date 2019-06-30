# AZ 100 Module 3 - Deploy and Manage Virtual Machines
# Lab 2: Configure compute and storage resources of Azure VMs and Azure VM scale sets

Estimated Time: 90 minutes

All tasks in this lab are performed from the Azure portal (including a PowerShell Cloud Shell session) 

   > **Note**: When not using Cloud Shell, the lab virtual machine must have the Azure PowerShell 1.2.0 module (or newer) installed [https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-1.2.0](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-1.2.0)

Lab files: 

-  **Allfiles/Labfiles/AZ-100.3/az-100-03b_01_azuredeploy.json**

-  **Allfiles/Labfiles/AZ-100.3/az-100-03b_01_azuredeploy.parameters.json**

-  **Allfiles/Labfiles/AZ-100.3/az-100-03b_02_azuredeploy.json**

-  **Allfiles/Labfiles/AZ-100.3/az-100-03b_02_azuredeploy.parameters.json**

### Scenario
  
Adatum Corporation wants to scale compute and storage resources for workloads running on Azure VMs and Azure VM scale sets


### Objectives
  
After completing this lab, you will be able to:

-  Deploy Azure VMs and Azure VM scale sets by using Azure Resource Manager templates

-  Configure compute and storage resources of Azure VMs 

-  Configure compute and storage resources of Azure VM scale sets


### Exercise 0: Prepare the lab environment
  
Estimated Time: 20 minutes

The main tasks for this exercise are as follows:

1. Deploy an Azure VM by using an Azure Resource Manager template

1. Deploy an Azure VM scale set by using an Azure Resource Manager template


#### Task 1: Deploy an Azure VM by using an Azure Resource Manager template

1. From the lab virtual machine, start Microsoft Edge, browse to the Azure portal at [**http://portal.azure.com**](http://portal.azure.com) and sign in by using a Microsoft account that has the Owner role in the Azure subscription you intend to use in this lab.

1. In the Azure portal, navigate to the **New** blade.

1. From the **New** blade, search Azure Marketplace for **Template deployment**.

1. Use the list of search results to navigate to the **Custom deployment** blade.

1. On the **Custom deployment** blade, select the **Build your own template in the editor**.

1. From the **Edit template** blade, load the template file **az-100-03b_01_azuredeploy.json**. 

   > **Note**: Review the content of the template and note that it defines deployment of an Azure VM hosting Windows Server 2016 Datacenter.

1. Save the template and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, navigate to the **Edit parameters** blade.

1. From the **Edit parameters** blade, load the parameters file **az-100-03b_01_azuredeploy.parameters.json**. 

1. Save the parameters and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, initiate a template deployment with the following settings:

    - Subscription: the name of the subscription you are using in this lab

    - Resource group: the name of a new resource group **az1000301b-RG**

    - Location: the name of the Azure region which is closest to the lab location and where you can provision Azure VMs

    - Vm Name: **az1000301b-vm1**

    - Vm Size: **Standard_DS1_v2**

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

   > **Note**: To identify Azure regions where you can provision Azure VMs, refer to [https://azure.microsoft.com/en-us/regions/offers/](https://azure.microsoft.com/en-us/regions/offers/)

   > **Note**: Do not wait for the deployment to complete but proceed to the next task of this exercise. You will use the virtual machine included in this deployment in the next exercise of this lab.


#### Task 2: Deploy an Azure VM scale set by using an Azure Resource Manager template

1. On the lab virtual machine, in the Azure portal, navigate to the **New** blade.

1. From the **New** blade, search Azure Marketplace for **Template deployment**.

1. Use the list of search results to navigate to the **Custom deployment** blade.

1. On the **Custom deployment** blade, select the **Build your own template in the editor**.

1. From the **Edit template** blade, load the template file **az-100-03b_02_azuredeploy.json**. 

   > **Note**: Review the content of the template and note that it defines deployment of an Azure VM scale set hosting Windows Server 2016 Datacenter.

1. Save the template and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, navigate to the **Edit parameters** blade.

1. From the **Edit parameters** blade, load the parameters file **az-100-03b_02_azuredeploy.parameters.json**. 

1. Save the parameters and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, initiate a template deployment with the following settings:

    - Subscription: the name of the subscription you are using in this lab

    - Resource group: the name of a new resource group **az1000302b-RG**

    - Location: the name of the Azure region which is closest to the lab location and where you can provision Azure VMs

    - Vmss Name: **az1000302bvmss1**

    - Vm Size: **Standard_DS1_v2**

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

    - Instance Count: **1**

   > **Note**: Do not wait for the deployment to complete but proceed to the next exercise. You will use the virtual machine scale set included in this deployment in the last exercise of this lab.

> **Result**: After you completed this exercise, you have initiated a template deployment of an Azure VM **az1000301b-vm1** and an Azure VM scale set **az1000302bvmss1** that you will use in the next exercise of this lab.


## Exercise 1: Configure compute and storage resources of Azure VMs 
  
Estimated Time: 40 minutes

The main tasks for this exercise are as follows:

1. Scale vertically compute resources of the Azure VM by using the Azure portal

1. Scale vertically compute resources of the Azure VM by using an ARM template

1. Attach data disks to the Azure VM

1. Configure data volumes within an Azure VM


#### Task 1: Scale vertically compute resources of the Azure VM by using the Azure portal

1. From the lab virtual machine, in the Azure portal, navigate to the **az1000301b-RG** resource group blade.

1. From the **az1000301b-RG** resource group blade, navigate to the **az1000301b-vm1** virtual machine blade.

1. From the **az1000301b-vm1** virtual machine blade, navigate to the **az1000301b-vm1 - Size** virtual machine blade.

1. From the **az1000301b-vm1 - Size** virtual machine blade, increase the size of the virtual machine to **DS2_v2** **Standard**. 

   > **Note**: If this size is not available, choose another size. To identify Azure VM sizes that you can choose from, refer to [https://azure.microsoft.com/en-us/global-infrastructure/services/](https://azure.microsoft.com/en-us/global-infrastructure/services/)

   > **Note**: Keep in mind that resizing an Azure VM requires restarting its operating system. 


#### Task 2: Scale vertically compute resources of the Azure VM by using an ARM template

1. From the lab virtual machine, in the Azure portal, navigate to the **az1000301b-RG** resource group blade.

1. From the **az1000301b-RG** resource group blade, navigate to the **az1000301b-RG - Deployments** blade.

1. From the **az1000301b-RG - Deployments** blade, navigate to the **Microsoft.Template - Overview** blade showing the most recent successful deployment.

1. On the **az1000301b-RG - Deployments** blade, use the **Redeploy** option to navigate to the **Custom deployment** blade.

1. From the **Custom deployment** blade, navigate to the **Edit parameters** blade.

1. On the **Edit parameters** blade, review the values of the original parameters used for the most recent successful deployment. 

   > **Note**: The value of the **adminPassword** parameter is null because that parameter has the **securestring** data type. 

1. Save the parameters and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, initiate a template deployment with the following settings:

    - Subscription: the name of the subscription you are using in this lab

    - Resource group: **az1000301b-RG**

    - Location: the name of the same Azure region you chose previously

    - Vm Name: **az1000301b-vm1**

    - Vm Size: **Standard_DS1_v2**

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

   > **Note**: Wait for the deployment to complete before you proceed to the next task of this exercise. The deployment will change the size of the Azure VM back to its original value, effectively scaling it down.


#### Task 3: Attach data disks to the Azure VM

1. In the Azure portal, navigate to the **az1000301b-vm1** virtual machine blade.

1. From the **az1000301b-vm1** virtual machine blade, navigate to the **az1000301b-vm1 - Disks** blade.

1. From the **az1000301b-vm1 - Disks** blade, use the **+ Add data disk** option to navigate to the **Create managed disk** blade.

1. From the **Create managed disk** blade, create a new data disk with the following settings:

    - Name: **az1000301b-vm1-DataDisk0**

    - Resource group: **az1000301b-RG**

    - Account type: **Standard HDD**

    - Source type: **None (empty disk)**

    - Size (GiB): **1023**

1. Back on the **az1000301b-vm1 - Disks** blade, configure the following settings for the newly created disk:

    - LUN: **0**

    - HOST CACHING: **None**

1. From the **az1000301b-vm1 - Disks** blade, use the **+ Add data disk** option to navigate to the **Create managed disk** blade.

1. From the **Create managed disk** blade, create a new data disk with the following settings:

    - Name: **az1000301b-vm1-DataDisk1**

    - Resource group: **az1000301b-RG**

    - Account type: **Standard HDD**

    - Source type: **None (empty disk)**

    - Size (GiB): **1023**

1. Back on the **az1000301b-vm1 - Disks** blade, configure the following settings for the newly created disk:

    - LUN: **1**

    - HOST CACHING: **None**


#### Task 4: Configure data volumes within an Azure VM

1. In the Azure portal, navigate to the **az1000301b-vm1** blade.

1. From the **az1000301b-vm1** blade, connect to the Azure VM via the RDP protocol and, when prompted to sign in, provide the following credentials:

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

1. Within the RDP session, in Server Manager, navigate to **File and Storage Services**, use New Storage Pool Wizard to create a new storage pool named **StoragePool1** consisting of two disks you attached in the previous task, use New Virtual Disk Wizard to create a new virtual disk named **VirtualDisk1** with **Simple** layout, **Fixed** provisioning type, and maximum size, and use the New Volume Wizard to create a single volume occupying entire virtual disk with the following settings:

    - Drive letter: **F**

    - File system: **NTFS**

    - Allocation unit size: **Default**

    - Volume label: **Data**

> **Result**: After you completed this exercise, you have scaled vertically compute resources of the Azure VM by using the Azure portal and by using an ARM template, attached data disks to the Azure VM, and configured data volumes within an Azure VM.


## Exercise 2: Configure compute and storage resources of Azure VM scale sets
  
Estimated Time: 30 minutes

The main tasks for this exercise are as follows:

1. Scale vertically compute resources of the Azure VM scale set by using an Azure Resource Manager template

1. Attach data disks to the Azure VM scale set

1. Configure data volumes in the Azure VM scale set

1. Scale horizontally compute resources of the Azure VM scale set


#### Task 1: Scale vertically compute resources of the Azure VM scale set by using an Azure Resource Manager template

1. From the lab virtual machine, in the Azure portal, navigate to the **az1000302b-RG** resource group blade.

1. From the **az1000302b-RG** resource group blade, navigate to the **az1000302b-RG - Deployments** blade.

1. From the **az1000302b-RG - Deployments** blade, navigate to the **Microsoft.Template - Overview** blade showing the most recent successful deployment.

1. On the **az1000302b-RG - Deployments** blade, use the **Redeploy** option to navigate to the **Custom deployment** blade.

1. From the **Custom deployment** blade, initiate a template deployment with the following settings:

    - Subscription: the name of the subscription you are using in this lab

    - Resource group: **az1000302b-RG**

    - Location: the name of the same Azure region you chose previously

    - Vm Name: **az1000302bvmss1**

    - Vm Size: the name of the VM size you used to scale up the Azure VM in the previous exercise of this lab

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

   > **Note**: Wait for the deployment to complete before you proceed to the next task of this exercise. The deployment will increase the size of Azure VM instances of the Azure VM scale set. 

1. In the Azure portal, navigate to the **az1000302bvmss1** blade and verify that the size of the VM instances of the VM scale set has changed.


#### Task 2: Attach data disks to the Azure VM scale set

1. From the Azure Portal, start a PowerShell session in the Cloud Shell. 

   > **Note**: If this is the first time you are launching the Cloud Shell in the current Azure subscription, you will be asked to create an Azure file share to persist Cloud Shell files. If so, accept the defaults, which will result in creation of a storage account in an automatically generated resource group.

1. In the Cloud Shell pane, run the following in order to attach one 128 GB disk to each VM instance in the VM scale set:

   ```pwsh
   $vmss = Get-AzVmss -ResourceGroupName 'az1000302b-RG' -VMScaleSetName 'az1000302bvmss1'

   Add-AzVmssDataDisk -VirtualMachineScaleSet $vmss -CreateOption Empty -Lun 1 -DiskSizeGB 128 -StorageAccountType 'Standard_LRS'

   Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -VirtualMachineScaleSet $vmss -VMScaleSetName $vmss.Name 
   ```

1. In the Azure portal, navigate to the **az1000302bvmss1 - Storage** blade and verify that the data disk has been added. 

#### Task 3: Configure data volumes in the Azure VM scale set
 
1. In the Cloud Shell pane, run the following in order to configure a simple volume on the newly added disk by using Custom Script Extension:

   ```pwsh
   $vmss = Get-AzVmss -ResourceGroupName 'az1000302b-RG' -VMScaleSetName 'az1000302bvmss1'

   $publicSettings = @{"fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.ps1");"commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File prepare_vm_disks.ps1"}

   Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "customScript" -Publisher "Microsoft.Compute" -Type "CustomScriptExtension" -TypeHandlerVersion 1.8 -Setting $publicSettings   
   
   Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -VirtualMachineScaleSet $vmss -VMScaleSetName $vmss.Name 
   ```

   > **Note**: To confirm that the volume has been configured, you will connect to the VM instance in the VM scale set via RDP.

1. To identify the public IP address of the Azure load balancer in front of the VM scale set, in the Azure portal, navigate to the **az1000302bvmss1** blade and note the value of the **Public IP address** entry.

1. To identify the port on which you should connect, navigate to the **az1000302b-RG** resource group blade.

1. From the **az1000302b-RG** resource group blade, navigate to the **az1000302bvmss1-lb** load balancer blade.

1. On the **az1000302bvmss1-lb** load balancer blade, note that the value of the **Public IP address** setting matches the value of the IP address used by the VM scale set you identified earlier in this task.

1. From the **az1000302bvmss1-lb** load balancer blade, navigate to the **az1000302bvmss1-lb - Inbound NAT rules** blade.

1. From the **az1000302bvmss1-lb - Inbound NAT rules** blade, navigate to the **natpool.0** blade and note that the port mappings start with **50000**. 

1. From the lab computer, initiate the Remote Desktop Connection to the first VM instance in the VM scale set by running the following from **Start -> Run** text box (where `<public_IP_address>` represents the public IP address you identified earlier in this task):

   ```
   mstsc /f /v:<public_IP_address>:50000
   ```

   > **Note**: Make sure to replace the placeholder `<public_IP_address>` with the value of the public IP address you identified earlier in this task.

1. When prompted to sign in via RDP to a VM instance of the VM scale set, provide the following credentials:

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

   > **Note**: Ignore any messages regarding signing in with temporary profile. 

1. Within the RDP session, launch File Explorer and verify that the VM has an extra volume of 127 GB in size with F: drive letter assigned to it. 

1. Sign out from the RDP session.


#### Task 4: Scale horizontally compute resources of the Azure VM scale set

1. From the lab virtual machine, in the Azure portal, navigate to the **az1000302bvmss1** VM scale set blade.

1. From the **az1000302bvmss1** VM scale set blade, navigate to the **az1000302bvmss1 - Scaling** blade.

1. On the **az1000302bvmss1 - Scaling** blade, use the **Override condition** setting to increase the instance count to 2.

1. Navigate to the **az1000302bvmss1 - Instances** blade and verify that the number of instances has increased to 2.

   > **Note**: You might need to refresh the display on the **az1000302bvmss1 - Instances** blade to view the process of provisioning the additional instance.


> **Result**: After you completed this exercise, you have scaled vertically compute resources of the Azure VM scale set by using an Azure Resource Manager template, attached data disks to the Azure VM scale set, configured data volumes in the Azure VM scale set, and scaled horizontally compute resources of the Azure VM scale set.
