---
title: Vergleich der Features von Azure Stream Analytics
description: Dieser Artikel vergleicht die für die Azure Stream Analytics-Cloud und IoT Edge-Aufträge in Azure-Portal, Visual Studio und Visual Studio Code unterstützten Features.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 8cdba27ac949584e1fa96e3f7b0874f4fc0d4212
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75443656"
---
# <a name="azure-stream-analytics-feature-comparison"></a>Vergleich der Features von Azure Stream Analytics

Mit Azure Stream Analytics können Sie mit [Azure-Portal](stream-analytics-quick-create-portal.md), [Visual Studio](stream-analytics-quick-create-vs.md) und [Visual Studio Code](quick-create-vs-code.md) in der Cloud und in IoT Edge Streaminglösungen erstellen. Die Tabellen in diesem Artikel zeigen, welche Features von jeder Plattform für beide Auftragstypen unterstützt werden.

## <a name="cloud-job-features"></a>Cloudauftragsfeatures


|Funktion  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Plattformübergreifend     |Mac</br>Linux</br>Windows         |Windows        |Mac</br>Linux</br>Windows          |
|Erstellen von Skripts     |Ja         |Ja         |Ja         |
|IntelliSense-Skript     |Syntaxhervorhebung         |Syntaxhervorhebung</br>Codevervollständigung</br>Fehlermarkierung         |Syntaxhervorhebung</br>Codevervollständigung</br>Fehlermarkierung         |
|Definieren aller Typen von Eingaben, Ausgaben und Auftragskonfigurationen     |Ja         |Ja         |Ja         |
|Quellcodeverwaltung     |Nein         |Ja         |Ja         |
|CI/CD-Unterstützung     |Teilweise         |Ja         |Ja         |
|Mehrere Abfragen übergreifende Freigabe von Eingaben und Ausgaben     |Nein         |Ja         |Ja         |
|Abfragetests mit einer Beispieldatei     |Ja         |Ja        |Ja         |
|Lokales Testen mit Livedaten     |Nein         |Ja       |Ja      |
|Auflisten von Aufträgen und Anzeigen von Auftragsentitäten     |Ja         |Ja        |Ja         |
|Exportieren eines Auftrags in ein lokales Projekt     |Nein         |Ja         |Ja         |
|Senden, Starten und Beenden von Aufträgen     |Ja         |Ja         |Ja         |
|Anzeigen von Auftragsmetriken und Diagramm     |Ja         |Ja         |Im Portal öffnen         |
|Anzeigen von Auftragslaufzeitfehlern     |Ja         |Ja         |Nein         |
|Diagnoseprotokolle     |Ja         |Nein         |Nein         |
|Benutzerdefinierte Nachrichteneigenschaften     |Ja         |Ja         |Nein       |
|Benutzerdefinierte C#-Codefunktion und Deserialisierer|Schreibgeschützter Modus|Ja|Nein|
|JavaScript-UDF und -UDA     |Ja         |Ja         |Nur Windows         |
|Machine Learning Service     |Ja, aber die Abfrage kann nicht getestet werden        |Ja         |Nein         |
|Machine Learning Studio     |Ja, aber die Abfrage kann nicht getestet werden        |Ja |Nein         |
|Kompatibilitätsgrad     |1.0</br>1.1</br>1.2 (Standard)         |1.0</br>1.1</br>1.2 (Standard)           |1.0</br>1.1</br>1.2 (Standard)           |
|Integrierte Anomalieerkennungsfunktionen auf ML-Basis     |Ja         |Ja         |Ja         |
|Integrierte Geofunktionen     |Ja         |Ja         |Ja         |



## <a name="iot-edge-job-features"></a>IoT Edge-Auftragsfeatures

|Funktion  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Auftragserstellung     |Ja         |Ja         |Nein         |
|Quellcodeverwaltung     |Nein         |Ja         |Nein         |
|Exportieren eines Auftrags in ein lokales Projekt     |Nein         |Ja         |Nein         |
|Abfragetests mit einer Beispieldatei     |Ja         |Ja         |Nein         |
|Mehrere Abfragen übergreifende Freigabe von Eingaben und Ausgaben     |Nein         |Ja         |Nein         |
|C#-UDF     |Nein         |Ja         |Nein         |
|Übermitteln von Aufträgen     |Ja         |Ja         |Nein         |
|Auflisten von Aufträgen und Anzeigen von Auftragsentitäten     |Ja         |Ja         |Nein         |
|Anzeigen von Auftragsmetriken und Diagramm     |Ja         |Teilweise         |Nein         |
|Anzeigen von Auftragslaufzeitfehlern     |Ja         |Teilweise         |Nein         |
|CI/CD-Unterstützung     |Nein         |Nein         |Nein         |


## <a name="next-steps"></a>Nächste Schritte

* [Azure Stream Analytics auf IoT Edge](stream-analytics-edge.md)
* [Tutorial: Schreiben einer benutzerdefinierten C#-Funktion für einen Azure Stream Analytics IoT Edge-Auftrag (Vorschauversion)](stream-analytics-edge-csharp-udf.md)
* [Entwickeln von Stream Analytics IoT Edge-Aufträgen mit Visual Studio-Tools](stream-analytics-tools-for-visual-studio-edge-jobs.md)
* [Anzeigen von Azure Stream Analytics-Aufträgen mit Visual Studio](stream-analytics-vs-tools.md)
* [Erkunden von Azure Stream Analytics mit Visual Studio Code (Vorschauversion)](visual-studio-code-explore-jobs.md)


