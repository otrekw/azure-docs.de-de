---
title: 'Konzepte: API Management'
description: Hier erfahren Sie, wie auf virtuellen AVS-Computern (Azure VMware Solution) ausgeführte APIs durch API Management geschützt werden.
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: 62112bf3c0bf551232e09e5910e3eaae228dc202
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85306748"
---
# <a name="api-management-to-publish-and-protect-apis-running-on-avs-based-vms"></a>API Management zum Veröffentlichen und Schützen von APIs, die auf virtuellen AVS-basierten Computern ausgeführt werden

Microsoft Azure [API Management](https://azure.microsoft.com/services/api-management/) ermöglicht Entwicklern und DevOps-Teams eine sichere Veröffentlichung für interne oder externe Consumer.

Die Komponente wird zwar in mehreren SKUs angeboten, die Azure Virtual Network-Integration für die Veröffentlichung von APIs, die in AVS-Workloads (Azure VMware Solution) ausgeführt werden, steht jedoch nur in der Developer- und in der Premium-SKU zur Verfügung. Diese beiden SKUs ermöglichen eine sichere Konnektivität zwischen dem API Management-Dienst und dem Back-End. Die Developer-SKU ist für Entwicklungs- und Testzwecke vorgesehen, die Premium-SKU für Produktionsbereitstellungen.

Bei Back-End-Diensten, die auf virtuellen AVS-Computern ausgeführt werden, ist die Konfiguration in API Management standardmäßig die gleiche wie bei lokalen Back-End-Diensten. Von API Management wird sowohl bei internen als auch bei externen Bereitstellungen die virtuelle IP-Adresse (VIP) des Lastenausgleichs als Back-End-Endpunkt konfiguriert, wenn der Back-End-Server hinter einem AVS-seitigen NSX-Lastenausgleich platziert wird.

## <a name="external-deployment"></a>Externe Bereitstellung

Bei einer externen Bereitstellung werden APIs veröffentlicht, die von externen Benutzern über einen öffentlichen Endpunkt verwendet werden. Entwickler und DevOps-Techniker können APIs über das Azure-Portal oder mithilfe von PowerShell sowie über das API Management-Entwicklerportal verwalten.

Das Diagramm für die externe Bereitstellung zeigt den gesamten Prozess und die beteiligten Akteure (ganz oben). Zu den Akteuren zählen:

- **Administrator(en):** Das Administrator- oder DevOps-Team, das AVS über das Azure-Portal sowie über Automatisierungsmechanismen wie PowerShell oder Azure DevOps verwaltet.

- **Benutzer:**  Die Consumer der verfügbar gemachten APIs (sowohl Benutzer als auch Dienste).

Die Daten durchlaufen die API Management-Instanz, von der die in das virtuelle Hub-Netzwerk eingebundenen Back-End-Dienste abstrahiert werden. Der Datenverkehr wird vom ExpressRoute-Gateway an den Kanal „ExpressRoute Global Reach“ weitergeleitet und erreicht einen NSX-Lastenausgleich, durch den der eingehende Datenverkehr auf die verschiedenen Back-End-Dienstinstanzen verteilt wird.

API Management verfügt über eine öffentliche Azure-API, und es empfiehlt sich, den Dienst „Azure DDoS Protection“ zu aktivieren. 

:::image type="content" source="media/api-management/external-deployment.png" alt-text="Externe Bereitstellung: API Management für AVS":::


## <a name="internal-deployment"></a>Interne Bereitstellung

Bei einer internen Bereitstellung werden APIs veröffentlicht, die von internen Benutzern oder Systemen genutzt werden. DevOps-Team und API-Entwickler verwenden die gleichen Verwaltungstools und das gleiche Entwicklerportal wie bei der externen Bereitstellung.

Bei internen Bereitstellungen kann [Azure Application Gateway](../api-management/api-management-howto-integrate-internal-vnet-appgateway.md) verwendet werden, um einen öffentlichen und sicheren Endpunkt für die API zu erstellen.  Die API nutzt die zugehörigen Funktionen und erstellt eine Hybridbereitstellung, die verschiedene Szenarien ermöglicht.

* Verwenden der gleichen API Management-Ressource für die Nutzung durch interne und externe Consumer

* Verwenden einer einzelnen API Management-Ressource mit einer Teilmenge von APIs, die für externe Consumer definiert und verfügbar ist

* Bereitstellen einer einfachen Möglichkeit zum Aktivieren/Deaktivieren des Zugriffs auf API Management über das öffentliche Internet

Das Bereitstellungsdiagramm weiter unten zeigt Consumer, die intern oder extern sein können, wobei jeder Typ auf die gleichen oder auf unterschiedliche APIs zugreift.

Bei einer internen Bereitstellung werden APIs für die gleiche API Management-Instanz verfügbar gemacht. Application Gateway wird vor API Management mit aktivierter WAF-Funktion (Web Application Firewall) und einer Reihe von HTTP-Listenern und Regeln zum Filtern des Datenverkehrs bereitgestellt, sodass nur eine Teilmenge der in AVS ausgeführten Back-End-Dienste verfügbar gemacht wird.

* Interner Datenverkehr wird per ExpressRoute-Gateway an Azure Firewall und anschließend an API Management weitergeleitet (wenn Datenverkehrsregeln eingerichtet wurden) – oder aber direkt an API Management.  

* Externer Datenverkehr wird über Application Gateway unter Verwendung der externen Schutzebene für API Management in Azure eingebracht.


:::image type="content" source="media/api-management/internal-deployment.png" alt-text="Interne Bereitstellung: API Management für AVS":::