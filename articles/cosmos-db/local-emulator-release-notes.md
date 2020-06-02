---
title: 'Azure Cosmos-Emulator: Download und Versionshinweise'
description: Hier finden Sie Hinweise zu verschiedenen Versionen des Azure Cosmos-Emulators sowie Downloadinformationen.
ms.service: cosmos-db
ms.topic: tutorial
author: milismsft
ms.author: adrianmi
ms.date: 06/20/2019
ms.openlocfilehash: 6dad625bbbcb8279e83ac42e1492c251d5b0299c
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83747007"
---
# <a name="azure-cosmos-emulator---release-notes-and-download-information"></a>Azure Cosmos-Emulator: Versionshinweise und Informationen zum Download

Dieser Artikel zeigt die Versionshinweise zum Azure Cosmos-Emulator mit einer Liste der Featureupdates, die in den Versionen vorgenommen wurden. Auch die zum Herunterladen und Verwenden verfügbare neueste Version des Emulators wird aufgeführt.

## <a name="download"></a>Download

| | |
|---------|---------|
|**MSI-Download**|[Microsoft Download Center](https://aka.ms/cosmosdb-emulator)|
|**Erste Schritte**|[Lokale Entwicklung mit dem Azure Cosmos-Emulator](local-emulator.md)|

## <a name="release-notes"></a>Versionshinweise

### <a name="2110"></a>2.11.0

- In dieser Version wird die Unterstützung für den per Autoskalierung bereitgestellten Durchsatz eingeführt. Mit diesen neuen Features können Sie benutzerdefiniert und in Form von Anforderungseinheiten (RU/s) die Stufe für den maximal bereitgestellten Durchsatz festlegen, die automatische Skalierung für vorhandene Datenbanken und Container aktivieren und die programmgesteuerte Unterstützung durch Azure Cosmos DB SDKs nutzen.
- Behoben wurde ein Problem bei der Abfrage umfangreicher Dokumente (mehr als 1 GB), bei der ein Emulatorfehler mit dem internen Fehlerstatuscode 500 auftrat.

### <a name="292"></a>2.9.2

- In dieser Version wurde ein Fehler beim Aktivieren des Supports für die MongoDB-Endpunktversion 3.2 behoben. Hinzugefügt wurde außerdem Unterstützung für das Erstellen von ETL-Ablaufverfolgungen für die Problembehandlung mit WPR anstelle von LOGMAN.

### <a name="291"></a>2.9.1

- In diesem Release wurden einige Probleme mit der Abfrage-API-Unterstützung behoben, und die Kompatibilität mit älteren Betriebssystemen wie Windows Server 2012 wurde wiederhergestellt.

### <a name="290"></a>2.9.0

- In diesem Release werden die Option zum Festlegen der Konsistenz auf ein konsistentes Präfix sowie die Option zum Erhöhen der Obergrenze für Benutzer und Berechtigungen hinzugefügt.

### <a name="272"></a>2.7.2

- In diesem Release wird dem Cosmos-Emulator Serverunterstützung für MongoDB-Version 3.6 hinzugefügt. Wenn Sie einen MongoDB-Endpunkt für Version 3.6 des Diensts starten möchten, starten Sie den Emulator über eine Administratorbefehlszeile mit der Option „/EnableMongoDBEndpoint=3.6“.

### <a name="270"></a>2.7.0

- In diesem Release wurde eine Regression behoben, die Benutzer am Ausführen von Abfragen für das SQL-API-Konto über den Emulator bei Verwendung von .NET Core- oder x86 .NET-basierten Clients hinderte.

### <a name="246"></a>2.4.6

- Diese Version bietet Parität mit den Features im Azure Cosmos-Dienst ab Juli 2019, mit den in [Lokale Entwicklung mit dem Azure Cosmos-Emulator](local-emulator.md) angegebenen Ausnahmen. Außerdem werden mehrere Fehler im Zusammenhang mit dem Herunterfahren des Emulators behoben, wenn es über die Befehlszeile aufgerufen wird, sowie mit Überschreibungen von internen IP-Adressen für SDK-Clients bei Konnektivität des direkten Modus.

### <a name="243"></a>2.4.3

- Das Starten des MongoDB-Diensts ist standardmäßig deaktiviert. Nur der SQL-Endpunkt ist standardmäßig aktiviert. Der Benutzer muss den Endpunkt manuell über die Befehlszeilenoption „/EnableMongoDbEndpoint“ des Emulators starten. Jetzt ist es wie bei allen anderen Dienstendpunkten wie Gremlin, Cassandra und Table.
- Ein Fehler im Emulator beim Start mit „/AllowNetworkAccess“ wurde behoben, bei dem die Endpunkte Gremlin, Cassandra und Table Anfragen von externen Clients nicht korrekt behandelten.
- Direkte Verbindungsports wurden den Einstellungen für Firewallregeln hinzugefügt.

### <a name="240"></a>2.4.0

- Es wurde das Problem behoben, dass der Emulator nicht gestartet werden konnte, wenn Netzwerküberwachungs-Apps wie Pulse Client auf dem Hostcomputer vorhanden waren.
