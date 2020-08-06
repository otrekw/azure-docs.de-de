---
title: 'Abrufen des Angebotsstatus: Azure Marketplace'
description: API zum Abrufen des aktuellen Status des Angebots.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 336f23f83c33bcee1887d0e41710e686b794a663
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87272010"
---
# <a name="retrieve-offer-status"></a>Abrufen des Angebotsstatus

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und funktionieren auch weiterhin. Der Übergang führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](./cloud-partner-portal-api-overview.md) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach dem Übergang zu Partner Center weiterhin funktioniert. CPP-APIs sollten nur für vorhandene Produkte verwendet werden, die bereits vor dem Übergang zu Partner Center integriert wurden. Neue Produkte sollten die Partner Center-APIs für die Übermittlung verwenden.

Ruft den aktuellen Status des Angebots ab.

  `GET  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/status?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI-Parameter

|  **Name**       |   **Beschreibung**                            |  **Datentyp** |
|  -------------  |  ------------------------------------------  |  ------------  |
|  publisherId    | Herausgeber-ID, z.B. `Contoso`  |     String     |
|  offerId        | GUID, die das Angebot eindeutig identifiziert      |     String     |
|  api-version    | Neueste Version der API                        |     Date       |
|  |  |

## <a name="header"></a>Header


|  Name           |  Wert               |
|  -------------  | -------------------  |
|  Content-Type   |  `application/json`  |
|  Authorization  | `Bearer YOUR_TOKEN`  |
|  |  |

## <a name="body-example"></a>Beispiel für Hauptteil

### <a name="response"></a>Antwort

``` json
  {
      "status": "succeeded",
      "messages": [],
      "steps": [
      {
          "estimatedTimeFrame": "< 15 min",
          "id": "displaydummycertify",
          "stepName": "Validate Pre-Requisites",
          "description": "Offer settings provided are validated.",
          "status": "complete",
          "messages": [
              {
                  "messageHtml": "Step completed.",
                  "level": "information",
                  "timestamp": "2018-03-16T17:50:45.7215661Z"
              }
          ],       
          "progressPercentage": 100
      },
      {
          "estimatedTimeFrame": "~2-3 days",
          "id": "displaycertify",
          "stepName": "Certification",
          "description": "Your offer is analyzed by our certification systems for issues.",
          "status": "notStarted",
          "messages": [],
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
          "description": "Your virtual machine is being packaged for customers. Additionally, lead systems are being configured and set up.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "publisher-signoff",
          "stepName": "Publisher signoff",
          "description": "Offer is available to preview. Ensure that everything looks good before making your offer live.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "~2-5 days",
          "id": "live",
          "stepName": "Live",
          "description": "Offer is publicly visible and is available for purchase.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      }
      ],
      "previewLinks": [],
      liveLinks": [],
  }
```

### <a name="response-body-properties"></a>Eigenschaften für Antworthauptteil

|  **Name**             |    **Beschreibung**                                                                             |
| --------------------  |   -------------------------------------------------------------------------------------------- |
|  status               | Der Status des Angebots. Die Liste der möglichen Werte finden Sie weiter unten unter [Angebotsstatus](#offer-status). |
|  Cloud an das Gerät             | Array mit Nachrichten, die mit dem Angebot verknüpft sind                                                    |
|  steps                | Array mit den Schritten, die das Angebot während einer Angebotsveröffentlichung durchläuft                      |
|  estimatedTimeFrame   | Schätzung der Zeit, die zum Ausführen dieses Schritts erforderlich sein wird, mit Angabe in einem benutzerfreundlichen Format                       |
|  id                   | Bezeichner (ID) des Schritts                                                                         |
|  stepName             | Name des Schritts                                                                               |
|  description          | Beschreibung des Schritts                                                                        |
|  status               | Status des Schritts. Die Liste der möglichen Werte finden Sie weiter unten unter [Schrittstatus](#step-status).    |
|  Cloud an das Gerät             | Array mit Meldungen, die zu dem Schritt gehören                                                          |
|  processPercentage    | Prozentsatz, bis zu dem der Schritt abgeschlossen ist                                                              |
|  previewLinks         | *Derzeit nicht implementiert*                                                                    |
|  liveLinks            | *Derzeit nicht implementiert*                                                                    |
|  notificationEmails   | Veraltet für Angebote, die zu Partner Center migriert wurden. Benachrichtigungs-E-Mails für migrierte Angebote werden an die E-Mail-Adresse gesendet, die in den Kontoeinstellungen unter den Kontaktinformationen des Verkäufers angegeben ist.<br><br>Bei nicht migrierten Angeboten handelt es sich hierbei um eine kommagetrennte Liste der E-Mail-Adressen, die über den Fortschritt des Vorgangs informiert werden sollen.        |
|  |  |

### <a name="response-status-codes"></a>Antwortstatuscodes

| **Code** |   **Beschreibung**                                                                                 |
| -------  |   ----------------------------------------------------------------------------------------------- |
|  200     |  `OK` – Die Anforderung wurde erfolgreich verarbeitet, und der aktuelle Status des Angebots wurde zurückgegeben. |
|  400     | `Bad/Malformed request` – der Fehlerantworttext enthält möglicherweise weitere Informationen.                 |
|  404     | `Not found`: Die angegebene Entität ist nicht vorhanden.                                                |
|  |  |

### <a name="offer-status"></a>Angebotsstatus

|  **Name**                    |    **Beschreibung**                                       |
|  --------------------------  |  ------------------------------------------------------  |
|  NeverPublished              | Das Angebot wurde nie veröffentlicht.                          |
|  NotStarted                  | Das Angebot ist neu und nicht gestartet.                            |
|  WaitingForPublisherReview   | Das Angebot wartet auf die Herausgebergenehmigung.                 |
|  Wird ausgeführt                     | Die Angebotsübermittlung wird verarbeitet.                     |
|  Erfolgreich                   | Die Verarbeitung der Angebotsübermittlung ist abgeschlossen.               |
|  Canceled                    | Die Angebotsübermittlung wurde abgebrochen.                           |
|  Fehler                      | Fehler bei der Angebotsübermittlung.                                 |
|  |  |

### <a name="step-status"></a>Schrittstatus

|  **Name**                    |    **Beschreibung**                           |
|  -------------------------   |  ------------------------------------------  |
|  NotStarted                  | Der Schritt wurde nicht gestartet.                        |
|  InProgress                  | Der Schritt wird ausgeführt.                             |
|  WaitingForPublisherReview   | Für den Schritt wird auf die Herausgebergenehmigung gewartet.      |
|  WaitingForApproval          | Für den Schritt wird auf die Verarbeitungsgenehmigung gewartet.        |
|  Blockiert                     | Der Schritt ist blockiert.                             |
|  Rejected (Abgelehnt)                    | Der Schritt wurde abgelehnt.                            |
|  Abgeschlossen                    | Der Schritt ist abgeschlossen.                            |
|  Canceled                    | Der Schritt wurde abgebrochen.                           |
|  |  |
