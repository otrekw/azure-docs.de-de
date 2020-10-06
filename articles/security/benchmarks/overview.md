---
title: Übersicht über den Azure-Sicherheitsvergleichstest (V2)
description: Übersicht über den Azure-Sicherheitsvergleichstest (V2)
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 09/11/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 2be5df88a665c800f55f773b2470cc095fa27b2b
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2020
ms.locfileid: "90058681"
---
# <a name="overview-of-the-azure-security-benchmark"></a>Übersicht über den Vergleichstest für die Azure-Sicherheit

Der Azure-Sicherheitsvergleichstest bietet ausführliche bewährte Methoden und Empfehlungen, um die Sicherheit von Workloads, Daten und Diensten in Azure zu verbessern.

Dieser Vergleichstest ist Teil einer Gruppe ganzheitlichen Sicherheitsanleitungen, zu der auch Folgendes gehört:

- **Cloud Adoption Framework**: Leitfaden für Sicherheit, einschließlich [Strategie](/azure/cloud-adoption-framework/strategy/define-security-strategy), [Rollen und Verantwortlichkeiten](/azure/cloud-adoption-framework/organize/cloud-security), [10 wichtigste bewährte Methoden für die Sicherheit in Azure](/azure/cloud-adoption-framework/get-started/security#step-1-establish-essential-security-practices)und [Referenzimplementierung](/azure/cloud-adoption-framework/ready/enterprise-scale/).
- **Azure Well-Architected Framework​**: Leitfaden zum [Absichern Ihrer Workloads](https://docs.microsoft.com/assessments/?mode=pre-assessment&session=local) in Azure.
- **Bewährte Sicherheitsmethoden von Microsoft**: [Empfehlungen](/security/compass/microsoft-security-compass-introduction) mit Beispielen für Azure.
 Der Azure-Sicherheitsvergleichstest konzentriert sich auf cloudorientierte Kontrollbereiche. Diese Kontrollmechanismen stehen im Einklang mit bekannten Sicherheitsvergleichstests, wie sie beispielsweise vom Center for Internet Security (CIS) Controls, Version 7.1, und dem National Institute of Standards and Technology (NIST) SP800-53 beschrieben werden.
Die folgenden Steuerungsmechanismen werden im Azure-Sicherheitsvergleichstest verwendet:

| Kontrollbereiche für den Azure-Sicherheitsvergleichstest | Beschreibung 
|--|--|
| [Netzwerksicherheit](security-controls-v2-network-security.md) | Netzwerksicherheit umfasst Kontrollen zum Absichern und Schützen von Azure-Netzwerken, einschließlich der Absicherung virtueller Netzwerke, der Einrichtung privater Verbindungen, der Verhinderung und Eindämmung externer Angriffe und der Absicherung von DNS. |
| [Identitätsverwaltung](security-controls-v2-identity-management.md) | Identitätsverwaltung umfasst Kontrollen zum Einrichten sicherer Identitäts- und Zugriffssteuerungen mit Azure Active Directory, darunter einmaliges Anmelden, sichere Authentifizierungen, verwaltete Identitäten (und Dienstprinzipale) für Anwendungen, bedingter Zugriff und Überwachung von Anomalien bei Konten. |
| [Privilegierter Zugriff](security-controls-v2-privileged-access.md) | Privilegierter Zugriff umfasst Kontrollen zum Schutz des privilegierten Zugriffs auf Ihren Azure-Mandanten und Ressourcen, einschließlich einer Reihe von Kontrollmechanismen zum Schutz Ihres Verwaltungsmodells, von administrativen Konten und von Arbeitsstationen mit privilegiertem Zugriff vor bewussten und ungewollten Risiken. |
| [Datenschutz](security-controls-v2-data-protection.md) | Datenschutz umfasst die Kontrolle des Schutzes von Daten im Ruhezustand, während der Übertragung und über autorisierte Zugriffsmechanismen, einschließlich des Auffindens, Klassifizierens, Schützens und Überwachens sensibler Datenbestände mithilfe von Zugriffssteuerung, Verschlüsselung und Protokollierung in Azure. |
| [Ressourcenverwaltung](security-controls-v2-asset-management.md) | Die Ressourcenverwaltung umfasst Kontrollen zur Gewährleistung von Transparenz und Governance in Bezug auf die Sicherheit von Azure-Ressourcen, einschließlich Empfehlungen zu Berechtigungen für das Sicherheitspersonal, zum sicheren Zugriff auf den Ressourcenbestand und zur Verwaltung von Genehmigungen für Dienste und Ressourcen (Bestandsaufnahme, Nachverfolgung und Korrektur). |
| [Protokollierung und Bedrohungserkennung](security-controls-v2-logging-threat-detection.md) | Protokollierung und Bedrohungserkennung umfasst Kontrollen zur Erkennung von Bedrohungen in Azure und zum Aktivieren, Sammeln und Speichern von Überwachungsprotokollen für Azure-Dienste. Dazu gehört die Aktivierung von Erkennungs-, Untersuchungs- und Abhilfeprozessen mit Mechanismen zur Generierung nützlicher Warnungen mithilfe der nativen Bedrohungserkennung in Azure-Diensten. Außerdem gehört dazu das Sammeln von Protokollen mit Azure Monitor, die Zentralisierung der Sicherheitsanalyse mit Azure Sentinel, die Zeitsynchronisierung und die Aufbewahrung von Protokollen. |
| [Reaktion auf Vorfälle (Incident Response, IR)](security-controls-v2-incident-response.md) | Reaktion auf Vorfälle umfasst Kontrollen im Lebenszyklus der Reaktion auf Vorfälle. Dazu gehören Vorbereitung, Erkennung und Analyse, Eindämmung und Nachbereitung von Vorfällen, einschließlich der Nutzung von Azure-Diensten wie Azure Security Center und Azure Sentinel zur Automatisierung des Reaktionsprozesses auf Vorfälle. |
| [Status- und Sicherheitsrisikoverwaltung](security-controls-v2-posture-vulnerability-management.md) | Status- und Sicherheitsrisikoverwaltung konzentriert sich auf Kontrollen zur Bewertung und Verbesserung des Sicherheitsstatus von Azure, einschließlich Überprüfung auf Sicherheitsrisiken, Penetrationstests und Wartung sowie Nachverfolgung, Berichterstellung und Korrektur der Sicherheitskonfiguration von Azure-Ressourcen. |
| [Endpunktsicherheit](security-controls-v2-endpoint-security.md) | Endpunktsicherheit umfasst Kontrollen zur Endpunkterkennung und -antwort, einschließlich des Einsatzes von Endpunkterkennung und -antwort und eines Antischadsoftwarediensts für Endpunkte in Azure-Umgebungen. |
| [Sicherung und Wiederherstellung](security-controls-v2-backup-recovery.md) | Sicherung und Wiederherstellung umfasst Kontrollen, die sicherstellen, dass Daten- und Konfigurationssicherungen auf den verschiedenen Dienstebenen durchgeführt, überprüft und geschützt werden. |
| [Governance und Strategie](security-controls-v2-governance-strategy.md) | Governance und Strategie bietet Leitlinien für die Gewährleistung einer stimmigen Sicherheitsstrategie und eines dokumentierten Governance-Ansatzes zur Lenkung und Aufrechterhaltung der Sicherheitsgarantie, einschließlich Festlegung von Rollen und Verantwortlichkeiten für die verschiedenen Cloudsicherheitsfunktionen, einer einheitlichen technischen Strategie und der Unterstützung von Richtlinien und Standards. |

Sie können den Azure-Sicherheitsvergleichstest auch in einem [Tabellenformat](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Security%20Benchmark) herunterladen.

## <a name="azure-security-benchmark-recommendations"></a>Empfehlungen des Vergleichstests für die Azure-Sicherheit

Jede Empfehlung umfasst die folgenden Informationen:

- **Azure-ID**: Die Azure-Sicherheitsbenchmark-ID, die der Empfehlung entspricht.
- **CIS Controls v7.1 ID(s)** : Die Kontrollen in CIS Controls 7.1, die dieser Empfehlung entsprechen.
- **NIST SP800-53 r4 ID(s)** : Die Kontrollen in NIST SP800-53 r4 (moderat), die dieser Empfehlung entsprechen.
- **Details**: Enthält die Begründung für die Empfehlung sowie Links zu Implementierungsanleitungen. Wenn die Empfehlung von Azure Security Center unterstützt wird, ist dies hier ebenfalls angegeben.
- **Verantwortlichkeit**: Ob der Kunde, der Dienstanbieter oder beide für die Umsetzung dieser Empfehlung verantwortlich ist. Die Sicherheitsverantwortlichkeiten werden in der öffentlichen Cloud geteilt. Einige Sicherheitskontrollen sind nur für den Clouddienstanbieter verfügbar, weshalb der Anbieter für diese verantwortlich ist. Dabei handelt es sich um allgemeine Beobachtungen. Für einige einzelne Dienste unterscheidet sich die Verantwortlichkeit von den Angaben im Azure-Sicherheitsvergleichstest. Diese Unterschiede werden in den grundlegenden Empfehlungen für den jeweiligen Dienst beschrieben.
- **Sicherheitsverantwortliche beim Kunden**: Die Sicherheitsverantwortlichen in der Kundenorganisation, die für den jeweiligen Kontrollbereich ggf. verantwortlich, rechenschaftspflichtig oder ansprechbar sind. Sie können je nach der Sicherheitsorganisationsstruktur Ihres Unternehmens und den Rollen und Verantwortlichkeiten, die Sie im Zusammenhang mit der Azure-Sicherheit einrichten, von Organisation zu Organisation unterschiedlich sein.

> [!NOTE]
> Die Kontrollzuordnungen zwischen dem Azure-Sicherheitsvergleichstest und branchenüblichen Vergleichstests (wie NIST und CIS) weisen lediglich darauf hin, dass ein bestimmtes Azure-Feature verwendet werden kann, um eine in NIST oder CIS definierte Kontrollanforderung ganz oder teilweise zu erfüllen. Sie sollten sich darüber im Klaren sein, dass eine solche Implementierung nicht notwendigerweise zu einer vollständigen Einhaltung der entsprechenden Kontrollmechanismen in CIS oder NIST führt.

Wir freuen uns über Ihr detailliertes Feedback und die aktive Teilnahme an den Bemühungen im Zusammenhang mit dem Vergleichstest für die Azure-Sicherheit. Wenn Sie die dem Team für Azure-Sicherheitsvergleichstest eine unmittelbare Rückmeldung geben möchten, füllen Sie das Formular unter https://aka.ms/AzSecBenchmark aus.


- Schauen Sie sich die erste Sicherheitskontrolle an: [Netzwerksicherheit](security-control-network-security.md)
- Lesen Sie die [Einführung zum Vergleichstest für die Azure-Sicherheit](introduction.md)
- Laden Sie die [Excel-Tabelle mit Version 2 des Azure-Sicherheitsvergleichstests](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Security%20Benchmark) herunter.