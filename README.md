This template deploys a DataStax Enterprise (DSE) cluster to Azure running on Ubuntu virtual machines. The template also provisions a storage account, virtual network and public IP address required by the installation.

The button below will deploy this template to Azure.  The template will be dynamically linked directly from this github repository.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FDSPN%2Fazure-arm-dse%2Fmaster%2FmainTemplate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

The template expects the following parameters:

| Name   | Description |
|:--- |:---|
| region | Region where the cluster will be created |
| nodeCount | The number of virtual machines to provision for the cluster |
| virtualMachineSize | The size of virtual machine to provision for each cluster node |
| username  | SSH username for the virtual machines |
| password  | SSH password for the virtual machines |
| datastaxUsername | Your DataStax account username.  You can register at http://www.datastax.com/download |
| datastaxPassword | Your DataStax account password.  You can register at http://www.datastax.com/download |

Once the Azure VMs, virtual network and storage are setup, the template installs prerequisites like Java on the DSE nodes.  These have static IPs starting at 10.0.0.6 which are accessible on the internal virtual network.  

The template also sets up a node to run DataStax OpsCenter.  This node has the internal IP 10.0.0.5 as well as an external IP.  Ports 22 (SSH), 8888 (HTTP), and 8443 (HTTPS) are enabled.

The script opscenter.sh installs OpsCenter and creates a cluster using the OpsCenter REST API.  When the API call is made, OpsCenter installs DSE on all the cluster nodes and starts it up.  

On completion, OpsCenter will be accessible at `http://{resourceGroup}cluster.{region}.cloudapp.azure.com:8888` For instance, if you created a deployment with the resourceGroup parameter set to datastax in the West US region you could access OpsCenter for the deployment at `http://datastaxcluster.westus.cloudapp.azure.com:8888`  The default username and password for OpsCenter is admin.

