---
title: Zugriff auf virtuelle Azure-Netzwerke
description: Übersicht darüber, wie Logik-Apps mit Integrationsdienstumgebungen (ISEs) auf virtuelle Azure-Netzwerke (VNETs) zugreifen können
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 03/12/2020
ms.openlocfilehash: 9d5e0c088fe773f16e1fc57f292ca812906aa09c
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2020
ms.locfileid: "79127249"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Zugreifen auf Ressourcen virtueller Azure-Netzwerke über Azure Logic Apps mit Integrationsdienstumgebungen (ISEs)

Manchmal benötigen Ihre Logik-Apps Zugriff auf geschützte Ressourcen wie virtuelle Computer (virtual machines, VMs) und andere Systeme oder Dienste innerhalb eines [virtuellen Azure-Netzwerks](../virtual-network/virtual-networks-overview.md). Um diesen Zugriff einzurichten, können Sie eine [*Integrationsdienstumgebung* (Integration Service Environment, ISE) erstellen](../logic-apps/connect-virtual-network-vnet-isolated-environment.md). Eine ISE ist eine isolierte Instanz des Logic Apps-Diensts, die dedizierte Ressourcen verwendet und getrennt vom „globalen“ mehrinstanzenfähigen Logic Apps-Dienst ausgeführt wird.

Das Ausführen von Logik-Apps als eigene separate isolierte Instanz trägt dazu bei, mögliche Auswirkungen anderer Azure-Mandanten auf die Leistung Ihrer Apps zu verringern. Dies ist auch als [„Noisy Neighbors“-Problem](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors) bekannt. Eine ISE bietet darüber hinaus die folgenden Vorteile:

* Ihre eigenen statischen IP-Adressen. Diese sind gesondert von den statischen IP-Adressen, die von den Logik-Apps im mehrinstanzenfähigen Dienst gemeinsam verwendet werden. Sie können auch eine einzelne öffentliche, statische und vorhersagbare ausgehende IP-Adresse für die Kommunikation mit Zielsystemen einrichten. Auf diese Weise müssen Sie nicht für jede ISE an den Zielsystemen zusätzliche Firewallzugänge einrichten.

* Höhere Grenzwerte für Ausführungsdauer, Speicheraufbewahrung, Durchsatz, Zeitlimits für HTTP-Anforderungen und -Antworten, Nachrichtengröße und benutzerdefinierte Connectoranforderungen. Weitere Informationen finden Sie unter [Grenzwerte und Konfiguration für Azure Logic Apps](logic-apps-limits-and-config.md).

> [!NOTE]
> Einige virtuelle Azure-Netzwerke verwenden private Endpunkte ([Azure Private Link](../private-link/private-link-overview.md)), um den Zugriff auf Azure-PaaS-Dienste wie Azure Storage, Azure Cosmos DB oder Azure SQL-Datenbank sowie auf Partnerdienste oder auf Kundendienste zu ermöglichen, die in Azure gehostet werden. Falls Ihre Logik-Apps Zugriff auf virtuelle Netzwerke mit privaten Endpunkten benötigen, müssen diese Logik-Apps in einer ISE erstellt, bereitgestellt und ausgeführt werden.

Wenn Sie eine ISE erstellen, *injiziert* Azure diese ISE bzw. stellt sie in Ihrem virtuellen Azure-Netzwerk bereit. Anschließend können Sie diese ISE als Speicherort für Logik-Apps und Integrationskonten verwenden, die Zugriff benötigen.

![Auswählen der Integrationsdienstumgebung](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Logik-Apps können nun unter Verwendung eines der folgenden Elemente, die in derselben ISE wie Ihre Logik-Apps ausgeführt werden, auf Ressourcen zugreifen, die sich in Ihrem virtuellen Netzwerk befinden oder mit diesem verbunden sind:

* Ein integrierter Trigger oder eine integrierte Aktion mit der Bezeichnung **CORE**, z. B. der HTTP-Trigger oder die HTTP-Aktion
* Ein Connector mit der Bezeichnung **ISE** für dieses System oder diesen Dienst
* Ein benutzerdefinierter Connector

Sie können mit den Logik-Apps in Ihrer ISE weiterhin Connectors verwenden, die nicht über eine der Bezeichnungen **CORE** oder **ISE** verfügen. Diese Connectors werden stattdessen im mehrinstanzenfähigen Logic Apps-Dienst ausgeführt. Weitere Informationen finden Sie in den folgenden Abschnitten:

* [Isoliert im Vergleich zu mehrinstanzenfähig](#difference)
* [Herstellen einer Verbindung aus einer Integrationsdienstumgebung](../connectors/apis-list.md#integration-service-environment)
* [ISE-Connectors](../connectors/apis-list.md#ise-connectors)

> [!IMPORTANT]
> Für Logik-Apps, integrierte Trigger, integrierte Aktionen und Connectors, die in Ihrer ISE ausgeführt werden, gilt ein anderer als der nutzungsbasierte Tarif. Weitere Informationen finden Sie unter [Logic Apps – Preismodell](../logic-apps/logic-apps-pricing.md#fixed-pricing). Preisdetails finden Sie unter [Logik-Apps – Preise](../logic-apps/logic-apps-pricing.md).

Diese Übersicht enthält weitere Informationen dazu, wie eine ISE es Logik-Apps ermöglicht, direkt auf Ihr virtuelles Azure-Netzwerk zuzugreifen. Außerdem werden die Unterschiede zwischen einer ISE und dem mehrinstanzenfähigen Logic Apps-Dienst herausgestellt.

<a name="difference"></a>

## <a name="isolated-versus-multi-tenant"></a>Isoliert im Vergleich zu mehrinstanzenfähig

Wenn Sie Logik-Apps in einer ISE erstellen und ausführen, erhalten Sie die gleiche Benutzeroberfläche und ähnliche Funktionen wie mit dem mehrinstanzenfähigen Logic Apps-Dienst. Sie können dieselben integrierten Trigger, Aktionen und verwalteten Connectors verwenden, die im mehrinstanzenfähigen Logic Apps-Dienst verfügbar sind. Einige verwaltete Connectors bieten zusätzliche ISE-Versionen. Der Unterschied zwischen ISE-Connectors und Nicht-ISE-Connectors besteht darin, wo sie ausgeführt werden und welche Bezeichnungen im Logik-App-Designer angezeigt werden, wenn Sie in einer ISE arbeiten.

![Connectors mit und ohne Bezeichnungen in einer ISE](./media/connect-virtual-network-vnet-isolated-environment-overview/labeled-trigger-actions-integration-service-environment.png)

* Integrierte Trigger und Aktionen weisen die Bezeichnung **CORE** auf. Sie werden immer in derselben ISE wie Ihre Logik-App ausgeführt. Verwaltete Connectors, die die Bezeichnung **ISE** anzeigen, werden ebenfalls in derselben ISE wie Ihre Logik-App ausgeführt.

  Im Folgenden finden Sie beispielsweise einige Connectors, die ISE-Versionen bieten:

  * Azure Blob Storage, File Storage und Table Storage
  * Azure Queues, Azure Service Bus, Azure Event Hubs und IBM MQ
  * FTP und SFTP-SSH
  * SQL Server, Azure SQL Data Warehouse, Azure Cosmos DB
  * AS2, X12 und EDIFACT

* Verwaltete Connectors, die keine zusätzlichen Bezeichnungen aufweisen, werden immer im mehrinstanzenfähigen Logic Apps-Dienst ausgeführt, Sie können diese Connectors jedoch weiterhin in einer Logik-App in einer ISE verwenden.

<a name="on-premises"></a>

### <a name="access-to-on-premises-systems"></a>Zugriff auf lokale Systeme

Für den Zugriff auf lokale Systeme oder Datenquellen, die mit einem virtuellen Azure-Netzwerk verbunden sind, können Logik-Apps in einer ISE folgende Elemente verwenden:

* HTTP-Aktion

* Connector mit der Bezeichnung „ISE“ für dieses System

  > [!NOTE]
  > Wenn Sie Windows-Authentifizierung mit dem SQL Server-Connector in einer [Integrationsdienstumgebung (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)verwenden möchten, verwenden Sie die Nicht-ISE-Version des Connectors mit dem [lokalen Datengateway](../logic-apps/logic-apps-gateway-install.md). Die Version mit der Bezeichnung „ISE“ unterstützt keine Windows-Authentifizierung.

* Benutzerdefinierter Connector

  * Wenn Sie über benutzerdefinierte Connectors verfügen, für die das lokale Datengateway erforderlich ist, und Sie diese Connectors außerhalb einer ISE erstellt haben, können Logik-Apps in einer ISE diese Connectors ebenfalls verwenden.

  * Benutzerdefinierte Connectors, die in einer ISE erstellt wurden, funktionieren nicht mit dem lokalen Datengateway. Diese Connectors können jedoch direkt auf lokale Datenquellen zugreifen, die mit dem virtuellen Netzwerk verbunden sind, das die ISE hostet. Deshalb benötigen Logik-Apps in einer ISE sehr wahrscheinlich das Datengateway nicht, wenn sie mit diesen Ressourcen kommunizieren.

Bei lokalen Systemen, die nicht mit einem virtuellen Netzwerk verbunden sind oder keine Connectors mit der Bezeichnung „ISE“ aufweisen, müssen Sie zuerst [das lokale Datengateway einrichten](../logic-apps/logic-apps-gateway-install.md), bevor Ihre Logik-Apps eine Verbindung zu diesen Systemen aufbauen können.

<a name="ise-level"></a>

## <a name="ise-skus"></a>ISE-SKUs

Wenn Sie Ihre ISE erstellen, können Sie die Developer-SKU oder die Premium-SKU auswählen. Im Folgenden finden Sie die Unterschiede zwischen den beiden SKUs:

* **Developer**

  Bietet eine kostengünstigere ISE, die Sie für Experimente, Entwicklung und Tests, jedoch nicht für Produktion oder Leistungstests verwenden können. Die Developer-SKU umfasst integrierte Trigger und Aktionen, Standardconnectors, Unternehmensconnectors und ein einzelnes [Free-Tarif](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits)-Integrationskonto für einen festen monatlichen Preis. Diese SKU umfasst jedoch keine Vereinbarung zum Servicelevel (Service-Level Agreement, SLA), Optionen zum zentralen Hochskalieren der Kapazität oder Redundanz während der Wiederverwendung. Dies bedeutet, dass Verzögerungen oder Ausfallzeiten auftreten können.

* **Premium**

  Bietet eine ISE, die Sie für die Produktion verwenden können, und umfasst SLA-Unterstützung, integrierte Trigger und Aktionen, Standardconnectors, Unternehmensconnectors, ein einzelnes [Standard-Tarif](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits)-Integrationskonto, Optionen zum zentralen Hochskalieren der Kapazität und Redundanz während der Wiederverwendung für einen festen monatlichen Preis.

> [!IMPORTANT]
> Die SKU-Option ist nur bei der ISE-Erstellung verfügbar und kann später nicht mehr geändert werden.

Eine Preisübersicht finden Sie unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps/). Weitere Informationen zur Preisgestaltung und Abrechnung für ISEs finden Sie unter [Feststehendes Preismodell](../logic-apps/logic-apps-pricing.md#fixed-pricing).

<a name="endpoint-access"></a>

## <a name="ise-endpoint-access"></a>ISE-Endpunktzugriff

Beim Erstellen Ihrer ISE können Sie auswählen, ob interne oder externe Zugriffsendpunkte verwendet werden sollen. Ihre Auswahl bestimmt, ob Anforderungs- oder Webhooktrigger für Logik-Apps in Ihrer ISE Aufrufe von außerhalb Ihres virtuellen Netzwerks empfangen können.

Diese Endpunkte haben auch Einfluss auf die Methode, mit der Sie auf Eingaben und Ausgaben im Ausführungsverlauf Ihrer Logik-App zugreifen können.

* **Intern:** Private Endpunkte, die Aufrufe von Logik-Apps in Ihrer ISE *nur innerhalb des virtuellen Netzwerks* zulassen, wo Sie auf die Ein- und Ausgaben Ihrer Logik-Apps im Ausführungsverlauf zugreifen können.

* **Extern:** Öffentliche Endpunkte, die Aufrufe von Logik-Apps in Ihrer ISE *von außerhalb des virtuellen Netzwerks* zulassen, wo Sie auf die Ein- und Ausgaben Ihrer Logik-Apps im Ausführungsverlauf zugreifen können. Wenn Sie Netzwerksicherheitsgruppen (NSGs) verwenden, stellen Sie sicher, dass Sie mit eingehenden Regeln eingerichtet sind, um den Zugriff auf die Eingaben und Ausgaben des Ausführungsverlaufs zuzulassen. Weitere Informationen finden Sie unter [Aktivieren des Zugriffs für ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#enable-access).

> [!IMPORTANT]
> Die Zugriffsendpunktoption ist nur bei der ISE-Erstellung verfügbar und kann später nicht mehr geändert werden.

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Integrationskonten mit ISE

In einer Integrationsdienstumgebung (ISE) haben Sie die Möglichkeit, Integrationskonten mit Logik-Apps zu verwenden. Diese Integrationskonten müssen jedoch auf *dieselbe ISE* wie die verknüpften Logik-Apps zugreifen. Logik-Apps in einer ISE können nur auf die Integrationskonten verweisen, die sich in derselben ISE befinden. Wenn Sie ein Integrationskonto erstellen, können Sie Ihre ISE als Speicherort für Ihr Integrationskonto auswählen. Informationen zur Preisgestaltung und Abrechnung für Integrationskonten mit einer ISE finden Sie unter [Integrationskonten](../logic-apps/logic-apps-pricing.md#fixed-pricing). Eine Preisübersicht finden Sie unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps/).

## <a name="next-steps"></a>Nächste Schritte

* [Herstellen einer Verbindung mit virtuellen Azure-Netzwerken über Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* Erfahren Sie mehr über [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Erfahren Sie mehr über die [Integration virtueller Netzwerke für Azure-Dienste](../virtual-network/virtual-network-for-azure-services.md).
