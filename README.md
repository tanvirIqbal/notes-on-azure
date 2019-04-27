# Learning Azure

## create a resource group

```shell
az group create --name --location  
az group create --name rg --location southeastasia  
```

## create a username and password

```shell
USERNAME=azureuser  
PASSWORD=$(openssl rand -base64 32)  
```

The username and password here are stored as Bash variables.  

## create a VM

```shell
az vm create \
  --name myVM \
  --resource-group 59e9841f-2999-48b1-bc8f-fea72f5f98c8 \
  --image Win2016Datacenter \
  --size Standard_DS2_v2 \
  --location eastus \
  --admin-username $USERNAME \
  --admin-password $PASSWORD  
  ```

* The VM is named myVM. This name identifies the VM in Azure. It also becomes the VM's internal hostname, or computer name.  
* The resource group, or the VM's logical container, is named 59e9841f-2999-48b1-bc8f-fea72f5f98c8.  
* Win2016Datacenter specifies the Windows Server 2016 VM image.  
* Standard_DS2_v2 refers to the size of the VM. This size has two virtual CPUs and 7 GB of memory.  
* The username and password enable you to connect to your VM later. For example, you can connect over Remote Desktop or WinRM to work with and configure the system  

## Verify your VM is running  

```shell
az vm get-instance-view \
  --name myVM \
  --resource-group 59e9841f-2999-48b1-bc8f-fea72f5f98c8 \
  --output table  
```