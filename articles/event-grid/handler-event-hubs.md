---
title: Event Hub als Ereignishandler für Azure Event Grid-Ereignisse
description: Hier wird beschrieben, wie Sie Event Hubs als Ereignishandler für Azure Event Grid-Ereignisse verwenden können.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: spelluru
ms.openlocfilehash: c9ce4e7da51005dcb06c9df420d80f4d2c7b93e9
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83596019"
---
# <a name="event-hub-as-an-event-handler-for-azure-event-grid-events"></a>Event Hub als Ereignishandler für Azure Event Grid-Ereignisse
Ein Ereignishandler ist der Ort, an den das Ereignis gesendet wird. Der Handler ergreift zur Verarbeitung des Ereignisses eine Maßnahme. Mehrere Azure-Dienste werden automatisch für die Behandlung von Ereignissen konfiguriert. **Azure Event Hubs** ist einer dieser Dienste. 

Verwenden Sie **Event Hubs**, wenn Ihre Lösung Ereignisse von Event Grid schneller abruft, als die Ereignisse verarbeitet werden können. Sobald sich die Ereignisse in einem Event Hub befinden, kann Ihre Anwendung Ereignisse aus dem Event Hub gemäß einem eigenen Zeitplan verarbeiten. Sie können die Ereignisverarbeitung in Anpassung an die eingehenden Ereignisse skalieren.

## <a name="tutorials"></a>Tutorials
Hierzu folgende Beispiele: 

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Schnellstart: Weiterleiten benutzerdefinierter Ereignisse an Azure Event Hubs mit der Azure CLI und Event Grid](custom-event-to-eventhub.md) | Sendet ein benutzerdefiniertes Ereignis an einen Event Hub, damit es durch eine Anwendung verarbeitet wird. |
| [Resource Manager-Vorlage: Erstellen eines benutzerdefinierten Event Grid-Themas und Senden von Ereignissen an einen Event Hub](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Eine Resource Manager-Vorlage, mit der ein Abonnement für ein benutzerdefiniertes Thema erstellt wird. Sendet Ereignisse an Azure Event Hubs. |

## <a name="message-properties"></a>Nachrichteneigenschaften
Wenn Sie einen **Event Hub** als Ereignishandler für Ereignisse von Event Grid verwenden, legen Sie die folgenden Nachrichtenheader fest: 

| Eigenschaftenname | BESCHREIBUNG |
| ------------- | ----------- | 
| aeg-subscription-name | Name des Ereignisabonnements |
| aeg-delivery-count | <p>Anzahl von Versuchen, die für das Ereignis durchgeführt wurden</p> <p>Beispiel: „1“</p> |
| aeg-event-type | <p>Der Typ des Ereignisses</p><p> Beispiel: „Microsoft.Storage.blobCreated“</p> | 
| aeg-metadata-version | <p>Die Metadatenversion des Ereignisses</p> <p>Beispiel: „1“</p><p> Bei einem **Event Grid-Ereignisschema** stellt diese Eigenschaft die Metadatenversion und bei einem **Cloudereignisschema** die **Spezifikationsversion** dar. </p>|
| aeg-data-version | <p>Die Datenversion des Ereignisses</p><p>Beispiel: „1“</p><p>Bei einem **Event Grid-Ereignisschema** stellt diese Eigenschaft die Datenversion dar. Bei einem **Cloudereignisschema** kommt sie nicht zur Anwendung</p> |
| aeg-output-event-id | ID des Event Grid-Ereignisses |


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der unterstützten Ereignishandler finden Sie im Artikel zu [Ereignishandlern](event-handlers.md). 
