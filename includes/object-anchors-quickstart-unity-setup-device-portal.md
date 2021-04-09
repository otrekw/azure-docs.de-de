---
author: craigktreasure
ms.service: azure-object-anchors
ms.topic: include
ms.date: 04/03/2020
ms.author: crtreasu
ms.openlocfilehash: 81c4e95660eb2875cd1b03bdfa670aeabadedc3f
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2021
ms.locfileid: "105104101"
---
### <a name="set-up-the-windows-device-portal"></a>Einrichten des Windows-Geräteportals

Führen Sie die folgenden Schritte aus, um über das WLAN eine Verbindung zu Ihrem HoloLens-Gerät herzustellen:

1. [Verbinden Sie zunächst Ihr HoloLens-Gerät mit dem WLAN](/hololens/hololens-network).

2. Rufen Sie dann auf dem Gerät unter **Einstellungen > Netzwerk und Internet > WLAN > Erweiterte Optionen** die IP-Adresse auf.

3. Rufen Sie in einem Webbrowser auf dem PC „`https://<YOUR_HOLOLENS_IP_ADDRESS>`“ auf. Im Browser wird die folgende Meldung angezeigt: „Es besteht ein Problem mit dem Sicherheitszertifikat dieser Website“. Diese Nachricht tritt auf, weil das für das Geräteportal ausgestellte Zertifikat ein selbstsigniertes Zertifikat ist. Sie können diesen Zertifikatfehler ignorieren und fortfahren.

Weitere Informationen zum Einrichten des Windows-Geräteportals finden Sie [unter diesem Link](/windows/mixed-reality/using-the-windows-device-portal).