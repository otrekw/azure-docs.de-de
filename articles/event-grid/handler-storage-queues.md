---
title: Speicherwarteschlange als Ereignishandler für Azure Event Grid-Ereignisse
description: Hier wird beschrieben, wie Sie Azure-Speicherwarteschlangen als Ereignishandler für Azure Event Grid-Ereignisse verwenden können.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: spelluru
ms.openlocfilehash: 9b767caa1041f865d8e15cd57796b186f7a4a6bb
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83596269"
---
# <a name="storage-queue-as-an-event-handler-for-azure-event-grid-events"></a>Speicherwarteschlange als Ereignishandler für Azure Event Grid-Ereignisse
Ein Ereignishandler ist der Ort, an den das Ereignis gesendet wird. Der Handler ergreift zur Verarbeitung des Ereignisses weitere Maßnahmen. Mehrere Azure-Dienste werden automatisch für die Behandlung von Ereignissen konfiguriert. **Azure Queue Storage** ist einer dieser Dienste. 

Verwenden Sie **Queue Storage**, um Ereignisse zu empfangen, die gepullt werden müssen. Bei einem Prozess mit langer Ausführungszeit und zu langer Antwortdauer können Sie Queue Storage verwenden. Wenn die Ereignisse an Queue Storage gesendet werden, kann die App Ereignisse nach einem eigenen Zeitplan pullen und verarbeiten.

## <a name="tutorials"></a>Tutorials
Im folgenden Tutorial finden Sie ein Beispiel für die Verwendung von Queue Storage als Ereignishandler. 

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Schnellstart: Weiterleiten benutzerdefinierter Ereignisse an Azure Queue Storage mit Azure CLI und Event Grid](custom-event-to-queue-storage.md) | Beschreibt das Senden von benutzerdefinierten Ereignisse an eine Queue Storage-Instanz. |

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der unterstützten Ereignishandler finden Sie im Artikel zu [Ereignishandlern](event-handlers.md). 
