---
title: Azure Event Grid – Übermittlung und Wiederholung
description: Beschreibt, wie Azure Event Grid Ereignisse übermittelt und wie nicht übermittelte Nachrichten verarbeitet werden.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 02/27/2020
ms.author: spelluru
ms.openlocfilehash: dda2fd98c4c0d330059156a5ec00baa97ffaf627
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921061"
---
# <a name="event-grid-message-delivery-and-retry"></a>Event Grid – Übermittlung und Wiederholung von Nachrichten

In diesem Artikel wird beschrieben, wie Azure Event Grid Ereignisse verarbeitet, wenn die Übermittlung nicht bestätigt wird.

Event Grid bietet permanente Übermittlung. Jede Nachricht wird für jedes Abonnement mindestens einmal übermittelt. Ereignisse werden sofort an den registrierten Endpunkt des jeweiligen Abonnements gesendet. Wenn ein Endpunkt den Eingang eines Ereignisses nicht bestätigt, wiederholt Event Grid die Übermittlung des Ereignisses.

## <a name="batched-event-delivery"></a>Batchübermittlung von Ereignissen

Event Grid sendet jedes Ereignis standardmäßig einzeln an Abonnenten. Der Abonnent empfängt ein Array mit einem einzelnen Ereignis. Sie können Event Grid zum Zusammenstellen von Ereignissen für die Übermittlung konfigurieren, um die HTTP-Leistung in Szenarios mit hohen Durchsätzen zu verbessern.

Es gibt zwei Einstellungen für die Batchübermittlung:

* **Maximale Anzahl von Ereignissen pro Batch** bezeichnet die maximale Anzahl der Ereignisse, die Event Grid pro Batch übermittelt. Diese Zahl wird nie überschritten, jedoch können weniger Ereignisse übermittelt werden, wenn keine anderen Ereignisse zum Zeitpunkt der Veröffentlichung verfügbar sind. Event Grid verzögert Ereignisse nicht, um ein Batch zu erstellen, wenn weniger Ereignisse verfügbar sind. Der Wert muss zwischen 1 und 5.000 liegen.
* **Bevorzugte Batchgröße in KB** bezeichnet die Obergrenze für die Batchgröße in Kilobyte. Ähnlich wie bei der maximalen Anzahl von Ereignissen, kann die Batchgröße kleiner sein, wenn nicht mehr Ereignisse zum Zeitpunkt der Veröffentlichung vorhanden sind. Es ist möglich, dass ein Batch die bevorzugte Batchgröße überschreitet, *wenn* ein einzelnes Ereignis größer als die bevorzugte Batchgröße ist. Wenn die bevorzugte Größe beispielsweise 4 KB ist und ein Ereignis mit 10 KB an Event Grid gepusht wird, wird das Ereignis mit 10 KB dennoch in einem eigenen Batch übermittelt, anstatt gelöscht zu werden.

Die Batchübermittlung wird pro Ereignisabonnement über das Portal, eine Befehlszeilenschnittstelle, PowerShell oder über SDKs konfiguriert.

### <a name="azure-portal"></a>Azure-Portal: 
![Einstellungen der Batchübermittlung](./media/delivery-and-retry/batch-settings.png)

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Wenn Sie ein Ereignisabonnement erstellen, verwenden Sie die folgenden Parameter: 

- **max-events-per-batch**: maximale Anzahl von Ereignissen in einem Batch. Muss eine Zahl im Bereich 1 bis 5.000 sein.
- **preferred-batch-size-in-kilobytes**: bevorzugte Batchgröße in Kilobyte. Muss eine Zahl im Bereich 1 bis 1.024 sein.

```azurecli
storageid=$(az storage account show --name <storage_account_name> --resource-group <resource_group_name> --query id --output tsv)
endpoint=https://$sitename.azurewebsites.net/api/updates

az eventgrid event-subscription create \
  --resource-id $storageid \
  --name <event_subscription_name> \
  --endpoint $endpoint \
  --max-events-per-batch 1000 \
  --preferred-batch-size-in-kilobytes 512
```

Weitere Informationen zur Verwendung von Azure CLI mit Event Grid finden Sie unter [Weiterleiten von Speicherereignissen an den Webendpunkt mit Azure CLI](../storage/blobs/storage-blob-event-quickstart.md).

## <a name="retry-schedule-and-duration"></a>Wiederholungszeitplan und Dauer

Event Grid wartet nach der Zustellung einer Nachricht 30 Sekunden auf eine Antwort. Nach 30 Sekunden, wenn der Endpunkt nicht geantwortet hat, wird die Nachricht zur Wiederholung in die Warteschlange eingereiht. Event Grid verwendet exponentiell ansteigende Wartezeiten für Wiederholungsversuche für die Ereignisübermittlung. Event Grid wiederholt die Zustellung nach folgendem Zeitplan auf Basis der besten Leistung:

- 10 Sekunden
- 30 Sekunden
- 1 Minute
- 5 Minuten
- 10 Minuten
- 30 Minuten
- 1 Stunde
- Pro Stunde für bis zu 24 Stunden

Wenn der Endpunkt innerhalb von 3 Minuten antwortet, versucht Event Grid, das Ereignis aus der Wiederholungswarteschlange auf bestmögliche Weise zu entfernen, aber es können dennoch weiterhin Duplikate empfangen werden.

Event Grid fügt allen Wiederholungsschritten eine geringfügige Randomisierung hinzu und kann opportunistisch bestimmte Wiederholungen überspringen, wenn ein Endpunkt konsistent fehlerhaft ist, für einen längeren Zeitraum ausgefallen ist oder überlastet zu sein scheint.

Um deterministisches Verhalten zu erreichen, legen Sie die Gültigkeitsdauer des Ereignisses und die maximale Zahl der Zustellversuche in den [Richtlinien für unzustellbare Nachrichten und Wiederholungen](manage-event-delivery.md) fest.

Event Grid markiert alle Ereignisse standardmäßig als abgelaufen, die nicht innerhalb von 24 Stunden übermittelt werden. Bei der Erstellung von Ereignisabonnements können Sie die [Wiederholungsrichtlinie anpassen](manage-event-delivery.md). Sie geben die maximale Anzahl von Zustellversuchen (Standardwert 30) und die Gültigkeitsdauer des Ereignisses (Standardwert 1.440 Minuten) ein.

## <a name="delayed-delivery"></a>Verzögerte Übermittlung

Wenn bei einem Endpunkt Übermittlungsfehler auftreten, beginnt Event Grid, die Übermittlung von Ereignissen an diesen Endpunkt und erneute Versuche zu verzögern. Wenn beispielsweise bei den ersten zehn an einem Endpunkt veröffentlichten Ereignissen Fehler auftreten, geht Event Grid davon aus, dass am Endpunkt Probleme auftreten, und verzögert alle nachfolgenden Wiederholungsversuche *und neuen* Übermittlungen für einige Zeit – in einigen Fällen bis zu mehreren Stunden.

Der funktionale Zweck der verzögerten Übermittlung besteht darin, sowohl fehlerhafte Endpunkte als auch das Event Grid-System zu schützen. Ohne Backoff und Verzögerung der Übermittlung an fehlerhafte Endpunkte können die Wiederholungsrichtlinie und Volumefunktionen von Event Grid ein System leicht überfordern.

## <a name="dead-letter-events"></a>„Unzustellbare Nachrichten“-Ereignisse

Wenn ein Ereignis nicht übermittelt werden kann, kann Event Grid das nicht übermittelte Ereignis an ein Speicherkonto senden. Dieser Prozess wird als Speicherung unzustellbarer Nachrichten bezeichnet. Die Speicherung unzustellbarer Nachrichten ist standardmäßig nicht aktiviert. Wenn Sie das Feature aktivieren möchten, müssen Sie bei der Erstellung des Ereignisabonnements ein Speicherkonto zum Speichern nicht übermittelter Ereignisse angeben. Ereignisse werden aus diesem Speicherkonto gepullt, um Übermittlungsprobleme zu beheben.

Event Grid sendet ein Ereignis an den Speicherort für unzustellbare Nachrichten, wenn alle Wiederholungsversuche ausgeführt wurden. Wenn Event Grid den Antwortcode 400 (Ungültige Anforderung) oder 413 (Anforderungsentität zu groß) erhält, sendet der Dienst das Ereignis sofort an den Endpunkt für unzustellbare Nachrichten. Diese Antwortcodes geben an, dass die Übermittlung des Ereignisses nie erfolgreich ausgeführt wird.

Es gibt eine fünfminütige Verzögerung zwischen dem letzten Versuch, ein Ereignis zu übermitteln, und der Übermittlung an den Speicherort für unzustellbare Nachrichten. Diese Verzögerung dient dazu, die Anzahl der Blob Storage-Vorgänge zu reduzieren. Wenn der Speicherort für unzustellbare Nachrichten vier Stunden lang nicht verfügbar ist, wird das Ereignis gelöscht.

Wenn Sie den Speicherort für unzustellbare Nachrichten festlegen möchten, benötigen Sie ein Speicherkonto mit einem Container. Der Endpunkt für diesen Container wird bei der Erstellung des Ereignisabonnements angegeben. (Format des Endpunkts: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`)

Es wird empfohlen, sich benachrichtigen zu lassen, wenn ein Ereignis an den Speicherort für unzustellbare Nachrichten gesendet wurde. Wenn Event Grid auf nicht übermittelte Ereignisse reagieren soll, [erstellen Sie ein Ereignisabonnement](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) für den Blobspeicher für unzustellbare Nachrichten. Sobald bei Ihrem Blobspeicher für unzustellbare Nachrichten ein nicht übermitteltes Ereignis eingeht, informiert Event Grid Ihren Handler. Der Handler reagiert mit den gewünschten Aktionen zur Abstimmung nicht übermittelter Ereignisse.

Ein Beispiel für das Einrichten eines Speicherorts für unzustellbare Nachrichten finden Sie unter [Unzustellbare Nachrichten und Wiederholungsrichtlinien](manage-event-delivery.md).

## <a name="message-delivery-status"></a>Nachrichtenübermittlungsstatus

Event Grid verwendet HTTP-Antwortcodes zum Bestätigen des Eingangs von Ereignissen. 

### <a name="success-codes"></a>Erfolgscodes

Event Grid berücksichtigt **nur** die folgenden HTTP-Antwortcodes als erfolgreiche Übermittlungen. Alle anderen Statuscodes gelten als Übermittlungen, bei denen Fehler aufgetreten sind, und die Übermittlungen werden entsprechend neu versucht oder als unzustellbar behandelt. Nach dem Empfang eines erfolgreichen Statuscodes betrachtet Event Grid die Übermittlung als abgeschlossen.

- 200 – OK
- 201 – Erstellt
- 202 – Akzeptiert
- 203 Keine Autorisierungsinformationen
- 204 Kein Inhalt

### <a name="failure-codes"></a>Fehlercodes

Alle anderen Codes, die nicht zur obigen Gruppe (200-204) gehören, werden als Fehler angesehen, und es wird ein neuer Versuch unternommen. Für einige gelten spezifische Wiederholungsrichtlinien, die im Folgenden beschrieben werden, alle anderen folgen dem standardmäßigen exponentiellen Backoffmodell. Es ist wichtig zu beachten, dass aufgrund der stark parallelisierten Architektur von Event Grid das Wiederholungsverhalten nicht deterministisch ist. 

| Statuscode | Wiederholungsverhalten |
| ------------|----------------|
| 400 – Ungültige Anforderung | Wiederholen Sie den Vorgang nach mindestens 5 Minuten (das Verschieben in die Warteschlange für unzustellbare Nachrichten erfolgt sofort, wenn dies eingerichtet ist). |
| 401 – Nicht autorisiert | Wiederholen Sie den Vorgang nach mindestens 5 Minuten. |
| 403 Verboten | Wiederholen Sie den Vorgang nach mindestens 5 Minuten. |
| 404 – Nicht gefunden | Wiederholen Sie den Vorgang nach mindestens 5 Minuten. |
| 408 Anforderungstimeout | Wiederholen Sie den Vorgang nach mindestens 2 Minuten. |
| 413 – Anforderungsentität zu groß | Wiederholen Sie den Vorgang nach mindestens 10 Sekunden (das Verschieben in die Warteschlange für unzustellbare Nachrichten erfolgt sofort, wenn dies eingerichtet ist). |
| 503 Dienst nicht verfügbar | Wiederholen Sie den Vorgang nach mindestens 30 Sekunden. |
| Alle anderen | Wiederholen Sie den Vorgang nach mindestens 10 Sekunden. |


## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Anzeigen des Status von Ereignisübermittlungen finden Sie unter [Überwachen der Event Grid-Nachrichtenübermittlung](monitor-event-delivery.md).
* Um Optionen für die Ereignisübermittlung anzupassen, lesen Sie [Unzustellbare Nachrichten und Wiederholungsrichtlinien](manage-event-delivery.md).
