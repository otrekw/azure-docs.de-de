---
title: Wichtige bevorstehende Änderungen an Azure Security Center
description: Bevorstehende Änderungen an Azure Security Center, die Sie beachten sollten und für die Sie ggf. planen müssen
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: overview
ms.date: 06/13/2021
ms.author: memildin
ms.openlocfilehash: a490a08946a7357af41cce04051ef01765c8fbe5
ms.sourcegitcommit: 23040f695dd0785409ab964613fabca1645cef90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112062242"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Wichtige bevorstehende Änderungen an Azure Security Center

> [!IMPORTANT]
> Die Informationen auf dieser Seite beziehen sich auf Vorabversionen von Produkten oder Features, die vor einer möglichen Veröffentlichung für den Handel wesentlich verändert werden können. Microsoft gewährt in Bezug auf die hier angegebenen Informationen keine Zusicherungen oder Garantien, weder ausdrücklich noch konkludent.

Auf dieser Seite erfahren Sie, welche Änderungen für Security Center geplant sind. Es werden geplante Änderungen am Produkt beschrieben, die sich auf Dinge wie Ihre Sicherheitsbewertung oder Workflows auswirken könnten.

Die neuesten Versionshinweise finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).


## <a name="planned-changes"></a>Geplante Änderungen

| Geplante Änderung                                                                                                                                                        | Voraussichtliches Datum der Änderung |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------|
| [Die Legacy-Implementierung von ISO 27001 wird durch die neue ISO-Norm 27001:2013 ersetzt.](#legacy-implementation-of-iso-27001-is-being-replaced-with-new-iso-270012013)          | Juni 2021                 |
| [Verbesserungen für die Empfehlung zur SQL-Datenklassifizierung](#enhancements-to-sql-data-classification-recommendation)                                                     | Q3 2021                   |
| [Aktivieren der Einbindung der Azure Defender-Sicherheitskontrolle in die Sicherheitsbewertung](#enable-azure-defender-security-control-to-be-included-in-secure-score)                       | Q3 2021                   |
|                                                                                                                                                                       |                           |


### <a name="legacy-implementation-of-iso-27001-is-being-replaced-with-new-iso-270012013"></a>Die Legacyimplementierung von ISO 27001 wird durch die neue ISO-Norm 27001:2013 ersetzt.

Die Legacyimplementierung von ISO 27001 wird vom Security Center-Dashboard für die Einhaltung gesetzlicher Bestimmungen entfernt. Wenn Sie Ihre ISO 27001-Konformität mit Security Center nachverfolgen, integrieren Sie den neuen Standard „ISO 27001:2013“ für alle relevanten Verwaltungsgruppen oder Abonnements. Die derzeitige Legacynorm „ISO 27001“ wird in Kürze vom Dashboard entfernt.

:::image type="content" source="media/upcoming-changes/removing-iso-27001-legacy-implementation.png" alt-text="Security Center-Dashboard zur Einhaltung gesetzlicher Bestimmungen mit der Meldung, dass die Legacyimplementierung von ISO 27001 entfernt wird" lightbox="media/upcoming-changes/removing-iso-27001-legacy-implementation.png":::

### <a name="enhancements-to-sql-data-classification-recommendation"></a>Verbesserungen für die Empfehlung zur SQL-Datenklassifizierung

**Geschätztes Datum für die Änderung:** Q3 2021

Die Empfehlung **Sensible Daten in Ihren SQL-Datenbanken müssen klassifiziert werden** in der Sicherheitskontrolle **Datenklassifizierung anwenden** wird durch eine neue Version ersetzt, die stärker an der Microsoft-Strategie zur Datenklassifizierung ausgerichtet ist. Infolgedessen ändert sich auch die ID der Empfehlung (derzeit b0df6f56-862d-4730-8597-38c0fd4ebd59).

### <a name="enable-azure-defender-security-control-to-be-included-in-secure-score"></a>Aktivieren der Einbindung der Azure Defender-Sicherheitskontrolle in die Sicherheitsbewertung

**Geschätztes Datum für die Änderung:** Q3 2021

Die Security Center-Empfehlungen zur Härtung sind in Form von Sicherheitskontrollen gruppiert. Bei jeder Sicherheitskontrolle handelt es sich um eine logische Gruppe mit verwandten Sicherheitsempfehlungen, die eine anfällige Angriffsfläche widerspiegelt. Der Beitrag jeder Sicherheitskontrolle zur gesamten Sicherheitsbewertung wird sowohl auf der Seite mit den Empfehlungen als auch unter [Sicherheitskontrollen und deren Empfehlungen](secure-score-security-controls.md#security-controls-and-their-recommendations) eindeutig angezeigt.

Seit ihrer Einführung verfügt die Sicherheitskontrolle **Azure Defender aktivieren** über eine maximal mögliche Bewertung von 0 Punkten. **Nach dieser Änderung trägt die Sicherheitskontrolle zu Ihrer Sicherheitsbewertung bei**.

Beim Aktivieren von Azure Defender erweitern Sie die Funktionen des kostenlosen Modus von Security Center auf Workloads, die in privaten und anderen öffentlichen Clouds ausgeführt werden. So erhalten Sie eine einheitliche Sicherheitsverwaltung und Schutz vor Bedrohungen für Ihre Hybrid Cloud-Workloads. Einige wichtige Features von Azure Defender sind: integrierte Microsoft Defender für Endpunkt-Lizenzen für Ihre Server, Sicherheitsrisikoüberprüfung für virtuelle Computer und Containerregistrierungen, Sicherheitswarnungen basierend auf erweiterten Verhaltensanalysen und maschinellem Lernen und Containersicherheitsfeatures. Eine vollständige Liste finden Sie unter [Azure Security Center Free und Azure Defender-Aktivierung](security-center-pricing.md).

Aufgrund dieser Änderung ergibt sich eine Auswirkung auf die Sicherheitsbewertung von Abonnements, die nicht durch Azure Defender geschützt sind. Wir empfehlen Ihnen, Azure Defender vor dieser Änderung zu aktivieren, um sicherzustellen, dass sich keine Auswirkungen auf Ihre Bewertungen ergeben. 

Erfahren Sie mehr in [Schnellstart: Aktivieren von Azure Defender](enable-azure-defender.md).


## <a name="next-steps"></a>Nächste Schritte

Alle aktuellen Änderungen am Produkt finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).