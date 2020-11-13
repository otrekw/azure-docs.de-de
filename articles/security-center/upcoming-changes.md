---
title: Wichtige bevorstehende Änderungen an Azure Security Center
description: Bevorstehende Änderungen an Azure Security Center, die Sie beachten sollten und für die Sie ggf. planen müssen
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2020
ms.author: memildin
ms.openlocfilehash: df863372cbf7abfb6fee145b7d13bb00d8deb074
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2020
ms.locfileid: "94380164"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Wichtige bevorstehende Änderungen an Azure Security Center

> [!IMPORTANT]
> Die Informationen auf dieser Seite beziehen sich auf Vorabversionen von Produkten oder Features, die vor einer möglichen Veröffentlichung für den Handel wesentlich verändert werden können. Microsoft gewährt in Bezug auf die hier angegebenen Informationen keine Zusicherungen oder Garantien, weder ausdrücklich noch konkludent.

Auf dieser Seite erfahren Sie, welche Änderungen für Security Center geplant sind. Es werden geplante Änderungen am Produkt beschrieben, die sich auf Dinge wie Ihre Sicherheitsbewertung oder Workflows auswirken könnten.

Die neuesten Versionshinweise finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).


## <a name="planned-changes"></a>Geplante Änderungen

### <a name="system-updates-should-be-installed-on-your-machines-recommendation-getting-sub-recommendations"></a>Empfehlung „Auf Ihren Computern sollten Systemupdates installiert werden“ mit Unterempfehlungen

#### <a name="summary"></a>Zusammenfassung

| Aspekt | Details |
|---------|---------|
|Ankündigungsdatum | 9\. November 2020  |
|Datum dieser Änderung  |  Mitte-Ende November 2020 |
|Auswirkung     | Während des Übergangs von der aktuellen zur neuen Version dieser Empfehlung kann sich Ihre Sicherheitsbewertung ändern. |
|  |  |

Wir veröffentlichen eine verbesserte Version der Empfehlung **Auf Ihren Computern sollten Systemupdates installiert werden**. Die neue Version *ersetzt* die aktuelle Version in der Sicherheitssteuerung „Systemupdates anwenden“ und bietet die folgenden Verbesserungen:

- Unterempfehlungen für jedes fehlende Update
- Eine neu gestaltete Benutzeroberfläche auf den Azure Security Center-Seiten des Azure-Portals
- Erweiterte Daten für die Empfehlung aus Azure Resource Graph

#### <a name="transition-period"></a>Übergangszeitraum

Es wird einen Übergangszeitraum von (ungefähr) 36 Std. geben. Um mögliche Unterbrechungen zu minimieren, wurde die Durchführung des Updates über ein Wochenende geplant. Während des Übergangs können Ihre Sicherheitsbewertung betroffen sein.

#### <a name="redesigned-portal-experience"></a>Neu gestaltete Benutzeroberfläche des Portals

Die Seite mit den Empfehlungsdetails für **Auf Ihren Computern sollten Systemupdates installiert werden** enthält die unten gezeigte Liste der Ergebnisse. Wenn Sie ein einzelnes Ergebnis auswählen, wird die Detailansicht mit einem Link zu den Informationen zur Risikominderung und einer Liste der betroffenen Ressourcen geöffnet.

:::image type="content" source="./media/upcoming-changes/system-updates-should-be-installed-subassessment.png" alt-text="Öffnen einer der Unterempfehlungen in der Benutzeroberfläche des Portals für die aktualisierte Empfehlung":::


#### <a name="richer-data-from-azure-resource-graph"></a>Ausführlichere Daten aus Azure Resource Graph

Azure Resource Graph ist ein Dienst in Azure, der für eine effiziente Untersuchung von Ressourcen konzipiert ist. Sie können ARG verwenden, um über einen bestimmten Satz von Abonnements Abfragen im großen Stil durchzuführen und Ihre Umgebung so effektiv zu verwalten. 

Für Azure Security Center können Sie ARG und die [Kusto-Abfragesprache (KQL)](https://docs.microsoft.com/azure/data-explorer/kusto/query/) verwenden, um eine Vielzahl von Daten zum Sicherheitsstatus abzufragen.

Wenn Sie die aktuelle Version von **Auf Ihren Computern sollten Systemupdates installiert werden** abfragen, ist die einzige von ARG bereitgestellte Information die, dass die Empfehlung auf einem Computer korrigiert werden muss. Wenn die aktualisierte Version veröffentlicht wird, gibt die folgende Abfrage alle fehlenden Systemupdates, gruppiert nach Computer, zurück.

```kusto
securityresources
| where type =~ "microsoft.security/assessments/subassessments"
| where extract(@"(?i)providers/Microsoft.Security/assessments/([^/]*)", 1, id) == "4ab6e3c5-74dd-8b35-9ab9-f61b30875b27"
| where properties.status.code == "Unhealthy"
```

## <a name="next-steps"></a>Nächste Schritte

Alle aktuellen Änderungen am Produkt finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).