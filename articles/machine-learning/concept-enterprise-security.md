---
title: Unternehmenssicherheit
titleSuffix: Azure Machine Learning
description: 'Verwenden Sie Azure Machine Learning sicher: Authentifizierung, Autorisierung, Netzwerksicherheit, Datenverschlüsselung und Überwachung.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 05/19/2020
ms.openlocfilehash: 5afa6b9127317fcd1a683651be86cdfe078cfcd6
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259437"
---
# <a name="enterprise-security-for-azure-machine-learning"></a>Unternehmenssicherheit für Azure Machine Learning

In diesem Artikel lernen Sie Sicherheitsfeatures kennen, die für Azure Machine Learning zur Verfügung stehen.

Wenn Sie einen Clouddienst verwenden, empfiehlt es sich, den Zugriff auf die Benutzer zu beschränken, die ihn benötigen. Als Erstes sollten Sie dabei das vom Dienst verwendete Authentifizierungs- und Autorisierungsmodell kennenlernen. Ggf. ist es auch erforderlich, den Zugriff auf das Netzwerk einzuschränken oder Ressourcen in Ihrem lokalen Netzwerk sicher mit der Cloud zu verknüpfen. Datenverschlüsselung ist auch von zentraler Bedeutung, sowohl im Ruhezustand als auch während des Verschiebens der Daten zwischen Diensten. Schließlich müssen Sie in der Lage sein, den Dienst zu überwachen und ein Überwachungsprotokoll aller Aktivitäten zu erstellen.

> [!NOTE]
> Die Informationen in diesem Artikel arbeiten mit dem Azure Machine Learning Python-SDK Version 1.0.83.1 oder höher.

## <a name="authentication"></a>Authentifizierung

Die mehrstufige Authentifizierung wird unterstützt, wenn Azure Active Directory (Azure AD) dafür konfiguriert ist. So verläuft der Authentifizierungsprozess:

1. Der Client meldet sich bei Azure AD an und ruft das Azure Resource Manager-Token ab.  Benutzer und Dienstprinzipale werden vollständig unterstützt.
1. Der Client übergibt das Token an Azure Resource Manager und an Azure Machine Learning.
1. Machine Learning Service stellt ein Machine Learning Service-Token für das Benutzercomputeziel bereit (z. B. Machine Learning Compute). Dieses Token wird nach Abschluss der Ausführung vom Benutzercomputeziel für einen erneuten Aufruf von Machine Learning Service verwendet. Der Bereich ist auf den Arbeitsbereich beschränkt.

[![Authentifizierung in Azure Machine Learning](media/concept-enterprise-security/authentication.png)](media/concept-enterprise-security/authentication-expanded.png#lightbox)

Weitere Informationen finden Sie unter [Einrichten der Authentifizierung für Azure Machine Learning-Ressourcen und -Workflows](how-to-setup-authentication.md). Dieser Artikel enthält Informationen und Beispiele zur Authentifizierung, einschließlich der Verwendung von Dienstprinzipalen und automatisierten Workflows.

### <a name="authentication-for-web-service-deployment"></a>Authentifizierung für die Webdienstbereitstellung

Azure Machine Learning unterstützt zwei Authentifizierungsarten für Webdienste: Schlüssel und Token. Die einzelnen Webdienste können jeweils nur eine Authentifizierungsart verwenden.

|Authentifizierungsmethode|BESCHREIBUNG|Azure Container Instances|AKS|
|---|---|---|---|
|Schlüssel|Schlüssel sind statisch und müssen nicht aktualisiert werden. Schlüssel können manuell erneut generiert werden.|Standardmäßig deaktiviert| Standardmäßig aktiviert|
|Token|Token laufen nach einem bestimmten Zeitraum ab und müssen aktualisiert werden.| Nicht verfügbar| Standardmäßig deaktiviert |

Codebeispiele finden Sie im Abschnitt [Webdienstauthentifizierung](how-to-setup-authentication.md#web-service-authentication).

## <a name="authorization"></a>Authorization

Sie können mehrere Arbeitsbereiche erstellen, und jeder Arbeitsbereich kann von mehreren Benutzern gemeinsam genutzt werden. Den Zugriff auf einen freigegebenen Arbeitsbereich können Sie steuern, indem Sie Benutzern die folgenden Rollen zuweisen:

* Besitzer
* Mitwirkender
* Leser

Die folgende Tabelle enthält einige der wichtigsten Azure Machine Learning-Vorgänge und die Rollen, von denen sie ausgeführt werden können:

| Azure Machine Learning-Vorgang | Besitzer | Mitwirkender | Leser |
| ---- |:----:|:----:|:----:|
| Arbeitsbereich erstellen | ✓ | ✓ | |
| Freigeben des Arbeitsbereichs | ✓ | |  |
| Upgraden des Arbeitsbereichs auf Enterprise Edition | ✓ | |
| Erstellen eines Computeziels | ✓ | ✓ | |
| Anfügen eines Computeziels | ✓ | ✓ | |
| Anfügen von Datenspeichern | ✓ | ✓ | |
| Ausführen des Experiments | ✓ | ✓ | |
| Ausführungen/Metriken anzeigen | ✓ | ✓ | ✓ |
| Registrieren des Modells | ✓ | ✓ | |
| Image erstellen | ✓ | ✓ | |
| Bereitstellen des Webdiensts | ✓ | ✓ | |
| Modelle/Images anzeigen | ✓ | ✓ | ✓ |
| Webdienst aufrufen | ✓ | ✓ | ✓ |

Wenn die integrierten Rollen Ihren Anforderungen nicht entsprechen, können Sie benutzerdefinierte Rollen erstellen. Benutzerdefinierte Rollen werden nur für Vorgänge im Arbeitsbereich und Machine Learning Compute unterstützt. Benutzerdefinierte Rollen können Lese-, Schreib- oder Löschberechtigungen für den Arbeitsbereich und die Computeressource in diesem Arbeitsbereich haben. Sie können die Rolle auf einer bestimmten Arbeitsbereichsebene, einer bestimmten Ressourcengruppenebene oder einer bestimmten Abonnementebene verfügbar machen. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf einen Azure Machine Learning-Arbeitsbereich](how-to-assign-roles.md).

> [!WARNING]
> Azure Machine Learning wird mit der Azure Active Directory-Business-to-Business-Zusammenarbeit unterstützt, jedoch derzeit noch nicht mit der Azure Active Directory-Business-to-Consumer-Zusammenarbeit.

### <a name="securing-compute-targets-and-data"></a>Sichern von Computezielen und Daten

Besitzer und Mitwirkende können alle Computeziele und Datenspeicher verwenden, die dem Arbeitsbereich angefügt sind.  

Jedem Arbeitsbereich ist außerdem eine vom System zugewiesene verwaltete Identität zugeordnet, die den gleichen Namen wie der Arbeitsbereich trägt. Die verwaltete Identität verfügt über die folgenden Berechtigungen für angefügte Ressourcen, die im Arbeitsbereich verwendet werden.

Weitere Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

| Resource | Berechtigungen |
| ----- | ----- |
| Arbeitsbereich | Mitwirkender |
| Speicherkonto | Mitwirkender an Storage-Blobdaten |
| Schlüsseltresor | Zugriff auf alle Schlüssel, Geheimnisse und Zertifikate |
| Azure Container Registry | Mitwirkender |
| Ressourcengruppe, die den Arbeitsbereich enthält | Mitwirkender |
| Ressourcengruppe, die den Schlüsseltresor enthält (wenn sie nicht mit der Ressourcengruppe identisch ist, die den Arbeitsbereich enthält) | Mitwirkender |

Administratoren sollten den Zugriff der verwalteten Identität auf die in der vorhergehenden Tabelle genannten Ressourcen nicht widerrufen. Sie können den Zugriff mithilfe des Vorgangs zum erneuten Synchronisieren von Schlüsseln wiederherstellen.

Azure Machine Learning erstellt eine weitere Anwendung (deren Name mit `aml-` oder `Microsoft-AzureML-Support-App-` beginnt) mit Zugriff auf Mitwirkendenebene in Ihrem Abonnement für jede Region des Arbeitsbereichs. Wenn Sie z. B. im selben Abonnement über einen Arbeitsbereich in „USA, Osten“ und einen anderen in „Europa, Norden“ verfügen, sehen Sie zwei dieser Anwendungen. Azure Machine Learning kann Sie mithilfe dieser Anwendungen beim Verwalten der Computeressourcen unterstützen.

## <a name="network-security"></a>Netzwerksicherheit

Azure Machine Learning ist hinsichtlich Computeressourcen auf andere Azure-Dienste angewiesen. Computeressourcen (Computeziele) dienen zum Trainieren und Bereitstellen von Modellen. Sie können diese Computeziele in einem virtuellen Netzwerk erstellen. So können Sie beispielsweise Azure Data Science Virtual Machine verwenden, um ein Modell zu trainieren, und das Modell anschließend in AKS bereitstellen.  

Weitere Informationen finden Sie unter [Sicheres Ausführen von Experimenten und Ziehen von Rückschlüssen innerhalb eines isolierten virtuellen Netzwerks](how-to-enable-virtual-network.md).

Sie können auch Azure Private Link für Ihren Arbeitsbereich aktivieren. Private Link ermöglicht es Ihnen, die Kommunikation aus einem Azure Virtual Network auf Ihren Arbeitsbereich zu beschränken. Weitere Informationen finden Sie unter [Konfigurieren von Private Link](how-to-configure-private-link.md).

## <a name="data-encryption"></a>Datenverschlüsselung

> [!IMPORTANT]
> Für die Verschlüsselung in der Produktionsqualität während des __Trainings__ empfiehlt Microsoft, den Azure Machine Learning-Computecluster zu verwenden. Für die Verschlüsselung in der Produktionsqualität während der __Inferenz__ empfiehlt Microsoft die Verwendung von Azure Kubernetes Service.
>
> Die Azure Machine Learning-Computeinstanz ist eine Dev/Test-Umgebung. Wenn Sie sie verwenden, wird empfohlen, dass Sie Ihre Dateien in einer Dateifreigabe speichern (z. B. Notebooks und Skripte). Ihre Daten sollten in einem Datenspeicher gespeichert werden.

### <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten

> [!IMPORTANT]
> Wenn Ihr Arbeitsbereich vertrauliche Daten enthält, wird empfohlen, das [hbi_workspace-Flag](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-) während der Erstellung Ihres Arbeitsbereichs festzulegen. 

Das Flag `hbi_workspace` kontrolliert die Menge der von Microsoft zu Diagnosezwecken gesammelten Daten und ermöglicht eine zusätzliche Verschlüsselung in von Microsoft verwalteten Umgebungen. Darüber hinaus ermöglicht es folgende Aktionen:

* Startet die Verschlüsselung des lokalen Scratch-Datenträgers in Ihrem Azure Machine Learning-Computecluster, sofern Sie in diesem Abonnement keine vorherigen Cluster erstellt haben. Andernfalls müssen Sie ein Supportticket erstellen, um die Verschlüsselung des Scratch-Datenträgers Ihrer Computecluster zu aktivieren. 
* Bereinigt Ihren lokalen Scratch-Datenträger zwischen den Ausführungen.
* Führt unter Verwendung Ihres Schlüsseltresors die sichere Übergabe der Anmeldeinformationen für Speicherkonto, Containerregistrierung und SSH-Konto von der Ausführungsebene zu Ihren Computeclustern durch.
* Aktiviert IP-Filterung, um sicherzustellen, dass die zugrunde liegenden Batch-Pools nicht von anderen externen Diensten als AzureMachineLearningService aufgerufen werden können.


Weitere Informationen zur Verschlüsselung ruhender Daten in Azure finden Sie unter [Azure-Datenverschlüsselung ruhender Daten](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest).

#### <a name="azure-blob-storage"></a>Azure Blob Storage

Azure Machine Learning speichert Momentaufnahmen, Ausgaben und Protokolle in dem Azure Blob Storage-Konto, das mit dem Azure Machine Learning-Arbeitsbereich und Ihrem Abonnement verknüpft ist. Alle in Azure Blob Storage gespeicherten Daten werden im Ruhezustand mit von Microsoft verwalteten Schlüsseln verschlüsselt.

Informationen dazu, wie Sie Ihre eigenen Schlüssel für die in Azure Blob Storage gespeicherten Daten verwenden können, finden Sie unter [Konfigurieren von Kunden verwalteter Schlüssel für die Azure Storage-Verschlüsselung mithilfe des Azure-Portals](../storage/common/storage-encryption-keys-portal.md).

Trainingsdaten werden in der Regel auch in Azure Blob Storage gespeichert, damit sie zum Trainieren von Computezielen zur Verfügung stehen. Dieser Speicher wird nicht von Azure Machine Learning verwaltet, sondern er wird Computezielen als Remotedateisystem bereitgestellt.

Wenn Sie Ihren Schlüssel __rotieren oder widerrufen__ müssen, können Sie dies jederzeit veranlassen. Wenn ein Schlüssel rotiert wird, beginnt das Speicherkonto, den neuen Schlüssel (neueste Version) zur Verschlüsselung der ruhenden Daten zu verwenden. Wenn ein Schlüssel widerrufen (deaktiviert) wird, kümmert sich das Speicherkonto um fehlgeschlagene Anforderungen. Normalerweise dauert es eine Stunde, bis die Rotation oder der Widerruf wirksam wird.

Informationen zum erneuten Generieren der Zugriffsschlüssel finden Sie unter [Erneutes Generieren von Speicherzugriffsschlüsseln](how-to-change-storage-access-key.md).

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Machine Learning speichert Metriken und Metadaten in einer Azure Cosmos DB-Instanz. Diese Instanz ist mit einem Microsoft-Abonnement verbunden, das von Azure Machine Learning verwaltet wird. Alle in Azure Cosmos DB gespeicherten Daten werden im Ruhezustand mit von Microsoft verwalteten Schlüsseln verschlüsselt.

Um Ihre eigenen (vom Kunden verwalteten) Schlüssel zur Verschlüsselung der Azure-Cosmos-DB-Instanz zu verwenden, können Sie eine dedizierte Cosmos-DB-Instanz zur Verwendung mit Ihrem Arbeitsbereich erstellen. Wir empfehlen diesen Ansatz, wenn Sie Ihre Daten, z. B. Informationen zum Ausführungsverlauf, außerhalb der mehrinstanzenfähigen Cosmos DB-Instanz speichern möchten, die in unserem Microsoft-Abonnement gehostet wird. 

Um die Bereitstellung einer Cosmos DB-Instanz in Ihrem Abonnement mit vom Kunden verwalteten Schlüsseln zu ermöglichen, führen Sie die folgenden Aktionen durch:

* Registrieren Sie die Anbieter von Microsoft.MachineLearning- und Microsoft.DocumentDB-Ressourcen in Ihrem Abonnement, falls dies noch nicht geschehen ist.

* Autorisieren Sie die Machine Learning-App (in der Identitäts- und Zugriffsverwaltung) mit den Berechtigungen für Mitwirkende in Ihrem Abonnement.

    ![Autorisieren Sie die „Azure Machine Learning-App“ in der Identitäts- und Zugriffsverwaltung im Portal](./media/concept-enterprise-security/authorize-azure-machine-learning.png)

* Verwenden Sie die folgenden Parameter, wenn Sie den Azure Machine Learning-Arbeitsbereich erstellen. Beide Parameter sind obligatorisch und werden in SDK-, CLI-, REST-APIs und Resource Manager-Vorlagen unterstützt.

    * `resource_cmk_uri`: Dieser Parameter ist der vollständige Ressourcen-URI des vom Kunden verwalteten Schlüssels in Ihrem Schlüsseltresor, einschließlich der [Versionsinformationen für den Schlüssel](../key-vault/about-keys-secrets-and-certificates.md#objects-identifiers-and-versioning). 

    * `cmk_keyvault`: Dieser Parameter ist die Ressourcen-ID des Schlüsseltresors in Ihrem Abonnement. Dieser Schlüsseltresor muss sich in derselben Region und demselben Abonnement befinden, das Sie für den Azure Machine Learning-Arbeitsbereich verwenden werden. 
    
        > [!NOTE]
        > Diese Schlüsseltresorinstanz kann sich von dem Schlüsseltresor unterscheiden, der von Azure Machine Learning beim Bereitstellen des Arbeitsbereichs erstellt wird. Wenn Sie dieselbe Schlüsseltresorinstanz für den Arbeitsbereich verwenden möchten, übergeben Sie denselben Schlüsselspeicher beim Bereitstellen des Arbeitsbereichs mithilfe des [key_vault-Parameters](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-). 

Diese Cosmos DB-Instanz wird in einer von Microsoft verwalteten Ressourcengruppe in Ihrem Abonnement erstellt. Die verwaltete Ressourcengruppe wird im Format `<AML Workspace Resource Group Name><GUID>` benannt.

> [!IMPORTANT]
> * Wenn Sie diese Cosmos DB-Instanz löschen müssen, müssen Sie den Azure Machine Learning-Arbeitsbereich löschen, der diese verwendet. 
> * Die standardmäßigen [__Anforderungseinheiten__](../cosmos-db/request-units.md) für dieses Cosmos-DB-Konto sind auf __8000__ festgelegt. Die Änderung dieses Werts wird nicht unterstützt. 

Wenn Sie Ihren Schlüssel __rotieren oder widerrufen__ müssen, können Sie dies jederzeit veranlassen. Wenn ein Schlüssel rotiert wird, beginnt Cosmos DB, den neuen Schlüssel (neueste Version) zur Verschlüsselung der ruhenden Daten zu verwenden. Wenn ein Schlüssel widerrufen (deaktiviert) wird, kümmert sich Cosmos DB um fehlgeschlagene Anforderungen. Normalerweise dauert es eine Stunde, bis die Rotation oder der Widerruf wirksam wird.

Weitere Informationen zu von Kunden verwalteten Schlüsseln mit Cosmos DB finden Sie unter [Konfigurieren von kundenseitig verwalteten Schlüsseln für Ihr Azure Cosmos DB-Konto](../cosmos-db/how-to-setup-cmk.md).

#### <a name="azure-container-registry"></a>Azure Container Registry

Alle Containerimages in Ihrer Registrierung (Azure Container Registry) werden im Ruhezustand verschlüsselt. Azure verschlüsselt ein Image automatisch vor dessen Speicherung, und entschlüsselt es, wenn Azure Machine Learning das Image herunterlädt (pullt).

Um Ihre eigenen (vom Kunden verwalteten) Schlüssel zur Verschlüsselung Ihrer Azure Container Registry zu verwenden, müssen Sie Ihre eigene ACR erstellen und diese während der Bereitstellung des Arbeitsbereichs anfügen oder die Standardinstanz verschlüsseln, die zum Zeitpunkt der Arbeitsbereichsbereitstellung erstellt wird.

> [!IMPORTANT]
> Azure Machine Learning erfordert, dass das Administratorkonto in Ihrer Azure Container Registry aktiviert ist. Diese Einstellung ist standardmäßig deaktiviert, wenn Sie eine Containerregistrierung erstellen. Informationen zum Aktivieren des Administratorkontos finden Sie unter [Administratorkonto](/azure/container-registry/container-registry-authentication#admin-account).
>
> Nachdem eine Azure Container Registry für einen Arbeitsbereich erstellt wurde, dürfen Sie diese nicht mehr löschen. Dadurch wird Ihr Azure Machine Learning-Arbeitsbereich beschädigt.

Ein Beispiel für die Erstellung eines Arbeitsbereichs unter Verwendung einer bestehenden Azure Container Registry finden Sie in den folgenden Artikeln:

* [Erstellen eines Arbeitsbereichs für Azure Machine Learning mit der Azure CLI](how-to-manage-workspace-cli.md).
* [Verwenden einer Azure Resource Manager-Vorlage zum Erstellen eines Arbeitsbereichs für Azure Machine Learning](how-to-create-workspace-template.md)

#### <a name="azure-container-instance"></a>Azure Container Instances

Sie können eine bereitgestellte ACI-Ressource (Azure Container Instances) mit vom Kunden verwalteten Schlüsseln verschlüsseln. Der für ACI verwendete vom Kunden verwaltete Schlüssel kann in Azure Key Vault für Ihren Arbeitsbereich gespeichert werden. Informationen zum Generieren eines Schlüssels finden Sie unter [Verschlüsseln von Daten mit einem kundenseitig verwalteten Schlüssel](../container-instances/container-instances-encrypt-data.md#generate-a-new-key).

Erstellen Sie zur Verwendung des Schlüssels bei der Bereitstellung eines Modells in Azure Container Instance eine neue Bereitstellungskonfiguration mit `AciWebservice.deploy_configuration()`. Geben Sie die Schlüsselinformationen unter Verwendung der folgenden Parameter an:

* `cmk_vault_base_url`: Die URL des Schlüsseltresors, der den Schlüssel enthält.
* `cmk_key_name`: Der Name des Schlüssels.
* `cmk_key_version`: Die Version des Schlüssels.

Weitere Informationen zum Erstellen und Verwenden einer Bereitstellungskonfiguration finden Sie in den folgenden Artikeln:

* [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none-)-Referenz
* [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md)
* [Bereitstellen eines Modells in Azure Container Instances](how-to-deploy-azure-container-instance.md)

Weitere Informationen zur Verwendung eines vom Kunden verwalteten Schlüssels mit ACI finden Sie unter [Verschlüsseln von Daten mit einem kundenseitig verwalteten Schlüssel](../container-instances/container-instances-encrypt-data.md#encrypt-data-with-a-customer-managed-key).

#### <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Sie können eine bereitgestellte Ressource des Azure Kubernetes Service jederzeit mit vom Kunden verwalteten Schlüsseln verschlüsseln. Weitere Informationen finden Sie unter [Bring Your Own Keys (BYOK) mit Azure Kubernetes Service (AKS)](../aks/azure-disk-customer-managed-keys.md). 

Dieser Prozess ermöglicht es Ihnen, sowohl die Daten als auch den Betriebssystemdatenträger der bereitgestellten virtuellen Computer im Kubernetes-Cluster zu verschlüsseln.

> [!IMPORTANT]
> Dieser Prozess funktioniert nur mit AKS K8s Version 1.17 oder höher. Azure Machine Learning hat am 13. Januar 2020 die Unterstützung für AKS 1.17 hinzugefügt.

#### <a name="machine-learning-compute"></a>Machine Learning Compute

Der Betriebssystem-Datenträger für jeden in Azure Storage gespeicherten Computeknoten wird mit von Microsoft verwalteten Schlüsseln in Speicherkonten von Azure Machine Learning verschlüsselt. Dieses Computeziel ist kurzlebig, und Cluster werden in der Regel zentral herunterskaliert, wenn keine Ausführungen in der Warteschlange stehen. Die Bereitstellung des zugrunde liegenden virtuellen Computers wird aufgehoben, und der Betriebssystem-Datenträger wird gelöscht. Azure Disk Encryption wird für den Betriebssystem-Datenträger nicht unterstützt.

Jeder virtuelle Computer verfügt auch über einen lokalen temporären Datenträger für Betriebssystem-Vorgänge. Wenn Sie möchten, können Sie den Datenträger zum Bereitstellen von Trainingsdaten verwenden. Der Datenträger wird standardmäßig für Arbeitsbereiche mit dem Parameter `hbi_workspace` verschlüsselt, der auf `TRUE` festgelegt ist. Diese Umgebung ist auf die Dauer Ihrer Ausführung befristet, und die Unterstützung für die Verschlüsselung beschränkt sich auf vom System verwaltete Schlüssel.

#### <a name="azure-databricks"></a>Azure Databricks

Azure Databricks kann in Azure Machine Learning-Pipelines verwendet werden. Das von Azure Databricks verwendete Databricks-Dateisystem (Databricks File System, DBFS) wird standardmäßig mithilfe eines von Microsoft verwalteten Schlüssels verschlüsselt. Informationen zum Konfigurieren von Azure Databricks für die Verwendung von kundenseitig verwalteten Schlüsseln finden Sie unter [Konfigurieren der kundenseitig verwalteten Schlüsseln im Standard-DBFS (Stamm)](/azure/databricks/security/customer-managed-keys-dbfs).

### <a name="encryption-in-transit"></a>Verschlüsselung während der Übertragung

Azure Machine Learning verwendet TLS zum Sichern der internen Kommunikation zwischen Azure Machine Learning-Microservices. Der gesamte Azure Storage-Zugriff erfolgt ebenfalls über einen sicheren Kanal.

Zum Sichern externer Aufrufe an den Bewertungsendpunkt verwendet Azure Machine Learning TLS. Weitere Informationen finden Sie unter [Verwenden von TLS zum Absichern eines Webdiensts mit Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/how-to-secure-web-service).

### <a name="using-azure-key-vault"></a>Verwenden von Azure Key Vault

Azure Machine Learning verwendet die mit dem Arbeitsbereich verknüpfte Azure Key Vault-Instanz zum Speichern von Anmeldeinformationen verschiedener Art:

* Der verknüpften Speicherkonto-Verbindungszeichenfolge
* Kennwörter in Azure Container Repository-Instanzen
* Verbindungszeichenfolgen zur Verbindung mit Datenspeichern.

SSH-Kennwörter und Schlüssel für Computeziele wie Azure HDInsight und virtuelle Computer werden in einem separaten Schlüsseltresor gespeichert, der dem Microsoft-Abonnement zugeordnet ist. Azure Machine Learning speichert weder Kennwörter noch Schlüssel, die von Benutzern bereitgestellt wurden. Stattdessen werden eigene SSH-Schlüssel generiert, autorisiert und gespeichert, um eine Verbindung mit virtuellen Computern und HDInsight herzustellen und die Experimente auszuführen.

Jedem Arbeitsbereich ist eine vom System zugewiesene verwaltete Identität zugeordnet, die den gleichen Namen hat wie der Arbeitsbereich. Diese verwaltete Identität verfügt über Zugriff auf alle Schlüssel, Geheimnisse und Zertifikate im Schlüsseltresor.

## <a name="data-collection-and-handling"></a>Datensammlung und -verarbeitung

### <a name="microsoft-collected-data"></a>Von Microsoft gesammelte Daten

Microsoft sammelt möglicherweise Informationen, z. B. Ressourcennamen (wie den Datasetnamen oder den Namen des Machine Learning-Experiments) oder Variablen von Auftragsumgebungen zu Diagnosezwecken, die nicht den Benutzer identifizieren. Alle diese Daten werden mit von Microsoft verwalteten Schlüsseln in einem Speicher gespeichert, der in Microsoft-eigenen Abonnements gehostet wird, und folgen den [Standarddatenschutzrichtlinien und Datenverarbeitungsstandards von Microsoft](https://privacy.microsoft.com/privacystatement).

Microsoft empfiehlt außerdem, keine vertraulichen Informationen (z. B. Geheimnisse von Kontoschlüsseln) in Umgebungsvariablen zu speichern. Umgebungsvariablen werden von uns protokolliert, verschlüsselt und gespeichert. Vermeiden Sie beim Benennen von [run_id](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) daher vertrauliche Informationen wie Benutzernamen oder geheime Projektnamen. Diese Informationen können in Telemetrieprotokollen enthalten sein, auf die Microsoft-Supporttechniker zugreifen können.

Sie können sich von der Erfassung von Diagnosedaten abmelden, indem Sie den Parameter `hbi_workspace` während der Bereitstellung des Arbeitsbereichs auf `TRUE` festlegen. Diese Funktionalität wird unterstützt, wenn AzureML Python-SDK-, CLI-, REST-APIs oder Azure Resource Manager-Vorlagen verwendet werden.

### <a name="microsoft-generated-data"></a>Von Microsoft generierte Daten

Bei der Verwendung von Diensten wie automatisiertes ML generiert Microsoft möglicherweise vorübergehende, vorverarbeitete Daten für das Training mehrerer Modelle. Diese Daten werden in einem Datenspeicher in Ihrem Arbeitsbereich gespeichert, der es Ihnen ermöglicht, die Zugriffssteuerung und Verschlüsselung entsprechend durchzusetzen.

Möglicherweise möchten Sie auch [Diagnoseinformationen verschlüsseln, die von Ihrem bereitgestellten Endpunkt](how-to-enable-app-insights.md) in Ihrer Azure Application Insights-Instanz protokolliert werden.

## <a name="monitoring"></a>Überwachung

### <a name="metrics"></a>Metriken

Sie können Azure Monitor-Metriken verwenden, um Metriken für Ihren Azure Machine Learning-Arbeitsbereich anzuzeigen und zu überwachen. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren Arbeitsbereich und anschließend **Metriken** aus:

[![Screenshot, der Beispielmetriken für einen Arbeitsbereich zeigt](media/concept-enterprise-security/workspace-metrics.png)](media/concept-enterprise-security/workspace-metrics-expanded.png#lightbox)

Die Metriken umfassen Informationen zu Ausführungen, Bereitstellungen und Registrierungen.

Weitere Informationen finden Sie unter [Azure Monitor-Metriken](/azure/azure-monitor/platform/data-platform-metrics).

### <a name="activity-log"></a>Aktivitätsprotokoll

Im Aktivitätsprotokoll eines Arbeitsbereichs können Sie verschiedene Vorgänge anzeigen, die im Arbeitsbereich ausgeführt werden. Das Protokoll enthält grundlegende Informationen wie den Vorgangsnamen, den Ereignisinitiator und den Zeitstempel.

Der folgende Screenshot zeigt das Aktivitätsprotokoll eines Arbeitsbereichs:

[![Screenshot, der das Aktivitätsprotokoll eines Arbeitsbereichs zeigt](media/concept-enterprise-security/workspace-activity-log.png)](media/concept-enterprise-security/workspace-activity-log-expanded.png#lightbox)

Details der Bewertungsanforderung werden in Application Insights gespeichert. Application Insights wird in Ihrem Abonnement erstellt, wenn Sie einen Arbeitsbereich erstellen. Zu den protokollierten Informationen zählen Felder wie:

* HTTPMethod
* UserAgent
* ComputeType
* RequestUrl
* StatusCode
* RequestId
* Duration

> [!IMPORTANT]
> Einige Aktionen im Azure Machine Learning-Arbeitsbereich protokollieren keine Informationen im Aktivitätsprotokoll. Der Start einer Trainingsausführung und die Registrierung eines Modells werden beispielsweise nicht protokolliert.
>
> Einige dieser Aktionen werden im Bereich **Aktivitäten** Ihres Arbeitsbereichs angezeigt, diese Benachrichtigungen geben jedoch nicht an, wer die Aktivität initiiert hat.

## <a name="data-flow-diagrams"></a>Datenflussdiagramme

### <a name="create-workspace"></a>Arbeitsbereich erstellen

Das folgende Diagramm zeigt den Workflow des Erstellens des Arbeitsbereichs.

* Sie melden sich von einem der unterstützten Azure Machine Learning-Clients (Azure CLI, Python, SDK, Azure-Portal) aus bei Azure AD an und fordern das entsprechende Azure Resource Manager-Token an.
* Sie rufen Azure Resource Manager auf, um den Arbeitsbereich zu erstellen. 
* Der Azure Resource Manager fordert den Azure Machine Learning-Ressourcenanbieter auf, den Arbeitsbereich bereitzustellen.

Zusätzliche Ressourcen werden bei der Erstellung des Arbeitsbereichs im Abonnement des Benutzers erstellt:

* Key Vault (zum Speichern von Geheimnissen)
* Ein Azure Storage-Konto (einschließlich Blob und Dateifreigabe)
* Azure Container Registry (zum Speichern von Docker-Images für Rückschlüsse/Bewertungen und Experimente)
* Application Insights (zum Speichern von Telemetriedaten)

Der Benutzer kann bei Bedarf auch andere Computeziele bereitstellen, die an einen Arbeitsbereich angefügt sind (wie Azure Kubernetes Service oder VMs).

[![Erstellen eines Arbeitsbereichs-Workflows](media/concept-enterprise-security/create-workspace.png)](media/concept-enterprise-security/create-workspace-expanded.png#lightbox)

### <a name="save-source-code-training-scripts"></a>Speichern des Quellcodes (Trainingsskripts)

Das folgende Diagramm zeigt den Workflow der Codemomentaufnahme.

Einem Azure Machine Learning-Arbeitsbereich sind Verzeichnisse (Experimente) zugeordnet, die den Quellcode (Trainingsskripts) enthalten. Diese Skripts werden auf Ihrem lokalen Computer und in der Cloud (in Azure Blob Storage für Ihr Abonnement) gespeichert. Die Codemomentaufnahmen werden für die Ausführung oder Überprüfung für die Verlaufsüberwachung verwendet.

[![Workflow der Codemomentaufnahme](media/concept-enterprise-security/code-snapshot.png)](media/concept-enterprise-security/code-snapshot-expanded.png#lightbox)

### <a name="training"></a>Training

Das folgende Diagramm zeigt den Trainingsworkflow.

* Azure Machine Learning wird mit der Momentaufnahmen-ID für die im vorherigen Abschnitt gespeicherte Codemomentaufnahme aufgerufen.
* Azure Machine Learning erstellt eine Ausführungs-ID (optional) und ein Azure Machine Learning-Token, das später von Computezielen wie Machine Learning Compute/VMs für die Kommunikation mit Machine Learning verwendet wird.
* Sie können entweder ein verwaltetes Computeziel (wie Machine Learning Compute) oder ein nicht verwaltetes Computeziel (wie VMs) auswählen, um Trainingsaufträge auszuführen. Im Folgenden finden Sie die Datenflüsse für beide Szenarien:
   * VMs/HDInsight, Zugriff erfolgt über SSH-Anmeldeinformationen in einem Schlüsseltresor im Microsoft-Abonnement. Azure Machine Learning führt verwalteten Code auf dem Computeziel aus, der die folgenden Aktionen ausführt:

   1. Vorbereiten der Umgebung (Docker ist eine Option für VMs und lokale Computer. In den folgenden Schritten für Machine Learning Compute erfahren Sie, wie die Ausführung eines Experiments in Docker-Containern funktioniert.)
   1. Herunterladen des Codes
   1. Einrichten von Umgebungsvariablen und Konfigurationen
   1. Ausführen von Benutzerskripts (die im vorherigen Abschnitt genannte Codemomentaufnahme)

   * Machine Learning Compute, Zugriff erfolgt über eine vom Arbeitsbereich verwaltete Identität.
Da Machine Learning Compute ein verwaltetes Computeziel ist (d. h. es wird von Microsoft verwaltet), wird es unter Ihrem Microsoft-Abonnement ausgeführt.

   1. Starten der Docker-Remotekonstruktion (sofern erforderlich)
   1. Schreiben des Verwaltungscodes in die Azure Files-Freigabe des Benutzers
   1. Starten des Containers mit einem ersten Befehl. Dies ist der Verwaltungscode, der im vorherigen Schritt beschrieben wurde.

#### <a name="querying-runs-and-metrics"></a>Abfragen von Ausführungen und Metriken

Im folgenden Flussdiagramm wird dieser Schritt ausgeführt, wenn das Trainingscomputeziel die Ausführungsmetriken aus dem Speicher in der Cosmos DB-Datenbank an Azure Machine Learning zurückschreibt. Clients können Azure Machine Learning aufrufen. Machine Learning pullt wiederum die Metriken aus der Cosmos DB-Datenbank und gibt sie an den Client zurück.

[![Trainingsworkflow](media/concept-enterprise-security/training-and-metrics.png)](media/concept-enterprise-security/training-and-metrics-expanded.png#lightbox)

### <a name="creating-web-services"></a>Erstellen von Webdiensten

Das folgende Diagramm zeigt den Rückschlussworkflow. Rückschlüsse oder Modellbewertungen stellen die Phase dar, in der das bereitgestellte Modell für die Vorhersage verwendet wird (meist für Produktionsdaten).

Es folgen die Details:

* Der Benutzer registriert ein Modell, indem er einen Client wie das Azure Machine Learning-SDK verwendet.
* Der Benutzer erstellt das Image mit einem Modell, einer Bewertungsdatei und anderen Modellabhängigkeiten.
* Das Docker-Image wird erstellt und in Azure Container Registry gespeichert.
* Der Webdienst wird auf dem Computeziel (Container Instances/AKS) mithilfe des Images bereitgestellt, das im vorherigen Schritt erstellt wurde.
* Details der Bewertung werden in Application Insights gespeichert (im Abonnement des Benutzers enthalten).
* Telemetriedaten werden ebenfalls per Push an das Microsoft/Azure-Abonnement übertragen.

[![Rückschlussworkflow](media/concept-enterprise-security/inferencing.png)](media/concept-enterprise-security/inferencing-expanded.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

* [Sichere Azure Machine Learning-Webdienste mit TLS](how-to-secure-web-service.md)
* [Nutzen eines als Webdienst bereitgestellten Machine Learning-Modells](how-to-consume-web-service.md)
* [Verwenden von Azure Machine Learning hinter Azure Firewall](how-to-access-azureml-behind-firewall.md)
* [Verwenden von Azure Machine Learning mit einem virtuellen Azure-Netzwerk](how-to-enable-virtual-network.md)
* [Bewährte Methoden für das Erstellen von Empfehlungssystemen](https://github.com/Microsoft/Recommenders)
* [Erstellen einer Echtzeitempfehlungs-API in Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
