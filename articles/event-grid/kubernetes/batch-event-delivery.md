---
title: Azure Event Grid auf Kubernetes – Batch-Ereignisbereitstellung
description: Dieser Artikel beschreibt, wie Sie ein Batch-Ereignis an das Ziel liefern.
author: jfggdl
ms.subservice: kubernetes
ms.author: jafernan
ms.date: 05/25/2021
ms.topic: conceptual
ms.openlocfilehash: 46ae19af49b827af857f5f224ee5f0013d620a43
ms.sourcegitcommit: 5163ebd8257281e7e724c072f169d4165441c326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2021
ms.locfileid: "112414591"
---
# <a name="event-grid-on-kubernetes---batch-event-delivery"></a>Event Grid auf Kubernetes – Batch-Ereignslieferung 
Event Grid auf Kubernetes mit Azure Arc unterstützt die Bereitstellung von mehr als einem Ereignis in einer einzigen Bereitstellungsanforderung. Diese Funktion ermöglicht es, den gesamten Lieferdurchsatz zu erhöhen, ohne die HTTP-per-Anforderung-Betriebskosten. Die Batch-Ereignislieferung ist standardmäßig deaktiviert und kann über die Ereignisabonnement-Konfiguration aktiviert werden. 

[!INCLUDE [event-grid-preview-feature-note.md](../includes/event-grid-preview-feature-note.md)]

> [!WARNING]
> Die maximal zulässige Dauer für die Verarbeitung jeder Lieferanforderung ändert sich nicht, auch wenn der Ereignishandler-Code potenziell mehr Arbeit pro Batch-Anforderung leisten muss. Das Zeitlimit für Übermittlungen beträgt standardmäßig 60 Sekunden.

## <a name="batch-event-delivery-policy"></a>Batch-Ereignislieferungsrichtlinie
Das Batch-Ereignislieferverhalten in Event Grid in Kubernetes kann pro Ereignisabonnement angepasst werden, indem die folgenden beiden Einstellungen optimiert werden:

- **Maximale Anzahl von Ereignissen pro Batch**
    
    Durch diese Einstellung wird eine Obergrenze für die Anzahl der Ereignisse festgelegt, die einer Batch-Übermittlungsanforderung hinzugefügt werden können.
- **Die bevorzugte Batchgröße in Kilobytes**
    
    Dieses Konfigurationselement wird verwendet, um die maximale Anzahl von Kilobytes, die pro Zustellungsanforderung übertragen werden können, zunehmend zu regeln.

## <a name="batch-event-delivery-behavior"></a>Batch-Ereignislieferverhalten   

- **Alle oder keine**

    Event Grid auf Kubernetes funktioniert basierend auf einer Alles-oder-nichts-Semantik. Es wird kein Teilerfolg einer Batch-Ereignislieferung unterstützt. Abonnenten sollten darauf achten, nur so viele Ereignisse pro Batch anzufordern, wie Sie gemäß sinnvollem Ermessen in 60 Sekunden tatsächlich verarbeiten können.
- **Optimistische Batchverarbeitung**

    Die Einstellungen der Batching-Richtlinie sind keine strikten Grenzen für das Batching-Verhalten und werden nach bestem Wissen und Gewissen beachtet. Bei niedrigen Ereignisraten stellen Sie häufig fest, dass die Batchgröße unter der angeforderten maximalen Anzahl von Ereignissen pro Batch liegt.
- **Die Batch-Lieferung ist standardmäßig auf OFF eingestellt**

    Standardmäßig fügt Event Grid auf Kubernetes jeder Lieferanforderung nur ein Ereignis hinzu. Zum Aktivieren der Batch-Lieferung müssen Sie eine der zuvor in diesem Artikel beschriebenen Einstellungen im JSON-Code des Ereignisabonnements einstellen.
- **Standardwerte**

    Beim Erstellen eines Ereignisabonnements müssen nicht beide Einstellungen („Maximale Anzahl von Ereignissen pro Batch“ und „Bevorzugte Batchgröße in KB“) angegeben werden. Wenn nur eine Einstellung festgelegt ist, verwendet Event Grid auf Kubernetes (konfigurierbare) Standardwerte. 

## <a name="example"></a>Beispiel
Das folgende Beispiel zeigt, wie Sie in den Endpunkteigenschaften `maxEventsPerBatch` und `preferredBatchSizeInKilobytes` einstellen um die Batch-Verarbeitung zu aktivieren. 

```json
{
    "properties":
    {
        "destination":
        {
            "endpointType": "WebHook",
            "properties":
             {
                "endpointUrl": "<your_webhook_url>",
                "maxEventsPerBatch": 10,
                "preferredBatchSizeInKilobytes": 64
             }
        },
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Informationen zu Zielen und Handlern, die von Event Grid in Azure Arc für Kubernetes unterstützt werden, finden Sie unter [Event Grid in Kubernetes – Ereignishandler](event-handlers.md).