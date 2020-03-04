---
title: Übersicht über Azure-Diensttags
titlesuffix: Azure Virtual Network
description: Hier erhalten Sie Informationen zu Diensttags. Diensttags reduzieren die Komplexität bei der Erstellung von Sicherheitsregeln.
services: virtual-network
documentationcenter: na
author: jispar
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/22/2019
ms.author: jispar
ms.reviewer: kumud
ms.openlocfilehash: 47488403a10a6b955e15b0048d455cc17016f009
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77526245"
---
# <a name="virtual-network-service-tags"></a>Diensttags in virtuellen Netzwerken
<a name="network-service-tags"></a>

Ein Diensttag steht für eine Gruppe von IP-Adresspräfixen aus einem bestimmten Azure-Dienst. Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Tag automatisch, wenn sich die Adressen ändern. Auf diese Weise wird die Komplexität häufiger Updates an Netzwerksicherheitsregeln minimiert.

Sie können mithilfe von Diensttags Netzwerkzugriffssteuerungen in [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules)  oder  [Azure Firewall](https://docs.microsoft.com/azure/firewall/service-tags) definieren. Verwenden Sie Diensttags anstelle von spezifischen IP-Adressen, wenn Sie Sicherheitsregeln erstellen. Wenn Sie den Diensttagnamen (z. B. **ApiManagement**) im entsprechenden Feld *Quelle*  oder  *Ziel*  einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern.

Mithilfe von Diensttags können Sie Netzwerkisolation erreichen und Ihre Azure-Ressourcen vor dem allgemeinen Internet schützen, während Sie auf Azure-Dienste mit öffentlichen Endpunkten zugreifen. Erstellen Sie Regeln für eingehende/ausgehende Netzwerksicherheitsgruppen, um Datenverkehr in das **Internet** und aus diesem zu verweigern und Datenverkehr in die **Azure-Cloud** oder andere [verfügbare Diensttags](#available-service-tags) von bestimmten Azure-Diensten und aus diesen zuzulassen.

## <a name="available-service-tags"></a>Verfügbare Diensttags
Die folgende Tabelle listet alle Diensttags auf, die zur Verwendung in Regeln für [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) verfügbar sind.

In den Spalten wird angegeben, ob das jeweilige Tag:

- für Regeln geeignet ist, die eingehenden oder ausgehenden Datenverkehr abdecken.
- einen [regionalen](https://azure.microsoft.com/regions) Bereich unterstützt.
- in [Azure Firewall](https://docs.microsoft.com/azure/firewall/service-tags)-Regeln verwendet werden kann.

Standardmäßig spiegeln Diensttags die Bereiche für die gesamte Cloud wider. Einige Diensttags ermöglichen eine präzisere Steuerung, indem sie die entsprechenden IP-Adressbereiche auf eine bestimmte Region beschränken. Das Diensttag **Storage** beispielsweise repräsentiert Azure Storage für die gesamte Cloud, **Storage.WestUS** schränkt den Bereich jedoch auf die IP-Adressbereiche der Region „USA, Westen“ ein. Die folgende Tabelle gibt an, ob die einzelnen Diensttags einen solchen regionalen Bereich unterstützen.  

| Tag | Zweck | Eingehend oder ausgehend möglich? | Regional möglich? | Einsatz mit Azure Firewall möglich? |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **ApiManagement** | Verwaltungsdatenverkehr für dedizierte Azure API Management-Bereitstellungen. <br/><br/>*Hinweis:* Dieses Tag stellt den Azure API Management-Dienstendpunkt für die Steuerungsebene pro Region dar. Dadurch können Kunden Verwaltungsvorgänge für die APIs, Vorgänge, Richtlinien und NamedValues ausführen, die für den API Management Dienst konfiguriert sind.  | Eingehend | Ja | Ja |
| **ApplicationInsightsAvailability** | Verfügbarkeit von Application Insights | Eingehend | Nein | Nein |
| **AppService**    | Azure App Service Dieses Tag wird für ausgehende Sicherheitsregeln zu Web-App-Front-Ends empfohlen. | Ausgehend | Ja | Ja |
| **AppServiceManagement** | Verwaltungsdatenverkehr für dedizierte Bereitstellungen der App Service-Umgebung. | Beide | Nein | Ja |
| **AzureActiveDirectory** | Azure Active Directory | Ausgehend | Nein | Ja |
| **AzureActiveDirectoryDomainServices** | Verwaltungsdatenverkehr für dedizierte Azure Active Directory Domain Services-Bereitstellungen. | Beide | Nein | Ja |
| **AzureAdvancedThreatProtection** | Azure Advanced Threat Protection | Ausgehend | Nein | Nein |
| **AzureBackup** |Azure Backup.<br/><br/>*Hinweis:* Dieses Tag weist eine Abhängigkeit vom Tag **Storage** und **AzureActiveDirectory** auf. | Ausgehend | Nein | Ja |
| **AzureBotService** | Azure Bot Service | Ausgehend | Nein | Nein |
| **AzureCloud** | Alle [öffentlichen IP-Adressen im Rechenzentrum](https://www.microsoft.com/download/details.aspx?id=56519). | Ausgehend | Ja | Ja |
| **AzureCognitiveSearch** | Azure Cognitive Search. <br/><br/>Dieses Tag oder die von diesem Tag abgedeckten IP-Adressen können verwendet werden, um Indexern einen sicheren Zugriff auf Datenquellen zu gewähren. Weitere Informationen finden Sie in der [Dokumentation zur Indexerverbindung](https://docs.microsoft.com/azure/search/search-indexer-troubleshooting#connection-errors). | Eingehend | Nein | Nein |
| **AzureConnectors** | Azure Logic Apps-Connectors für Test- oder Back-End-Verbindungen. | Eingehend | Ja | Ja |
| **AzureContainerRegistry** | Azure Container Registry. | Ausgehend | Ja | Ja |
| **AzureCosmosDB** | Azure Cosmos DB. | Ausgehend | Ja | Ja |
| **AzureDatabricks** | Azure Databricks | Beide | Nein | Nein |
| **AzureDataExplorerManagement** | Azure Data Explorer-Verwaltung | Eingehend | Nein | Nein |
| **AzureDataLake** | Azure Data Lake. | Ausgehend | Nein | Ja |
| **AzureEventGrid** | Azure Event Grid: <br/><br/>*Hinweis:* Dieses Tag deckt nur Azure Event Grid-Endpunkte in den Regionen „USA, Süden-Mitte“, „USA, Osten“, „USA, Osten 2“, „USA, Westen 2“ und „USA, Mitte“ ab. | Beide | Nein | Nein |
| **AzureFrontDoor** | Azure Front Door | Beide | Nein | Nein |
| **AzureInformationProtection** | Azure Information Protection.<br/><br/>*Hinweis:* Dieses Tag weist eine Abhängigkeit von den Tags **AzureActiveDirectory** und **AzureFrontDoor.Frontend** auf. Fügen Sie auch die folgenden IP-Adressen in die Whitelist ein (diese Abhängigkeit wird bald entfernt): 13.107.6.181 und 13.107.9.181. | Ausgehend | Nein | Nein |
| **AzureIoTHub** | Azure IoT Hub. | Ausgehend | Nein | Nein |
| **AzureKeyVault** | Azure Key Vault:<br/><br/>*Hinweis:* Dieses Tag weist eine Abhängigkeit vom Tag **AzureActiveDirectory** auf. | Ausgehend | Ja | Ja |
| **AzureLoadBalancer** | Das Lastenausgleichsmodul der Azure-Infrastruktur. Das Tag wird in eine [virtuelle IP-Adresse des Hosts](security-overview.md#azure-platform-considerations) (168.63.129.16) umgewandelt, die als Ausgangspunkt für die Integritätstests von Azure dient. Dies schließt keinen Datenverkehr zu ihrer Azure Load Balancer-Ressource ein. Sie können diese Regel außer Kraft setzen, wenn Azure Load Balancer nicht verwendet wird. | Beide | Nein | Nein |
| **AzureMachineLearning** | Azure Machine Learning. | Beide | Nein | Ja |
| **AzureMonitor** | Log Analytics, Application Insights, AzMon und benutzerdefinierte Metriken (GiG-Endpunkte).<br/><br/>*Hinweis:* Für Log Analytics weist dieses Tag eine Abhängigkeit vom **Storage**-Tag auf. | Ausgehend | Nein | Ja |
| **AzurePlatformDNS** | Der grundlegende (standardmäßige) DNS-Dienst für die Infrastruktur.<br/><br>Sie können dieses Tag verwenden, um den standardmäßigen DNS-Dienst zu deaktivieren. Bei Verwendung dieses Tags ist Vorsicht geboten. Es wird empfohlen, die [Überlegungen zur Azure-Plattform](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) durchzulesen. Außerdem wird empfohlen, vor der Verwendung dieses Tags entsprechende Tests auszuführen. | Ausgehend | Nein | Nein |
| **AzurePlatformIMDS** | Azure Instance Metadata Service (IMDS), ein grundlegender Infrastrukturdienst.<br/><br/>Sie können dieses Tag verwenden, um den standardmäßigen IMDS-Dienst zu deaktivieren. Bei Verwendung dieses Tags ist Vorsicht geboten. Es wird empfohlen, die [Überlegungen zur Azure-Plattform](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) durchzulesen. Außerdem wird empfohlen, vor der Verwendung dieses Tags entsprechende Tests auszuführen. | Ausgehend | Nein | Nein |
| **AzurePlatformLKM** | Windows-Lizenzierungs- oder Schlüsselverwaltungsdienst.<br/><br/>Sie können dieses Tag verwenden, um die Standardwerte für die Lizenzierung zu deaktivieren. Bei Verwendung dieses Tags ist Vorsicht geboten. Es wird empfohlen, die [Überlegungen zur Azure-Plattform](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) durchzulesen.  Außerdem wird empfohlen, vor der Verwendung dieses Tags entsprechende Tests auszuführen. | Ausgehend | Nein | Nein |
| **AzureResourceManager** | Azure Resource Manager | Ausgehend | Nein | Nein |
| **AzureSiteRecovery** | Azure Site Recovery.<br/><br/>*Hinweis:* Dieses Tag weist eine Abhängigkeit von den Tags **Storage**, **AzureActiveDirectory** und **EventHub** auf. | Ausgehend | Nein | Nein |
| **AzureTrafficManager** | Test-IP-Adressen von Azure Traffic Manager.<br/><br/>Weitere Informationen zu Test-IP-Adressen von Traffic Manager finden Sie unter [Häufig gestellte Fragen (FAQ) zu Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs). | Eingehend | Nein | Ja |  
| **BatchNodeManagement** | Verwaltungsdatenverkehr für dedizierte Azure Batch-Bereitstellungen. | Beide | Nein | Ja |
| **CognitiveServicesManagement** | Die Adressbereiche des Datenverkehrs für Azure Cognitive Services. | Ausgehend | Nein | Nein |
| **Dynamics365ForMarketingEmail** | Die Adressbereiche für den Marketing-E-Mail-Dienst von Dynamics 365. | Ausgehend | Ja | Nein |
| **ElasticAFD** | Azure Front Door (elastisch) | Beide | Nein | Nein |
| **EventHub** | Azure Event Hubs. | Ausgehend | Ja | Ja |
| **GatewayManager** | Verwaltungsdatenverkehr für dedizierte Azure VPN Gateway- und Application Gateway-Bereitstellungen. | Eingehend | Nein | Nein |
| **GuestAndHybridManagement** | Azure Automation und Gastkonfiguration | Ausgehend | Nein | Ja |
| **HDInsight** | Azure HDInsight. | Eingehend | Ja | Nein |
| **Internet** | Der IP-Adressraum, der außerhalb des virtuellen Netzwerks liegt und über das öffentliche Internet erreichbar ist.<br/><br/>Der Adressbereich schließt den [Azure-eigenen öffentlichen IP-Adressraum](https://www.microsoft.com/download/details.aspx?id=41653) ein. | Beide | Nein | Nein |
| **MicrosoftCloudAppSecurity** | Microsoft Cloud App Security | Ausgehend | Nein | Nein |
| **MicrosoftContainerRegistry** | Containerregistrierung für Microsoft-Containerimages. <br/><br/>*Hinweis:* Fügen Sie auch die folgende IP-Adresse in die Whitelist ein (diese Abhängigkeit wird bald entfernt): 204.79.197.219. | Ausgehend | Ja | Ja |
| **ServiceBus** | Azure Service Bus-Datenverkehr, der die Dienstebene „Premium“ verwendet. | Ausgehend | Ja | Ja |
| **ServiceFabric** | Azure Service Fabric.<br/><br/>*Hinweis:* Dieses Tag stellt den Service Fabric-Dienstendpunkt für die Steuerungsebene pro Region dar. Dadurch können Kunden Verwaltungsvorgänge für ihre Service Fabric-Cluster aus ihrem VNET vornehmen (Beispiel für Endpunkt https:// westus.servicefabric.azure.com) | Beide | Nein | Nein |
| **SQL** | Azure SQL-Datenbank, Azure Database for MySQL, Azure Database for PostgreSQL und Azure SQL Data Warehouse.<br/><br/>*Hinweis:* Dieses Tag steht für den Dienst, nicht für bestimmte Instanzen des Diensts. Beispielsweise steht das Tag für den Azure SQL-Datenbank-Dienst, aber nicht für eine bestimmte SQL-Datenbank oder einen bestimmten SQL-Server. | Ausgehend | Ja | Ja |
| **SqlManagement** | Verwaltungsdatenverkehr für dedizierte SQL-Bereitstellungen. | Beide | Nein | Ja |
| **Storage** | „Azure Storage“. <br/><br/>*Hinweis:* Dieses Tag steht für den Dienst, nicht für bestimmte Instanzen des Diensts. Beispielsweise steht das Tag für den Azure Storage-Dienst, aber nicht für ein bestimmtes Azure Storage-Konto. | Ausgehend | Ja | Ja |
| **VirtualNetwork** | Der Adressraum des virtuellen Netzwerks (alle für das virtuelle Netzwerk definierten IP-Adressbereiche), alle verbundenen lokalen Adressräume, [per Peering verknüpfte](virtual-network-peering-overview.md) virtuelle Netzwerke, virtuelle Netzwerke, die mit einem [Gateway des virtuellen Netzwerks](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%3ftoc.json) verbunden sind, die [virtuelle IP-Adresse des Hosts](security-overview.md#azure-platform-considerations) und Adresspräfixe, die in [benutzerdefinierten Routen](virtual-networks-udr-overview.md) verwendet werden. Dieses Tag kann auch Standardrouten enthalten. | Beide | Nein | Nein |

>[!NOTE]
>Im klassischen Bereitstellungsmodell (vor Azure Resource Manager) wird ein Teil der in der obigen Tabelle aufgeführten Tags unterstützt. Diese Tags werden anderes geschrieben:
>
>| Klassische Schreibweise | Entsprechendes Resource Manager-Tag |
>|---|---|
>| AZURE_LOADBALANCER | AzureLoadBalancer |
>| INTERNET | Internet |
>| VIRTUAL_NETWORK | VirtualNetwork |

> [!NOTE]
> Mit Diensttags von Azure-Diensten werden die Adresspräfixe der jeweils verwendeten Cloud angegeben. Beispielsweise unterscheiden sich die zugrunde liegenden IP-Adressbereiche, die dem Wert des **Sql**-Tags in der öffentlichen Azure-Cloud entsprechen, von den zugrunde liegenden Bereichen in der chinesischen Azure-Cloud.

> [!NOTE]
> Wenn Sie einen [VNET-Dienstendpunkt](virtual-network-service-endpoints-overview.md) für einen Dienst implementieren, z. B. Azure Storage oder Azure SQL-Datenbank, fügt Azure eine [Route](virtual-networks-udr-overview.md#optional-default-routes) zu einem Subnetz des virtuellen Netzwerks für den Dienst hinzu. Die Adresspräfixe für die Route sind die gleichen Adresspräfixe bzw. CIDR-Bereiche wie diejenigen für das entsprechende Diensttag.

## <a name="service-tags-on-premises"></a>Lokale Diensttags  
Sie können die aktuellen Informationen zu Diensttag und Bereich abrufen und in Ihren lokalen Firewallkonfigurationen verwenden. Bei diesen Informationen handelt es sich um die zum jetzigen Zeitpunkt gültige Liste der IP-Adressbereiche, die den einzelnen Diensttags entsprechen. Sie können die Informationen programmgesteuert oder über einen JSON-Dateidownload abrufen (siehe Beschreibung in den folgenden Abschnitten).

### <a name="use-the-service-tag-discovery-api-public-preview"></a>Verwendung der Diensttagermittlungs-API (öffentliche Vorschau)
Sie können die aktuelle Liste der Diensttags zusammen mit Details zum IP-Adressbereich programmgesteuert abrufen:

- [REST](https://docs.microsoft.com/rest/api/virtualnetwork/servicetags/list)
- [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/Get-AzNetworkServiceTag?view=azps-2.8.0&viewFallbackFrom=azps-2.3.2)
- [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/network?view=azure-cli-latest#az-network-list-service-tags)

> [!NOTE]
> Obwohl sie sich in der öffentlichen Vorschauphase befindet, gibt die Ermittlungs-API u. U. weniger aktuelle Informationen zurück als die Informationen, die von den JSON-Downloads zurückgegeben werden. (Siehe nächster Abschnitt.)


### <a name="discover-service-tags-by-using-downloadable-json-files"></a>Ermitteln von Diensttags mithilfe von herunterladbaren JSON-Dateien 
Sie können JSON-Dateien mit der aktuellen Liste der Diensttags zusammen mit Details zum IP-Adressbereich herunterladen. Diese Listen werden wöchentlich aktualisiert und veröffentlicht. Hier finden Sie die Speicherorte für die einzelnen Clouds:

- [Azure öffentlich](https://www.microsoft.com/download/details.aspx?id=56519)
- [Azure US Government](https://www.microsoft.com/download/details.aspx?id=57063)  
- [Azure China](https://www.microsoft.com/download/details.aspx?id=57062) 
- [Azure Deutschland](https://www.microsoft.com/download/details.aspx?id=57064)   

> [!NOTE]
>Ein Teil dieser Informationen wurde in XML-Dateien für [Azure öffentlich](https://www.microsoft.com/download/details.aspx?id=41653), [Azure China](https://www.microsoft.com/download/details.aspx?id=42064) und [Azure Deutschland](https://www.microsoft.com/download/details.aspx?id=54770) veröffentlicht. Diese XML-Downloads werden zum 30. Juni 2020 eingestellt und stehen nach diesem Datum nicht mehr zur Verfügung. Sie sollten stattdessen wie in den vorherigen Abschnitten beschrieben die Ermittlungs-API oder JSON-Dateidownloads verwenden.

### <a name="tips"></a>Tipps 
- Sie können Updates zwischen verschiedenen Veröffentlichungen anhand der erhöhten *changeNumber*-Werte in der JSON-Datei erkennen. Jeder Teilabschnitt (z. B. **Storage.WestUS**) weist einen eigenen *changeNumber*-Wert auf, der bei Änderungen erhöht wird. Die oberste Ebene des *changeNumber*-Werts einer Datei wird erhöht, wenn einer der Teilabschnitte geändert wird.
- Beispiele für die Analyse der Diensttaginformationen (z. B. zum Abrufen aller Adressbereiche für „Storage“ in „USA, Westen“) finden Sie in der Dokumentation zur [Diensttagermittlungs-API in PowerShell](https://aka.ms/discoveryapi_powershell).

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen einer Netzwerksicherheitsgruppe](tutorial-filter-network-traffic.md).
