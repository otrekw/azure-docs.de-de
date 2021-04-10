---
title: Pullen von Azure Industrial IoT-Daten in ADX
description: In diesem Tutorial erfahren Sie, wie Sie IIoT-Daten in ADX pullen.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: tutorial
ms.date: 3/22/2021
ms.openlocfilehash: 4c344dc09ad6c8aa4b2aa431952fc271d946b60d
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104787240"
---
# <a name="tutorial-pull-azure-industrial-iot-data-into-adx"></a>Tutorial: Pullen von Azure Industrial IoT-Daten in ADX

Die IIoT-Plattform (Azure Industrial IoT) kombiniert Edge-Module und cloudbasierte Microservices mit einer Reihe von Azure-PaaS-Diensten, um Funktionen für die Ermittlung von Industrial-Assets und die Erfassung von Daten aus diesen Assets per OPC UA zu ermöglichen. [Azure Data Explorer (ADX)](https://docs.microsoft.com/azure/data-explorer) ist ein natürliches Ziel für IIoT-Daten und bietet Datenanalysefeatures, die flexible Abfragen für die erfassten Daten der OPC UA-Server ermöglichen, die über OPC Publisher mit der IoT Hub-Instanz verbunden sind. Von einem ADX-Cluster können Daten zwar direkt aus der IoT Hub-Instanz erfasst werden, von der IIoT-Plattform werden die Daten jedoch noch weiter verarbeitet, um ihren Nutzen zu erhöhen, bevor sie auf dem Event Hub platziert werden, der bei Verwendung einer vollständigen Bereitstellung der Microservices bereitgestellt wird. (Sehen Sie sich dazu die IIoT-Plattformarchitektur an.)

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Erstellen einer Tabelle in ADX
> * Herstellen einer Verbindung zwischen Event Hub und ADX-Cluster
> * Analysieren der Daten in ADX

## <a name="how-to-make-the-data-available-in-the-adx-cluster-to-query-it-effectively"></a>Bereitstellen der Daten im ADX-Cluster, um effektive Abfragen zu ermöglichen 

Das durch die Klasse „Microsoft.Azure.IIoT.OpcUa.Subscriber.Models.MonitoredItemMessageModel“ definierte Event Hub-Nachrichtenformat lässt auf die Struktur schließen, die für das ADX-Tabellenschema benötigt wird.

![Struktur](media/tutorial-iiot-data-adx/industrial-iot-in-azure-data-explorer-pic-1.png)

Im Anschluss finden Sie die Schritte, die ausgeführt werden müssen, um die Daten im ADX-Cluster verfügbar zu machen und effektive Abfragen zu ermöglichen.  
1. Erstellen Sie einen ADX-Cluster. Falls Sie mit der IIoT-Plattform noch keinen ADX-Cluster bereitgestellt haben oder einen anderen Cluster verwenden möchten, führen Sie die [hier](https://docs.microsoft.com/azure/data-explorer/create-cluster-database-portal#create-a-cluster) beschriebenen Schritte aus. 
2. Aktivieren Sie die Streamingerfassung für den ADX-Cluster, wie [hier](https://docs.microsoft.com/azure/data-explorer/ingest-data-streaming#enable-streaming-ingestion-on-your-cluster) beschrieben. 
3. Erstellen Sie eine ADX-Datenbank. Die entsprechenden Schritte finden Sie [hier](https://docs.microsoft.com/azure/data-explorer/create-cluster-database-portal#create-a-database).

Im folgenden Schritt wird die [ADX-Weboberfläche](https://docs.microsoft.com/azure/data-explorer/web-query-data) verwendet, um die erforderlichen Abfragen auszuführen. Fügen Sie Ihren Cluster der Weboberfläche hinzu, wie unter dem Link beschrieben.  
 
4. Erstellen Sie in ADX eine Tabelle für die erfassten Daten.  Das Schema der ADX-Tabelle kann zwar mithilfe der Klasse „MonitoredItemMessageModel“ definiert werden, es empfiehlt sich jedoch, die Daten zunächst in einer Stagingtabelle mit einer Spalte vom Typ [Dynamisch](https://docs.microsoft.com/azure/data-explorer/kusto/query/scalar-data-types/dynamic) zu erfassen. Dadurch haben Sie mehr Flexibilität bei der Handhabung der Daten und bei der Verarbeitung für andere Tabellen, die für mehrere Anwendungsfälle geeignet sind (sowie ggf. die Möglichkeit, die Daten mit anderen Datenquellen zu kombinieren). Mit der folgenden ADX-Abfrage wird die Stagingtabelle „iiot_stage“ mit einer Spalte namens „payload“ erstellt:

```
.create table ['iiot_stage']  (['payload']:dynamic)
```

Darüber hinaus benötigen wir eine JSON-Erfassungszuordnung, um den Cluster anzuweisen, die gesamte JSON-Nachricht aus dem Event Hub in der Stagingtabelle zu platzieren:

```
.create table ['iiot_stage'] ingestion json mapping 'iiot_stage_mapping' '[{"column":"payload","path":"$","datatype":"dynamic"}]'
```

5. Die Tabelle ist nun bereit, Daten aus dem Event Hub zu empfangen. 
6. Gehen Sie wie [hier](https://docs.microsoft.com/azure/data-explorer/ingest-data-event-hub#connect-to-the-event-hub) beschrieben vor, um den Event Hub mit dem ADX-Cluster zu verbinden und damit zu beginnen, Daten in der Stagingtabelle zu erfassen. Wir müssen lediglich die Verbindung herstellen, da wir bereits über einen Event Hub verfügen, der von der IIoT-Plattform bereitgestellt wurde.  
7. Nach erfolgreicher Überprüfung der Verbindung werden der Tabelle nach und nach Daten hinzugefügt, die wir nach einer kurzen Wartezeit untersuchen können. Verwenden Sie die folgende Abfrage auf der ADX-Weboberfläche, um sich ein Datenbeispiel mit zehn Zeilen anzusehen. Wie Sie sehen, ähneln die Nutzdaten der bereits erwähnten Klasse „MonitoredItemMessageModel“.

![Abfrage](media/tutorial-iiot-data-adx/industrial-iot-in-azure-data-explorer-pic-2.png)

8. In diesem Abschnitt führen wir einige Analysen für die Daten durch. Hierbei werden die dynamischen Daten in der Spalte „payload“ direkt ausgewertet. In diesem Beispiel berechnen wir den Durchschnitt der durch den Anzeigenamen „PositiveTrendData“ identifizierten Telemetriedaten in einem Zeitfenster von zehn Minuten für alle Datensätze, die seit einem bestimmten (durch die Variable „min_t“ definierten) Zeitpunkt erfasst wurden: let min_t = datetime(2020-10-23); iiot_stage | where todatetime(payload.Timestamp) > min_t | where tostring(payload.DisplayName)== 'PositiveTrendData' | summarize event_avg = avg(todouble(payload.Value)) by bin(todatetime(payload.Timestamp), 10 m)
 
Da die Spalte „payload“ einen dynamischen Datentyp enthält, muss die Datenkonvertierung zur Abfragezeit erfolgen, damit unsere Berechnungen für die korrekten Datentypen durchgeführt werden.

![Zeitstempel der Nutzdaten](media/tutorial-iiot-data-adx/industrial-iot-in-azure-data-explorer-pic-3.png)

Die Erfassung der OPC UA-Daten in einer Stagingtabelle mit einer Spalte vom Typ „Dynamisch“ sorgt wie bereits erwähnt für Flexibilität. Die zur Abfragezeit erforderlichen Datentypkonvertierungen können allerdings zu Verzögerungen bei der Abfrageausführung führen. Das gilt insbesondere bei umfangreichen Daten sowie bei zahlreichen gleichzeitigen Abfragen. In dieser Phase können wir eine weitere Tabelle mit bereits bestimmten Datentypen erstellen, um Datentypenkonvertierungen zur Abfragezeit zu vermeiden.
 
9. Erstellen Sie eine neue Tabelle für die analysierten Daten, die eine begrenzte Auswahl aus dem Inhalt der dynamischen Nutzdaten in der Stagingtabelle umfasst. Wie Sie sehen, haben wir eine Wertspalte für jeden in unseren Telemetriedaten erwarteten Datentyp erstellt.

```
.create table ['iiot_parsed']  
    (['Tag_Timestamp']: datetime ,  
    ['Tag_PublisherId']:string ,  
    ['Tag']:string ,
    ['Tag_Datatype']:string ,  
    ['Tag_NodeId']:string,  
    ['Tag_value_long']:long ,  
    ['Tag_value_double']:double,  
    ['Tag_value_boolean']:bool)
```

10. Erstellen Sie eine Funktion (auf Datenbankebene), um die erforderlichen Daten aus der Stagingtabelle zu projizieren. Hier werden die Elemente „Timestamp“, „PublisherId“, „DisplayName“, „Datatype“ und „NodeId“ aus der Spalte „payload“ ausgewählt und als „Tag_Timestamp“, „Tag_PublisherId“, „Tag“, „Tag_Datatype“ und „Tag_NodeId“ projiziert. Das Element „Value“ wird basierend auf dem Datentyp (DataType) in Form von drei verschiedenen Komponenten projiziert.

```
.create-or-alter function fn_InflightParseIIoTEvent()
{
iiot_stage
| extend Tag_Timestamp =  todatetime(payload.Timestamp)
| extend Tag_PublisherId = tostring(payload.PublisherId)
| extend Tag = tostring(payload.DisplayName)
| extend Tag_Datatype = tostring(payload.DataType)
| extend Tag_NodeId = tostring(payload.NodeId)
| extend Tag_value_long = case(Tag_Datatype == "Int64", tolong(payload.Value), long(null))
| extend Tag_value_double = case(Tag_Datatype == "Double", todouble(payload.Value), double(null))
| extend Tag_value_boolean = case(Tag_Datatype == "Boolean", tobool(payload.Value), bool(null))
| project Tag_Timestamp, Tag_PublisherId, Tag, Tag_Datatype, Tag_NodeId, Tag_value_long, Tag_value_double, Tag_value_boolean
}
```

Weitere Informationen zur Zuordnung von Datentypen in ADX finden Sie [hier](https://docs.microsoft.com/azure/data-explorer/kusto/query/scalar-data-types/dynamic). Informationen zu Funktionen in ADX stehen [hier](https://docs.microsoft.com/azure/data-explorer/kusto/query/schema-entities/stored-functions) zur Verfügung.
 
11. Wenden Sie die Funktion aus dem vorherigen Schritt mithilfe einer Aktualisierungsrichtlinie auf die analysierte Tabelle an. Durch die [Aktualisierungsrichtlinie](https://docs.microsoft.com/azure/data-explorer/kusto/management/updatepolicy) wird ADX angewiesen, Daten automatisch an eine Zieltabelle anzufügen, wenn neue Daten in die Quelltabelle eingefügt werden (basierend auf einer Transformationsabfrage, die für die in die Quelltabelle eingefügten Daten ausgeführt wird). Wir können die folgende Abfrage verwenden, um die analysierte Tabelle als Ziel und die Stagingtabelle als Quelle für die Aktualisierungsrichtlinie zuzuweisen, die durch die im vorherigen Schritt erstellte Funktion definiert wird.

```
.alter table iiot_parsed policy update
@'[{"IsEnabled": true, "Source": "iiot_stage", "Query": "fn_InflightParseIIoTEvent()", "IsTransactional": true, "PropagateIngestionProperties": true}]'
```

Sobald die obige Abfrage ausgeführt wird, werden nach und nach Daten in die Zieltabelle „iiot_parsed“ eingefügt. Die Daten in „iiot_parsed“ können wie folgt betrachtet werden:

![Analysierte Tabelle](media/tutorial-iiot-data-adx/industrial-iot-in-azure-data-explorer-pic-4.png)

12. Als Nächstes sehen wir uns an, wie wir die Analyse aus einem der vorherigen Schritte wiederholen können. Berechnen Sie den Durchschnitt der durch den Anzeigenamen „PositiveTrendData“ identifizierten Telemetriedaten in einem Zeitfenster von zehn Minuten für alle Datensätze, die seit einem bestimmten (durch die Variable „min_t“ definierten) Zeitpunkt erfasst wurden. Da die Werte des Tags „PositveTrendData“ nun in einer Spalte mit Double-Datentyp gespeichert sind, sollte sich die Abfrageleistung verbessern.

![Wiederholte Analyse](media/tutorial-iiot-data-adx/industrial-iot-in-azure-data-explorer-pic-5.png)

13. Vergleichen wir abschließend die Abfrageleistung der beiden Fälle. Die Dauer der Abfrageausführung ist auf der ADX-Benutzeroberfläche in der Statistik (oberhalb der Abfrageergebnisse) angegeben.  

![Abfrageleistung 1](media/tutorial-iiot-data-adx/industrial-iot-in-azure-data-explorer-pic-6.png)

![Abfrageleistung 2](media/tutorial-iiot-data-adx/industrial-iot-in-azure-data-explorer-pic-7.png)

Wie Sie sehen, ist die Abfrage mit der analysierten Tabelle etwa doppelt so schnell wie die Abfrage für die Stagingtabelle. Da in diesem Beispiel nur ein kleines Dataset verwendet wurde und keine gleichzeitigen Abfragen ausgeführt wurden, sind die Auswirkungen der Ausführungsdauer gering. Bei einer realistischen Workload wären die Auswirkungen dagegen erheblich. Aus diesem Grund empfiehlt es sich, die verschiedenen Datentypen auf verschiedene Spalten aufzuteilen.

> [!NOTE] 
> Die Aktualisierungsrichtlinie kann nur für Daten verwendet werden, die nach der Einrichtung der Richtlinie in der Stagingtabelle erfasst werden. Auf bereits vorhandene Daten kann sie nicht angewendet werden. Dies muss beispielsweise im Falle einer Änderung der Aktualisierungsrichtlinie berücksichtigt werden. Umfassende Informationen finden Sie in der ADX-Dokumentation.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun wissen, wie Sie die Standardwerte der Konfiguration ändern, können Sie sich als Nächstes mit Folgendem vertraut machen: 

> [!div class="nextstepaction"]
> [Tutorial: Konfigurieren der Industrial IoT-Komponenten](tutorial-configure-industrial-iot-components.md)

> [!div class="nextstepaction"]
> [Tutorial: Visualisieren von Daten mit Time Series Insights (TSI)](tutorial-visualize-data-time-series-insights.md)