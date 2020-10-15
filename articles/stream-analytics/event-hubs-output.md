---
title: Event Hubs-Ausgabe für Azure Stream Analytics
description: In diesem Artikel wird beschrieben, wie Daten aus Azure Stream Analytics in Azure Event Hubs ausgegeben werden.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/23/2020
ms.openlocfilehash: 86a6c1a15d804a6c758e90dbd4bdd7057a7a2716
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91295271"
---
# <a name="event-hubs-output-from-azure-stream-analytics"></a>Event Hubs-Ausgabe für Azure Stream Analytics

Der Dienst [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) ist ein hoch skalierbarer Veröffentlichen-Abonnieren-Ereignisingestor. Er kann mehrere Millionen Ereignisse pro Sekunde erfassen. Eine Verwendungsmöglichkeit eines Event Hubs als Ausgabe ergibt sich, wenn die Ausgabe eines Stream Analytics-Auftrags zur Eingabe eines anderen Streamingauftrags wird. Weitere Informationen zum Optimieren der maximalen Nachrichtengröße und Batchgröße finden Sie im Abschnitt zur [Ausgabebatchgröße](#output-batch-size).

## <a name="output-configuration"></a>Ausgabekonfiguration

Die folgende Tabelle enthält die Parameter, die zur Konfiguration von Datenströmen von Event Hubs als Ausgabe erforderlich sind.

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Ausgabealias | Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diesen Event Hub weiterzuleiten. |
| Event Hub-Namespace | Ein Container für einen Satz von Nachrichtenentitäten. Sie haben bei der Erstellung eines neuen Event Hubs auch einen Event Hub-Namespace erstellt. |
| Event Hub-Name | Der Name Ihrer Event Hub-Ausgabe. |
| Event Hub-Richtlinienname | Die Richtlinie für den gemeinsamen Zugriff, die Sie auf der Registerkarte **Konfigurieren** des Event Hubs erstellen können. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. |
| Event Hub-Richtlinienschlüssel | Der Schlüssel für den gemeinsamen Zugriff, der für die Authentifizierung des Zugriffs auf den Event Hub-Namespace verwendet wird. |
| Partitionsschlüsselspalte | Optional. Eine Spalte, die den Partitionsschlüssel für die Event Hub-Ausgabe enthält. |
| Ereignisserialisierungsformat | Das Serialisierungsformat für Ausgabedaten. Es werden JSON, CSV und Avro unterstützt. |
| Codieren | Bei CSV und JSON ist UTF-8 gegenwärtig das einzige unterstützte Codierungsformat. |
| Trennzeichen | Gilt nur für die CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen zum Serialisieren der Daten im CSV-Format. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Tabstopp und senkrechter Strich. |
| Format | Gilt nur für die JSON-Serialisierung. **Separate Zeile** gibt an, dass die Ausgabe so formatiert wird, dass jedes JSON-Objekt in einer neuen Zeile enthalten ist. Wenn Sie **Separate Zeile** auswählen, wird der JSON-Code objektweise gelesen. Der gesamte Inhalt an sich wäre kein gültiger JSON-Code. **Array** gibt an, dass die Ausgabe als Array aus JSON-Objekten formatiert wird.  |
| Eigenschaftenspalten | Optional. Durch Komma getrennte Spalten, die anstelle der Nutzlast als Benutzereigenschaften der ausgehenden Nachricht angefügt werden müssen. Weitere Informationen zu diesem Feature finden Sie im Abschnitt [Benutzerdefinierte Metadateneigenschaften für die Ausgabe](#custom-metadata-properties-for-output). |

## <a name="partitioning"></a>Partitionierung

Die Partitionierung variiert je nach Partitionsausrichtung. Wenn der Partitionsschlüssel für die Event Hub-Ausgabe gleichmäßig mit dem (vorherigen) Upstream-Abfrageschritt ausgerichtet ist, entspricht die Anzahl der Writer der Anzahl der Partitionen in der Event Hub-Ausgabe. Jeder Writer verwendet die [EventHubSender-Klasse](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet&preserve-view=true), um Ereignisse an die jeweilige Partition zu senden. Wenn der Partitionsschlüssel für die Event Hub-Ausgabe nicht mit dem (vorherigen) Upstream-Abfrageschritt ausgerichtet ist, entspricht die Anzahl der Writer der Anzahl der Partitionen in diesem vorherigen Schritt. Jeder Writer verwendet die [SendBatchAsync-Klasse](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet&preserve-view=true) in **EventHubClient**, um Ereignisse an alle Ausgabepartitionen zu senden. 

## <a name="output-batch-size"></a>Ausgabebatchgröße

Die maximale Nachrichtengröße beträgt 256 KB oder 1 MB pro Nachricht. Weitere Informationen finden Sie unter [Event Hubs-Grenzwerte](../event-hubs/event-hubs-quotas.md). Wenn die E/A-Partitionierung nicht ausgerichtet ist, wird jedes Ereignis einzeln in `EventData` verpackt und als Batch gesendet, dessen Größe bis zur maximalen Nachrichtengröße reichen kann. Dies geschieht auch, wenn [benutzerdefinierte Metadateneigenschaften](#custom-metadata-properties-for-output) verwendet werden. Wenn die E/A-Partitionierung ausgerichtet ist, werden mehrere Ereignisse bis zur maximalen Nachrichtengröße in eine einzelne `EventData`-Instanz verpackt und gesendet.

## <a name="custom-metadata-properties-for-output"></a>Benutzerdefinierte Metadateneigenschaften für die Ausgabe

Sie können Abfragespalten als Benutzereigenschaften an Ihre ausgehenden Nachrichten anfügen. Diese Spalten gelangen nicht in die Nutzlast. Die Eigenschaften liegen in Form eines Wörterbuchs für die Ausgabemeldung vor. *Schlüssel* ist der Spaltenname und der *Wert* ist der Spaltenwert im Eigenschaftenwörterbuch. Alle Stream Analytics-Datentypen werden mit Ausnahme von „Datensatz“ und „Array“ unterstützt.

Im folgenden Beispiel werden die Felder `DeviceId` und `DeviceStatus` zu den Metadaten hinzugefügt.

1. Verwenden Sie die folgende Abfrage:

   ```sql
   select *, DeviceId, DeviceStatus from iotHubInput
   ```

1. Konfigurieren Sie `DeviceId,DeviceStatus` als Eigenschaftenspalten in der Ausgabe.

   :::image type="content" source="media/event-hubs-output/property-columns.png" alt-text="Eigenschaftenspalten":::

Die folgende Abbildung zeigt die erwarteten Eigenschaften der Ausgabemeldung, die in EventHub mit dem [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer) überprüft wurden.

:::image type="content" source="media/event-hubs-output/custom-properties.png" alt-text="Eigenschaftenspalten":::

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals](stream-analytics-quick-create-portal.md)
* [Schnellstart: Erstellen eines Azure Stream Analytics-Auftrags mit der Azure CLI](quick-create-azure-cli.md)
* [Schnellstart: Erstellen eines Azure Stream Analytics-Auftrags mithilfe einer ARM-Vorlage](quick-create-azure-resource-manager.md)
* [Schnellstart: Erstellen eines Stream Analytics-Auftrags mit Azure PowerShell](stream-analytics-quick-create-powershell.md)
* [Schnellstart: Erstellen eines Azure Stream Analytics-Auftrags mithilfe von Visual Studio](stream-analytics-quick-create-vs.md)
* [Schnellstart: Erstellen eines Azure Stream Analytics-Auftrags in Visual Studio Code](quick-create-visual-studio-code.md)
