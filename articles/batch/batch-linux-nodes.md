---
title: Ausführen von Linux auf virtuellen Computeknoten
description: Erfahren Sie, wie parallele Compute-Workloads auf Pools auf virtuellen Linux-Computern in Azure Batch verarbeitet werden.
ms.topic: how-to
ms.date: 06/01/2018
ms.custom: H1Hack27Feb2017, tracking-python
ms.openlocfilehash: 1a87b72b16c1d41e5569c4ce9e5e1a6e2c51a2e0
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2020
ms.locfileid: "85960077"
---
# <a name="provision-linux-compute-nodes-in-batch-pools"></a>Bereitstellen von Linux-Computeknoten in Batch-Pools

Sie können Azure Batch verwenden, um Computeworkloads auf virtuellen Linux- und Windows-Computern parallel auszuführen. In diesem Artikel erfahren Sie, wie Sie im Batch-Dienst mithilfe der Clientbibliotheken [Batch Python][py_batch_package] und [Batch .NET][api_net] Pools mit Linux-Computeknoten erstellen.

> [!NOTE]
> Anwendungspakete werden für alle Batch-Pools unterstützt, die nach dem 5. Juli 2017 erstellt wurden. Für Batch-Pools, die zwischen dem 10. März 2016 und dem 5. Juli 2017 erstellt wurden, werden sie nur unterstützt, wenn der Pool mit einer Clouddienstkonfiguration erstellt wurde. Batch-Pools, die vor dem 10. März 2016 erstellt wurden, unterstützen keine Anwendungspakete. Weitere Informationen zum Bereitstellen von Anwendungen für Batch-Knoten mithilfe von Anwendungspaketen finden Sie unter [Anwendungsbereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md).
>
>

## <a name="virtual-machine-configuration"></a>Konfiguration des virtuellen Computers
Wenn Sie einen Pool von Computeknoten in Batch erstellen, stehen Ihnen zwei Optionen zur Auswahl der Knotengröße und des Betriebssystems zur Verfügung: die Clouddienstkonfiguration und die Konfiguration des virtuellen Computers.

**Clouddienstkonfiguration** stellt *nur*. Die verfügbaren Größen für Computeknoten sind unter [Größen für Clouddienste](../cloud-services/cloud-services-sizes-specs.md) aufgelistet. Die verfügbaren Betriebssysteme sind unter [Azure-Gastbetriebssystemversionen und SDK-Kompatibilitätsmatrix](../cloud-services/cloud-services-guestos-update-matrix.md) aufgeführt. Beim Erstellen eines Pools, der Azure Cloud Services-Knoten enthält, geben Sie die Knotengröße und dessen Betriebssystemfamilie an, die in den zuvor angegebenen Artikeln beschrieben werden. Für Pools mit Windows-Computeknoten wird Cloud Services am häufigsten verwendet.

**Virtual Machine Configuration** stellt sowohl Linux- als auch Windows-Images für Computeknoten bereit. Die verfügbaren Größen für Computeknoten sind unter [Größen für virtuelle Computer in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) und [Größen für virtuelle Computer in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows) aufgelistet. Beim Erstellen eines Pools, der Knoten mit der Konfiguration des virtuellen Computers enthält, müssen Sie die Knotengröße, die VM-Imagereferenz und die Knoten-Agent-SKU von Batch für die Installation auf den Knoten angeben.

### <a name="virtual-machine-image-reference"></a>VM-Imagereferenz

Der Batch-Dienst verwendet [VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md), um Computeknoten in der VM-Konfiguration bereitzustellen. Sie können ein Image aus dem [Azure Marketplace][vm_marketplace] angeben oder ein benutzerdefiniertes Image bereitstellen, das Sie vorbereitet haben. Ausführlichere Informationen zu benutzerdefinierten Images finden Sie unter [Erstellen eines Pools mit Shared Image Gallery](batch-sig-images.md).

Wenn Sie eine VM-Imagereferenz konfigurieren, müssen Sie die Eigenschaften des VM-Images festlegen. Die folgenden Eigenschaften sind erforderlich, wenn Sie eine VM-Imagereferenz erstellen:

| **Imagereferenzeigenschaften** | **Beispiel** |
| --- | --- |
| Herausgeber |Canonical |
| Angebot |UbuntuServer |
| SKU |18.04-LTS |
| Version |latest |

> [!TIP]
> Weitere Informationen zu diesen Eigenschaften und zur Auflistung von Marketplace-Images finden Sie unter [Navigieren zu und Auswählen von Images virtueller Linux-Computer in Azure mithilfe der Befehlszeilenschnittstelle oder PowerShell](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Beachten Sie, dass nicht alle Marketplace-Images derzeit mit Batch kompatibel sind. Weitere Informationen hierzu finden Sie unter [Knoten-Agent-SKU](#node-agent-sku).
>
>

### <a name="node-agent-sku"></a>Knoten-Agent-SKU
Der Batch-Knoten-Agent ist ein Programm, das auf jedem Knoten im Pool ausgeführt wird. Er stellt die Befehls- und Steuerungsschnittstelle zwischen dem Knoten und dem Batch-Dienst dar. Es gibt verschiedene Implementierungen des Knoten-Agents (SKUs) für verschiedene Betriebssysteme. Insbesondere beim Erstellen einer Konfiguration für einen virtuellen Computer legen Sie zuerst die VM-Imagereferenz und anschließend den Knoten-Agent fest, der auf dem Image installiert werden soll. In der Regel ist jede Knoten-Agent-SKU mit mehreren VM-Images kompatibel. Hier sind einige Beispiele für Knoten-Agent-SKUs:

* batch.node.ubuntu 18.04
* batch.node.centos 7
* batch.node.windows amd64

> [!IMPORTANT]
> Nicht alle im Marketplace verfügbaren VM-Images sind mit den derzeit verfügbaren Batch-Knoten-Agents kompatibel. Verwenden Sie die Batch-SDKs, um die verfügbaren Knoten-Agent-SKUs und die VM-Images, mit denen sie kompatibel sind, aufzulisten. Weitere Informationen und Beispiele, wie eine Liste der gültigen Images zur Runtime abgerufen wird, finden Sie in diesem Artikel unter [Liste der VM-Images](#list-of-virtual-machine-images).
>
>

## <a name="create-a-linux-pool-batch-python"></a>Erstellen eines Linux-Pools: Batch Python
Der folgende Codeausschnitt zeigt ein Beispiel für die Erstellung eines Pools mit Ubuntu Server-Computeknoten mithilfe der [Microsoft Azure Batch-Clientbibliothek für Python][py_batch_package]. Die Referenzdokumentation für das Batch-Python-Modul finden Sie unter [azure.batch-Paket][py_batch_docs] unter „Dokumentation lesen“.

In diesem Ausschnitt erstellen wir explizit einen Imageverweis ([ImageReference][py_imagereference]) und legen die einzelnen Eigenschaften (Herausgeber, Angebot, SKU, Version) fest. Im Produktionscode empfehlen wir Ihnen jedoch die Verwendung der Methode [list_supported_images][py_list_supported_images], um die verfügbaren Kombinationen aus Image und Knoten-Agent-SKU zur Laufzeit zu bestimmen und eine Auswahl zu treffen.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_D2_V3"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, batch_url)
client = batch.BatchServiceClient(creds, batch_url)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id=pool_id, vm_size=vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher="Canonical",
    offer="UbuntuServer",
    sku="18.04-LTS",
    version="latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference=ir,
    node_agent_sku_id="batch.node.ubuntu 18.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Wie bereits erwähnt, wird empfohlen, [ImageReference][py_imagereference] nicht explizit zu erstellen, sondern die Methode [list_supported_images][py_list_supported_images] zu verwenden, um aus den derzeit unterstützten Kombinationen aus Knoten-Agent und Marketplace-Image dynamisch eine Auswahl zu treffen. Der folgende Python-Ausschnitt veranschaulicht die Verwendung dieser Methode.

```python
# Get the list of supported images from the Batch service
images = client.account.list_supported_images()

# Obtain the desired image reference
image = None
for img in images:
  if (img.image_reference.publisher.lower() == "canonical" and
        img.image_reference.offer.lower() == "ubuntuserver" and
        img.image_reference.sku.lower() == "18.04-lts"):
    image = img
    break

if image is None:
  raise RuntimeError('invalid image reference for desired configuration')

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference=image.image_reference,
    node_agent_sku_id=image.node_agent_sku_id)
```

## <a name="create-a-linux-pool-batch-net"></a>Erstellen eines Linux-Pools: Batch .NET
Der folgende Codeausschnitt zeigt ein Beispiel für die Erstellung eines Pools mit Ubuntu Server-Computeknoten mithilfe der [Batch .NET][nuget_batch_net]-Clientbibliothek. Die [Batch .NET-Referenzdokumentation][api_net] finden Sie unter docs.microsoft.com.

Im folgenden Codeausschnitt wird die Methode [PoolOperations][net_pool_ops].[ListSupportedImages][net_list_supported_images] verwendet, um aus der Liste mit den derzeit unterstützten Kombinationen von Marketplace-Image und Knoten-Agent-SKU eine Auswahl zu treffen. Diese Technik ist von Vorteil, da sich die Liste mit den unterstützten Kombinationen von Zeit zu Zeit ändern kann. In den meisten Fällen werden unterstützte Kombinationen hinzugefügt.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_D2_V3";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<ImageInformation> images =
    batchClient.PoolOperations.ListSupportedImages().ToList();

// Find the appropriate image information
ImageInformation image = null;
foreach (var img in images)
{
    if (img.ImageReference.Publisher == "Canonical" &&
        img.ImageReference.Offer == "UbuntuServer" &&
        img.ImageReference.Sku == "18.04-LTS")
    {
        image = img;
        break;
    }
}

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(image.ImageReference, image.NodeAgentSkuId);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicatedComputeNodes: nodeCount);

// Commit the pool to the Batch service
await pool.CommitAsync();
```

Im vorherigen Codeausschnitt wird zwar die Methode [PoolOperations][net_pool_ops].[ListSupportedImages][net_list_supported_images] verwendet, um unterstützte Kombinationen aus Image und Knoten-Agent-SKU dynamisch aufzulisten und eine Auswahl zu treffen (empfohlene Vorgehensweise), Sie können aber auch explizit einen [Imageverweis][net_imagereference] (ImageReference) konfigurieren:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "18.04-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Liste mit VM-Images
Um die Liste aller unterstützten Marketplace-VM-Images für den Batch-Dienst und die entsprechenden Knoten-Agents zu erhalten, verwenden Sie [list_supported_images][py_list_supported_images] (Python), [ListSupportedImages][net_list_supported_images] (Batch.NET) oder die entsprechende API im jeweiligen SDK der Sprache Ihrer Wahl.

## <a name="connect-to-linux-nodes-using-ssh"></a>Herstellen einer Verbindung mit Linux-Knoten über SSH
Während der Entwicklung oder bei der Fehlerbehebung ist es unter Umständen erforderlich, sich bei den Knoten im Pool anzumelden. Im Gegensatz zu Windows-Computeknoten können Sie das Remotedesktopprotokoll (RDP) für die Herstellung einer Verbindung mit Linux-Knoten nicht verwenden. Stattdessen ermöglicht der Batch-Dienst den SSH-Zugriff auf jeden Knoten zum Herstellen einer Remoteverbindung.

Mit dem folgenden Python-Codeausschnitt wird ein Benutzer für jeden Knoten eines Pools erstellt, der für eine Remoteverbindung erforderlich ist. Anschließend werden die SSH-Verbindungsinformationen (Secure Shell) für die einzelnen Knoten ausgegeben.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
    credentials,
    base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Hier ist eine Beispielausgabe für den vorherigen Code eines Pools mit vier Linux-Knoten angegeben:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Anstelle eines Kennworts können Sie einen öffentlichen SSH-Schlüssel festlegen, wenn Sie einen Benutzer auf einem Knoten erstellen. Verwenden Sie im Python-SDK den Parameter **ssh_public_key** für [ComputeNodeUser][py_computenodeuser]. Verwenden Sie in .NET die Eigenschaft [ComputeNodeUser][net_computenodeuser].[SshPublicKey][net_ssh_key].

## <a name="pricing"></a>Preise
Azure Batch basiert auf der Technologie von Azure Cloud Services und von Azure Virtual Machines. Der Batch-Dienst selbst wird kostenlos angeboten, d. h. dass Ihnen nur die Computeressourcen (und die damit verbundenen Kosten) in Rechnung gestellt werden, die Ihre Batch-Lösungen beanspruchen. Wenn Sie sich für die **Cloud Services-Konfiguration** entscheiden, erfolgt die Abrechnung auf der Grundlage der Preisstruktur, die Sie unter [Cloud Services-Preise][cloud_services_pricing] finden. Wenn Sie sich für die **VM-Konfiguration** entscheiden, erfolgt die Abrechnung auf der Grundlage der Preisstruktur unter [Virtuelle Computer – Preise][vm_pricing].

Wenn Sie Anwendungen mit [Anwendungspaketen](batch-application-packages.md) auf Ihren Batch-Knoten bereitstellen, werden auch die von Ihren Anwendungspaketen genutzten Azure Storage-Ressourcen in Rechnung gestellt.

## <a name="next-steps"></a>Nächste Schritte

Die [Python-Codebeispiele][github_samples_py] im Repository [azure-batch-samples][github_samples] auf GitHub enthalten Skripts, die die Durchführung allgemeiner Batch-Vorgänge wie etwa die Erstellung von Pools, Aufträgen und Aufgaben veranschaulichen. Die jeweils zu den Python-Beispielen gehörende [INFODATEI][github_py_readme] enthält Details zur Installation der erforderlichen Pakete.

[api_net]: /dotnet/api/microsoft.azure.batch
[api_net_mgmt]: /dotnet/api/overview/azure/batch
[api_rest]: /rest/api/batchservice/
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: /dotnet/api/microsoft.azure.batch.cloudpool
[net_computenodeuser]: /dotnet/api/microsoft.azure.batch.computenodeuser?view=azure-dotnet
[net_imagereference]: /dotnet/api/microsoft.azure.batch.imagereference
[net_list_supported_images]: /dotnet/api/microsoft.azure.batch.pooloperations.listsupportedimages
[net_pool_ops]: /dotnet/api/microsoft.azure.batch.pooloperations
[net_ssh_key]: /dotnet/api/microsoft.azure.batch.computenodeuser.sshpublickey?view=azure-dotnet#Microsoft_Azure_Batch_ComputeNodeUser_SshPublicKey
[nuget_batch_net]: https://www.nuget.org/packages/Microsoft.Azure.Batch/
[rest_add_pool]: /rest/api/batchservice/pool/add
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: https://azure.github.io/azure-sdk-for-python/ref/Batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: /python/api/azure-batch/azure.batch.models.computenodeuser
[py_imagereference]: /python/api/azure-mgmt-batch/azure.mgmt.batch.models.imagereference
[py_list_supported_images]: /python/api/azure-batch/azure.batch.operations.AccountOperations?view=azure-python
[vm_marketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/category/compute?filters=virtual-machine-images&page=1
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
