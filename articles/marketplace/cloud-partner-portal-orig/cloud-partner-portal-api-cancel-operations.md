---
title: Abbrechen der Operations-API | Azure Marketplace
description: Abbrechen von Vorgängen.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: f9e55ff2c581f9392a125f6dc3ec8d903e9876a4
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "81256432"
---
# <a name="cancel-operation"></a>Abbrechen eines Vorgangs

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und werden auch nach der Migration Ihrer Angebote zu Partner Center weiterhin funktionieren. Die Integration führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach der Migration zu Partner Center weiterhin funktioniert.

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
| Position    | Der relative Pfad für den Abruf des Status dieses Vorgangs. |
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
