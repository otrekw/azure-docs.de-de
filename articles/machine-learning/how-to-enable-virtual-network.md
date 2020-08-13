---
title: Netzwerkisolation und Datenschutz
titleSuffix: Azure Machine Learning
description: Verwenden Sie ein virtuelles Azure-Netzwerk mit Azure Machine Learning zum Absichern von Experiment-/Trainings- und Rückschluss-/Bewertungsaufträgen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 07/07/2020
ms.topic: conceptual
ms.custom: how-to, contperfq4, tracking-python
ms.openlocfilehash: 16065b45a6afea25615b985d3c89445dee48bd1d
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88167724"
---
# <a name="network-isolation-during-training--inference-with-private-virtual-networks"></a>Netzwerkisolation während Training und Rückschluss mit privaten virtuellen Netzwerken
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie Ihre Lebenszyklen beim maschinellen Lernen durch Isolieren von Trainings- und Rückschlussaufträgen in Azure Machine Learning innerhalb eines virtuellen Azure-Netzwerks (VNET) sichern können. Azure Machine Learning nutzt andere Azure-Dienste für Computeressourcen, auch als [Computeziele](concept-compute-target.md) bezeichnet, um Modelle zu trainieren und bereitzustellen. Die Ziele können innerhalb eines virtuellen Netzwerks erstellt werden. Beispielsweise können Sie Computeressourcen von Azure Machine Learning verwenden, um ein Modell zu trainieren, und das Modell anschließend in Azure Kubernetes Service (AKS) bereitstellen. 

Ein __virtuelles Netzwerk__ fungiert als Sicherheitsgrenze und schirmt Ihre Azure-Ressourcen vom öffentlichen Internet ab. Sie können ein virtuelles Azure-Netzwerk auch in Ihr lokales Netzwerk einbinden. Durch die Einbindung in Netzwerke können Sie auf sichere Weise Ihre Modelle trainieren und auf Ihre bereitgestellten Modelle zugreifen, um Rückschlüsse zu ziehen.

## <a name="prerequisites"></a>Voraussetzungen

+ Ein Azure Machine Learning-[Arbeitsbereich](how-to-manage-workspace.md).

+ Allgemeine Kenntnisse zum [Azure Virtual Network-Dienst](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) und [IP-Netzwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)

+ Ein bereits vorhandenes virtuelles Netzwerk und Subnetz, das mit Ihren Computeressourcen verwendet werden kann

## <a name="private-endpoints"></a>Private Endpunkte

Sie können auch [Azure Private Link aktivieren](how-to-configure-private-link.md), um eine Verbindung mit Ihrem Arbeitsbereich über einen privaten Endpunkt herzustellen. Bei einem privaten Endpunkt handelt es sich um eine Gruppe privater IP-Adressen in Ihrem virtuellen Netzwerk. [Erfahren Sie, wie Sie diesen privaten Endpunkt einrichten.](how-to-configure-private-link.md)



> [!TIP]
> Sie können das virtuelle Netzwerk und Private Link miteinander kombinieren, um die Kommunikation zwischen Ihrem Arbeitsbereich und anderen Azure-Ressourcen zu schützen. Für einige Kombinationen ist jedoch ein Enterprise Edition-Arbeitsbereich erforderlich. Verwenden Sie die folgende Tabelle, um zu verstehen, welche Szenarien die Enterprise Edition erfordern:
>
> | Szenario | Enterprise</br>Edition | Basic</br>Edition |
> | ----- |:-----:|:-----:| 
> | Kein virtuelles Netzwerk oder Private Link | ✔ | ✔ |
> | Arbeitsbereich ohne Private Link Andere Ressourcen (außer Azure Container Registry) in einem virtuellen Netzwerk | ✔ | ✔ |
> | Arbeitsbereich ohne Private Link Andere Ressourcen mit Private Link | ✔ | |
> | Arbeitsbereich mit Private Link Andere Ressourcen (außer Azure Container Registry) in einem virtuellen Netzwerk | ✔ | ✔ |
> | Arbeitsbereich und beliebige andere Ressourcen mit Private Link | ✔ | |
> | Arbeitsbereich mit Private Link Andere Ressourcen ohne Private Link oder virtuelles Netzwerk | ✔ | ✔ |
> | Azure Container Registry in einem virtuellen Netzwerk | ✔ | |
> | Vom Kunden verwaltete Schlüssel für den Arbeitsbereich | ✔ | |
> 

> [!WARNING]
> 
> Die Vorschauversion von Azure Machine Learning-Computeinstanzen wird in einem Arbeitsbereich, in dem Private Link aktiviert ist, nicht unterstützt.

<a id="amlcompute"></a>

## <a name="machine-learning-studio"></a>Machine Learning Studio

Wenn Ihre Daten in einem virtuellen Netzwerk gespeichert sind, müssen Sie einen [Verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md)-Arbeitsbereich verwenden, um dem Studio Zugriff auf Ihre Daten zu gewähren.

> [!IMPORTANT]
> Obwohl Studio überwiegend mit Daten arbeitet, die in einem virtuellen Netzwerk gespeichert sind, gilt dies __nicht__ auch für integrierte Notebooks. Integrierte Notebooks unterstützen die Verwendung von Speicher in einem virtuellen Netzwerk nicht. Stattdessen können Sie Jupyter-Notebooks aus einer Compute-Instanz verwenden. Weitere Informationen finden Sie im Abschnitt [Zugreifen auf Daten auf einem Compute-Instanz-Notebook](#access-data-in-a-compute-instance-notebook).

Wenn Sie dem Studio keinen Zugriff gewähren, erhalten Sie die Fehlermeldung `Error: Unable to profile this dataset. This might be because your data is stored behind a virtual network or your data does not support profile.` und deaktivieren die folgenden Vorgänge:

* Vorschau der Daten im Studio.
* Visualisieren von Daten im Designer.
* Senden eines AutoML-Experiments.
* Starten eines Beschriftungsprojekts.

Das Studio unterstützt das Lesen von Daten aus den folgenden Datenspeichertypen in einem virtuellen Netzwerk:

* Azure Blob
* Azure Data Lake Storage Gen1
* Azure Data Lake Storage Gen2
* Azure SQL-Datenbank

### <a name="add-resources-to-the-virtual-network"></a>Hinzufügen von Ressourcen zum virtuellen Netzwerk 

Fügen Sie Ihren Arbeitsbereich und Ihr Speicherkonto dem gleichen virtuellen Netzwerk hinzu, damit sie aufeinander zugreifen können.

1. Um Ihren Arbeitsbereich mit dem virtuellen Netzwerk zu verbinden, [aktivieren Sie Azure Private Link](how-to-configure-private-link.md). Diese Funktion befindet sich derzeit in der Vorschauphase und ist in den Regionen „USA, Osten“, „USA, Westen 2“ und „USA, Süden-Mitte“ verfügbar.

1. Um Ihr Speicherkonto mit dem virtuellen Netzwerk zu verbinden, [konfigurieren Sie die Einstellungen für Firewalls und virtuelle Netzwerke](#use-a-storage-account-for-your-workspace).

### <a name="configure-a-datastore-to-use-managed-identity"></a>Konfigurieren eines Datenspeichers für die Verwendung der verwalteten Identität

Nachdem Sie den Arbeitsbereich und das Speicherdienstkonto dem virtuellen Netzwerk hinzugefügt haben, müssen Sie die Datenspeicher so konfigurieren, dass der Zugriff auf Ihre Daten mit der verwalteten Identität möglich ist. Mit diesen Schritten wird die verwaltete Identität des Arbeitsbereichs mithilfe der ressourcenbasierten Zugriffssteuerung (Resource-Based Access Control, RBAC) von Azure dem Speicherdienst als __Reader__ hinzugefügt. Mit __Reader__-Zugriff kann der Arbeitsbereich Firewalleinstellungen abrufen und sicherstellen, dass die Daten das virtuelle Netzwerk nicht verlassen.

1. Wählen Sie __Datastores__ (Datenspeicher) im Studio aus.

1. Um einen neuen Datenspeicher zu erstellen, wählen Sie __+ New datastore__ (+ Neuer Datenspeicher) aus. Wenn Sie einen vorhandenen aktualisieren möchten, wählen Sie den Datenspeicher aus, und wählen Sie __Update credentials__ (Anmeldeinformationen aktualisieren).

1. Wählen Sie in den Datenspeichereinstellungen __Yes__ (Ja) für __Allow Azure Machine Learning service to access the storage using workspace managed identity__ (Azure Machine Learning Service den Zugriff auf den Speicher über die verwaltete Identität des Arbeitsbereichs erlauben).

> [!NOTE]
> Diese Änderungen können bis zu 10 Minuten dauern.

### <a name="azure-blob-storage-blob-data-reader"></a>Blobdatenleser von Azure Blob Storage

Für __Azure Blob Storage__ wird die verwaltete Identität des Arbeitsbereichs auch als [Blob Data Reader](../role-based-access-control/built-in-roles.md#storage-blob-data-reader) (Blobdatenleser) hinzugefügt, sodass Daten aus Blobspeicher gelesen werden können.


### <a name="azure-machine-learning-designer-default-datastore"></a>Standarddatenspeicher für Azure Machine Learning-Designer

Der Designer verwendet als Ausgabespeicher standardmäßig das mit Ihrem Arbeitsbereich verknüpfte Speicherkonto. Sie können jedoch einen beliebigen Datenspeicher, auf den Sie Zugriff haben, als Ausgabespeicher angeben. Wenn Sie in Ihrer Umgebung virtuelle Netzwerke verwenden, können Sie mit diesen Steuerelementen sicherstellen, dass Ihre Daten sicher und zugänglich bleiben.

So legen Sie einen neuen Standardspeicher für eine Pipeline fest

1. Wählen Sie in einem Pipeline-Entwurf das **Zahnradsymbol für Einstellungen** neben dem Pipeline-Titel aus.
1. Wählen Sie **Standarddatenspeicher auswählen** aus.
1. Geben Sie einen neuen Datenspeicher an.

Sie können den Standarddatenspeicher auch auf Modulbasis überschreiben. Dadurch können Sie den Speicherort für jedes einzelne Modul steuern.

1. Wählen Sie das Modul aus, für das Sie den Ausgabespeicher angeben möchten.
1. Erweitern Sie den Abschnitt **Ausgabeeinstellungen**.
1. Wählen Sie **Standardausgabeeinstellungen außer Kraft setzen** aus.
1. Wählen Sie **Ausgabeeinstellungen festlegen** aus.
1. Geben Sie einen neuen Datenspeicher an.

### <a name="azure-data-lake-storage-gen2-access-control"></a>Zugriffssteuerung von Azure Data Lake Storage Gen2

Sie können den Datenzugriff innerhalb eines virtuellen Netzwerks mit RBAC- und POSIX-Zugriffssteuerungslisten (Access Control Lists, ACLs) steuern.

Um RBAC zu verwenden, fügen Sie die verwaltete Identität des Arbeitsbereichs der [Blobdatenleser](../role-based-access-control/built-in-roles.md#storage-blob-data-reader)-Rolle hinzu. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung](../storage/blobs/data-lake-storage-access-control.md#role-based-access-control).

Um ACLs zu verwenden, kann der verwalteten Identität des Arbeitsbereichs wie jedem anderen Sicherheitsprinzip Zugriff gewährt werden. Weitere Informationen finden Sie unter [Zugriffssteuerungslisten für Dateien und Verzeichnisse](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).


### <a name="azure-data-lake-storage-gen1-access-control"></a>Zugriffssteuerung von Azure Data Lake Storage Gen1

Azure Data Lake Storage Gen1 unterstützt nur Zugriffssteuerungslisten im POSIX-Format. Sie können der verwalteten Identität des Arbeitsbereichs wie jedem anderen Sicherheitsprinzip Zugriff auf Ressourcen zuweisen. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md).


### <a name="azure-sql-database-contained-user"></a>Eigenständiger Benutzer in Azure SQL-Datenbank

Für den Zugriff mithilfe der verwalteten Identität auf Daten, die in einer Azure SQL-Datenbank-Instanz gespeichert sind, müssen Sie einen eigenständigen SQL-Benutzer erstellen, der der verwalteten Identität zugeordnet ist. Weitere Informationen zum Erstellen eines Benutzers von einem externen Anbieter finden Sie unter [Erstellen eigenständiger Benutzer mit Zuordnung zu Azure AD-Identitäten](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities).

Nachdem Sie einen eigenständigen SQL-Benutzer erstellt haben, erteilen Sie mithilfe des [GRANT T-SQL-Befehls](https://docs.microsoft.com/sql/t-sql/statements/grant-object-permissions-transact-sql) Berechtigungen.

### <a name="connect-to-the-studio"></a>Herstellen der Verbindung mit dem Studio

Wenn Sie über eine Ressource innerhalb eines virtuellen Netzwerks (z. B. eine Computeinstanz oder eine VM) auf das Studio zugreifen, müssen Sie aus dem virtuellen Netzwerk an das Studio ausgehenden Datenverkehr zulassen. 

Wenn Sie z. B. ausgehenden Datenverkehr mit Netzwerksicherheitsgruppen (NSG) einschränken, fügen Sie einem __Diensttag__-Ziel von __AzureFrontDoor.Frontend__ eine Regel hinzu.

## <a name="use-a-storage-account-for-your-workspace"></a>Verwenden eines Speicherkontos für Ihren Arbeitsbereich

> [!IMPORTANT]
> Sie können sowohl das _Standardspeicherkonto_ für Azure Machine Learning als auch _Nicht-Standardspeicherkonten_ in einem virtuellen Netzwerk anordnen.
>
> Das Standardspeicherkonto wird automatisch bereitgestellt, wenn Sie einen Arbeitsbereich erstellen.
>
> Für Nicht-Standardspeicherkonten können Sie mit dem `storage_account`-Parameter in der [`Workspace.create()`-Funktion](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-) ein benutzerdefiniertes Speicherkonto über die Azure-Ressourcen-ID angeben.

Führen Sie die folgenden Schritte aus, um einen Azure-Speicherdienst für den Arbeitsbereich in einem virtuellen Netzwerk zu verwenden:

1. Erstellen Sie eine Computeressource (z. B. Machine Learning-Compute-Instanz oder -Cluster) hinter einem virtuellen Netzwerk, oder fügen Sie eine Computeressource an den Arbeitsbereich an (z. B. einen HDInsight-Cluster, virtuellen Computer oder Azure Kubernetes Service-Cluster). Die Computeressource kann zum Experimentieren oder zur Modellimplementierung verwendet werden.

   Weitere Informationen finden Sie in diesem Artikel in den Abschnitten [Verwenden einer Machine Learning Compute-Instanz](#amlcompute), [Verwenden eines virtuellen Computers oder HDInsight-Clusters](#vmorhdi) und [Verwenden von Azure Kubernetes Service](#aksvnet).

1. Wechseln Sie im Azure-Portal zu dem Speicherdienst, den Sie in Ihrem Arbeitsbereich verwenden möchten.

   [![Der Speicher, der an den Azure Machine Learning-Arbeitsbereich angefügt ist](./media/how-to-enable-virtual-network/workspace-storage.png)](./media/how-to-enable-virtual-network/workspace-storage.png#lightbox)

1. Wählen Sie auf der Speicherdienstkonto-Seite __Firewalls und virtuelle Netzwerke__ aus.

   ![Der Bereich „Firewalls und virtuelle Netzwerke“ auf der Seite „Azure Storage“ im Azure-Portal.](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks.png)

1. Führen Sie auf der Seite __Firewalls und virtuelle Netzwerke__ folgende Aktionen aus:
    - Klicken Sie auf __Ausgewählte Netzwerke__.
    - Wählen Sie unter __Virtuelle Netzwerke__ den Link __Vorhandenes virtuelles Netzwerk hinzufügen__ aus. Durch diese Aktion wird das virtuelle Netzwerk an dem Ort hinzugefügt, wo sich Ihre Computeressource befindet (siehe Schritt 1).

        > [!IMPORTANT]
        > Das Speicherkonto muss sich in demselben virtuellen Netzwerk und Subnetz befinden wie die Compute-Instanzen oder Cluster, die für Training oder Rückschluss verwendet werden.

    - Aktivieren Sie das Kontrollkästchen __Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben__.

    > [!IMPORTANT]
    > Wenn Sie mit dem Azure Machine Learning SDK arbeiten, muss Ihre Entwicklungsumgebung eine Verbindung mit dem Azure Storage-Konto herstellen können. Wenn sich das Speicherkonto innerhalb eines virtuellen Netzwerks befindet, muss die Firewall den Zugriff über die IP-Adresse der Entwicklungsumgebung zulassen.
    >
    > Um den Zugriff auf das Speicherkonto zu ermöglichen, öffnen Sie die __Firewalls und virtuellen Netzwerke__ für das Speicherkonto *über einen Webbrowser auf dem Entwicklungsclient*. Fügen Sie dann mithilfe des Kontrollkästchens __Client-IP-Adresse hinzufügen__ die IP-Adresse Ihres Clients dem __ADRESSBEREICH__ hinzu. Sie können die IP-Adresse der Entwicklungsumgebung im Feld __ADRESSBEREICH__ auch manuell eingeben. Nachdem die IP-Adresse für den Client hinzugefügt wurde, kann er mit dem SDK auf das Speicherkonto zugreifen.

   [![Der Bereich „Firewalls und virtuelle Netzwerke“ im Azure-Portal](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png)](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png#lightbox)

## <a name="use-datastores-and-datasets"></a>Verwenden von Datasets und Datenspeichern

In diesem Abschnitt wird die Verwendung von Datenspeichern und Datasets für mit dem SDK behandelt. Weitere Informationen zu Studio finden Sie im Abschnitt [Machine Learning Studio](#machine-learning-studio).

Standardmäßig führt Azure Machine Learning Überprüfungen von Datengültigkeit und Anmeldeinformationen aus, wenn Sie versuchen, mit dem SDK auf Daten zuzugreifen. Wenn sich Ihre Daten hinter einem virtuellen Netzwerk befinden, hat Azure Machine Learning keinen Zugriff auf die Daten haben und kann sie nicht überprüfen. Um dies zu vermeiden, müssen Sie Datenspeicher und Datasets erstellen, die die Überprüfung überspringen.

### <a name="use-a-datastore"></a>Verwenden eines Datenspeichers

 Azure Data Lake Store Gen1 und Azure Data Lake Store Gen2 überspringen standardmäßig die Überprüfung, sodass keine weitere Aktion erforderlich ist. Für die folgenden Dienste können Sie jedoch eine ähnliche Syntax verwenden, um die Datenspeichervalidierung zu überspringen:

- Azure Blob Storage
- Azure-Dateifreigabe
- PostgreSQL
- Azure SQL-Datenbank

Im folgenden Codebeispiel wird ein neuer Azure-Blobdatenspeicher erstellt und `skip_validation=True` festgelegt.

```python
blob_datastore = Datastore.register_azure_blob_container(workspace=ws,  

                                                         datastore_name=blob_datastore_name,  

                                                         container_name=container_name,  

                                                         account_name=account_name, 

                                                         account_key=account_key, 

                                                         skip_validation=True ) // Set skip_validation to true
```

### <a name="use-a-dataset"></a>Verwenden eines Datasets

Die Syntax zum Überspringen der Überprüfung von Datasets ähnelt den folgenden Datasettypen:
- Durch Trennzeichen getrennte Datei
- JSON 
- Parquet
- SQL
- Datei

Der folgende Code erstellt ein neues JSON-Dataset und legt `validate=False`fest.

```python
json_ds = Dataset.Tabular.from_json_lines_files(path=datastore_paths, 

validate=False) 

```


## <a name="compute-clusters--instances"></a><a name="compute-instance"></a>Computecluster und -instanzen 

Wenn Sie ein [verwaltetes Azure Machine Learning-__Computeziel__](concept-compute-target.md#azure-machine-learning-compute-managed) oder eine [Azure Machine Learning-__Computeinstanz__](concept-compute-instance.md) in einem virtuellen Netzwerk verwenden möchten, müssen die folgenden Netzwerkanforderungen erfüllt sein:

> [!div class="checklist"]
> * Das virtuelle Netzwerk muss sich im selben Abonnement und in derselben Region befinden wie der Azure Machine Learning-Arbeitsbereich.
> * Das für die Computeinstanz oder den Computecluster angegebene Subnetz muss für die vorgesehene Anzahl virtueller Computer über genügend nicht zugewiesene IP-Adressen verfügen. Falls das Subnetz nicht genügend zugewiesene IP-Adressen hat, wird ein Computecluster teilweise zugeordnet.
> * Überprüfen Sie, ob Berechtigungen für die Verwaltung des virtuellen Netzwerks durch Ihre Sicherheitsrichtlinien oder -sperren für das Abonnement oder die Ressourcengruppe Ihres virtuellen Netzwerks eingeschränkt werden. Wenn Sie zum Schutz des virtuellen Netzwerks den Datenverkehr einschränken möchten, lassen Sie einige Ports für den Compute-Dienst geöffnet. Weitere Informationen finden Sie im Abschnitt [Erforderliche Ports](#mlcports).
> * Wenn Sie mehrere Compute-Instanzen oder -cluster in einem einzelnen virtuellen Netzwerk platzieren möchten, müssen Sie möglicherweise eine Kontingenterhöhung für eine oder mehrere Ihrer Ressourcen anfordern.
> * Wenn die Azure Storage-Konten für den Arbeitsbereich ebenfalls in einem virtuellen Netzwerk geschützt sind, müssen sie sich im selben virtuellen Netzwerk befinden wie die Azure Machine Learning Compute-Instanz oder der Cluster. 
> * Damit die Jupyter-Funktionen für Computeinstanzen eingesetzt werden können, stellen Sie sicher, dass die Websocketkommunikation nicht deaktiviert ist.

> [!TIP]
> Die Machine Learning-Compute-Instanz bzw. der Cluster ordnet __in der Ressourcengruppe mit dem virtuellen Netzwerk__ automatisch zusätzliche Netzwerkressourcen zu. Für alle Compute-Instanzen und -cluster ordnet der Dienst folgende Ressourcen zu:
> 
> * Eine Netzwerksicherheitsgruppe
> * Eine öffentliche IP-Adresse
> * Ein Lastenausgleichsmodul
> 
> Bei Clustern werden diese Ressourcen jedes Mal, wenn der Cluster auf 0 Knoten herunterskaliert wird, gelöscht (und neu erstellt). Die Ressourcen werden jedoch noch einen Moment lang beibehalten, bis die Instanz vollständig gelöscht ist (durch Beenden werden die Ressourcen nicht entfernt). 
> Diese Ressourcen werden durch die [Ressourcenkontingente](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits) des Abonnements beschränkt.


### <a name="required-ports"></a><a id="mlcports"></a>Erforderliche Ports

Wenn Sie planen, das virtuelle Netzwerk zu schützen, indem Sie den Netzwerkverkehr zum bzw. aus dem öffentlichen Internet einschränken, müssen Sie eingehende Kommunikation vom Azure Batch-Dienst zulassen.

Der Batch-Dienst fügt Netzwerksicherheitsgruppen (NSG) auf der Ebene der Netzwerkschnittstellen (NIC) hinzu, die an virtuelle Computer angefügt sind. Diese NSGs konfigurieren automatisch Eingangs- und Ausgangsregeln, um folgenden Datenverkehr zuzulassen:

- Eingehender TCP-Datenverkehr an den Ports 29876 und 29877 aus einem __Diensttag__ von __BatchNodeManagement__.

    ![Eine Regel für eingehenden Datenverkehr, die das BatchNodeManagement-Diensttag verwendet.](./media/how-to-enable-virtual-network/batchnodemanagement-service-tag.png)

- (Optional) Eingehender TCP-Datenverkehr an Port 22, um Remotezugriff zu ermöglichen. Verwenden Sie diesen Port nur, wenn Sie mithilfe von SSH über die öffentliche IP-Adresse eine Verbindung herstellen möchten.

- Ausgehender Datenverkehr an allen Ports zum virtuellen Netzwerk

- Ausgehender Datenverkehr an allen Ports zum Internet

- Für eingehenden TCP-Datenverkehr für Computeinstanzen an Port 44224 vom __Diensttag__ von __AzureMachineLearning__.

> [!IMPORTANT]
> Seien Sie vorsichtig, wenn Sie Eingangs- und Ausgangsregeln in von Batch konfigurierten NSGs ändern. Wenn eine NSG die Kommunikation mit den Computeknoten blockiert, legt der Compute-Dienst den Zustand der Computeknoten auf „Nicht verwendbar“ fest.
>
> Sie müssen keine NSGs auf der Subnetzebene angeben, da der Azure Batch-Dienst eigene NSGs konfiguriert. Wenn dem Subnetz, in dem sich das Azure Machine Learning-Compute befindet, jedoch NSGs oder eine Firewall zugeordnet sind, müssen Sie auch den zuvor aufgelisteten Datenverkehr zulassen.

Die NSG-Regelkonfiguration im Azure-Portal wird in den folgenden Abbildungen dargestellt:

:::image type="content" source="./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png" alt-text="Die NSG-Eingangsregeln für Machine Learning Compute" border="true":::



![Die NSG-Ausgangsregeln für Machine Learning Compute.](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a name="limit-outbound-connectivity-from-the-virtual-network"></a><a id="limiting-outbound-from-vnet"></a> Beschränken ausgehender Verbindungen aus dem virtuellen Netzwerk

Wenn Sie die Standardausgangsregeln nicht verwenden möchten und den ausgehenden Zugriff auf Ihr virtuelles Netzwerk beschränken möchten, gehen Sie wie folgt vor:

- Verweigern Sie ausgehende Internetverbindungen mit NSG-Regeln.

- Beschränken Sie für eine __Compute-Instanz__ oder einen __Computecluster__ den ausgehenden Verkehr auf die folgenden Elemente:
   - Azure Storage mithilfe der __Dienstkennung__ von __Storage.RegionName__. Dabei ist `{RegionName}` der Name einer Azure-Region.
   - Azure Container Registry mithilfe der __Dienstkennung__ von __AzureContainerRegistry.RegionName__. Dabei ist `{RegionName}` der Name einer Azure-Region.
   - Azure Machine Learning, mit dem __Diensttag__ von __AzureMachineLearning__
   - Azure Resource Manager mithilfe der __Dienstkennung__ von __AzureResourceManager__
   - Azure Active Directory mithilfe der __Dienstkennung__ von __AzureActiveDirectory__

Die NSG-Regelkonfiguration im Azure-Portal wird in der folgenden Abbildung dargestellt:

[![Die NSG-Ausgangsregeln für Machine Learning Compute](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png)](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png#lightbox)

> [!NOTE]
> Wenn Sie planen, die von Microsoft bereitgestellten standardmäßigen Docker-Images zu verwenden und vom Benutzer verwaltete Abhängigkeiten zu aktivieren, müssen Sie auch die folgenden __Diensttags__ verwenden:
>
> * __MicrosoftContainerRegistry__
> * __AzureFrontDoor.FirstParty__
>
> Diese Konfiguration ist erforderlich, wenn Sie in Ihrem Trainingsskript über Code ähnlich den folgenden Codeausschnitten verfügen:
>
> __RunConfig-Training__
> ```python
> # create a new runconfig object
> run_config = RunConfiguration()
> 
> # configure Docker 
> run_config.environment.docker.enabled = True
> # For GPU, use DEFAULT_GPU_IMAGE
> run_config.environment.docker.base_image = DEFAULT_CPU_IMAGE 
> run_config.environment.python.user_managed_dependencies = True
> ```
>
> __Schätzungstraining__
> ```python
> est = Estimator(source_directory='.',
>                 script_params=script_params,
>                 compute_target='local',
>                 entry_script='dummy_train.py',
>                 user_managed=True)
> run = exp.submit(est)
> ```

### <a name="forced-tunneling"></a>Tunnelerzwingung

Wenn Sie das [erzwungene Tunneling](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm) mit Azure Machine Learning Compute verwenden, müssen Sie dem Subnetz mit der Computeressource die Kommunikation mit dem öffentlichen Internet erlauben. Diese Kommunikation wird für die Aufgabenplanung und den Zugriff auf Azure Storage verwendet.

Es gibt zwei Möglichkeiten, dies zu erreichen:

* Verwenden Sie [Virtual Network NAT](../virtual-network/nat-overview.md). Ein NAT-Gateway stellt für ein oder mehrere Subnetze in Ihrem virtuellen Netzwerk die ausgehende Internetverbindung bereit. Weitere Informationen hierzu finden Sie unter [Entwerfen von virtuellen Netzwerken mit NAT-Gatewayressourcen](../virtual-network/nat-gateway-resource.md).

* Fügen Sie dem Subnetz mit der Computeressource [Benutzerdefinierte Routen (UDRs)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) hinzu. Richten Sie für jede IP-Adresse eine benutzerdefinierte Route ein, die vom Azure Batch-Dienst in der Region Ihrer Ressourcen verwendet wird. Diese benutzerdefinierten Routen ermöglichen dem Batch-Dienst, für die zeitliche Planung von Tasks mit den Serverknoten zu kommunizieren. Fügen Sie außerdem die IP-Adresse für Azure Machine Learning Service hinzu, in dem die Ressourcen enthalten sind, da dies für den Zugriff auf Computeinstanzen erforderlich ist. Über die folgenden Methoden können Sie eine Liste der IP-Adressen des Batch-Diensts und von Azure Machine Learning Service abrufen:

    * Laden Sie die [Azure-IP-Adressbereiche und Diensttags](https://www.microsoft.com/download/details.aspx?id=56519) herunter, und suchen Sie in der Datei nach `BatchNodeManagement.<region>` und `AzureMachineLearning.<region>`, wobei `<region>` Ihre Azure-Region ist.

    * Laden Sie die Informationen mithilfe der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) herunter. Im folgenden Beispiel werden die IP-Adressinformationen heruntergeladen und die Informationen für die Region „USA, Osten 2“ herausgefiltert:

        ```azurecli-interactive
        az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'Batch')] | [?properties.region=='eastus2']"
        az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'AzureMachineLearning')] | [?properties.region=='eastus2']"
        ```
    
    Wenn Sie die benutzerdefinierten Routen hinzufügen, definieren Sie die Route für jedes zugehörige Batch-IP-Adresspräfix, und legen Sie den __Typ des nächsten Hops__ auf __Internet__ fest. Die folgende Abbildung zeigt ein Beispiel dieser benutzerdefinierten Route im Azure-Portal:

    ![Beispiel für eine benutzerdefinierte Route für ein Adresspräfix](./media/how-to-enable-virtual-network/user-defined-route.png)

    Zusätzlich zu allen von Ihnen angegebenen benutzerdefinierten Routen muss der ausgehende Datenverkehr zu Azure Storage über Ihre lokale Netzwerkappliance zugelassen werden. Die speziellen URLs für diesen Datenverkehr sind in folgenden Formularen zu finden: `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` und `<account>.blob.core.windows.net`. 

    Weitere Informationen finden Sie unter [Erstellen eines Azure Batch-Pools in einem virtuellen Netzwerk](../batch/batch-virtual-network.md#user-defined-routes-for-forced-tunneling).


### <a name="create-a-compute-cluster-in-a-virtual-network"></a>Erstellen eines Computeclusters in einem virtuellen Netzwerk

Gehen Sie zum Erstellen eines Machine Learning Compute-Clusters wie folgt vor:

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com/) an, und wählen Sie dann Ihr Abonnement und den Arbeitsbereich aus.

1. Wählen Sie links __Compute__ aus.

1. Wählen Sie in der Mitte __Trainingscluster__ aus, und wählen Sie dann __+__ aus.

1. Erweitern Sie im Dialogfeld __Neuer Trainingscluster__ den Abschnitt __Erweiterte Einstellungen__.

1. Um diese Computeressource für die Verwendung eines virtuellen Netzwerks zu konfigurieren, führen Sie im Abschnitt __Virtuelles Netzwerk konfigurieren__ die folgenden Schritte aus:

    1. Wählen Sie in der Dropdownliste __Ressourcengruppe__ die Ressourcengruppe aus, die das virtuelle Netzwerk enthält.
    1. Wählen Sie in der Dropdownliste __Virtuelles Netzwerk__ das virtuelle Netzwerk aus, das das Subnetzwerk enthält.
    1. Wählen Sie in der Dropdownliste __Subnetz__ das zu verwendende Subnetz aus.

   ![Die virtuellen Netzerkeinstellungen für Machine Learning Compute](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

Ein Machine Learning Compute-Cluster kann auch mithilfe des Azure Machine Learning SDK erstellt werden. Der folgende Code erstellt einen neuen Machine Learning Compute-Cluster im Subnetz `default` eines virtuellen Netzwerks namens `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# The Azure virtual network name, subnet, and resource group
vnet_name = 'mynetwork'
subnet_name = 'default'
vnet_resourcegroup_name = 'mygroup'

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print("Found existing cpucluster")
except ComputeTargetException:
    print("Creating new cpucluster")

    # Specify the configuration for the new cluster
    compute_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                           min_nodes=0,
                                                           max_nodes=4,
                                                           vnet_resourcegroup_name=vnet_resourcegroup_name,
                                                           vnet_name=vnet_name,
                                                           subnet_name=subnet_name)

    # Create the cluster with the specified name and configuration
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

    # Wait for the cluster to be completed, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

Nach Abschluss des Erstellungsprozesses trainieren Sie Ihr Modell, indem Sie den Cluster in einem Experiment verwenden. Weitere Informationen finden Sie unter [Auswählen und Verwenden eines Computeziels für das Training](how-to-set-up-training-targets.md).

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

### <a name="access-data-in-a-compute-instance-notebook"></a>Zugreifen auf Daten auf einem Compute-Instanz-Notebook

Wenn Sie Notebooks auf einer Azure-Compute-Instanz verwenden, müssen Sie sicherstellen, dass Ihr Notebook auf einer Computeressource hinter dem gleichen virtuellen Netzwerk und Subnetz wie Ihre Daten ausgeführt wird. 

Sie müssen die Compute-Instanz so konfigurieren, das sie sich während der Erstellung unter **Erweiterte Einstellungen** > **Konfigurieren des virtuellen Netzwerks** in demselben virtuellen Netzwerk befindet. Eine vorhandene Compute-Instanz kann in einem virtuellen Netzwerk nicht hinzugefügt werden.

<a id="aksvnet"></a>

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Führen Sie die folgenden Schritte aus, um Ihrem Arbeitsbereich in einem virtuellen Netzwerk Azure Kubernetes Service (AKS) hinzuzufügen:

> [!IMPORTANT]
> Bevor Sie mit dem folgenden Verfahren beginnen, befolgen Sie die Anweisungen im Abschnitt „Voraussetzungen“ von [Konfigurieren von Azure CNI-Netzwerken in Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/configure-azure-cni#prerequisites),und planen Sie die IP-Adressierung für Ihren Cluster.
>
> Die AKS-Instanz und das virtuelle Azure-Netzwerk müssen sich in derselben Region befinden. Wenn Sie die Azure Storage-Konten, die vom Arbeitsbereich verwendet werden, in einem virtuellen Netzwerk schützen, müssen sie sich im selben virtuellen Netzwerk befinden wie die AKS-Instanz.

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com/) an, und wählen Sie dann Ihr Abonnement und den Arbeitsbereich aus.

1. Wählen Sie links __Compute__ aus.

1. Wählen Sie in der Mitte __Rückschlusscluster__ aus, und wählen Sie dann __+__ aus.

1. Wählen Sie im Dialogfeld __Neuer Rückschlusscluster__ unter __Netzwerkkonfiguration__ die Option __Erweitert__ aus.

1. Gehen Sie zum Konfigurieren dieser Computeressource für die Verwendung eines virtuellen Netzwerks wie folgt vor:

    1. Wählen Sie in der Dropdownliste __Ressourcengruppe__ die Ressourcengruppe aus, die das virtuelle Netzwerk enthält.
    1. Wählen Sie in der Dropdownliste __Virtuelles Netzwerk__ das virtuelle Netzwerk aus, das das Subnetzwerk enthält.
    1. Wählen Sie in der Dropdownliste __Subnetz__ das Subnetz aus.
    1. Geben Sie in das Feld __Kubernetes Service Address Range__ (Kubernetes-Dienstadressbereich) den Kubernetes-Dienstadressbereich ein. Dieser Adressbereich verwendet einen IP-Adressbereich in CIDR-Notation (Classless Inter-Domain Routing), um die für den Cluster verfügbaren IP-Adressen zu definieren. Er darf sich mit keinem IP-Adressbereich eines Subnetzes überschneiden (z. B. 10.0.0.0/16).
    1. Geben Sie in das Feld __Kubernetes DNS Service IP Address__ (Kubernetes-DNS-Dienst-IP-Adresse) die Kubernetes-DNS-Dienst-IP-Adresse ein. Diese IP-Adresse wird dem Kubernetes-DNS-Dienst zugewiesen. Sie muss innerhalb des Kubernetes-Dienstadressbereichs liegen (z. B. 10.0.0.10).
    1. Geben Sie in das Feld __Adresse der Docker-Brücke__ die Adresse der Docker-Brücke ein. Diese IP-Adresse wird der Docker-Brücke zugewiesen. Sie darf weder dem IP-Adressbereich eines Subnetzes noch dem Adressbereich des Kubernetes-Diensts angehören (z. B. 172.17.0.1/16).

   ![Azure Machine Learning: VNET-Einstellungen für Machine Learning Compute](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

1. Stellen Sie sicher, dass für die Netzwerksicherheitsgruppe, die das virtuelle Netzwerk steuert, eine Sicherheitsregel für eingehenden Datenverkehr für den Bewertungsendpunkt aktiviert ist, damit sie von außerhalb des virtuellen Netzwerks aus aufgerufen werden kann.
   > [!IMPORTANT]
   > Behalten Sie die Standardausgangsregeln für die NSG bei. Weitere Informationen finden Sie unter [Sicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules) bei den Standardsicherheitsregeln.

   [![Eine Eingangssicherheitsregel](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png)](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png#lightbox)

Sie können auch das Azure Machine Learning SDK verwenden, um Azure Kubernetes Service einem virtuellen Netzwerk hinzuzufügen. Wenn Sie bereits über einen AKS-Cluster in einem virtuellen Netzwerk verfügen, können Sie ihn dem Arbeitsbereich anfügen (siehe [Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst](how-to-deploy-and-where.md)). Der folgende Code erstellt eine neue AKS-Instanz im Subnetz `default` eines virtuellen Netzwerks namens `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AksCompute

# Create the compute configuration and set virtual network information
config = AksCompute.provisioning_configuration(location="eastus2")
config.vnet_resourcegroup_name = "mygroup"
config.vnet_name = "mynetwork"
config.subnet_name = "default"
config.service_cidr = "10.0.0.0/16"
config.dns_service_ip = "10.0.0.10"
config.docker_bridge_cidr = "172.17.0.1/16"

# Create the compute target
aks_target = ComputeTarget.create(workspace=ws,
                                  name="myaks",
                                  provisioning_configuration=config)
```

Nach Abschluss des Erstellungsprozesses können Sie Rückschlüsse für einen AKS-Cluster hinter einem virtuellen Netzwerk ziehen oder das Modell bewerten. Weitere Informationen finden Sie unter [Bereitstellen im AKS](how-to-deploy-and-where.md).

### <a name="use-private-ips-with-azure-kubernetes-service"></a>Verwenden von privaten IP-Adressen mit Azure Kubernetes Service

Standardmäßig wird den AKS-Bereitstellungen eine öffentliche IP-Adresse zugewiesen. Wenn Sie AKS innerhalb eines virtuellen Netzwerks verwenden, können Sie stattdessen eine private IP-Adresse verwenden. Private IP-Adressen sind nur innerhalb des virtuellen Netzwerks oder von verknüpften Netzwerken aus zugänglich.

Eine private IP-Adresse wird aktiviert, indem AKS zur Verwendung eines _internen Lastenausgleichs_ konfiguriert wird. 

#### <a name="network-contributor-role"></a>Rolle „Netzwerkmitwirkender“

> [!IMPORTANT]
> Wenn Sie einen AKS-Cluster erstellen oder anfügen, indem Sie ein zuvor erstelltes virtuelles Netzwerk bereitstellen, müssen Sie dem Dienst Prinzipal (Service Principal, SP) oder der verwalteten Identität für den AKS-Cluster die Rolle _Netzwerkmitwirkender_ der Ressourcengruppe zuweisen, die das virtuelle Netzwerk enthält. Dies muss geschehen, bevor Sie versuchen, den internen Load Balancer auf eine private IP-Adresse umzustellen.
>
> Um die Identität als Netzwerkmitwirkenden hinzuzufügen, führen Sie die folgenden Schritte aus:

1. Verwenden Sie zum Ermitteln der ID des Dienstprinzipals oder der verwalteten Identität für AKS die folgenden Azure CLI Befehle. Ersetzen Sie `<aks-cluster-name>` durch den Namen des Clusters. Ersetzen Sie `<resource-group-name>` durch den Namen der Ressourcengruppe, die den _AKS-Cluster enthält_:

    ```azurecli-interactive
    az aks show -n <aks-cluster-name> --resource-group <resource-group-name> --query servicePrincipalProfile.clientId
    ``` 

    Wenn dieser Befehl den Wert `msi` zurückgibt, verwenden Sie den folgenden Befehl, um die Prinzipal-ID für die verwaltete Identität zu bestimmen:

    ```azurecli-interactive
    az aks show -n <aks-cluster-name> --resource-group <resource-group-name> --query identity.principalId
    ```

1. Verwenden Sie den folgenden Befehl, um die ID der Ressourcengruppe zu ermitteln, die das virtuelle Netzwerk enthält. Ersetzen Sie `<resource-group-name>` durch den Namen der Ressourcengruppe, die das _virtuelle Azure-Netzwerk enthält_:

    ```azurecli-interactive
    az group show -n <resource-group-name> --query id
    ```

1. Um den Dienstprinzipal oder die verwaltete Identität als Netzwerkmitwirkenden hinzuzufügen, verwenden Sie den folgenden Befehl. Ersetzen Sie `<SP-or-managed-identity>` durch die für den Dienstprinzipal oder die verwaltete Identität zurückgegebene ID. Ersetzen Sie `<resource-group-id>` durch die ID, die für die Ressourcengruppe mit dem virtuellen Netzwerk zurückgegeben wurde:

    ```azurecli-interactive
    az role assignment create --assignee <SP-or-managed-identity> --role 'Network Contributor' --scope <resource-group-id>
    ```
Weitere Informationen zur Verwendung des internen Lastenausgleichs mit AKS finden Sie unter [Verwenden des internen Lastenausgleichs mit Azure Kubernetes Service](/azure/aks/internal-lb).

#### <a name="enable-private-ip"></a>Aktivieren einer privaten IP-Adresse

> [!IMPORTANT]
> Sie können beim Erstellen des Azure Kubernetes Service-Clusters keine private IP-Adresse aktivieren. Er muss als Update eines bestehenden Clusters aktiviert werden.

Der folgende Codeausschnitt veranschaulicht, wie Sie __einen neuen AKS-Cluster erstellen__ und ihn dann aktualisieren, um eine private IP-Adresse bzw. einen internen Lastenausgleich zu verwenden:

```python
import azureml.core
from azureml.core.compute.aks import AksUpdateConfiguration
from azureml.core.compute import AksCompute, ComputeTarget

# Verify that cluster does not exist already
try:
    aks_target = AksCompute(workspace=ws, name=aks_cluster_name)
    print("Found existing aks cluster")

except:
    print("Creating new aks cluster")

    # Subnet to use for AKS
    subnet_name = "default"
    # Create AKS configuration
    prov_config = AksCompute.provisioning_configuration(location = "eastus2")
    # Set info for existing virtual network to create the cluster in
    prov_config.vnet_resourcegroup_name = "myvnetresourcegroup"
    prov_config.vnet_name = "myvnetname"
    prov_config.service_cidr = "10.0.0.0/16"
    prov_config.dns_service_ip = "10.0.0.10"
    prov_config.subnet_name = subnet_name
    prov_config.docker_bridge_cidr = "172.17.0.1/16"

    # Create compute target
    aks_target = ComputeTarget.create(workspace = ws, name = "myaks", provisioning_configuration = prov_config)
    # Wait for the operation to complete
    aks_target.wait_for_completion(show_output = True)
    
    # Update AKS configuration to use an internal load balancer
    update_config = AksUpdateConfiguration(None, "InternalLoadBalancer", subnet_name)
    aks_target.update(update_config)
    # Wait for the operation to complete
    aks_target.wait_for_completion(show_output = True)
```

__Azure-Befehlszeilenschnittstelle__

```azurecli-interactive
az rest --method put --uri https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.MachineLearningServices/workspaces/<workspace>/computes/<compute>?api-version=2018-11-19 --body @body.json
```

Der Inhalt der Datei `body.json`, auf die der Befehl verweist, ist dem folgenden JSON-Dokument ähnlich:

```json
{ 
    "location": "<region>", 
    "properties": { 
        "resourceId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.ContainerService/managedClusters/<aks-resource-name>", 
        "computeType": "AKS", 
        "provisioningState": "Succeeded", 
        "properties": { 
            "loadBalancerType": "InternalLoadBalancer", 
            "agentCount": <agent-count>, 
            "agentVmSize": "vm-size", 
            "clusterFqdn": "<cluster-fqdn>" 
        } 
    } 
} 
```

Wenn Sie einen __vorhandenen Cluster__ an den Arbeitsbereich anfügen, müssen Sie nach dem Anfügevorgang mit der Load Balancer-Konfiguration warten.

Weitere Informationen zum Anfügen eines Clusters finden Sie unter [Anfügen eines vorhandenen AKS-Clusters](how-to-deploy-azure-kubernetes-service.md#attach-an-existing-aks-cluster).

Nachdem Sie den vorhandenen Cluster angefügt haben, können Sie den Cluster so aktualisieren, dass er eine private IP verwendet.

```python
import azureml.core
from azureml.core.compute.aks import AksUpdateConfiguration
from azureml.core.compute import AksCompute

# ws = workspace object. Creation not shown in this snippet
aks_target = AksCompute(ws,"myaks")

# Change to the name of the subnet that contains AKS
subnet_name = "default"
# Update AKS configuration to use an internal load balancer
update_config = AksUpdateConfiguration(None, "InternalLoadBalancer", subnet_name)
aks_target.update(update_config)
# Wait for the operation to complete
aks_target.wait_for_completion(show_output = True)
```

## <a name="use-azure-container-instances-aci"></a>Azure Container Instances (ACIs)

Azure Container Instances werden beim Bereitstellen eines Modells dynamisch erstellt. Damit Azure Machine Learning ACIs innerhalb des virtuellen Netzwerks erstellen kann, müssen Sie __Subnetzdelegierung__ für das von der Bereitstellung verwendete Subnetz aktivieren.

> [!WARNING]
> Wenn Sie Azure Container Instances in einem virtuellen Netzwerk verwenden, muss sich das virtuelle Netzwerk in derselben Ressourcengruppe wie der Azure Machine Learning-Arbeitsbereich befinden.
>
> Wenn Sie Azure Container Instances innerhalb des virtuellen Netzwerks verwenden, darf sich die Azure Container Registry (ACR) für Ihren Arbeitsbereich nicht ebenfalls im virtuellen Netzwerk befinden.

Gehen Sie folgendermaßen vor, um ACIs in einem virtuellen Netzwerk zu Ihrem Arbeitsbereich hinzuzufügen:

1. Anweisungen zum Aktivieren der Subnetzdelegierung in einem virtuellen Netzwerk finden Sie im Artikel [Hinzufügen oder Entfernen einer Subnetzdelegierung](../virtual-network/manage-subnet-delegation.md). Sie können die Delegierung beim Erstellen eines virtuellen Netzwerks aktivieren oder einem bestehenden Netzwerk hinzufügen.

    > [!IMPORTANT]
    > Wenn Sie Delegierung aktivieren, verwenden Sie `Microsoft.ContainerInstance/containerGroups` als Wert für __Subnetz an einen Dienst delegieren__.

2. Stellen Sie das Modell mithilfe von [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none--vnet-name-none--subnet-name-none-) unter Angabe der Parameter `vnet_name` und `subnet_name` bereit. Legen Sie diese Parameter auf den Namen und das Subnetz des virtuellen Netzwerks fest, in dem Sie Delegierung aktiviert haben.

## <a name="azure-firewall"></a>Azure Firewall

Informationen zur Verwendung von Azure Machine Learning mit Azure Firewall finden Sie unter [Verwenden von Azure Machine Learning-Arbeitsbereichen hinter Azure Firewall](how-to-access-azureml-behind-firewall.md).

## <a name="azure-container-registry"></a>Azure Container Registry

> [!IMPORTANT]
> Azure Container Registry (ACR) kann in einem virtuellen Netzwerk platziert werden, jedoch müssen die folgenden Voraussetzungen erfüllt sein:
>
> * Der Azure Machine Learning-Arbeitsbereich muss Enterprise Edition aufweisen. Informationen zum Durchführen eines Upgrades finden Sie unter [Upgrade auf Enterprise Edition](how-to-manage-workspace.md#upgrade).
> * Ihre Azure Machine Learning-Arbeitsbereichsregion sollte eine [private, über Link aktivierte Region](https://docs.microsoft.com/azure/private-link/private-link-overview#availability) sein. 
> * Azure Container Registry muss die Premium-Version aufweisen. Informationen zum Durchführen eines Upgrades finden Sie unter [Wechseln von SKUs](/azure/container-registry/container-registry-skus#changing-skus).
> * Azure Container Registry muss sich in demselben virtuellen Netzwerk und Subnetz befinden wie das Speicherkonto und die Computeziele, die für Training oder Rückschluss verwendet werden.
> * Der Azure Machine Learning-Arbeitsbereich muss einen [Azure Machine Learning-Computecluster](how-to-set-up-training-targets.md#amlcompute) enthalten.
>
>     Wenn sich ACR hinter einem virtuellen Netzwerk befindet, kann Azure Machine Learning es nicht zum direkten Erstellen von Docker-Images verwenden. Stattdessen wird zum Erstellen der Images der Computecluster verwendet.

1. Suchen Sie den Namen der Instanz von Azure Container Registry für Ihren Arbeitsbereich mit einer der folgenden Methoden:

    __Azure portal__

    Im Abschnitt „Übersicht“ des Arbeitsbereichs ist der Wert __Registrierung__ mit der Azure Container Registry-Instanz verknüpft.

    :::image type="content" source="./media/how-to-enable-virtual-network/azure-machine-learning-container-registry.png" alt-text="Azure Container Registry für den Arbeitsbereich" border="true":::

    __Azure-Befehlszeilenschnittstelle__

    Wenn Sie die [Machine Learning-Erweiterung für die Azure CLI installiert](reference-azure-machine-learning-cli.md) haben, können Sie mit dem Befehl `az ml workspace show` die Arbeitsbereichsinformationen anzeigen.

    ```azurecli-interactive
    az ml workspace show -w yourworkspacename -g resourcegroupname --query 'containerRegistry'
    ```

    Der Befehl gibt einen Wert zurück, der `"/subscriptions/{GUID}/resourceGroups/{resourcegroupname}/providers/Microsoft.ContainerRegistry/registries/{ACRname}"` ähnelt. Der letzte Teil der Zeichenfolge ist der Name der Azure Container Registry-Instanz für den Arbeitsbereich.

1. Um den Zugriff auf das virtuelle Netzwerk einzuschränken, führen Sie die Schritte in [Konfigurieren des Netzwerkzugriffs für die Registrierung](../container-registry/container-registry-vnet.md#configure-network-access-for-registry) aus. Wenn Sie das virtuelle Netzwerk hinzufügen, wählen Sie das virtuelle Netzwerk und das Subnetz für die Azure Machine Learning-Ressourcen aus.

1. Verwenden Sie das Python-SDK für Azure Machine Learning, um einen Computecluster zum Erstellen von Docker-Images zu konfigurieren. Im folgenden Codeausschnitt wird diese Vorgehensweise veranschaulicht:

    ```python
    from azureml.core import Workspace
    # Load workspace from an existing config file
    ws = Workspace.from_config()
    # Update the workspace to use an existing compute cluster
    ws.update(image_build_compute = 'mycomputecluster')
    ```

    > [!IMPORTANT]
    > Das Speicherkonto, der Computecluster und die Azure Container Registry-Instanz müssen sich in demselben Subnetz des virtuellen Netzwerks befinden.
    
    Weitere Informationen finden Sie in der Referenz zur [update()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#update-friendly-name-none--description-none--tags-none--image-build-compute-none--enable-data-actions-none-)-Methode.

1. Sie müssen die folgende Azure Resource Manager-Vorlage anwenden. Durch diese Vorlage kann Ihr Arbeitsbereich mit ACR kommunizieren.

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "keyVaultArmId": {
        "type": "string"
        },
        "workspaceName": {
        "type": "string"
        },
        "containerRegistryArmId": {
        "type": "string"
        },
        "applicationInsightsArmId": {
        "type": "string"
        },
        "storageAccountArmId": {
        "type": "string"
        },
        "location": {
        "type": "string"
        }
    },
    "resources": [
        {
        "type": "Microsoft.MachineLearningServices/workspaces",
        "apiVersion": "2019-11-01",
        "name": "[parameters('workspaceName')]",
        "location": "[parameters('location')]",
        "identity": {
            "type": "SystemAssigned"
        },
        "sku": {
            "tier": "enterprise",
            "name": "enterprise"
        },
        "properties": {
            "sharedPrivateLinkResources":
    [{"Name":"Acr","Properties":{"PrivateLinkResourceId":"[concat(parameters('containerRegistryArmId'), '/privateLinkResources/registry')]","GroupId":"registry","RequestMessage":"Approve","Status":"Pending"}}],
            "keyVault": "[parameters('keyVaultArmId')]",
            "containerRegistry": "[parameters('containerRegistryArmId')]",
            "applicationInsights": "[parameters('applicationInsightsArmId')]",
            "storageAccount": "[parameters('storageAccountArmId')]"
        }
        }
    ]
    }
    ```

## <a name="key-vault-instance"></a>Key Vault-Instanz 

Die mit dem Arbeitsbereich verknüpfte Key Vault-Instanz wird von Azure Machine Learning zum Speichern der folgenden Anmeldeinformationen verwendet:
* Der verknüpften Speicherkonto-Verbindungszeichenfolge
* Kennwörter in Azure Container Repository-Instanzen
* Verbindungszeichenfolgen zur Verbindung mit Datenspeichern.

Um die Azure Machine Learning-Experimentierfunktionen mit Azure Key Vault hinter einem virtuellen Netzwerk zu verwenden, gehen Sie wie folgt vor:

1. Wechseln Sie zu dem Schlüsseltresor, der mit dem Arbeitsbereich verknüpft ist.

   [![Der Schlüsseltresor, der mit dem Azure Machine Learning-Arbeitsbereich verknüpft ist](./media/how-to-enable-virtual-network/workspace-key-vault.png)](./media/how-to-enable-virtual-network/workspace-key-vault.png#lightbox)

1. Wählen Sie auf der Seite __Key Vault__ im linken Bereich __Firewalls und virtuelle Netzwerke__ aus.

   ![Der Abschnitt „Firewalls und virtuelle Netzwerke“ im Bereich „Key Vault“.](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks.png)

1. Führen Sie auf der Seite __Firewalls und virtuelle Netzwerke__ folgende Aktionen aus:
    - Wählen Sie unter __Zugriff erlauben von__ den Eintrag __Ausgewählte Netzwerke__ aus.
    - Wählen Sie unter __Virtuelle Netzwerke__ die Option __Vorhandenes virtuelles Netzwerk hinzufügen__ aus, um das virtuelle Netzwerk hinzuzufügen, in dem sich die Computeressourcen für Ihre Experimente befinden.
    - Wählen Sie unter __Vertrauenswürdigen Microsoft-Diensten die Umgehung dieser Firewall erlauben__ die Option __Ja__ aus.

   [![Der Abschnitt „Firewalls und virtuelle Netzwerke“ im Bereich „Key Vault“](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png)](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png#lightbox)


## <a name="azure-databricks"></a>Azure Databricks

Damit Sie Azure Databricks in einem virtuellen Netzwerk mit Ihrem Arbeitsbereich verwenden können, müssen die folgenden Voraussetzungen erfüllt sein:

> [!div class="checklist"]
> * Das virtuelle Netzwerk muss sich im selben Abonnement und in derselben Region befinden wie der Azure Machine Learning-Arbeitsbereich.
> * Wenn die Azure Storage-Konten für den Arbeitsbereich ebenfalls in einem virtuellen Netzwerk geschützt sind, müssen sie sich im selben virtuellen Netzwerk befinden wie der Azure Databricks-Cluster.
> * Zusätzlich zu den von Azure Databricks verwendeten Subnetzen __databricks-private__ und __databricks-public__ ist auch das für das virtuelle Netzwerk erstellte __standardmäßige__ Subnetz erforderlich.

Weitere Informationen zur Verwendung von Azure Databricks mit einem virtuellen Netzwerk finden Sie unter [Bereitstellen von Azure Databricks in Ihrem Azure Virtual Network](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html).

<a id="vmorhdi"></a>

## <a name="virtual-machine-or-hdinsight-cluster"></a>Virtueller Computer oder HDInsight-Cluster

> [!IMPORTANT]
> Azure Machine Learning unterstützt nur virtuelle Computer, auf denen Ubuntu ausgeführt wird.

Gehen Sie wie folgt vor, um einen virtuellen Computer oder einen Azure HDInsight-Cluster in einem virtuellen Netzwerk mit Ihrem Arbeitsbereich zu verwenden:

1. Erstellen Sie einen virtuellen Computer oder einen HDInsight-Cluster mithilfe des Azure-Portals oder der Azure-Befehlszeilenschnittstelle, und platzieren Sie den Cluster in einem virtuellen Azure-Netzwerk. Weitere Informationen finden Sie in den folgenden Artikeln:
    * [Erstellen und Verwalten virtueller Azure-Netzwerke für virtuelle Linux-Computer](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [Erweitern von Azure HDInsight per Azure Virtual Network](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network)

1. Damit Azure Machine Learning mit dem SSH-Port des virtuellen Computers oder des Clusters kommunizieren kann, konfigurieren Sie einen Quelleintrag für die Netzwerksicherheitsgruppe. Der SSH-Port ist in der Regel der Port 22. Gehen Sie wie folgt vor, um Datenverkehr aus dieser Quelle zuzulassen:

    * Wählen Sie in der Dropdownliste __Quelle__ den Eintrag __Diensttag__ aus.

    * Wählen Sie in der Dropdownliste __Quelldiensttag__ den Eintrag __AzureMachineLearning__ aus.

    * Wählen Sie in der Dropdownliste __Quellportbereiche__ den Eintrag __*__ aus.

    * Wählen in der Dropdownliste __Ziel__ die Option __Alle__ aus.

    * Wählen Sie in der Dropdownliste __Quellportbereiche__ den Eintrag __22__ aus.

    * Wählen Sie unter __Protokoll__ die Option __Alle__ aus.

    * Wählen Sie für __Aktion__ die Option __Zulassen__ aus.

   ![Eingangsregeln für Experimente auf einem virtuellen Computer oder in einem HDInsight-Cluster innerhalb eines virtuellen Netzwerks](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Behalten Sie die Standardausgangsregeln für die Netzwerksicherheitsgruppe bei. Weitere Informationen finden Sie unter [Sicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules) bei den Standardsicherheitsregeln.

    Wenn Sie die Standardausgangsregeln nicht verwenden möchten und den ausgehenden Zugriff auf Ihr virtuelles Netzwerk beschränken möchten, finden Sie weitere Informationen im Abschnitt [Beschränken der ausgehenden Konnektivität aus dem virtuellen Netzwerk](#limiting-outbound-from-vnet).

1. Fügen Sie den virtuellen Computer oder den HDInsight-Cluster an Ihren Azure Machine Learning-Arbeitsbereich an. Weitere Informationen finden Sie unter [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md).


## <a name="next-steps"></a>Nächste Schritte

* [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md)
* [Einrichten von privaten Endpunkten](how-to-configure-private-link.md)
* [Deploy models with the Azure Machine Learning service](how-to-deploy-and-where.md) (Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst)
* [Verwenden von TLS zum Absichern eines Webdiensts mit Azure Machine Learning](how-to-secure-web-service.md)