---
title: Azure Event Grid in Kubernetes – Ereignisschemata
description: Dieser Artikel beschreibt Ereignisschemata, die von Event Grid auf Azure Arc für Kubernetes unterstützt werden
author: jfggdl
ms.subservice: kubernetes
ms.author: jafernan
ms.date: 05/25/2021
ms.topic: conceptual
ms.openlocfilehash: 78576362ff3258344b6a340762799dfa1a52f623
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386282"
---
# <a name="event-schemas-in-event-grid-on-kubernetes"></a>Ereignisschemata in Event Grid auf Kubernetes
Event Grid auf Kubernetes akzeptiert und liefert Ereignisse im JSON-Format. Es unterstützt die [Cloud Events 1.0-Schemaspezifikation](https://github.com/cloudevents/spec/blob/v1.0/spec.md), wobei dies das Schema darstellt, das bei der Veröffentlichung von Events im Event Grid verwendet werden sollte. 

[!INCLUDE [event-grid-preview-feature-note.md](../../../includes/event-grid-preview-feature-note.md)]



## <a name="cloudevent-schema"></a>CloudEvents-Schema
[CloudEvents](https://cloudevents.io/) ist eine offene Spezifikation zur Beschreibung von Ereignisdaten. Sie vereinfacht die Interoperabilität, indem ein allgemeines Ereignisschema für die Veröffentlichung bereitgestellt wird und Ereignisse genutzt werden. Informationen zu den obligatorischen Kontexteigenschaften finden Sie unter [CloudEvents-Spezifikation.](https://github.com/cloudevents/spec/blob/master/json-format.md#3-envelope)

## <a name="example--event-using-cloudevents-schema"></a>Beispiel – Ereignis anhand des CloudEvents-Schemas

```json
[{
      "specversion": "1.0",
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

## <a name="next-steps"></a>Nächste Schritte
Informationen zu Zielen und Handlern, die von Event Grid in Azure Arc für Kubernetes unterstützt werden, finden Sie unter [Event Grid in Kubernetes – Ereignishandler](event-handlers.md).