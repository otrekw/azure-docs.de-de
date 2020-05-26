---
title: Relay-Hybridverbindung als Ereignishandler für Azure Event Grid-Ereignisse
description: Hier wird beschrieben, wie Sie Azure Relay-Hybridverbindungen als Ereignishandler für Azure Event Grid-Ereignisse verwenden können.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: spelluru
ms.openlocfilehash: 0631724e688a71d7e9685f5f0ad738d81e2a8034
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83596109"
---
# <a name="relay-hybrid-connection-as-an-event-handler-for-azure-event-grid-events"></a>Relay-Hybridverbindung als Ereignishandler für Azure Event Grid-Ereignisse
Ein Ereignishandler ist der Ort, an den das Ereignis gesendet wird. Der Handler ergreift zur Verarbeitung des Ereignisses weitere Maßnahmen. Mehrere Azure-Dienste werden automatisch für die Behandlung von Ereignissen konfiguriert. **Azure Relay** ist einer dieser Dienste. 

Verwenden Sie **Azure Relay-Hybridverbindungen** zum Senden von Ereignissen an Anwendungen, die sich in einem Unternehmensnetzwerk befinden und nicht über einen öffentlich zugänglichen Endpunkt verfügen.

## <a name="tutorials"></a>Tutorials
Im folgenden Tutorial finden Sie ein Beispiel für die Verwendung einer Azure Relay-Hybridverbindung als Ereignishandler: 

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Tutorial: Senden von Ereignissen an eine Hybridverbindung](custom-event-to-hybrid-connection.md) | Sendet ein benutzerdefiniertes Ereignis zur Verarbeitung durch eine Listeneranwendung an eine bestehende Hybridverbindung. |

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der unterstützten Ereignishandler finden Sie im Artikel zu [Ereignishandlern](event-handlers.md). 