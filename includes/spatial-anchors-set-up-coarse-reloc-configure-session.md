---
author: bucurb
ms.author: bobuc
ms.date: 09/18/2019
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: 574003a150ef294aa6a2ebc035fe48cf877dec66
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "76545205"
---
## <a name="configure-the-cloud-spatial-anchor-session"></a>Konfigurieren der Raumanker-Cloudsitzung

Als Nächstes kümmern wir uns um die Konfiguration der Raumanker-Cloudsitzung. In der ersten Zeile legen wir den Sensoranbieter für die Sitzung fest. Ab jetzt werden alle Anker, die wir während der Sitzung erstellen, einer Gruppe von Sensormesswerten zugeordnet. Als Nächstes instanziieren wir Ermittlungskriterien für Geräte in der Nähe und führen eine Initialisierung durch, um eine Anpassung an die Anwendungsanforderungen vorzunehmen. Abschließend weisen wir die Sitzung an, beim Ermitteln von Ankern Sensordaten zu nutzen, indem wir einen „Watcher“ für unsere Kriterien für Geräte in der Nähe erstellen.