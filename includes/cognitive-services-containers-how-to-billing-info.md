---
author: IEvangelist
ms.author: dapine
ms.date: 02/19/2020
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 2ac93f5aba722eea78267a512999a5581a887b99
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77474138"
---
Abfragen des Containers werden gemäß des Tarifs der Azure-Ressource in Rechnung gestellt, die für `ApiKey` verwendet wird.

Für die Ausführung von Azure Cognitive Services-Containern besteht keine Lizenz, wenn sie nicht mit dem Endpunkt für Messung/Abrechnung verbunden sind. Sie müssen sicherstellen, dass die Container jederzeit Abrechnungsinformationen an den Abrechnungsendpunkt übermitteln können. Cognitive Services-Container senden keine Kundendaten (z. B. das analysierte Bild oder den analysierten Text) an Microsoft.

### <a name="connect-to-azure"></a>Herstellen einer Verbindung mit Azure

Der Container benötigt die Abrechnungsargumentwerte, um ausgeführt werden zu können. Diese Werte ermöglichen es dem Container, eine Verbindung mit dem Abrechnungsendpunkt herzustellen. Der Container meldet die Nutzung etwa alle 10 bis 15 Minuten. Wenn der Container nicht innerhalb des zulässigen Zeitfensters eine Verbindung mit Azure herstellt, wird der Container weiterhin ausgeführt, verarbeitet aber keine Anfragen, bis der Abrechnungsendpunkt wiederhergestellt wurde. Es erfolgen 10 Verbindungsversuche im gleichen Zeitintervall von 10 bis 15 Minuten. Wenn die Verbindung mit dem Abrechnungsendpunkt nicht innerhalb der 10 Versuche hergestellt werden kann, wird die Bereitstellung von Anforderungen durch den Container beendet.

### <a name="billing-arguments"></a>Abrechnungsargumente

Der Befehl <a href="https://docs.docker.com/engine/reference/commandline/run/" target="_blank">`docker run` <span class="docon docon-navigate-external x-hidden-focus"></span></a> startet den Container, wenn alle drei der folgenden Optionen über gültige Werte verfügen:

| Option | BESCHREIBUNG |
|--------|-------------|
| `ApiKey` | Der API-Schlüssel der Cognitive Services-Ressource, der zum Nachverfolgen von Abrechnungsinformationen verwendet wird.<br/>Der Wert dieser Option muss für die bereitgestellte Ressource, die in `Billing` angegeben wurde, auf einen API-Schlüssel festgelegt werden. |
| `Billing` | Der Endpunkt der Cognitive Services-Ressource, der zum Nachverfolgen von Abrechnungsinformationen verwendet wird.<br/>Der Wert dieser Option muss auf den Endpunkt-URI einer bereitgestellten Azure-Ressource festgelegt werden.|
| `Eula` | Gibt an, dass Sie die Lizenz für den Container akzeptiert haben.<br/>Der Wert dieser Option muss auf **accept** festgelegt werden. |
