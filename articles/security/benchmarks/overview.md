---
title: Vergleichstest für die Azure-Sicherheit – Übersicht
description: Vergleichstest für die Sicherheit – Übersicht
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: mbaldwin
ms.custom: security-baselines
ms.openlocfilehash: de6bbb81b04b93cce8ab7ddaf1d983cb7e4a700d
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2020
ms.locfileid: "76990588"
---
# <a name="overview-of-azure-security-controls"></a>Übersicht über Azure-Sicherheitssteuerungen

Der Vergleichstest für die Azure-Sicherheit enthält Empfehlungen, die Sie dabei unterstützen, die Sicherheit Ihrer Anwendungen und Daten auf Azure zu verbessern.

Dieser Vergleichstest konzentriert sich auf cloudzentrierte Steuerungsbereiche. Diese Steuerungen sind mit bekannten Sicherheitsbenchmarks konsistent – beispielsweise mit Version 7.1 der CIS-Steuerungen (Center for Internet Security).

Die folgenden Steuerungen werden im Vergleichstest für die Azure-Sicherheit verwendet: 

- [Netzwerksicherheit](security-control-network-security.md)
- [Protokollierung und Überwachung](security-control-logging-monitoring.md)
- [Identität und Zugriffssteuerung](security-control-identity-access-control.md)
- [Datenschutz](security-control-data-protection.md)
- [Verwaltung von Sicherheitsrisiken](security-control-vulnerability-management.md)
- [Bestands- und Ressourcenverwaltung](security-control-inventory-asset-management.md)
- [Sichere Konfiguration](security-control-secure-configuration.md)
- [Schutz vor Schadsoftware](security-control-malware-defense.md)
- [Datenwiederherstellung](security-control-data-recovery.md)
- [Reaktion auf Vorfälle](security-control-incident-response.md)
- [Penetrationstests und Red Team-Übungen](security-control-penetration-tests-red-team-exercises.md)

Sie können auch die [Excel-Tabelle mit Version 1 des Vergleichstests für die Azure-Sicherheit](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/spreadsheets) herunterladen.

## <a name="azure-security-benchmark-recommendations"></a>Empfehlungen des Vergleichstests für die Azure-Sicherheit 

Jede Empfehlung umfasst die folgenden Informationen: 

- **Azure-ID**: Die Azure-Sicherheitsbenchmark-ID, die der Empfehlung entspricht. 
- **CIS-ID(s)** : Die CIS-Vergleichstest-Empfehlungen, die dieser Empfehlung entsprechen.  
- **Verantwortlichkeit**: Ob der Kunde oder der Dienstanbieter (oder beide) für die Implementierung dieser Empfehlung verantwortlich ist. Die Sicherheitsverantwortlichkeiten werden in der öffentlichen Cloud geteilt. Einige Sicherheitskontrollen sind nur für den Clouddienstanbieter verfügbar, daher ist der Anbieter für diese verantwortlich. Dabei handelt es sich um allgemeine Beobachtungen – für einige einzelne Dienste unterscheidet sich die Verantwortlichkeit von den Angaben im Vergleichstest für die Azure-Sicherheit. Diese Unterschiede werden in den grundlegenden Empfehlungen für den jeweiligen Dienst beschrieben. 
- **Details**: Enthält die Begründung für die Empfehlung sowie Links zu Implementierungsanleitungen. Wenn die Empfehlung von Azure Security Center unterstützt wird, ist dies hier ebenfalls angegeben.

Wir freuen uns über Ihr detailliertes Feedback und die aktive Teilnahme an den Bemühungen im Zusammenhang mit dem Vergleichstest für die Azure-Sicherheit. Wenn Sie die dem Benchmarkteam eine unmittelbare Rückmeldung geben möchten, füllen Sie das Formular unter [https://aka.ms/AzSecBenchmark](https://aka.ms/AzSecBenchmark) aus.

## <a name="next-steps"></a>Nächste Schritte

- Schauen Sie sich die erste Sicherheitskontrolle an: [Netzwerksicherheit](security-control-network-security.md)
- Lesen Sie die [Einführung zum Azure Security-Vergleichstest](introduction.md).
- Laden Sie die [Excel-Tabelle mit Version 1 des Vergleichstests für die Azure-Sicherheit](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/spreadsheets) herunter.
