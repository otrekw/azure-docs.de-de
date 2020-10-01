---
title: Einführung zum Azure Security-Vergleichstest
description: Einführung zum Security-Vergleichstest
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 53473b6be9e91adb636c3c9528e97ec644616115
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2020
ms.locfileid: "90058613"
---
# <a name="azure-security-benchmark-introduction"></a>Einführung zum Azure Security-Vergleichstest

Täglich werden in Azure neue Dienste und Features veröffentlicht, Entwickler veröffentlichen rasch neue Cloudanwendungen, die auf diesen Diensten basieren, und Angreifer suchen ständig nach neuen Wegen, um falsch konfigurierte Ressourcen auszunutzen. Die Cloud bewegt sich schnell, Entwickler bewegen sich schnell, und Angreifer sind ständig in Bewegung. Wie halten Sie Schritt und können die Sicherheit Ihrer Cloudbereitstellungen gewährleisten? Inwiefern unterscheiden sich Sicherheitspraktiken für Cloudsysteme von lokalen Systemen? Wie können Sie die Konsistenz über viele unabhängige Entwicklungsteams hinweg überwachen?

Microsoft hat festgestellt, dass die Verwendung von *Sicherheitsvergleichstests* Ihnen helfen kann, schnellen Schutz für Cloudbereitstellungen herzustellen. Die Vergleichstestempfehlungen Ihres Clouddienstanbieters stellen einen Ausgangspunkt für die Auswahl bestimmter Einstellungen für die Sicherheitskonfiguration in Ihrer Umgebung dar und ermöglichen es Ihnen, das Risiko für Ihr Unternehmen schnell zu reduzieren.

Der Azure-Sicherheitsvergleichstest enthält eine Sammlung von wirkungsvollen Sicherheitsempfehlungen, mit denen Sie die von Ihnen in Azure genutzten Dienste sichern können:

- Sicherheitskontrollelemente: Diese Empfehlungen gelten allgemein für Ihren Azure-Mandanten und die Azure-Dienste. Jede Empfehlung enthält eine Liste von Projektbeteiligter, die in der Regel an der Planung, Genehmigung oder Implementierung des Vergleichstests beteiligt sind. 
- Grundwerte für Dienste: Diese wenden die Kontrollelemente auf einzelne Azure-Dienste an, um Empfehlungen für die Sicherheitskonfiguration des jeweiligen Diensts zu geben.

## <a name="implement-the-azure-security-benchmark"></a>Implementieren des Azure-Sicherheitsvergleichstests
- **Planen** Sie die Implementierung Ihres Azure-Sicherheitsvergleichstests, indem Sie die [Dokumentation](overview.md) für die Unternehmenskontrollelemente und dienstspezifischen Grundwerte zurate ziehen, um Ihr Kontrollframework und dessen Einhaltung der Richtlinien wie CIS (Controls v7.1) und NIST (SP800-53) zu planen.
- **Überwachen** Sie die Compliance des Status Ihres Azure-Sicherheitsvergleichstests (und anderer Steuerelementsätze) mit dem Azure Security Center-[Dashboard für die Einhaltung gesetzlicher Bestimmungen](../../security-center/security-center-compliance-dashboard.md).
- **Richten Sie Schutzmaßnahmen ein**, um sichere Konfigurationen zu automatisieren und die Einhaltung des Azure-Sicherheitsvergleichstests (und anderer Anforderungen in Ihrer Organisation) mit Azure Blueprints und Azure Policy durchzusetzen.
 
Beachten Sie, dass Azure-Sicherheitsvergleichstest v2 die [bewährten Methoden für Microsoft-Sicherheit](/security/compass/microsoft-security-compass-introduction) (ehemals Kompass zur Azure-Sicherheit) erfüllt, sodass der Azure-Sicherheitsvergleichstest eine einzige konsolidierte Ansicht der Azure-Sicherheitsempfehlungen von Microsoft bietet.

## <a name="common-use-cases"></a>Gängige Anwendungsfälle

Der Azure-Sicherheitsvergleichstest wird häufig verwendet, um die folgenden für Kunden oder Servicepartner häufig auftretenden Herausforderungen anzugehen:
- Sie sind neu bei Azure und suchen nach bewährten Sicherheitsmethoden, um eine sichere Bereitstellung zu gewährleisten.
- Sie möchten den Sicherheitsstatus bei bestehenden Azure-Bereitstellungen verbessern, um die Maßnahmen zum Schutz vor den größten Risiken zu priorisieren.
- Genehmigen von Azure-Diensten für die Verwendung durch Technologie und geschäftliche Verwendung, um bestimmte Sicherheitsrichtlinien zu erfüllen.
- Erfüllung regulatorischer Anforderungen für Kunden aus staatlichen oder stark regulierten Branchen wie dem Finanz- und Gesundheitswesen (oder für Dienstleistungsanbieter, die Systeme für diese Kunden entwickeln müssen). Diese Kunden müssen sicherstellen, dass ihre Konfiguration von Azure den Sicherheitsfunktionen entspricht, die in einem Branchenframework wie z. B. CIS, NIST oder PCI festgelegt sind. Der Azure-Sicherheitsvergleichstest bietet einen effizienten Ansatz, bei dem die Kontrollelemente bereits auf diese Branchenvergleichstests abgestimmt sind.

## <a name="terminology"></a>Begriff

Die Begriffe „Kontrollelement“, „Vergleichstest“ und „Grundwerte“ kommen in der Dokumentation des Azure-Sicherheitsvergleichstests häufig vor, und es ist wichtig, zu verstehen, wie Azure diese Begriffe verwendet.


| Begriff | Beschreibung | Beispiel |
|--|--|--|
| Control | Ein Kontrollelement ist eine allgemeine Beschreibung eines Features oder einer Aktivität, das bzw. die berücksichtigt werden muss, und ist nicht spezifisch für eine Technologie oder Implementierung. | Datenschutz ist eines dieser Sicherheitskontrollelemente. Dieses Kontrollelement enthält spezifische Maßnahmen, die zur Gewährleistung des Datenschutzes erforderlich sind. |
| Vergleichstest | Ein Vergleichstest enthält Sicherheitsempfehlungen für eine bestimmte Technologie, wie z. B. Azure. Die Empfehlungen werden nach dem Kontrollelement kategorisiert, zu dem sie gehören. | Der Azure-Sicherheitsvergleichstest umfasst die für die Azure-Plattform spezifischen Sicherheitsempfehlungen. |
| Grundwert | Grundwerte bezieht sich auf die Implementierung des Vergleichstests für den jeweiligen Azure-Dienst. Jede Organisation entscheidet über eine Vergleichstestempfehlung, und im Rahmen der Azure-Implementierung sind entsprechende Konfigurationen erforderlich. | Die Firma Contoso versucht, die Azure SQL-Sicherheitsfeatures zu aktivieren, indem sie die in der Azure SQL-Sicherheitsbaseline empfohlene Konfiguration befolgt.

Wir freuen uns über Ihre Rückmeldungen zum Azure Security-Vergleichstest! Im Feedbackbereich unten können Sie uns gern Ihre Kommentare hinterlassen. Wenn Sie Ihr Feedback lieber diskreter an das Team des Azure Security-Vergleichstest weitergeben möchten, können Sie gern das Formular unter https://aka.ms/AzSecBenchmark ausfüllen.
