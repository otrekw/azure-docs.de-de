---
title: Wichtige bevorstehende Änderungen an Azure Security Center
description: Bevorstehende Änderungen an Azure Security Center, die Sie beachten sollten und für die Sie ggf. planen müssen
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: overview
ms.date: 04/27/2021
ms.author: memildin
ms.openlocfilehash: b8b8b15083711c868add7ac041514bcf1facc30d
ms.sourcegitcommit: 2e123f00b9bbfebe1a3f6e42196f328b50233fc5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108076885"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Wichtige bevorstehende Änderungen an Azure Security Center

> [!IMPORTANT]
> Die Informationen auf dieser Seite beziehen sich auf Vorabversionen von Produkten oder Features, die vor einer möglichen Veröffentlichung für den Handel wesentlich verändert werden können. Microsoft gewährt in Bezug auf die hier angegebenen Informationen keine Zusicherungen oder Garantien, weder ausdrücklich noch konkludent.

Auf dieser Seite erfahren Sie, welche Änderungen für Security Center geplant sind. Es werden geplante Änderungen am Produkt beschrieben, die sich auf Dinge wie Ihre Sicherheitsbewertung oder Workflows auswirken könnten.

Die neuesten Versionshinweise finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).


## <a name="planned-changes"></a>Geplante Änderungen

| Geplante Änderung                                                                                                                                                        | Voraussichtliches Datum der Änderung |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------|
| [Zwei Empfehlungen der Sicherheitskontrolle „Systemupdates anwenden“ als veraltet eingestuft](#two-recommendations-from-apply-system-updates-security-control-being-deprecated) | April 2021                |
| [Die Legacy-Implementierung von ISO 27001 wird durch die neue ISO-Norm 27001:2013 ersetzt.](#legacy-implementation-of-iso-27001-is-being-replaced-with-new-iso-270012013)          | Juni 2021                 |
| [Die Empfehlungen von AWS werden zur allgemeinen Verfügbarkeit (GA) veröffentlicht.](#recommendations-from-aws-will-be-released-for-general-availability-ga)                     | **August** 2021           |
| [Verbesserungen für die Empfehlung zur SQL-Datenklassifizierung](#enhancements-to-sql-data-classification-recommendation)                                                     | Q2 2021                   |
|                                                                                                                                                                       |                           |


### <a name="two-recommendations-from-apply-system-updates-security-control-being-deprecated"></a>Zwei Empfehlungen der Sicherheitskontrolle „Systemupdates anwenden“ als veraltet eingestuft

**Geschätztes Datum der Änderung:** April 2021

Die beiden folgenden Empfehlungen werden als veraltet eingestuft:

- **Die Betriebssystemversion sollte für Ihre Clouddienstrollen aktualisiert werden**: Azure aktualisiert Ihr Gastbetriebssystem standardmäßig in regelmäßigen Abständen auf das neueste Image innerhalb der BS-Familie, die Sie in der Dienstkonfiguration (CSCFG-Datei) angegeben haben (z B. Windows Server 2016).
- **Für Kubernetes Service muss ein Upgrade auf eine Kubernetes-Version ohne Sicherheitsrisiko durchgeführt werden**: Die Evaluierungen dieser Empfehlung sind uns nicht weitreichend genug. Die aktuelle Version dieser Empfehlung wird später durch eine verbesserte Version ersetzt, die besser auf die Sicherheitsanforderungen unserer Kunden abgestimmt ist.


### <a name="legacy-implementation-of-iso-27001-is-being-replaced-with-new-iso-270012013"></a>Die Legacyimplementierung von ISO 27001 wird durch die neue ISO-Norm 27001:2013 ersetzt.

Die Legacyimplementierung von ISO 27001 wird vom Security Center-Dashboard für die Einhaltung gesetzlicher Bestimmungen entfernt. Wenn Sie Ihre ISO 27001-Konformität mit Security Center nachverfolgen, integrieren Sie den neuen Standard „ISO 27001:2013“ für alle relevanten Verwaltungsgruppen oder Abonnements. Die derzeitige Legacynorm „ISO 27001“ wird in Kürze vom Dashboard entfernt.

:::image type="content" source="media/upcoming-changes/removing-iso-27001-legacy-implementation.png" alt-text="Security Center-Dashboard zur Einhaltung gesetzlicher Bestimmungen mit der Meldung, dass die Legacyimplementierung von ISO 27001 entfernt wird" lightbox="media/upcoming-changes/removing-iso-27001-legacy-implementation.png":::

### <a name="recommendations-from-aws-will-be-released-for-general-availability-ga"></a>Empfehlungen von AWS werden zur allgemeinen Verfügbarkeit (GA) veröffentlicht.

**Geschätztes Datum der Änderung:** August 2021

Azure Security Center schützt Workloads in Azure, Amazon Web Services (AWS) und Google Cloud Platform (GCP).

Die Empfehlungen von AWS Security Hub waren seit der Einführung der Cloud Connectors als Vorschauversion verfügbar. Empfehlungen, die als **Vorschau** gekennzeichnet sind, sind nicht in den Berechnungen Ihrer Sicherheitsbewertung enthalten, sollten aber nach Möglichkeit immer noch wieder bearbeitet werden, damit Sie nach Abschluss des Vorschauzeitraums zu ihrer Bewertung beitragen.

Mit dieser Änderung werden zwei von AWS-Empfehlungenspakete in die allgemeine Verfügbarkeit verschoben:

- [PCI-DSS Steuerelemente des Sicherheitshubs](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html)
- [AWS Foundations Benchmark-Steuerelemente des Sicherheitshubs](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html)

Wenn diese in allgemeiner Verfügbarkeit sind und die Bewertungen Ihrer AWS-Ressourcen ausgeführt werden, wirken sich die Ergebnisse auf die kombinierte Sicherheitsbewertung all Ihrer Multi- und Hybrid-Cloud-Ressourcen aus.



### <a name="enhancements-to-sql-data-classification-recommendation"></a>Verbesserungen für die Empfehlung zur SQL-Datenklassifizierung

**Geschätztes Datum für die Änderung:** Q2 2021

Die Empfehlung **Sensible Daten in Ihren SQL-Datenbanken müssen klassifiziert werden** in der Sicherheitskontrolle **Datenklassifizierung anwenden** wird durch eine neue Version ersetzt, die stärker an der Microsoft-Strategie zur Datenklassifizierung ausgerichtet ist. Infolgedessen ändert sich auch die ID der Empfehlung (derzeit b0df6f56-862d-4730-8597-38c0fd4ebd59).



## <a name="next-steps"></a>Nächste Schritte

Alle aktuellen Änderungen am Produkt finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).