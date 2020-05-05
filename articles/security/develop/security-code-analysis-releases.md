---
title: Releases der Microsoft-Sicherheitscodeanalyse
description: In diesem Artikel werden zukünftige Updates für die Erweiterung „Microsoft-Sicherheitscodeanalyse“ beschrieben.
author: sukhans
manager: sukhans
ms.author: terrylan
ms.date: 04/24/2020
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: d4281d3b6132e551283a71cd1801ef462fbfc68c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146125"
---
# <a name="microsoft-security-code-analysis-releases-and-roadmap"></a>Releases und Roadmap für die Microsoft-Sicherheitscodeanalyse

Das Team für die Microsoft-Sicherheitscodeanalyse in Partnerschaft mit Developer Support ist stolz darauf, aktuelle und zukünftige Verbesserungen an unserer MSCA-Erweiterung ankündigen zu können. Weitere Informationen finden Sie in der Roadmap weiter unten.

![Releases](./media/security-code-analysis-releases/releases.png)

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

- FEATURE: Update auf endgültiges SARIF v2 (Version 2.1.16). Dies ermöglicht das Zwischenspeichern von Ergebnissen bei der Übergabe von --Hashes in der Befehlszeile – eine erhebliche Leistungsverbesserung beim rekursiven Analysieren von Verzeichnissen mit mehreren Kopien von Scanzielen.
- FEHLERBEHEBUNG: Fix typo in BA2021.DoNotMarkWritableSectionsAsExecutable output. (Tippfehler in Ausgabe „BA2021.DoNotMarkWritableSectionsAsExecutable“ behoben).
- LEISTUNG: Entfernen Sie das PDB-Laden bei allen nicht gemischten Modi für verwaltete Assemblys, einschließlich der Binärdateien für die IL-Bibliothek (Ahead-of-time-Kompilierung).
- FALSCH NEGATIVER FIX: Überprüfen Sie, ob eine PDB-Datei, die zusammen mit einer Binärdatei gespeichert wurde, mit der analysierten Binärdatei tatsächlich übereinstimmt.
- FEATURE: Geben Sie das Argument „--local-symbol-directories“ an, um zusätzliche (lokale, Nicht-Symbolserver) PDB-Suchspeicherorte anzugeben.
- FALSCH POSITIVER FIX: Überspringen Sie die PDB-gesteuerte Analyse für die generierte native .NET Core-Bootstrap-EXE-Datei (die vom Benutzer nicht gesteuert werden kann).

## <a name="whats-next-in-fy20"></a>Wie geht es weiter in FY20?

- Java-Sicherheitsanalyse-Tool
- Python-Sicherheitsanalyse-Tool
- ES Lint zum Ersetzen von TS Lint für TypeScript und JavaScript

## <a name="next-steps"></a>Nächste Schritte

Anweisungen zum Onboarding und zur Installation der Microsoft-Sicherheitscodeanalyse finden Sie in der Anleitung zu [Onboarding und Installation](security-code-analysis-onboard.md).

Wenn Sie weitere Fragen zur Erweiterung und zu den angebotenen Tools haben, lesen Sie unsere [Seite mit FAQs](security-code-analysis-faq.md).
