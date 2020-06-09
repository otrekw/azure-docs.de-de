---
title: DNS-Konfiguration für private Azure-Endpunkte
description: Erfahren Sie etwas über die DNS-Konfiguration von privaten Azure-Endpunkten.
services: private-link
author: mblanco77
ms.service: private-link
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: allensu
ms.openlocfilehash: 7c8ff0808ada522dc24ef3c27156f4151832fbcd
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83715935"
---
# <a name="azure-private-endpoint-dns-configuration"></a>DNS-Konfiguration für private Azure-Endpunkte


Wenn Sie eine Verbindung mit einer Private Link-Ressource über einen voll qualifizierten Domänennamen (FQDN) als Teil der Verbindungszeichenfolge herstellen, ist es wichtig, Ihre DNS-Einstellungen so zu konfigurieren, dass sie in die zugewiesene private IP-Adresse aufgelöst werden. Bestehende Azure-Dienste verfügen möglicherweise bereits über eine DNS-Konfiguration, die beim Herstellen einer Verbindung über einen öffentlichen Endpunkt verwendet werden kann. Diese Konfiguration muss überschrieben werden, um eine Verbindung mithilfe Ihres privaten Endpunkts herzustellen. 
 
Die Netzwerkschnittstelle, die dem privaten Endpunkt zugeordnet ist, enthält alle Informationen, die zur Konfiguration Ihres DNS erforderlich sind, einschließlich FQDN und privater IP-Adressen, die einer bestimmten Private Link-Ressource zugewiesen wurden. 
 
Mit den folgenden Optionen können Sie Ihre DNS-Einstellungen für private Endpunkte konfigurieren: 
- **Hostdatei (wird nur für Tests empfohlen)** . Sie können die Hostdatei auf einem virtuellen Computer verwenden, um das DNS zu überschreiben.  
- **Private DNS-Zone**. Sie können [private DNS-Zonen](../dns/private-dns-privatednszone.md) verwenden, um die DNS-Auflösung für einen bestimmten privaten Endpunkt außer Kraft zu setzen. Eine private DNS-Zone kann mit Ihrem virtuellen Netzwerk verknüpft werden, um bestimmte Domänen aufzulösen.
- **DNS-Weiterleitung (optional)** . Sie können Ihre DNS-Weiterleitung verwenden, um die DNS-Auflösung für eine angegebene Private Link-Ressource zu überschreiben. Wenn Ihr [DNS-Server](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) in einem virtuellen Netzwerk gehostet wird, können Sie zum Verwenden einer privaten DNS-Zone eine DNS-Weiterleitungsregel erstellen, um die Konfiguration für alle Private Link-Ressourcen zu vereinfachen.
 
> [!IMPORTANT]
> Es wird nicht empfohlen, eine Zone zu überschreiben, die aktiv zur Auflösung öffentlicher Endpunkte genutzt wird. Verbindungen mit Ressourcen können ohne DNS-Weiterleitung an das öffentliche DNS nicht ordnungsgemäß aufgelöst werden. Um Probleme zu vermeiden, erstellen Sie einen anderen Domänennamen, oder halten Sie sich an den vorgeschlagenen Namen für jeden der folgenden Dienste. 

## <a name="azure-services-dns-zone-configuration"></a>DNS-Zonenkonfiguration für Azure-Dienste
Azure-Dienste erstellen einen DNS-Eintrag vom Typ CNAME (kanonischer Name) im öffentlichen DNS, um die Auflösung an den vorgeschlagenen Namen der privaten Domäne umzuleiten. Sie können die Auflösung mit der privaten IP-Adresse Ihrer privaten Endpunkte überschreiben. 
 
Ihre Anwendungen müssen die Verbindungs-URL nicht ändern. Beim Versuch der Auflösung mithilfe eines öffentlichen DNS wird der DNS-Server nun in Ihre privaten Endpunkten aufgelöst. Der Vorgang wirkt sich nicht auf Ihre vorhandenen Anwendungen aus. 

Verwenden Sie für Azure-Dienste die empfohlenen Zonennamen in der folgenden Tabelle:

| Typ der Private Link-Ressource/Unterressource |Name der privaten DNS-Zone | Weiterleitungen der öffentlichen DNS-Zone |
|---|---|---|---|
| SQL DB (Microsoft.Sql/servers)/SQL Server | privatelink.database.windows.net | database.windows.net |
| Azure Synapse Analytics (Microsoft.Sql/servers)/SQL Server  | privatelink.database.windows.net | database.windows.net |
| Storage-Konto (Microsoft.Storage/storageAccounts)/Blob (blob, blob_secondary) | privatelink.blob.core.windows.net | blob.core.windows.net |
| Storage-Konto (Microsoft.Storage/storageAccounts)/Tabelle (table, table_secondary) | privatelink.table.core.windows.net | table.core.windows.net |
| Storage-Konto (Microsoft.Storage/storageAccounts)/Warteschlange (queue, queue_secondary) | privatelink.queue.core.windows.net | queue.core.windows.net |
| Storage-Konto (Microsoft.Storage/storageAccounts)/Datei (file, file_secondary) | privatelink.file.core.windows.net | file.core.windows.net |
| Storage-Konto (Microsoft.Storage/storageAccounts)/Web (web, web_secondary) | privatelink.web.core.windows.net | web.core.windows.net |
| Data Lake-Dateisystem Gen2 (Microsoft.Storage/storageAccounts)/Data Lake-Dateisystem Gen2 (dfs, dfs_secondary) | privatelink.dfs.core.windows.net | dfs.core.windows.net |
| Azure Cosmos DB (Microsoft.AzureCosmosDB/databaseAccounts)/SQL | privatelink.documents.azure.com | documents.azure.com |
| Azure Cosmos DB (Microsoft.AzureCosmosDB/databaseAccounts)/MongoDB | privatelink.mongo.cosmos.azure.com | mongo.cosmos.azure.com |
| Azure Cosmos DB (Microsoft.AzureCosmosDB/databaseAccounts)/Cassandra | privatelink.cassandra.cosmos.azure.com | cassandra.cosmos.azure.com |
| Azure Cosmos DB (Microsoft.AzureCosmosDB/databaseAccounts)/Gremlin | privatelink.gremlin.cosmos.azure.com | gremlin.cosmos.azure.com |
| Azure Cosmos DB (Microsoft.AzureCosmosDB/databaseAccounts)/Tabelle | privatelink.table.cosmos.azure.com | table.cosmos.azure.com |
| Azure Database for PostgreSQL – Einzelserver (Microsoft.DBforPostgreSQL/servers)/postgresqlServer | privatelink.postgres.database.azure.com | postgres.database.azure.com |
| Azure Database for MySQL (Microsoft.DBforMySQL/servers)/mysqlServer | privatelink.mysql.database.azure.com | mysql.database.azure.com |
| Azure Database for MariaDB (Microsoft.DBforMariaDB/servers)/mariadbServer | privatelink.mariadb.database.azure.com | mariadb.database.azure.com |
| Azure Key Vault (Microsoft.KeyVault/vaults)/Tresor | privatelink.vaultcore.azure.net | vault.azure.net <br> vaultcore.azure.net |
| Azure Kubernetes Service: Kubernetes-API (Microsoft.ContainerService/managedClusters)/managedCluster | privatelink.{Region}.azmk8s.io | {Region}.azmk8s.io |
| Azure Search (Microsoft.Search/searchServices)/searchService | privatelink.search.windows.net | search.windows.net |
| Azure Container Registry (Microsoft.ContainerRegistry/registries)/Registrierung | privatelink.azurecr.io | azurecr.io |
| Azure App Configuration (Microsoft.AppConfiguration/configurationStores)/configurationStore | privatelink.azconfig.io | azconfig.io |
| Azure Backup (Microsoft.RecoveryServices/vaults)/Tresor | privatelink.{region}.backup.windowsazure.com | {Region}.backup.windowsazure.com |
| Azure Event Hub (Microsoft.EventHub/namespaces)/Namespace | privatelink.servicebus.windows.net | servicebus.windows.net |
| Azure Service Bus (Microsoft.ServiceBus/namespaces)/Namespace | privatelink.servicebus.windows.net | servicebus.windows.net |
| Azure Relay (Microsoft.Relay/namespaces)/Namespace | privatelink.servicebus.windows.net | servicebus.windows.net |
| Azure Event Grid (Microsoft.EventGrid/topics)/Thema | privatelink.eventgrid.azure.net | eventgrid.azure.net |
| Azure Event Grid (Microsoft.EventGrid/domains)/Domäne | privatelink.eventgrid.azure.net | eventgrid.azure.net |
| Azure-Web-Apps (Microsoft.Web/sites)/Site | privatelink.azurewebsites.net | azurewebsites.net |
| Azure Machine Learning (Microsoft.MachineLearningServices/workspaces)/Arbeitsbereich | privatelink.api.azureml.ms | api.azureml.ms |

 


## <a name="dns-configuration-scenarios"></a>Szenarien der DNS-Konfiguration

Der FQDN der Dienste wird automatisch in eine öffentliche IP-Adresse aufgelöst. Sie müssen also Ihre DNS-Konfiguration entsprechend ändern, damit die Auflösung in die private IP-Adresse des privaten Endpunkts durchgeführt wird.

DNS ist eine zentrale Komponente, damit die Anwendung ordnungsgemäß funktioniert, indem die IP-Adresse des privaten Endpunkts auf die richtige Weise aufgelöst wird.

Basierend auf Ihren Einstellungen sind die folgenden Szenarien für integrierte DNS-Auflösung verfügbar:

- [Virtual Network-Workloads ohne benutzerdefinierten DNS-Server](#virtual-network-workloads-without-custom-dns-server)
- [Lokale Workloads mit DNS-Weiterleitung](#on-premises-workloads-using-a-dns-forwarder)

## <a name="virtual-network-workloads-without-custom-dns-server"></a>Virtual Network-Workloads ohne benutzerdefinierten DNS-Server

Diese Konfiguration ist für Workloads virtueller Netzwerke ohne benutzerdefinierten DNS-Server geeignet. In diesem Szenario fragt der Client die IP-Adresse des privaten Endpunkts beim von Azure bereitgestellten DNS als [168.63.129.16](../virtual-network/what-is-ip-address-168-63-129-16.md) ab. Azure DNS ist für die DNS-Auflösung der privaten DNS-Zonen verantwortlich.


> [!NOTE]
> In diesem Szenario wird die empfohlene private DNS-Zone für Azure SQL-Datenbank verwendet. Für andere Dienste können Sie das Modell mithilfe der folgenden Referenz anpassen: [DNS-Zonenkonfiguration für Azure-Dienste](#azure-services-dns-zone-configuration).

Für eine korrekte Konfiguration benötigen Sie die folgenden Ressourcen:

- Virtuelles Clientnetzwerk

- Private DNS-Zone [privatelink.database.windows.net](../dns/private-dns-privatednszone.md) mit [A-Datensatz](../dns/dns-zones-records.md#record-types)

- Informationen zum privaten Endpunkt (Name des FQDN-Datensatzes und private IP-Adresse)

Das folgende Diagramm veranschaulicht die DNS-Auflösungssequenz aus Workloads virtueller Netzwerke über private DNS-Zonen.

:::image type="content" source="media/private-endpoint-dns/single-vnet-azure-dns.png" alt-text="Einzelnes virtuelles Netzwerk und von Azure bereitgestelltes DNS":::

Dieses Modell kann auf mehrere virtuelle Peeringnetzwerke erweitert werden, die demselben privaten Endpunkt zugeordnet sind. Dies können Sie erreichen, indem Sie [neue Verknüpfungen für virtuelle Netzwerke](../dns/private-dns-virtual-network-links.md) mit der privaten DNS-Zone für alle mittels Peering verbundenen virtuellen Netzwerke hinzufügen.

> [!IMPORTANT]
> Für diese Konfiguration ist eine einzelne private DNS-Zone erforderlich. Zum Erstellen mehrerer Zonen mit demselben Namen für verschiedene virtuelle Netzwerke müssen die DNS-Einträge manuell zusammengeführt werden.

In diesem Szenario wird eine [Hub-Spoke](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)-Netzwerktopologie genutzt, in der die Spokenetzwerke einen privaten Endpunkt gemeinsam nutzen und alle virtuellen Spokenetzwerke mit derselben privaten DNS-Zone verknüpft sind. 

:::image type="content" source="media/private-endpoint-dns/hub-and-spoke-azure-dns.png" alt-text="Hub-and-Spoke-Modell mit von Azure bereitgestelltem DNS":::

## <a name="on-premises-workloads-using-a-dns-forwarder"></a>Lokale Workloads mit DNS-Weiterleitung

Damit lokale Workloads den FQDN eines privaten Endpunkts in die private IP-Adresse auflösen können, müssen Sie eine DNS-Weiterleitung verwenden, um die Auflösung der [öffentlichen DNS-Zone](#azure-services-dns-zone-configuration) des Azure-Diensts in Azure bereitzustellen.


Das folgende Szenario gilt für ein lokales Netzwerk mit einer DNS-Weiterleitung in Azure, von der alle DNS-Abfragen aufgelöst werden, die über eine Weiterleitung auf Serverebene an das von Azure bereitgestellte DNS [168.63.129.16](../virtual-network/what-is-ip-address-168-63-129-16.md) fließen. 

> [!NOTE]
> In diesem Szenario wird die empfohlene private DNS-Zone für Azure SQL-Datenbank verwendet. Für andere Dienste können Sie das Modell mithilfe der folgenden Referenz anpassen:  [DNS-Zonenkonfiguration für Azure-Dienste](#azure-services-dns-zone-configuration).

Für eine korrekte Konfiguration benötigen Sie die folgenden Ressourcen:

- Lokales Netzwerk
- Virtuelles Netzwerk [mit Verbindung mit der lokalen Umgebung](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/)
- In Azure bereitgestellte DNS-Weiterleitung 
- Zonen des privaten DNS ( [privatelink.database.windows.net](../dns/private-dns-privatednszone.md)) mit [A-Eintrag](../dns/dns-zones-records.md#record-types)
- Informationen zum privaten Endpunkt (Name des FQDN-Eintrags und private IP-Adresse)

Im folgenden Diagramm ist die DNS-Auflösungssequenz eines lokalen Netzwerks dargestellt, in dem eine in Azure bereitgestellte DNS-Weiterleitung verwendet wird. Die Auflösung wird hierbei von einer Zone des privaten DNS durchgeführt, die [mit einem virtuellen Netzwerk verknüpft ist](../dns/private-dns-virtual-network-links.md).

:::image type="content" source="media/private-endpoint-dns/on-premises-using-azure-dns.png" alt-text="Lokal per Azure DNS":::

Diese Konfiguration kann für ein lokales Netzwerk erweitert werden, für das bereits eine DNS-Lösung vorhanden ist. 
Die lokale DNS-Lösung muss so konfiguriert werden, dass der DNS-Datenverkehr über eine [bedingte Weiterleitung](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) an das Azure DNS weitergeleitet wird, indem auf die in Azure bereitgestellte DNS-Weiterleitung verwiesen wird.

> [!NOTE]
> In diesem Szenario wird die empfohlene Zone des privaten DNS für Azure SQL-Datenbank verwendet. Für andere Dienste können Sie das Modell mithilfe der folgenden Referenz anpassen:  [DNS-Zonenkonfiguration für Azure-Dienste](#azure-services-dns-zone-configuration).

Für eine korrekte Konfiguration benötigen Sie die folgenden Ressourcen:

- Lokales Netzwerk mit einer benutzerdefinierten DNS-Lösung 
- Virtuelles Netzwerk [mit Verbindung mit der lokalen Umgebung](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/)
- In Azure bereitgestellte DNS-Weiterleitung
- Zonen des privaten DNS ( [privatelink.database.windows.net](../dns/private-dns-privatednszone.md))  mit [A-Eintrag](../dns/dns-zones-records.md#record-types)
- Informationen zum privaten Endpunkt (Name des FQDN-Eintrags und private IP-Adresse)

Im folgenden Diagramm ist die DNS-Auflösungssequenz eines lokalen Netzwerks dargestellt, in dem eine bedingte Weiterleitung des DNS-Datenverkehrs an Azure verwendet wird. Die Auflösung wird hierbei von einer Zone des privaten DNS durchgeführt, die  [mit einem virtuellen Netzwerk verknüpft ist](../dns/private-dns-virtual-network-links.md).

> [!IMPORTANT]
> Die bedingte Weiterleitung muss über die empfohlene [**Weiterleitung der Zone des öffentlichen DNS** erfolgen](#azure-services-dns-zone-configuration) Beispiel: `database.windows.net`  anstelle von „ **privatelink**.database.windows.net“.

:::image type="content" source="media/private-endpoint-dns/on-premises-forwarding-to-azure.png" alt-text="Lokale Weiterleitung an Azure DNS":::


## <a name="next-steps"></a>Nächste Schritte
- [Weitere Informationen zu privaten Endpunkten](private-endpoint-overview.md)
