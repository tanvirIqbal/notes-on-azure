# Learning Azure

## create a resource group

```shell
az group create --name --location  
az group create --name myResourceGroup --location southeastasia  
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
  --resource-group myResourceGroup \
  --image Win2016Datacenter \
  --size Standard_DS2_v2 \
  --location eastus \
  --admin-username $USERNAME \
  --admin-password $PASSWORD  
  ```

* The VM is named myVM. This name identifies the VM in Azure. It also becomes the VM's internal hostname, or computer name.  
* The resource group, or the VM's logical container, is named myResourceGroup.  
* Win2016Datacenter specifies the Windows Server 2016 VM image.  
* Standard_DS2_v2 refers to the size of the VM. This size has two virtual CPUs and 7 GB of memory.  
* The username and password enable you to connect to your VM later. For example, you can connect over Remote Desktop or WinRM to work with and configure the system  
* By default, Azure assigns a public IP address to your VM.  

## Verify your VM is running  

```shell
az vm get-instance-view \
  --name myVM \
  --resource-group myResourceGroup \
  --output table  
```  

## create and Configure IIS using Custom Script Extension

```shell
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name CustomScriptExtension \
  --publisher Microsoft.Compute \
  --settings "{'fileUris':['https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-iis.ps1']}" \
  --protected-settings "{'commandToExecute': 'powershell -ExecutionPolicy Unrestricted -File configure-iis.ps1'}"
```  

## open port 80 (HTTP) through the firewall  

```shell
az vm open-port \
  --name myVM \
  --resource-group myResourceGroup \
  --port 80
```

## list your VM's public IP address

```shell
az vm show \
  --name myVM \
  --resource-group myResourceGroup \
  --show-details \
  --query [publicIps] \
  --output tsv
```  

You see your VM's public IP address, for example, 104.211.9.245.

In a new browser tab, navigate to your VM's IP address (<http://> followed by the IP address).