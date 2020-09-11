---
title: Erstellen von Computeressourcen mit dem Python-SDK
titleSuffix: Azure Machine Learning
description: Verwenden des Python-SDK für Azure Machine Learning zum Erstellen von Computeressourcen für Training und Bereitstellung (Computeziele) für maschinelles Lernen
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.date: 07/08/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, contperfq1
ms.openlocfilehash: 96aa6839fe51bb8a8c26f411c1a1f9df6b8c5a7f
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2020
ms.locfileid: "89147263"
---
# <a name="create-compute-targets-for-model-training-and-deployment-with-python-sdk"></a>Erstellen von Computezielen für das Training und die Bereitstellung von Modellen mit dem Python-SDK

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie mit dem Python-SDK für Azure Machine Learning Computeziele erstellen und verwalten. Sie können Computeziele auch mit folgenden Komponenten erstellen und verwalten:
* [Azure Machine Learning Studio](how-to-create-attach-compute-studio.md), 
* [CLI-Erweiterung](reference-azure-machine-learning-cli.md#resource-management) für Azure Machine Learning
* [VS Code-Erweiterung](how-to-manage-resources-vscode.md#compute-clusters) für Azure Machine Learning


## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.
* über das [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* Ein [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md).

## <a name="whats-a-compute-target"></a>Was ist ein Computeziel?

Mit Azure Machine Learning können Sie Ihr Modell für eine Vielzahl von Ressourcen oder Umgebungen trainieren, die zusammen als [__Computeziele__](concept-azure-machine-learning-architecture.md#compute-targets) bezeichnet werden. Ein Computeziel kann ein lokaler Computer oder eine Cloudressource sein, wie beispielsweise Azure Machine Learning Compute, Azure HDInsight oder ein virtueller Remotecomputer.  Sie können auch Computeziele für die Modellimplementierung erstellen, wie in [Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst](how-to-deploy-and-where.md) beschrieben.

## <a name="compute-targets-for-training"></a>Computeziele für das Training

Azure Machine Learning bietet unterschiedliche Unterstützung für verschiedene Computeziele. Ein typischer Modellentwicklungslebenszyklus beginnt mit der Entwicklung/Experimenten mit einer kleinen Menge von Daten. In dieser Phase empfehlen wir die Verwendung einer lokalen Umgebung. Verwenden Sie beispielsweise Ihren lokalen Computer oder eine cloudbasierte VM. Wenn Sie Ihr Training zur Verwendung größerer Datasets hochskalieren oder sich für ein verteiltes Training entscheiden, empfehlen wir, mit Azure Machine Learning Compute einen Cluster mit einem einzelnen oder mehreren Knoten zu erstellen, der bei jeder Übermittlung einer Ausführung automatisch skaliert wird. Sie können auch Ihre eigene Computeressource anfügen. Die Unterstützung für verschiedene Szenarien variiert jedoch wie unten beschrieben:

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]

> [!NOTE]
> Azure Machine Learning Compute-Cluster können als dauerhafte Ressourcen oder dynamisch beim Anfordern einer Ausführung erstellt werden. Bei der ausführungsbasierten Erstellung wird das Computeziel nach Abschluss des Trainingsauftrags entfernt, sodass Sie auf diese Weise erstellte Computeziele nicht wiederverwenden können.

Konfigurieren Sie die Computeziele anhand der folgenden Abschnitte:

* [Lokaler Computer](#local)
* [Azure Machine Learning Compute-Cluster](#amlcompute)
* [Azure Machine Learning-Computeinstanz](#instance)
* [Virtuelle Remotecomputer](#vm)
* [Azure HDInsight](#hdinsight)


## <a name="local-computer"></a><a id="local"></a>Lokaler Computer

Wenn Sie Ihren lokalen Computer für das Training verwenden, ist es nicht erforderlich, ein Computeziel zu erstellen.  [Übermitteln Sie einfach die Trainingsausführung](how-to-set-up-training-targets.md) von Ihrem lokalen Computer aus.

## <a name="azure-machine-learning-compute-cluster"></a><a id="amlcompute"></a>Azure Machine Learning-Computecluster

Ein Azure Machine Learning-Computecluster ist eine verwaltete Computeinfrastruktur, die Ihnen das einfache Erstellen von Computezielen mit einem oder mehreren Knoten ermöglicht. Das Computeziel wird in Ihrer Arbeitsbereichsregion als Ressource erstellt, die für andere Benutzer im Arbeitsbereich freigegeben werden kann. Es wird automatisch zentral hochskaliert, wenn ein Auftrag übermittelt wird, und kann in einem virtuellen Azure-Netzwerk platziert werden kann. Das Computeziel wird in einer Containerumgebung ausgeführt und packt die Abhängigkeiten Ihres Modells in einem [Docker-Container](https://www.docker.com/why-docker).

Sie können Azure Machine Learning Compute verwenden, um den Trainingsprozess auf einen Cluster von CPU- oder GPU-Computeknoten in der Cloud zu verteilen. Weitere Informationen zu den VM-Größen mit GPUs finden Sie unter [Für GPU optimierte VM-Größen](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu). 

Bei Azure Machine Learning Compute gelten Standardgrenzwerte, beispielsweise für die Anzahl von Kernen, die zugeordnet werden können. Weitere Informationen finden Sie unter [Verwalten und Anfordern von Kontingenten für Azure-Ressourcen](how-to-manage-quotas.md).

> [!TIP]
> Cluster können in der Regel auf bis zu 100 Knoten skaliert werden, solange Ihr Kontingent für die Anzahl der erforderlichen Kerne ausreicht. Standardmäßig werden Cluster mit aktivierter Kommunikation zwischen den Knoten des Clusters eingerichtet, um beispielsweise MPI-Aufträge zu unterstützen. Sie können Ihre Cluster jedoch auf 1.000 Knoten skalieren, indem Sie einfach [ein Supportticket einreichen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) und die Aufnahme Ihrer Anwendung, des Arbeitsbereichs oder eines bestimmten Clusters in die Zulassungsliste anfordern, um die Kommunikation zwischen den Knoten zu deaktivieren. 

Azure Machine Learning Compute kann in mehreren Ausführungen wiederverwendet werden. Compute kann für andere Benutzer im Arbeitsbereich freigegeben werden und wird zwischen den Ausführungen beibehalten. Dabei werden Knoten basierend auf der Anzahl der übermittelten Ausführungen und der Einstellung „max_nodes“ für Ihren Cluster automatisch hoch- oder herunterskaliert. Die Einstellung „min_nodes“ steuert die Mindestanzahl verfügbarer Knoten.

[!INCLUDE [min-nodes-note](../../includes/machine-learning-min-nodes.md)]

1. **Erstellen und Anfügen**: Um eine persistente Azure Machine Learning Compute-Ressource in Python zu erstellen, geben Sie die Eigenschaften **vm_size** und **max_nodes** an. Azure Machine Learning verwendet dann für die restlichen Eigenschaften intelligente Standardwerte. Die Computeressource skaliert automatisch auf Null herunter, wenn sie nicht benötigt wird.   Dedizierte VMs werden erstellt, um Ihre Aufträge bei Bedarf auszuführen.
    
    * **vm_size**: Die VM-Familie der von Azure Machine Learning Compute erstellten Knoten.
    * **max_nodes**: Die maximale Anzahl von Knoten, auf die das Computeziel bei der Ausführung eines Auftrags in Azure Machine Learning Compute automatisch hochskaliert wird.
    
   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=cpu_cluster)]

   Sie können beim Erstellen von Azure Machine Learning Compute auch mehrere erweiterte Eigenschaften konfigurieren. Mithilfe der Eigenschaften können Sie einen persistenten Cluster mit einer festen Größe oder in einem vorhandenen virtuellen Azure-Netzwerk in Ihrem Abonnement erstellen.  Weitere Informationen finden Sie in der [AmlCompute-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py
    ).

    Sie können auch eine persistente Azure Machine Learning Compute-Ressource [ in Azure Machine Learning Studio](how-to-create-attach-compute-studio.md#portal-create) erstellen und anfügen.

Nachdem Sie nun das Computeziel angefügt haben, besteht der nächste Schritt im [Übermitteln der Trainingsausführung](how-to-set-up-training-targets.md).

 ### <a name="lower-your-compute-cluster-cost"></a><a id="low-pri-vm"></a> Senken Ihrer Computeclusterkosten

Sie können sich auch dafür entscheiden, [virtuelle Computer mit niedriger Priorität](concept-plan-manage-cost.md#low-pri-vm) zu verwenden, um einige oder alle Ihre Workloads auszuführen. Diese virtuellen Computer haben keine garantierte Verfügbarkeit und können während der Verwendung vorzeitig entfernt werden. Ein vorzeitig entfernter Auftrag wird neu gestartet, nicht fortgesetzt. 

Verwenden Sie eine der folgenden Methoden, um einen virtuellen Computer mit niedriger Priorität anzugeben:
    
* Wählen Sie in Studio beim Erstellen einer VM **Niedrige Priorität** aus.
    
* Legen Sie mit dem Python SDK das `vm_priority`-Attribut in Ihrer Bereitstellungskonfiguration fest.  
    
    ```python
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                                vm_priority='lowpriority',
                                                                max_nodes=4)
    ```
    
* Legen Sie an der Befehlszeilenschnittstelle die `vm-priority` fest:
    
    ```azurecli-interactive
    az ml computetarget create amlcompute --name lowpriocluster --vm-size Standard_NC6 --max-nodes 5 --vm-priority lowpriority
    ```

 ### <a name="set-up-managed-identity"></a><a id="managed-identity"></a> Einrichten einer verwalteten Identität

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-managed-identity-intro.md)]

Konfigurieren der verwalteten Identität in der Bereitstellungskonfiguration:  
    
* Systemseitig zugewiesene verwaltete Identität:
    ```python
    # configure cluster with a system-assigned managed identity
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                            max_nodes=5,
                                                            identity_type="SystemAssigned",
                                                            )
    ```

* Benutzerseitig zugewiesene verwaltete Identität: 

    ```python
    # configure cluster with a user-assigned managed identity
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                            max_nodes=5,
                                                            identity_type="UserAssigned",
                                                            identity_id=['/subscriptions/<subcription_id>/resourcegroups/<resource_group>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<user_assigned_identity>'])

    cpu_cluster_name = "cpu-cluster"
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)
    ```

Hinzufügen einer verwalteten Identität zu einem vorhandenen Computecluster:  
    
* Systemseitig zugewiesene verwaltete Identität:

    ```python
    # add a system-assigned managed identity
    cpu_cluster.add_identity(identity_type="SystemAssigned")
    ````

* Benutzerseitig zugewiesene verwaltete Identität:

    ```python
    # add a user-assigned managed identity
    cpu_cluster.add_identity(identity_type="UserAssigned", 
                                identity_id=['/subscriptions/<subcription_id>/resourcegroups/<resource_group>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<user_assigned_identity>'])
    ```

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-managed-identity-note.md)]

#### <a name="managed-identity-usage"></a>Nutzung von verwalteten Identitäten

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-managed-identity-default.md)]


## <a name="azure-machine-learning-compute-instance"></a><a id="instance"></a>Azure Machine Learning-Computeinstanz

Eine [Azure Machine Learning-Computeinstanz](concept-compute-instance.md) ist eine verwaltete Computeinfrastruktur, die Ihnen das einfache Erstellen einer einzelnen VM ermöglicht. Das Computeziel wird in Ihrer Arbeitsbereichsregion erstellt, aber im Gegensatz zu einem Computecluster kann eine Instanz nicht für andere Benutzer in Ihrem Arbeitsbereich freigegeben werden. Außerdem wird die Instanz nicht automatisch herunterskaliert.  Sie müssen die Ressource beenden, um laufende Gebühren zu vermeiden.

Computeinstanzen können mehrere Aufträge parallel ausführen und verfügen über eine Auftragswarteschlange. 

Sie können Aufträge sicher in einer [virtuellen Netzwerkumgebung](how-to-enable-virtual-network.md#compute-instance) ausführen, ohne dass Unternehmen hierfür SSH-Ports öffnen müssen. Der Auftrag wird in einer Containerumgebung ausgeführt und packt die Abhängigkeiten Ihres Modells in einen Docker-Container. 

1. **Erstellen und Anfügen**: 
    
    ```python
    import datetime
    import time
    
    from azureml.core.compute import ComputeTarget, ComputeInstance
    from azureml.core.compute_target import ComputeTargetException
    
    # Choose a name for your instance
    # Compute instance name should be unique across the azure region
    compute_name = "ci{}".format(ws._workspace_id)[:10]
    
    # Verify that instance does not exist already
    try:
        instance = ComputeInstance(workspace=ws, name=compute_name)
        print('Found existing instance, use it.')
    except ComputeTargetException:
        compute_config = ComputeInstance.provisioning_configuration(
            vm_size='STANDARD_D3_V2',
            ssh_public_access=False,
            # vnet_resourcegroup_name='<my-resource-group>',
            # vnet_name='<my-vnet-name>',
            # subnet_name='default',
            # admin_user_ssh_public_key='<my-sshkey>'
        )
        instance = ComputeInstance.create(ws, compute_name, compute_config)
        instance.wait_for_completion(show_output=True)
    ```

Nachdem Sie nun das Computeziel angefügt und die Ausführung konfiguriert haben, besteht der nächste Schritt im [Übermitteln der Trainingsausführung](how-to-set-up-training-targets.md).


## <a name="remote-virtual-machines"></a><a id="vm"></a>Virtuelle Remotecomputer

Azure Machine Learning bietet Ihnen auch die Möglichkeit, eine eigene Computeressource zu verwenden und diese an Ihren Arbeitsbereich anzufügen. Eine Remote-VM, auf die über Azure Machine Learning zugegriffen werden kann, ist ein solcher Ressourcentyp. Die Ressource kann entweder eine Azure-VM, ein Remoteserver in Ihrer Organisation oder lokal sein. Durch Angabe der IP-Adresse und Anmeldeinformationen (Benutzername und Kennwort oder SSH-Schlüssel) können Sie jede VM, auf die zugegriffen werden kann, für Remoteausführungen verwenden.

Sie können eine systemseitig erstellte Conda-Umgebung, eine bereits vorhandene Python-Umgebung oder einen Docker-Container verwenden. Zur Ausführung auf einem Docker-Container muss die Docker-Engine auf der VM ausgeführt werden. Diese Funktion ist besonders nützlich, wenn Sie eine cloudbasierte Entwicklungs-/Experimentierumgebung nutzen möchten, die Ihnen mehr Flexibilität bietet als Ihr lokaler Computer.

Verwenden Sie in diesem Szenario Azure Data Science Virtual Machine (DSVM) als virtuellen Azure-Computer. Diese VM ist eine vorkonfigurierte Data Science- und KI-Entwicklungsumgebung in Azure. Die VM bietet eine zusammengestellte Auswahl an Tools und Frameworks für die Entwicklung des maschinellen Lernens über den gesamten Lebenszyklus. Weitere Informationen zum Verwenden der DSVM mit Azure Machine Learning finden Sie unter [Konfigurieren einer Entwicklungsumgebung](https://docs.microsoft.com/azure/machine-learning/how-to-configure-environment#dsvm).

1. **Erstellen**: Erstellen Sie eine DSVM, bevor Sie si zum Trainieren Ihres Modells verwenden. Informationen zum Erstellen dieser Ressource finden Sie in [Bereitstellen der Data Science Virtual Machine für Linux (Ubuntu)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro).

    > [!WARNING]
    > Von Azure Machine Learning werden nur virtuelle Computer unterstützt, auf denen **Ubuntu** ausgeführt wird. Wenn Sie eine VM erstellen oder eine vorhandene VM auswählen, müssen Sie eine VM auswählen, die Ubuntu verwendet.
    > 
    > Von Azure Machine Learning wird außerdem vorausgesetzt, dass der virtuelle Computer über eine __öffentliche IP-Adresse__ verfügt.

1. **Anfügen**: Wenn Sie einen vorhandenen virtuellen Computer als Computeziel anfügen möchten, müssen Sie die Ressourcen-ID, den Benutzernamen und das Kennwort für den virtuellen Computer angeben. Die Ressourcen-ID des virtuellen Computers kann unter Verwendung der Abonnement-ID, des Ressourcengruppennamens und des Namens des virtuellen Computers im folgenden Zeichenfolgenformat erstellt werden: `/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Compute/virtualMachines/<vm_name>`.

 
   ```python
   from azureml.core.compute import RemoteCompute, ComputeTarget

   # Create the compute config 
   compute_target_name = "attach-dsvm"
   
   attach_config = RemoteCompute.attach_configuration(resource_id='<resource_id>',
                                                   ssh_port=22,
                                                   username='<username>',
                                                   password="<password>")

   # Attach the compute
   compute = ComputeTarget.attach(ws, compute_target_name, attach_config)

   compute.wait_for_completion(show_output=True)
   ```

   Sie können die DSVM auch [über Azure Machine Learning Studio](how-to-create-attach-compute-studio.md#attached-compute) an Ihren Arbeitsbereich anfügen.

1. **Konfigurieren**: Erstellen Sie eine Ausführungskonfiguration für das DSVM-Computeziel. Zum Erstellen und Konfigurieren der Trainingsumgebung in der DSVM werden Docker und Conda verwendet.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/dsvm.py?name=run_dsvm)]


Nachdem Sie nun die Computeressource angefügt und Ihre Ausführung konfiguriert haben, besteht der nächste Schritt im [Übermitteln der Trainingsausführung](how-to-set-up-training-targets.md).

## <a name="azure-hdinsight"></a><a id="hdinsight"></a>Azure HDInsight 

Azure HDInsight ist eine beliebte Plattform für Big Data-Analysen. Die Plattform stellt Apache Spark bereit, das zum Training Ihres Modells verwendet werden kann.

1. **Erstellen**:  Erstellen Sie den HDInsight-Cluster, bevor Sie ihn zum Training Ihres Modells verwenden. Informationen zum Erstellen von Spark in einem HDInsight-Cluster finden Sie unter [Erstellen eines Spark-Clusters in HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql). 

    > [!WARNING]
    > Von Azure Machine Learning wird vorausgesetzt, dass der HDInsight-Cluster über eine __öffentliche IP-Adresse__ verfügt.

    Beim Erstellen des Clusters müssen Sie einen SSH-Benutzernamen und ein Kennwort angeben. Notieren Sie diese Werte, da Sie sie benötigen, wenn Sie HDInsight als Computeziel verwenden.
    
    Stellen Sie nach der Erstellung des Clusters eine Verbindung mit dem Hostnamen „\<clustername>-ssh.azurehdinsight.net“ her, wobei \<clustername> der Name ist, den Sie für den Cluster angegeben haben. 

1. **Anfügen**: Wenn Sie einen HDInsight-Cluster als Computeziel anfügen möchten, müssen Sie den die Ressourcen-ID, den Benutzernamen und das Kennwort für den HDInsight-Cluster angeben. Die Ressourcen-ID des HDInsight-Clusters kann unter Verwendung der Abonnement-ID, des Ressourcengruppennamens und des Namens des HDInsight-Clusters im folgenden Zeichenfolgenformat erstellt werden: `/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.HDInsight/clusters/<cluster_name>`.

    ```python
   from azureml.core.compute import ComputeTarget, HDInsightCompute
   from azureml.exceptions import ComputeTargetException

   try:
    # if you want to connect using SSH key instead of username/password you can provide parameters private_key_file and private_key_passphrase

    attach_config = HDInsightCompute.attach_configuration(resource_id='<resource_id>',
                                                          ssh_port=22, 
                                                          username='<ssh-username>', 
                                                          password='<ssh-pwd>')
    hdi_compute = ComputeTarget.attach(workspace=ws, 
                                       name='myhdi', 
                                       attach_configuration=attach_config)

   except ComputeTargetException as e:
    print("Caught = {}".format(e.message))

   hdi_compute.wait_for_completion(show_output=True)
   ```

   Sie können den HDInsight-Cluster auch [über Azure Machine Learning Studio](how-to-create-attach-compute-studio.md#attached-compute) an Ihren Arbeitsbereich anfügen.

1. **Konfigurieren**: Erstellen Sie eine Ausführungskonfiguration für das HDI-Computeziel. 

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/hdi.py?name=run_hdi)]


Nachdem Sie nun die Computeressource angefügt und Ihre Ausführung konfiguriert haben, besteht der nächste Schritt im [Übermitteln der Trainingsausführung](how-to-set-up-training-targets.md).

## <a name="azure-batch"></a><a id="azbatch"></a>Azure Batch 

Azure Batch wird verwendet, um umfangreiche, auf Parallelverarbeitung ausgelegte HPC-Anwendungen effizient in der Cloud auszuführen. AzureBatchStep kann in einer Azure Machine Learning-Pipeline verwendet werden, um Aufträge an einen Azure Batch Pool von Computern zu übermitteln.

Um Azure Batch als Computeziel anzufügen, müssen Sie das Azure Machine Learning SDK verwenden und die folgenden Informationen angeben:

-    **Azure Batch-Computename**: Ein Computeanzeigename, der innerhalb des Arbeitsbereichs verwendet wird.
-    **Azure Batch-Kontoname**: Der Name des Azure Batch-Kontos.
-    **Ressourcengruppe**: Die Ressourcengruppe, die das Azure Batch-Konto enthält.

Der folgende Code veranschaulicht, wie Azure Batch als Computeziel angefügt wird:

```python
from azureml.core.compute import ComputeTarget, BatchCompute
from azureml.exceptions import ComputeTargetException

# Name to associate with new compute in workspace
batch_compute_name = 'mybatchcompute'

# Batch account details needed to attach as compute to workspace
batch_account_name = "<batch_account_name>"  # Name of the Batch account
# Name of the resource group which contains this account
batch_resource_group = "<batch_resource_group>"

try:
    # check if the compute is already attached
    batch_compute = BatchCompute(ws, batch_compute_name)
except ComputeTargetException:
    print('Attaching Batch compute...')
    provisioning_config = BatchCompute.attach_configuration(
        resource_group=batch_resource_group, account_name=batch_account_name)
    batch_compute = ComputeTarget.attach(
        ws, batch_compute_name, provisioning_config)
    batch_compute.wait_for_completion()
    print("Provisioning state:{}".format(batch_compute.provisioning_state))
    print("Provisioning errors:{}".format(batch_compute.provisioning_errors))

print("Using Batch compute:{}".format(batch_compute.cluster_resource_id))
```

### <a name="azure-databricks"></a><a id="databricks"></a>Azure Databricks

Azure Databricks ist eine Apache Spark-basierte Umgebung in der Azure-Cloud. Sie kann mit einer Azure Machine Learning-Pipeline als Computeziel verwendet werden.

Erstellen Sie vor der Verwendung einen Azure Databricks-Arbeitsbereich. Informationen zum Erstellen einer Arbeitsbereichsressource finden Sie im Dokument [Ausführen eines Spark-Auftrags in Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

Geben Sie zum Anfügen von Azure Databricks als Computeziel die folgenden Informationen an:

* __Databricks-Computename__: Der Name, der dieser Computeressource zugewiesen werden soll.
* __Name des Databricks-Arbeitsbereichs__: Der Name des Azure Databricks-Arbeitsbereichs.
* __Databricks-Zugriffstoken__: Das zur Authentifizierung bei Azure Databricks verwendete Zugriffstoken. Informationen zum Generieren eines Zugriffstokens finden Sie im Dokument [Authentifizierung](https://docs.azuredatabricks.net/dev-tools/api/latest/authentication.html).

Der folgende Code veranschaulicht, wie Sie Azure Databricks als Computeziel mit dem Azure Machine Learning SDK anfügen (__der Databricks-Arbeitsbereich muss im gleichen Abonnement wie der AML-Arbeitsbereich vorhanden sein__):

```python
import os
from azureml.core.compute import ComputeTarget, DatabricksCompute
from azureml.exceptions import ComputeTargetException

databricks_compute_name = os.environ.get(
    "AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_workspace_name = os.environ.get(
    "AML_DATABRICKS_WORKSPACE", "<databricks_workspace_name>")
databricks_resource_group = os.environ.get(
    "AML_DATABRICKS_RESOURCE_GROUP", "<databricks_resource_group>")
databricks_access_token = os.environ.get(
    "AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(
        workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_workspace_name {}'.format(databricks_workspace_name))
    print('databricks_access_token {}'.format(databricks_access_token))

    # Create attach config
    attach_config = DatabricksCompute.attach_configuration(resource_group=databricks_resource_group,
                                                           workspace_name=databricks_workspace_name,
                                                           access_token=databricks_access_token)
    databricks_compute = ComputeTarget.attach(
        ws,
        databricks_compute_name,
        attach_config
    )

    databricks_compute.wait_for_completion(True)
```

Ein ausführlicheres Beispiel finden Sie in einem [Beispiel-Notebook](https://aka.ms/pl-databricks) auf GitHub.

### <a name="azure-data-lake-analytics"></a><a id="adla"></a>Azure Data Lake Analytics

Azure Data Lake Analytics ist eine umfangreiche Datenanalyseplattform in der Azure-Cloud. Sie kann mit einer Azure Machine Learning-Pipeline als Computeziel verwendet werden.

Erstellen Sie vor der Verwendung ein Azure Data Lake Analytics-Konto. Informationen zum Erstellen dieser Ressource finden Sie im Dokument [Erste Schritte mit Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal).

Um Data Lake Analytics als Computeziel anzufügen, müssen Sie das Azure Machine Learning SDK verwenden und die folgenden Informationen angeben:

* __Computename__: Der Name, der dieser Computeressource zugewiesen werden soll.
* __Ressourcengruppe__: Die Ressourcengruppe, die das Data Lake Analytics-Konto enthält.
* __Kontoname__: Der Name des Data Lake Analytics-Kontos.

Der folgende Code veranschaulicht, wie Data Lake Analytics als Computeziel angefügt wird:

```python
import os
from azureml.core.compute import ComputeTarget, AdlaCompute
from azureml.exceptions import ComputeTargetException


adla_compute_name = os.environ.get(
    "AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_group = os.environ.get(
    "AML_ADLA_RESOURCE_GROUP", "<adla_resource_group>")
adla_account_name = os.environ.get(
    "AML_ADLA_ACCOUNT_NAME", "<adla_account_name>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_group))
    print('adla_account_name {}'.format(adla_account_name))
    # create attach config
    attach_config = AdlaCompute.attach_configuration(resource_group=adla_resource_group,
                                                     account_name=adla_account_name)
    # Attach ADLA
    adla_compute = ComputeTarget.attach(
        ws,
        adla_compute_name,
        attach_config
    )

    adla_compute.wait_for_completion(True)
```

Ein ausführlicheres Beispiel finden Sie in einem [Beispiel-Notebook](https://aka.ms/pl-adla) auf GitHub.

> [!TIP]
> Azure Machine Learning-Pipelines können nur ausgeführt werden, wenn die Daten im Standarddatenspeicher des Data Lake Analytics-Kontos gespeichert werden. Wenn die Daten, die Sie verwenden möchten, in einem nicht standardmäßigen Speicher gespeichert sind, können Sie die Daten mithilfe von [`DataTransferStep`](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) vor dem Trainieren kopieren.

## <a name="notebook-examples"></a>Notebook-Beispiele

Beispiele für das Training mit verschiedenen Computeziele finden Sie in diesen Notebooks:
* [how-to-use-azureml/training](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [tutorials/img-classification-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/image-classification-mnist-data/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

* Verwenden Sie die Computeressource zum [Übermitteln einer Trainingsausführung](how-to-set-up-training-targets.md).
* [Tutorial: Trainieren eines Modells](tutorial-train-models-with-aml.md) verwendet ein verwaltetes Computeziel zum Trainieren eines Modells.
* Erfahren Sie, wie [Hyperparameter optimiert werden](how-to-tune-hyperparameters.md), um bessere Modelle zu erstellen.
* Erfahren Sie nach der Erstellung eines trainierten Modells, [wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md).
* [Verwenden von Azure Machine Learning mit virtuellen Azure-Netzwerken](how-to-enable-virtual-network.md)
