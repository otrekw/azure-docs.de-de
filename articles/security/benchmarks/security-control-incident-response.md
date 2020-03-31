---
title: 'Azure-Sicherheitskontrolle: Reaktion auf Vorfälle'
description: 'Sicherheitskontrolle: Reaktion auf Vorfälle'
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: fb3560aa2d3fbf48ab63c4da4d3a8d69cb677209
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934419"
---
# <a name="security-control-incident-response"></a>Sicherheitskontrolle: Reaktion auf Vorfälle

Schützen Sie die Informationen der Organisation sowie ihren Ruf durch die Entwicklung und Implementierung einer Infrastruktur zur Reaktion auf Vorfälle (z. B. Pläne, definierte Rollen, Schulung, Kommunikation, Überwachung durch das Management), um einen Angriff schnell zu ermitteln und dann den Schaden effektiv einzudämmen, die Präsenz des Angreifers zu beseitigen und die Integrität des Netzwerks und der Systeme wiederherzustellen.

## <a name="101-create-an-incident-response-guide"></a>10.1: Erstellen eines Leitfadens für die Reaktion auf Vorfälle

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 10.1 | 19.1, 19.2, 19.3 | Kunde |

Erarbeiten Sie einen Leitfaden für die Reaktion auf Vorfälle für Ihre Organisation. Stellen Sie sicher, dass es schriftliche Pläne für die Reaktion auf Vorfälle gibt, in denen alle Rollen der Mitarbeiter sowie die Phasen der Bearbeitung und Verwaltung von Vorfällen von der Ermittlung bis zur abschließenden Überprüfung definiert sind.

Konfigurieren von Workflowautomatisierungen in Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

Anleitung zum Entwickeln eines Prozesses für die Reaktion auf Sicherheitsvorfälle:

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Struktur eines Vorfalls laut Microsoft Security Response Center:

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Kunden können außerdem den Computer Security Incident Handling Guide des US-amerikanischen National Institute of Standards and Technology (NIST) nutzen, um einen Plan zur Reaktion auf Vorfälle zu entwickeln:

https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf

## <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Erstellen eines Verfahrens zur Bewertung und Priorisierung von Vorfällen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 10.2 | 19,8 | Kunde |

Security Center weist jeder Warnung einen Schweregrad zu, damit Sie priorisieren können, welche Warnungen zuerst untersucht werden sollen. Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

Kennzeichnen Sie außerdem die Abonnements (z. B. Produktion, Nicht-Produktion), und erstellen Sie ein Namenssystem, um Azure-Ressourcen eindeutig zu identifizieren und zu kategorisieren.

## <a name="103-test-security-response-procedures"></a>10.3: Verfahren zum Testen der Reaktion auf Sicherheitsvorfälle

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 10.3 | 19 | Kunde |

Führen Sie in regelmäßigen Abständen Tests zur Reaktionsfähigkeit Ihrer Systeme auf Vorfälle durch. Identifizieren Sie Schwachstellen und Lücken, und überarbeiten Sie den Plan bei Bedarf.

Informationen finden Sie in der folgenden Veröffentlichung des NIST: Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities:

https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

## <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Angeben von Kontaktdaten für Sicherheitsvorfälle und Konfigurieren von Warnungsbenachrichtigungen für Sicherheitsvorfälle

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 10,4 | 19.5 | Kunde |

Microsoft kontaktiert Sie unter den für Sicherheitsvorfälle angegebenen Kontaktdaten, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf die Kundendaten zugegriffen haben.  Überprüfen Sie die Vorfälle anschließend, um sicherzustellen, dass die Probleme behoben wurden.

Festlegen der Kontaktinformationen in Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

## <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Integrieren von Sicherheitswarnungen in das System zur Reaktion auf Vorfälle

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 10,5 | 19.6 | Kunde |

Exportieren Sie die Azure Security Center-Warnungen und -Empfehlungen über die Funktion „Fortlaufender Export“. Über „Fortlaufender Export“ können Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich exportieren. Sie können den Azure Security Center-Datenconnector verwenden, um die Warnungen an Sentinel zu streamen.

Konfigurieren des fortlaufenden Exports:

https://docs.microsoft.com/azure/security-center/continuous-export

Streamen von Warnungen in Azure Sentinel:

https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

## <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatisieren der Reaktion auf Sicherheitswarnungen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 10.6 | 19 | Kunde |

Verwenden Sie die Funktion „Workflowautomatisierung“ in Azure Security Center, um über &quot;Logic Apps&quot; automatisch Reaktionen auf Sicherheitswarnungen und -empfehlungen auszulösen.

Konfigurieren von „Workflowautomatisierung“ und Logic Apps:

https://docs.microsoft.com/azure/security-center/workflow-automation

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der nächsten Sicherheitskontrolle: [Penetrationstests und Red Team-Übungen](security-control-penetration-tests-red-team-exercises.md)