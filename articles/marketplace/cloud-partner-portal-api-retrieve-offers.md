---
title: API zum Abrufen von Angeboten – Azure Marketplace
description: API zum Abrufen einer zusammengefassten Liste der Angebote unter einem Herausgebernamespace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 39d07751c708d5555799ecbb3b3bc66d3f44f43a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87271959"
---
# <a name="retrieve-offers"></a>Abrufen von Angeboten

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und funktionieren auch weiterhin. Der Übergang führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](./cloud-partner-portal-api-overview.md) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach dem Übergang zu Partner Center weiterhin funktioniert. CPP-APIs sollten nur für vorhandene Produkte verwendet werden, die bereits vor dem Übergang zu Partner Center integriert wurden. Neue Produkte sollten die Partner Center-APIs für die Übermittlung verwenden.

Ruft eine zusammengefasste Liste der Angebote unter einem Herausgebernamespace ab.

 `GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI-Parameter

| **Name**         |  **Beschreibung**                         |  **Datentyp** |
| -------------    |  ------------------------------------    |  -----------   |
|  publisherId     | Herausgeber-ID, z.B. `contoso` |   String    |
|  api-version     | Neueste Version der API                    |    Date        |
|  |  |

## <a name="header"></a>Header

|  **Name**        |         **Wert**       |
|  --------------- |       ----------------  |
|  Content-Type    | `application/json`      |
|  Authorization   | `Bearer YOUR_TOKEN`     |
|  |  |

## <a name="body-example"></a>Beispiel für Hauptteil

### <a name="response"></a>Antwort

``` json
  200 OK 
  [ 
      {  
          "offerTypeId": "microsoft-azure-virtualmachines",
          "publisherId": "contoso",
          "status": "published",
          "id": "059afc24-07de-4126-b004-4e42a51816fe",
          "version": 1,
          "definition": {
              "displayText": "Contoso Virtual Machine"
          },
          "changedTime":"2017-05-23T23:33:47.8802283Z"
      }
  ]
```

### <a name="response-body-properties"></a>Eigenschaften für Antworthauptteil

|  **Name**       |       **Beschreibung**                                                                                                  |
|  -------------  |      --------------------------------------------------------------------------------------------------------------    |
|  offerTypeId    | Gibt den Typ des Angebots an                                                                                           |
|  publisherId    | Bezeichner, der den Herausgeber eindeutig identifiziert                                                                      |
|  status         | Status des Angebots. Die Liste der möglichen Werte finden Sie weiter unten unter [Angebotsstatus](#offer-status).                         |
|  id             | GUID, die das Angebot im Herausgebernamespace eindeutig identifiziert.                                                    |
|  version        | Aktuelle Version des Angebots. Die version-Eigenschaft kann vom Client nicht geändert werden. Sie wird nach jeder Veröffentlichung inkrementiert. |
|  Definition     | Enthält eine zusammengefasste Ansicht der tatsächlichen Definition der Workload. Für eine ausführliche Definition verwenden Sie die API zum [Abrufen eines bestimmten Angebots](./cloud-partner-portal-api-retrieve-specific-offer.md). |
|  changedTime    | Uhrzeit (UTC), zu der das Angebot letztmalig geändert wurde                                                                              |
|  |  |

### <a name="response-status-codes"></a>Antwortstatuscodes

| **Code**  |  **Beschreibung**                                                                                                   |
| -------   |  ----------------------------------------------------------------------------------------------------------------- |
|  200      | `OK`: Die Anforderung wurde erfolgreich verarbeitet, und alle Angebote unter dem Herausgeber wurden an den Client zurückgegeben.  |
|  400      | `Bad/Malformed request` – der Fehlerantworttext enthält möglicherweise weitere Informationen.                                    |
|  403      | `Forbidden` – der Client hat keinen Zugriff auf den angegebenen Namespace.                                          |
|  404      | `Not found`: Die angegebene Entität ist nicht vorhanden.                                                                 |
|  |  |

### <a name="offer-status"></a>Angebotsstatus

|  **Name**                    | **Beschreibung**                                  |
|  ------------------------    | -----------------------------------------------  |
|  NeverPublished              | Das Angebot wurde nie veröffentlicht.                  |
|  NotStarted                  | Das Angebot ist neu, aber nicht gestartet.                 |
|  WaitingForPublisherReview   | Das Angebot wartet auf die Herausgebergenehmigung.         |
|  Wird ausgeführt                     | Die Angebotsübermittlung wird verarbeitet.             |
|  Erfolgreich                   | Die Verarbeitung der Angebotsübermittlung ist abgeschlossen.       |
|  Canceled                    | Die Angebotsübermittlung wurde abgebrochen.                   |
|  Fehler                      | Fehler bei der Angebotsübermittlung.                         |
|  |  |
