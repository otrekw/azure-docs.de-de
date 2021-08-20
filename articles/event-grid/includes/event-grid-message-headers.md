---
title: Datei einfügen
description: include file
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 11/19/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: c37e2c28eb6aee9edf4bdd97066ce5f15e7447c1
ms.sourcegitcommit: 5163ebd8257281e7e724c072f169d4165441c326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2021
ms.locfileid: "112413895"
---
## <a name="message-headers"></a>Nachrichtenheader
Dies sind die Eigenschaften, die Sie in den Nachrichtenheadern empfangen:

| Eigenschaftenname | BESCHREIBUNG |
| ------------- | ----------- | 
| aeg-subscription-name | Name des Ereignisabonnements. |
| aeg-delivery-count | Anzahl von Versuchen, die für das Ereignis durchgeführt wurden |
| aeg-event-type | <p>Der Typ des Ereignisses</p><p>Es kann sich um einen der folgenden Werte handeln:</p><ul><li>SubscriptionValidation</li><li>Benachrichtigung</li><li>SubscriptionDeletion</li></ul> | 
| aeg-metadata-version | <p>Die Metadatenversion des Ereignisses<p> Bei einem **Event Grid-Ereignisschema** stellt diese Eigenschaft die Metadatenversion und bei einem **Cloudereignisschema** die **Spezifikationsversion** dar. </p>|
| aeg-data-version | <p>Die Datenversion des Ereignisses</p><p>Bei einem **Event Grid-Ereignisschema** stellt diese Eigenschaft die Datenversion dar. Bei einem **Cloudereignisschema** kommt sie nicht zur Anwendung</p> |
| aeg-output-event-id | ID des Event Grid-Ereignisses |


