---
title: Was ist Azure Private Link?
description: Übersicht über Features, Architektur und Implementierung von Azure Private Link. Es wird beschrieben, wie private Azure-Endpunkte und der Dienst „Azure Private Link“ funktionieren und wie Sie diese Features nutzen.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: overview
ms.date: 06/02/2020
ms.author: allensu
ms.custom: fasttrack-edit
ms.openlocfilehash: ec64facd554ca32ddc1357d9b92b08f1d844e377
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84673410"
---
# <a name="what-is-azure-private-link"></a>Was ist Azure Private Link? 
Mit Azure Private Link können Sie über einen [privaten Endpunkt](private-endpoint-overview.md) in Ihrem virtuellen Netzwerk auf Azure-PaaS-Dienste (beispielsweise Azure Storage und SQL Database) sowie auf in Azure gehostete kundeneigene Dienste/Partnerdienste zugreifen.

Der Datenverkehr zwischen Ihrem virtuellen Netzwerk und dem Dienst verläuft über das Microsoft-Backbone-Netzwerk. Es ist nicht mehr erforderlich, dass Sie Ihren Dienst über das öffentliche Internet verfügbar machen. Sie können Ihren eigenen [Private Link-Dienst](private-link-service-overview.md) in Ihrem virtuellen Netzwerk erstellen und Ihren Kunden zur Verfügung stellen. Die Einrichtung und Nutzung von Azure Private Link ist in Azure PaaS-, Kunden- und gemeinsam genutzten Partnerdiensten einheitlich.

> [!IMPORTANT]
> Azure Private Link ist jetzt allgemein verfügbar. Sowohl der Dienst für private Endpunkte als auch der Dienst „Private Link“ (Dienst im Hintergrund von Load Balancer Standard) ist allgemein verfügbar. Für unterschiedliche Azure PaaS-Instanzen wird das Onboarding für Azure Private Link basierend auf verschiedenen Zeitplänen durchgeführt. Der Abschnitt [Verfügbarkeit](https://docs.microsoft.com/azure/private-link/private-link-overview#availability) weiter unten enthält Informationen zum genauen Status von Azure PaaS unter Private Link. Weitere Informationen zu bekannten Einschränkungen finden Sie unter [Privater Endpunkt](private-endpoint-overview.md#limitations) und [Private Link-Dienst](private-link-service-overview.md#limitations). 

![Privater Endpunkt – Übersicht](media/private-link-overview/private-endpoint.png)

## <a name="key-benefits"></a>Hauptvorteile
Azure Private Link bietet folgende Vorteile:  
- **Private Zugriffsdienste auf der Azure-Plattform**: Verbinden Sie Ihr virtuelles Netzwerk mit Diensten in Azure ohne öffentliche IP-Adresse an der Quelle oder am Ziel. Dienstanbieter können ihre Dienste in ihrem eigenen virtuellen Netzwerk rendern, und Consumer können in ihrem lokalen virtuellen Netzwerk auf diese Dienste zugreifen. Die Private Link-Plattform verarbeitet die Konnektivität zwischen dem Consumer und den Diensten über das Azure-Backbone-Netzwerk. 
 
- **Lokale Netzwerke und Peernetzwerke**: Greifen Sie mithilfe privater Endpunkte von einer lokalen Umgebung über privates ExpressRoute-Peering, VPN-Tunnel und virtuelle Netzwerke mit Peering auf Dienste zu, die in Azure ausgeführt werden. Es ist nicht erforderlich, öffentliches Peering einzurichten oder über das Internet auf den Dienst zuzugreifen. Private Link ist eine sichere Möglichkeit, um Workloads zu Azure zu migrieren.
 
- **Schutz vor Datenlecks**: Ein privater Endpunkt wird nicht dem gesamten Dienst, sondern der Instanz einer PaaS-Ressource zugeordnet. Consumer können nur eine Verbindung mit der entsprechenden Ressource herstellen. Der Zugriff auf alle anderen Ressourcen des Diensts ist blockiert. Dieser Mechanismus ermöglicht den Schutz vor Risiken aufgrund von Datenlecks. 
 
- **Globale Reichweite**: Stellen Sie private Verbindungen zu Diensten her, die in anderen Regionen ausgeführt werden. Das virtuelle Netzwerk des Consumers kann sich beispielsweise in Region A befinden und eine Verbindung mit Diensten hinter Private Link in Region B herstellen.  
 
- **Ausweiten auf Ihre eigenen Dienste**: Aktivieren Sie die gleichen Funktionen, um Ihren Dienst privat für Consumer in Azure zu rendern. Wenn Sie Ihren Dienst hinter einem Azure Load Balancer Standard platzieren, können Sie ihn für Private Link aktivieren. Der Consumer kann dann mithilfe eines privaten Endpunkts in seinem eigenen virtuellen Netzwerk eine direkte Verbindung mit dem Dienst herstellen. Die Verbindungsanforderungen können mithilfe eines Ablaufs für Genehmigungsaufrufe verwaltet werden. Azure Private Link funktioniert für Consumer und Dienste, die zu unterschiedlichen Azure Active Directory-Mandanten gehören. 

## <a name="availability"></a>Verfügbarkeit 
 In der folgenden Tabelle sind die Private Link-Dienste und die Regionen, in denen sie verfügbar sind, aufgelistet. 

|Unterstützte Dienste  |Verfügbare Regionen | Status  |
|:-------------------|:-----------------|:--------|
|Private Link-Dienste hinter Azure Load Balancer Standard | Alle öffentlichen Regionen  | Allgemein verfügbar <br/> [Weitere Informationen](https://docs.microsoft.com/azure/private-link/private-link-service-overview) |
| Azure Storage        |  Alle öffentlichen Regionen       | Allgemein verfügbar <br/> [Weitere Informationen](/azure/storage/common/storage-private-endpoints)  |
| Azure Data Lake Storage Gen2        |  Alle öffentlichen Regionen      | Allgemein verfügbar <br/> [Weitere Informationen](/azure/storage/common/storage-private-endpoints)  |
|  Azure SQL-Datenbank         | Alle öffentlichen Regionen      |   Allgemein verfügbar <br/> [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview)      |
|Azure Synapse Analytics (SQL Data Warehouse)| Alle öffentlichen Regionen |Allgemein verfügbar <br/> [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview)|
|Azure Cosmos DB|  Alle öffentlichen Regionen |Allgemein verfügbar <br/> [Weitere Informationen](https://docs.microsoft.com/azure/cosmos-db/how-to-configure-private-endpoints)|
|  Azure Database for PostgreSQL (Einzelserver)         | Alle öffentlichen Regionen      |   Allgemein verfügbar <br/> [Weitere Informationen](https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-private-link)      |
|  Azure Database for MySQL         | Alle öffentlichen Regionen      |   Allgemein verfügbar <br/> [Weitere Informationen](https://docs.microsoft.com/azure/mysql/concepts-data-access-security-private-link)     |
|  Azure Database for MariaDB         | Alle öffentlichen Regionen      |   Allgemein verfügbar <br/> [Weitere Informationen](https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-private-link)      |
|  Azure-Schlüsseltresor         | Alle öffentlichen Regionen      |   Allgemein verfügbar   <br/> [Weitere Informationen](https://docs.microsoft.com/azure/key-vault/private-link-service)   |
|Azure Kubernetes Service: Kubernetes-API | Alle öffentlichen Regionen      |   Allgemein verfügbar   <br/> [Weitere Informationen](https://docs.microsoft.com/azure/aks/private-clusters)   |
|Azure Search | Alle öffentlichen Regionen |   Allgemein verfügbar   <br/> [Weitere Informationen](https://docs.microsoft.com/azure/search/search-security-overview#endpoint-access)    |
|Azure Container Registry | Alle öffentlichen Regionen      |   Allgemein verfügbar   <br/> [Weitere Informationen](https://docs.microsoft.com/azure/container-registry/container-registry-private-link)   |
|Azure App Configuration | Alle öffentlichen Regionen      |   Vorschau   |
|Azure Backup | Alle öffentlichen Regionen     |   Allgemein verfügbar   <br/> [Weitere Informationen](https://docs.microsoft.com/azure/backup/private-endpoints)   |
|Azure Event Hub | Alle öffentlichen Regionen      |    Allgemein verfügbar   <br/> [Weitere Informationen](https://docs.microsoft.com/azure/event-hubs/private-link-service)  |
|Azure-Servicebus | Alle öffentlichen Regionen      |  Allgemein verfügbar   <br/> [Weitere Informationen](https://docs.microsoft.com/azure/service-bus-messaging/private-link-service)    |
|Azure Relay | Alle öffentlichen Regionen      |   Vorschau <br/> [Weitere Informationen](https://docs.microsoft.com/azure/service-bus-relay/private-link-service)  |
|Azure Event Grid| Alle öffentlichen Regionen       |   Allgemein verfügbar   <br/> [Weitere Informationen](https://docs.microsoft.com/azure/event-grid/network-security) |
|Azure-Web-Apps | Alle öffentlichen Regionen      |   Vorschau   <br/> [Weitere Informationen](https://docs.microsoft.com/azure/app-service/networking/private-endpoint)   |
|Azure Machine Learning | „USA, OSTEN“, „USA, WESTEN 2“, „USA, SÜDEN-MITTE“      |   Vorschau   <br/> [Weitere Informationen](https://docs.microsoft.com/azure/machine-learning/how-to-configure-private-link)   |
| Azure IoT Hub | Alle öffentlichen Regionen    |   Allgemein verfügbar   <br/> [Weitere Informationen](https://docs.microsoft.com/azure/iot-hub/virtual-network-support ) |
| Azure SignalR | „USA, OSTEN“, „USA, WESTEN 2“, „USA, SÜDEN-MITTE“      |   Vorschau   <br/> [Weitere Informationen](https://aka.ms/asrs/privatelink)   |
| Azure Monitor <br/>(Log Analytics und Application Insights) | Alle öffentlichen Regionen      |   Allgemein verfügbar   <br/> [Weitere Informationen](https://docs.microsoft.com/azure/azure-monitor/platform/private-link-security)   |


Aktuelle Benachrichtigungen finden Sie auf der Seite [Azure Virtual Network-Updates](https://azure.microsoft.com/updates/?product=virtual-network).

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

Azure Private Link verfügt eine Integration in Azure Monitor. Diese Kombination ermöglicht Folgendes:

 - Archivierung von Protokollen in einem Speicherkonto
 - Streaming von Ereignissen an Ihren Event Hub
 - Azure Monitor-Protokollierung

Sie können über Azure Monitor auf die folgenden Informationen zugreifen: 
- **Privater Endpunkt**: 
    - Die vom privaten Endpunkt verarbeiteten Daten (ein-/ausgehend)
 
- **Private Link-Dienst**:
    - Die vom Private Link-Dienst verarbeiteten Daten (ein-/ausgehend)
    - NAT-Portverfügbarkeit  
 
## <a name="pricing"></a>Preise   
Ausführliche Preisinformationen finden Sie unter [Azure Private Link – Preise](https://azure.microsoft.com/pricing/details/private-link/).
 
## <a name="faqs"></a>Häufig gestellte Fragen  
Häufig gestellte Fragen finden Sie unter [Häufig gestellte Fragen zu Azure Private Link](private-link-faq.md).
 
## <a name="limits"></a>Einschränkungen  
Informationen zu Einschränkungen finden Sie unter [Einschränkungen für Azure Private Link](../azure-resource-manager/management/azure-subscription-service-limits.md#private-link-limits).

## <a name="service-level-agreement"></a>Vereinbarung zum Servicelevel
Informationen zur Vereinbarung zum Servicelevel (SLA) finden Sie unter [SLA für Azure Private Link](https://azure.microsoft.com/support/legal/sla/private-link/v1_0/).

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstart: Erstellen eines privaten Endpunkts über das Azure-Portal](create-private-endpoint-portal.md)
- [Schnellstart: Erstellen eines Private Link-Diensts über das Azure-Portal](create-private-link-service-portal.md)


 
