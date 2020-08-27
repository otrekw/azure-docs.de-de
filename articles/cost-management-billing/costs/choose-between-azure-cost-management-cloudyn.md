---
title: Wählen zwischen Azure Cost Management und Cloudyn
description: 'Anhand dieses Artikels können Sie ermitteln, welcher Dienst sich besser für Ihre Anforderungen in Bezug auf die Kostenverwaltung eignet: Azure Cost Management oder Cloudyn.'
author: bandersmsft
ms.author: banders
ms.date: 03/20/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: adwise
ms.openlocfilehash: a46e02bfa9590b5ef34affa45f3c468fba6999c2
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88684965"
---
# <a name="choose-between-azure-cost-management-and-cloudyn"></a>Wählen zwischen Azure Cost Management und Cloudyn

Cloudyn wird Ende 2020 eingestellt. Vorhandene Cloudyn-Funktionen werden nach Möglichkeit direkt in das Azure-Portal integriert. Mit Ausnahme von CSP-Kunden können zu diesem Zeitpunkt keine neuen Kunden integriert werden. Die Unterstützung für das vorhandene Produkt bleibt so lange erhalten, bis es vollständig eingestellt wurde.

Nach der Übernahme durch Microsoft werden die Cloudyn-Kostenverwaltungsfeatures aus dem Cloudyn-Portal nativ zu Azure migriert. Um die neuen Feature verwenden zu können, müssen Sie sich beim Azure-Portal anmelden und in der Liste mit den Azure-Diensten zu [Kostenverwaltung + Abrechnung](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) navigieren. Im Vergleich zu Cloudyn bietet die native Umgebung eine verbesserte Leistung sowie eine geringere Datenlatenz von etwa acht Stunden.

Die Migration wichtiger Features für Enterprise Agreement, nutzungsbasierte Zahlung und MSDN-Angebotskategorien zu Azure Cost Management ist abgeschlossen. CSP-Abonnements werden gerade zu Azure Cost Management migriert.

Sollte eine Ihrer Angebotskategorien noch nicht migriert worden sein, verwenden Sie am besten weiterhin das Cloudyn-Portal. Alle anderen können Azure Cost Management verwenden.

## <a name="recommended-services-by-offer"></a>Empfohlene Dienste nach Angebot

| Microsoft Azure-Angebote | Empfohlener Kostenverwaltungsdienst |
| --- | --- |
| Azure Enterprise Agreement | [Azure Cost Management](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| Azure Web Direct (PAYG/MSDN) | [Azure Cost Management](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| Azure Government | [Azure Cost Management](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| Microsoft-Kundenvereinbarung | [Azure Cost Management](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview)|
| Von Partnern unterstützte Microsoft-Kundenvereinbarung | [Azure Cost Management](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview)|
| Azure CSP | [Cloudyn](https://azure.cloudyn.com) |


## <a name="available-cost-management-features"></a>Verfügbare Cost Management-Features

Einige der folgenden Features sind in Cloudyn verfügbar. In Azure Cost Management stehen dagegen alle zur Verfügung.

- APIs
- Empfehlungen zur Azure-Kostenoptimierung, einschließlich, aber nicht beschränkt auf:
    - Empfehlungen zur richtigen Dimensionierung und zum Herunterfahren von Azure-Instanzen
    - Azure-Reservierungsempfehlungen
- Budgets
- Kostenanalyse
- Exportieren von Daten in ein Azure-Speicherkonto
- Kürzere Wartezeit
- Power BI-Vorlagen-App
- Unterstützung von Ressourcentags
- Unterstützung cloudübergreifender Kostenanalysen für AWS

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Azure Cost Management](../cost-management-billing-overview.md).