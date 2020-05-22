---
title: Was ist Azure SQL Edge (Vorschau)?
description: Informationen zu Azure SQL Edge (Vorschau)
keywords: Einführung in SQL Edge, was ist SQL Edge, Übersicht über SQL Edge
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 6e5e54f7bf61d933240403ff952c1aa6e162958f
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83594579"
---
# <a name="what-is-azure-sql-edge-preview"></a>Was ist Azure SQL Edge (Vorschau)?

Azure SQL Edge (Vorschau) ist eine optimierte relationale Datenbank-Engine für IoT- und IoT Edge-Bereitstellungen. Azure SQL Database Edge bietet die Möglichkeit zur Erstellung eines Hochleistungs-Datenspeichers und einer Verarbeitungsschicht für IoT-Anwendungen und -Lösungen. Azure SQL Edge bietet Funktionen zum Streamen, Verarbeiten und Analysieren von relationalen und nicht relationalen Daten wie JSON-, Grafik- und Zeitreihendaten und ist damit die richtige Wahl für eine Vielzahl moderner IoT-Anwendungen.

Azure SQL Edge basiert auf den aktuellen Versionen der [Microsoft SQL Server-Datenbank-Engine](/sql/sql-server/sql-server-technical-documentation?toc=/azure/azure-sql-edge/toc.json), die branchenführende Leistung, Sicherheit und Funktionen für die Abfrageverarbeitung bietet. Da Azure SQL Edge auf der gleichen Engine wie SQL Server und Azure SQL-Datenbank basiert, bietet es die gleiche T-SQL-Programmieroberfläche für die einfachere und schnellere Entwicklung von Anwendungen oder Lösungen und vereinfacht gleichzeitig die Anwendungsportierbarkeit zwischen IoT Edge-Geräten, Rechenzentren und der Cloud.

> [!NOTE]
> Azure SQL Edge ist derzeit als Vorschau verfügbar und sollte daher NICHT in Produktionsumgebungen verwendet werden.

## <a name="deployment-models"></a>Bereitstellungsmodelle

Azure SQL Edge ist in Azure Marketplace verfügbar und kann als Modul für [Azure IoT Edge](../iot-edge/about-iot-edge.md) bereitgestellt werden. Weitere Informationen finden Sie unter [Bereitstellen von Azure SQL Edge](deploy-portal.md).<br>

![Abbildung: Übersicht über SQL Edge](media/overview/overview.png)

## <a name="editions-of-sql-edge"></a>Editionen von SQL Edge

SQL Edge wird in drei unterschiedlichen Editionen oder Softwareplänen bereitgestellt. Diese Editionen weisen denselben Umfang an Features auf und unterscheiden sich lediglich in Bezug auf die Nutzungsrechte und die Menge an unterstützten CPU-/Arbeitsspeicherressourcen.

   |**Planen**  |**Beschreibung**  |
   |---------|---------|
   |Azure SQL Edge Developer  |  Reine Entwicklungs-SKU, jeder SQL Edge-Container ist auf bis zu vier Kerne und 32 GB Arbeitsspeicher beschränkt.  |
   |Azure SQL Edge    |  Produktions-SKU, jeder SQL Edge-Container ist auf bis zu acht Kerne und 64 GB Arbeitsspeicher beschränkt. |

## <a name="pricing-and-availability"></a>Preise und Verfügbarkeit

Azure SQL Edge befindet sich zurzeit in der Vorschau. Weitere Informationen zu Preisen und Verfügbarkeit finden Sie unter [Azure SQL Edge](https://azure.microsoft.com/services/sql-database-edge/).

> [!IMPORTANT]
> Erläuterungen zu den funktionellen Unterschieden zwischen Azure SQL Edge und SQL Server sowie zu den Unterschieden zwischen den verschiedenen Optionen für Azure SQL Edge finden Sie unter [Unterstützte Funktionen von Azure SQL Edge](features.md).

## <a name="streaming-capabilities"></a>Streamingfunktionen  

Azure SQL Edge bietet integrierte Streamingfunktionen für die Echtzeitanalyse und die Verarbeitung komplexer Ereignisse. Die integrierte Streamingfunktionalität verwendet dieselben Konstrukte wie [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) und bietet ähnliche Funktionen wie [Azure Stream Analytics unter IoT Edge](../stream-analytics/stream-analytics-edge.md).

Die Streaming-Engine für Azure SQL Edge ist auf geringe Latenz, Resilienz, eine effiziente Bandbreitennutzung und Compliance ausgelegt. 

Weitere Informationen zum Streamen von Daten in SQL Edge finden Sie unter [Datenstreaming](stream-data.md)

## <a name="machine-learning-and-artificial-intelligence-capabilities"></a>Machine Learning-Funktionen und künstliche Intelligenz

Azure SQL Edge bietet integrierte ML- und Analysefunktionen durch Integration der ONNX-Runtime (Open Neural Network Exchange), die den Austausch von Deep Learning- und neuronalen Netzwerkmodellen zwischen verschiedenen Frameworks ermöglicht. Weitere Informationen zu ONNX finden Sie [hier](https://onnx.ai/). Die ONNX-Runtime bietet die Flexibilität, Modelle in einer Sprache oder mit den Tools Ihrer Wahl zu entwickeln und anschließend für die Ausführung in SQL Edge eine Konvertierung in das ONNX-Format durchzuführen. Weitere Informationen finden Sie unter [Machine Learning und künstliche Intelligenz mit ONNX in SQL Edge](onnx-overview.md).

## <a name="working-with-azure-sql-edge"></a>Arbeiten mit Azure SQL Edge

Azure SQL Edge sorgt für einfachere und produktivere Abläufe beim Entwickeln und Verwalten von Anwendungen. Benutzer können auf alle vertrauten Tools und Fähigkeiten zurückgreifen, um herausragende Apps und Lösungen für ihre IoT Edge-Anforderungen zu entwickeln. Benutzer können beispielsweise die folgenden Tools für die Entwicklung in SQL Edge nutzen:

- [Azure-Portal](https://portal.azure.com/): Eine webbasierte Anwendung für die Verwaltung aller Azure-Dienste.
- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms/): Eine kostenlose, herunterladbare Clientanwendung für die Verwaltung beliebiger SQL-Infrastrukturen – von SQL Server bis SQL-Datenbank.
- [SQL Server Data Tools in Visual Studio](/sql/ssdt/download-sql-server-data-tools-ssdt/): Eine kostenlose, herunterladbare Clientanwendung, mit der Sie relationale SQL Server-Datenbanken, SQL-Datenbanken, Integration Services-Pakete, Analysis Services-Datenmodelle und Reporting Services-Berichte erstellen können.
- [Azure Data Studio](/sql/azure-data-studio/what-is/): Ein kostenloses, herunterladbares, plattformübergreifendes Datenbanktool für Datenprofis, die die Microsoft-Produktfamilie aus lokalen und Clouddatenplattformen unter Windows, macOS und Linux nutzen.
- [Visual Studio Code](https://code.visualstudio.com/docs): Ein kostenloser, herunterladbarer Open-Source-Code-Editor für Windows, macOS und Linux. Dieser Editor unterstützt Erweiterungen, etwa die [Erweiterung MSSQLSERVER](https://aka.ms/mssql-marketplace), zum Abfragen von Microsoft SQL Server, Azure SQL-Datenbank und Azure SQL Data Warehouse.


## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von SQL Edge über das Azure-Portal](deploy-portal.md)
- [Machine Learning und künstliche Intelligenz mit SQL Edge](onnx-overview.md)
- [Erstellen einer End-to-End-IoT-Lösung mit SQL Edge](tutorial-deploy-azure-resources.md)
