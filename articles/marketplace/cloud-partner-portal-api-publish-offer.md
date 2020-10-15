---
title: 'Veröffentlichen eines Angebots: Azure Marketplace'
description: API zum Veröffentlichen des angegebenen Angebots.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 60e75aff79913896bdf1dcdc8754b6ecf5620b06
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87272044"
---
# <a name="publish-an-offer"></a>Veröffentlichen eines Angebots

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und funktionieren auch weiterhin. Der Übergang führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](./cloud-partner-portal-api-overview.md) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach dem Übergang zu Partner Center weiterhin funktioniert. CPP-APIs sollten nur für vorhandene Produkte verwendet werden, die bereits vor dem Übergang zu Partner Center integriert wurden. Neue Produkte sollten die Partner Center-APIs für die Übermittlung verwenden.

Startet den Veröffentlichungsprozess für das angegebene Angebot. Bei diesem Aufruf handelt es sich um einen Vorgang mit langer Ausführungsdauer.

  `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/publish?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI-Parameter
--------------

|  **Name**      |    **Beschreibung**                               |  **Datentyp** |
|  ------------- |  ------------------------------------            |   -----------  |
|  publisherId   | Herausgeber-ID, z.B. `contoso`      |   String       |
|  offerId       | Angebots-ID                                 |   String       |
|  api-version   | Aktuelle Version der API                        |   Date         |
|  |  |

## <a name="header"></a>Header
------

|  **Name**        |    **Wert**          |
|  --------        |    ---------          |
|  Content-Type    | `application/json`    |
|  Authorization   |  `Bearer YOUR_TOKEN`  |
|  |  |


## <a name="body-example"></a>Beispiel für Hauptteil
------------

### <a name="request"></a>Anforderung

``` json
  { 
      'metadata': 
          { 
              'notification-emails': 'jdoe@contoso.com'
          } 
  }
```

### <a name="request-body-properties"></a>Eigenschaften des Anforderungstexts

|  **Name**               |   **Beschreibung**                                                                                 |
|  ---------------------  | ------------------------------------------------------------------------------------------------- |
|  notification-emails    | Kommagetrennte Liste der E-Mail-Adressen, die über den Fortschritt des Veröffentlichungsvorgangs benachrichtigt werden sollen. |
|  |  |

### <a name="response"></a>Antwort

#### <a name="migrated-offers"></a>Migrierte Angebote

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Nicht migrierte Angebote

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`

### <a name="response-header"></a>Antwortheader

|  **Name**             |    **Wert**                                                                 |
|  -------------------- | ---------------------------------------------------------------------------- |
| Standort    | Der relative Pfad zum Abrufen des Status dieses Vorgangs.     |
|  |  |

### <a name="response-status-codes"></a>Antwortstatuscodes

| **Code** |  **Beschreibung**                                                                                                                           |
| ------   |  ----------------------------------------------------------------------------------------------------------------------------------------- |
| 202   | `Accepted`: Die Anforderung wurde erfolgreich angenommen. Die Antwort enthält einen Speicherort, der zum Nachverfolgen des gestarteten Vorgangs verwendet werden kann. |
| 400   | `Bad/Malformed request`: Der Fehlerantworttext stellt möglicherweise weitere Informationen bereit.                                                               |
| 422   | `Un-processable entity`: Gibt an, dass die Validierung der zu veröffentlichenden Entität fehlgeschlagen ist.                                                        |
| 404   | `Not found`: Die vom Client angegebene Entität ist nicht vorhanden.                                                                              |
|  |  |
