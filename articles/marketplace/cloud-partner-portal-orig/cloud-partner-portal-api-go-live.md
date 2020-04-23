---
title: Live schalten | Azure Marketplace
description: Der Live schalten-API initiiert den Prozess, in dem das Angebot zur Auflistung freigeschaltet (live geschaltet) wird.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: ef22f7720a4af2239c55d1a01f9d3f11c878d66e
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "81256313"
---
# <a name="go-live"></a>Live schalten

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und werden auch nach der Migration Ihrer Angebote zu Partner Center weiterhin funktionieren. Die Integration führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach der Migration zu Partner Center weiterhin funktioniert.

Diese API startet den Prozess zum Überführen einer App in den Produktionsstatus. Dieser Vorgang dauert üblicherweise ziemlich lange. In diesem Aufruf wird die E-Mail-Benachrichtigungsliste (notification-emails) aus dem Vorgang in der [Veröffentlichen](./cloud-partner-portal-api-publish-offer.md)-API verwendet.

 `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/golive?api-version=2017-10-31` 

## <a name="uri-parameters"></a>URI-Parameter
--------------

|  **Name**      |   **Beschreibung**                                                           | **Datentyp** |
|  --------      |   ---------------                                                           | ------------- |
| publisherId    | Herausgeber-ID für das abzurufende Angebot, z. B. `contoso`       |  String       |
| offerId        | Angebots-ID des abzurufenden Angebots                                   |  String       |
| api-version    | Aktuelle Version der API                                                   |  Date         |
|  |  |  |

## <a name="header"></a>Header
------

|  **Name**       |     **Wert**       |
|  ---------      |     ----------      |
| Content-Type    | `application/json`  |
| Authorization   | `Bearer YOUR_TOKEN` |
|  |  |

## <a name="body-example"></a>Beispiel für Hauptteil

### <a name="response"></a>Antwort

#### <a name="migrated-offers"></a>Migrierte Angebote

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Nicht migrierte Angebote

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`

### <a name="response-header"></a>Antwortheader

|  **Name**             |      **Wert**                                                            |
|  --------             |      ----------                                                           |
| Position    |  Der relative Pfad zum Abrufen des Status dieses Vorgangs.            |
|  |  |

### <a name="response-status-codes"></a>Antwortstatuscodes

| **Code** |  **Beschreibung**                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted`: Die Anforderung wurde erfolgreich angenommen. Die Antwort enthält eine Position, um den Vorgangsstatus nachzuverfolgen. |
|  400     | `Bad/Malformed request`: Zusätzliche Fehlerinformationen sind im Antworttext zu finden. |
|  404     |  `Not found` – die angegebene Entität ist nicht vorhanden.                                       |
|  |  |
