---
title: Überwachen von Azure Blockchain Service (ABS)
description: Überwachen von Azure Blockchain Service über Azure Monitor
ms.date: 01/08/2020
ms.topic: article
ms.reviewer: v-umha
ms.openlocfilehash: 6f2a91a8ffce67d3c4008a7587f2787f6446c341
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76293248"
---
# <a name="monitor-azure-blockchain-service-through-azure-monitor"></a>Überwachen von Azure Blockchain Service über Azure Monitor  

Wenn Kunden Blockchainszenarien in einer Produktionsumgebung mit Azure Blockchain Service (ABS) ausführen, ist es von entscheidender Bedeutung, die Ressourcen hinsichtlich Verfügbarkeit, Leistung und Vorgängen zu überwachen. Dieser Artikel beschreibt die Überwachungsdaten, die von Azure Blockchain Service generiert werden, und erläutert, wie die verschiedenen Funktionen und Integrationen von Azure Monitor zur Analyse und Benachrichtigung verwendet werden können, um eine Produktionsumgebung zu verwalten.  

## <a name="what-is-azure-monitor"></a>Was ist Azure Monitor?

Azure Blockchain Service erstellt Überwachungsdaten mit Azure Monitor. Dies ist ein Azure-Dienst zur vollständigen Stapelüberwachung, der sämtliche Funktionen für das Überwachen Ihrer Azure-Ressourcen bereitstellt. Weitere Informationen zu Azure Monitor finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource).
 

Die folgenden Abschnitte basieren auf diesem Artikel, indem sie die speziellen von Azure Blockchain Service gesammelten Daten beschreiben und Beispiele für die Konfiguration der Datensammlung und die Analyse dieser Daten mit Azure-Tools bereitstellen.

## <a name="monitor-data-collected-from-azure-blockchain-service"></a>Überwachen von Daten, die von Azure Blockchain Service gesammelt wurden  

Azure Blockchain Service sammelt dieselben Arten von Überwachungsdaten wie andere Azure-Ressourcen, die unter [Überwachungsdaten](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource#monitoring-data) von Azure-Ressourcen beschrieben sind. Eine ausführliche Referenz zu den Protokollen und Metriken, die von Azure Blockchain Service erstellt werden, finden Sie unter [Überwachen von Azure Blockchain Service-Daten – Referenz](#monitor-azure-blockchain-service-data-reference).

Die im Azure-Portal für jede Azure Blockchain Service-Mitgliedsressource angezeigte Übersichtsseite enthält eine Kurzansicht der Transaktionen, einschließlich der verwalteten und verarbeiteten Blöcke. Einige dieser Daten werden automatisch gesammelt und sind für Analysen verfügbar, sobald Sie die Azure Blockchain Service-Mitgliedsressource erstellt haben. Darüber hinaus können Sie mit einer weiteren Konfiguration zusätzliche Datensammlungen aktivieren.

## <a name="diagnostic-settings"></a>Diagnoseeinstellungen  

Plattformmetriken und das Aktivitätsprotokoll werden automatisch gesammelt, aber Sie müssen eine Diagnoseeinstellung erstellen, um Ressourcenprotokolle zu sammeln oder außerhalb von Azure Monitor weiterzuleiten. Ausführliche Informationen zum Erstellen einer Diagnoseeinstellung über das Azure-Portal, die Befehlszeilenschnittstelle oder PowerShell finden Sie unter [Erstellen einer Diagnoseeinstellung zum Sammeln von Plattformprotokollen und Metriken in Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings).

Wenn Sie eine Diagnoseeinstellung erstellen, legen Sie fest, welche Kategorien von Protokollen gesammelt werden sollen. Die Kategorien für Azure Blockchain Service sind nachstehend aufgeführt.

**Blockchain-Proxyprotokolle**: Wählen Sie diese Kategorie aus, wenn Sie die NGNIX-Proxyprotokolle überwachen möchten. Alle Details zu Kundentransaktionen sind für Überprüfung und Debuggen verfügbar.  

**Blockchainanwendungsprotokolle**: Wählen Sie diese Kategorie aus, um Protokolle der Blockchainanwendung abzurufen, die vom verwalteten Dienst gehostet wird. Für ein ABS-Quorum-Mitglied sind diese Protokolle z. B. die Protokolle des Quorums selbst.  

**Metrikanforderungen**: Wählen Sie diese Option aus, um Metrikdaten von Azure Cosmos DB zu den Zielen in der Diagnoseeinstellung zu sammeln. Diese Daten werden automatisch in Azure-Metriken gesammelt. Sammeln Sie Metrikdaten mit Ressourcenprotokollen, um beide Arten von Daten gemeinsam zu analysieren und Metrikdaten außerhalb von Azure Monitor zu senden.

## <a name="analyze-metric-data"></a>Analysieren von Metrikdaten  

Sie können Metriken für Azure Blockchain Service mit dem Metrik-Explorer analysieren. Navigieren Sie zur Registerkarte „Metriken“ im Abschnitt „Überwachung“ auf dem Blatt für ABS-Ressourcen. Ausführliche Informationen zur Verwendung dieses Tools finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started). Die vollständigen Metriken für Azure Blockchain Service befinden sich im Namespace für Azure Blockchain Service-Standardmetriken.

Sie können die **Knoten**-Dimension verwenden, wenn Sie einen Filter hinzufügen oder die Metriken teilen. Dadurch werden im Wesentlichen Metrikwerte pro Transaktionsknoten und Validierungsknoten des ABS-Mitglieds bereitgestellt.

## <a name="analyze-log-data"></a>Analysieren von Protokolldaten

Es folgen einige Abfragen, die Sie in die Leiste für Protokollsuchvorgänge eingeben können, um Ihre Azure Blockchain Service-Mitglieder zu verwalten. Diese Abfragen arbeiten mit der [neuen Sprache](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

Um die Fehlerbedingungen in den Blockchainanwendungsprotokollen abzufragen, verwenden Sie die folgende Abfrage:

```
BlockchainApplicationLog | where BlockchainMessage contains "ERROR" or BlockchainMessage contains "fatal"

```

Um die Fehlerbedingungen in den Blockchain-Proxyprotokollen abzufragen, verwenden Sie die folgende Abfrage:  


```
BlockchainProxyLog
| filter Code != 200
| limit 500

```
Mit den in Azure-Protokollen verfügbaren Zeitfiltern können Sie die Abfrage nach einem bestimmten Zeitbereich filtern.

## <a name="monitor-azure-blockchain-service-data-reference"></a>Überwachen von Azure Blockchain Service-Daten – Referenz  

Dieser Artikel enthält eine Referenz von Protokoll- und Metrikdaten, die gesammelt werden, um die Leistung und Verfügbarkeit von Azure Blockchain Service zu analysieren.  

### <a name="resource-logs"></a>Ressourcenprotokolle

Für alle Ressourcenprotokolle wird ein gemeinsames grundsätzliches Schema verwendet, das einige eindeutige Eigenschaften enthält, die nur für den Blockchain-Dienst gelten. Weitere Informationen finden Sie im Artikel [Ressourcenprotokollschema der obersten Ebene](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-schema#top-level-resource-logs-schema). Details zu den speziellen Azure Blockchain Service-Eigenschaften sind nachstehend aufgeführt.  

In der folgenden Tabelle sind die Eigenschaften für Azure Blockchain-Proxyprotokolle aufgelistet, wenn sie in Azure Monitor-Protokollen oder Azure Storage gesammelt werden.  


| Eigenschaftenname  | BESCHREIBUNG |
|:---|:---|
| time | Datum und Uhrzeit (UTC), zu denen der Vorgang aufgetreten ist. |
| resourceID  | Die Azure Blockchain Service-Ressource, für die Protokolle aktiviert sind.  |
| category  |Für Azure Blockchain Service sind **Proxylogs** und **Applicationlogs** die möglichen Werte. |
| operationName  | Der Name des Vorgangs, für den dieses Ereignis steht.   |
| Log level  | Standardmäßig ist für Azure Blockchain Service die Protokollebene **Informational** aktiviert.   |
| NodeLocation  | Die Azure-Region, in der das Blockchainmitglied bereitgestellt wird.  |
| BlockchainNodeName  | Der Name des Knotens des Azure Blockchain Service-Mitglieds, für den der Vorgang ausgeführt wurde.   |
| EthMethod  | Die-Methode, die vom zugrunde liegenden Blockchainprotokoll aufgerufen wird. In Quorum kann dies „eth_sendTransactions“, „eth_getBlockByNumber“ usw. sein.  |
| Agent  | Der Benutzer-Agent, der im Auftrag eines Benutzers agiert (z. B. ein Webbrowser wie Mozilla, Microsoft Edge usw.). Ein Beispiel mit entsprechenden Werten sieht wie folgt aus: „Mozilla/5.0 (Linux x64) node.js/8.16.0 v8/6.2.414.77“  |
| Code   | HTTP-Fehlercodes. Normalerweise sind 4XX und 5XX Fehlerbedingungen.  |
| NodeHost  | Der DNS-Name des Knotens.   |
| RequestMethodName | Die aufgerufene HTTP-Methode. Hier sind folgende Werte möglich: PUT für Erstellen eines Mitglieds, GET für Abrufen von Details eines vorhandenen Mitglieds, DELETE für Löschen eines Mitglieds, PATCH für Aktualisieren eines Mitglieds.   |
| BlockchainMemberName  | Der vom Benutzer bereitgestellte Name des Azure Blockchain Service-Mitglieds.  |
| Konsortium | Der Name des Konsortiums, wie er vom Benutzer bereitgestellt wurde.   |
| Remote  | Die IP-Adresse des Clients, von dem die Anforderung stammt.  |
| RequestSize  | Die Größe der gesendeten Anforderung in Bytes.  |
| RequestTime  | Die Dauer der Anforderung in Millisekunden.|




In der folgenden Tabelle sind die Eigenschaften für Azure Blockchain-Anwendungsprotokolle aufgeführt.


| Eigenschaftenname  | BESCHREIBUNG |
|:---|:---|
| time | Datum und Uhrzeit (UTC), zu denen der Vorgang aufgetreten ist. |
| resourceID  | Die Azure Blockchain Service-Ressource, für die Protokolle aktiviert sind.|
| category  |Für Azure Blockchain Service sind **Proxylogs** und **Applicationlogs** die möglichen Werte.  |
| operationName  | Der Name des Vorgangs, für den dieses Ereignis steht.   |
| Log level  | Standardmäßig ist für Azure Blockchain Service die Protokollebene **Informational** aktiviert.   |
| NodeLocation  | Die Azure-Region, in der das Blockchainmitglied bereitgestellt wird.  |
| BlockchainNodeName  | Der Name des Knotens des Azure Blockchain Service-Mitglieds, für den der Vorgang ausgeführt wurde.   |
| BlockchainMessage    | Dieses Feld enthält das Blockchainanwendungsprotokoll, bei dem es sich um die reinen Datenprotokolle handelt. Für ABS-Quorum wären hier Quorum-Protokolle enthalten. Das Protokoll enthält Informationen über den Typ des Protokolleintrags, d. h. Information (Informational), Fehler (error) oder Warnung (warning), und eine Zeichenfolge, die weitere Informationen zur ausgeführten Aktion angibt.   |
| Mandanten-ID    | Der regionsspezifische Mandant des Azure Blockchain Service. Dieses Felds hat das Format „https://westlake-rp-prod.<region>.cloudapp.Azure.com“, wobei „region“ die Azure-Region des bereitgestellten Mitglieds angibt.       |
| SourceSystem   | Das System, das die Protokolle schreibt. In diesem Fall ist dies **Azure**.    |



### <a name="metrics"></a>Metriken

In den nachstehenden Tabellen sind die für Azure Blockchain Service gesammelten Plattformmetriken aufgelistet. Alle Metriken werden im Namespace **Azure Blockchain Service**-Standardmetriken gespeichert.

Eine Liste aller von Azure Monitor unterstützten Metriken (einschließlich Azure Blockchain Service) finden Sie unter [Unterstützte Metriken von Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported).

### <a name="blockchain-metrics"></a>Blockchainmetriken

In der folgenden Tabelle sind die Blockchainmetriken angegeben, die für die Azure Blockchain Service-Mitgliedsressource erfasst werden.


| Metrikname | Einheit  |  Aggregationstyp| BESCHREIBUNG   |
|---|---|---|---|
| Ausstehende Transaktionen   | Anzahl  |  Average | Die Anzahl der Transaktionen, die auf Verarbeitung warten.   |
| Verarbeitete Blöcke   | Anzahl  | SUM  |  Die Anzahl der Blöcke, die in jedem Zeitintervall verarbeitet wurden. Derzeit beträgt die Blockgröße 5 Sekunden. Daher verarbeitet jeder Knoten in einer Minute 12 Blöcke und in 5 Minuten 60 Blöcke.   |
|Verarbeitete Transaktionen    | Anzahl  | SUM  | Die Anzahl der in einem Block verarbeiteten Transaktionen.    |
|Transaktionen in Warteschlange    |  Anzahl | Average  | Die Anzahl der Transaktionen, die nicht sofort verarbeitet werden konnten. Dies kann darauf zurückzuführen sein, dass sie nicht in der richtigen Reihenfolge angekommen waren und eine spätere Transaktion auf das Eintreffen einer vorherigen Transaktion gewartet hat. Es kann aber auch sein, dass zwei Transaktionen dieselbe Zahl, die nur einmal verwendet wird (Nonce), und denselben gas-Wert hatten, sodass die zweite Transaktion nicht verarbeitet werden konnte.   |

### <a name="connection-metrics"></a>Verbindungsmetriken  

In der folgenden Tabelle sind die unterschiedlichen Verbindungsmetriken aufgeführt, die für die Azure Blockchain Service-Mitgliedsressource erfasst werden. Diese Metriken sind NGINX-Proxymetriken.


| Metrikname | Einheit  |  Aggregationstyp| BESCHREIBUNG |
|---|---|---|---|
| Akzeptierte Verbindungen   | Anzahl  |  SUM | Die Gesamtzahl der akzeptierten Clientverbindungen.   |
| Die aktiven Verbindungen.  | Anzahl  | Average  |  Die aktuelle Anzahl der aktiven Clientverbindungen, einschließlich der wartenden Verbindungen.    |
|Behandelte Verbindungen    | Anzahl  | SUM  | Die Gesamtzahl der behandelten Verbindungen. Üblicherweise ist der Parameterwert mit dem der akzeptierten Verbindungen identisch, es sei denn, es wurden einige Ressourcengrenzwerte erreicht.     |
|Behandelte Anforderungen     |  Anzahl | SUM  | Die Gesamtzahl der Clientanforderungen.  |


### <a name="performance-metrics"></a>Leistungsmetriken

In der folgenden Tabelle sind die Leistungsmetriken aufgeführt, die für jeden Knoten der Azure Blockchain-Mitgliedsressource erfasst werden.  


| Metrikname | Einheit  |  Aggregationstyp| BESCHREIBUNG   |
|---|---|---|---|
| Prozentuale CPU-Auslastung   | Prozentwert  |  Max | Der Prozentsatz der CPU-Auslastung.     |
| Gelesene E/A-Bytes   | Kilobytes   | SUM  |  Die Summe der gelesenen E/A-Bytes über alle Knoten der Blockchain-Mitgliedsressource.      |
|Geschriebene E/A-Bytes     | Kilobytes   | SUM  | Die Summe der geschriebenen E/A-Bytes über alle Knoten der Blockchain-Mitgliedsressource.     |
|Arbeitsspeicherlimit       |  Gigabyte   | Average    | Maximaler Arbeitsspeicher, der für den Blockchainprozess pro Knoten verfügbar ist. |
|Speicherauslastung     | Gigabyte  |  Average | Die Menge des verwendeten Arbeitsspeichers als Mittelwert über alle Knoten hinweg.  |
| Prozentuale Arbeitsspeicherauslastung     | Prozentwert   | Average  |  Der Prozentsatz des verwendeten Arbeitsspeichers als Mittelwert über alle Knoten hinweg.       |
|Speicherauslastung      | Gigabyte   | Average  | Die GB des verwendeten Speichers als Mittelwert über alle Knoten hinweg.       |


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Blockchain Data Manager](https://docs.microsoft.com/azure/blockchain/service/data-manager), um Blockchaindaten zu erfassen und nach Azure Event Grid zu transformieren.
