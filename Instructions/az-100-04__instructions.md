# AZ 100 Module 4 - Configure and Manage Virtual Networks
# Lab: Configure VNet peering and service chaining
  
Estimated Time: 45 minutes

All tasks in this lab are performed from the Azure portal except for Excercise 2 Task 3, Exercise 3 Task 1, and Exercise 3 Task 2, which include steps performed from a Remote Desktop session to an Azure VM

Lab files: 

-  **Labfiles\\AZ100\\Mod04\\az-100-04_01_azuredeploy.json**

-  **Labfiles\\AZ100\\Mod04\\az-100-04_02_azuredeploy.json**

-  **Labfiles\\AZ100\\Mod04\\az-100-04_azuredeploy.parameters.json**

### Scenario
  
ADatum Corporation wants to implement service chaining between Azure virtual networks in its Azure subscription. 


### Objectives
  
After completing this lab, you will be able to:

- Create Azure virtual networks and deploy Azure VM by using Azure Resource Manager templates.

- Configure VNet peering.

- Implement custom routing

- Validate service chaining


### Exercise 0: Prepare the Azure environment
  
Estimated Time: 10 minutes

The main tasks for this exercise are as follows:

1. Create the first virtual network hosting two Azure VMs by using an Azure Resource Manager template

1. Create the second virtual network in the same region hosting a single Azure VM by using an Azure Resource Manager template

#### Task 1: Create the first virtual network hosting two Azure VMs by using an Azure Resource Manager template

1. From the lab virtual machine, start Microsoft Edge, browse to the Azure portal at [**http://portal.azure.com**](http://portal.azure.com) and sign in by using a Microsoft account that has the Owner role in the Azure subscription you intend to use in this lab.

1. In the Azure portal, navigate to the **Create a resource** blade.

1. From the **Create a resource** blade, search Azure Marketplace for **Template deployment**.

1. Use the list of search results to navigate to the **Deploy a custom template** blade.

1. On the **Custom deployment** blade, select the **Build your own template in the editor**.

1. From the **Edit template** blade, load the template file **Labfiles\\AZ100\\Mod04\\az-100-04_01_azuredeploy.json**. 

   > **Note**: Review the content of the template and note that it defines deployment of an Azure VM hosting Windows Server 2016 Datacenter.

1. Save the template and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, navigate to the **Edit parameters** blade.

1. From the **Edit parameters** blade, load the parameters file **Labfiles\\AZ100\\Mod04\\az-100-04_azuredeploy.parameters.json**. 

1. Save the parameters and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, initiate a template deployment with the following settings:

    - Subscription: the name of the subscription you are using in this lab

    - Resource group: the name of a new resource group **az1000401-RG**

    - Location: the name of the Azure region which is closest to the lab location and where you can provision Azure VMs

    - Vm Size: **Standard_DS1_v2**

    - Vm1Name: **az1000401-vm1**

    - Vm2Name: **az1000401-vm2**

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

    - Virtual Network Name: **az1000401-vnet1**

   > **Note**: To identify Azure regions where you can provision Azure VMs, refer to [**https://azure.microsoft.com/en-us/regions/offers/**](https://azure.microsoft.com/en-us/regions/offers/)

   > **Note**: Do not wait for the deployment to complete but proceed to the next task. You will use the network and the virtual machines included in this deployment in the second exercise of this lab.


#### Task 2: Create the second virtual network in the same region hosting a single Azure VM by using an Azure Resource Manager template

1. In the Azure portal, navigate to the **Create a resource** blade.

1. From the **Create a resource** blade, search Azure Marketplace for **Template deployment**.

1. Use the list of search results to navigate to the **Deploy a custom template** blade.

1. On the **Custom deployment** blade, select the **Build your own template in the editor**.

1. From the **Edit template** blade, load the template file **Labfiles\\AZ100\\Mod04\\az-100-04_02_azuredeploy.json**. 

   > **Note**: Review the content of the template and note that it defines deployment of an Azure VM hosting Windows Server 2016 Datacenter.

1. Save the template and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, navigate to the **Edit parameters** blade.

1. From the **Edit parameters** blade, load the parameters file **Labfiles\\AZ100\\Mod04\\az-100-04_azuredeploy.parameters.json**. 

1. Save the parameters and return to the **Custom deployment** blade. 

1. From the **Custom deployment** blade, initiate a template deployment with the following settings:

    - Subscription: the name of the subscription you are using in this lab

    - Resource group: the name of a new resource group **az1000402-RG**

    - Location: the name of the Azure region which you selected in the previous task

    - Vm Size: **Standard_DS1_v2**

    - VmName: **az1000402-vm3**

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

    - Virtual Network Name: **az1000402-vnet2**

   > **Note**: Do not wait for the deployment to complete but proceed to the next task. You will use the network and the virtual machines included in this deployment in the second exercise of this lab.

> **Result**: After you completed this exercise, you have created two Azure virtual networks and initiated deployments of three Azure VM by using Azure Resource Manager templates.


## Exercise 1: Configure VNet peering 

Estimated Time: 5 minutes
  
The main tasks for this exercise are as follows:

1. Configure VNet peering for the first virtual network

1. Configure VNet peering for the second virtual network


#### Task 1: Configure VNet peering for the first virtual network
  
1. In the Azure portal, navigate to the **az1000401-vnet1** virtual network blade.

1. From the **az1000401-vnet1** virtual network blade, display its **Peerings** blade.

1. From the **az1000401-vnet1 - Peerings** blade, create a VNet peering with the following settings:

    - Name: **az1000401-vnet1-to-az1000402-vnet2**

    - Virtual network deployment model: **Resource manager**

    - Subscription: the name of the Azure subscription you are using in this lab

    - Virtual network: **az1000402-vnet2**

    - Allow virtual network access: **Enabled**

    - Allow forwarded traffic: disabled

    - Allow gateway transit: disabled

    - Use remote gateways: disabled


#### Task 2: Configure VNet peering for the second virtual network
  
1. In the Azure portal, navigate to the **az1000402-vnet2** virtual network blade.

1. From the **az1000402-vnet2** virtual network blade, display its **Peerings** blade.

1. From the **az1000402-vnet2 - Peerings** blade, create a VNet peering with the following settings:

    - Name: **az1000402-vnet2-to-az1000401-vnet1**

    - Virtual network deployment model: **Resource manager**

    - Subscription: the name of the Azure subscription you are using in this lab

    - Virtual network: **az1000401-vnet1**

    - Allow virtual network access: **Enabled**

    - Allow forwarded traffic: disabled

    - Allow gateway transit: disabled

    - Use remote gateways: disabled

> **Result**: After you completed this exercise, you have configured virtual network peering between the two virtual networks.


## Exercise 2: Implement custom routing
  
Estimated Time: 20 minutes

The main tasks for this exercise are as follows:

1. Enable IP forwarding for a network interface of an Azure VM

1. Configure user defined routing 

1. Configure routing in an Azure VM running Windows Server 2016


#### Task 1: Enable IP forwarding for a network interface of an Azure VM
  
   > **Note**: Before you start this task, ensure that the template deployments you started in Exercise 0 have completed. 

1. In the Azure portal, navigate to the blade of the second Azure VM **az1000401-vm2**.

1. From the **az1000401-vm2** blade, display its **Networking** blade. 

1. From the **az1000401-vm2 - Networking** blade, display the blade of the network adapter (**az1000401-nic2**) of the Azure VM.

1. From the **az1000401-nic2** blade, display its **IP configurations** blade.

1. From the **az1000401-nic2 - IP configurations** blade, enable **IP forwarding**.

   > **Note**: The Azure VM **az1000401-vm2**, which network interface you configured in this task, will function as a router, facilitating service chaining between the two virtual networks.


#### Task 2: Configure user defined routing 

1. In the Azure portal, navigate to the **Create a resource** blade.

1. From the **Create a resource** blade, search Azure Marketplace for **Route table**.

1. Use the list of search results to navigate to the **Create route table** blade.

1. From the **Create route table** blade, create a new route table with the following settings:

    - Name: **az1000402-rt1**

    - Subscription: the name of the Azure subscription you use for this lab

    - Resource group: **az1000402-LabRG**

    - Location: the same Azure region in which you created the virtual networks
  
    - BGP route propagation: **Disabled**

1. In the Azure portal, navigate to the **az1000402-rt1** blade.

1. From the **az1000402-rt1** blade, display its **Routes** blade. 

1. From the **az1000402-rt1 - Routes** blade, add to the route table a route with the following settings: 

    - Route name: **custom-route-to-az1000401-vnet1**

    - Address prefix: **10.104.0.0/16**

    - Next hop type: **Virtual appliance**

    - Next hop address: **10.104.1.4**

   > **Note**: **10.104.1.4** is the IP address of the network interface of **az1000401-vm2**, which will provide service chaining between the two virtual networks.

1. From the **az1000402-rt1** blade, display its **Subnets** blade. 

1. From the **az1000402-rt1 - Subnets** blade, associate the route table **az1000402-rt1** with **subnet0** of **az1000402-vnet2**.


#### Task 3: Configure routing in an Azure VM running Windows Server 2016

1. In the Azure portal, navigate to the blade of the **az1000401-vm2** Azure VM. 

1. From the **Overview** pane of the **az1000401-vm2** blade, generate an RDP file and use it to connect to **az1000401-vm2**.

1. When prompted, authenticate by specifying the following credentials:

    - User name: **Student**

    - Password: **Pa55w.rd1234**

1. Within the Remote Desktop session to **az1000401-vm2**, from **Server Manager**, use the **Add Roles and Features Wizard** to add the **Remote Access** server role with the **Routing** role service and all required features. 

   > **Note**: If you receive an error message **There may be a version mismatch between this computer and the destination server or VHD** once you select the **Remote Access**  checkbox on the **Server Roles** page of the **Add Roles and Features Wizard**, clear the checkbox, click **Next**, click **Previous** and select the **Remote Access**  checkbox again.

1. Within the Remote Desktop session to **az1000401-vm2**, from Server Manager, start the **Routing and Remote Access** console. 

1. In the **Routing and Remote Access** console, run **Routing and Remote Access Server Setup Wizard**, use the **Custom configuration** option, enable **LAN routing**, and start **Routing and Remote Access** service.

1. Within the Remote Desktop session to **az1000401-vm2**, start the **Windows Firewall with Advanced Security** console and enable **File and Printer Sharing (Echo Request - ICMPv4-In)** inbound rule for all profiles.

> **Result**: After completing this exercise, you have implemented custom routing between peered Azure virtual networks. 


## Exercise 3: Validating service chaining

Estimated Time: 10 minutes
  
The main tasks for this exercise are as follows:

1. Configure Windows Firewall with Advanced Security on the target Azure VM

1. Test service chaining between peered virtual networks


#### Task 1: Configure Windows Firewall with Advanced Security on the target Azure VM

1. In the Azure portal, navigate to the blade of the **az1000401-vm1** Azure VM. 

1. From the **Overview** pane of the **az1000401-vm1** blade, generate an RDP file and use it to connect to **az1000401-vm1**.

1. When prompted, authenticate by specifying the following credentials:

    - User name: **Student**

    - Password: **Pa55w.rd1234**

1. Within the Remote Desktop session to **az1000401-vm1**, open the **Windows Firewall with Advanced Security** console and enable **File and Printer Sharing (Echo Request - ICMPv4-In)** inbound rule for all profiles.


#### Task 2: Test service chaining between peered virtual networks
  
1. In the Azure portal, navigate to the blade of the **az1000402-vm3** Azure VM. 

1. From the **Overview** pane of the **az1000402-vm3** blade, generate an RDP file and use it to connect to **az1000402-vm3**.

1. When prompted, authenticate by specifying the following credentials:

    - User name: **Student**

    - Password: **Pa55w.rd1234**

1. Once you are connected to **az1-1000402-vm3** via the Remote Desktop session, start **Windows PowerShell**.

1. In the **Windows PowerShell** window, run the following:

   ```
   Test-NetConnection -ComputerName 10.104.0.4 -TraceRoute
   ```

   > **Note**: **10.104.0.4** is the IP address of the network interface of the first Azure VM **az1000401-vm1**

1. Verify that test is successful and note that the connection was routed over **10.104.1.4**

   > **Note**: Without custom routing in place, the traffic would flow directly between the two Azure VMs. 
> **Result**: After you completed this exercise, you have validated service chaining between peered Azure virtual networks.
