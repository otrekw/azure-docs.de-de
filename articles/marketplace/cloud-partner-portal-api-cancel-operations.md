---
title: 'API zum Abbrechen von Vorgängen: kommerzieller Microsoft-Marketplace'
description: Die API zum Abbrechen eine Vorgangs wird derzeit für das Angebot weiterentwickelt.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: anbene
ms.author: mingshen
ms.date: 06/16/2020
ms.openlocfilehash: e65f0a8ee0a5dfafab681010006fe190cb5bad70
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86102751"
---
# <a name="cancel-operation"></a>Abbrechen eines Vorgangs

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und werden auch nach der Migration Ihrer Angebote zu Partner Center weiterhin funktionieren. Die Integration führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](./cloud-partner-portal-api-overview.md) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach der Migration zu Partner Center weiterhin funktioniert.

Diese API bricht einen Vorgang ab, der gerade für das Angebot ausgeführt wird. Verwenden Sie die [API zum Abrufen von Vorgängen](./cloud-partner-portal-api-retrieve-operations.md), um eine Vorgangs-ID (`operationId`) zu erhalten, die an diese API übergeben werden kann. Das Abbrechen eines Vorgangs ist in der Regel ein synchroner Vorgang. In bestimmten komplexen Szenarien wird zum Abbrechen eines vorhandenen Vorgangs jedoch unter Umständen ein neuer Vorgang benötigt. In diesem Fall enthält der HTTP-Antworttext den Ort des Vorgangs, der zum Abfragen des Status verwendet werden muss.

  `POST https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/cancel?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI-Parameter

--------------

|  **Name**    |      **Beschreibung**                                  |    **Datentyp**  |
| ------------ |     ----------------                                  |     -----------   |
| publisherId  |  Herausgeber-ID (Beispiel: `contoso`)         |   String          |
| offerId      |  Angebots-ID                                     |   String          |
| api-version  |  Aktuelle Version der API                               |    Date           |
|  |  |  |

## <a name="header"></a>Header
------

|  **Name**              |  **Wert**         |
|  ---------             |  ----------        |
|  Content-Type          |  Anwendung/json  |
|  Authorization         |  Bearer <IHR TOKEN> |
|  |  |

## <a name="body-example"></a>Beispiel für Hauptteil
------------

### <a name="request"></a>Anforderung

``` json
{
   "metadata": {
     "notification-emails": "jondoe@contoso.com"
    }
}     
```

### <a name="request-body-properties"></a>Eigenschaften des Anforderungstexts

|  **Name**                |  **Beschreibung**                                               |
|  --------                |  ---------------                                               |
|  notification-emails     | Kommagetrennte Liste mit den E-Mail-IDs, die über den Fortschritt des Veröffentlichungsvorgangs benachrichtigt werden sollen. |
|  |  |

### <a name="response"></a>Antwort

#### <a name="migrated-offers"></a>Migrierte Angebote

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Nicht migrierte Angebote

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`

### <a name="response-header"></a>Antwortheader

|  **Name**             |    **Wert**                       |
|  ---------            |    ----------                      |
| Standort    | Der relative Pfad für den Abruf des Status dieses Vorgangs. |
|  |  |

### <a name="response-status-codes"></a>Antwortstatuscodes

| **Code**  |  **Beschreibung**                                                                       |
|  ------   |  ------------------------------------------------------------------------               |
|  200      | OK: Die Anforderung wurde erfolgreich verarbeitet, und der Vorgang wird synchron abgebrochen. |
|  202      | Akzeptiert: Die Anforderung wurde erfolgreich verarbeitet, und der Vorgang wird abgebrochen. Der Ort des Abbruchvorgangs wird im Antwortheader zurückgegeben. |
|  400      | Ungültige/falsch formatierte Anforderung: Der Fehlerantworttext enthält ggf. weitere Informationen.  |
|  403      | Zugriff verweigert: Der Client hat keinen Zugriff auf den in der Anforderung angegebenen Namespace. |
|  404      | Nicht gefunden: Die angegebene Entität ist nicht vorhanden. |
|  |  |
