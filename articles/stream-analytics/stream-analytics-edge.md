---
title: Azure Stream Analytics auf IoT Edge
description: Erstellen von Edge-Aufträgen in Azure Stream Analytics und Bereitstellung dieser Aufträge auf Geräten, auf denen Azure IoT Edge ausgeführt wird.
ms.service: stream-analytics
author: an-emma
ms.author: raan
ms.topic: conceptual
ms.date: 12/18/2020
ms.custom: contperf-fy21q2
ms.openlocfilehash: c2a062b75caa84a0e3c342ca1ce45ccce12f2bb7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98012613"
---
# <a name="azure-stream-analytics-on-iot-edge"></a>Azure Stream Analytics auf IoT Edge
 
Mit Azure Stream Analytics unter IoT Edge können Entwickler Analyseinformationen nahezu in Echtzeit und näher an den IoT-Geräten bereitstellen, sodass die von den Geräten generierten Daten optimal verwertet werden können. Azure Stream Analytics ist für geringe Latenz, Resilienz, eine effiziente Bandbreitennutzung und Compliance konzipiert. So können Unternehmen eine Steuerlogik in der Nähe ihrer gewerblichen Betriebe bereitstellen und in der Cloud ausgeführte Big-Data-Analysen ergänzen.

Azure Stream Analytics auf IoT Edge wird innerhalb des [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/)-Frameworks ausgeführt. Nach dem Erstellen des Auftrags in Stream Analytics können Sie ihn mithilfe von IoT Hub bereitstellen und verwalten.

## <a name="common-scenarios"></a>Häufige Szenarios

In diesem Abschnitt werden die gängigen Szenarios für Stream Analytics in IoT Edge beschrieben. Das folgende Diagramm veranschaulicht den Datenfluss zwischen IoT-Geräten und der Azure-Cloud.

:::image type="content" source="media/stream-analytics-edge/edge-high-level-diagram.png" alt-text="Übersichtsdiagramm zu IoT Edge":::

### <a name="low-latency-command-and-control"></a>Latenzarme Befehls- und Steuerungsstruktur

Systeme für die Fertigungssicherheit müssen im Bedarfsfall praktisch verzögerungsfrei auf Betriebsdaten reagieren. Mit Stream Analytics in IoT Edge können Sie Sensordaten nahezu in Echtzeit analysieren und bei Erkennung von Anomalien Befehle ausgeben, um eine Maschine abzuschalten oder Warnungen auszugeben.

### <a name="limited-connectivity-to-the-cloud"></a>Eingeschränkte Cloudkonnektivität

Unternehmenswichtige Systeme wie beispielsweise ferngesteuerte Bergbaugeräte, vernetzte Schiffe oder Offshorebohrer müssen Daten auch dann analysieren und bei Bedarf reagieren können, wenn die Cloudverbindung nicht stabil ist. Mit Stream Analytics wird Ihre Streaminglogik unabhängig von der Netzwerkkonnektivität ausgeführt, und Sie können auswählen, welche Daten Sie zur weiteren Verarbeitung oder Speicherung in die Cloud senden möchten.

### <a name="limited-bandwidth"></a>Eingeschränkte Bandbreite

Das von Düsentriebwerken oder vernetzten Autos erzeugte Datenvolumen kann so groß werden, dass die Daten gefiltert oder vor dem Versand in die Cloud einer Vorverarbeitung unterzogen werden müssen. Mithilfe von Stream Analytics können Sie Daten, die in die Cloud gesendet werden sollen, filtern und aggregieren.

### <a name="compliance"></a>Konformität

Zur Einhaltung gesetzlicher Auflagen ist es möglicherweise erforderlich, einige Daten vor dem Senden in die Cloud lokal zu anonymisieren oder zu aggregieren.

## <a name="edge-jobs-in-azure-stream-analytics"></a>Edge-Aufträge in Azure Stream Analytics

Stream Analytics-Edgeaufträge werden in Containern ausgeführt, die auf [Azure IoT Edge-Geräten](../iot-edge/about-iot-edge.md) bereitgestellt sind. Edgeaufträge bestehen aus zwei Teilen:

* Einem Cloudteil, der für die Auftragsdefinition zuständig ist: Benutzer definieren Eingaben, Ausgaben, Abfragen und andere Einstellungen (z. B. Ereignisse in falscher Reihenfolge) in der Cloud.

* Einem Modul, das auf Ihren IoT-Geräten ausgeführt wird. Das Modul enthält die Stream Analytics-Engine und empfängt die Auftragsdefinition aus der Cloud. 

Stream Analytics verwendet IoT Hub zur Bereitstellung von Edgeaufträgen auf Geräten. Weitere Informationen finden Sie unter [IoT Edge-Bereitstellung](../iot-edge/module-deployment-monitoring.md).

:::image type="content" source="media/stream-analytics-edge/stream-analytics-edge-job.png" alt-text="Azure Stream Analytics-Edgeauftrag":::

## <a name="edge-job-limitations"></a>Einschränkungen für Edgeaufträge

Ziel ist die Herstellung von Parität zwischen IoT Edge- und Cloudaufträgen. Die meisten Features der SQL-Abfragesprache werden für Edge- und Cloudaufträge unterstützt. Allerdings werden die folgenden Features für Edgeaufträge nicht unterstützt:
* Benutzerdefinierte Funktionen (User-defined Functions, UDF) in JavaScript. UDF sind in [C# für IoT Edge-Aufträge](./stream-analytics-edge-csharp-udf.md) (Vorschau) verfügbar.
* Benutzerdefinierte Aggregate (User-Defined Aggregates, UDA).
* Azure ML-Funktionen
* AVRO-Format für die Eingabe/Ausgabe. Gegenwärtig werden nur CSV und JSON unterstützt.
* Folgende SQL-Operatoren:
    * PARTITION BY
    * GetMetadataPropertyValue
* Richtlinie für die Eingangsverzögerung

### <a name="runtime-and-hardware-requirements"></a>Runtime- und Hardwareanforderungen
Sie benötigen Geräte, die [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/) ausführen können, um Stream Analytics in IoT Edge ausführen zu können. 

Stream Analytics und Azure IoT Edge verwenden **Docker**-Container zum Bereitstellen einer portierbaren Lösung, die auf unterschiedlichen Hostbetriebssystemen (Windows, Linux) ausgeführt werden kann.

Stream Analytics in IoT Edge ist als Windows- und Linux-Image verfügbar, das auf x86-, x64- oder ARM-Architekturen (Advanced RISC Machines) ausgeführt werden kann. 


## <a name="input-and-output"></a>Eingabe und Ausgabe

Stream Analytics-Edgeaufträge können Eingaben und Ausgaben von anderen Modulen auf IoT Edge-Geräten abrufen. Um Verbindungen von und mit bestimmten Modulen herzustellen, können Sie die Routingkonfiguration zum Zeitpunkt der Bereitstellung festlegen. Weitere Informationen finden Sie in der [Dokumentation zur IoT Edge-Modulzusammenstellung](../iot-edge/module-composition.md).

Für Ein- und Ausgaben werden die Formate CSV und JSON unterstützt.

Für jeden Eingabe- und Ausgabestream, den Sie in Ihrem Stream Analytics-Auftrag erstellen, wird ein entsprechender Endpunkt in Ihrem bereitgestellten Modul erstellt. Diese Endpunkte können in den Routen Ihrer Bereitstellung verwendet werden.

Die folgenden Streameingabetypen werden unterstützt:
* Edge Hub
* Event Hub
* IoT Hub

Die folgenden Streamausgabetypen werden unterstützt:
* Edge Hub
* SQL-Datenbank
* Event Hub
* Blobspeicher/ADLS Gen2

Die Referenzeingabe unterstützt den Dateityp des Verweises. Andere Ausgaben lassen sich durch das Verwenden eines nachgeschalteten Cloudauftrags erreichen. Beispielsweise sendet ein in Edge gehosteter Stream Analytics-Auftrag eine Ausgabe an eine Edge Hub-Instanz, die sie dann an IoT Hub senden kann. Sie können einen zweiten in der Cloud gehosteten Azure Stream Analytics-Auftrag mit einer Eingabe aus IoT Hub verwenden und die Ausgabe in Power BI oder einem anderen Ausgabetyp erfolgen lassen.

## <a name="license-and-third-party-notices"></a>Lizenz- und Drittanbieterhinweise
* [Azure Stream Analytics auf IoT Edge-Lizenz](https://go.microsoft.com/fwlink/?linkid=862827). 
* [Drittanbieterhinweis zu Azure Stream Analytics auf IoT Edge](https://go.microsoft.com/fwlink/?linkid=862828).

## <a name="azure-stream-analytics-module-image-information"></a>Informationen zum Azure Stream Analytics-Modulimage 

Diese Versionsinformationen wurden zuletzt am 21.9.2020 aktualisiert:

- Image: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.9-linux-amd64`
   - Basisimage: mcr.microsoft.com/dotnet/core/runtime:2.1.13-alpine
   - Plattform:
      - Architektur: amd64
      - Betriebssystem: Linux
 
- Image: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.9-linux-arm32v7`
   - Basisimage: mcr.microsoft.com/dotnet/core/runtime:2.1.13-bionic-arm32v7
   - Plattform:
      - Architektur: ARM
      - Betriebssystem: Linux
 
- Image: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.9-linux-arm64`
   - Basisimage: mcr.microsoft.com/dotnet/core/runtime:3.0-bionic-arm64v8
   - Plattform:
      - Architektur: arm64
      - Betriebssystem: Linux
      
      
## <a name="get-help"></a>Hier erhalten Sie Hilfe
Weitere Unterstützung finden Sie auf der [Frageseite von Microsoft Q&A (Fragen und Antworten) zu Azure Stream Analytics](/answers/topics/azure-stream-analytics.html).

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu Azure IoT Edge](../iot-edge/about-iot-edge.md)
* [Tutorial: Stream Analytics in IoT Edge](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Entwickeln von Stream Analytics-Edge-Aufträgen mit Visual Studio-Tools](./stream-analytics-tools-for-visual-studio-edge-jobs.md)
* [Implementieren von CI/CD für Stream Analytics mithilfe von APIs](stream-analytics-cicd-api.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: /stream-analytics-query/stream-analytics-query-language-reference
[stream.analytics.rest.api.reference]: /rest/api/streamanalytics/
