---
title: Verbessern der Resilienz
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie die Resilienz Ihrer Azure Machine Learning-Ressourcen gegenüber Ausfällen mithilfe einer Hochverfügbarkeitskonfiguration verbessern.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jhirono
author: jhirono
ms.reviewer: larryfr
ms.date: 07/16/2020
ms.openlocfilehash: 4f2bf239e1157f5c927c4449857ad5f7793ccb49
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87094924"
---
# <a name="increase-the-resiliency-of-azure-machine-learning"></a>Verbessern der Resilienz von Azure Machine Learning

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Hier erfahren Sie, wie Sie die Resilienz Ihrer Azure Machine Learning-Ressourcen mithilfe einer Hochverfügbarkeitskonfiguration verbessern. Die Azure-Dienste, von denen Azure Machine Learning abhängig ist, können für Hochverfügbarkeit konfiguriert werden. Dieser Artikel enthält Informationen dazu, welche Dienste für Hochverfügbarkeit konfiguriert werden können, und Links, unter denen Sie Informationen zum Konfigurieren dieser Ressourcen finden.

> [!NOTE]
> Azure Machine Learning selbst bietet keine Notfallwiederherstellung.

## <a name="understand-azure-services-for-azure-machine-learning"></a>Grundlegendes zu Azure-Diensten für Azure Machine Learning

Azure Machine Learning ist von mehreren Azure-Diensten abhängig und verfügt über mehrere Ebenen. Einige davon werden in Ihrem Abonnement (Kundenabonnement) bereitgestellt. Sie sind für die Hochverfügbarkeitskonfiguration dieser Dienste verantwortlich. Einige der Dienste werden in einem Microsoft-Abonnement erstellt und von Microsoft verwaltet.

* **Azure Machine Learning-Infrastruktur**: Eine von Microsoft verwaltete Umgebung für den Azure Machine Learning-Arbeitsbereich.

* **Zugeordnete Ressourcen**: Ressourcen, die während der Erstellung des Azure Machine Learning-Arbeitsbereichs in Ihrem Abonnement bereitgestellt werden. Dazu zählen Azure Storage, Azure Key Vault, Azure Container Registry (ACR) und Application Insights. Sie sind für die Hochverfügbarkeitseinstellungen dieser Ressourcen verantwortlich.
  * Standardspeicher enthält Daten wie etwa Modelle, Trainingsprotokolldaten und Datasets.
  * Key Vault enthält Anmeldeinformationen für Speicher, ACR und Datenspeicher.
  * ACR enthält ein Docker-Image für die Trainings- und Rückschlussumgebung.
  * Application Insights dient zum Überwachen von Azure Machine Learning.

* **Computeressourcen**: Ressourcen, die Sie nach der Bereitstellung des Arbeitsbereichs erstellen. Beispielsweise können Sie eine Compute-Instanz oder einen Computecluster erstellen, um ein Machine Learning-Modell zu trainieren.
  * Compute-Instanz und Computecluster: Eine von Microsoft verwaltete Umgebung für die Modellentwicklung.
  * Weitere Ressourcen: Computingressourcen, die an Azure Machine Learning angefügt werden können, z. B. Azure Kubernetes Service (AKS), Azure Databricks, Azure Container Instances (ACI) und HDInsight. Sie sind für die Hochverfügbarkeitseinstellung verantwortlich.

* **Zusätzliche Datenspeicher**: Azure Machine Learning kann für Trainingsdaten zusätzliche Datenspeicher einbinden, z. B. Azure Storage, Azure Data Lake Storage und Azure SQL-Datenbank.  Diese Datenspeicher befinden sich in Ihrem Abonnement, und Sie sind verantwortlich für die Hochverfügbarkeitseinstellung.

Die folgende Tabelle zeigt, von wem die Dienste verwaltet werden (von Microsoft oder Ihnen) und welche Dienste standardmäßig hoch verfügbar sind:

| Dienst | Verwaltet von | Standardmäßig hoch verfügbar |
| ----- | ----- | ----- |
| **Azure Machine Learning-Infrastruktur** | Microsoft | |
| **Zugeordnete Ressourcen** |
| Azure Storage | Sie | |
| Azure-Schlüsseltresor | Sie | ✓ |
| Azure Container Registry | Sie | |
| Application Insights | Sie | Nicht verfügbar |
| **Computeressourcen** |
| Compute-Instanz | Microsoft |  |
| Computecluster | Microsoft |  |
| Andere Ressourcen wie Azure Kubernetes Service, <br>Azure Databricks, Azure Container Instances und Azure HDInsight | Sie |  |
| **Zusätzliche Datenspeicher** wie Azure Storage, Azure SQL-Datenbank,<br> Azure Database for PostgreSQL und Azure Database for MySQL und <br>Azure Databricks-Dateisystem | Sie | |

In den verbleibenden Abschnitten dieses Artikels finden Sie Informationen dazu, welche Aktionen erforderlich sind, um die einzelnen Dienste hoch verfügbar zu machen.

## <a name="associated-resources"></a>Zugeordnete Ressourcen

> [!IMPORTANT]
> Azure Machine Learning unterstützt kein Failover des Standardspeicherkontos mit georedundantem Speicher (GRS), geozonenredundantem Speicher (GZRS), georedundantem Speicher mit Lesezugriff (RA-GRS) oder geozonenredundantem Speicher mit Lesezugriff (RA-GZRS).

Halten Sie sich für die Hochverfügbarkeitseinstellung der einzelnen Ressourcen an die folgenden Informationen.

* **Azure Storage**: Informationen zum Konfigurieren der Hochverfügbarkeitseinstellung finden Sie unter [Azure Storage-Redundanz](https://docs.microsoft.com/azure/storage/common/storage-redundancy).
* **Azure Key Vault**: Dieser Dienst bietet standardmäßig Hochverfügbarkeit, und es ist keine Benutzeraktion erforderlich.  Weitere Informationen finden Sie unter [Azure Key Vault: Verfügbarkeit und Redundanz](https://docs.microsoft.com/azure/key-vault/general/disaster-recovery-guidance).
* **Azure Container Registry**: Wählen Sie die Premium-SKU für Georeplikation aus. Weitere Informationen finden Sie unter [Georeplikation in Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).
* **Application Insights**: Dieser Dienst bietet keine Hochverfügbarkeitseinstellung. Sie können den Datenaufbewahrungszeitraum optimieren. Details finden Sie unter [Datensammlung, -aufbewahrung und -speicherung in Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/data-retention-privacy#how-long-is-the-data-kept).

## <a name="compute-resources"></a>Computeressourcen

Halten Sie sich für die Hochverfügbarkeitseinstellung der einzelnen Ressourcen an die unten genannten Dokumente.

* **Azure Kubernetes Service:** Siehe [Best Practices für Geschäftskontinuität und Notfallwiederherstellung in Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region) und [Erstellen eines Azure Kubernetes Service-Clusters (AKS), der Verfügbarkeitszonen verwendet](https://docs.microsoft.com/azure/aks/availability-zones). Wenn der AKS-Cluster über Azure Machine Learning erstellt wurde (mit Studio, dem SDK oder der ML CLI), wird regionsübergreifende Hochverfügbarkeit nicht unterstützt.
* **Azure Databricks**: Siehe [Regionale Notfallwiederherstellung für Azure Databricks-Cluster](https://docs.microsoft.com/azure/azure-databricks/howto-regional-disaster-recovery).
* **Azure Container Instances**: Der ACI-Orchestrator ist für Failover verantwortlich. Siehe [Azure Container Instances und Containerorchestratoren](https://docs.microsoft.com/azure/container-instances/container-instances-orchestrator-relationship).
* **Azure HDInsight:** Siehe [Von Azure HDInsight unterstützte Hochverfügbarkeitsdienste](https://docs.microsoft.com/azure/hdinsight/hdinsight-high-availability-components).

## <a name="additional-data-stores"></a>Zusätzliche Datenspeicher

Halten Sie sich für die Hochverfügbarkeitseinstellung der einzelnen Ressourcen an die unten genannten Dokumente.

* **Azure-Blobcontainer/Azure-Dateifreigabe/Azure Data Lake Gen2**: Gleiche Vorgehensweise wie bei Standardspeicher.
* **Azure Data Lake Gen1**: Siehe [Leitfaden zur Notfallwiederherstellung für Daten in Azure Data Lake Storage Gen1](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-disaster-recovery-guidance).
* **Azure SQL-Datenbank**: Informationen finden Sie unter [Hochverfügbarkeit und Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-high-availability).
* **Azure Database for PostgreSQL**: Siehe [Hochverfügbarkeitskonzepte von Azure Database for PostgreSQL – Einzelserver](https://docs.microsoft.com/azure/postgresql/concepts-high-availability).
* **Azure Database for MySQL**: Siehe [Informationen zur Geschäftskontinuität in Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/concepts-business-continuity).
* **Databricks-Dateisystem**: Siehe [Regionale Notfallwiederherstellung für Azure Databricks-Cluster](https://docs.microsoft.com/azure/azure-databricks/howto-regional-disaster-recovery).

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

Wenn Sie Ihren eigenen Schlüssel (kundenseitig verwalteter Schlüssel) zum Bereitstellen des Azure Machine Learning-Arbeitsbereichs angeben, wird Cosmos DB ebenfalls in Ihrem Abonnement bereitgestellt. In diesem Fall sind Sie für die Hochverfügbarkeitseinstellung verantwortlich. Siehe [Hochverfügbarkeit mit Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/high-availability).

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie eine [Azure Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced), um Azure Machine Learning mit zugeordneten Ressourcen mit Ihren Hochverfügbarkeitseinstellungen bereitzustellen.