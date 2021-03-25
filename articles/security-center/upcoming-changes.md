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
ms.date: 03/04/2021
ms.author: memildin
ms.openlocfilehash: 6bec9f0a1c22691d818566cec3f59c1ec0f3d3bb
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102051615"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Wichtige bevorstehende Änderungen an Azure Security Center

> [!IMPORTANT]
> Die Informationen auf dieser Seite beziehen sich auf Vorabversionen von Produkten oder Features, die vor einer möglichen Veröffentlichung für den Handel wesentlich verändert werden können. Microsoft gewährt in Bezug auf die hier angegebenen Informationen keine Zusicherungen oder Garantien, weder ausdrücklich noch konkludent.

Auf dieser Seite erfahren Sie, welche Änderungen für Security Center geplant sind. Es werden geplante Änderungen am Produkt beschrieben, die sich auf Dinge wie Ihre Sicherheitsbewertung oder Workflows auswirken könnten.

Die neuesten Versionshinweise finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).


## <a name="planned-changes"></a>Geplante Änderungen

- [Die Empfehlungen von AWS werden zur allgemeinen Verfügbarkeit (GA) veröffentlicht.](#recommendations-from-aws-will-be-released-for-general-availability-ga)
- [Von zwei Legacy-Empfehlungen werden keine Daten mehr direkt in das Azure-Aktivitätsprotokoll geschrieben](#two-legacy-recommendations-will-no-longer-write-data-directly-to-azure-activity-log)
- [Zwei Empfehlungen der Sicherheitskontrolle „Systemupdates anwenden“ als veraltet eingestuft](#two-recommendations-from-apply-system-updates-security-control-being-deprecated)
- [Verbesserungen für die Empfehlung zur SQL-Datenklassifizierung](#enhancements-to-sql-data-classification-recommendation)
- [Ausmusterung von elf Azure Defender-Warnungen](#deprecation-of-11-azure-defender-alerts)


### <a name="recommendations-from-aws-will-be-released-for-general-availability-ga"></a>Empfehlungen von AWS werden zur allgemeinen Verfügbarkeit (GA) veröffentlicht.

**Geschätztes Datum der Änderung:** April 2021

Azure Security Center schützt Workloads in Azure, Amazon Web Services (AWS) und Google Cloud Platform (GCP).

Die Empfehlungen von AWS Security Hub waren seit der Einführung der Cloud Connectors als Vorschauversion verfügbar. Empfehlungen, die als **Vorschau** gekennzeichnet sind, sind nicht in den Berechnungen Ihrer Sicherheitsbewertung enthalten, sollten aber nach Möglichkeit immer noch wieder bearbeitet werden, damit Sie nach Abschluss des Vorschauzeitraums zu ihrer Bewertung beitragen.

Mit dieser Änderung werden zwei von AWS-Empfehlungenspakete in die allgemeine Verfügbarkeit verschoben:

- [PCI-DSS Steuerelemente des Sicherheitshubs](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html)
- [AWS Foundations Benchmark-Steuerelemente des Sicherheitshubs](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html)

Wenn diese in allgemeiner Verfügbarkeit sind und die Bewertungen Ihrer AWS-Ressourcen ausgeführt werden, wirken sich die Ergebnisse auf die kombinierte Sicherheitsbewertung all Ihrer Multi- und Hybrid-Cloud-Ressourcen aus. 



### <a name="two-legacy-recommendations-will-no-longer-write-data-directly-to-azure-activity-log"></a>Von zwei Legacy-Empfehlungen werden keine Daten mehr in das Azure-Aktivitätsprotokoll geschrieben. 

**Geschätztes Datum für die Änderung:** März 2021

Von Security Center werden die Daten für nahezu alle Sicherheitsempfehlungen an Azure Advisor übergeben und anschließend von Azure Advisor in das [Azure-Aktivitätsprotokoll](../azure-monitor/essentials/activity-log.md) geschrieben.

Bei zwei Empfehlungen werden die Daten gleichzeitig direkt in das Azure-Aktivitätsprotokoll geschrieben. Diese Änderung sorgt dafür, dass Daten für diese Legacy-Sicherheitsempfehlungen von Security Center nicht mehr direkt in das Aktivitätsprotokoll geschrieben werden. Stattdessen werden die Daten genau wie bei allen anderen Empfehlungen in Azure Advisor exportiert. 

Die beiden Legacy-Empfehlungen sind:
-  Integritätsprobleme in Endpoint Protection sollten auf Ihren Computern behoben werden.
- Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer sollten beseitigt werden.

Wenn Sie auf Informationen für diese beiden Empfehlungen in der Kategorie „Empfehlung vom Typ TaskDiscovery“ des Aktivitätsprotokolls zugegriffen haben, ist dies nicht länger möglich.

### <a name="two-recommendations-from-apply-system-updates-security-control-being-deprecated"></a>Zwei Empfehlungen der Sicherheitskontrolle „Systemupdates anwenden“ als veraltet eingestuft 

**Geschätztes Datum für die Änderung:** März 2021

Für die beiden folgenden Empfehlungen ist geplant, sie im Februar 2021 als veraltet einzustufen:

- **Ihre Computer sollten neu gestartet werden, damit Systemupdates wirksam werden.** Dies kann zu geringfügigen Auswirkungen auf Ihre Sicherheitsbewertung führen.
- **Der Überwachungs-Agent sollte auf Ihren Computern installiert werden.** Diese Empfehlung gilt nur für lokale Computer. Ein Teil der Logik wird in eine andere Empfehlung übertragen: **Log Analytics-Agent-Integritätsprobleme müssen auf Computern gelöst werden**. Dies kann zu geringfügigen Auswirkungen auf Ihre Sicherheitsbewertung führen.

Wir empfehlen Ihnen, Ihre Konfigurationen für den fortlaufenden Export und die Workflowautomatisierung zu überprüfen, um zu ermitteln, ob diese Empfehlungen darin enthalten sind. Darüber hinaus sollten Sie alle Dashboards oder anderen Überwachungstools, für die diese ggf. genutzt werden, entsprechend aktualisieren.

Weitere Informationen zu diesen Empfehlungen finden Sie auf der [Referenzseite zu Sicherheitsempfehlungen](recommendations-reference.md).


### <a name="enhancements-to-sql-data-classification-recommendation"></a>Verbesserungen für die Empfehlung zur SQL-Datenklassifizierung

**Geschätztes Datum für die Änderung:** Q2 2021

Die Empfehlung **Sensible Daten in Ihren SQL-Datenbanken müssen klassifiziert werden** in der Sicherheitskontrolle **Datenklassifizierung anwenden** wird durch eine neue Version ersetzt, die stärker an der Microsoft-Strategie zur Datenklassifizierung ausgerichtet ist. Infolgedessen ändert sich auch die ID der Empfehlung (derzeit b0df6f56-862d-4730-8597-38c0fd4ebd59).


### <a name="deprecation-of-11-azure-defender-alerts"></a>Ausmusterung von elf Azure Defender-Warnungen

**Geschätztes Datum für die Änderung:** März 2021

Im nächsten Monat werden die unten aufgeführten elf Azure Defender-Warnungen ausgemustert.

- Neue Warnungen werden diese beiden Warnungen ersetzt und für eine bessere Abdeckung sorgen:

    | AlertType                | AlertDisplayName                                                         |
    |--------------------------|--------------------------------------------------------------------------|
    | ARM_MicroBurstDomainInfo | PREVIEW – MicroBurst toolkit „Get-AzureDomainInfo“ function run detected (VORSCHAU – Ausführung der MicroBurst-Toolkitfunktion „Get-AzureDomainInfo“ erkannt) |
    | ARM_MicroBurstRunbook    | PREVIEW – MicroBurst toolkit „Get-AzurePasswords“ function run detected (VORSCHAU – Ausführung der MicroBurst-Funktion „Get-AzurePasswords“ erkannt)  |
    |                          |                                                                          |

- Die folgenden neun Warnungen beziehen sich auf einen Azure Active Directory Identity Protection-Connector, der bereits veraltet ist:

    | AlertType           | AlertDisplayName              |
    |---------------------|-------------------------------|
    | UnfamiliarLocation  | Ungewöhnliche Anmeldeeigenschaften |
    | AnonymousLogin      | Anonyme IP-Adresse          |
    | InfectedDeviceLogin | Mit Schadsoftware verknüpfte IP-Adresse     |
    | ImpossibleTravel    | Ungewöhnlicher Ortswechsel               |
    | MaliciousIP         | Schädliche IP-Adresse          |
    | LeakedCredentials   | Kompromittierte Anmeldeinformationen            |
    | PasswordSpray       | Kennwortspray                |
    | LeakedCredentials   | Azure AD Threat Intelligence  |
    | AADAI               | Azure AD-KI                   |
    |                     |                               |
 



## <a name="next-steps"></a>Nächste Schritte

Alle aktuellen Änderungen am Produkt finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).
