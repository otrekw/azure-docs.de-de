---
title: Azure Event Grid in Kubernetes – Filtern von Ereignissen
description: In diesem Artikel erfahren Sie, wie Sie Ereignisse beim Erstellen eines Azure Event Grid-Abonnements filtern können.
author: jfggdl
ms.subservice: kubernetes
ms.author: jafernan
ms.date: 05/25/2021
ms.topic: conceptual
ms.openlocfilehash: e4dbdbb2f46546391b6e749a94bfa655bb9de45e
ms.sourcegitcommit: 5163ebd8257281e7e724c072f169d4165441c326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2021
ms.locfileid: "112417341"
---
# <a name="event-grid-on-kubernetes---event-filtering-for-subscriptions"></a>Event Grid in Kubernetes – Ereignisfilterung für Abonnements
In Event Grid in Kubernetes können Filter für eine beliebige Eigenschaft in den JSON-Nutzdaten angegeben werden. Diese Filter werden als Gruppe von AND-Bedingungen erstellt, wobei jede äußere Bedingung über optionale innere OR-Bedingungen verfügt. Für jede AND-Bedingung geben Sie die folgenden Werte an:

- OperatorType: Der Typ des Vergleichs.
- Key: Der JSON-Pfad zur Eigenschaft, auf die der Filter angewendet werden soll.
- Value: Referenzwert, für den der Filter ausgeführt wird (oder) Values: Gruppe von Referenzwerten, für die der Filter ausgeführt wird.

[!INCLUDE [event-grid-preview-feature-note.md](../includes/event-grid-preview-feature-note.md)]



## <a name="filter-by-event-type"></a>Filtern nach Ereignistyp
Standardmäßig werden alle [Ereignistypen](event-schemas.md) (`type`-Attribute) für die Ereignisquelle an den Endpunkt gesendet. Sie können entscheiden, nur bestimmte Ereignistypen an Ihren Endpunkt zu senden. Die JSON-Syntax für das Filtern nach Ereignistyp lautet:

```json
"filter": {
  "includedEventTypes": [
    "orderCreated",
    "orderUpdated"
  ]
}
```

Im Beispiel oben werden nur Ereignisse vom Typ `orderCreated` und `orderUpdated`-Ereignisse an den Abonnentenendpunkt gesendet. 

Hier sehen Sie ein Beispiel für ein Ereignis:

```json
[{
      "specVersion": "1.0",
      "type" : "orderCreated",
      "source": "myCompanyName/us/webCommerceChannel/myOnlineCommerceSiteBrandName",
      "id" : "eventId-n",
      "time" : "2020-12-25T20:54:07+00:00",
      "subject" : "account/acct-123224/order/o-123456",
      "dataSchema" : "1.0",
      "data" : {
         "orderId" : "123",
         "orderType" : "PO",
         "reference" : "https://www.myCompanyName.com/orders/123"
      }
}]
```

## <a name="filter-by-subject"></a>Filtern nach Betreff
Zum einfachen Filtern nach Betreff geben Sie einen Start- oder Endwert für den Betreff ein. Die JSON-Syntax für das Filtern nach Betreff lautet:

```json
"filter": {
  "subjectBeginsWith": "/account/acct-123224/"
}
``` 

Beispielsweise sendet der oben konfigurierte Filter alle Bestellungen, die dem Konto `acct-123224` zugeordnet sind, an den Abonnentenendpunkt. 

Erstellen Sie beim Veröffentlichen von Ereignissen für Themen Betreffinformationen für Ihre Ereignisse, an denen Abonnenten einfach erkennen können, ob sie Interesse am Ereignis haben. Abonnenten verwenden die Betreffeigenschaft zum Filtern und Weiterleiten von Ereignissen. Erwägen Sie, den Pfad zum Ereignisort hinzuzufügen, damit Abonnenten nach Segmenten dieses Pfads filtern können. Mit dem Pfad können Abonnenten Ereignisse speziell oder allgemein filtern. Wenn Sie einen Pfad mit drei Segmenten im Betreff angeben, z. B. /A/B/C, können Abonnenten nach dem ersten Segment /A filtern, um eine umfassendere Gruppe von Ereignissen angezeigt zu bekommen. Diese Abonnenten erhalten Ereignisse mit Betreffinformationen wie /A/B/C oder /A/D/E. Andere Abonnenten können nach /A/B filtern, um eine eingeschränktere Gruppe mit Ereignissen zu erhalten.

## <a name="filter-by-values-in-event-data"></a>Filtern nach Werten in Ereignisdaten
Im Abschnitt [Erweiterte Filterung im Artikel „Event Grid in Azure“](../event-filtering.md) finden Sie detaillierte Informationen zur erweiterten Filterung. Die folgenden Features und Operatoren werden von Event Grid in Kubernetes nicht unterstützt. 

- Filterung nach Arraydaten in Schlüsseln eingehender Ereignisse
- Zulassen der Filterung nach [Kontextattributen für CloudEvents-Erweiterungen](https://github.com/cloudevents/spec/blob/v1.0/documented-extensions.md).
- Folgende Operatoren
    - StringNotContains
    - StringNotBeginsWith
    - StringNotEndsWith
    - NumberInRange
    - NumberNotInRange
    - IsNullOrUndefined
    - IsNotNull
    

## <a name="next-steps"></a>Nächste Schritte
Informationen zu Zielen und Handlern, die von Event Grid in Azure Arc für Kubernetes unterstützt werden, finden Sie unter [Event Grid in Kubernetes – Ereignishandler](event-handlers.md).