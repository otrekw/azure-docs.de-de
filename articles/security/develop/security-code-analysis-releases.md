---
title: Releases der Microsoft-Sicherheitscodeanalyse
description: In diesem Artikel werden zukünftige Updates für die Erweiterung „Microsoft-Sicherheitscodeanalyse“ beschrieben.
author: sukhans
manager: sukhans
ms.author: terrylan
ms.date: 03/22/2021
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 7596df66dbcbe1b7cdefab4811da7174bc83ac65
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104801178"
---
# <a name="microsoft-security-code-analysis-releases-and-roadmap"></a>Releases und Roadmap für die Microsoft-Sicherheitscodeanalyse

> [!Note]
> Ab dem 1. März 2022 wird die Erweiterung „Microsoft-Sicherheitscodeanalyse“ (Microsoft Security Code Analysis, MSCA) eingestellt. MSCA-Kunden behalten ihren Zugriff auf die MSCA bis zum 1. März 2022. Das [OWASP-Tool für die Quellcodeanalyse](https://owasp.org/www-community/Source_Code_Analysis_Tools) bietet alternative Möglichkeiten in Azure DevOps. Kunden, die eine Migration zu GitHub planen, können sich über [GitHub Advanced Security](https://docs.github.com/github/getting-started-with-github/about-github-advanced-security) informieren.

Das Team für die Microsoft-Sicherheitscodeanalyse in Partnerschaft mit Developer Support ist stolz darauf, aktuelle und zukünftige Verbesserungen an unserer MSCA-Erweiterung ankündigen zu können.


## <a name="credential-scanner-v20-released-in-april-2020"></a>Credential Scanner v2.0: Veröffentlicht im April 2020

### <a name="innovations--improvements"></a>Neuerungen und Verbesserungen

- **Kern-Engine**

   - Aktualisierung der durchschnittlichen Leistung von 25 % mit nahezu linearen Laufzeiten
   - Kontext-/beweisbasierte Suche und Rangfolge für größere Genauigkeit
   - Verbesserungen an allgemeinen Kennworterkennungen und Abgleichslogik für offensichtliche Platzhalter (z. B. „fakePassword“)

- **Coverage**: Unterstützung für 25+ geheime Typen, einschließlich der folgenden am meisten angeforderten:

   - Passphrase für Fabric-Kontozertifikat
   - Geheimer Clientschlüssel/API-Schlüssel
   - HTTP-Autorisierungsheader
   - Zugriffsschlüssel für geheimen Amazon S3-Clientschlüssel
   - Zugriffstoken für Azure Active Directory-Client
   - Azure-Funktionsmaster/API-Schlüssel
   - Power BI-Zugriffsschlüssel
   - Kennwortmuster für Azure Resource Manager-Vorlage

- **Ausgaben**

   - Unterstützung für die Ausgabedateiformate von SARIF 2.1 und CSV-Datei

## <a name="binskim-v160-released-in-april-2020"></a>BinSkim v1.6.0: Veröffentlicht im April 2020

### <a name="improvements"></a>Verbesserungen

- FEATURE: Update auf endgültiges SARIF v2 (Version 2.1.16). Dieses Update ermöglicht das Zwischenspeichern von Ergebnissen bei der Übergabe von --Hashes in der Befehlszeile. Dies ist eine erhebliche Leistungsverbesserung beim rekursiven Analysieren von Verzeichnissen mit mehreren Kopien von Scanzielen.
- FEHLERBEHEBUNG: Fix typo in BA2021.DoNotMarkWritableSectionsAsExecutable output. (Tippfehler in Ausgabe „BA2021.DoNotMarkWritableSectionsAsExecutable“ behoben).
- LEISTUNG: Entfernen Sie das PDB-Laden bei allen nicht gemischten Modi für verwaltete Assemblys, einschließlich der Binärdateien für die IL-Bibliothek (Ahead-of-time-Kompilierung).
- FALSCH NEGATIVER FIX: Überprüfen Sie, ob eine PDB-Datei, die zusammen mit einer Binärdatei gespeichert wurde, mit der analysierten Binärdatei tatsächlich übereinstimmt.
- FEATURE: Geben Sie das Argument „--local-symbol-directories“ an, um zusätzliche (lokale, Nicht-Symbolserver) PDB-Suchspeicherorte anzugeben.
- FALSCH POSITIVER FIX: Überspringen Sie die PDB-gesteuerte Analyse für die generierte native .NET Core-Bootstrap-EXE-Datei (die vom Benutzer nicht gesteuert werden kann).

## <a name="whats-next-in-q3-cy20"></a>Wie geht es weiter in Q3 CY20?

- Java-Sicherheitsanalyse-Tool
- Python-Sicherheitsanalyse-Tool
- ES Lint zum Ersetzen von TS Lint für TypeScript und JavaScript
- Analysetool für Resource Manager-Vorlagen

## <a name="tool-deprecation-notification"></a>Benachrichtigung zur Einstellung des Tools

### <a name="microsoft-security-risk-detection-msrd-is-deprecated-on-june-26-2020"></a>Microsoft Security Risk Detection (MSDR) wird am 26. Juni 2020 eingestellt.

Der veraltete MSDR-Testdienst wird durch eine selbstgehostete Open-Source-Testplattform für Azure-Entwickler ersetzt. Diese Plattform befindet sich aktuell noch in der Entwicklung und wird zusammen mit mehreren Teams der Hauptprodukte von Microsoft getestet. In diese Testplattform werden Sanitizer integriert. Außerdem ist es möglich, anpassbare, sich weiterentwickelnde Tests in CI/CD-Pipelines zu integrieren, die im Laufe der Zeit entsprechend der Softwareprojekte größer werden. Das Open-Source-Release dieser Plattform ist für die zweite Hälfte des Jahres 2020 geplant.

## <a name="next-steps"></a>Nächste Schritte

Anweisungen zum Onboarding und zur Installation der Microsoft-Sicherheitscodeanalyse finden Sie in der Anleitung zu [Onboarding und Installation](security-code-analysis-onboard.md).

Wenn Sie weitere Fragen zur Erweiterung und zu den angebotenen Tools haben, lesen Sie unsere [Seite mit FAQs](security-code-analysis-faq.md).
