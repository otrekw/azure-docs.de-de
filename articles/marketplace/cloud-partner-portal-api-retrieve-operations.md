---
title: Abrufen der Operations-API – Azure Marketplace
description: API zum Abrufen aller Vorgänge im Angebot oder eines bestimmten Vorgangs für die angegebene Vorgangs-ID.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: cb44d977407a7e854603e6bbacf3591752b109c2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87271942"
---
# <a name="retrieve-operations"></a>Abrufen von Vorgängen

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und funktionieren auch weiterhin. Der Übergang führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](./cloud-partner-portal-api-overview.md) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach dem Übergang zu Partner Center weiterhin funktioniert. CPP-APIs sollten nur für vorhandene Produkte verwendet werden, die bereits vor dem Übergang zu Partner Center integriert wurden. Neue Produkte sollten die Partner Center-APIs für die Übermittlung verwenden.

Ruft alle Vorgänge im Angebot oder einen bestimmten Vorgang für die angegebene Vorgangs-ID ab. Der Client kann Abfrageparameter zum Filtern nach ausgeführten Vorgängen verwenden.

``` https

  GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/submissions/?api-version=2017-10-31&status=<filteredStatus>

  GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/operations/<operationId>?api-version=2017-10-31

```

## <a name="uri-parameters"></a>URI-Parameter

|  **Name**          |      **Beschreibung**                                                                                           | **Datentyp** |
|  ----------------  |     --------------------------------------------------------------------------------------------------------   |  -----------  |
|  publisherId       |  Herausgeber-ID, z.B. `Contoso`                                                                   |  String       |
|  offerId           |  Angebots-ID                                                                                              |  String       |
|  operationId       |  GUID, die den Vorgang im Angebot eindeutig identifiziert. Die Vorgangs-ID kann mithilfe dieser API abgerufen werden. Sie wird auch im HTTP-Header der Antwort für lang ausgeführte Vorgänge zurückgegeben, wie z.B. bei der [API zum Veröffentlichen von Angeboten](./cloud-partner-portal-api-publish-offer.md).  |   Guid   |
|  api-version       | Neueste Version der API |    Date      |
|  |  |  |

## <a name="header"></a>Header

|  **Name**          |  **Wert**           |
|  ---------------   | -------------------- |
|  Content-Type      | `application/json`   |
|  Authorization     | `Bearer YOUR_TOKEN`  |
|  |  |

## <a name="body-example"></a>Beispiel für Hauptteil

### <a name="response"></a>Antwort

#### <a name="get-operations"></a>GET-Vorgänge

``` json
    [
        {
            "id": "5a63deb5-925b-4ee0-938b-7c86fbf287c5",
            "offerId": "56615b67-2185-49fe-80d2-c4ddf77bb2e8",
            "offerVersion": 1,
            "offerTypeId": "microsoft-azure-virtualmachines",
            "publisherId": "contoso",
            "submissionType": "publish",
            "submissionState": "running",
            "publishingVersion": 2,
            "slot": "staging",
            "version": 636576975611768314,
            "definition": {
                "metadata": {
                    "emails": "jdoe@contoso.com"
                }
            },
            "changedTime": "2018-03-26T21:46:01.179948Z"
        }
    ]
```

#### <a name="get-operation"></a>GET-Vorgang

``` json
    [
        {
            "status" : "running",
            "messages" : [],
            "publishingVersion" : 2,
            "offerVersion" : 1,
            "cancellationRequestState": "canCancel",
            "steps": [
                        {
                            "estimatedTimeFrame": "< 15 min",
                            "id": "displaydummycertify",
                            "stepName": "Validate Pre-Requisites",
                            "description": "Offer settings provided are validated",
                            "status": "complete",
                            "messages": 
                            [
                                {
                                    "messageHtml": "Step completed.",
                                    "level": "information",
                                    "timestamp": "2017-03-28T19:50:36.500052Z"
                                }
                            ],
                            "progressPercentage": 100
                        },
                        {
                            "estimatedTimeFrame": "< 5 day",
                            "id": "displaycertify",
                            "stepName": "Certification",
                            "description": "Your offer is analyzed by our certification systems for issues.",
                            "status": "blocked",
                            "messages": 
                            [
                                {
                                    "messageHtml": "No virtual machine image was found for the plan contoso.",
                                    "level": "error",
                                    "timestamp": "2017-03-28T19:50:39.5506018Z"
                                },
                                {
                                    "messageHtml": "This step has not started yet.",
                                    "level": "information",
                                    "timestamp": "2017-03-28T19:50:39.5506018Z"
                                }
                            ],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "< 1 day",
                            "id": "displayprovision",
                            "stepName": "Provisioning",
                            "description": "Your virtual machine is being replicated in our production systems.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "< 1 hour",
                            "id": "displaypackage",
                            "stepName": "Packaging and Lead Generation Registration",
                            "description": "Your virtual machine is packaged for being shown to your customers. Additionally, we hookup our lead generation systems to send leads for your offer.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "id": "publisher-signoff",
                            "stepName": "Publisher signoff",
                            "description": "Offer is available to preview. Ensure that everything looks good before making your offer live.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "~2-5 days",
                            "id": "live",
                            "stepName": "Live",
                            "description": "Offer is publicly visible and is available for purchase.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        }
                    ],
                "previewLinks": [],
                "liveLinks": [],
            }
        }
    ]
```

### <a name="response-body-properties"></a>Eigenschaften für Antworthauptteil

|  **Name**                    |  **Beschreibung**                                                                                  |
|  --------------------        |  ------------------------------------------------------------------------------------------------ |
|  id                          | GUID, die den Vorgang eindeutig identifiziert                                                       |
|  submissionType              | Gibt den Typ des Vorgangs, der für das Angebot gemeldet wird, an, z.B. `Publish/GoLive`      |
|  createdDateTime             | UTC-Datum und -Uhrzeit der Erstellung des Vorgangs                                                       |
|  lastActionDateTime          | UTC-Datum und -Uhrzeit der letzten Änderung des Vorgangs                                       |
|  status                      | Status des Vorgangs: `not started` \| `running` \| `failed` \| `completed` Nur ein Vorgang kann jeweils den Status `running` aufweisen. |
|  error                       | Fehlermeldung für Vorgangsfehler                                                               |
|  |  |

### <a name="response-step-properties"></a>Antwortschritteigenschaften

|  **Name**                    |  **Beschreibung**                                                                                  |
|  --------------------        |  ------------------------------------------------------------------------------------------------ |
| estimatedTimeFrame | Die geschätzte Dauer dieses Vorgangs |
| id | Der eindeutige Bezeichner für den Schrittprozess |
| description | Beschreibung des Schritts |
| stepName | Der Anzeigename für den Schritt |
| status | Der Status des Schritts (`notStarted` \| `running` \| `failed` \| `completed`) |
| Cloud an das Gerät | Im Rahmen des Schritts aufgetretene Benachrichtigungen oder Warnungen. Array von Zeichenfolgen |
| progressPercentage | Eine ganze Zahl zwischen 0 und 100, die den Fortschritt des Schritts angibt |
| | |

### <a name="response-status-codes"></a>Antwortstatuscodes

| **Code**  |   **Beschreibung**                                                                                  |
|  -------- |   -------------------------------------------------------------------------------------------------|
|  200      | `OK` – die Anforderung wurde erfolgreich verarbeitet, und die angeforderten Vorgänge wurden zurückgegeben.        |
|  400      | `Bad/Malformed request` – der Fehlerantworttext enthält möglicherweise weitere Informationen.                    |
|  403      | `Forbidden` – der Client hat keinen Zugriff auf den angegebenen Namespace.                          |
|  404      | `Not found` – die angegebene Entität ist nicht vorhanden.                                                 |
|  |  |
