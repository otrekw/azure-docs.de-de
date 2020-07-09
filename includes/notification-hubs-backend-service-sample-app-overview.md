---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 5c3277394350036b8863185e83b7cfcd4975be1c
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095284"
---
Es ermöglicht Ihnen die Registrierung und die Aufhebung der Registrierung von einem Benachrichtigungshub über den von Ihnen erstellten Back-End-Dienst. 

Wird eine Aktion angegeben, und befindet sich die Anwendung im Vordergrund, wird ein Alarm angezeigt. Andernfalls erscheinen die Benachrichtigungen in der Mitteilungszentrale.

> [!NOTE]
> Normalerweise würden Sie die Aktionen zur Registrierung und Aufhebung der Registrierung während des entsprechenden Zeitpunkts im Lebenszyklus der Anwendung (oder vielleicht als Teil Ihres Eindrucks beim ersten Ausführen der Anwendung) ohne explizite Eingaben zur Registrierung/Aufhebung der Registrierung des Benutzers durchführen. Dieses Beispiel erfordert jedoch eine explizite Benutzereingabe, damit diese Funktionalität leichter untersucht und getestet werden kann.
