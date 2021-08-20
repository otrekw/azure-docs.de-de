---
title: Wichtige bevorstehende Änderungen an Azure Security Center
description: Bevorstehende Änderungen an Azure Security Center, die Sie beachten sollten und für die Sie ggf. planen müssen
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: overview
ms.date: 07/25/2021
ms.author: memildin
ms.openlocfilehash: 2770c3532dd83051f9c2c7dcc770850e85aeae4b
ms.sourcegitcommit: 63f3fc5791f9393f8f242e2fb4cce9faf78f4f07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2021
ms.locfileid: "114689569"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Wichtige bevorstehende Änderungen an Azure Security Center

> [!IMPORTANT]
> Die Informationen auf dieser Seite beziehen sich auf Vorabversionen von Produkten oder Features, die vor einer möglichen Veröffentlichung für den Handel wesentlich verändert werden können. Microsoft gewährt in Bezug auf die hier angegebenen Informationen keine Zusicherungen oder Garantien, weder ausdrücklich noch konkludent.

Auf dieser Seite erfahren Sie, welche Änderungen für Security Center geplant sind. Es werden geplante Änderungen am Produkt beschrieben, die sich auf Dinge wie Ihre Sicherheitsbewertung oder Workflows auswirken könnten.

Die neuesten Versionshinweise finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).


## <a name="planned-changes"></a>Geplante Änderungen

| Geplante Änderung                                                                                                                                                                                          | Voraussichtliches Datum der Änderung |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------|
| [Die Legacy-Implementierung von ISO 27001 wird durch die neue ISO-Norm 27001:2013 ersetzt.](#legacy-implementation-of-iso-27001-is-being-replaced-with-new-iso-270012013)                                            | Juli 2021                 |
| [Empfehlung „Log Analytics-Agent-Integritätsprobleme müssen auf Computern gelöst werden“ als veraltet gekennzeichnet](#deprecating-recommendation-log-analytics-agent-health-issues-should-be-resolved-on-your-machines) | Juli 2021                 |
| [Logische Neuorganisation des Azure Defender für Resource Manager-Warnungen](#logical-reorganization-of-azure-defender-for-resource-manager-alerts)                                                           | August 2021               |
| [CSV-Exporte auf 20 MB beschränkt](#csv-exports-to-be-limited-to-20-mb)                                                                                                                               | August 2021               |
| [Aktivieren der Einbindung der Azure Defender-Sicherheitskontrolle in die Sicherheitsbewertung](#enable-azure-defender-security-control-to-be-included-in-secure-score)                                                         | Q3 2021                   |
| [Verbesserungen bei der Empfehlung zur Klassifizierung vertraulicher Daten in den SQL Datenbanken](#enhancements-to-recommendation-to-classify-sensitive-data-in-sql-databases)                                               | Q1 2022                   ||                                                                                                                                                                                                         |                           |


### <a name="legacy-implementation-of-iso-27001-is-being-replaced-with-new-iso-270012013"></a>Die Legacyimplementierung von ISO 27001 wird durch die neue ISO-Norm 27001:2013 ersetzt.

**Voraussichtliches Datum der Änderung:** Juli 2021

Die Legacyimplementierung von ISO 27001 wird vom Security Center-Dashboard für die Einhaltung gesetzlicher Bestimmungen entfernt. Wenn Sie Ihre ISO 27001-Konformität mit Security Center nachverfolgen, integrieren Sie den neuen Standard „ISO 27001:2013“ für alle relevanten Verwaltungsgruppen oder Abonnements. Die derzeitige Legacynorm „ISO 27001“ wird in Kürze vom Dashboard entfernt.

:::image type="content" source="media/upcoming-changes/removing-iso-27001-legacy-implementation.png" alt-text="Security Center-Dashboard zur Einhaltung gesetzlicher Bestimmungen mit der Meldung, dass die Legacyimplementierung von ISO 27001 entfernt wird" lightbox="media/upcoming-changes/removing-iso-27001-legacy-implementation.png":::

### <a name="deprecating-recommendation-log-analytics-agent-health-issues-should-be-resolved-on-your-machines"></a>Empfehlung „Log Analytics-Agent-Integritätsprobleme müssen auf Computern gelöst werden“ als veraltet gekennzeichnet

**Voraussichtliches Datum der Änderung:** Juli 2021

Wir haben festgestellt, dass die Empfehlung **Log Analytics-Agent-Integritätsprobleme müssen auf Computern gelöst werden** Auswirkungen auf die Sicherheitsbewertungen haben, die dem Schwerpunkt von Security Center Cloud Security Posture Management (CSPM) nicht entsprechen. In der Regel bezieht sich CSPM auf die Ermittlung von Fehlkonfigurationen bei der Sicherheit. Probleme mit der Agent-Integrität gehören nicht in diese Problemkategorie.

Darüber hinaus handelt es sich bei der Empfehlung im Gegensatz zu den anderen Agents im Zusammenhang mit Security Center um eine Anomalie: Dies ist der einzige Agent mit einer Empfehlung im Zusammenhang mit Integritätsproblemen.

Die Empfehlung wird in Kürze eingestellt.

Aufgrund dieser Einstellung nehmen wir auch geringfügige Änderungen an den Empfehlungen für die Installation des Log Analytics-Agents vor (**Der Log Analytics-Agent muss auf ... installiert sein**).

Diese Änderung wirkt sich mit hoher Wahrscheinlichkeit auf Ihre Sicherheitsbewertungen aus. Bei den meisten Abonnements erwarten wir, dass die Änderung zu einer höheren Bewertung führt. Es ist aber möglich, dass die Updates der Installationsempfehlung in einigen Fällen zu niedrigeren Bewertungen führen können.

> [!TIP]
> Diese Änderung wirkt sich auch auf die Seite [Ressourcenbestand](asset-inventory.md) aus, da sie auch Informationen darüber anzeigt, ob ein Computer überwacht, nicht überwacht oder teilweise überwacht wird (ein Zustand, der einen Agent mit Integritätsproblemen anzeigt). 


### <a name="logical-reorganization-of-azure-defender-for-resource-manager-alerts"></a>Logische Neuorganisation des Azure Defender für Resource Manager-Warnungen

**Geschätztes Datum der Änderung:** August 2021

Die unten aufgeführten Warnungen werden derzeit im Rahmen des [Azure Defender für Resource Manager](defender-for-resource-manager-introduction.md)-Plans bereitgestellt.

Im Rahmen einer logischen Neuorganisation für einige der Azure Defender-Pläne verschieben wir einzelne Warnungen vom **Azure Defender für Resource Manager** in den **Azure Defender für Server**.

Die Warnungen werden nach zwei Hauptprinzipien organisiert:

- Warnungen, die Schutz auf der Steuerungsebene für viele Azure-Ressourcentypen bieten, werden ein Teil des Azure Defender für Resource Manager
- Warnungen, die bestimmte Workloads schützen, werden in den entsprechenden Azure Defender-Plan verschoben, der sich auf diese Workload bezieht

Dies sind die Warnungen, die derzeit Teil des Azure Defender für Resource Manager sind und aufgrund dieser Änderung in den Azure Defender für Server verschoben werden:

- ARM_AmBroadFilesExclusion
- ARM_AmDisablementAndCodeExecution
- ARM_AmDisablement
- ARM_AmFileExclusionAndCodeExecution
- ARM_AmTempFileExclusionAndCodeExecution
- ARM_AmTempFileExclusion
- ARM_AmRealtimeProtectionDisabled
- ARM_AmTempRealtimeProtectionDisablement
- ARM_AmRealtimeProtectionDisablementAndCodeExec
- ARM_AmMalwareCampaignRelatedExclusion
- ARM_AmTemporarilyDisablement
- ARM_UnusualAmFileExclusion
- ARM_CustomScriptExtensionSuspiciousCmd
- ARM_CustomScriptExtensionSuspiciousEntryPoint
- ARM_CustomScriptExtensionSuspiciousPayload
- ARM_CustomScriptExtensionSuspiciousFailure
- ARM_CustomScriptExtensionUnusualDeletion
- ARM_CustomScriptExtensionUnusualExecution
- ARM_VMAccessUnusualConfigReset
- ARM_VMAccessUnusualPasswordReset
- ARM_VMAccessUnusualSSHReset

Weitere Informationen finden Sie unter [Azure Defender für Resource Manager](defender-for-resource-manager-introduction.md) und [Azure Defender für Server](defender-for-servers-introduction.md).


### <a name="csv-exports-to-be-limited-to-20-mb"></a>CSV-Exporte auf 20 MB beschränkt

**Geschätztes Datum der Änderung:** August 2021

Beim Exportieren von Daten zu Security Center-Empfehlungen gibt es derzeit keine Beschränkung für die Datenmenge, die Sie herunterladen können.

:::image type="content" source="media/upcoming-changes/download-csv-report.png" alt-text="Schaltfläche „CSV-Bericht herunterladen“ zum Exportieren von Empfehlungsdaten in Security Center":::

Mit dieser Änderung wird ein Grenzwert von 20 MB festgelegt.

Wenn Sie größere Datenmengen exportieren müssen, verwenden Sie vor der Auswahl die verfügbaren Filter, oder wählen Sie Teilmengen Ihrer Abonnements aus, und laden Sie die Daten in Batches herunter.

:::image type="content" source="media/upcoming-changes/filter-subscriptions.png" alt-text="Filtern von Abonnements im Azure-Portal":::

Erfahren Sie mehr über das [Ausführen eines CSV-Exports Ihrer Sicherheitsempfehlungen](continuous-export.md#manual-one-time-export-of-alerts-and-recommendations).

### <a name="enable-azure-defender-security-control-to-be-included-in-secure-score"></a>Aktivieren der Einbindung der Azure Defender-Sicherheitskontrolle in die Sicherheitsbewertung

**Geschätztes Datum für die Änderung:** Q3 2021

Die Security Center-Empfehlungen zur Härtung sind in Form von Sicherheitskontrollen gruppiert. Bei jeder Sicherheitskontrolle handelt es sich um eine logische Gruppe mit verwandten Sicherheitsempfehlungen, die eine anfällige Angriffsfläche widerspiegelt. Der Beitrag jeder Sicherheitskontrolle zur gesamten Sicherheitsbewertung wird sowohl auf der Seite mit den Empfehlungen als auch unter [Sicherheitskontrollen und deren Empfehlungen](secure-score-security-controls.md#security-controls-and-their-recommendations) eindeutig angezeigt.

Seit ihrer Einführung verfügt die Sicherheitskontrolle **Azure Defender aktivieren** über eine maximal mögliche Bewertung von 0 Punkten. **Nach dieser Änderung trägt die Sicherheitskontrolle zu Ihrer Sicherheitsbewertung bei**.

Beim Aktivieren von Azure Defender erweitern Sie die Funktionen des kostenlosen Modus von Security Center auf Workloads, die in privaten und anderen öffentlichen Clouds ausgeführt werden. So erhalten Sie eine einheitliche Sicherheitsverwaltung und Schutz vor Bedrohungen für Ihre Hybrid Cloud-Workloads. Einige wichtige Features von Azure Defender sind: integrierte Microsoft Defender für Endpunkt-Lizenzen für Ihre Server, Sicherheitsrisikoüberprüfung für virtuelle Computer und Containerregistrierungen, Sicherheitswarnungen basierend auf erweiterten Verhaltensanalysen und maschinellem Lernen und Containersicherheitsfeatures. Eine vollständige Liste finden Sie unter [Azure Security Center Free und Azure Defender-Aktivierung](security-center-pricing.md).

Aufgrund dieser Änderung ergibt sich eine Auswirkung auf die Sicherheitsbewertung von Abonnements, die nicht durch Azure Defender geschützt sind. Wir empfehlen Ihnen, Azure Defender vor dieser Änderung zu aktivieren, um sicherzustellen, dass sich keine Auswirkungen auf Ihre Bewertungen ergeben. 

Erfahren Sie mehr in [Schnellstart: Aktivieren von Azure Defender](enable-azure-defender.md).

### <a name="enhancements-to-recommendation-to-classify-sensitive-data-in-sql-databases"></a>Verbesserungen bei der Empfehlung zur Klassifizierung vertraulicher Daten in den SQL Datenbanken

**Geschätztes Datum für die Änderung:** 1. Quartal 2022

Die Empfehlung **Sensible Daten in Ihren SQL-Datenbanken müssen klassifiziert werden** in der Sicherheitskontrolle **Datenklassifizierung anwenden** wird durch eine neue Version ersetzt, die stärker an der Microsoft-Strategie zur Datenklassifizierung ausgerichtet ist. Infolgedessen ändert sich auch die ID der Empfehlung (derzeit b0df6f56-862d-4730-8597-38c0fd4ebd59).


## <a name="next-steps"></a>Nächste Schritte

Alle aktuellen Änderungen am Produkt finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).