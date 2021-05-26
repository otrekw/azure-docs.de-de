---
title: Azure Event Grid auf Kubernetes – Konzepte
description: Dieser Artikel erklärt die Kernkonzepte von Azure Event Grid auf Kubernetes mit Azure Arc (Vorschau)
author: jfggdl
ms.subservice: kubernetes
ms.author: jafernan
ms.date: 05/25/2021
ms.topic: conceptual
ms.openlocfilehash: e9d7fff98a6d6c062f529528f9f1d3071a038049
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385777"
---
# <a name="event-grid-on-kubernetes---concepts"></a>Azure Event Grid on Kubernetes – Konzepte
Dieser Artikel beschreibt die wichtigsten Konzepte in Event Grid auf Kubernetes mit Azure Arc (Vorschau).

[!INCLUDE [event-grid-preview-feature-note.md](../../../includes/event-grid-preview-feature-note.md)]

## <a name="events"></a>Ereignisse
Bei einem Ereignis handelt es sich um einen Datensatz, der eine Tatsache bezüglich des Betriebs eines Softwaresystems ankündigt. In der Regel kündigt ein Ereignis eine Zustandsänderung aufgrund eines vom System ausgelösten Signals oder eines vom System beobachteten Signals an. Ereignisse enthalten zwei Arten von Informationen: 

- [Ereignisdaten](https://github.com/cloudevents/spec/blob/master/spec.md#event-data), die das Auftreten einer Zustandsänderung darstellen. 
- [Kontextattribute](https://github.com/cloudevents/spec/blob/master/spec.md#context-attributes), die kontextbezogene Informationen zum Auftreten des Ereignisses bereitstellen.     

    Sowohl Ereignisdaten als auch Kontextattribute können zum Filtern von Ereignissen verwendet werden. 

Event Grid in Kubernetes unterstützt die [CloudEvents](https://github.com/cloudevents/spec/tree/master)-Schemaspezifikation. Hier sehen Sie ein Beispiel für ein Ereignis, welches das CloudEvents-Schema verwendet. Event Grid unterstützt ein Ereignis mit einer Größe von bis zu 1 MB.

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

## <a name="source"></a>`Source`
Das Quellattribut beschreibt den Kontext, in dem das Ereignis aufgetreten ist. Die Quelle ist möglicherweise der Verursacher von Ereignissen. In einigen Fällen gibt es jedoch Produzenten, die Ereignisse erstellen und veröffentlichen. Und diese Produzenten unterscheiden sich von der Quelle. Der Einfachheit halber wird in diesem Artikel angenommen, dass die Quelle der Produzent der Ereignisses ist. 

Jede Event-Quelle erzeugt Ereignisse eines oder mehrerer Ereignistypen. Als Quelle von Ereignissen definiert eine Anwendung einen Satz von zusammenhängenden Ereignissen, um ihre Zustandsänderungen anzukündigen. Jedes Ereignis enthält allgemeine Informationen wie Quelle des Ereignisses, Zeitpunkt, an dem das Ereignis aufgetreten ist, und den eindeutigen Bezeichner. Jedes Ereignis hat auch spezifische Informationen, die nur für die jeweilige Art von Ereignis relevant sind. Die Unterstützung für ein Ereignis von einer Größe bis zu 1 MB ist derzeit in der Vorschauversion verfügbar.

Für die Eigenschaften, die in einem Ereignis enthalten sind, siehe [CloudEvents-Schema](event-schemas.md#cloudevent-schema).

## <a name="publishers"></a>Herausgeber
Bei Ereignisherausgebern handelt es sich um Anwendungen oder Systeme, die Ereignisse an Event Grid senden, um sie an Ereignisabonnenten zu liefern.

## <a name="topics"></a>Themen
Ein Thema ist eine Art Eingabekanal, der einen Endpunkt verfügbar macht, an den Herausgeber Ereignisse an Event Grid senden.

Ein Thema kann für eine Sammlung von zusammenhängenden Ereignissen verwendet werden. Sie könnten für jede Kategorie von verwandten Ereignissen ein Thema erstellen. In einigen Fällen kann die Quelle verwendet werden, um Ereignisse in Kategorien zu organisieren, da Quellen in der Regel mit einer Reihe von eng verwandten Ereignistypen („MyApp.OrderCreated“, „MyApp.OderDeleted“, „MyApp.OrderRejected“, usw.) verbunden sind. 

Betrachten Sie eine Anwendung, die Ereignisse im Zusammenhang mit der Verwaltung von Benutzerkonten und der Verarbeitung von Bestellungen sendet. Es ist unwahrscheinlich, dass ein Ereignisabonnent daran interessiert ist, beide Ereigniskategorien zu konsumieren. Erstellen Sie zwei benutzerdefinierte Themen, und lassen Sie Ereignishandler das jeweils relevante Thema abonnieren. Für kleine Lösungen empfiehlt es sich ggf., alle Ereignisse an ein Thema zu senden. 

## <a name="event-subscribers"></a>Ereignisabonnenten
Bei Ereignisabonnenten handelt es sich um Softwaresysteme wie z. B. Microservices, die Endpunkte exponieren, an die Event Grid Events Ereignisse liefert. 

## <a name="event-subscriptions"></a>Ereignisabonnements
Ein Ereignisabonnement teilt Event Grid mit, welche Ereignisse Sie zu einem Thema empfangen möchten (Ereignisfilterung) und wohin diese gesendet werden sollen (Ereignis-Routing). Beim Erstellen eines Ereignisabonnements geben Sie einen Endpunkt für die Behandlung des Ereignisses an. Sie können die Ereignisse auswählen, die an Ihren Endpunkt geliefert werden sollen, indem Sie Filterklauseln für das Ereignisabonnement konfigurieren. 

## <a name="event-handlers"></a>Ereignishandler
Beim Ereignishandler handelt es sich um ein Softwaresystem, das einen Endpunkt bereitstellt, an den Ereignisse gesendet werden. Der Handler empfängt das Ereignis und führt Aktionen zur Verarbeitung des Ereignisses aus. Event Grid unterstützt verschiedene Handlertypen. Als Handler können Sie einen unterstützten Azure-Dienst verwenden, der auf Kubernetes oder Azure gehostet wird, oder Ihre eigene Lösung, die einen Webhook (Endpunkt) bereitstellt, wo immer dieser gehostet wird. Je nach Handlertyp übermittelt Event Grid die Ereignisse auf unterschiedliche Art und Weise. Wenn es sich beim Ziel-Ereignishandler um ein HTTP-Webhook handelt, wird das Ereignis wiederholt, bis der Handler den Statuscode 200 - OK zurückgibt. Weitere Informationen finden Sie unter [Eeignishandler](event-handlers.md).

## <a name="sas-authentication"></a>SAS-Authentifizierung
Event Grid in Kubernetes bietet die SAS-Schlüsselbasierte Authentifizierung zum Veröffentlichen von Ereignissen in Themen.

## <a name="event-delivery"></a>Ereignisbereitstellung
Event Grid in Kubernetes bietet einen zuverlässigen Liefer- und Erneut-Versuchen-Mechanismus. Wenn Event Grid nicht bestätigen kann, dass ein Ereignis vom Endpunkt des Ereignishandlers empfangen wurde, wird das Ereignis erneut geliefert. Weitere Informationen finden Sie unter [Event Grid – Nachrichtenübermittlung und -wiederholung](delivery-retry.md).

## <a name="batch-event-publishing"></a>Batch-Ereignisveröffentlichung
Wenn Sie ein benutzerdefiniertes Thema verwenden, müssen die Ereignisse immer in einer Serie veröffentlicht werden. Bei Szenarios mit geringem Durchsatz hat das Array nur einen Ereignis. Für Anwendungsfälle mit hohem Volumen wird aber empfohlen, pro Veröffentlichung mehrere Ereignisse zu Batches zusammenzufassen, um eine höhere Effizienz zu erzielen. Batches können eine Größe von bis zu 1 MB haben. Für die einzelnen Ereignisse sollte trotzdem eine Größe von 1 KB nicht überschritten werden. Weitere Informationen finden Sie unter [Batch-Ereignislieferung](batch-event-delivery.md).

## <a name="event-grid-on-kubernetes-components"></a>Azure Event Grid on Kubernetes Komponenten

- Der **Event Grid-Operator** implementiert das [Operator-Muster](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/). Er überwacht die Zustandsänderungen von Event-Grid-Ressourcen als Ergebnis von Steuerungsebenen-Anforderungen, die an den API-Server von Kubernetes gestellt werden. Im Falle einer Anforderung, die den Zustand einer der Event Grid-Ressourcen beeinflusst, synchronisiert der Event Grid-Operator diesen Zustand mit dem Event Grid Broker.
- Der **Event Grid Broker** dient sowohl als Steuerungsebenen- als auch als Datenebenen-Vorgänge.

   Als Steuerungsebenen-Dienst ist er dafür verantwortlich, den Zustand von Event Grid in den vom Event Grid Operator mitgeteilten gewünschten Zustand zu bringen. Wenn z. B. eine Anforderung zum Erstellen eines neuen Themas gestellt wird, wird diese Anforderung erfüllt und die Metadaten des Dienstes werden aktualisiert.

   Als Datenebenen-Dienst verarbeitet er alle Event-Veröffentlichungsanforderungen und übermittelt Ereignisse an die Ziele, die in Ereignisabonnements konfiguriert sind.

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den ersten Schritten finden Sie unter [Erstellen von Themen und Abonnements](create-topic-subscription.md).