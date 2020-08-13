---
title: Verwenden von Computeziele für das Modelltraining
titleSuffix: Azure Machine Learning
description: Konfigurieren Sie die Trainingsumgebungen (Computeziele) für das Machine Learning-Modelltraining. Sie können problemlos zwischen Trainingsumgebungen wechseln. Beginnen Sie das Training lokal. Wenn ein Aufskalieren erforderlich ist, wechseln Sie zu einem cloudbasierten Computeziel.
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.date: 07/08/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: 093f86f0373791326cf4658bdab7179fbca1307d
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87849657"
---
# <a name="set-up-and-use-compute-targets-for-model-training"></a>Einrichten und Verwenden von Computezielen für das Modelltraining 
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Mit Azure Machine Learning können Sie Ihr Modell für eine Vielzahl von Ressourcen oder Umgebungen trainieren, die zusammen als [__Computeziele__](concept-azure-machine-learning-architecture.md#compute-targets) bezeichnet werden. Ein Computeziel kann ein lokaler Computer oder eine Cloudressource sein, wie beispielsweise Azure Machine Learning Compute, Azure HDInsight oder ein virtueller Remotecomputer.  Sie können auch Computeziele für die Modellimplementierung erstellen, wie in [Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst](how-to-deploy-and-where.md) beschrieben.

Sie können Computeziele mit dem Azure Machine Learning SDK, in Azure Machine Learning Studio, über die Azure-Befehlszeilenschnittstelle oder über die Azure Machine Learning-VS Code-Erweiterung erstellen und verwalten. Wenn Sie Computeziele haben, die über einen anderen Dienst (z. B. einen HDInsight-Cluster) erstellt wurden, können Sie diese an Ihren Azure Machine Learning-Arbeitsbereich anfügen, um sie verwenden zu können.
 
In diesem Artikel erfahren Sie, wie Sie verschiedene Computeziele für das Modelltraining verwenden.  Die Schritte für alle Computeziele führen Sie den gleichen Workflow:
1. __Erstellen__ Sie ein Computeziel, wenn noch keines vorhanden ist.
2. __Fügen__ Sie das Computeziel zu Ihrem Arbeitsbereich hinzu.
3. __Konfigurieren__ Sie das Computeziel, sodass es die von Ihren Skript benötigte Python-Umgebung und Paketabhängigkeiten enthält.


>[!NOTE]
> Der Code in diesem Artikel wurde mit Version 1.0.74 des Azure Machine Learning SDK getestet.

## <a name="compute-targets-for-training"></a>Computeziele für das Training

Azure Machine Learning bietet unterschiedliche Unterstützung für verschiedene Computeziele. Ein typischer Modellentwicklungslebenszyklus beginnt mit der Entwicklung/Experimenten mit einer kleinen Menge von Daten. In dieser Phase empfehlen wir die Verwendung einer lokalen Umgebung. Verwenden Sie beispielsweise Ihren lokalen Computer oder eine cloudbasierte VM. Wenn Sie Ihr Training zur Verwendung größerer Datasets hochskalieren oder sich für ein verteiltes Training entscheiden, empfehlen wir, mit Azure Machine Learning Compute einen Cluster mit einem einzelnen oder mehreren Knoten zu erstellen, der bei jeder Übermittlung einer Ausführung automatisch skaliert wird. Sie können auch Ihre eigene Computeressource anfügen. Die Unterstützung für verschiedene Szenarien variiert jedoch wie unten beschrieben:

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]


> [!NOTE]
> Azure Machine Learning Compute-Cluster können als dauerhafte Ressourcen oder dynamisch beim Anfordern einer Ausführung erstellt werden. Bei der ausführungsbasierten Erstellung wird das Computeziel nach Abschluss des Trainingsauftrags entfernt, sodass Sie auf diese Weise erstellte Computeziele nicht wiederverwenden können.

## <a name="whats-a-run-configuration"></a>Was ist eine Laufzeitkonfiguration?

Beim Training ist es üblich, auf dem lokalen Computer zu starten und dieses Trainingsskript später auf einem anderen Computeziel auszuführen. Mit Azure Machine Learning können Sie Ihr Skript auf unterschiedlichen Computezielen ausführen, ohne das Skript zu ändern.

Sie müssen lediglich die Umgebung für jedes einzelne Computeziel in einer **Laufzeitkonfiguration** definieren.  Wenn Sie Ihr Trainingsexperiment auf einem anderen Computeziel ausführen möchten, geben Sie die Laufzeitkonfiguration für diese Computeressource an. Weitere Informationen zum Angeben einer Umgebung und zum Binden an die Laufzeitkonfiguration finden Sie unter [Erstellen und Verwalten von Umgebungen für Training und Bereitstellung](how-to-use-environments.md).

Mehr über das [Übermitteln von Experimenten](#submit) finden Sie am Ende dieses Artikels.

## <a name="whats-an-estimator"></a>Was ist ein Kalkulator?

Um das Modelltraining mit beliebten Frameworks zu erleichtern, bietet das Python SDK für Azure Machine Learning eine alternative allgemeine Abstraktion, die Kalkulatoren-Klasse.  Diese Klasse erlaubt es Ihnen, Laufzeitkonfigurationen mühelos zu erstellen. Sie können einen generischen [Kalkulator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) erstellen und verwenden, um Trainingsskripts zu übermitteln, die ein beliebiges, von Ihnen ausgewähltes Learning-Framework verwenden (z. B. scikit-learn). Es wird empfohlen, einen Schätzwert für das Training zu verwenden, da es eingebettete Objekte wie Umgebungs- oder RunConfiguration-Objekte automatisch für Sie erstellt. Wenn Sie mehr Kontrolle darüber haben möchten, wie diese Objekte erstellt werden, und angeben möchten, welche Pakete für Ihre Experimentausführung installiert werden sollen, befolgen Sie [diese Schritte](#amlcompute), um Ihre Trainingsexperimente mithilfe eines RunConfiguration-Objekts an ein Azure Machine Learning Compute zu übermitteln.

Azure Machine Learning bietet spezifische Schätzer für [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py), [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) und [Ray RLlib](how-to-use-reinforcement-learning.md).

Weitere Informationen finden Sie unter [Trainieren von ML-Modellen mit Kalkulatoren](how-to-train-ml-models.md).

## <a name="whats-an-ml-pipeline"></a>Was ist eine ML-Pipeline?

Mit ML-Pipelines können Sie Ihren Workflow durch Einfachheit, Geschwindigkeit, Portabilität und Wiederverwendung optimieren. Beim Erstellen von Pipelines mit Azure Machine Learning können Sie sich auf Ihre Fachkenntnisse (Machine Learning) und nicht auf die Infrastruktur und Automatisierung konzentrieren.

ML-Pipelines werden in mehreren **Schritten** erstellt, bei denen es sich um unterschiedliche Recheneinheiten in der Pipeline handelt. Jeder Schritt kann unabhängig ausgeführt werden und isolierte Computeressourcen verwenden. Durch diesen Ansatz können mehrere Datenanalysten gleichzeitig an derselben Pipeline arbeiten, ohne dass die Computeressourcen überlastet werden. Außerdem ist es so einfacher, für jeden Schritt verschiedene Computetypen/-größen zu verwenden.

> [!TIP]
> ML-Pipelines können beim Trainieren von Modellen Laufzeitkonfiguration oder Kalkulatoren verwenden.

ML-Pipelines können nicht nur Modelle trainieren, sondern auch Daten vor dem Training aufbereiten und Modelle nach dem Training bereitstellen. Einer der primären Anwendungsfälle für Pipelines ist die Batchbewertung. Weitere Informationen finden Sie im Artikel [Pipelines: Optimieren von Machine Learning-Workflows](concept-ml-pipelines.md).

## <a name="set-up-in-python"></a>Einrichtung in Python

Konfigurieren Sie die Computeziele anhand der folgenden Abschnitte:

* [Lokaler Computer](#local)
* [Azure Machine Learning Compute-Cluster](#amlcompute)
* [Azure Machine Learning-Computeinstanz](#instance)
* [Virtuelle Remotecomputer](#vm)
* [Azure HDInsight](#hdinsight)


### <a name="local-computer"></a><a id="local"></a>Lokaler Computer

1. **Erstellen und Anfügen**: Es ist nicht erforderlich, ein Computeziel zu erstellen oder anzufügen, um Ihren lokalen Computer als Trainingsumgebung zu verwenden.  

1. **Konfigurieren**:  Wenn Sie Ihren lokalen Computer als Computeziel verwenden, wird der Trainingscode in Ihrer [Entwicklungsumgebung](how-to-configure-environment.md) ausgeführt.  Wenn diese Umgebung bereits über die von Ihnen benötigten Python-Pakete verfügt, verwenden Sie die vom Benutzer verwaltete Umgebung.

 [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=run_local)]

Nachdem Sie nun die Computeressource angefügt und Ihre Ausführung konfiguriert haben, besteht der nächste Schritt im [Übermitteln der Trainingsausführung](#submit).

### <a name="azure-machine-learning-compute-cluster"></a><a id="amlcompute"></a>Azure Machine Learning-Computecluster

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

    Sie können auch eine persistente Azure Machine Learning Compute-Ressource [ in Azure Machine Learning Studio](#portal-create) erstellen und anfügen.

   
1. **Konfigurieren**: Erstellen Sie eine Ausführungskonfiguration für das persistente Computeziel.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=run_amlcompute)]

Nachdem Sie nun die Computeressource angefügt und Ihre Ausführung konfiguriert haben, besteht der nächste Schritt im [Übermitteln der Trainingsausführung](#submit).

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



### <a name="azure-machine-learning-compute-instance"></a><a id="instance"></a>Azure Machine Learning-Computeinstanz

Eine [Azure Machine Learning-Computeinstanz](concept-compute-instance.md) ist eine verwaltete Computeinfrastruktur, die Ihnen das einfache Erstellen einer einzelnen VM ermöglicht. Das Computeziel wird in Ihrer Arbeitsbereichsregion erstellt, aber im Gegensatz zu einem Computecluster kann eine Instanz nicht für andere Benutzer in Ihrem Arbeitsbereich freigegeben werden. Außerdem wird die Instanz nicht automatisch herunterskaliert.  Sie müssen die Ressource beenden, um laufende Gebühren zu vermeiden.

Computeinstanzen können mehrere Aufträge parallel ausführen und verfügen über eine Auftragswarteschlange. 

Sie können Aufträge sicher in einer [virtuellen Netzwerkumgebung](how-to-enable-virtual-network.md#compute-instance) ausführen, ohne dass Unternehmen hierfür SSH-Ports öffnen müssen. Der Auftrag wird in einer Containerumgebung ausgeführt und packt die Abhängigkeiten Ihres Modells in einen Docker-Container. 

1. **Erstellen und Anfügen**: 
    
    [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-computeinstance/train-on-computeinstance.ipynb?name=create_instance)]

1. **Konfigurieren**: Erstellen Sie eine Laufzeitkonfiguration.
    
    ```python
    
    from azureml.core import ScriptRunConfig
    from azureml.core.runconfig import DEFAULT_CPU_IMAGE
    
    src = ScriptRunConfig(source_directory='', script='train.py')
    
    # Set compute target to the one created in previous step
    src.run_config.target = instance
    
    # Set environment
    src.run_config.environment = myenv
     
    run = experiment.submit(config=src)
    ```

Weitere nützliche Befehle für die Computeinstanz finden Sie im Notebook [train-on-computeinstance](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-computeinstance/train-on-computeinstance.ipynb). Dieses Notebook befindet sich auch im Studio-Ordner **Beispiele** in *training/train-on-computeinstance*.

Nachdem Sie nun das Computeziel angefügt und die Ausführung konfiguriert haben, besteht der nächste Schritt im [Übermitteln der Trainingsausführung](#submit).


### <a name="remote-virtual-machines"></a><a id="vm"></a>Virtuelle Remotecomputer

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

   Sie können die DSVM auch [über Azure Machine Learning Studio](#portal-reuse) an Ihren Arbeitsbereich anfügen.

1. **Konfigurieren**: Erstellen Sie eine Ausführungskonfiguration für das DSVM-Computeziel. Zum Erstellen und Konfigurieren der Trainingsumgebung in der DSVM werden Docker und Conda verwendet.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/dsvm.py?name=run_dsvm)]


Nachdem Sie nun die Computeressource angefügt und Ihre Ausführung konfiguriert haben, besteht der nächste Schritt im [Übermitteln der Trainingsausführung](#submit).

### <a name="azure-hdinsight"></a><a id="hdinsight"></a>Azure HDInsight 

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

   Sie können den HDInsight-Cluster auch [über Azure Machine Learning Studio](#portal-reuse) an Ihren Arbeitsbereich anfügen.

1. **Konfigurieren**: Erstellen Sie eine Ausführungskonfiguration für das HDI-Computeziel. 

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/hdi.py?name=run_hdi)]


Nachdem Sie nun die Computeressource angefügt und Ihre Ausführung konfiguriert haben, besteht der nächste Schritt im [Übermitteln der Trainingsausführung](#submit).


### <a name="azure-batch"></a><a id="azbatch"></a>Azure Batch 

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

## <a name="set-up-in-azure-machine-learning-studio"></a>Einrichten in Azure Machine Learning Studio

Sie können in Azure Machine Learning Studio auf Computeziele zugreifen, die Ihrem Arbeitsbereich zugeordnet sind.  Sie können Studio für Folgendes verwenden:

* [Anzeigen von Computezielen](#portal-view), die an Ihren Arbeitsbereich angefügt sind
* [Erstellen eines Computeziels](#portal-create) in Ihrem Arbeitsbereich
* [Anfügen eines Computeziels](#portal-reuse), das außerhalb des Arbeitsbereichs erstellt wurde


Nachdem ein Ziel erstellt und an Ihren Arbeitsbereich angefügt wurde, verwenden Sie es in Ihrer Laufzeitkonfiguration mit einem `ComputeTarget`-Objekt: 

```python
from azureml.core.compute import ComputeTarget
myvm = ComputeTarget(workspace=ws, name='my-vm-name')
```

### <a name="view-compute-targets"></a><a id="portal-view"></a>Anzeigen von Computezielen


Um die Compute-Ziele für Ihren Arbeitsbereich anzuzeigen, führen Sie die folgenden Schritte aus:

1. Navigieren Sie zu [Azure Machine Learning Studio](https://ml.azure.com).
 
1. Wählen Sie unter __Anwendungen__ den Eintrag __Compute__.

    [![Anzeigen der Registerkarte „Compute“](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace-expanded.png)

### <a name="create-a-compute-target"></a><a id="portal-create"></a>Erstellen eines Computeziels

Führen Sie die vorherigen Schritte zum Anzeigen der Liste der Computeziele aus. Führen Sie dann die folgenden Schritte aus, um ein Computeziel zu erstellen: 

1. Klicken Sie auf das Pluszeichen (+), um ein Computeziel hinzuzufügen.

    ![Hinzufügen eines Computeziels](./media/how-to-set-up-training-targets/add-compute-target.png) 

1. Geben Sie einen Namen für das Computeziel ein. 

1. Wählen Sie **Machine Learning Compute** als Computetyp für __Training__ aus. 

    >[!NOTE]
    >Azure Machine Learning Compute ist die einzige verwaltete Computeressource, die Sie in Azure Machine Learning Studio erstellen können.  Alle anderen Computeressourcen können erst nach dem Erstellen angefügt werden.

1. Füllen Sie das Formular aus. Geben Sie für die Einrichtung des Computeziels Werte für die Pflichteigenschaften ein, insbesondere die **VM-Familie** und die **maximale Anzahl von Knoten**.  

1. Klicken Sie auf __Erstellen__.


1. Sie können den Status des Erstellungsvorgangs anzeigen, indem Sie das Computeziel in der Liste auswählen:

    ![Auswählen eines Computeziels zum Anzeigen des Status des Erstellungsvorgangs](./media/how-to-set-up-training-targets/View_list.png)

1. Daraufhin werden die Details für das Computeziel angezeigt: 

    ![Anzeigen der Details des Computeziels](./media/how-to-set-up-training-targets/compute-target-details.png) 

### <a name="attach-compute-targets"></a><a id="portal-reuse"></a>Anfügen von Computezielen

Um Computeziele zu verwenden, die außerhalb des Azure Machine Learning-Arbeitsbereichs erstellt wurde, müssen Sie sie anfügen. Durch das Anfügen eines Computeziels wird es Ihrem Arbeitsbereich zur Verfügung gestellt.

Führen Sie die zuvor beschriebenen Schritte aus, um die Liste der Computeziele anzuzeigen. Führen Sie dann die folgenden Schritte aus, um ein Computeziel anzufügen: 

1. Klicken Sie auf das Pluszeichen (+), um ein Computeziel hinzuzufügen. 
1. Geben Sie einen Namen für das Computeziel ein. 
1. Wählen Sie den Computetyp aus, den Sie für __Training__ anfügen möchten:

    > [!IMPORTANT]
    > Nicht alle Computetypen können in Azure Machine Learning Studio angefügt werden. Derzeit können folgende Computetypen für das Training angefügt werden:
    >
    > * Eine Remote-VM
    > * Azure Databricks (zur Verwendung in Machine Learning-Pipelines)
    > * Azure Data Lake Analytics (zur Verwendung in Machine Learning-Pipelines)
    > * Azure HDInsight

1. Füllen Sie das Formular aus, und geben Sie Werte für die Pflichteigenschaften an.

    > [!NOTE]
    > Microsoft empfiehlt die Verwendung von SSH-Schlüsseln, die sicherer als Kennwörter sind. Kennwörter sind anfällig für Brute-Force-Angriffe. SSH-Schlüssel basieren auf kryptografische Signaturen. Informationen zum Erstellen von SSH-Schlüsseln für die Verwendung mit Azure Virtual Machines finden Sie in den folgenden Dokumenten:
    >
    > * [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Verwenden von SSH-Schlüsseln mit Windows in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Wählen Sie __Anfügen__ aus. 
1. Sie können den Status des Anfügevorgangs anzeigen, indem Sie das Computeziel in der Liste auswählen.

## <a name="set-up-with-cli"></a>Einrichtung mit der CLI

Sie können auf die mit Ihrem Arbeitsbereich verknüpften Computeziele mit der [CLI-Erweiterung](reference-azure-machine-learning-cli.md) für Azure Machine Learning zugreifen.  Mit der CLI haben Sie folgende Möglichkeiten:

* Erstellen eines verwalteten Computeziels
* Aktualisieren eines verwalteten Computeziels
* Anfügen eines nicht verwalteten Computeziels

Weitere Informationen finden Sie unter [Ressourcenverwaltung](reference-azure-machine-learning-cli.md#resource-management).

## <a name="set-up-with-vs-code"></a>Einrichtung mit VS Code

Sie können auf die mit Ihrem Arbeitsbereich verknüpften Computeziele mit der [VS Code-Erweiterung](how-to-manage-resources-vscode.md#compute-clusters) für Azure Machine Learning zugreifen, sie erstellen und verwalten.

## <a name="submit-training-run-using-azure-machine-learning-sdk"></a><a id="submit"></a>Übermitteln der Trainingsausführung mit dem Azure Machine Learning SDK

Nachdem Sie eine Laufzeitkonfiguration erstellt haben, verwenden Sie sie zum Ausführen Ihres Experiments.  Das Codemuster für die Übermittlung einer Trainingsausführung ist für alle Arten von Computezielen gleich:

1. Erstellen eines Experiments zum Ausführen
1. Übermitteln Sie die Ausführung.
1. Warten Sie, bis die Ausführung abgeschlossen ist.

> [!IMPORTANT]
> Wenn Sie eine Trainingsausführung übermitteln, wird eine Momentaufnahme des Verzeichnisses, das Ihre Trainingsskripts enthält, erstellt und an das Computeziel gesendet. Sie wird auch als Teil des Experiments in Ihrem Arbeitsbereich gespeichert. Wenn Sie Dateien ändern und die Ausführung erneut übermitteln, werden nur die geänderten Dateien hochgeladen.
>
> [!INCLUDE [amlinclude-info](../../includes/machine-learning-amlignore-gitignore.md)]
> 
> Weitere Informationen finden Sie unter [Momentaufnahmen](concept-azure-machine-learning-architecture.md#snapshots).

### <a name="create-an-experiment"></a>Erstellen eines Experiments

Erstellen Sie zunächst ein Experiment in Ihrem Arbeitsbereich.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=experiment)]

### <a name="submit-the-experiment"></a>Übermitteln des Experiments

Übermitteln Sie das Experiment mit einem `ScriptRunConfig`-Objekt.  Dieses Objekt enthält Folgendes:

* **source_directory**: Das Quellverzeichnis mit Ihrem Trainingsskript
* **script**: Bestimmen des Trainingsskripts
* **run_config**: Die Laufzeitkonfiguration, die wiederum definiert, wo das Training erfolgt.

Verwenden Sie beispielsweise die Konfiguration [lokales Ziel](#local):

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=local_submit)]

Wechseln Sie mit demselben Experiment zur Ausführung zu einem anderen Computeziel, indem Sie eine andere Laufzeitkonfiguration verwenden, z.B. [amlcompute-Ziel](#amlcompute):

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=amlcompute_submit)]

> [!TIP]
> In diesem Beispiel wird standardmäßig nur ein Knoten des Computeziels für das Training verwendet. Wenn Sie mehrere Knoten verwenden möchten, legen Sie die `node_count`-Eigenschaft der run-Konfiguration auf die gewünschte Anzahl von Knoten fest. Im folgende Code wird die Anzahl von Knoten, die für das Training verwendet werden, beispielsweise auf vier festgelegt:
>
> ```python
> src.run_config.node_count = 4
> ```

Alternative:

* Übermitteln Sie das Experiments mit einem `Estimator`-Objekt, wie in [Trainieren von ML-Modellen mit Kalkulatoren](how-to-train-ml-models.md) erläutert.
* Übermitteln Sie eine HyperDrive-Ausführung für die [Hyperparameteroptimierung](how-to-tune-hyperparameters.md).
* Übermitteln Sie ein Experiment über die [VS Code-Erweiterung](tutorial-train-deploy-image-classification-model-vscode.md#train-the-model).

Weitere Informationen finden Sie in der Dokumentation zu [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) und [RunConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py).

## <a name="create-run-configuration-and-submit-run-using-azure-machine-learning-cli"></a>Erstellen einer Laufzeitkonfiguration und Übermitteln der Ausführung mit der Azure Machine Learning CLI

Sie können die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) und die [Machine Learning CLI-Erweiterung](reference-azure-machine-learning-cli.md) zum Erstellen von Laufzeitkonfigurationen und Übermitteln von Ausführungen für verschiedene Computeziele verwenden. Die folgenden Beispiele gehen davon aus, dass Sie über einen vorhandenen Azure Machine Learning-Arbeitsbereich verfügen und sich mit dem CLI-Befehl `az login` bei Azure angemeldet haben. 

[!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 

### <a name="create-run-configuration"></a>Erstellen einer Laufzeitkonfiguration

Der einfachste Weg, eine Laufzeitkonfiguration zu erstellen, besteht darin, in den Ordner zu navigieren, der Ihre Machine Learning Python-Skripts enthält, und den CLI-Befehl zu verwenden.

```azurecli
az ml folder attach
```

Dieser Befehl erstellt einen Unterordner `.azureml`, der Vorlagen-Laufzeitkonfigurationsdateien für verschiedene Computeziele enthält. Sie können diese Dateien kopieren und bearbeiten, um Ihre Konfiguration anzupassen, z.B. um Python-Pakete hinzuzufügen oder Docker-Einstellungen zu ändern.  

### <a name="structure-of-run-configuration-file"></a>Struktur der Laufzeitkonfigurationsdatei

Die Laufzeitkonfigurationsdatei ist eine als YAML formatierte Datei mit den folgenden Abschnitten
 * Das auszuführende Skript und seine Argumente
 * Der Computezielname (entweder „local“ oder der Name eines Computeziels unter dem Arbeitsbereich).
 * Parameter für die Ausführung des Laufs: Framework, Communicator für verteilte Ausführungen, maximale Dauer und Anzahl der Computeknoten.
 * Umgebungsabschnitt. Einzelheiten zu den Feldern in diesem Abschnitt finden Sie unter [Erstellen und Verwalten von Umgebungen für Training und Bereitstellung](how-to-use-environments.md).
   * Um Python-Pakete für die Installation für die Ausführung anzugeben, erstellen Sie die [Conda-Umgebungsdatei ](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#create-env-file-manually), und legen Sie das Feld __condaDependenciesFile__ fest.
 * Führen Sie Verlaufsdetails aus, um den Ordner der Protokolldatei anzugeben und die Ausgabesammlung zu aktivieren oder zu deaktivieren sowie Verlaufsmomentaufnahmen auszuführen.
 * Konfigurationsdetails, die spezifisch für das ausgewählte Framework sind.
 * Datenverweis- und Datenspeicherdetails.
 * Konfigurationsdetails, die spezifisch für Machine Learning Compute für das Erstellen eines neuen Clusters sind.

Ein vollständiges runconfig-Schema finden Sie in der [JSON-Beispieldatei](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json).

### <a name="create-an-experiment"></a>Erstellen eines Experiments

Erstellen Sie zunächst ein Experiment für Ihre Ausführungen.

```azurecli
az ml experiment create -n <experiment>
```

### <a name="script-run"></a>Skriptausführung

Um eine Skriptausführung zu übermitteln, führen Sie einen Befehl aus.

```azurecli
az ml run submit-script -e <experiment> -c <runconfig> my_train.py
```

### <a name="hyperdrive-run"></a>HyperDrive-Ausführung

Sie können HyperDrive mit der Azure CLI verwenden, um Parameteroptimierungsausführungen auszuführen. Erstellen Sie zunächst eine HyperDrive-Konfigurationsdatei im folgenden Format. Weitere Informationen zu den Hyperparameter-Optimierungsparametern finden Sie im Artikel [Optimieren von Hyperparametern für Ihr Modell](how-to-tune-hyperparameters.md).

```yml
# hdconfig.yml
sampling: 
    type: random # Supported options: Random, Grid, Bayesian
    parameter_space: # specify a name|expression|values tuple for each parameter.
    - name: --penalty # The name of a script parameter to generate values for.
      expression: choice # supported options: choice, randint, uniform, quniform, loguniform, qloguniform, normal, qnormal, lognormal, qlognormal
      values: [0.5, 1, 1.5] # The list of values, the number of values is dependent on the expression specified.
policy: 
    type: BanditPolicy # Supported options: BanditPolicy, MedianStoppingPolicy, TruncationSelectionPolicy, NoTerminationPolicy
    evaluation_interval: 1 # Policy properties are policy specific. See the above link for policy specific parameter details.
    slack_factor: 0.2
primary_metric_name: Accuracy # The metric used when evaluating the policy
primary_metric_goal: Maximize # Maximize|Minimize
max_total_runs: 8 # The maximum number of runs to generate
max_concurrent_runs: 2 # The number of runs that can run concurrently.
max_duration_minutes: 100 # The maximum length of time to run the experiment before cancelling.
```

Fügen Sie diese Datei den Laufzeitkonfigurationsdateien hinzu. Übermitteln Sie dann eine HyperDrive-Ausführung:
```azurecli
az ml run submit-hyperdrive -e <experiment> -c <runconfig> --hyperdrive-configuration-name <hdconfig> my_train.py
```

Beachten Sie den Abschnitt *arguments* in der Laufzeitkonfiguration and *parameter space* in der HyperDrive-Konfiguration. Diese Abschnitte enthalten die Befehlszeilenargumente, die an das Trainingsskript übergeben werden sollen. Der Wert in der Laufzeitkonfiguration bleibt für jede Iteration gleich, während der Bereich in der HyperDrive-Konfiguration iteriert wird. Geben Sie nicht das gleiche Argument in beiden Dateien an.

Weitere Einzelheiten zu diesen ```az ml```-CLI-Befehlen finden Sie in der [Referenzdokumentation](reference-azure-machine-learning-cli.md).

<a id="gitintegration"></a>

## <a name="git-tracking-and-integration"></a>Git-Nachverfolgung und -Integration

Wenn Sie eine Trainingsausführung starten, bei der das Quellverzeichnis ein lokales Git-Repository ist, werden Informationen über das Repository im Ausführungsverlauf gespeichert. Weitere Informationen finden Sie unter [Git-Integration für Azure Machine Learning](concept-train-model-git-integration.md).

## <a name="notebook-examples"></a>Notebook-Beispiele

Beispiele für das Training mit verschiedenen Computeziele finden Sie in diesen Notebooks:
* [how-to-use-azureml/training](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [tutorials/img-classification-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/image-classification-mnist-data/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Trainieren eines Modells](tutorial-train-models-with-aml.md) verwendet ein verwaltetes Computeziel zum Trainieren eines Modells.
* Erfahren Sie, wie [Hyperparameter optimiert werden](how-to-tune-hyperparameters.md), um bessere Modelle zu erstellen.
* Erfahren Sie nach der Erstellung eines trainierten Modells, [wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md).
* Zeigen Sie die SDK-Referenz der [RunConfiguration-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) an.
* [Verwenden von Azure Machine Learning mit virtuellen Azure-Netzwerken](how-to-enable-virtual-network.md)
